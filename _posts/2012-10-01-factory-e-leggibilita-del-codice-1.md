---
layout: post
title:  "Factory e leggibilità del codice (1/2)"
comments: true
categories: Technology
---


Uno degli obbiettivi a cui, sempre più spesso, cerco di porre attenzione, nella scrittura del codice, è la leggibilità: il codice che scrivo deve essere auto-esplicativo sia per &#8220;futuri lettori&#8221;, sia perchè la mia memoria (ormai troppo a corto raggio), non mi permette di ricordare il giorno successivo perchè il giorno precedente ho scritto quelle righe di codice&#8230;sarà l&#8217;età :-S

Questo assunto è sempre vero e sempre applicabile, ma diventa fondamentale (IMHO) quando la parte in gioco è il cuore del software che si sta sviluppando: il domain model.

Immergendoci nel mondo DDD (e nel suo UL) e prendendo in considerazione la fase di creazione di un aggregate root, le motivazioni per cui viene creata una nuova istanza di un oggetto sono essenzialmente due:

- 
la creazione vera e propria di una nuova istanza

- 
la reidratazione di un&#8217;istanza già persistita nel &#8220;data store&#8221;



Cerchiamo di analizzare il primo punto sviscerando come, cosa e perchè di un concetto abbastanza sottovalutato in DDD, ma che, a fronte della premessa di questo post, è per me di fondamentale importanza: la factory.

Per prima cosa distinguiamo il concetto di [factory visto come &#8220;pattern&#8221; (GoF)](http://www.dofactory.com/Patterns/PatternFactory.aspx), dal punto di vista della factory &#8220;vista&#8221; in ottica DDD: il libro, analizza il pattern sotto un punto di vista tecnico (come e quando implementare un approccio del genere), la factory in DDD approccia la questione, inquadrando l&#8217;aspetto di creazione degli aggregate da un punto di vista semantico del modello.

La costruzione di un oggetto avviene attraverso il costruttore. E fin qui non ci piove. Ma pensiamoci un attimo: è giusto dare ad un oggetto le competenze per &#8220;autosetupparsi&#8221; nel modo corretto? A maggior ragione se quell&#8217;oggetto è un aggregate root?
Ovviamente la risposta non c&#8217;è, ma, personalmente parlando, non è una pratica che mi piace molto e, dicendola tutta, per me vìola l&#8217;[SRP principle](http://en.wikipedia.org/wiki/Single_responsibility_principle).
Ecco il primo motivo per cui introduco, spesso e voleventieri, la factory in stretta relazione ad un aggregate: la factory è l&#8217;unico punto del mio codice che sa come portare un aggregate root in uno stato consistente, prima di essere utilizzato.

L&#8217;altro aspetto, ancora più importante, anzi diventato quasi predominante è il rendere il codice il più esplicativo possibile: tramite il costruttore non ho modo di rendere evidente la semantica dell&#8217;operazione che sto eseguendo, tramite una factory ho la possibilità di creare più metodi che hanno lo stesso obbiettivo, creare un aggregate root, ma entry point potenzialemente differenti.

Un po&#8217; di codice, che non guasta mai, vale più di mille parole:

```

```

In questo modo, utilizzando il costruttore, il perché ci sia la necessità di creare una nuova istanza di ordine è assolutamente nascosta a chi leggerà il nostro codice. E questo non è bello&#8230;

Introducendo la factory, tutto torna ad essere molto più parlante:

```

```

Con l&#8217;ingresso in scena della factory, abbiamo poi la possibilità di sfruttare il polimorfismo nella fase di creazione degli oggetti, cosa che, ovviamente, con il solo costruttore non possiamo fare.
Fin qui mi sembra non ci sia nulla di nuovo e innovativo :-) Ma se siamo d&#8217;accordo fino a qui posso procedere con più serenità.

Ho volutamente lasciato i più generici possibile i parametri del primo metodo (e  &#8221;Object[] params&#8221; mi sembra abbastanza generico), perchè anche i parametri dei metodi della factory ricoprono un ruolo fondamentale nella leggibilità del codice&#8230;ma di questo parliamo la prossima volta.

