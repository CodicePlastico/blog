---
layout: post
title:  "DDD, Aggregate e FK"
comments: true
categories: Technology
---


I concetti di aggregate e di aggregate root nell&#8217;ecosistema di domain driven design ricoprono un ruolo fondamentale: l&#8217;aggregate garantisce un raggruppamento logico delle varie entities e value objects. L&#8217;aggregate root è il &#8220;guardiano&#8221; e l&#8217;unico punto di accesso ad un aggregate, che identifica una singola unità di persistenza.
Il concetto di raggruppamento logico e quindi di aggregate permette di gestire insiemi di entities e value objects come se fossero un&#8217;unica entità.

Molto spesso è proprio la mancanza o la non corretta identificazione di aggregate e aggregate root a portare alla falsa sensazione di approcciare lo sviluppo dell&#8217;applicazione in DDD e, soprattutto, alla possibilità di evolvere facilmente il nostro dominio. Questo falso sentore si scontra, solitamente, con la realtà nel breve volgere di qualche iterazione.

In un&#8217;ottica DDD-oriented anche il database deve essere al servizio del nostro modello e non vincolarne la sua evoluzione imponendo dei limiti dovuti alla necessità di sottostare al modello relazionale: definendo un aggregate vengono implicitamente definiti i confini dell&#8217;unità minima di consistenza. Foreign key che valicano i confini di un aggregate sono il sintomo di un qualcosa che non va: bisogna imparare a pensare in termini di &#8220;eventual consistency&#8221; oppure ripensare e ridisegnare i confini dei nostri aggregate.

[DDD](http://technorati.com/tag/DDD)

