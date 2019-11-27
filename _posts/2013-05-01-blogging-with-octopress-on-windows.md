---
layout: post
title:  "Blogging with octopress on windows"
comments: true
categories: Technology
---


Come ha già anticipato [ema](http://ema.codiceplastico.com)[qui](http://ema.codiceplastico.com/blog/2013/03/25/aggiornate-i-vostri-reader/), il blog di [codiceplastico](http://www.codiceplastico.com) ha subito la prima di una lunga serie di modifiche a cui sarà sottoposto (spero) nei prossimi mesi. Per ora stiamo parlando solo di &#8220;modifiche&#8221; infrastrutturali: è stato spostato da una server virtuale su Aruba, a [github](http://github.com).

Ma la modifica più sostanziale è stata quella relativa alla scelta del blog engine: abbiamo deciso di migrare dal famoso wordpress ad una engine ben più light, ma più in linea con le nostre esigenze&#8230;[octopress](http://octopress.org/)

Perchè questo &#8220;cambio di direzione&#8221;? Essenzilamente perchè eravamo alla &#8220;ricerca della semplicità&#8221; :)

Per quale motivo abbiamo bisogno di un db, pagine dinamiche (and so on) per un blog engine, quando una volta scritto un post, altro non è che una pagina statica che non cambierà mai e poi mai, ma che anche in caso di cambiamento possiamo facilmente rigenerare? 
Avere &#8220;il sorgente&#8221; dei nostri post non all&#8217;interno di un database (con tutte le operazioni di maintenance che ne conseguono), ma in semplici file *.markdown, nel caso di octopress, salvati su file system, ancora più facilmente backuppabili, è stato un altro dei capisaldi della scelta.

Detto questo, sul sito di octopress ci sono tutte le indicazioni per il setup su mac o linux, ma su windows? Funziona tutto anche lì? Anticipo la risposta: ovviamente sì, visto che io sto proprio bloggando da lì    
Essenzialmente octopress, banalizzando molto, altro non è che una serie di rake task che ci permettono di configurare, generare, deployare il nostro blog. Quindi diciamo che il primo passo è avere ruby, o meglio, la versione supportata di ruby (1.9.3) installata.

Come fare? Ho trovato un bel progetto open source che simula, anche sul SO di casa Microsoft, i comportamenti di [rbenv](https://github.com/sstephenson/rbenv/) o [rvm](https://rvm.io/).
Ecco a voi: [yari](https://github.com/scottmuc/yari)

I passi per il setup sono veramente rapidi ed indolore: 
*   clone del repository di yari (per esempio in d:\tools\yari) 
*   aggiunta di d:\tools\yari\bin al path 
*   installare la versione desiderata di ruby con il comando: yari 1.9.3

Fatto questo siamo a metà dell&#8217;opera: ora possiamo clonare il [repository di octopress](https://github.com/imathis/octopress) e seguire le istruzioni che troviamo sul [sito](http://octopress.org/docs/setup/) per configurare il blog e iniziare a bloggare. 
Ci sono una serie di plugin già pronti all&#8217;uso, come l&#8217;integrazione con twitter, con disqus, e chi più ne ha più ne metta.

Tutto ha funzionato al primo colpo, nessun problema di setup o altro, bisogna solo imparare un po&#8217; la sintassi di markdown per scrivere e presentare il post in un modo decente :), ma con una console e un editor di testo siete up&amp;running in pochi minuti.

Unica piccola nota: potrebbe capitare che uno dei task ruby, quello per la generazione vera e propria del sito, faccia &#8220;a cazzotti&#8221; con il code-page della vostra console, generando un errore del tipo

```
Liquid Exception: incompatible character encodings: UTF-8 and IBM437 in index.html   

```

Per ovviare a questo problema basta cambiare il code-page di default, ad uno che sappia digerire i caratteri UTF, con il comando

```
chcp 65001

```

Fatto questo dovrebbe essere tutto ok :) 
Che altro dire&#8230;happy blogging!!!

