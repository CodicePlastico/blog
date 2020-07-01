---
layout: post
current: post
cover: 'assets/images/post-cover/20200701-azure-functions.jpg'
socialcover: 'assets/images/post-cover//20200701-azure-functions-s.jpg'
slug: azure-functions
title: 'Azure Functions: ovvero il Cloud secondo me'
date: 2020-07-01 14:00:00
category : tech
tags: [azure, serverless, howto]
author: ale
---

Nell'ecosistema di Azure, negli ultimi anni, si sono affacciate le *Azure Functions*: la proposta di Azure per tutto cio' che sta sotto il cappello di "serverless computing".  
Non è un approccio applicabile ovunque, per ogni caso, ma si sposa perfettamente con l'idea di "cloud" che ho io.  

Il modello event-driven nativo, che sta alla base del servizio, è molto comodo in diversi scenari. Di per sè la metafora su cui si basa il funzionamento del servizio è molto semplice: quando *succede qualcosa*, dato un set di input applica l'elaborazione e genera un set di output.  

In gergo tecnico,
- "quando succede qualcosa" è chiamato *trigger*
- un "set di input" è il *binding in ingresso*
- "l'elaborazione" è la *function* vera e propria
- il "set di output" è il *binding in uscita*

L'idea di questo post non è tanto spiegare cosa sono le Functions, per questo rimando alla [documentazione ufficiale](https://azure.microsoft.com/en-us/services/functions/), che è sicuramente più esaustiva. L'idea è quella di capire, con un caso reale, *come risolvere scenari più complessi* di una chiamata HTTP e/o della inserimento di un nuovo documento in CosmosDB a fronte di un messaggio su una coda.  
<br/>
### Lo scenario: l'insidioso OCR
Ipotizziamo di dover estrarre il contenuto testuale di un documento pdf caricato su un container dello storage account, tramite un'opportuna elaborazione OCR. Lo scenario sembra semplice, ma nasconde delle (piccole) insidie. Vediamole.  
<br/>
### Step 1: il trigger
Il primo step del nostro workflow, prevede l'attivazione (trigger) della nostra funzione, quando un nuovo documento viene caricato sullo storage account. Su questo fronte, nessun problema: è proprio l'integrazione base di cui parlavamo prima.  

```javascript
// function.json
{
  "bindings": [
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "samples-workitems/{name}",
      "connection": ""
    }
  ]
}


// index.js
module.exports = async function (context, myBlob) {
    context.log("JavaScript blob trigger function processed blob \n Blob:", context.bindingData.blobTrigger, "\n Blob Size:", myBlob.length, "Bytes");
};
```
<br/>
All'interno di function.json è definita "l'infrastruttura" della funzione (trigger, input binding e ouptut binding), mentre in index.js c'è la funzione vera e propria.  
<br/>
### Invochiamo l'API dei Cognitive Service
Nel nostro caso, quando un nuovo file viene caricato, dobbiamo "semplicemente" invocare l'API dei *Cognitive Service* che si occupa dell'elaborazione OCR del file. Detto. Fatto.  

```javascript
// index.js
const axios = require('axios');

const client = axios.create({
    baseURL: process.env['AZURE_COGNITIVE_SERVICE_BASE_URI'],
    headers: {
        'Content-Typè: "application/json",
        'Ocp-Apim-Subscription-Key': process.env['AZURE_COGNITIVE_SERVICE_API_KEY']
    }
})

module.exports = async function (context) {
    context.log(`Executing async batch analyze activity on ${context.bindingData.uri} blob`);

    const data = {
        url: context.bindingData.uri
    };
    const response = await client.post("vision/v2.0/read/core/asyncBatchAnalyze", data)
    const location = response.headers['operation-location'];

    ???
};
```
<br/>
### Timer o non timer? Questo è il dilemma!
Eh, pero' qui abbiamo il primo problema: *l'elaborazione non è "sincrona"*.  
L'API non ci restituisce il risultato, ma una "location" in cui recuperare il risultato. Una volta pronto.

Non avendo alcun controllo su quando il risultato sarà pronto, l'unica opzione è *provarci, ad intervalli di tempo*.  

Azure Functions mette a disposizione un *trigger di tipo Timer*, ma in questo caso non è quello che fa al ca*so nostro: noi non abbiamo bisogno di un timer "continuo", ma abbiamo bisogno di *attivare un timer all'occorrenza*, permettendoci di interrogare la presenza del risultato, e, successivamente, spegnerlo. 
<br/><br/>
### Molte risorse per nulla
Altra opzione potrebbe essere quella di mettere un *messaggio su una coda*, in modo da attivare un'altra function che verifichi la presenza del risultato. Se il risultato è presente, viene salvato su CosmosDB o da qualsiasi altra parte. Se l'elaborazione OCR non fosse ancora terminata "basterebbe" ripubblicare lo stesso messaggio, riattivando di fatto la stessa funzione. Questa opzione, seppur funzionante, non è ottimale: *consumeremmo un "sacco" di risorse per nulla in un ciclo continuo*.   
<br/><br/>
### Extra bonus! Le Durable Functions
Quando il flusso è un po' più articolato e dobbiamo gestire dei workflow più articolati abbiamo bisogno di un extra bonus: le *Durable Functions*.  

Anche in questo caso, un [link](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview) vale più di mille parole.  

La nostra implementazione si complica un po', introducendo un *orchestratore*, ma il flusso risulta più partizionato e più efficace:  

```javascript
// index.js - orchestratore
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function* (context) {
    const { blobUri, containerName } = context.df.getInput();
    const location = yield context.df.callActivity("ExecuteAsyncBatchAnalyzeActivity", blobUri);

    const now = context.df.currentUtcDateTime;
    const expirationTime = moment.utc(now).add(30, 'm');

    while (moment.utc(now).isBefore(expirationTime)) {
        const { isCompleted, data } = yield context.df.callActivity("GetAsyncBatchStatusActivity", location)

        if (isCompleted) {
            yield context.df.callActivity("SaveAsyncBatchAnalyzeResultsActivity", { containerName, blobUri, data });
            break;
        } else {
            const nextCheckpoint = moment.utc(context.df.currentUtcDateTime).add(30, 's');
            yield context.df.createTimer(nextCheckpoint.toDate());
        }
    }
});
```

In questo modo abbiamo creato un *timer* che ogni 30 secondi, e per un massimo di 30 minuti, interroga l'API per *verificare la presenza del risultato*. Quando il risultato sarà disponibile, il workflow terminerà. In caso contrario il flusso proseguirà con l'iterazione successiva.  

**ExecuteAsyncBatchAnalyzeActivity**, **GetAsyncBatchStatusActivity** e **SaveAsyncBatchAnalyzeResultsActivity** sono chiamate activity.  
Sono di fatto le "funzioni" che compongono il workflow.

### Serveless + consumption plan = Cloud
Che dire...non per tutto, ma sicuramente per diversi scenari, un approccio "serverless" semplifica enormemente la vita. Unito al concetto di [consumption plan](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale#consumption-plan) è quello che per me dovrebbe essere il cloud.
