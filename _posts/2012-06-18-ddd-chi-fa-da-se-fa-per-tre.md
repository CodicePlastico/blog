---
layout: post
title:  "DDD: chi fa da sè fa per tre"
comments: true
categories: Technology
---


&#8230;riprendiamo l&#8217;esempio del [post precedente](http://blog.codiceplastico.com/melkio/index.php/2012/06/05/laggregate-root-la-fonte-della-verita/):

&#8220;&#8230;quando un utente (fisico o virtuale) del sistema modifica la sede operativa di un'azienda deve essere verificata la corretta associazione alla filiale della banca di riferimento secondo algoritmi, potenzialmente modificabili dinamicamente. Fino a che la nuova banca non ha confermato l'autorizzazione ad operare, l'azienda deve entrare in un limbo che non permetta operatività sull'azienda&#8230;&#8221;

Ok&#8230;per ora le logiche su cui scegliere la potenziale nuova banca, sono le seguenti:

- 
in base al valore del fatturato dell&#8217;ultimo bilancio

- 
in base al CAP



Iniziamo con una prima implementazione e proviamo a capirne pro e contro

```

```

Diciamo che ora le cose iniziano a piacermi sicuramente di più :-)
La logica di business è autocontenuta e l&#8217;aggregate-root fornisce la consistenza di cui si deve fare carico: in ogni momento, se ho fatto le cose per bene, questo aggregate sarà sempre in uno stato consistente. Il servizio di ricerca della nuova banca è iniettato dall&#8217;esterno, quindi ci dà la versatilità di poterne cambiare l&#8217;implementazione se dovessero cambiare le regole.

Proviamo ad alzare un po&#8217; l&#8217;asticella per vedere se possiamo fare di meglio. Una cosa di questa implementazione che proprio non mi piace è il fatto che il servizio che propone la nuova banca, esponendo direttamente i parametri che gli servono per i suoi calcoli, ci rende poco versatili: una semplice aggiunta/modifica nelle regole che dettano la proposta di una nuova banca, ci vincola a rifattorizzare il servizio e conseguentemente anche l&#8217;aggregate che da lui dipende.

La prima soluzione che mi viene in mente è quella di modificare la firma del servizio nel modo seguente:

```

```

In questo modo l&#8217;aggregate che utilizza il servizio non deve conoscere quali parametri servono al servizio stesso per poter determinare la nuova banca e non sarà quindi &#8220;toccato&#8221; da eventuali modifiche alla logica di assegnazione. Con questo refactoring è stata spostata un po&#8217; di logica nel servizio, che oltre al calcolo effettivo dovrà, direttamente o collaborando con altri servizi, recuperare i parametri necessari per il calcolo. Dato che lui sa quali gli servono, non lo vedo come un grosso problema.

Se proprio vogliamo fare i pignoli (e io direi di farlo&#8230;siamo qui per questo), l&#8217;implementazione precedente soffre di due potenziali problemi:

- 
l&#8217;aggregate root ha una forte dipendenza verso il servizio. Ciò implica che se il servizio, di cui l&#8217;aggregate non conosce l&#8217;implementazione concreta, non è disponibile, il processo di cambio della sede operativa potrebbe fallire

- 
il servizio potrebbe avere una forte dipendenza sui servizi accessori che gli permettono di recuperare i parametri necessari per il calcolo. Idem come sopra: se uno dei servizi non è &#8220;reperibile&#8221;, tutto si ferma



No bello!!! :(
(Anche) Qui entrano in gioco i domain events:

```

```

In questo modo abbiamo esplicitato dei cambiamenti di stato importanti che l&#8217;aggregate-root dell&#8217;azienda può e deve notificare al resto dell&#8217;ecosistema. Ma a questo punto, abbiamo tutte le informazioni per rendere autoconsistente e senza dipendenze da fattori esterni il sistema di ricerca e proposta della nuova banca.

```

```

Ma non solo: qualora dovessero entrare in gioco nuove regole per la determinazione della nuova filiale da proporre, il nostro component non dovrà far altro che stare in ascolto rispetto al nuovo domain event, o eventualmente &#8220;riallinearsi&#8221; qualora quel domain event fosse già in circolo nel nostro sistema.
Anche l&#8217;aggregate-root trova giovamento da questo &#8220;refactoring&#8221;, perchè diventa più snello e con meno (potenzialmente) fragili dipendenze.

```

```

Come abbiamo detto agli scorsi [communitydays](http://www.communitydays.it/events/communitydays-2012/arch01/): &#8220;think different&#8221;. E&#8217; questo il punto di partenza per scrollarci di dosso la solita, noiosa e poco versatile implementazione &#8220;tradizionale&#8221; dei domain model (anemici).

Alla prossima ;-)

