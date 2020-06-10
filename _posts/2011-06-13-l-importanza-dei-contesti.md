---
layout: post
title:  "L'importanza dei contesti"
comments: true
categories: tech
tags: [database, ddd]
---


Sollecitato dai commenti di [Davide](http://www.davidemauri.it) e Francesco, riprendiamo il discorso del [post precedente](http://blog.codiceplastico.com/melkio/2011/06/09/questo-database-amico-nemico/) e cerchiamo di analizzare e sviscerare un po&#8217; di più le situazioni che sempre più spesso mi capita di vedere.

Quello che Davide e Francesco &#8220;contestano&#8221; della visione descritta nel post precedente credo sia dettata da una loro visione data-centrica: il dato prima di tutto&#8230;e detta così potrebbe sembrare ovvia :-) Ma non è così semplice: il dato cambia, non solo nella sostanza, ma anche e soprattutto nella forma e ciò che ne delinea i contorni sono i processi di business che le nostre applicazioni modellano.
Guardare il dato in un particolare istante significa fare uno snapshot dello stato di un processo&#8230;ma mentre lo stiamo guardando e analizzando quel dato potrebbe già essere vecchio, ma non solo&#8230;potrebbe anche avere una &#8220;forma&#8221; talmente vecchia da non essere più utile.

La mia idea non è quella di creare dei gateway verso i dati, ma delineare in modo marcato i confini dei processi in cui ogni dato acquisisce un particolare significato. E&#8217; solo attraverso il processo che si può capire il valore di un dato.
Chiunque debba interagire con i dati in lettura lo può fare attraverso le API che i processi metteranno a disposizione, ma soprattutto quando si deve interagire in scrittura l&#8217;unico modo per non correre il rischio di portare il sistema in uno stato inconsistente è quello di delegare le nostre richieste a chi le sa gestire. Questo attore non può essere il database, che ha (e deve avere) altri compiti.

In un ambiente distribuito questa &#8220;compartimentazione&#8221; è manna dal cielo: come afferma il [CAP theorem](http://en.wikipedia.org/wiki/CAP_theorem), non è possibile garantire contemporaneamente consistenza, &#8220;disponibilità&#8221; e tolleranza al partizionamento di rete (non so se si traduce così :-) )per un sistema distribuito. Garantire due di questi aspetti, in base alle esigenze, suddividendo l&#8217;intero sistema in contesti è la soluzione per semplificare il problema&#8230;ma di questo ne parliamo la prossima volta.

