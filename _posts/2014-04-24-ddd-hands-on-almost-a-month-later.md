---
layout: post
title:  "DDDHandsOn - (quasi) un mese dopo"
categories: tech
---

Dopo il necessario mese di decompressione delle idee e delle sensazioni (o meglio questa e' la scusa migliore che ho trovato per giustificare il ritardo del post) posso dire che l'esperimento che abbiamo provato con lo [@ziobrando] (http://twitter.com/ziobrando) e il [team di Avanscoperta] (http://www.avanscoperta.it/en) e' assolutamente riuscito. Come "quale esperimento"? Ma il [workshop su DDD] (http://www.avanscoperta.it/it/training/hands-on-domain-driven-design-workshop-2014-2/)

Il tema era ovviamente uno di quelli a me tanto cari: Domain Driven Design, ma visto con gli occhi dello sviluppatore e soprattutto con "le mani" di chi, ogni giorno, deve "pigiare tasti" e risolvere problemi.

Tutto parte da una giornata "preliminare" di [#eventstorming] (http://ziobrando.blogspot.it/2013/11/introducing-event-storming.html#.U1jA1_mSyrI) in cui, con la "schiettezza" di un approccio visuale, si sviscera un (qualunque) processo di business, evidenziando cio' che accade (o dovrebbe accadere) nel sistema e facendo emergere ogni possibile criticita', garantendoci, "out-of-the-box" una visione molto dettagliata di quello che dovremo andare ad implementare. Nei due giorni seguenti si passa alla fase operativa. 

In prima battuta, per l'appuntamento di Roma, si era pensato di partire da una infrastruttura di base minimale, ma che permettesse a tutti di avere gia' del codice su cui lavorare durante i giorni del workshop, in modo da non doversi soffermare solo su dettagli tecnici, ma potersi spingere un po' piu' a fondo nei meandri di una possibile implementazione di CQRS, di un EventStore, e magari, perche' no, arrivare a capire/intravedere quali sono gli approcci da seguire, gli scenari e le problematiche in cui ci si imbatte nel momento in cui si ha a che fare con un sistema veramente distribuito.

Nella seconda puntata, quella di Bologna, abbiamo fatto un passetto indietro: zero codice preparato, si crea tutto "from scratch". Questo ci ha permesso di coprire una platea tecnologicamente piu' eterogenea (java, .Net, nodejs...) e permettere ad ognuno di rimanere nella propria comfort zone tecnologica.
Mani sulla tastiera, in (quasi) modalita' randori e...l'approccio si e' rivelato assolutamente vincente.  
Essenzialmente per due motivi: non essendoci una guideline da seguire, ognuno aveva carta bianca sulle possibili soluzioni. E quando metti nella stessa stanza otto teste pensanti quello che ne "salta fuori" e' sicuramente una due giorni di qualita'...e poi, in questo modo, io non ho dovuto preparare nulla :)  
Evento dopo evento, comando dopo comando, ma soprattutto test dopo test, abbiamo iniziato a dare risposte a quello che il business, evidenziato nella prima giornata, ci chiedeva.

Come al solito, in questi casi, le parti a corollario (colazione, pranzo e cena) non sono meno importanti: insomma una 48 ore non stop su DDD, CQRS ed EventSourcing (e MongoDB, Azure, RabbitMQ, ServiceBus, sistemi distribuiti...) che mi hanno veramente soddisfatto. Mi sono divertito un sacco e sentirsi dire "ma allora si puo' fare" e' stata la soddisfazione migliore. Abbiamo centrato il bersaglio.

Che dire poi del "compagno di merende"? @ziobrando lo conoscete: divertente, simpatico, chiacchierone...ah si, dimenticavo...e' anche un sacco preparato sull'argomento :)

Presto si replica e altre cose interessanti sono in cantiere...quindi, che dire: stay tuned!!!