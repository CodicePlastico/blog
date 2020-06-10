---
layout: post
title:  "Bentornato SQL Server"
comments: true
categories: Technology SQLServer

categories: tech
tags: [database, sql, doker, azure]
author: ale
---

E'da un po' di tempo che ho in mente di scrivere questo post. Come al solito, sono in ritardo :)  
Vediamo di rimediare...

Chi mi conosce sa che, fin da [tempi non sospetti](http://www.communitydays.it/events/communitydays-2012/arch01/), sono stato un "acceso" sostenitore di [mongodb](https://www.mongodb.com/).  
Al tempo mi aveva attirato per la sua semplicità di utilizzo, dall'installazione all'interazione stessa: un veloce download e l'esecuzione dell'engine è a distanza di un click.  

Prima che orde di DBA inferociti mi rincorrano alla prossima conferenza, ci tengo a sottolineare che comprendo perfettamente la necessità di un'installazione e configurazione accurata di qualsiasi strumento, database _in primis_. Ciò non nega il fatto che, in fase di sviluppo, un processo di setup molto lungo/complesso o un'interazione non ottimale introducono una frizione che impatta sulla produttività stessa del team.

Tornando a noi...negli ultimi anni MS, con SQL server, ha fatto un lavoro incredibile, migliorando di versione in versione l'engine, e introducendo una serie di feature che hanno fatto scalare a SQL Server la mia personale classifica, portandolo nuovamente nelle primissime posizioni.

### SQL Server e docker

Con l'introduzione e il supporto a docker, in un colpo solo, MS ha risolto tutte le mie frustrazioni per un'installazione silent di SQL, in modo automatico tramite per esempio vagrant. Chiunque ci abbia mai provato, sa di cosa sto parlando...potrebbe essere il topic per un nuovo post.  
Con la possibilità di eseguire l'engine di SQL all'interno di un container docker, la frizione per un nuovo dev che deve fare setup dell'infrastruttura prima di poter fare F5 è a distanza di uno script. 

### SQL Server e il supporto a json

Altra "frustrazione frustrante" per noi dev è la famosissima [impedence mismatch](https://en.wikipedia.org/wiki/Object-relational_impedance_mismatch) tra modello relazionale e modello object-oriented, che ci ha portato, nel corso degli anni a provarle un po' tutte, passando da StoredProcedure agli ORM più o meno light e a...chi più ne ha più ne metta.  
Il modello documentale, tipico di MongoDB e altri db no-sql, risolvono up-front questa difficoltà: niente tabelle e relazioni, ma documenti.  
Da un paio di versioni, però, anche SQL Server ha il [supporto nativo a json](https://docs.microsoft.com/en-us/sql/relational-databases/json/json-data-sql-server), con la possibilità di indicizzare attributi del documento a diverso livello di profondità garantendo al contempo un modello diverso da quello relazionale (quello documentale, appunto) e ottime performance.

### SQL Server e il modello a grafo

Un nuovo modello si è aggiunto a quelli supportati: [il modello a grafo](https://docs.microsoft.com/en-us/sql/relational-databases/graphs/sql-graph-overview). L'engine è lo stesso ma l'approccio è sostanzialmente differente: anche in questo caso vige la regola per cui diversi scenari hanno necessità differenti e soluzioni/tool diversi.  
Il fatto di avere un'unica tecnologia che li supporti tutti è un grande vantaggio, perchè permette di limitare le componenti infrastrutturali del sistema e quindi abbassare il costo della manutenzione e del monitoring.  
Nulla vieta ovviamente di valutare ulteriori tool qualora lo scenario da approcciare sia così specifico da non potere essere risolto con una tecnologia "cross". A questo punto il costo di un nuovo componente è assolutamente compensato dalla specificità dello scenario e dai benefici introdotti da un tool specifico.

### SQL Server e Azure

Ho lasciato volutamente per ultimo questo punto, ma ciò non significa che abbia meno importanza dei precedenti. Anzi...assume un ruolo cardine nelle mie scelte quando l'applicazione che sto progettando e su cui sto lavorando deve "funzionare nel cloud".  
In questo scenario avere SQL Server, in modalità PaaS è "tanta roba": significa potersi dimenticare di configurazione e monitoraggio perchè forniti direttamente dalla piattaforma. Significa poter sfruttare tutte le metriche raccolte dalla piattaforma stessa su tutti i servizi SQL Database attivati da tutti gli utenti, per ottimizzare o suggerire ottimizzazioni sulla nostra istanza. Significa avere un livello del servizio difficilmente ottenibile in uno scenario on-premise.

Tante altre sono le caratteristiche interessanti di SQL Server (infatti non abbiamo parlato di [In-Memory OLTP](https://docs.microsoft.com/en-us/sql/relational-databases/in-memory-oltp/overview-and-usage-scenarios) o [Columnstore indexes](https://docs.microsoft.com/en-us/sql/relational-databases/indexes/columnstore-indexes-overview))...diciamo che il pretesto del post era di riportare nella giusta prospettiva il mio modo di vedere e approcciare l'utilizzo di SQL Server.  
Che posso dire: "Welcome back SQL Server"...certi amori non finiscono, fanno dei giri immensi ma poi ritornano :D