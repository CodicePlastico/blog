---
layout: post
title:  "Il processo di bootstrapping di un'applicazione WPF con Caliburn.Micro"
comments: true
categories: tech
---


Nel [post precedente](http://blog.codiceplastico.com/melkio/index.php/2012/11/09/toolkit-sviluppo-applicazioni-wpf-sl-wp-w8/) abbiamo elencato una serie di framework/toolkit utili per il supporto allo sviluppo di applicazioni WPF/SL/WP/W8. Tutti questi framework, tranne MVVM-Light se non sbaglio, hanno il concetto di bootstrapping dell&#8217;applicazione, ovvero di setup, configurazione e avvio.

Questa funzionalità è tanto importante, quanto critica: maggiori sono le facilities che il toolkit ci mette a disposizione, molto meno codice di plumbing siamo costretti a scrivere. Questo però è uno dei primi punti che possono darci feedback immediati su quanto bene possa essere scritto un toolkit. Il framework scelto deve essere a supporto e non vincolare in scelte/implementazioni. Già da questo, come ho avuto modo di scrivere già in passato ([qui](http://blogs.ugidotnet.org/AMelchiori/archive/2008/09.aspx)), Prism se ne esce con un &#8220;rating&#8221; non del tutto sufficiente (visti i tentativi falliti riportati [qui](http://blog.codiceplastico.com/melkio/index.php/2012/01/09/prism-introduzione/) e [qui](http://blog.codiceplastico.com/melkio/index.php/2012/01/11/prism-il-bootstrapper/))

In questo post, vedremo come Caliburn.Micro approccia il problema.

Per prima cosa installiamo, tramite nuget, il package &#8220;Caliburn.Micro&#8221;

![](http://melkio.codiceplastico.com/images/uploads/2012/11/Nuget.png)

Creiamo ora una coppia &#8220;view&#8221; - &#8220;viewmodel&#8221;, ponendo particolare attenzione al fatto che i nomi devono, se vogliamo sfruttare out-of-the-box il sistema di convenzioni messo a disposizione dal toolkit, seguire il pattern &#8220;XxxView&#8221; - &#8220;XxxViewModel&#8221;. Nel nostro caso ShellView e ShellViewModel, come mostrato nell&#8217;immagine seguente:

![](http://melkio.codiceplastico.com/images/uploads/2012/11/Solution.png)

Caliburn.Micro, così come Radical, sfrutta pesanetemente il paradigma &#8220;convention over configuration&#8221; per semplificare drasticamente la vita allo sviluppatore, fornendo, praticamente a costo zero quasi tutta l&#8217;infrastruttura per fare wire-up dell&#8217;applicazione. Questa feature, è una delle discriminanti che ci hanno fatto preferire questi toolkit agli altri. Il livello di customizzabilità delle convenzioni, che valuteremo nei prossimi post per entrambi i toolkit, è uno dei parametri che ci permetteranno di valutare la bontà (o meno) dei framework in oggetto.

Tornando a noi, manca vermante poco, per poter avviare, finalmente, la nostra applicazione: il bootstrapper. Con il seguente snippet di codice e la customizzazione dello xaml dell&#8217;applicazione abbiamo soddisfatto i requisiti minimi per poter integrarci e iniziare a sfruttare le potenzialità di Caliburn.Micro

```

```

```

```

Premiamo F5, et-voilà l&#8217;applicazione magicamente parte senza aver fatto o configurato nulla di particolare.

![](http://melkio.codiceplastico.com/images/uploads/2012/11/ShellView.png)

Nel prossimo post della serie vedremo se e come approcciare lo stesso problema con Radical

