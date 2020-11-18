---
layout: post
current: post
cover: 'assets/images/post-cover/20201118-cloudconf.jpg'
socialcover: 'assets/images/post-cover/20201118-cloudconf-s.jpg'
slug: 'cloud-conf'
title: 'CloudConf: Docker, Kubernetes, monitoring e una spolverata di Machine Learning'
date: 2020-11-18 09:00:00
category : tech
tags: [cloud, kubernetes, howto]
author: Vale
---
<div class="post-intro">
    <p>In questi mesi l’organizzazione di conferenze e corsi è divenuta più complicata per l’impossibilità di fornire una modalità “in presenza”; è però fondamentale, per la crescita di ogni sviluppatore, poter dedicare in **modo continuativo del tempo alla formazione personale**. In questo articolo <a href="">Valeria</a> ci racconta la sua esperienza alla Cloud Conf, i temi più interessanti e quali soluzioni si intrecciano e integrano nei nostri progetti.</p>
</div>

Ancora in tempi non sospetti ho deciso di partecipare alla **[Cloud Conf](https://2020.cloudconf.it/)**, una delle conferenze sul mondo del Cloud più importanti in Europa, prevista inizialmente per la primavera. Con i vari imprevisti di questo periodo, la conferenza si è tenuta in modalità online la settimana scorsa, corredata da due giorni di workshop.
<br/><br/>
<p style="text-align:center"><img src="/assets/images/post-content/cloud/cloudconf_docker.png" alt="feedback"/></p>
<br/><br/>
Ho sempre avuto una predilezione per il **backend**, mi piace seguire cosa avviene dietro le quinte, come sono organizzati i sistemi e come comunicano tra di loro. Ho scelto questa conf per poter approfondire alcune scelte architetturali che abbiamo o stiamo intraprendendo.

In CodicePlastico la strada che stiamo seguendo è quella di **deployare i nostri applicativi come immagini Docker**; i vantaggi che otteniamo sono molteplici: miglior utilizzo delle risorse, migliore scalabilità, migliore sicurezza, possibilità di fare rollback con facilità..

Il _principio di singola responsabilità_ ci porta ad avere un “parco applicativi” molto numeroso; le attività di monitor e deploy, che in caso di pochi applicativi possono essere talvolta manuali, necessitano inevitabilmente di un’**automazione **e di una **robustezza **maggiore. 

Nell’ambito della Cloud Conf ho avuto modo di seguire due workshop molto interessanti a riguardo: _Da Docker a Kubernetes in semplicità_ (di [Manuel Valentino](https://www.linkedin.com/in/brisma/?originalSubdomain=it) ) e _App Monitoring_ (di [Andrea Di Blasi](https://it.linkedin.com/in/andrea-di-blasi-888850b2)).

<cite>Kubernetes permette di gestire con facilità interi cluster di host su cui girano container, semplificando i processi di deployment e scalabilità di applicazioni.</cite>

Ecco la piattaforma adatta per i nostri requisiti!


### Kubernetes: un’orchestra al tuo servizio
Gli attori principali di Kubernetes sono i **nodi** e il **manager**. Quando chiediamo al manager di eseguire un determinato processo, il manager sceglie e istruisce uno dei nodi che ha a disposizione per mandare in esecuzione tale processo. I nodi, detti anche **workers**, sono quindi la “forza lavoro”; il carico di lavoro è composto da container che vengono eseguiti in un runtime (nel nostro caso Docker). 
<br/><br/>
<p style="text-align:center"><img src="/assets/images/post-content/cloud/cloudconf_director.png" alt="feedback"/></p>
<br/><br/>

Il componente base di Kubernetes (così come un container lo è per Docker) è un **POD**, un insieme di uno o più container e delle relative configurazioni; esso può essere eseguito all’interno di un nodo.
<br/><br/>
<p style="text-align:center"><img src="/assets/images/post-content/cloud/cloudconf_workers.png" alt="feedback"/></p>
<br/><br/>

Pensando ad un’**orchestra musicale**, il manager è il direttore, colui che sa come dirigere i musicisti (i nostri nodi), ognuno con le sue peculiarità; inoltre è responsabile della buona riuscita del concerto e deve essere in grado di **prendere il controllo** qualora alcune sezioni dell’orchestra vadano fuori tempo; nel nostro caso il manager ha il compito ad esempio di riavviare i container che si bloccano, terminare container che non rispondono, gestire il traffico in ingresso ai container, etc...
<br/><br/>
<p style="text-align:center"><img src="/assets/images/post-content/cloud/cloudconf_clusters.png" alt="feedback"/></p>
<br/><br/>
La definizione di un **cluster Kubernetes** è l’insieme delle definizioni delle risorse che lo compongono: POD, Nodi, Controllers, Job.

**Il modo più semplice per definire le risorse è tramite un template YAML.**

Uno strumento utile per prendere confidenza con Kubernetes e poter sperimentare tutte le sue funzionalità è _Minikube,_ grazie al quale è possibile eseguire Kubernetes localmente come nodo singolo all’interno di una macchina virtuale. 

Poi non resta che divertirsi e iniziare a giocare con i mattoncini messi a disposizione da Kubernetes: definire ed avviare un pod, modificare un pod in esecuzione, eliminare un pod; oppure definire un _replica set_ (un tipo di risorsa il cui scopo è garantire che il numero di pod richiesto sia sempre in esecuzione) o un _deployment_ (un tipo di risorsa grazie al quale possiamo configurare più nel dettaglio le strategie di deploy di un replicaset) o un _job_ (una risorsa che esegue uno o più pod “una tantum” per poi fermarsi; si pensi ad esempio alle migrations da eseguire una volta in fase di deploy e non ad ogni eventuale riavvio del pod)... 

E questa è solo una piccola parte di ciò che si può definire e realizzare con Kubernetes: per tornare alla metafora di prima, **le melodie da comporre sono infinite**.

### I tre pilastri dell’osservabilità
Come abbiamo visto, il numero di applicativi che gestiamo in CodicePlastico è **molto numeroso**; essi sono dislocati su diversi server, sviluppati nel corso degli anni con tecnologie sempre più aggiornate. Il team di CodicePlastico inoltre è costantemente in crescita ed è quindi necessario **facilitare l’ingresso dei nuovi sviluppatori** all’interno di uno scenario architetturale talvolta complesso.  
<br/><br/>
<p style="text-align:center"><img src="/assets/images/post-content/cloud/cloudconf_pillars.png" alt="feedback"/></p>
<br/><br/>
Per poter garantire un **intervento rapido** in caso di qualche malfunzionamento o, meglio, per poter giocare sul tempo **prevenendo situazioni critiche**, ma anche per poter monitorare le **performance degli applicativi**, è necessario che essi siano **osservabili**. È quindi necessario che si possa determinare cosa sta avvenendo, quale è lo _stato di salute_ del nostro applicativo, deducendolo dagli output che esso produce.

E quali possono essere questi output? I **tre pilastri dell’osservabilità** sono i **logs**, le **metriche** e il **tracing**:



*   i **logs** ci consentono di visualizzare i cambi di stato dei dati e gli eventi generati all'interno del nostro applicativo ad un determinato istante temporale; tipicamente i nostri log sono di tipo testuale, espliciti e contraddistinti da un livello di gravità
*   le **metriche** ci consentono di stabilire lo stato dei nostri servizi
*   grazie al **tracing** possiamo tracciare l’insieme delle azioni svolte da un utente sul nostro applicativo

Esistono tre importanti **stack di applicativi** che ci permettono di persistere, elaborare ed interrogare tutti questi output dei nostri applicativi: ELK, TIK e gli strumenti messi a disposizione da AWS. 

Lo stack che ho approfondito durante il workshop è stato quello **ELK**, acronimo dei tre strumenti principali che lo compongono: Elasticsearch, Logstash e Kibana.


Alla base troviamo _The beats family_, un insieme di collettori di dati; citando la definizione ufficiale **"Beats are open source data shippers that you install as agents on your servers to send operational data to Elasticsearch”** ; è possibile integrarne uno o più a piacimento. Alcuni beats utilizzati frequentemente sono _Filebeat_, dedicato ai file di log_, oppure _packetbeat_, che legge i dati relativi al traffico di rete.

Tutti i dati raccolti da questi collettori vengono persistiti all’interno di un “database” denominato **Elasticsearch**; talvolta è però necessario che i dati subiscano un’ulteriore elaborazione prima di essere persistiti. Ecco il ruolo di **_Logstash._**

<br/><br/>
<p style="text-align:center"><img src="/assets/images/post-content/cloud/cloudconf_elasticsearch.png" alt="feedback"/></p>
<br/><br/>
Grazie ad Elasticsearch, i dati persistiti possono essere velocemente **analizzati**, **indicizzati** e **ricercati**. Il risultato di queste analisi e monitoraggi deve essere fruito e compreso in modo rapido da parte dell’utente che ne ha bisogno (se avviene un problema in un applicativo, devo poter intervenire tempestivamente e quindi è necessario che io possa catturare a colpo d’occhio il valore dei dati). 

Ci viene in soccorso a questo scopo _Kibana_, un’interfaccia web estensibile e configurabile per la visualizzazione e interazione con i dati di Elasticsearch; un grande punto di forza di questo strumento è rappresentato dalla possibilità di configurare degli _Alert_.

<cite>Alerting allows you to detect complex conditions within different Kibana apps and trigger actions when those conditions are met.</cite>

### Prevenire è meglio che curare!
Per poter prevenire situazioni critiche nei nostri sistemi, possiamo impostare dei **trigger** su determinate condizioni del sistema per essere notificati (via email, via slack...) quando essi vengono scatenati. **Che vantaggi ne avremo?** Potremo **ridurre i controlli manuali** che talvolta eseguiamo (_controlli sulla memoria occupata da determinati servizi, controllo di eventuali messaggi in errore sulle code di RabbitMq, controllo dello “stato di salute” di alcuni servizi, etc...)_ avendo la garanzia di essere **avvisati per tempo** (e poter intervenire tempestivamente) o al massimo quando si verifica l’evento, senza dover subire passivamente gli accadimenti a posteriori.

## Machine learning
Un altro tema “caldo” che spesso emerge in CodicePlastico è il** Machine Learning**: c’è interesse e vorremmo poterlo introdurre all’interno di nuovi progetti. Il titolo del talk di [Laurent Picard](https://twitter.com/picardparis) che ho seguito alla Cloud Conf, _“Building smarter solutions with no expertise in machine learning_” mi ha attirato perchè sembrava fatto apposta per me: voler approcciare questi meccanismi ma non avere una robusta conoscenza alle spalle.

<p style="text-align:center"><img src="/assets/images/post-content/cloud/cloudconf_machinelearning.png" alt="feedback"/></p>

Sono stati differenziati tre livelli di approccio al **Machine Learning**, in base alle proprie skills.


### Poco tempo? I modelli pre-addestrati fanno per te.
Se si hanno a disposizione poche ore e poche competenze si possono sfruttare **modelli pre-addestrati** interrogabili tramite API REST (ML API); ce ne sono di tutti i tipi, dall’analisi di immagini, testi, video, alla sintesi vocale e alla classificazione di testo non strutturato... Ecco la soluzione ideale per chi non ha troppo tempo da dedicare a questa tecnologia ma vuole già provare ad interagire con modelli di machine learning.


### Qualcosa di più? Puoi iniziare a giocare con i modelli esistenti
Aumentando skills e tempo di apprendimento, si può passare ad un livello superiore, utilizzando **Auto ML**, uno strumento messo a disposizione da Google Cloud, in cui si sfruttano **modelli esistenti**, che però vengono addestrati con i dati di proprio interesse, per ottenere dei modelli customizzati. Come output avremo si avranno sempre delle API predittive REST da poter interrogare


### Vuoi giocare di complessità? Ecco i modelli personalizzati.

Non c’è limite alla complessità che si può raggiungere per implementare modelli personalizzati, tramite ad esempio le reti neurali; ma è necessario studiare e consolidare le skills in materia di Machine Learning.

Scoprire che ci sono vari livelli incrementali per poter apprendere e sperimentare il Machine Learning non può che essere la spinta per iniziare a “sporcarsi le mani”: non resta che provare!
