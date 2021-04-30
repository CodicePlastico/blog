---
layout: post
current: post
cover: 'assets/images/post-cover/2021-SPA-vs-WA.jpg'
socialcover: 'assets/images/post-cover/2021-SPA-vs-WA-s.jpg'
slug: spa-vs-classic-web-app
title: "SPA vs Classic Web App"
date: 2021-04-29 09:00:00
category : tech
tags: [programming]
author: [ema]
---

## I bei vecchi tempi, erano davvero belli?

Programmo da abbastanza tempo per ricordarmi le prime applicazioni web sviluppate con [ASP.NET](http://asp.net/) ~~WebForm~~, MVC e Ruby on Rails, mi ricordo dei post-back, dei linguaggi di templating (quanto era bello [Haml](notion://www.notion.so/emadb/Spa-vs-Classic-Web-App-ee48da00e18747f6ac16d208ddedc3b2?)) e naturalmente di jQuery.

Ma ricordo anche che c’era qualcosa che stonava, che non era fatto come volevo, i browser stavano diventando sempre più veloci e ricchi di funzionalità, Chrome aveva lanciato il suo browser con all’interno V8, e noi con il rendering server side e jQuery lo stavamo sottoutilizzando caricando sempre di più il server.
Inoltre, mentre sul backend si faceva uso di pattern efficaci per ottenere la separation of concern e una struttura organizzata, lato client si abbozzava qualcosa di molto più procedurale senza una vera e propria forma.


<figure style="text-align:center"><img src="/assets/images/post-content/spavscwa/spavscla_ancient.png" alt="old times" /></figure>

Poi, nel 2010 durante l’**Agile Day** [Luca Grulla](https://twitter.com/lucagrulla) fece un talk dal titolo **[Secret art of javascripting](https://www.agileday.it/2010/www.agileday.it/front/programma/secret-art/index.html)** che fece chiarezza su quel malessere che sentivo mentre sviluppavo applicazioni web: la mancanza di dignità e di una vera architettura design nel codice Javascript.

Luca disse una frase, una cosa del tipo: 

<cite>nel browser gira un’applicazione che utilizza i dati che provengono dalle API</cite>

Sembra banale ma fu allora che capii che il client e il server erano due contesti separati che comunicavano via API, proprio come una vista di un’applicazione WPF comunicava con un servizio, o un client con un database.

Nel 2010 usci anche Knockout.js per me il primo framework che permetteva di dare una struttura sensata a tutto il codice client side applicando quello che era il pattern MVVM.
Poi fu la volta di Backbone, Angular, React e tutti gli altri. 

**Il mondo dello sviluppo web era cambiato.**

## **2010: benvenute SPA**

Le app che vengono eseguite nel browser si chiamano SPA (Single Page Application) proprio perché non esiste più il concetto di pagina, ma di *schermata* che cambia e si modifica a seconda dell’interazione con l’utente. Il **server si “limita” a servire i dati** e a ricevere i comandi dal browser (e naturalmente, si spera, a gestire tutta la logica dell’applicazione con la sicurezza necessaria).


<figure style="text-align:center"><img src="/assets/images/post-content/spavscwa/spavscla_serve.png" alt="let'serve" /></figure>

Tutto ciò ha portato ad un **notevole miglioramento per gli utenti** delle applicazioni web: interfacce più usabili, flussi più fluidi, performance migliori e interazioni paragonabili a quelle delle App desktop. 

Ma ha anche portato ad un aumento dei costi di sviluppo: sviluppare una SPA è quasi sempre più costoso. Comunque il bilancio è positivo: lieve aumento dei costi a fronte di un enorme miglioramento della UX.

O no?
Io direi di sì ma c’è un problema.

## Le SPA vanno di moda anche quando non servono?

La realtà è che oggi sviluppare applicazione web vuol dire dare per scontato di sviluppare SPA, senza mai mettere in dubbio questa scelta. Ma siamo certi che debba sempre essere così?

Racconto cosa è successo qualche mese fa in CodicePlastico.

Per un nostro cliente, una Startup del settore cosmesi, avevamo sviluppato un’applicazione di e-commerce (Nodejs + Angular). Nel tempo lo shop si è ingrandito e ci è stato chiesto di rivedere da zero il backend per gestire i prodotti a catalogo.
Come al solito, call di analisi, wireframe con Figma e, dopo l’accettazione da parte del cliente, stima dei tempi di realizzazione.
Senza porci domande su come l’avremmo fatta abbiamo stimato il tutto in circa **3-4 iterazioni da 1 settimana**, tecnologia: Nodejs + Angular.

Poi ci siamo fermati un attimo a riflettere. 

<figure style="text-align:center"><img src="/assets/images/post-content/spavscwa/spavscla_phoenix.png" alt="Phoenix" /></figure>

Di fatto, la richiesta era di un backend per permettere ai pochi amministratori (3-4 utenti) di fare CRUD sui prodotti: 4 iterazioni sono parecchie e di fatto molto costose, soprattutto per una startup.

Ci siamo chiesti: e se facessimo la classica applicazione MVC con le viste renderizzate server side, la validazione con HTML ed eventualmente qualche piccolo script lato client? 
E se usassimo un framework tipo RubyOnRails?

Non ci è voluto molto per convincerci che era la soluzione giusta e siamo partiti con **Phoenix** e tutte le facility e i generators che mette a disposizione.

Risultato, in **1 iterazione avevamo finito tutta l’applicazione.**

Il **cliente è stato contento del risultato**: ha risparmiato e ha ottenuto quello che serviva. Potevamo dare una UX migliore? Sicuramente, ma in questo caso il costo che avrebbe dovuto sostenere non giustificava il benefit che è davvero minimo in questo contesto.

## MVC con Server Side Rendering, is not dead

Vi ho raccontato questo per dire che secondo me le applicazioni web MVC con il server side rendering non sono morte, hanno ancora il loro **motivo di esistere e ci sono contesti in cui sono preferibili**. Sono generalmente più facili da realizzare e i framework forniscono una serie di tool che ne velocizza la scrittura.

<figure style="text-align:center"><img src="/assets/images/post-content/spavscwa/spavscla_mvc.png" alt="MVC" /></figure>

Penso anche a **MVP** da realizzare velocemente, a semplici applicazioni CRUD, a backend di amministrazioni, a pannelli di sola visualizzazione dati.

Insomma come al solito **la tecnologia è come la moda: certe cose tornano sempre e conviene tenerle negli armadi**. O no?

Direi di si e, proprio come la moda, torna con qualche variazione, quando dettaglio che prima era diverso, infatti per dirla tutta oggi stanno prendendo piede alcune librerie che sono a metà strada tra la SPA e il SSR, parlo di [Phoenix.LiveView](https://hexdocs.pm/phoenix_live_view/Phoenix.LiveView.html), [HTML Over The Wire con Hotwire](https://hotwire.dev/) [StimulusReflex](https://docs.stimulusreflex.com/). 

Ma questo è un ottimo argomento per un altro post.
Alla prossima!








