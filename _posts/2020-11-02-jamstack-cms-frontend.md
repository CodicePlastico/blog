---
layout: post
current: post
cover: 'assets/images/post-cover/20201102-css.jpg'
socialcover: 'assets/images/post-cover/20201102-css-s.jpg'
slug: 'jamstack-cms-frontend'
title: 'JamStack: prendiamo esempio dal passato per un futuro migliore'
date: 2020-11-02 09:00:00
category : tech
tags: [JAMstack, javascript, api, howto]
author: davide
---
<div class="post-intro">
    <p>In CodicePlastico amiamo realizzare progetti su misura che soddisfino al cento per cento le esigenze dei nostri clienti. La maggior parte del nostro lavoro consiste nell'<strong>analisi delle esigenze</strong> dell'azienda che abbiamo di fronte e nella <strong>progettazione e realizzazione</strong> di un applicativo software che risolva un problema specifico. </p>
    <p>Capita che i clienti ci chiedano di realizzare anche siti vetrina e in questo caso la scelta dello strumento migliore non è così scontata: <strong>CMS o NON CMS? questo è il dilemma!</strong> Come garantire performance e flessibilità per l'aggiornamento dei contenuti al cliente?<br/>
     <strong>E qui JAMStack che ci viene in aiuto.</strong></p>
</div>


<p style="text-align:center"><img src="/assets/images/post-content/jamstack/jamstack_problem.png" alt="feedback"/></p>

## Identificare il problema
Le richieste del cliente solitamente suonano più o meno così: _"Voglio un sito accattivante che mi permetta massima autonomia nella personalizzazione dei contenuti"_.

Tutti gli indizi sembrano portare ad una soluzione scontata: un [Content management system](https://en.wikipedia.org/wiki/Content_management_system).
<br>
<cite>Siamo sicuri che sia la soluzione migliore?</cite>

Sicuramente i CMS risolvono al meglio le richieste del cliente ma negli anni abbiamo scoperto alcuni difetti:

* _Performance_: la dinamicità delle pagine si paga in termini di elaborazione e quindi in tempi di riposta;
* _Hosting_: l'hosting di applicazioni dinamiche è generalmente più costoso di quello di applicazioni statiche;
* _Sicurezza_: bisogna prestare attenzione/porre rimedio alle vulnerabilità di server e database;
* _Applicazioni monolitiche_: la definizione, il recupero e la presentazione del dato sono in carico ad un unico software difficilmente segmentabile.
* _Scalabilità_: è generalmente più complicato far scalare un'applicazione dinamica e monolitica;

<p style="text-align:center"><img src="/assets/images/post-content/jamstack/jamstack_jamstack.png" alt="feedback"/></p>

## La soluzione
I problemi esposti sono facilmente superabili, ma la nostra allergia ai prodotti preconfezionati ci ha spinto ad esplorare altre strade e valutare **soluzioni alternative**.

Negli anni abbiamo valutato la possibilità di sviluppare un **CMS proprietario** ma non avremmo risolto nessuno dei problemi precedentemente esposti e, probabilmente, non avremmo grossi vantaggi rispetto ad un prodotto di terze parti.

In nostro soccorso è arrivato **JAMStack**, un nuovo stack di sviluppo basato su alcuni degli strumenti che utilizziamo quotidianamente: Javascript, API, Markdown.

Mathias Biilmann, CEO & Co-founder di Netlify lo definisce:

<cite>A modern web development architecture based on client-side JavaScript, reusable APIs, and prebuilt Markup</cite>

Non stiamo quindi parlando di uno specifico strumento ma bensì di un'idea, una metodologia, una filosofia di sviluppo.

<p style="text-align:center"><img src="/assets/images/post-content/jamstack/jamstack_dev-git-cicd.png" alt="feedback"/></p>

## Vantaggi

Ma quali sono i vantaggi di questa metodologia?

Innanzitutto il workflow tipico dello stack ben si sposa con le metodologie di sviluppo che applichiamo a tutti i nostri progetti:

* Sviluppo
* Utilizzo di un sistema di **version control (GIT)** per il salvataggio del codice e degli assets
* **CI/CD** per la generazione di build e il deploy automatico.

Il secondo grosso vantaggio è che **il risultato della build è un sito statico** che intrinsecamente risolve la totalità dei problemi che riscontravamo in un classico CMS. 

## Ma come si gestisce la dinamicità?
La domanda però sorge spontanea: come può un sito statico permettere al cliente di personalizzare i contenuti?

Le strade che si possono percorrere sono molteplici ma l'idea di fondo è quella di fornire all'utente uno **strumento che gli permetta di creare, modificare e cancellare contenuti**; a fronte di una modifica un sistema dovrà recuperare i nuovi contenuti, generare una nuova versione del sito e pubblicarla in produzione.

In pratica serve un CMS **(:facepalm!)**.

<p style="text-align:center"><img src="/assets/images/post-content/jamstack/jamstack_faceplam.png" alt="feedback"/></p>


_E quindi dove sta tutta la novità? Per risolvere i problemi di un CMS si utilizza un CMS?_

Sfruttando la **A** (API) o la **M** (Markdown) del JAMStack cerchiamo di riutilizzare questi strumenti in modo nuovo: ne prendiamo solamente la parte di creazione/gestione del dato tenendo separata la parte di presentazione.

<p style="text-align:center"><img src="/assets/images/post-content/jamstack/jamstack_headless.png" alt="feedback"/></p>

## Headless CMS

Sotto questa spinta sono nati alcuni prodotti chiamati Headless CMS. 
Si dividono principalmente in due categorie: 

* _Git-Based CMS_: producono Markdown che viene salvato sul sistema di Version Control;
* _API-first CMS_: espongono API che vengono consumate in fase di build del sito.

Nel primo caso andremo a produrre del Markdown che verrà salvato sul nostro sistema di Version Control.
Nel secondo esporremo delle API che verranno consumate in fase di build.
Ogni soluzione ha pro e contro. 

<p style="text-align:center"><img src="/assets/images/post-content/jamstack/jamstack_git-based.png" alt="feedback"/></p>

### Git-Based CMS
I vantaggi di un Git-Based CMS sono:

* Full version control di tutti i contenuti out of the box;
* I contenuti sono in un formato semplice da utilizzare;
* E' facile fare rollback delle modifiche;
* Seguono lo stesso git-based workflow che gli sviluppatori usano per il codice;
* No esiste vendor lock-in;
* Il setup è molto semplice.

Gli svantaggi sono:

* difficoltà di reperire i contenuti da fonti diverse;
* il processo di pubblicazione, modifica dei contenuti può essere lento.

Alcuni esempi di Git-Based CMS sono [Forestry](https://forestry.io/), [Crafter CMS](https://craftercms.org/) e [NetlifyCMS](https://www.netlifycms.org/)

<p style="text-align:center"><img src="/assets/images/post-content/jamstack/jamstack_api-based.png" alt="feedback"/></p>

### API-first CMS
Vediamo ora i vantaggi di una soluzione API-first:

* Permette di recuperare contenuti da più fonti;
* I contenuti possono essere fruiti da frontend diversi;
* Gestiscono facilmente grosse moli di dati.

I contro sono molto simili a quelli di un classico CMS e cioè costi di hosting, sicurezza e #scalabilità.

Alcuni esempi di API-first CMS sono [Storyblok](https://www.storyblok.com/), [Contentful](https://www.contentful.com/), [Sanity](https://www.sanity.io/), [Ghost](https://ghost.org/) e [Strapi](https://strapi.io/)


<p style="text-align:center"><img src="/assets/images/post-content/jamstack/jamstack_frontend.png" alt="feedback"/></p>

## E il frontend?
Sul fronte della presentazione del dato abbiamo anche qui diverse possibilità.

In CodicePlastico utilizziamo spesso Angular e React e siamo rimasti particolarmente colpiti da due prodotti: [Gatsby](https://www.gatsbyjs.com/) e [NextJS](https://nextjs.org/)

Entrambi i prodotti ci permettono di utilizzare React ma ci semplificano sul fronte di alcune tematiche per quanto riguarda lo sviluppo, il processo di build e il deploy.

## Quali vantaggi?

Nella nostra breve esperienza con questa nuova metodologia abbiamo riscontrato alcuni vantaggi:

* abbiamo ridotto (e in alcuni casi addirittura azzerato) i costi di hosting mantenendo comunque un ottimo livello di servizio;
* abbiamo riscontrato ottime performance dei siti in termini di First Contentful Paint e Time to Interactive;
* in alcuni casi abbiamo scelto un CMS [SASS](https://en.wikipedia.org/wiki/Software_as_a_service) come ad esempio Sanity per focalizzarci unicamente sul prodotto finale.

In CodicePlastico siamo soddisfatti di questo cambio di rotta e speriamo di avervi anche solo invogliato a valutarne la bontà in prima persona.

Siamo consapevoli che *non sia la soluzione definitiva* a tutti i problemi ma ci sembra un buon approccio per risolvere un problema complesso.