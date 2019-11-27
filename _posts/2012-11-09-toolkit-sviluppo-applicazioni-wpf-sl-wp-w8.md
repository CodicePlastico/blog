---
layout: post
title:  "Toolkit per lo sviluppo di applicazioni WPF/SL/WP/W8"
comments: true
categories: Technology
---


Chi ha sviluppato applicazioni mediamente complesse in WPF (e/o SL/WP/W8) nel 99% dei casi ha adottato il pattern MVVM, diventato ormai lo standard di fatto per l&#8217;organizzazione della parte di &#8220;presentation&#8221;. Tale pattern, se da una parte permette di cogliere tutte le potenzialità delle tecnologie in gioco, rischia, alla lunga, di diventare &#8220;molto prolisso&#8221; e di rischiare di portare &#8220;fuori controllo&#8221; i developer se non adeguatamente supportati da un toolkit.

Ad oggi, per quel che ne so (e quindi correggetemi se sbaglio), i toolkit presenti sul &#8220;mercato&#8221;, tra cui come dev possiamo attingere, sono essenzialmente quattro:

- 
[Prism](http://compositewpf.codeplex.com/)

- 
[MVVM-Light](http://mvvmlight.codeplex.com/)

- 
[Radical](http://radical.codeplex.com/)

- 
[Caliburn.Micro](http://caliburnmicro.codeplex.com/)



Il primo della lista, Prism, sviluppato dal team di P&amp;P di Microsoft è uno dei primi che [abbiamo](http://www.codiceplastico.com) cercato di adottare, fin dalle primissime edizioni (ormai anni fa), soprattutto per la gran mole di documentazione disponibile, purtroppo sempre con scarsi risultati. Nonostante le feature messe a disposizione siano notevoli (infrastruttura di base, UI composition, modularizzazione, solo per citare le più importanti) la complessità e la rigidità di tale framework ha sempre reso molto difficile la customizzazione di alcuni scenari: il toolkit non è pensato per permettere di &#8220;deviare&#8221; con semplicità dalle implementazioni messe a disposizione dal team. Ogni qual volta si è resa necessaria l&#8217;implementazione di una feature &#8220;fuori concorso&#8221; il rischio di non riuscire a centrare l&#8217;obbiettivo si è rivelato veramente molto alto. Per fare un semplice esempio, le difficoltà incontrate per &#8220;pluggare&#8221; un framework di IoC diverso da quello utilizzato di default (Unity/MEF) sono state veramente insostenibili, al punto da vincolare la scelta a MEF (che un container di IoC non è, ma questo è un altro discorso).

Questo aspetto enfatizza una delle caratteristiche da ricercare assolutamente per un&#8217;adozione serena di un toolkit/framework infrastrutturale: deve essere pensato per essere configurabile/customizzabile ed estendibile con estrema semplicità.

In seconda battuta, grazie anche al grande successo che ha ottenuto e sta tutt&#8217;ora ottenendo siamo passati a MVVM-Light. Il framework è ben fatto, ha alcune funzionalità di supporto (MessageBroker, ViewModelBase, EventToCommand per citarne alcune) molto utili, ma, a dispetto del grande successo ottenuto, una delle sue peculiarità (l&#8217;essere &#8220;light&#8221;) è anche un grosso limite se adottato in progetti di una certa complessità. E&#8217; vero, il nome è esplicativo: non vuole essere un framework &#8220;invadente&#8221;, ma fornire solo le feature indispensabili per un corretto impiego del pattern MVVM o poco più.
Ma questo implica la mancanza di una guida nella parte &#8220;infrastrutturale&#8221; di sviluppo dell&#8217;applicazione, obbligando il team a gestire anche alcuni aspetti che invece mi aspetterei di avere &#8220;out-of-the-box&#8221; da un toolkit che cura la parte di presentation.

Rimangono quindi gli ultimi due toolkit, Radical sviluppato dal mio amico [Mauro](http://milestone.topics.it/) e Caliburn.Micro. Entrambi erano stati scartati da una prima analisi per diversi motivi, ma soprattutto o per la scarsa documentazione (Radical) o per la numerosità di feature messa a disposizione, che nel caso di Prism si era rivelato essere un pesante side-effect.
Entrambi si sono invece rivelati, spulciando sia nel codice sorgente, sia nella (poca o tanta) documentazione che si trova in rete, ricchi di spunti interessanti, molto ben fatti e soprattutto completi, senza per questo essere vincolanti nelle scelte del team.

Nei prossimi post, cercheremo di evidenziare gli aspetti salienti di questi due framework (tenendo in considerazione, di tanto in tanto, anche MVVM-Light), quali funzionalità espongono,  le possibilità di customizzazione e la facilità di utilizzo. Non disdegneremo l&#8217;incursione nel codice sorgente, perchè ve lo assicuro, in entrambi ci sono delle &#8220;chicche&#8221; da non perdere.

