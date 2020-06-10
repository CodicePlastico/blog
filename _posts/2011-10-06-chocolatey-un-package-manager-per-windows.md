---
layout: post
title:  "Chocolatey: un package manager per windows"
comments: true
categories: tech
tags: [tech, .net]
---


Dopo diversi mesi, e diverse migliaia (3134 per l&#8217;esattezza, al momento) di [packages](http://nuget.org/List/Packages) uploadati&#8230;[NuGet](http://nuget.codeplex.com/) non è più una novità, ma una piacevole realtà.

Sempre più utile ed integrato anche nel [nostro](http://www.codiceplastico.com) processo di build (ne parleremo presto) è diventato uno strumento indispensabile, almeno per me, nella &#8220;cassetta degli attrezzi&#8221; del bravo dev.

Sulla scia di NuGet, è nato (da un po&#8217;) [Chocolatey](http://chocolatey.org/) che ha come obbiettivo quello di diventare IL package manager (alla &#8220;apt-get&#8221;) di riferimento per il mondo Windows. E per package manager intendo qualsiasi tipologia di package e non solo quelli dev-oriented.

Come fare ad installarlo? Con NuGet ovviamente, digitando dal prompt:

```
nuget install chocolatey

```

in modo da scaricare tutto il necessario, e per completare l&#8217;installazione, da una console powershell, puntando alla cartella tools scaricata al passo precedente:

```
ChocolateyInstall.ps1

```

Attenzione: per poter eseguire lo script powershell precedente bisogna impostare l&#8217;[execution-policy](http://technet.microsoft.com/en-us/library/ee176961.aspx) in modo che siano &#8220;runnabili&#8221; script remoti.

L&#8217;installazione di chocolatey imposta nelle variabili di sistema la folder di installazione di default (c:\Nuget\bin) in modo che siano sempre accessibili i comandi del package manager e l&#8217;accesso a tutti i package installati. Qualora si decidesse di installare il tutto in una folder diversa da quella di default, bisogna impostare, prima dell&#8217;installazione, una nuova variabile di sistema con chiave &#8220;ChocolateyInstall&#8221; e valore il path di destinazione.

A questo punto possiamo, per esempio, installare node.js in modo molto semplice:

```
chocolatey install nodejs

```

![](http://melkio.codiceplastico.com/images/uploads/2011/10/NodeJs.png)

&#8230;ed eseguirlo dal prompt&#8230;

![](http://melkio.codiceplastico.com/images/uploads/2011/10/RunNode.png)

Buon chocolatey a tutti!!!

