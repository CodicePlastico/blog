---
layout: post
title:  "DDD: chi fa da sè fa per tre (recap)"
comments: true
categories: tech
---


Con un po&#8217; di ritardo rispetto al desiderata, ecco la risposta ad alcune osservazioni/domande che sono state fatte al [post precedente](http://blog.codiceplastico.com/melkio/index.php/2012/06/18/ddd-chi-fa-da-se-fa-per-tre/):

Per semplicità il domain event che notificava l&#8217;avvenuto cambio di sede operativa riportava solo un subset di informazioni

```

```

Dato che un domain event deve essere il più autoconsistente possibile, ovviamente, deve contenere tutte le informazioni che identificano e determinano il cambio di stato dell&#8217;aggregate root che lo notifica. Volendo fare i &#8220;pignoli&#8221;, quindi, la corretta e completa implementazione di tale domain event potrebbe essere la presente:

```

```

In questo modo, l&#8217;handler (o i molteplici handler) deputati a gestire l&#8217;evento, hanno sia l&#8217;informazione semantica di ciò che è successo (&#8220;sede operativa modificata&#8221;) sia le informazioni relative al cambio di stato.

La seconda questione &#8220;aperta&#8221; dal post precedente è relativa all&#8217; &#8220;ordine delle operazioni&#8221;: quando un evento viene notificato l&#8217;aggregate è già stato persistito? Quindi, qualora dovessi averne bisogno, avrei la versione aggiornata oppure si corre il rischio di utilizzare una versione &#8220;vecchia&#8221;?
La risposta è un po&#8217; articolata, quindi partiamo dalla versione semplice: sì, assolutamente sì. Lo scopo dei domain event è quello di notificare un cambio di stato di un aggregate root, quindi il cambio di stato deve essere già avvenuto e, qualora necessario, persistito, altrimenti rischiamo di utilizzare una porzione di sistema non &#8220;consistente&#8221;.

Quando parliamo di cambio di stato di un aggregate root, parliamo del &#8220;domain model&#8221; vero e proprio, della parte che reagisce al &#8220;C&#8221; di CQRS. Questa parte è il cuore del nostro sistema e non possiamo correre il rischio che sia &#8220;eventualmente consistente&#8221;. Tutto diverso invece sull&#8217;altra sponda: la parte il lettura, &#8220;R&#8221; per gli amici. Lì sì possiamo permetterci, a seconda del contesto in cui siamo, di avere un delay nell&#8217;aggiornamento del dato che andiamo a leggere. Da questo versante non dipendono logiche di business, ma solo di visualizzazione.

Spingendosi un po&#8217; oltre, e abbracciando EventSourcing al 100% ci accorgeremmo che il problema in oggetto non si pone più in quanto lo snapshot dello stato dell&#8217;aggregate non ha più motivo di esistere e una volta &#8220;persistito&#8221; il domain event abbiamo la possibilità di ricostruire la nostra porzione di sistema al massimo dettaglio, garantendo quindi il massimo grado di consistenza possibile.

Dal mio punto di vista quindi gli handler devono essere sempre asincroni, perchè ciò permette al sistema di essere veramente &#8220;distribuito&#8221; e poco &#8220;accoppiato&#8221;. Ovviamente per poter sfruttare l&#8217;asincronicità degli handler bisogna garantire quanto sopra.

Per chi si approccia o si sta approcciando ad un sistema ad eventi, come faceva notare [GianMaria](http://blogs.ugidotnet.org/rgm/Default.aspx), è forte la sensazione di dispersione della logica di business tra un handler e l&#8217;altro. Ok, non preoccupatevi, quando avete questa percezione siete sulla strada giusta :-)
Ma bisogna introdurre un nuovo componente: la saga.

Ma per tutto questo, come fa il buon Ayende (e qualcuno riderà sotto i baffi)&#8230;vi rimando ai prossimi post :-)

