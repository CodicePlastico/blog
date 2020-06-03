---
layout: post
title:  "Questo database amico-nemico"
comments: true
categories: tech
tags: [database, ddd]
---


Sempre più spesso mi trovo a ripetere (e convincermi) che il database deve essere un servizio al servizio :-) dell&#8217;applicazione e non viceversa. In ottica DDD questa affermazione diventa ancora più forte: &#8220;il database è mio e lo gestisco io&#8221; (semi cit.)

Molto spesso però, questo modo di vedere le cose stride (eufemismo) con la forma mentis dei DBA e di chi deve gestire i dati. Ma non solo&#8230;il pensiero di non poter gestire il database ed i suoi dati sembra precludere qualsiasi forma di estensione ed apertura ad altre applicazioni all&#8217;utilizzo delle informazioni in nostro possesso. Sembra quasi che aprire, in sola lettura, le tabelle del nostro database ad altre applicazioni significhi avere un&#8217;architettura facilmente estendibile ed integrabile.

Bene&#8230;anzi male, molto male. Questo significa complicare ancora più le cose e rendere sempre meno rifattorizzabile il nostro database: adesso la modifica anche di  un solo campo di una tabella &#8220;condivisa&#8221; non impatta solo sulla nostra applicazione, ma anche su tutte quelle a cui abbiamo permesso di attingere direttamente ai nostri dati.
Un&#8217;operazione di questo genere significa non poter evolvere il database (dove per evolvere intendo anche correggere errori fatti in fase di sviluppo) e visto che il database è parte della nostra applicazione significa avere un debito tecnico costante che non potrà fare altro che aumentare nel corso dello sviluppo.

Se altre applicazioni hanno bisogno di &#8220;vedere&#8221; i miei dati, mi fanno la cortesia di chiedermeli (con gentilezza) ed, eventualmente, sarò ben lieto di metterglieli a disposizione senza l&#8217;obbligo di dovergli dire come e dove li ho &#8220;storati&#8221; e quindi senza l&#8217;obbligo di non poter cambiare più.

Queste false credenze mi ricordano tanto la sensazione di aver sviluppato un&#8217;applicazione distribuita semplicemente facendo &#8220;Add service reference&#8221; al nostro progetto da Visual Studio&#8230;ma questa è un&#8217;altra storia.

