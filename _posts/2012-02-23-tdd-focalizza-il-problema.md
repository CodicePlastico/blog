---
layout: post
title:  "TDD: focalizza il problema"
comments: true
categories: Technology
---


Prendo spunto da un [post](http://blog.codiceplastico.com/ema/?p=229) del [socio](http://blog.codiceplastico.com/ema), per dire la mia (non me ne voglia), in merito al TDD e soprattutto ad alcuni dei suoi vantaggi &#8220;intrinseci&#8221;.

Il ciclo &#8220;red-green-refactor&#8221;, ai più ormai noto, è, al primo impatto, molto semplice: parti da un test rosso e implementa il codice, il più velocemente possibile, per farlo diventare verde. Solo dopo procedi a colpi di refactoring, a migliorare ciò che hai scritto.
Questa frase porta con se alcuni &#8220;hidden-statements&#8221; che nascondono alcuni dei migliori side-effect del TDD. Uno di questi è la **focalizzazione** del problema.

L&#8217;utilizzo del TDD mi permette di definire, in modo chiaro ed inequivocabile, da dove parto e dove voglio arrivare, magari non seguendo, in prima battuta, la strada migliore. E quindi porre l&#8217;attenzione su uno specifico aspetto, senza far deragliare il mio cervello alla ricerca di soluzioni pseudo-alternative.

Il rischio di sovraingegnerazione è dietro l&#8217;angolo e divergere dall&#8217;obbiettivo preposto, è un costo che è difficile da ammortizzare. L&#8217;utilizzo del TDD è &#8220;pippa-free&#8221;: fai quel che serve alla tua applicazione, in prima battuta, nel modo più smart possibile. Nulla vieta, anzi è auspicato, in un secondo momento (la fase di refactoring, appunto), di affinarne l&#8217;implementazione.

Un altro di quegli aspetti che fanno del TDD una metodologia ad alto valore aggiunto è quello di perseguire **la semplicità**. L&#8217;utilizzo di questa metodologia porta con se una metrica molto importante: maggiore è il tempo di &#8221;latenza&#8221; in cui un test rimane rosso è un sintomo, abbastanza preciso, di alta complessità.
Avere un feedback molto rapidamente sul fatto che la strada intrapresa è troppo tortuosa e quindi difficilmente perseguibile, è un indicatore da non sottovalutare e che permettere di farci intestardire in soluzioni &#8220;senza futuro&#8221;.

Insomma&#8230;va bene il design, va bene la batteria di test implicita che andiamo via via a creare&#8230;ma anche il contorno (o forse soprattutto quello), fanno del TDD uno &#8220;strumento&#8221; sempre più indispensabile nel mio lavoro.

