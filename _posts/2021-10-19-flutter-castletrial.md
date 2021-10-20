---
layout: post
current: post
cover: 'assets/images/post-cover/2021-castletrial.jpg'
socialcover: 'assets/images/post-cover/2021-castletrial-s.jpg'
slug: flutter-castletrial
title: 'Facciamo un videogioco?'
date: 2021-10-19 09:00:00
category : [vita-plastica]
tags: [design, flutter, mobile]
author: [andrea, nicholas]
---

Certo! Ma perché? Prima di tutto, molti in CodicePlastico sono appassionati di videogiochi ([ehi, abbiamo anche un cabinato in ufficio, sai?](https://blog.codiceplastico.com/arcade)): c’è chi è più legato ai grandi classici, chi continua imperterrito a seguire le nuove uscite e chi invece seleziona accuratamente il titolo giusto per il tempo libero che ha a disposizione. Seconda cosa,creare qualcosa di interno all’azienda ci permette di _provare tecnologie diverse_ da quelle che usiamo abitualmente e che potrebbero, alla fine, portare nuove opportunità.

<figure style="text-align:center"><img src="/assets/images/post-content/castletrial/Castletrial_intro.png" alt="castletrial" /></figure>

## Da dove partire?

Partiamo con qualcosa di semplice, ci siamo detti.

Crediamo fermamente negli **MVP**, ci siamo dati uno scope ridotto (sia temporale che di funzionalità) rispetto a tutte le idee emerse in fase di brainstorming: siamo partiti agli **albori dell’industria videoludica** pensando a quei prodotti di stampo prettamente narrativo.

## Avventura testuale e librogame

Le avventure testuali erano quei videogiochi degli anni '80 in cui l’utente doveva inserire **comandi testuali per esplorare il mondo** e interagire con l’ambiente nel quale si trovava.

In alcuni casi queste avventure erano corredate con delle immagini evocative che aiutassero l’immersione del giocatore o che dessero un suggerimento visivo per particolari situazioni.

Se consideriamo questo tipo di approccio come un lavoro letterario applicato al videogioco, allora queste immagini non interattive rappresentano le _illustrazioni di un libro_.

<figure style="text-align:center"><img src="/assets/images/post-content/castletrial/Castletrial_01.png" alt="castletrial" /></figure>

A proposito di libri, ci siamo rifatti concettualmente ai **librogame** dove, al posto che inserire i comandi per muoversi nel gioco, le azioni vengono effettuate scegliendo tra poche opzioni disponibili che portano l’utente avanti nel racconto.

Trovata la tipologia, il genere, almeno per il team, era relativamente scontato: il **fantasy**.

Lo **stile grafico**? Quello del nostro blog, per mantenere continuità grafica e renderlo nostro ma puntando comunque a una reinterpretazione medievale per una maggiore immersione.

Il tipo di **narrazione** invece, è decisamente metafisico.

E per quanto riguarda la **durata**, non più di una pausa caffè lunga (così non ti distrai!).

## Sì, ma come l’avete sviluppato?

Abbiamo usato **Flutter**, che è un Toolkit UI creato da Google per lo sviluppo di applicazioni multipiattaforma compilate nativamente per Mobile (iOS/Android), Web, Desktop o embedded (come ad esempio macchine, frigoriferi, termostati).

Partito in sordina rispetto a React Native, oggi le cose stanno cambiando: se prima la community non era abbastanza grande, ora **sta crescendo esponenzialmente** e al momento è il framework di maggior interesse (su GitHub) paragonato ai suoi fratelli React Native e Ionic (grazie per l’info, Google Trends! :D).

<figure style="text-align:center"><img src="/assets/images/post-content/castletrial/Castletrial_02.png" alt="castletrial" /></figure>

**Flutter ha degli ottimi vantaggi rispetto ai concorrenti:**

### Multipiattaforma

Il primo grande vantaggio dell'utilizzo di Flutter è il motivo per cui è stato creato: la possibilità di avere una **singola codebase che è in grado di essere compilata nativamente per più piattaforme** mantenendo una user experience identica su tutti i dispositivi, da mobile a embedded.


### Dart

Molti mettono questo punto come "_contro_" poiché si tratta di un nuovo linguaggio da imparare al contrario di React Native, che utilizza Javascript ed è quindi di più facile accesso a chi ha già esperienza con lo sviluppo web.

Abbiamo trovato **Dart molto semplice da imparare**, con un approccio molto più simile a Java e fortemente tipizzato, per noi è stato un grande vantaggio perché ci ha permesso di strutturare il codice in modo più semplice e chiaro.


### Hot Reload

Molte persone hanno avuto molti problemi con l'hot reload di React Native, prevalentemente **crash**, e problemi di **velocità**: tutto questo con Flutter, su questo progetto non è accaduto. Lo sviluppo è molto fluido: scrivi codice, salvi nell'editor e in un attimo il codice è già stato caricato ed eseguito sul dispositivo, che sia fisico o virtuale.


### Widgets

**Ogni cosa è un widget**: dalla barra di navigazione, alle immagini, alla schermata che viene visualizzata. Flutter mette a disposizione moltissimi Widget che partono con uno stile basato su Material Design già pronti e *customizzabili* per poter ottenere ottimi risultati from scratch in pochissimo tempo. Questo permette anche di ottenere la stessa grafica su diverse versioni di Android per esempio. Volete un tema IOS? C’è Cupertino Widget.

Di contro, per ora abbiamo riscontrato che **ci sono (ovviamente) meno librerie rispetto a React Native** (che ha una manciata d’anni di vantaggio rispetto a Flutter) ma per il tipo di app che abbiamo sviluppato questa carenza non è stata non un problema. 

Il grande vantaggio di poter lavorare su **side project** di questo tipo risiede proprio nella possibilità di fare formazione, approfondire tecnologie e sviluppare dall’inizio alla fine (la fine di un’iterazione almeno :D) qualcosa di spendibile anche su altri progetti.

<figure style="text-align:center"><img src="/assets/images/post-content/castletrial/Castletrial_03.png" alt="castletrial" /></figure>

## Sì, ok la parte tecnica, ma dove trovo il gioco?

**Castletrial**, questo il suo nome, lo puoi trovare qui per Android [https://play.google.com/store/apps/details?id=com.codiceplastico.cp_castletrial](https://play.google.com/store/apps/details?id=com.codiceplastico.cp_castletrial)

E per IOS? **Sta arrivando!** (Se vuoi sapere esattamente quando ti basta aspettare la prossima newsletter. [Come, non sei ancora iscritto?](https://codiceplastico.us18.list-manage.com/subscribe/post?u=3a6d4d4fc68bc32031e87e865&id=2f6137b5df))

Questo è ovviamente un primo passo, ci piacerebbe sapere la tua per capire come possiamo ottimizzarlo e se le feature successive che avevamo in mente rientrano nei tuoi feedback. **Buona partita e mi raccomando...**

<figure style="text-align:center"><img src="/assets/images/post-content/castletrial/Castletrial_outro.png" alt="castletrial" /></figure>

## #nospoiler
