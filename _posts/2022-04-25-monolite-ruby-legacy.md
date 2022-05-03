---
layout: post
current: post
cover: 'assets/images/post-cover/2022-legacy.jpg'
socialcover: 'assets/images/post-cover/2022-legacy-s.jpg'
slug: come-rimodernare-app-legacy-monolite-ruby-microservizi
title: "Dal monolite legacy ai microservizi"
date: 2022-04-25 09:00:00
category : [tech]
tags: [legacy, coding, ruby]
author: [ema]
---

Nei mesi scorsi un nuovo cliente ci ha contattato per un progetto di _riscrittura della loro applicazione_ web nata come MVP ma arrivata ormai al limite del gestibile.

L’applicazione in questione è un **monolite scritto in Ruby on Rails**, nato circa 5 anni fa ed evoluto in quello che oggi è un’applicazione completa e funzionante ma difficilmente manutenibile e soprattutto con elevati costi di evoluzione. &egrave; un pattern abbastanza comune nelle startup: si parte con una serie di esperimenti e si cambia continuamente rotta.

Il cliente ha bisogno di nuove funzionalità tra cui un’app mobile e nuovi meccanismi di gestione del proprio business e ha chiesto a noi di prenderci in carico l’applicazione e di farla evolvere. Il tutto **senza andare ad impattare sul business esistente**.

Sfida non facile anche per questioni di **budget** e di **rischio** che non ci hanno permesso di riscrivere da zero tutta l’applicazione. Che fare allora?

<figure style="text-align:center"><img src="/assets/images/post-content/legacy/legacy_s_001.png" alt="monolite legacy" /></figure>

### Passo uno: capire i processi
Il primo step è stato quello di capire cosa voleva il cliente e come funzionavano i loro processi di business. Per questo abbiamo organizzato un primo [workshop](https://blog.codiceplastico.com/progettare-applicazioni-utili-per-i-clienti-pt2) per studiare la situazione attuale e capire quali erano gli **obiettivi** nel medio e lungo periodo.

Il workshop ci ha permesso di capire come funzionavano le cose dall’interno: abbiamo individuato almeno quattro applicazioni, all'interno del monolite Rails, che potevano (e **dovevano**) essere separate .

In termini **DDD** si tratta di quattro diversi **Bounded Context** ognuno con le proprie regole, i propri dati e i propri utenti.

<figure style="text-align:center"><img src="/assets/images/post-content/legacy/legacy_l_001.png" alt="monolite legacy" /></figure>

### Passo due: decidere da dove cominciare

Una volta compresa la struttura di questo _mostro a quattro teste_ abbiamo proposto al cliente di partire dallo scenario più semplice. L'idea era quella di creare una nuova App mobile (app, api e database), che gestisse una parte specifica dei processi, in grado di lavorare in **aggiunta all’esistente**, integrandosi in modo trasparente con il resto dell’applicazione Rails che doveva continuare a vivere (almeno per un po’).

<figure style="text-align:center"><img src="/assets/images/post-content/legacy/legacy_s_003.png" alt="monolite legacy" /></figure>

### Passo tre: sfida accettata, siamo partiti.

Siamo partiti progettando l’app mobile da zero, **come se il monolite legacy non esistesse**, quindi una volta chiariti i flussi abbiamo implementato lo stack app+api+db come greenfield project senza preoccuparci di come mettere in comunicazione e integrare il nuovo stack con il vecchio.

Arrivati a 3/4 di progetto, con le idee ormai chiare e l’app a buon punto (avevamo una prima beta) abbiamo iniziato ad affrontare il problema sincronizzazione.

L’app mobile generava una serie di **informazioni che dovevano essere passate all’app legacy** e viceversa l’app legacy aveva dei **dati che dovevano in qualche modo finire sull’app mobile**.

Il primo caso (app mobile → legacy) è stato abbastanza semplice, avevamo pieno controllo sulla nostra codebase, quindi ci è bastano mettere su una **coda di RabbitMq un set di eventi** contenenti le informazioni che dovevano essere travasate nell’app legacy e scrivere un terzo servizio che consumava i messaggi, predisponeva i dati nel formato giusto per l’app legacy e li salvava sul database.

Il viceversa è stato un po’ più complesso, i due vincoli che ci siamo imposti sono stati:

- 1) **non dobbiamo modificare la codebase** dell’app legacy: non la conosciamo, non ci sono test, utilizza librerie non più supportate e il rischio di fare danni è troppo elevato

- 2) non vogliamo che la nostra applicazione sia influenzata dalle scelte dell’app legacy. Leggendo il codice abbiamo notato degli **errori di modellazione** dovuti quasi certamente alla poca conoscenza del dominio nelle fasi iniziali, riportare questi errori nel nostro contesto ci sarebbe costato caro.

Con questi due vincoli abbiamo iniziato a studiare soluzioni tecniche che ci permettessero di risolvere il problema.

<figure style="text-align:center"><img src="/assets/images/post-content/legacy/legacy_s_002.png" alt="monolite legacy" /></figure>

### Passo quattro: notifichiamo
Abbiamo deciso di lavorare a livello di dati scrivendo un servizio che stia in ascolto delle modifiche sul database: Postgres mette a disposizione un meccanismo per notificare eventuali applicazioni collegate tramite il comando NOTIFY ([https://www.postgresql.org/docs/14/sql-notify.html](https://www.postgresql.org/docs/14/sql-notify.html)).

Abbiamo creato una funzione sul database legacy:

```sql
CREATE OR REPLACE FUNCTION notify_legacy_db_change()
  RETURNS trigger AS $$
  BEGIN
    PERFORM pg_notify(
          'notify_legacy_db_change',
          json_build_object(
            'table_name', TG_TABLE_NAME,
            'timestamp', NOW(),
            'operation', TG_OP,
            'record', row_to_json(NEW)
          )::text
        );

        RETURN NEW;
      END;
      $$ LANGUAGE plpgsql;
```
<br/><br/>
Questa funzione quando invocata invia sul canale `notify_legacy_db_change` un oggetto json con il nome della tabella che ha generato il messaggio, un timestamp, il tipo di operazione (INSERT/UPDATE/DELETE) e il record oggetto dell’operazione.

Dopo aver definito questa funzione abbiamo aggiunto dei *trigger* (😱) sulle tabelle che volevamo monitorare su operazioni di INSERT/UPDATE/DELETE, tali trigger non fanno altro che chiamare la funzione `notify_legacy_db_change`.

Lato applicativo, abbiamo creato un `GenServer` che rimane in ascolto sul canale di postgres:

```elixir
defmodule Fluttershy.Producer.LegacyDbListener do
  use GenServer
  require Logger

  def start_link(opts) do
    GenServer.start_link(__MODULE__, opts, name: __MODULE__)
  end

  def init([channel_name]) do
    {:ok, _} = Postgrex.Notifications.listen(Fluttershy.Producer.PgNotifier, channel_name)
    {:ok, %{channel_name: channel_name}}
  end

  def handle_info({:notification, _pid, _ref, _channel_name, payload}, _state) do
    {:ok, data} = Jason.decode(payload)
    table_name = Map.get(data, "table_name", "unknown_table")
    Logger.info("Received message from #{table_name}")

		# ... spawn processo per la gestione del messaggio ...

    {:noreply, :event_handled}
  end
end
```

Questo `GenServer` al suo avvio attiva una connessione con il database in modalità "_listen_" e ad ogni NOTIFY viene invocata la funzione `handle_info` la quale riceve il payload json definito da PostgreSQL. Il payload viene mandato a alcuni event handler (uno per tipo di messaggio).

Gli event handler, oltre al payload contenente il record,  hanno accesso al **database legacy per costruire dei messaggi** completi da inviare poi alla nuova applicazione sotto forma di **messaggi RabbitMQ**.

In questo modo abbiamo gestito lo **scambio di informazioni in modo bidirezionale**, senza dover scendere a compromessi sul design della nuova applicazione mobile e mantenendo una completa compatibilità con il sistema legacy che continua a convivere con il nuovo.

<figure style="text-align:center"><img src="/assets/images/post-content/legacy/legacy_l_002.png" alt="monolite legacy" /></figure>

### E domani? 

Questo è stato il primo step di un rework che ci porterà col tempo a riscrivere completamente il sistema legacy, ma ci permette di **farlo per step**, senza fretta e soprattutto **senza che il business ne risenta**. Una volta che il sistema legacy sarà dismesso potremmo spegnere il listener sul db legacy e rimuovere i messaggi che non servono.
