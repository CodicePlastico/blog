---
layout: post
title:  "CQRS: perchè e quando"
comments: true
categories: tech
---


Ok, nel [post precedente](http://blog.codiceplastico.com/melkio/index.php/2012/09/24/cqrs-un-po-di-confusione/) abbiamo cercato di definire l&#8217;argomento. Ora cerchiamo di capire due cose fondamentali per l&#8217;adozione di qualsiasi tool/metodologia/pattern: quando e perchè usarlo.
Se capiamo e padroneggiamo questi concetti, abbiamo molte più probabilità di successo nella scelta degli strumenti/metodologie da utilizzare a seconda del contesto in cui ci troviamo.

Innanzitutto **perchè**.
Non credo ci sia un perchè uguale per tutti, ma per me e per la mia personale esperienza il valore aggiunto &#8220;indotto&#8221; nei progetti in cui [decidiamo](http://www.codiceplastico.com) di adottare questa &#8220;metodologia&#8221; (*) è il fatto che, CQRS, ci permette di focalizzare l&#8217;attenzione e far emergere in modo più marcato i comportamenti a cui la nostra applicazione deve reagire.

Il fatto di scrivere uno snippet di codice del tipo:

```

```

permette di esplicitare in modo chiaro i desiderata dell&#8217;utente e un behaviour che &#8220;qualcuno&#8221;, all&#8217;interno del mio modello, dovrà soddisfare. Questa semplicissima classe, oltre che a veicolare le informazioni necessarie all&#8217;esecuzione del comando, porta con sè un alto valore informativo: la semantica del comportamento da &#8220;eseguire&#8221;. Il nome non è ovviamente di secondaria importanza :-)
Di command in command, emergeranno quasi naturalmente i confini di ogni aggregate (la cosa più difficile in un approccio DDD, secondo il mio punto di vista) e il partizionamento verticale, più o meno spinto, del dominio, vi garantirà una maggiore flessibilità nell&#8217;evoluzione dello stesso, evitando che un object model fortemente interconnesso, costituito da un grafo più o meno profondo, sia il freno all&#8217;evoluzione del vostro sistema.

Un&#8217;altra componente che verrà pesantemente rivisitata (leggi stravolta), acquisendo finalmente senso, è il tanto vituperato repository. Quanti di noi si sono trovati con la definizione dell&#8217;interfaccia di un repository simile alla seguente:

```

```

Per testare l&#8217;interazione con il nostro repository, iniettato nel controller di turno :-) dovevamo decorare la definizione del contratto con dei metodi sempre più assurdi, tanto più assurde erano le richieste di **ricerca** che il nostro cliente richiedeva (e vi posso assicurare che la fantasia dei cliente non ha mai fine&#8230;ma che ve lo dico a fare), violando non solo i concetti base di DDD, ma di qualsiasi forma di OOD.
Quei metodi, se non hanno a che fare con la parte &#8220;comportamentale&#8221; del nostro dominio, non devono stare lì. Punto e basta.

Sfruttando questo approccio, in un progetto su cui stiamo lavorando con [Mauro](http://milestone.topics.it/), i repository hanno solo e soltanto i metodi necessari per interagire con il dominio, rendendo molto più light la sua definizione. E dando soprattutto un senso compiuto alla sua esistenza, non vedendolo più solo come una &#8220;facade&#8221; che ci permette di testare l&#8217;interazione con uno storage.

Ed ora passiamo al **quando**. Sinceramente non è semplice definire una regola su quando &#8220;tentare&#8221; un approccio del genere. E&#8217; molto più semplice dirvi quando non usarlo :-)

Se vi trovate dei command del tipo:

```

```

statene pur certi che c&#8217;è qualcosa che non va, ve lo posso garantire. In un contesto del genere, non avete comportamenti, la famosa logica di business, ma solo operazioni CRUD.
Non solo CQRS, ma anche lo stesso approccio con DDD, in un contesto del genere è semplicemente un overhead inutile e costoso, che non porterà alcun beneficio al vostro progetto, rallentando inutilmente i lavori.

Che dire&#8230;spesso nei talk che tengo sull&#8217;argomento tendo a ripetere fino alla nausea la definizione che [Martin Fowler dà di DomainModel](http://martinfowler.com/eaaCatalog/domainModel.html):

&#8220;&#8230;An object model of the domain that incorporates both behavior and data&#8230;&#8221;

La parolina magica è quel &#8220;both&#8221;: se abbiamo solo stato (e quindi siamo in un&#8217;ottica data driven) o stiamo sbagliando qualcosa, o stiamo semplicemente cercando di ficcare un domain model laddove non serve. E ficcare le cose a forza dove non servono, non è mai molto piacevole :-)

