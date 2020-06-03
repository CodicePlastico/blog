---
layout: post
title:  "CQRS: un po' di confusione?"
comments: true
categories: tech
tags: [cqrs, metodologie]
---


Ultimamente si sente sempre più spesso parlare di CQRS ed EventSourcing: ormai se non spari qualche evento su un bus non sei nessuno :-)

L&#8217;intento di questo post è quello di cercare di fare un po&#8217; di chiarezza, per evitare che per risolvere piccoli problemi, se ne creino tanti-e-tanto-grossi.

Innanzitutto partiamo dal primo assunto: CQRS ed ES non viaggiano per forza di pari passo. Applicare CQRS non vincola in alcun modo a, potenzialmente, &#8220;sovrastrutturare&#8221; la nostra architettura, introducendo code, ESB, messaggi, asincronicità (and so on). Quindi è importante capire che non sempre ho bisogno di tutto.

Ed ora veniamo a CQRS.

Tutto prende spunto da un assunto di [Bertrand Mayer](http://en.wikipedia.org/wiki/Bertrand_Meyer), che riporto fedelmente: &#8220;Every method should either be a command that performs an action, or a query that returns data to the caller, but NOT BOTH.&#8221;

Vi ricorda qualcosa? :-)

Contestualizzando questa affermazione in ottica DDD ecco la &#8220;corretta traduzione&#8221; fornita da Greg Young: &#8220;A single model cannot be appropriate for reporting, searching and transactional behavior.&#8221;

CQRS è un &#8220;paradigma&#8221; molto semplice che enfatizza alcuni concetti basilari di DDD, che alla sola lettura del libro potrebbero rimanere &#8220;sommersi&#8221;:

- 
il domain model non nasce per collezionare o mostrare dati

- 
il domain model deve sempre essere in uno stato consistente

- 
i behavior stanno nel modello e non sicurazmente nella UI

- 
la UI &#8220;potrebbe&#8221; non necessariamente avere bisogno di un modello

- 
la UI &#8220;potrebbe&#8221; non avere alcun vantaggio dalla normalizzazione e dall&#8217;integrità referenziale



Il domain model è quello &#8220;strato&#8221; del nostro software che modella i behavior del business e che reagisce ad input esterni, siano essi input dell&#8217;utente tramite la UI, input da sistemi terzi, input derivanti da &#8220;cambi di stato&#8221; del nostro sistema. Non si può e non deve occuparsi di risolvere l&#8217;annoso problema delle ricerche, più o meno complesse, che sempre servono agli utenti.

Sembra l&#8217;uovo di colombo, ma ci scommetto quello che volete che tutti, almeno una volta nella vita (ma credo anche qualche volta di più), abbiamo commesso l&#8217;errore di pensare che un grafo di oggetti, più o meno complesso, potesse essere un domain model, che con l&#8217;ORM di turno avremmo risolto tutti i problemi di persistenza **e** di &#8220;caricamento&#8221;, pensando che lo stesso modello andasse bene per tutti i gusti, anzi fosse un grosso vantaggio.
Bene, ora che abbiamo fatto outing, pensiamo, per esempio, a quanto tempo abbiamo perso speso per ottimizzare query più o meno complesse, perché, con il modello che avevamo a disposizione, non potevamo fare altro che &#8220;caricare il mondo&#8221; sperando che un po&#8217; di lazy-loading qua e là avrebbe risolto il problema&#8230;

Se siamo d&#8217;accordo su quanto detto, abbiamo intuito/capito di quali problematiche stiamo parlando, le abbiamo vissute sulla nostra pelle e abbiamo la necessità di limitarle/risolverle.

Ecco in questo caso, forse, CQRS potrebbe fare al caso nostro.

Per ora mi fermerei qua, ricordandovi semplicemente la parte, per me, più importante del [famoso libro](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=pd_sim_b_4), il sottotitolo: &#8220;Tackling complexity in the heart of  software&#8221;.
Se non dobbiamo gestire complessità, probabilmente non siamo nel contesto ottimale per &#8220;sfruttare&#8221; i principi di DDD, non sempre abbiamo bisogno di CQRS, non sempre abbiamo bisogno di un domain model. Usarli per forza è uno degli anti-pattern per eccellenza&#8230;

