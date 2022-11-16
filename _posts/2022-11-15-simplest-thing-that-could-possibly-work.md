---
layout: post
current: post
cover: 'assets/images/post-cover/2022-11-simplest-thing.jpg'
socialcover: 'assets/images/post-cover/2022-11-simplest-thing-s.jpg'
slug: the-simplest-thing-that-could-possibly-work
title: 'The simplest thing that could possibly work'
date: 2022-11-15 09:00:00
category : [tech]
tags: [agile, coding, ddd]
author: [ema]


---
<div class="post-intro">La storia narra che, negli anni 90, Ward Cunningam e Kent Beck stavano lavorando in pair all’implementazione di una feature e si trovarono davanti ad un problema di difficile soluzione. Fu qui che Ward disse a Kent:

“Fai la cosa più semplice che possa funzionare.”

Questa frase è diventata uno dei mantra delle metodologie agili, tant’è che la semplicità è un valore, e trovare soluzioni semplici è una cosa da perseguire con dedizione durante lo sviluppo delle nostre applicazioni.</div>

<figure style="text-align:center"><img src="/assets/images/post-content/simplest-thing/simplest-thing_l_001.png" alt="coderdojo" />
</figure>

**Allora perché ci troviamo 30 anni dopo a parlarne ancora?**

Facciamo un passo indietro e capiamo perché la semplicità è importante. Partiamo da un dato di fatto: chi programma passa molto più tempo a modificare codice esistente piuttosto che ad aggiungere nuovo codice (sapete che le modalità di VIM sono nate proprio per questo motivo?)
In termini di costi se il codice sul quale dobbiamo apportare modifiche è semplice il costo delle modifiche rimarrà contenuto e tenderà a non crescere (troppo) col tempo.

<figure style="text-align:center"><img src="/assets/images/post-content/simplest-thing/simplest-thing_s_001.png" alt="coderdojo" />
</figure>

**Ma come si definisce il codice semplice?**

So che Martin Fowler scrisse la famosa frase “*Any fool can write code that a computer can understand. Good programmers write code that humans can understand”*, ma sono certo che gli umani che lui cita siano programmatori, quindi con la giusta conoscenza della grammatica del linguaggio di programmazione che stanno usando.

Possiamo affermare che il codice sia semplice quando:

- E’ facile da leggere
- E’ facile da capire
- E’ facile da cambiare, mantenere e/o cancellare.

<figure style="text-align:center"><img src="/assets/images/post-content/simplest-thing/simplest-thing_s_002.png" alt="coderdojo" />
</figure>

**Facile da leggere**
Si spiega da sé e, secondo me, va considerato anche il bilanciamento visivo: evitare codice troppo denso ed evitare righe di codice troppo lunghe.
Sono storicamente non troppo favorevole alle fluent interface: `Assert(x > 42)`  è molto più leggibile di `Assert.Than(x).Is().Greater().Than(42)`

<figure style="text-align:center"><img src="/assets/images/post-content/simplest-thing/simplest-thing_s_003.png" alt="coderdojo" />
</figure>

**Facile da capire**
Si tratta di un concetto più complesso del precedente e dipende da diversi fattori: l’idea è che, leggendo un pezzo di codice all’interno di un’applicazione, dovrei capire che ruolo ha quel pezzo di codice e cosa fa all’interno del contesto applicativo.

<figure style="text-align:center"><img src="/assets/images/post-content/simplest-thing/simplest-thing_s_004.png" alt="coderdojo" />
</figure>

**Facile da cambiare, mantenere e/o cancellare**
Riguarda la gestione dell’applicazione nel tempo e si affronta con il disaccoppiamento e la modularizzazione per poter apportare modifiche e nuove funzionalità nella codebase esistente.

<figure style="text-align:center"><img src="/assets/images/post-content/simplest-thing/simplest-thing_l_002.png" alt="coderdojo" />
</figure>

**Torniamo alla domanda di partenza**

Se la definizione è chiara per quale motivo si deve ancora parlare di simple code? Perché ancora oggi molte scelte architetturali sono notevolmente complesse anche in scenari in cui non è necessario.

Secondo me non c’è un’unica causa: possiamo citare l’esperienza del team, la fretta di consegnare, la voglia di sperimentare nuove architetture per rendere il progetto interessante oppure la sottovalutazione delle complessità di certe scelte.

Un esempio pratico è usare CQRS/ES in contesti in cui l’applicazione è una semplice CRUD senza che esista una vera logica applicativa e neppure la necessità di scalare su milioni di utenti.
Vi è mai capitato? Avete presente quanto è complesso realizzare e gestire, debuggare, deployare un’applicazione che ragiona completamente ad eventi?
Altro esempio sono le applicazioni a Microservizi, termine molto di moda, bellissimo concetto, ma a che costo? E i vantaggi che porta sono realmente indispensabili nel vostro contesto?
Entrambi gli esempi rientrano nel concetto di “Big Design Upfront” che è in se un anti-pattern che porta a soluzioni sovradimensionate e ad inevitabili complicazioni.

<figure style="text-align:center"><img src="/assets/images/post-content/simplest-thing/simplest-thing_s_005.png" alt="coderdojo" />
</figure>

**Possiamo fare diversamente? Forse sì…**

Ho usato più volte la parola **contesto** e, in effetti, la somma delle condizioni di contorno, gli obiettivi, i requisiti (funzionali e non funzionali) determina la situazione nella quale devo prendere le decisioni quindi, a volte, la scelta architetturale fatta in questo momento può giustificare Microservizi o altre complessità, ci sono casi ben noti in cui hanno senso.

In molti casi invece il contesto non è subito chiaro, non lo sono le specifiche né gli obiettivi e ai clienti piacerebbe essere Amazon anche se oggi sono più simili ad un piccolo negozio. Proprio per questo, da bravi architetti, dovremmo puntare ad una soluzione che permette di evolvere e supportare scenari futuri più complessi, evitando di introdurre sovrastrutture fin dall’inizio del progetto.

<figure style="text-align:center"><img src="/assets/images/post-content/simplest-thing/simplest-thing_s_006.png" alt="coderdojo" />
</figure>

**Un approccio, due livelli di azione**

A livello progettuale si può scegliere di non partire subito con architetture complesse come CQRS/ES o con strutture a Microservizi, ma partire con soluzioni più semplici che possono, eventualmente in futuro, evolvere verso CQRS/ES o Microservizi qualora ce ne sia la reale necessità (scriveremo altri articoli sul tema per spiegare meglio cosa si intende).

A livello pratico invece si può usare il **TDD** insieme a Domain Driven Design per costruire applicazioni che rispettino i tre principi del codice semplice. Il TDD è lo strumento con cui scrivere codice leggibile, mantenibile e che si capisca: far crescere la codebase test dopo test e fare continuo refactoring sono i due ingredienti indispensabili.
L’aspetto del TDD che permette di tenere il codice semplice credo sia l’approccio outside-in: progettare le classi e le funzioni da utilizzatore, definirne l’interfaccia dall’esterno, decidere come eseguire certe azioni e come chiamare le classi e i metodi.

Il **DDD** invece è uno strumento utilissimo per progettare un dominio consistente con i requisiti di business ma, soprattutto, per identificare i contesti e gli aggregati: due elementi che permettono di definire i boundaries (logici) della nostra applicazione. Fondamentale non dimenticare l’utilizzo di un ubiquitous language per dare i giusti nomi ai costrutti (ad esempio le classi e le funzioni, ma non solo).

Ho parlato di questi e altri temi all’**agile day 2022** che si è tenuto a Brescia a fine ottobre.
Se vi ho incuriosito su qual è la cosa più semplice che possa funzionare, potete trovare [qui le slide](https://www.slideshare.net/emadb/the-simplest-thing-that-could-possibly-work) e [qui il video](https://vimeo.com/768884921).