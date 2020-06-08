---
layout: post
title:  "La ricerca non è solo una ricerca"
comments: true
tags: [ddd, cqrs]
categories: tech

author: ale
---

Ultimo post datato: 07/10/2015...direi che è ora di ricominciare a bloggare, anche per evitare che [@mauroservienti](http://blogs.ugidotnet.org/topics) possa continuare indisturbato la sua serie di post su _Service (UI) composition_ senza qualcuno che faccia da bastian contrario :)  
Non raggiungerò mai il suo rate di post, ma qualcosina da dire o aggiungere sull'argomento lo possiamo di sicuro trovare.

Partiamo da questo [post](http://blogs.ugidotnet.org/topics/archive/2016/12/09/services-ui-composition-la-ricerca-maledetti-utenti.aspx) di Mauro in merito alla "ricerca". Nulla da eccepire su quanto ha scritto, ma mi piacerebbe vedere "una semplice search" anche sotto un altro punto di vista, soprattutto nel mondo del commercio elettronico, contesto dal quale Mauro ha preso spunto per i suoi post.  
Siamo davvero sicuri che la ricerca sia sempre e "solo" una ricerca? Cerco di dettagliare meglio la domanda: siamo proprio sicuri che la ricerca fatta da un utente percorra il lato "di lettura" dei nostri due mondi (command & query)?  
Secondo me **no** o almeno non sempre.

La ricerca di un utente porta con se una serie di informazioni semantiche molto importanti ed è fondamentale non perderle nascondendole dietro una, più o meno complessa, "query". Ma non solo: in determinati scenari l'esecuzione di una _ricerca_ non solo potrebbe, ma **deve** cambiare lo stato del sistema, entrando di diritto nella categoria dei _comandi_.  

Ok, prendiamo un bel respiro e cerchiamo di capire cosa intendo, per evitare di fare confusione: il fatto che l'utente _Mauro_ abbia ricercato un _portabanana_ in una certa _data_ sono informazioni importanti che cambiano per forza di cose lo stato del **contesto funzionale** della ricerca ed influenzano potenzialmente anche altri contesti.
Mantenere queste informazioni potrebbe aiutare il contesto del _marketing_ a "suggerire" agli utenti del sistema prodotti che potrebbero essere di loro interesse. Queste informazioni nel contesto di _sales_ potrebbero essere molto utili per gestire le politiche di pricing...e chi più ne ha più ne metta!

Ma facciamo un ulteriore passo.  

Non è solo la ricerca in sè ad essere importante, ma anche il risultato della ricerca stessa contiene implicitamente informazioni che non possiamo disperdere: per esempio una ricerca che non produce risultati o che produce un numero di risultati inferiori ad una certa soglia ha un altissimo valore di business. Perdere un'informazione del genere impedirebbe al business di sapere cosa gli utenti stanno cercando e non trovano perchè, per esempio, non disponibili a catalogo negando di fatto delle potenziali vendite.

Lo scenario della ricerca è uno di quei contesti in cui è molto semplice per la nostra mente prendere velocemente la tangente tecnologica, pensando a quale strumento "cool" del momento possiamo utilizzare per fare ricerche full-text o aggregazioni (stile facet) molto performanti, perdendo di vista gli scenari di business _hidden_ che si nascondono solo qualche passo più in là.  

I principi di Domain Driven Design, ma soprattutto l'approccio pratico e diretto di [Event Storming](http://ziobrando.blogspot.it/2013/11/introducing-event-storming.html) aiutano molto a far emergere scenari di business che molto spesso lo stesso business non sa di avere :)

Nel post di Mauro c'è una frase che ha dato origine a questi miei pensieri, che riporto testualmente:

> Ma cosa c’è di male a buttare tutto in un bel robo fatto con Elastic Search?   
> E la risposta sarebbe: nulla, non c’è nulla di sbagliato.  
> La cosa sbagliata è pensare che l’unica soluzione sia buttare tutto in un robo fatto con Elastic Search.

Parafrasando Mauro:

> La cosa sbagliata è pensare che la ricerca sia solo una "semplice" lettura

Molto spesso dietro una ricerca si nasconde un _bounded context_ inesplorato, che ha una dignità propria e che detiene un ricco contenuto informativo utlie anche al resto del sistema.

Come al solito le risposte valide in questo contesto sono due: "dipende" e "42" :)
