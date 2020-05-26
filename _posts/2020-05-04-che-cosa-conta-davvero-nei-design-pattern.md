---
layout: post
current: post
cover: 'assets/images/post-cover/20200504-design-pattern.jpg'
socialcover: 'assets/images/post-cover/20200504-design-pattern-s.jpg'
slug: che-cosa-conta-davvero-nei-design-pattern
title: 'Che cosa conta davvero nei design pattern'
date: 2020-05-04 00:00:00
category : tech
tags: [design pattern, illustrazioni]
author: [pietro, andrea]
---

<code>Un post di Pietro Martinelli, con le illustrazioni di Andrea Gadaldi </code>
<br/><br/>

Una delle attività di cui mi occupo in CodicePlastico consiste nell'organizzare e tenere sessioni di consulenza e formazione su varie tematiche, alcune più tecnologiche (Docker, MongoDb, Git, Java, ...) ed altre più teoriche (Design Pattern, Principi di Design, Testing).
Questo mi dà l'occasione di riflettere da punti di vista sempre diversi su questioni che sono il pane quotidiano di un ingegnere del software; il tema dei *[design pattern](https://en.wikipedia.org/wiki/Software_design_pattern)* è uno di questi - e vorrei cogliere l'occasione di questo post per condividere una consapevolezza che ho maturato di recente in proposito.

Come da definizione di [Wikipedia](https://en.wikipedia.org/wiki/Software_design_pattern), un *design pattern* è
<cite>una soluzione generica, riutilizzabile ed accettata ad un problema di design comune e ricorrente</cite> 
si tratta cioè di una sorta di *template* che può essere d'aiuto nel risolvere un problema di una *classe* nota.

<h3 style="color:#D93232">Cominciamo dal "Principio"</h3>

<figure class="image">
  <img src="/assets/images/post-content/design-pattern-1.jpg" alt="Il Catalogo">
  <figcaption>Catalogo</figcaption>
</figure>

Quando ne parlo, cerco di insistere sul fatto che, al di là del nome che diamo ai *pattern* presi da questo o da quel *catalogo* famoso, al di là del ricordare o meno i dettagli di design che ciascun *pattern* suggerisce, è interessante notare come, dallo studio di *pattern* comunemente accettati, emerga una serie di **principi astratti** la cui applicazione sistematica fa emergere le scelte di design descritte nei *pattern*: l'idea è che chi cercasse di risolvere lo stesso problema che rappresenta il *contesto* di un *pattern* senza sapere nulla di *pattern* ma applicando quei **principi** giungerebbe con ogni probabilità ad una soluzione identica - o molto simile - a quella che il *pattern* in questione standardizza.

<figure class="image">
  <img src="/assets/images/post-content/design-pattern-2.jpg" alt="grasp e solid">
 
</figure>

I **principi** cui faccio riferimento sono, tra gli altri, le linee guida [GRASP](https://en.wikipedia.org/wiki/GRASP_(object-oriented_design)) per l'assegnazione di responsabilità ed i [principi SOLID](https://en.wikipedia.org/wiki/SOLID).


Se dunque la conoscenza e l'applicazione di principi astratti porta all'adozione di soluzioni di design analoghe a quelle suggerite da *design pattern* più o meno noti, qual è il motivo per cui è importante studiare, analizzare ed interiorizzare i *cataloghi* di *pattern* più noti? 
 <figure class="image"> <img src="/assets/images/post-content/design-pattern-3.jpg" alt="Vocabolario">
  
</figure>
<h3 style="color:#D93232">Costruire un vocabolario comune</h3>
Per come la vedo io, **il motivo principale** consiste nell'acquisizione e nella disponibilità di un ***framework di comunicazione***: se parlo ad un collega o ad un cliente di *decorator* la mia comunicazione è molto più rapida, precisa ed efficace di quanto non sarebbe se parlassi di *scrivere due implementazioni di una stessa interfaccia, facendo in modo che una delle due lavori sull'input o sull'output dell'altra senza che il client dell'interfaccia lo sappia*. Se parlo di *object adapter* chi mi ascolta sa di che cosa parlo, sa della necessità di implementare un'interfaccia estendendo una classe che già ne implementa, con interfaccia diversa, le responsabilità, e sa quali vantaggi ci sono rispetto all'adozione di un design a *class adapter*.
<figure class="image">
  <img src="/assets/images/post-content/design-pattern-4.jpg" alt="Vocabolario">
  
</figure>
Un *catalogo* di *design pattern* è dunque, prima di tutto ed essenzialmente, un **vocabolario comune**, ogni elemento del quale facilita la comunicazione e la condivisione ed evoca - o dovrebbe evocare ;-) - una serie di dettagli mnemonici su contesto, pro e contro della soluzione nominata: dare un nome a qualcosa, in fondo, è il primo passo sulla strada che porta a controllarla.
<figure class="image">
  <img src="/assets/images/post-content/design-pattern-5.jpg" alt="Vocabolario">
  
</figure>

<h3 style="color:#D93232">Fotografare gli intenti</h3>

**Il secondo motivo** per cui penso sia importante, se non fondamentale, disporre di un *framework concettuale* come quello reso disponibile dalla conoscenza di un *pattern* consiste nel fatto che dare un nome ad una soluzione ne *fotografa* in qualche modo l'**intento**, aspetto importante almeno quanto il dettaglio della soluzione stessa.

Se poniamo l'accento su questo aspetto, infatti, ci rendiamo conto di come sia quasi immediato individuare fortissimi similitudini strutturali tra i *design* proposti da *pattern* diversi, benché l'**intento** di ciascuno di essi sia diverso.
<figure class="image">
  <img src="/assets/images/post-content/design-pattern-6.jpg" alt="Vocabolario">
  <figcaption>State & Strategy</figcaption>
</figure>
*State* e *strategy*, ad esempio, sono due *pattern* "gemelli", identici nella struttura di design suggerita ma chiaramente differenti nell'intento: organizzare in classi diverse la gestione dei possibili stati di una [FSM](https://en.wikipedia.org/wiki/Finite-state_machine) nel primo caso, supportare la possibilità di *pluggare* l'implementazione di un algoritmo nel secondo (cfr. [5]).
<figure class="image">
  <img src="/assets/images/post-content/design-pattern-7.jpg" alt="Vocabolario">
  <figcaption>Decorator, Proxy & Chain of Responsibility</figcaption>
</figure>
Allo stesso modo, *decorator*, *proxy* e *chain of responsibility* condividono la struttura (n implementazioni di una stessa interfaccia, ciascuna delle quali possiede un riferimento ad un'altra implementazione) ma la sfruttano per supportare intenti diversi: arricchire le funzionalità di un'implementazione base senza modificarla (*decorator*), incapsulare logiche di accesso condizionato ad un'implementazione esistente (*proxy*), gestire la scelta dell'handler giusto per una richiesta in modo [OCP](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)-compliant (*chain of responsibility* o varianti come *[set of responsibility](https://javapeanuts.blogspot.com/2018/10/set-of-responsibility.html)*).
<figure class="image">
  <img src="/assets/images/post-content/design-pattern-8.jpg" alt="Vocabolario">
  <figcaption>Bridge & Object Adapter </figcaption>
</figure>
Ancora, *object adapter* e *bridge* condividono la struttura ma non l'intento: adattare un'implementazione esistente ad un'interfaccia nel primo caso, permettere ad interfaccia ed implementazione di variare indipendentemente nel secondo.

Ricapitolando:
- un *design pattern* è una soluzione nota e condivisa ad un problema ricorrente
- il design proposto da un *pattern* deriva da **principi più astratti**, l'applicazione dei quali porterebbe ad un design simile anche chi non fosse a conoscenza del *pattern* stesso
- l'importanza di un catalogo di *pattern* risiede dunque nella definizione di un **vocabolario condiviso**, che facilita la comunicazione individuando per ogni *pattern* un **intento**, che lo differenzia rispetto ad altri *pattern* con struttura identica o simile
<br/><br/><br/>
#### Riferimenti

[1] - [Software Dedign Patterns](https://en.wikipedia.org/wiki/Software_design_pattern) <br/>
[2] - [Design Patterns, Elements of reusable object-oriented software](https://en.wikipedia.org/wiki/Design_Patterns)<br/>
[3] - [GRASP](https://en.wikipedia.org/wiki/GRASP_(object-oriented_design))<br/>
[4] - [SOLID principles](https://en.wikipedia.org/wiki/SOLID)<br/>
[5] - [Head First, Design Patterns](https://www.amazon.com/Head-First-Design-Patterns-Brain-Friendly/dp/0596007124)<br/>
