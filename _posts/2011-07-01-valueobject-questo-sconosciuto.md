---
layout: post
title:  "ValueObject: questo sconosciuto"
comments: true
categories: tech
tags: [value object]
---


Uno dei concetti apparentemente più semplici del libro di Evans è quello di value object. Riporto fedelmente la sua definizione per non incorrere in errori e/o omissioni:

&#8220;&#8230;there are cases when we need to contain some attributes of a domain element. We are not interested in which object it is, but  what attributes it has. An object that is used to describe certain  aspects of a domain, and which does not have identity, is named Value Object&#8230;&#8221;

Ogni value object porta (o almeno dovrebbe) con se alcune peculiarità che lo caratterizzano come il fatto di essere immutable, &#8220;sharabile&#8221; tra varie entity e non avere alcun concetto di identità.
Queste caratteristiche si traducono in esplicite richieste implementative che vanno assolutamente soddisfatte:

- 
ogni value object viene creato inizializzandone i suoi attributi e per tutto il suo life-cycle non deve avere la possibilità di modificare il suo stato interno

- 
per mia scelta personale, se possibile, evito di esporre lo stato interno di un value object. Se sono costretto a farlo (sotto tortura :-) ) lo faccio comunque solo in lettura

- 
se si vuole cambiare lo stato interno di un value object se ne crea uno nuovo

- 
non avendo intrinsecamente un concetto di identità deve ridefinire il modo in cui è possibile valutarne l&#8217;uguaglianza con un altro value object dello stesso tipo



Individuare correttamente i value object che decorano il nostro dominio non è un&#8217;attività semplice e soprattutto è un&#8217;attività spesso trascurata/tralasciata. Solo dopo un&#8217;attenta (l&#8217;ennesima) rilettura di alcune parti del libro di Evans ho colto l&#8217;importanza che viene data a questo particolare aspetto del nostro dominio.

Con poche accortezze, il nostro anemico object model acquisisce sostanza e ogni componente del puzzle si incastra perfettamente al suo posto.

