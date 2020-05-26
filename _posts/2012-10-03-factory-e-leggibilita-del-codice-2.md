---
layout: post
title:  "Factory e leggibilità del codice (2/2)"
comments: true
categories: tech
---


Nel [post precedente](http://blog.codiceplastico.com/melkio/index.php/2012/10/01/factory-e-leggibilita-del-codice-1/) abbiamo visto come &#8220;addurre scuse plausibili&#8221; (cit.) per introdurre il concetto di factory all&#8217;interno del nostro domain model, come unico entry point per la creazione (e setup iniziale) dei nostri aggregate root.

Ci eravamo lasciati con l&#8217;idea di rendere più parlanti non solo i metodi di creazione, ma anche i parametri degli stessi, cercando di virare, perchè no, verso un DSL-light, con poco sforzo, che possa aumentare la leggibilità del codice. Per intenderci, passare da:

```

```

a qualcosa del tipo:

```

```

Direi molto, ma molto meglio&#8230;che dite?

Sfruttando alcune funzionalità &#8220;nascoste&#8221; (nel senso di poco, ma poco, utilizzate) di c#, il comportamento è facilmente raggiungibile.
Innanzitutto, dobbiamo oscurare il costruttore del nostro aggregate root, in modo che nessun altro, oltre alla factory, abbia la possibilità di istanziare il nostro oggetto.

Niente di più facile, ma ora come è possibile &#8220;abilitare&#8221; la factory alla costruzione dell&#8217;oggetto? Rendendo il costruttore &#8220;internal&#8221; permettendo a tutti gli altri componenti dello stesso assembly di potervi accedere? E se usassimo le [nested class](http://msdn.microsoft.com/en-us/library/ms173120(v=vs.80).aspx)?

Aggregate root e factory, per come li abbiamo definiti, vanno veramente a braccetto, entrambi fanno parte del modello e uno non ha senso di esistere senza l&#8217;altro. Quindi non lo vedo come un male che uno (la factory) abbia la possibilità di accedere ai membri nascosti dell&#8217;altro (aggregate root) per poterne fare il setup&#8230;è quello che ci serve in fin dei conti.

```

```

Se poi utilizzassimo anche la keyword [partial](http://msdn.microsoft.com/en-us/library/wa80x488(v=vs.80).aspx), potremmo tranquillamente &#8220;pulire&#8221; anche il layout del file, splittando aggregate root e factory su due file differenti.

Ora dobbiamo fare in modo di poter utilizzare la sintassi fluent nel metodo della factory. Creiamo un po&#8217; di codice, poco di concetto, ma molto utile ai fini dell&#8217;obbiettivo :-)

```

```

il più è fatto, e con poco sforzo la factory cambia nel modo seguente:

```

```

Obbiettivo centrato. Poco sforzo, massimo risultato.
Ben fatto!

