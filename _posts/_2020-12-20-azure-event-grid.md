---
layout: post
current: post
cover: 'assets/images/post-cover/20201118-cloudconf.jpg'
socialcover: 'assets/images/post-cover/20201118-cloudconf-s.jpg'
slug: 'cloud-conf'
title: 'Azure Event Grid: a message to rule them all'
date: 2020-11-18 09:00:00
category : tech
tags: [azure, event grid, distributed systems, reactive systems]
author: Ale
---
Immaginiamo di vivere in un mondo parallelo in cui, a fronte di ogni azione, venga generato un evento. E immaginiamo che esista una piattaforma/servizio che nativamente metta a disposizione queste caratteristiche. Sarebbe fantastico, no? Beh, forse una soluzione gia' c'e'...Azure Event Grid

[Azure Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/?WT.mc_id=AZ-MVP-5001625) non e' l'unico servizio disponibile su Azure che ha a che fare con "la messaggistica". Prima di capire come usare Azure Event Grid e' quindi forse meglio capire quando Event Grid fa al caso nostro.

Nel "mondo" dei messaggi, possiamo identificare due macro-categorie che ne definiscono l'ambito: i comandi e gli eventi.

I comandi, come da [definizione](https://www.treccani.it/vocabolario/comando1/) sono assertivi: qualcuno chiede a qualcun altro di fare qualcosa.  
La comunicazione e' "point-to-point", tra due attori del sistema e, solitamente, il produttore del comando e' interessato al risultato dell'operazione richiesta.

Gli eventi, invece, vengono utilizzati per notificare che qualcosa e' avvenuto nel sistema, che il sistema ha subito un cambio di stato. La comunicazione non e' piu' tra due attori specifici, ma tra un componente e N eventuali altri componenti interessati a quel tipo di informazione. E' una comunicazion di tipo broadcast, in cui il numero di componenti N puo' essere _uno, nessuno o centomila_.

Focalizzando l'attenzione sulla categoria degli eventi, possiamo affinare la distinzione individuando due peculiarita' che possono caratterizzare e distinguere gli eventi:
- eventi "atomici": ovvero eventi che hanno la loro ragione di essere completamente scorrelata dalla produzione di altri eventi dello stesso tipo. Per esempio "un ordine e' stato registrato" in un sistema di e-commerce potrebbe essere un evento di questo tipo
- eventi correlati (nel tempo): il contenuto informativo di tali eventi non e' rappresentato dall'i-esimo evento in particolare, ma dallo stream di tutti gli eventi. Basti pensare ad un sensore di temperatura: e' molto piu' importante sapere se la "tendenza" della temperatura e' in discesa, per attivare la caldaia, piuttosto che sapere che in un particolare istante temporale, la temperatura rilevata era di 20.3 gradi

In queste tre macro categorie si configurano i tre servizi principali che Azure mette a disposizione per la gestione dei messaggi:
- [Azure Event Hubs](https://azure.microsoft.com/en-us/services/event-hubs/?WT.mc_id=AZ-MVP-5001625) per la gestione degli stream di eventi
- [Azure Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/?WT.mc_id=AZ-MVP-5001625) per la gestione degli eventi di qualsiasi forma e origine
- [Azure Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview?WT.mc_id=AZ-MVP-5001625) per la gestione di scenari di business "critici" in cui si rivelano indispensabili le feature di un enterprise message broker completo

Ora che abbiamo capito in che scenario ha senso pensare ad Event Grid, cerchiamo di capire un po' meglio cos'e': prima del suo avvento, tutti i servizi di Azure erano una serie di puntini, che potevano cooperare tra di loro, ma con modalita' completamente differenti tra di loro. Event Grid e' la lingua franca con cui mettere in comunicazione tutti questi servizi. La linea che permette di unire tutti i puntini.

I [componenti principali](https://docs.microsoft.com/en-us/azure/event-grid/concepts?WT.mc_id=AZ-MVP-5001625) che caratterizzano Event Grid sono:
- un evento, ovvero l'informazione che si vuol far "viaggiare" nel sistema
- una sorgente (source), ovvero il componente che genera l'evento
- un topic, ovvero l'endpoint a cui viene "inviato" l'evento
- un handler, ovvero un componente interessato ad un particolare evento
- una subscription, ovvero il collegamento tra un topic ed un componente interessato all'evento generato

Nell'ecosistema di Azure, una _source_ puo' essere uno dei servizi esposti dalla piattaforma, come per esempio [Azure Container Registry](https://docs.microsoft.com/en-us/azure/event-grid/event-schema-container-registry?WT.mc_id=AZ-MVP-5001625) o [Azure App Configuration](https://docs.microsoft.com/en-us/azure/event-grid/event-schema-app-configuration?WT.mc_id=AZ-MVP-5001625). Un handler puo' essere per esempio una [Azure Function](https://docs.microsoft.com/en-us/azure/event-grid/handler-functions?WT.mc_id=AZ-MVP-5001625) o un [WebHook](https://docs.microsoft.com/en-us/azure/event-grid/handler-webhooks?WT.mc_id=AZ-MVP-5001625).  

Visto che solitamente un'immagine vale piu' di mille parole, ecco tutti i servizi che e' possibile "collegare" sfruttando Event Grid:

![functional model](https://docs.microsoft.com/en-us/azure/event-grid/media/overview/functional-model.png?WT.mc_id=AZ-MVP-5001625)

Il fatto che uno dei possibili handler supportati da Event Grid sia il meccanismo dei [webhook](https://docs.microsoft.com/en-us/azure/event-grid/handler-webhooks?WT.mc_id=AZ-MVP-5001625) apre le porte all'integrazione di sistemi terzi (come per esempio nostre applicazioni) con le sorgenti supportate da Event Grid.

Event Grid mette a disposizione la possibilita' di integrarsi anche dall'altro lato della barricata, ovvero dalla parte di chi produce gli eventi.  
Tutte le sorgenti _native_ della piattaforma sfruttano dei [topic di sistema](https://docs.microsoft.com/en-us/azure/event-grid/system-topics?WT.mc_id=AZ-MVP-5001625), _system topic_ appunto, ma e' possibile definire dei [topic custom](https://docs.microsoft.com/en-us/azure/event-grid/custom-topics?WT.mc_id=AZ-MVP-5001625), in cui verranno veicolati eventi prodotti da sorgenti esterne ad Azure (come per esempio nostre applicazioni).

Con Azure Event Grid termina l'era del dispendioso "polling" per valutare eventuali cambi di stato del nostro sistema. Ora, qualsiasi scenario _message driven_ e' veramente a distanza di...un messaggio.


