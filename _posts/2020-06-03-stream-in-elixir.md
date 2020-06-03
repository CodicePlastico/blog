---
layout: post
current: post
cover: 'assets/images/post-cover/stream-elixir.jpg'
socialcover: 'assets/images/post-cover/stream-elixir-s.jpg'
slug: stream-in-elixir
title: 'Stream in Elixir'
date: 2020-06-03 00:00:00
categories: [tech]
tags: [elixir, kata, stream]
author: [ema]
---


Il concetto di **stream** non è di certo una novità, nel [SICP](https://www.amazon.com/Structure-Interpretation-Computer-Programs-Engineering/dp/0262510871) ci sono interi capitoli dedicati all’argomento e tutti i principali linguaggi di programmazione hanno un’implementazione nella standard library ma la mia sensazione è che sono poco usati e conosciuti.

Il caso vuole che nelle ultime settimane lavorando con [Alessandro](https://twitter.com/amelchiori) su un progetto in Elixir, ci siamo imbattuti nel **problema perfetto** per l’uso degli stream.
L’applicazione che stavamo implementando tra le funzionalità ne aveva una che prevedeva il **caricamento di un file CSV**, che doveva essere elaborato appoggiandosi a delle **API esterne**, per poi generare un **nuovo file CSV** contente i risultati dell’elaborazione.

### Una riga per volta
La prima implementazione che abbiamo scritto era quella classica: il file veniva caricato in memoria, **per ogni riga veniva fatta la chiamata all’API** e il risultato veniva tenuto in memoria fino al termine delle operazioni poi salvato in un nuovo file CSV.

La soluzione, non certo elegantissima, ha un grosso problema: quando il file ha **dimensioni notevoli** il fatto di tenerlo in memoria può **saturare il server** su cui gira l’applicazione. 

<p style="text-align:center; min-width:100%;">
    <img src="https://media.giphy.com/media/PkAZ1JzOu4QH6/source.gif" style="min-width:100%; height: auto;">
</p>

E qui gli **stream risolvono il problema nel modo migliore possibile**.<br/>
Cominciamo col capire cosa è uno stream e quali vantaggi ha.

## Stream
In programmazione uno stream è un struttura dati che rappresenta una **sequenza di elementi resi disponibili uno dopo l’altro**.

Questo funzionamento ha una conseguenza abbastanza interessante. Supponiamo di avere una **lista di numeri**, uno stream “snocciolerà” la lista elemento per elemento **senza** avere la possibilità di **conoscere la dimensione della lista** o **accedere** direttamente all’elemento **i-esimo**.

L’implementazione corretta di uno stream composta da numeri interi **può essere infinita eppure non occupare “infinita” memoria** per poterla contenere. Questa caratteristica è attribuibile al fatto che gli elementi emessi da uno stream vengono **generati e valutati solo quando sono necessari** e non subito come con le liste “tradizionali”.
<p style="text-align:center; min-width:100%;">
    <img src="https://media.giphy.com/media/2LV8F7cqGunUA/source.gif" style="min-width:100%; height: auto;">
</p>

In Elixir i Range sono a tutti gli effetti degli stream
<br/><br/>
```elixir
iex>  1..42
1..42
```
<br/>
Un range come quello sopra rappresenta uno stream di numeri da 1 a 42. 
**Attenzione non è una lista che contiene tutti i numeri, non contiene nulla.**
<br/><br/>
```elixir
1..5311379928167670986858820655246862732959311
7727031923199444138200403559860852242739162502
2652292856688893294862465010153465793376527072
3940951997876658735194383127083539321903172812
```
<br/>
è uno stream valido nonostante il numero di elementi.

Con Elixir per lavorare con uno stream e compiere delle operazioni possiamo utilizzare il modulo [Stream](https://hexdocs.pm/elixir/Stream.html).<br/>
Il modulo contiene una serie di **funzioni che operano su uno stream**: ad esempio si possono prendere tutti i numeri generati da uno stream e raddoppiarli utilizzando la funzione Stream.map
<br/><br/>
```elixir
iex> 1..10 |> Stream.map(&(&1 * 2))
#Stream<[enum: 1..10, funs: [#Function<48.48559900/1 in Stream.map/2>]]>
```
<br/>
E qui riusciamo bene a vedere l’altra interessante caratteristica, alcuni di voi  forse si aspettavano che risultato dell’operazione sopra fosse una lista contenente i numeri 2, 4, 6, 8, ... invece il **risultato è uno stream**.
<br/>
Il motivo è che gli stream vengono valutati in modalità **lazy**, ossia solo quando effettivamente richiesto, questo vuol dire che lo stream non ha eseguito nessuna operazione. Anche se andassimo ad aggiungere un inspect per “vedere” l’esecuzione otterremmo questo:
<br/><br/>
```elixir
iex> 1..10 |> Stream.map(&(&1 * 2)) |> Stream.map(&IO.inspect/1) 
#Stream<[
  enum: 1..10,
  funs: [#Function<48.48559900/1 in Stream.map/2>,
   #Function<48.48559900/1 in Stream.map/2>]
]>
```
<br/>
Ossia uno stream con all’interno le due funzioni di elaborazione.
Per eseguire lo stream e fare effettivamente qualcosa dobbiamo utilizzare la funzione **Stream.run**:
<br/><br/>
```elixir
iex> 1..10 |> Stream.map(&(&1 * 2)) 
           |> Stream.map(&IO.inspect/1) 
           |> Stream.run
2
4
6
8
10
12
14
16
18
20
:ok
```
<br/>
Quindi fino a quando non passo lo stream alla funzione run lo stream è una struttura dati che contiene il set di operazioni da eseguire ma non esegue nulla. 
Ci sono delle similitudini con la **programmazione funzionale**: programmi come strutture dati passati tra le funzioni e mandati in esecuzione quando serve.

## Let's Go: the birthday greetings kata
Ora che abbiamo visto brevemente cosa sono gli stream vediamo come utilizzarli in un contesto reale. Prendiamo come esempio il problema citato all’inizio dell’articolo con alcune modifiche per renderlo piu’ comprensibile.
Consideriamo il caso in cui si debba scaricare da **Amazon S3 un file CSV** contenente un elenco di persone (nome, data di nascita, email) e che **vada inviata una mail per ogni persona che compie** gli anni oggi (ok, è il [The birthday greetings kata](http://matteo.vaccari.name/blog/archives/154), ma il succo non cambia)


### Scaricare un file da S3
Il primo step del nostro programma prevede di scaricare da S3 il file da elaborare. Questa operazione puo’ essere implementata **scaricando l’intero contenuto del file in memoria** ma per ottimizzare l’uso della memoria **iniziamo la lettura da S3 creando uno stream**:
<br/><br/>
```elixir
def get_lines(filename) do
 %{bucket: "test", path: filename, opts: [chunk_size: 1024]}
    |> Download.build_chunk_stream(config)
    |> Stream.map(&Download.get_chunk(op, &1, config))
    |> Stream.transform("", fn {_, chunk}, acc -> 
                            chunk_to_lines(chunk, acc) 
                            end)
    |> Stream.map(fn line ->
      [name, date, email] = String.split(line, ",")
      %{name: name, birth_date: date, email: email}
    end)
end
```
<br/>
Questo pezzo di codice fa uso della la **libreria [ExAws.S3](https://hexdocs.pm/ex_aws_s3/ExAws.S3.html) per interfacciarsi con le API di S3**. La libreria contiene già al suo interno alcune funzioni che creano uno stream di lettura (o di scrittura) di un file (Download.build_chunk_stream).

La complessità di questo codice è nascosto nella funzione `chunck_to_lines` che risolve il problema di **trasformare uno stream di dati destrutturato in uno stream di “righe”** per poter processare il file CSV in modo corretto (ci aspettiamo che le funzioni successive ricevano in ingresso una struttura dati che rappresenta una riga del file). Per ottenere uno stream di righe del file CSV dobbiamo in qualche modo **bufferizzare gli eventuali resti** delle righe precedenti.
<br/><br/>
```elixir
defp chunk_to_lines(chunk, prev) do
  [last_line | lines] = Enum.reverse(String.split(prev <> chunk, "\r\n"))

  if String.ends_with?(last_line, "\r\n") do
    {Enum.reverse([last_line] ++ lines), ""}
  else
    {Enum.reverse(lines), last_line}
  end
end
```
<br/>
La funzione `chunk_to_lines` riceve in ingresso il **chunk corrente** (ossia “il pezzo” dello stream che è appena stato letto) e l’eventuale **resto del chunk precedente**.  La funzione manipola lo stream pervenuto splittandolo in righe (\r\n) e ritorna la riga letta e l’eventuale rimanenza che verrà gestita nel prossimo chunk.

Quindi l’output della funzione sopra è uno **stream di strutture dati (hashmap)** cosi strutturate:
<br/><br/>
```elixir
%{name: "emanuele", date_of_birth: "1973-04-09", email: "e@test.com"}
```

Queste strutture devono entrare nel secondo blocco che si occupa di processare lo stream:
<br/><br/>
```elixir
"test_file.csv"
  |> get_lines()
  |> Stream.drop(1)
  |> Stream.drop_while(&is_not_birthday/1)
  |> Stream.map(&send_mail/1)
  |> Stream.run
```
<br/>
Analizziamo i singoli step di questa pipeline.
La `get_lines` abbiamo già visto ritorna uno stream di strutture dati da processare.
Lo step successivo rimuove dallo stream un elemento, nel caso specifico si tratta dell’header presente nel file. Viene scartato perché non interessa agli step successivi.

Il primo elemento valido viene passato alla funzione `Stream.drop_while` che filtra gli elementi applicando la funzione `is_not_birthday`. Quindi rimuove dallo stream le persone che non compiono gli anni (is_not_birthday controlla la data di nascita con il giorno corrente).

Gli elementi rimasti entrano nello step successivo che usa la funzione `Stream.map` per applicare la funzione `send_mail` a tutte le persone rimaste nello stream.

Prima di parlare dell’ultimo step è bene ricordare che fino a questo punto **non è stato eseguito ancora nulla**, quello che abbiamo costruito è una pipeline di operazioni che partono dalla lettura di un file su Amazon S3, questo insieme di operazioni possono essere passate ad altre funzioni e messe in esecuzione.

La funzione `Stream.run` è proprio quella che esegue lo stream e fa partire le operazioni **avviando il download e facendo passare gli elementi letti nella pipeline**.
Anche se il file avesse dimensioni importanti, a parte l’attesa per processarlo tutto, la memoria del server su cui gira l’applicazione non verrebbe sollecitata e **le mail inizierebbero ad arrivare mano a mano che le righe del file CSV di partenza vengono processate**. 

<p style="text-align:center; min-width:100%;">
    <img src="https://media.giphy.com/media/3oEduHMN7pshESmP5e/source.gif" style="min-width:100%; height: auto;">
</p>

Non c’è accumulo, non c’è occupazione di memoria ma solo un **flusso di operazioni che vengono eseguite in sequenza* (In realtà vista l’indipendenza delle righe si potrebbero anche parallelizzare le operazioni più lente per scalare e migliorare le performance, magari in un prossimo post…).

Gli stream sono uno strumento **davvero utile e versatile ma purtroppo dimenticato e poco utilizzato**. Buona parte delle applicazioni che scriviamo traebbero enormi benefici dall’uso sapiente di uno o più stream: pensate ad un API che invia i dati al client creando uno stream.
Il protocollo HTTP supporta lo streaming, molti driver di accesso ai database hanno la possibilità di stremare i dati letti, il file system e la rete sono degli stream... **non rimane che sperimentare**.