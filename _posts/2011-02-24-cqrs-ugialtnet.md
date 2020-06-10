---
layout: post
title:  "CQRS @ UgiAltNet"
comments: true
categories: tech
tags: [cqrs, ddd]
---


Sabato si è tenuta la UgiAltNet conference: ottima conferenza, complimenti ancora a tutti gli organizzatori.

Questa volta, oltre che partecipare, ho avuto il piacere di tenere una sessione su CQRS ed EventSourcing. Come al solito l&#8217;argomento ha destato parecchio interesse, ma ha sollevato anche parecchi dubbi.

Il problema, secondo il mio punto di vista, non risiede tanto in come CQRS modifica il nostro modo di pensare e sviluppare applicazioni, ma sta alla base: come implementiamo (o crediamo di implementare) DDD?
Il grosso problema, come dicevo sabato e come è spesso capitato anche a me, è che si crede di fare DDD, ma invece si è molto lontani da una forma &#8220;razionale&#8221; di Domain Driven Design.

Sviluppando applicazioni in cui i behaviors in gioco sono tanti e, magari, anche complessi, non si può pensare di avere un unico modello che in garantisca la consistenza del suo stato in ogni istante temporale. Non sto dicendo che non è possibile, ma sto semplicemente dicendo che se così fosse non stiamo facendo DDD. Soprattutto non stiamo dando al nostro modello l&#8217;agilità necessaria per evolvere mano a mano che la nostra conoscenza del business cresce, ma lo stiamo soffocando dietro a forti vincoli.

Ragionare in ottica di behaviors e non di state (o meglio non solo di state), ovvero fare veramente DDD, è la vera rivoluzione, non tanto CQRS che, in un ecosistema di aggregate, bounded context e domain event, si innesta naturalmente.

Che ne pensate?

[DDD](http://technorati.com/tag/DDD), [CQRS](http://technorati.com/tag/CQRS), [Conf](http://technorati.com/tag/Conf)

