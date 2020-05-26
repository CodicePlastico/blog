---
layout: post
title:  "Blob storage e hosting di siti statici"
categories: tech
---


Questo post è parte di una serie su windows azure. Ecco il [link](http://blog.codiceplastico.com/melkio/index.php/2012/10/15/azure-intro/) per la lista di tutti i post precedenti.

Le funzionalità messe a disposizione da Windows Azure sono decisamente tante: Web Site, Cloud Services, Storage&#8230;Alcuni scenari in cui trarre vantaggio da queste funzionalità, però, non sono così evidenti come altri e bisogna utilizzare un po&#8217; di immaginazione per definirne alcuni un po&#8217; &#8220;insoliti&#8221;. Uno di questi, oggetto di questo post, è la possibilità di sfruttare il Windows Azure Blob Storage come sistema di hosting per siti dai contenuti statici.

Dato che un sito statico non ha bisogno di un data store e, per esempio, di un worker role per il processing in background delle operazioni, la scelta di un web role o di un web site, potrebbe essere &#8220;sovradimensionata&#8221;. Dato che i blob contenuti in un container pubblico sono accessibili attraverso delle semplici API (GET over HTTP), il gioco è fatto. Quello che possiamo esporre, non sono solo dei semplici file html, ma anche immagini, pdf, javascript&#8230;

Nulla di più facile insomma&#8230;una delle poche cose di cui tenere conto è il setup del ContentType di ogni blob, in modo che il browser sappia come gestire la risorsa.

Con la seguente funzione ricorsiva è possibile caricare su un particolare container la struttura, più o meno complessa, di un sito, partendo dalla root e mantenendo la gerarchia che caratterizza la struttura del sito stesso, simulando, come abbiamo visto nel [post precedente](http://blog.codiceplastico.com/melkio/index.php/2012/11/06/blob-storage-directory/), una gerarchia simil-filesystem, tramite l&#8217;utilizzo di &#8220;directory vistuali&#8221;.

```

```

Il risultato di tutto ciò è, per esempio, la disponibilità del sito di [codiceplastico](http://codiceplastico.blob.core.windows.net/website/index.html) hostato dal Windows Azure Blob Storage

