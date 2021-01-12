---
layout: post
current: post
cover: 'assets/images/post-cover/202101-azure-event-grid.jpg'
socialcover: 'assets/images/post-cover/202101-azure-event-grid.jpg'
slug: 'azure-event-grid'
title: 'Azure Event Grid: a message to rule them all'
date: 2021-01-11 09:00:00
category : tech
tags: [azure, event grid, distributed systems, reactive systems]
author: ale
---
Immaginiamo di vivere in un mondo parallelo in cui, a fronte di ogni azione, venga generato un evento. 

E immaginiamo che esista una piattaforma/servizio che nativamente metta a disposizione queste caratteristiche. 

Sarebbe fantastico, no?<br/>
Beh, forse una soluzione già c'è... **Azure Event Grid**


<figure style="text-align:center"><img src="/assets/images/post-content/azure-event-grid/eventgrid_azure-icons.png" alt="azure" />

</figure>

[Azure Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/?WT.mc_id=AZ-MVP-5001625) non è l'unico servizio disponibile su Azure che ha a che fare con la messaggistica. Ma prima di capire come usare Azure Event Grid, è megli approfondire _quando_ Event Grid fa al caso nostro.

Nel mondo dei messaggi, possiamo identificare due macro-categorie che ne definiscono l'ambito: i _comandi_ e gli _eventi_.

I **comandi**, come da [definizione](https://www.treccani.it/vocabolario/comando1/) sono assertivi: *qualcuno chiede a qualcun altro di fare qualcosa*. La comunicazione è *"point-to-point"*, tra due attori del sistema e, solitamente, il produttore del comando è interessato al risultato dell'operazione richiesta.

Gli **eventi**, invece, vengono utilizzati per **notificare** che qualcosa è avvenuto nel sistema, che il sistema ha subito un cambio di stato. La comunicazione non è più tra due attori specifici, ma *tra un componente* e *N eventuali altri componenti* interessati a quel tipo di informazione. &Egrave; una comunicazion di tipo broadcast, in cui il numero di componenti N puo' essere _uno, nessuno o centomila_.
<figure style="text-align:center"><img src="/assets/images/post-content/azure-event-grid/eventgrid_bottle.png" alt="azure" />
</figure>

### Eventi: le caratteristiche
Focalizzando l'attenzione sulla categoria degli eventi, possiamo affinare la distinzione individuando due peculiarità che possono caratterizzare e distinguere gli eventi:
- **eventi "atomici"**, ovvero eventi che hanno la loro ragione di essere completamente scorrelata dalla produzione di altri eventi dello stesso tipo. Per esempio "un ordine è stato registrato" in un sistema di e-commerce potrebbe essere un evento di questo tipo;
- **eventi correlati (nel tempo)**, ovvero eventi il cui contenuto informativo non è rappresentato solo dall'i-esimo evento in particolare, ma dallo _stream di tutti gli eventi_. Basti pensare ad un **sensore di temperatura**: è molto più importante sapere se la "tendenza" della temperatura e' in discesa, per attivare la caldaia, piuttosto che sapere che, in un particolare istante temporale, la temperatura rilevata era di 20.3 gradi.
<figure style="text-align:center"><img src="/assets/images/post-content/azure-event-grid/eventgrid_letters.png" alt="azure" />

</figure>

### Azure: i servizi e i componenti principali
In queste tre macro categorie si configurano i tre servizi principali che Azure mette a disposizione per la gestione dei messaggi:
- [Azure Event Hubs](https://azure.microsoft.com/en-us/services/event-hubs/?WT.mc_id=AZ-MVP-5001625) per la gestione degli stream di eventi
- [Azure Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/?WT.mc_id=AZ-MVP-5001625) per la gestione degli eventi di qualsiasi forma e origine
- [Azure Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview?WT.mc_id=AZ-MVP-5001625) per la gestione di scenari di business "critici" in cui si rivelano indispensabili le feature di un enterprise message broker completo

Ora che abbiamo capito in che **scenario** ha senso pensare ad Event Grid, cerchiamo di approfondire che cos'è.

Prima del suo avvento, tutti i servizi di Azure erano una serie di _"puntini"_, che potevano cooperare tra di loro, ma con modalità completamente differenti. <br/>
_Event Grid è la lingua franca_ con cui mettere in comunicazione tutti questi servizi. _La linea che permette di unire tutti i puntini_.

<figure style="text-align:center"><img src="/assets/images/post-content/azure-event-grid/eventgrid_mail.png" alt="azure" />

</figure>
I [componenti principali](https://docs.microsoft.com/en-us/azure/event-grid/concepts?WT.mc_id=AZ-MVP-5001625) che caratterizzano Event Grid sono:
- un _evento_, ovvero l'informazione che si vuol far "viaggiare" nel sistema
- una _sorgente_, ovvero il componente che genera l'evento
- un _topic_, ovvero l'endpoint a cui viene "inviato" l'evento
- un _handler_, ovvero un componente interessato ad un particolare evento
- una _subscription_, ovvero il collegamento tra un topic ed un componente interessato all'evento generato
<figure style="text-align:center"><img src="/assets/images/post-content/azure-event-grid/eventgrid_postbox.png" alt="azure" />
</figure>

### Ok, Ok, ma come si usa?
La prima cosa da fare per poter sfruttare Azure Event Grid è _abilitare il relativo provider sulla piattaforma_.

Utilizzando per esempio la CLI, tramite il semplice comando `az provider register --namespace Microsoft.EventGrid` è possibile attivare il servizio all'interno della propria subscription.  

Un semplice use-case, potrebbe essere quello per cui, ogni volta che viene _caricato un nuovo blob_ su uno [Storage Account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview?WT.mc_id=AZ-MVP-5001625) deve essere invocata una nostra API che si occuperà di _gestire la notifica_.

Per poter abilitare questa integrazione è necessario creare una _subscription_ tra un _topic_ (di sistema), quello appunto in cui la piattaforma veicola gli eventi relativi allo Storage Account, e la nostra API. Per poterlo fare basta eseguire il seguente comando:

```
az eventgrid event-subscription create \
  --source-resource-id $STORAGE_ACCOUNT_ID \
  --name $SUBSCRIPTION_NAME \
  --endpoint $ENDPOINT_URI
```

Dove:
- `$STORAGE_ACCOUNT_ID` è appunto _l'ID dello storage account_ che genera l'evento;
- `$SUBSCRIPTION_NAME` è il _nome della subscription_ che stiamo creando;
- `$ENDPOINT_URI` è _l'endpoint_ verso cui verranno veicolati gli eventi;

<figure style="text-align:center"><img src="/assets/images/post-content/azure-event-grid/eventgrid_mailbox.png" alt="azure" />

</figure>

Nell'ecosistema di Azure, una _source_ può essere uno dei servizi esposti dalla piattaforma, come per esempio [Azure Container Registry](https://docs.microsoft.com/en-us/azure/event-grid/event-schema-container-registry?WT.mc_id=AZ-MVP-5001625) o [Azure App Configuration](https://docs.microsoft.com/en-us/azure/event-grid/event-schema-app-configuration?WT.mc_id=AZ-MVP-5001625). Un _handler_ può essere per esempio una [Azure Function](https://docs.microsoft.com/en-us/azure/event-grid/handler-functions?WT.mc_id=AZ-MVP-5001625) o un [WebHook](https://docs.microsoft.com/en-us/azure/event-grid/handler-webhooks?WT.mc_id=AZ-MVP-5001625).  

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

dove il solo attributo `data` è tipico del contesto in cui l'evento viene generato. In fin dei conti, il contenuto informativo di un messaggio generato, per esempio, quando una nuova immagine viene caricata su _Azure Container Registry_, è sensibilmente differente da quello prodotto da un cambio di configurazione su **Azure App Configuration**.

Visto che spesso un'immagine vale più di mille parole, ecco tutti i servizi che è possibile "collegare" sfruttando Event Grid:


![functional model](https://docs.microsoft.com/en-us/azure/event-grid/media/overview/functional-model.png?WT.mc_id=AZ-MVP-5001625)

### WebHook e applicazioni esterne
Il fatto che uno dei possibili handler supportati da Event Grid sia il meccanismo dei [webhook](https://docs.microsoft.com/en-us/azure/event-grid/handler-webhooks?WT.mc_id=AZ-MVP-5001625) apre alla possibilità di integrazione non solo da/verso servizi di Azure, ma anche _da servizi della piattaforma a sistemi terzi_, come per esempio una nostra applicazione.  

Ma non solo... Azure Event Grid supporta la "produzione" di _eventi custom_, quindi non generati da servizi della piattaforma, ma da servizi/applicazioni esterne alla piattaforma stessa (anche in questo caso, una nostra applicazione).  
<figure style="text-align:center"><img src="/assets/images/post-content/azure-event-grid/eventgrid_missing-part.png" alt="azure" />

</figure>
A differenza di quanto avviene con i servizi di Azure che utilizzano topic _"pre-configurati"_, chiamati appunto [topic di sistema](https://docs.microsoft.com/en-us/azure/event-grid/system-topics?WT.mc_id=AZ-MVP-5001625), per potrer integrare sorgenti esterne alla piattaforma sarà necessario creare un [topic custom](https://docs.microsoft.com/en-us/azure/event-grid/custom-topics?WT.mc_id=AZ-MVP-5001625).  
Il tutto è a **distanza di un comando sulla CLI**:

```
az eventgrid topic create \
  --name $TOPIC_NAME 
  -l $REGION 
  -g $RESOURCE_GROUP
```

Il topic appena creato è _esposto tramite un uri_, che e' "recuperabile" tramite il seguente comando:

```
endpoint=$(az eventgrid topic show --name $topicname -g gridResourceGroup --query "endpoint" --output tsv)
```

A questo punto, l'**integrazione** è presto fatta: basta un qualsiasi linguaggio e/o tool che supporti la possibilità di eseguire una `POST` HTTP verso questo endpoint, a cui possiamo inviare i nostri eventi, che verranno veicolati, attraverso le _subscription_ registrate, verso i vari handler.

_Con Azure Event Grid termina l'era del dispendioso "polling" per valutare eventuali cambi di stato del nostro sistema._ <br/>
Ora, qualsiasi scenario _message driven_ è, veramente, a distanza di un messaggio.


