---
layout: post
title:  "Eventual consistency...ma de che?"
comments: true
categories: Technology
---

Ma cos'e' questa benedetta "eventual consistency" di cui tanto si parla? Inizio fermamente a credere che sia la nuova *buzzword* per classificare i failure dei sistemi come "feature by design" :)  

Ma cosa significa veramente "eventual consistency"? Potremmo discuterne per un tempo infinito colorando la definizione con tante sfumature diverse e tutte potenzialmente corrette. L'unica cosa di cui sono certo e' che  **non** vuole dire vedere/usare dati "ad minkiam" (cit.)  

Quando si ha a che fare con un sistema distribuito, non si puo' spingere indiscriminatamente sull'acceleratore nelle tre direzioni: [consistency, availability e partition tolerance](http://en.wikipedia.org/wiki/CAP_theorem). Da qualche parte bisogna rilassare i constraint, e una di queste possibilita' e' quella di sposare il concetto di "eventual consistency".  
Ma dove/quando questo ha senso? Dove possiamo permetterci di non avere dati consistenti? E poi cosa significa avere dei dati "potenzialmente non consistenti"?  
In tutto questo marasma il concetto di *aggregate* tanto caro a chi, come me, bazzica il mondo DDD e', per me, stato fondamentale: un aggregate, essendo l'unita' minima di consistenza del mio sistema, non puo' mai essere in uno stato inconsistente: o c'e' o non c'e'. In un dato istante temporale, un ordine con tutti i suoi item o c'e' o non c'e', ma di sicuro non ho l'ordine con alcuni degli item si e altri no, giusto per fare un esempio.

Quando siamo in un ambito distribuito, ne siamo consapevoli e, soprattutto, ne abbiamo veramente bisogno il design e l'infrastruttura del sistema devono essere consci di questo *piccolo particolare* e poter, eventualmente, compensare e gestire situazioni di consistenza eventuale.  
Non sto dicendo che e' una cosa semplice, ma in certi scenari non si puo' proprio fare diversamente. Il sistema (potenzialmente) e' piu' complesso, ma non per questo deve risultare non affidabile.  

Ovviamente il design e il partizionamento devono essere fatti "*cum grano salis*": sono le logiche di business, una giusta definizione dei confini di ogni bounded contex, per tornare in tema di DDD, e la nostra esperienza che ci guidano in questa attivita' tanto critica quanto fondamentale. La scelta corretta ci permette veramente di rilassare (un po') i vincoli piu' stringenti e garantire **comunque** la correttezza del sistema.

Ultima domanda/considerazione che cerco sempre di tenere in considerazione quando ho a che fare con questi scenari: ma per quanto tempo sono eventualmente consistente?. Tendenzialmente la risposta e' sempre un..."tanto casino per nulla"



