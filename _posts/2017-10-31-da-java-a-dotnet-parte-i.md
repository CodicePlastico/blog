---
layout: post
title:  "Da Java a .Net: la mia esperienza, due anni dopo"
subtitle: "A che cosa non rinuncerei e che cosa mi manca (parte prima)"
date: 2017-10-31 12:00:00
categories: java,.net
slug: da-java-a-dotnet-parte-i
tags: [Tech]
author: pietro
---
## Un chiarimento
Il presente post vuole affrontare l'annosa questione di un confronto tra due delle piattaforme di sviluppo più utilizzate (Java/JEE e .Net), con l'intento di raccontare qualcosa dei miei ultimi due anni e mezzo di lavoro, nei quali mi sono avventurato nel mondo Microsoft dopo un decennio di sviluppo sulla piattaforma Sun/Oracle.
Affronterò la questione non solo confrontando i due (per altro molto simili) linguaggi principali utilizzati per fare sviluppo su JVM e su .Net, bensì anche riportando le mie impressioni su quali sono le differenze tra le due piattaforme in questione e, più in generale, tra gli ecosistemi che ad esse fanno riferimento.

## Disclaimer
L'intento di questo post non è quello di dare il via a _flame_ o guerre di religione: è semplicemente quello di riportare la mia esperienza, per quel che vale. Presenterò pertanto le mie opinioni, le quali - come dovrebbe essere ovvio ma è sempre utile ricordare - dipendono fortemente dalla mia _forma mentis_ e non hanno la pretesa di avere un qualche valore assoluto.

## Due linguaggi
Java e C#, senza dubbio i due linguaggi più utilizzati per fare sviluppo sulle due piattaforme in questione, sono per molti versi due linguaggi molto simili: entrambi linguaggi _C-like_, si differenziano nell'uso di tutti i giorni per alcune _feature_, che uno solo dei due ha o che sono implementate in modo diverso nei due casi. Di seguito, una lista (non esaustiva) delle principali differenze (e del mio parere in proposito).

### Property
Cresciuto _javista_, nel primo periodo di passaggio a C# non apprezzavo più di tanto questa _feature_, che ora invece mi piace molto; nulla di indispensabile (fondamentalmente uno zucchero sintattico), ma un modo utile per distinguere - a livello di interfaccia [OO](https://en.wikipedia.org/wiki/Object-oriented_programming) - tra _query_ e _command_. Non un motivo sufficiente per preferire C# a Java ma, dato che ci sono... perché non usarle?

### Enum
Implementazione _C-style_ in C# (ogni elemento di una `enum` è, fondamentalmente, una costante intera, sul cui utilizzo il compilatore fa ben pochi controlli), implementazione _object oriented_ in Java (ogni elemento dell'enum è istanza di una classe e le classi corrispondenti ai diversi elementi dell'enum hanno una superclasse comune): ogni elemento dell'enum può avere attributi e metodi (con polimorfismo). Nulla che non si possa fare anche in C#, col vantaggio però che, nel caso di codice Java, il compilatore si occupa dei dettagli _noiosi_.

### Generics
Simile nella sintassi, il supporto per l'utilizzo di _generics_ costituisce la differenza più grossa tra i due ambienti a livello di piattaforma (vedi sezione successiva); questo si riflette in differenti possibilità di utilizzo, per cui nel codice C# ci sono possibilità che agli sviluppatori Java non sono disponibili. Su tutte:

* `new T()`
* _overloading_ sui tipi parametrici (possibilità cioè di definire metodi _generici_ le cui firme differiscono solo per il numero di tipi parametrici)

Senza dubbio l'unico motivo serio per cui sceglierei un linguaggio piuttosto che l'altro (ma, come dicevo, è questione di piattaforma sottostante, più che di linguaggio in sé).

### Conversioni implicite di tipo ed extension method
Si tratta di due _feature_ di C# che mi piacciono molto (benché sia facile finire per abusarne): in particolare, mi sembrano assai preziose quando

* si ha necessità di aggiungere regole/comportamenti _di business_ a tipi standard (penso ad esempio a metodi di utilità che estraggano un elemento a caso da una lista o convertano un'istanza di `DateTime` in _epoch_ UNIX)
* si vuole sviluppare qualcosa di simile ad un [DSL](https://en.wikipedia.org/wiki/Domain-specific_language)
* si vogliono separare alcuni comportamenti aggiuntivi (metodi _di utilità_) dal modello di base che rende disponibili struttura dati e logica di business

### Parametri `out` e `ref`
Si tratta di una _feature_ disponibile in C# e non in Java; non ne sentivo la mancanza e, se non si tratta di utilizzare API scritte da altri che ne fanno uso, per parte mia probabilmente non la utilizzerei mai (fondamentalmente perché _rompe_ l'idea di metodi come funzioni pure, utilizzando un approccio che decisamente non è nelle mie corde).

## Convenzioni
Diversi linguaggi, diverse piattaforme... e diverse convenzioni - tema sul quale tradizionalmente le persone si accapigliano senza cambiare di una virgola le proprie abitudini: il Bene contro il Male. Nel dettaglio:
### Formattazione
Due anni e mezzo di _coding_ C#, ed ancora non riesco a considerare _bello_ il modo in cui tale codice si presenta per quanto riguarda la formattazione. La posizione delle graffe, che nel mondo Java vede coesistere una scuola di pensiero maggioritaria ed una minoritaria con abitudini diverse, nel mondo C# è tradizionalmente quella mutuata dal mondo C. A livello di pura estetica, non avrei dubbi: non mi piace per nulla. A livello di opportunità, al solito, sono convinto che il punto focale sia, più che utilizzare una convenzione _propria_, utilizzare una convenzione _di team_, quale che sia, condivisa ed accettata, che renda facile agli uni mettere mano a codice scritto da altri. Al di là delle convenzioni _ufficiali_ (Microsoft, come Sun, pubblica le proprie), dunque, darei valore a convenzioni _localmente condivise_ - non per niente qualunque ambiente di sviluppo, per qualunque linguaggio o piattaforma sia pensato, permette di personalizzare facilmente le regole di formattazione da applicare. In questo senso, per giungere alla definizione di uno stile _di team_ o _di progetto_, valuterei - oltre ovviamente alle abitudini pregresse dei membri del _team_ - il grado di _frizione_ che l'adozione di una particolare convenzione produce a fronte del fatto che su un particolare progetto si lavora spesso in più di un linguaggio. Produce più frizione leggere codice C# formattato in modo inusuale per quel linguaggio, o passare di continuo tra sorgenti C# formattati in un modo e sorgenti JavaScript formattati in un altro?

### Naming convention
Altro tema _difficoltoso_ per chi passa da una piattaforma all'altra, che possiamo riassumere citando il caso più evidente: _iniziale maiuscola o minuscola per il nome dei metodi?_ Personalmente qui non avrei dubbi: adottare la stessa convenzione adottata nella libreria standard su cui si basa l'utilizzo del linguaggio (oltre che per quanto riguarda il _case_ delle iniziali, tradizionalmente differente in Java ed in C#, lo stesso discorso vale per quanto riguarda l'adozione di uno stile di nomenclatura [_camel case_](https://en.wikipedia.org/wiki/Camel_case) piuttosto che di uno stile [_snake case_](https://en.wikipedia.org/wiki/Snake_case)): dunque metodi con nome maiuscolo in C#, con nome minuscolo in Java.
E comunque dopo due anni e mezzo inizio *quasi* a _pensare_ i nomi dei metodi con la maiuscola... ;-)

Tra una settimana proseguiremo parlando delle caratteristiche peculiari delle due piattaforme e della vitalità dei due ecosistemi.