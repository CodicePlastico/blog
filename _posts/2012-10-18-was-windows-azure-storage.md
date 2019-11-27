---
layout: post
title:  "WAS: windows azure storage"
comments: true
categories: Technology
---


Questo post è parte di una serie su windows azure. Ecco il [link](http://blog.codiceplastico.com/melkio/index.php/2012/10/15/azure-intro/) per la lista di tutti i post precedenti.

Uno delle feature di Azure più interessanti, facilmente utilizzabili ed integrabili anche nelle applicazioni già in essere è il Windows Azure Storage.

Azure mette a disposizione quattro diverse tipologie di storage:

- 
Blob: storage di dati binari (file e simili).

- 
Table: storage di dati non relazionali.

- 
Queue: storage di messaggi (reliability)

- 
Windows Azure disk: fondamentalmente è un volume NTFS, utilizzabile a runtime da un &#8220;role&#8221;. Lo storage effettivo dei dati scritti su un Windows Azure  è memorizzato in un page blob, garantendo, in questo modo, che i dati rimangano disponibili anche a seguito di un recycle dell&#8217;istanza di un &#8220;role&#8221;.



La creazione e il setup di uno storage avviene tramite il [&#8220;management portal&#8221; di Azure](https://manage.windowsazure.com) ed è, alla stregua di tutte le altre operazioni fatte tramite il portale, molto semplice: basta impostare il nome dell&#8217;account, la region in cui si sceglie di &#8220;deployare&#8221; lo storage e abilitare/disabilitare la, cosìdetta, &#8220;geo-replication&#8221;.

![](http://melkio.codiceplastico.com/images/uploads/2012/10/CreateNewStorageAccount-300x147.png)

Alcune semplici note nella scelta dei parametri:

- 
la scelta della &#8220;region&#8221; va fatta in base alla &#8220;prossimità&#8221; con le applicazioni che sfrutteranno lo storage. Qualora tali applicazioni fossero dei role (cloud service) sarebbe opportuno/auspicabile/preferibile che siano nello stesso data center per massimizzare le prestazioni (e contenere i costi)

- 
abilitare la geo-replication è una scelta a costo zero. Abilitarla in un secondo momento, invece, potrebbe impattare economicamente in base alla quantità di dati da replicare nel secondo data center.



La possibilità di abilitare la replica dei dati anche su data center geograficamente distribuiti aumenta a dismisura l&#8217;affidabilità del servizio: qualora ci siano &#8220;major disaster&#8221; nel data center principale, i dati, già replicati nel data center secondario, verranno messi a disposizione della piattaforma aggiornando il DNS e facendolo puntare allo storage sul secondo datacenter,  in modo del tutto trasparente alle applicazioni che ne fanno già uso.

Quando la creazione dello storage viene completata (pochi minuti) abbiamo a disposizione blob, table e queue. Gli endpoint attraverso cui possiamo interagire con i tre tipi di storage hanno tutti la stessa semantica:


http://..core.windows.net//


dove:

- 
ACCOUNTNAME è il nome scelto in fase di creazione dello storage

- 
SERVICETYPE può essere blob, table o queue a seconda della tipologia di &#8220;servizio&#8221; con il quale si vuole interagire

- 
CONTAINER e RESOURCE li vedremo strada facendo :-)



Ora basta con tutta questa teoria (anche se un po&#8217; di basi credo proprio che non guastino), dal prossimi post entreremo nel dettaglio pratico del come e quando utilizzare il &#8220;cloud storage&#8221; di Azure.

