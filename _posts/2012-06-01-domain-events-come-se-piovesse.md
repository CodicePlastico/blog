---
layout: post
title:  "Domain events come se piovesse"
comments: true
categories: Technology
---


&#8220;&#8230;non c&#8217;è immagine, a qualsiasi risoluzione, che possa darti lo stesso contenuto informativo di un video&#8230;&#8221;

Non ricordo da chi ho sentito questa frase, ma calza a pennello con il concetto di domain events e di snapshot degli aggregate: il contenuto informativo che un domain event porta con sè è molto più alto rispetto ai soli &#8220;dati&#8221; che vengono esposti. Innanzitutto, il nome ha una informazione semantica implicita molto importante: ci dice cosa è accaduto in un dato istante ad una componente del nostro sistema.

Salvando semplicemente i dati del nostro aggregate su una tabella (o su una collection di #mongodb), perdiamo completamente le informazioni sul come siamo arrivati a quel punto, quali scelte/processi di business hanno determinato il ciclo di vita del nostro aggregate.

Introducendo il concetto di domain event, abbiamo poi il considerevole (e vantaggioso) side-effect di orientarci veramente verso un&#8217;architettura distribuita, dove ogni sistema (leggi bounded context) può ipoteticamente vivere di vita propria, senza dipendenze da altro/i.
Ma questo lo vedremo in un prossimo post :-)

Grossi domain model anemici (e grane conseguenti)&#8230;addio!

