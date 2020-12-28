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
- eventi "atomici", ovvero eventi che hanno la loro ragione di essere completamente scorrelata dalla produzione di altri eventi dello stesso tipo. Per esempio "un ordine e' stato registrato" in un sistema di e-commerce potrebbe essere un evento di questo tipo
- eventi correlati (nel tempo), ovvero eventi il cui contenuto informativo non e' rappresentato solo dall'i-esimo evento in particolare, ma dallo stream di tutti gli eventi. Basti pensare ad un sensore di temperatura: e' molto piu' importante sapere se la "tendenza" della temperatura e' in discesa, per attivare la caldaia, piuttosto che sapere che, in un particolare istante temporale, la temperatura rilevata era di 20.3 gradi.

In queste tre macro categorie si configurano i tre servizi principali che Azure mette a disposizione per la gestione dei messaggi:
- [Azure Event Hubs](https://azure.microsoft.com/en-us/services/event-hubs/?WT.mc_id=AZ-MVP-5001625) per la gestione degli stream di eventi
- [Azure Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/?WT.mc_id=AZ-MVP-5001625) per la gestione degli eventi di qualsiasi forma e origine
- [Azure Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview?WT.mc_id=AZ-MVP-5001625) per la gestione di scenari di business "critici" in cui si rivelano indispensabili le feature di un enterprise message broker completo

Ora che abbiamo capito in che scenario ha senso pensare ad Event Grid, cerchiamo di capire un po' meglio cos'e': prima del suo avvento, tutti i servizi di Azure erano una serie di "puntini", che potevano cooperare tra di loro, ma con modalita' completamente differenti. Event Grid e' la lingua franca con cui mettere in comunicazione tutti questi servizi. La linea che permette di unire tutti i puntini.

I [componenti principali](https://docs.microsoft.com/en-us/azure/event-grid/concepts?WT.mc_id=AZ-MVP-5001625) che caratterizzano Event Grid sono:
- un _evento_, ovvero l'informazione che si vuol far "viaggiare" nel sistema
- una _sorgente_, ovvero il componente che genera l'evento
- un _topic_, ovvero l'endpoint a cui viene "inviato" l'evento
- un _handler_, ovvero un componente interessato ad un particolare evento
- una _subscription_, ovvero il collegamento tra un topic ed un componente interessato all'evento generato

La prima cosa da fare per poter sfruttare Azure Event Grid e' abilitare il relativo provider sulla piattaforma.  
Utilizzando per esempio la CLI, tramite il semplice comando `az provider register --namespace Microsoft.EventGrid` e' possibile attivare il servizio all'interno della propria subscription.  

Un semplice use-case, potrebbe essere quello per cui, ogni volta che viene caricato un nuovo blob su uno [Storage Account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview?WT.mc_id=AZ-MVP-5001625) deve essere invocata una nostra API che si occupera' di gestire la notifica.  
Per poter abilitare questa integrazione e' necessario creare una _subscription_ tra un _topic_ (di sistema), quello appunto in cui la piattaforma veicola gli eventi relativi allo Storage Account, e la nostra API. Per poterlo fare basta eseguire il seguente comando:

```
az eventgrid event-subscription create \
  --source-resource-id $STORAGE_ACCOUNT_ID \
  --name $SUBSCRIPTION_NAME \
  --endpoint $ENDPOINT_URI
```

Dove:
- `$STORAGE_ACCOUNT_ID` e' appunto l'ID dello storage account che genera l'evento
- `$SUBSCRIPTION_NAME` e' il nome della subscription che stiamo creando
- `$ENDPOINT_URI` e' l'endpoint verso cui verranno veicolati gli eventi

Nell'ecosistema di Azure, una _source_ puo' essere uno dei servizi esposti dalla piattaforma, come per esempio [Azure Container Registry](https://docs.microsoft.com/en-us/azure/event-grid/event-schema-container-registry?WT.mc_id=AZ-MVP-5001625) o [Azure App Configuration](https://docs.microsoft.com/en-us/azure/event-grid/event-schema-app-configuration?WT.mc_id=AZ-MVP-5001625). Un _handler_ puo' essere per esempio una [Azure Function](https://docs.microsoft.com/en-us/azure/event-grid/handler-functions?WT.mc_id=AZ-MVP-5001625) o un [WebHook](https://docs.microsoft.com/en-us/azure/event-grid/handler-webhooks?WT.mc_id=AZ-MVP-5001625).  

Gli eventi che vengono prodotti, da qualunque servizio, hanno una struttura ben precisa, a cui tutte le sorgenti si devono adeguare:

```
{
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
}
```

dove il solo attributo `data` e' tipico del contesto in cui l'evento viene generato. In fin dei conti, il contenuto informativo di un messaggio generato, per esempio, quando una nuova immagine viene caricata su Azure Container Registry e' sensibilmente differente da quello prodotto da un cambio di configurazione su Azure App Configuration.

Visto che spesso un'immagine vale piu' di mille parole, ecco tutti i servizi che e' possibile "collegare" sfruttando Event Grid:

![functional model](https://docs.microsoft.com/en-us/azure/event-grid/media/overview/functional-model.png?WT.mc_id=AZ-MVP-5001625)

Il fatto che uno dei possibili handler supportati da Event Grid sia il meccanismo dei [webhook](https://docs.microsoft.com/en-us/azure/event-grid/handler-webhooks?WT.mc_id=AZ-MVP-5001625) apre alla possibilita' di integrazione non solo da/verso servizi di Azure, ma anche da servizi della piattaforma a sistemi terzi, come per esempio nostre applicazioni.  

Ma non solo...Azure Event Grid supporta la "produzione" di eventi custom, quindi non generati da servizi della piattaforma, ma da servizi/applicazioni esterne alla piattaforma stessa, come potrebbe essere una nostra applicazione.  
Per potrer integrare sorgenti esterne alla piattaforma e' necessario creare un [topic custom](https://docs.microsoft.com/en-us/azure/event-grid/custom-topics?WT.mc_id=AZ-MVP-5001625), a differenza di quanto avviene con i servizi di Azure, che utilizzano topic "pre-configurati", chiamati appunto [topic di sistema](https://docs.microsoft.com/en-us/azure/event-grid/system-topics?WT.mc_id=AZ-MVP-5001625).  
Anche in questo caso, pero', il tutto e' a distanza di un comando sulla CLI:

```
az eventgrid topic create \
  --name $TOPIC_NAME 
  -l $REGION 
  -g $RESOURCE_GROUP
```

Il topic appena creato e' esposto tramite un uri, che e' "recuperabile" tramite il seguente comando:

```
endpoint=$(az eventgrid topic show --name $topicname -g gridResourceGroup --query "endpoint" --output tsv)
```

A questo punto, l'integrazione e' presto fatta: basta un qualsiasi linguaggio e/o tool che supporti la possibilita' di eseguire una `POST` HTTP verso questo endpoint, a cui possiamo inviare i nostri eventi, che verranno veicolati, attraverso le _subscription_ registrate, verso i vari handler.

Con Azure Event Grid termina l'era del dispendioso "polling" per valutare eventuali cambi di stato del nostro sistema. Ora, qualsiasi scenario _message driven_ e' veramente a distanza di...un messaggio.


