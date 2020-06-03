---
layout: post
title:  "Blob storage directory"
categories: tech
tags: [azure, microsoft, tech]
---


Questo post è parte di una serie su windows azure. Ecco il [link](http://blog.codiceplastico.com/melkio/index.php/2012/10/15/azure-intro/) per la lista di tutti i post precedenti.

In un [post precedente](http://msdn.microsoft.com/en-us/library/windowsazure/dd179352.aspx) abbiamo definito in linea di massima le capabilities del windows azure blob storage. Anche da un overview sommaria la prima cosa che si nota è che, nel blob storage, non c&#8217;è alcun riferimento al concetto di &#8220;directory&#8221;: ogni file è un blob, ogni blob appartiene ad un container, ma non sembra sia possibile, definire una sorta di gerarchia che possa catalogare, in modo  molto simile a quanto siamo abituati a fare con il file system, i nostri file (blob).

Nonostante quanto detto sia assolutamente vero, è possibile simulare comunque il concetto di directory, in modo, direi, alquanto facile.

Il primo modo, il più diretto, è quello di utilizzare il nome del file per simulare l&#8217;appartenenza ad una gerarchia, come avviene nell&#8217;esempio seguente:

```

```

Un altro modo, più lineare, è quello di utilizzare le API messe a disposizione dall&#8217;SDK e descrivere il processo di upload in modo un po&#8217; più più dettagliato, ma anche più leggibile, definendo esplicitamente le proprie intenzioni:

```

```

Analizzando quello che succede, attraverso il management portal, abbiamo la conferma di quanto asserito in precedenza: non c&#8217;è alcune traccia del concetto di directory. Infatti, all&#8217;interno del container, abbiamo una visualizzazione piatta dei file caricati, siano essi nella root o in qualche &#8220;directory virtuale&#8221;.

![](http://melkio.codiceplastico.com/images/uploads/2012/11/Blob-storage-virtual-directory-300x54.png)

Il concetto di &#8220;virtual directory&#8221;, però, non è scomparso e interagendo con il container, tramite le API (ma anche con i servizi REST messi a disposizione dalla piattaforma) abbiamo la possibilità di navigare all&#8217;interno di questa gerarchia. Nell&#8217;esempio precedente, ciclando sui blob contenuti nella root del container, otteniamo il seguente risultato:

```

```

```

```

Ovvero, anche se non fisicamente, la directory è effettivamente presente.
Cerchiamo ora di capire cosa avviene &#8220;dietro le quinte&#8221;, utilizzando fiddler. A fronte dell&#8217;operazione di listing dei blob del container o di una virtual folder, le API, internamente, eseguono una GET verso l&#8217;url che identifica il blob storage, utilizzando dei parametri &#8220;ad hoc&#8221; nella query string:

```

```

Nel nostro caso, le due chiamate differiscono per un semplice parametro: &#8220;prefix&#8221;. Come [documentato](http://msdn.microsoft.com/en-us/library/windowsazure/dd179352.aspx), questo parametro permette di fare delle &#8220;sub-select&#8221; di tutti i blob che rispettano il pattern.

In questo, semplice, modo abbiamo la possibilità di simulare la gerarchia simile a quella del file system, anche sul windows azure blob storage.

