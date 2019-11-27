---
layout: post
title:  "Query polimorfiche con il table storage"
comments: true
categories: Technology
---


Il [table storage](https://www.windowsazure.com/en-us/develop/net/how-to-guides/table-services/) è l&#8217;implementazione, messa a disposizione da Windows Azure, di un datastore NoSQL, ideale per lo storage di dati strutturati, che non necessitano del concetto di &#8220;relazionalità&#8221;.

Con la nuova versione della &#8220;client library&#8221; per .Net sono state introdotte diverse feature interessanti. Fra queste la possibilità di interagire con la &#8220;pipeline&#8221; di serializzazione/deserializzazione di un&#8217;entità.  Questa possibilità apre scenari che prima di questo rilascio non erano (facilmente) implementabili.

In questo post vedremo come supportare, con poco sforzo, query polimorfiche con il table storage.

In prima battuta, ridefiniamo per la root della nostra gerarchia, il metodo di serializzazione, aggiungendo, oltre all&#8217;implementazione &#8220;standard&#8221; che prevede la serializzazione di tutte le property pubbliche, anche una nuova property &#8220;calcolata&#8221; che corrisponde al tipo concreto che stiamo persistendo:

```

```

&#8230;ed ora, innestiamoci effettivamente nella pipeline, con un&#8217;implementazione ad-hoc del delegate [EntityResolver](http://msdn.microsoft.com/en-us/library/windowsazure/jj733144.aspx):

```

```

In questo modo possiamo scrivere una query polimorfica, sfruttando il fatto che, prima dell&#8217; &#8220;idratazione&#8221; delle property della entity, possiamo utilizzare una logica custom (tramite una factory o altro), per la creazione del tipo concreto, ottenendo, con poco sforzo, il risultato cercato:

```

```

