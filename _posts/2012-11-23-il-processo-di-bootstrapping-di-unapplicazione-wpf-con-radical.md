---
layout: post
title:  "Il processo di bootstrapping di un'applicazione WPF con Radical"
comments: true
categories: Technology
---


In questo post vedremo, alla stessa stregua di quanto fatto in un [post precedente](http://blog.codiceplastico.com/melkio/index.php/2012/11/16/il-processo-di-bootstrapping-di-unapplicazione-wpf-con-caliburn-micro), il dettaglio delle operazioni da eseguire per avere un&#8217;applicazione WPF up-and-running sfruttando Radical come toolkit di supporto.

Per prima cosa installiamo, tramite nuget, tutti i packages necessari al corretto funzionamento del toolkit. Il root-package è [Radical.Windows.Presentation.CastleWindsor](https://www.nuget.org/packages/Radical.Windows.Presentation.CastleWindsor)

![](http://melkio.codiceplastico.com/images/uploads/2012/11/Nuget1.png)

A differenza di Caliburn.Micro, il &#8220;dependencies-tree&#8221; è assolutamente più corposo. Radical fin dall&#8217;inizio introduce il concetto di IoC container, fornendo un&#8217;implementazione out-of-the-box basata su Castle.Windsor. Il partizionamento dei progetti e delle dipendenze però sembra (e verificheremo nei prossimi post che è effettivamente così) ben fatta, garantendo potenzialmente un alto grado di customizzabilità.

Radical, così come abbiamo già visto per Caliburn.Micro, basa il suo funzionamento di default su un motore di convenzioni che dovrebbe garantire una curva di setup dell&#8217;applicazione molto bassa, se sfruttato nella sua configurazione standard. Anche per Radical la coppia view+viewmodel è la base di partenza, con il pattern che abbiamo visto essere valido anche per Caliburn.Micro. Per fare in modo però che i componenti in gioco (view e viewmodel) siano correttamente registrati all&#8217;interno del container, devono appartenere ad un namespace che soddisfa il seguente pattern : &#8220;*.Presentation&#8221;.
Per soddisfare questo requisito, basta creare una folder &#8220;Presentation&#8221; all&#8217;interno del progetto che funga da aggregatore per View e ViewModel, come mostrato nell&#8217;immagine seguente:

![](http://melkio.codiceplastico.com/images/uploads/2012/11/Solution1.png)

Fatto questo, basta attivare il bootstrapper, nel code behind dell&#8217;applicazione, come mostra lo snippet seguente:

```

```

Quello che fa, dietro le quinte, il toolkit è quello di creare l&#8217;entry point dell&#8217;applicazione (la view indicata nel tipo generico), creare il relativo viewmodel, e agganciare quest&#8217;ultimo al DataContext della view stessa in modo da avere la garanzia che tutto sia correttamente setuppato prima dell&#8217;avvio dell&#8217;applicazione.

Pochi istanti e, anche con Radical, l&#8217;applicazione è &#8220;runnabile&#8221;.

![](http://melkio.codiceplastico.com/images/uploads/2012/11/ShellView1.png)

