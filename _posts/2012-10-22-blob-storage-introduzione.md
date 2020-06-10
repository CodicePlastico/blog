---
layout: post
title:  "Blob storage: introduzione"
comments: true
categories: tech
tags: [azure, microsoft, tech, api]
author: [ale]
---


Questo post è parte di una serie su windows azure. Ecco il [link](http://blog.codiceplastico.com/melkio/index.php/2012/10/15/azure-intro/) per lista di tutti i post precedenti.

Nel [post](http://blog.codiceplastico.com/melkio/index.php/2012/10/18/was-windows-azure-storage/) precedente abbiamo introdotto il windows azure storage, nelle sue varie sfaccettature. Ora proviamo, anche con un po&#8217; di codice, a capire come interagire con il blob storage, messo a disposizione da azure.

Prendiamo spunto da una slide del [training kit](http://www.microsoft.com/en-us/download/details.aspx?id=8396) di windows azure rilasciato ad ottobre, per capire come sono &#8220;organizzati&#8221; il blob storage e i suoi contenuti:

![](http://melkio.codiceplastico.com/images/uploads/2012/10/Blob-storage-concepts-300x152.png)

Ogni file (blob) è contenuto in un container, che è assimilabile, quindi, alla root nella quale verranno poi caricate effettivamente le risorse. La piattaforma (windows azure) utilizza il blobname come &#8220;partition key&#8221; per bilanciare i carichi automaticamente e in modo del tutto trasparente, in base alle richieste, e distribuire le risorse tra i vari server.

La creazione di un container può avvenire o tramite il management portal oppure tramite codice. Il setup &#8220;up-front&#8221; fatto tramite il management portal è molto utile in tutti quegli scenari in cui l&#8217;environment deve essere già configurato correttamente prima dell&#8217;uso.

![](http://melkio.codiceplastico.com/images/uploads/2012/10/AddContainer-300x280.png)

In fase di setup è possibile, oltre al nome del container, definire anche il suo livello di visibilità, impostando uno dei valori consentiti: Private, Public blob o Public container

![](http://melkio.codiceplastico.com/images/uploads/2012/10/ContainerSettings-300x211.png)

Per interagire, lato codice, con il windows azure storage abbiamo sostanzialmente due opzioni, sia per il blob, sia per table e queue:

- 
la client library, disponibile per differenti piattaforme. Per i progetti .Net, utilizzando nuget basta installare il package corretto (WindowsAzure.Storage)

- 
REST api



Lo snippet di codice più semplice, utilizzando la client library per .Net, per poter uploadare un file sul container appena creato è il seguente (e direi che il codice è abbastanza autoesplicativo):

```

```

Con poche semplici righe di codice, abbiamo caricato il nostro primo file sul blob storage di azure. Nei prossimi post cercheremo di andare un po&#8217; più nel dettaglio&#8230;

