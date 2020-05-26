---
layout: post
current: post

slug: pair-programming-obiettivo
cover: 'assets/images/post-cover/obiettivi-pair-programming.jpg'
socialcover: 'assets/images/post-cover/obiettivi-pair-programming-s.jpg'
title: "Pair programming: qual è l’obiettivo?"
date: 2017-10-25 14:00:00
categories: tech
tags: [pair programming]
author: paolo
---


<div style="text-align: center">
 <img src="/assets/images/post-content/why.png" alt="why">
 
</div>
<br />
<br />
Il *pair programming* è uno degli strumenti più consueti all’interno della cassetta degli attrezzi di un team Agile.
Su questa pratica si è discusso tanto in passato (ed ancora oggi) soprattutto su quali siano i suoi reali vantaggi e come essa sia meglio metterla in atto.

Io in questo post **non** mi soffermerò su questi due aspetti.
Invece vorrei mettere l’accento su un ulteriore questione che spesso viene dimenticata o quanto meno fraintesa. Mi riferisco all’obiettivo che si intende raggiungere.

Cosa intendo?

Quando si sceglie di adottare una pratica di sviluppo solitamente non lo si fa perché ce lo prescrive il dottore. La si mette in opera perchè si presume di trarne dei vantaggi.
Parlando di *pair programming*, per la mia esperienza, ho spesso visto usarla per soddisfare due esigenze:
<ul>
<li>
migliorare la qualità del codice scritto e la velocità con cui le rispettive feature vengono rilasciate: il feedback loop estremamente ridotto tra i due programmatori permette di correggere molti errori non appena questi nascono avendo come effetto collaterale positivo l’aumento della velocità di rilascio;
</li><li>
formare sul campo programmatori meno esperti **[*]** (sia su conoscenze tecnologiche, sia su competenze business di dominio): il programmatore più esperto può spiegare “in diretta” il codice e il design del software colmando lacune tecniche o di business del programmatore meno esperto. Allo stesso tempo il più esperto può fornire un feedback immediato riguardo al codice prodotto dall’altro programmatore.
</li>
</ul>
Detto ciò, succede spesso che all’interno di un team di sviluppo le due esigenze di cui sopra siano contemporanee e perciò si decida di adottare il *pair programming* per “prendere due piccioni con una fava”.<br />
La realtà dei fatti, per la mia esperienza, è che questa aspettativa non potrà essere soddisfatta. <br/>Le due esigenze sono esattamente ortogonali e non vi è modo di ottenerle entrambe.

I motivi, a mio parere, sono i seguenti:

<ul>
<li>
se si vuole praticare *pair programming* per migliorare la qualità del codice e la velocità di rilascio è necessario che **i due programmatori siano entrambi esperti**. <br/>
Questo serve per permettere loro di mettere in atto quello scambio di opinioni e feedback atto ad individuare errori e risolvere efficacemente problemi assieme.
Se vi è uno squilibrio tra i due, il flusso della discussione tenderà spesso in un senso solo oppure si discuterà di questioni poco utili per risolvere il problema corrente;
</li><li>
se si vuole fare *pair programming* per scopo formativo allora va da sé che **il programmatore più esperto dovrà dedicare tempo a quello meno esperto** per spiegare concetti nuovi e rispondere alle domande del caso. <br />
Inoltre è bene far scrivere codice anche a chi ha meno esperienza il quale, per ovvie ragioni, tenderà a commettere più errori che dovranno essere compresi e discussi. <br />
Ciò comporterà un rallentamento nello sviluppo della feature corrente la quale dovrà essere più un pretesto per fare lavorare la coppia assieme invece che una necessità urgente da soddisfare il prima possibile.
</li><li>
Perciò, la mia opinione è la seguente: se si vuole “andare più veloce e con meno errori” non è possibile dedicare tempo alla formazione di base.
Se si vuole fare *pair programming* per formarsi, allora bisogna prendersi il tempo necessario per farlo senza avere grosse pressioni sulla qualità del codice prodotto e sui suoi tempi di rilascio. <br />
</li></ul>
Se si sceglie lo strumento *pair programming* e si chiariscono questi due concetti, a mio parere si avranno alte probabilità di ottenerne benefici. Altrimenti si correrà il rischio di frustrarsi e complicarsi inutilmente la vita da soli, magari dando colpa alla pratica stessa affermando che “non funziona…”.
<br />
<br />
<br />

**[*]** concordo sul fatto che la formazione avverrà anche tra programmatori equamente esperti ma si tratterà solamente di un effetto collaterale ben voluto, dovuto ad un proficuo scambio di opinioni. Inoltre in questo caso avverrà un affinamento delle competenze. La “grana” dei concetti appresi l’uno dall’altro sarà molto più fine rispetto ad una formazione intesa come apprendimento da parte di una persona poco esperta.
