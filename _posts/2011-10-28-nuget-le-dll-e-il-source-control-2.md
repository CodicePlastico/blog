---
layout: post
title:  "NuGet, le dll e il source control (2)"
comments: true
categories: tech

---


Nel [post](http://blog.codiceplastico.com/melkio/index.php/2011/10/19/nuget-le-dll-e-il-source-control/) precedente, abbiamo visto come sfruttare [NuGet](http://nuget.org) per evitare di committare tutti i packages (leggi dll) sul source control e integrarne il download da uno o più repository nel processo di build della nostra solution.
Per far funzionare tutto il processo, anche se non menzionato nel post, è necessario che NuGet.exe sia registrato nel path, in modo che sia facilmente accessibile dai nostri pre-build events.

Chiacchierando con [andrea](http://blogs.ugidotnet.org/pape), una soluzione alternativa potrebbe essere quella di creare una cartella &#8220;tools&#8221; per ogni solution, nella quale inserire l&#8217;eseguibile di nuget e far puntare a quell&#8217;eseguibile i pre-build events. In questo modo l&#8217;ingresso di un nuovo componente nel team o la messa in opera di un build server ha un impatto ancora minore: nessun presetting da soddisfare, un update dal source control, una build et voilà&#8230;tutto funziona.

Se il processo così descritto ci soddisfa, c&#8217;è una soluzione pronta all&#8217;uso, ancora più integrata e a minore impatto (grazie a [roberto](http://blogs.ugidotnet.org/robymes) per la segnalazione): i [NuGetPowerTools](http://nuget.org/List/Packages/NuGetPowerTools).

Questo package è composto da soli script di powershell che automatizzano le nostre esigenze&#8230;installiamolo con NuGet.

![](http://melkio.codiceplastico.com/images/uploads/2011/10/Install-NuGetPowerTools.png)

Il package arricchisce la nostra console PowerShell con alcuni nuovi comandi. Quello che fa al caso nostro è il comando

```
Enable-PackageRestore

```

Viene quindi creata, a livello di solution, una folder .nuget che contiene l&#8217;eseguibile NuGet.exe e un file nuget.targets contenente l&#8217;integrazione per/con msbuild.

A questo punto, ricordandosi di committare la folder .nuget, ad ogni rebuild della solution, ogni package necessario alla compilazione viene, ovviamente se necessario, downloadato e installato nel/i progetto/i.

Ottima soluzione, niente pre-build events e pronta all&#8217;uso.

Vi dirò però che dover committare la folder .nuget e l&#8217;eseguibile NuGet.exe non mi soddisfa appieno: risolve un problema (commit delle dll) per &#8220;crearne&#8221; un altro (commit dell&#8217;eseguibile). Ricordiamoci infatti che l&#8217;eseguibile di NuGet è passibile di aggiornamenti che, all&#8217;occorrenza, verrebbero commitatti integrando la &#8220;history&#8221; del file sul source control.

Buon nugetting a tutti!!!

