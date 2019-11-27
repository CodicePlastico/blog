---
layout: post
title:  "DDD: i behavior prima di tutto"
comments: true
categories: Technology
---


Molto spesso, come [già detto](http://blog.codiceplastico.com/melkio/index.php/2012/04/11/ddd-sogno-o-realta/), ci sembra di applicare DDD, ci autoconvinciamo della cosa (anche perché ultimamente se non fai DDD, non sei nessuno :-) ), ma siamo ben lontani dall&#8217;applicarlo.

Il primo, vero, campanello d&#8217;allarme, se di allarme vogliamo parlare, lo possiamo percepire da come è fatto il nostro dominio: se non abbiamo un [domain model](http://martinfowler.com/eaaCatalog/domainModel.html), non stiamo sicuramente facendo DDD. Avere un domain model, non significa che stiamo facendo DDD.
Riporto la definizione originale, a scanso di equivoci, di cosa un domain model dovrebbe essere:

&#8220;An object model of the domain that incorporates both behavior and data. [Martin Fowler]&#8221;

Ragazzi non ci si scappa: quel both è la parte più importante della definizione. E dato che sul data non ci piove (e non manca mai), è la parte behavior che fa la differenza, che è l&#8217;ago della bilancia che ci permette di asserire con un po&#8217; più di certezza di aver intrapreso la strada giusta (ovviamente se la strada che vogliamo intraprendere è quella del DDD)

Dire: &#8220;ho un domain model anemico&#8221; è una frase senza senso. Un domain model senza comportamento, non è un domain model. Punto.
Un object model, come quello dell&#8217;immagine seguente, dove la parte data la fa da padrone, nel senso che di behavior non se ne vedono, non è DDD-style e non ci permette di gestire la complessità del business che dobbiamo modellare. Se la complessità non c&#8217;è, siamo a posto, ma se c&#8217;è logica, più o meno complessa, potrebbe essere l&#8217;inizio della fine.

![](http://melkio.codiceplastico.com/images/uploads/2012/04/ClassDiagram-189x300.png)
Se poi pensiamo di delegare la consistenza di parti del nostro modello a layer differenti (per esempio il service layer) rispetto a quello del domain model, stiamo rischiando (è molto più di un rischio) di &#8220;rompere&#8221; uno dei principi caratterizzanti che guidano lo sviluppo in DDD: la definizione degli aggregati.

Se vogliamo applicare DDD, tutto parte da lì, dalla corretta definizione degli aggregati e dall&#8217;esposizione di behavior dall&#8217;aggregato.
Dato che questo è uno dei principi basilari, ma anche uno di quelli più difficili da &#8220;implementare&#8221;, quello che solitamente mi trovo a far fare a chi mi presenta un object model completamente anemico è: &#8220;togliamo tutte le proprietà pubbliche ed esponiamo funzionalità&#8221;&#8230;poi chiamo un medico, perché solitamente metà del team è svenuto :-)

Questa &#8220;prova di coraggio&#8221; tipicamente si scontra con il modus-operandi di gran parte dei team con cui mi trovo ad interagire. Indipendentemente dalla propensione al massiccio refactoring, gli scogli da superare sono comunque parecchi e toccano vari aspetti della nostra applicazione: la parte di persistenza, gli unit test, i tool da utilizzare&#8230;

Insomma c&#8217;è tanta carne al fuoco, nelle prossime puntate vedremo di dettagliare un po&#8217; meglio e di entrare un po&#8217; più nello specifico&#8230;se poi non vi ho annoiato troppo, rinnovo l&#8217;invito per il [primo meeting](http://guisa1.eventbrite.com/) organizzato da [guisa](http://www.guisa.org/): lì ne vedremo veramente delle belle.

Accorrete numerosi ;-)

