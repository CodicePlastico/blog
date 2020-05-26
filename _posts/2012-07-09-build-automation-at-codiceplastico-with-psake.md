---
layout: post
title:  "Build automation @codiceplastico with psake"
comments: true
categories: tech
---


Come bentornato da una settimana di pseudo-ferie, mi sono imposto di sistemare e soprattutto standardizzare il processo e i tool di build che [utilizziamo](http://www.codiceplastico.com). Fino ad ora, chi aveva l&#8217;ownership del progetto ne determinava, in fase di startup, anche il tool per la build automatica. Non che questo generasse grossi problemi, ma avere qualche script di build in ruby utilizzando rake, altri, su &#8220;vecchi progetti&#8221; in xml utilizzando nant, proprio non mi piaceva.

Era da un po&#8217; che avevo puntato [psake](https://github.com/psake/psake) e questa è stata l&#8217;occasione propizia per approfondirne un po&#8217; l&#8217;uso.

Facciamo le dovute presentazioni: come riporta anche l&#8217;home page del [wiki](https://github.com/psake/psake/wiki/What-Is-psake%3F) del progetto: &#8221;psake is a build automation tool written in PowerShell&#8220;&#8230;proprio quello che serve a me :-)
Psake non introduce nulla di sconvolgente, la semantica è la stessa di nant/rake (et simili): una serie di task da eseguire con una certa sequenza, con il grosso vantaggio di utilizzare un linguaggio di scripting che conosco abbastanza bene (PowerShell) e che ha tutta la potenza del .Net framework dalla sua parte.

Psake non è altro che un modulo PowerShell che contiene tutte le funzioni necessarie per eseguire i vari step. Per ottenerlo affidiamoci al solito fido [nuget](http://nuget.codeplex.com/).

```
nuget install psake

```

Ecco&#8230;abbiamo già tutto il necessario per poter utilizzare psake :-)

Come per tutti i tool di build automatica citati in precedenza, anche con psake abbiamo, ovviamente, bisogno di uno script file. Lo scheletro di un possibile script di build potrebbe essere il seguente:

```
properties { 
  $prop1 = "value 1"
  $prop2 = $par1 
  $execute_check = $true
}

include .\external.ps1

task default -depends LastTask

task CheckParameters -precondition { $execute_check -eq $true } {
  Assert ($prop2 -ne $null) "par1 should not be null"
}

task FirstTask -depends CheckParameters {
  Write-Host "First task in action with prop1 value equals to $prop1"
}

task LastTask -depends FirstTask {
  Write-Host "Last task in action with prop2 value equals to $prop2"
}

```

In questo script abbiamo (quasi) tutti gli scenari disponibili:

- 
dalla riga 1 alla 5 possiamo definire delle property che hanno lo stesso scope dello script e che sono quindi comuni a tutti i possibili task. Psake ha delle property standard (che vediamo tra un attimo) che sono valorizzate con dei valori di default, ovviamente, modificabili

- 
alla riga 3 viene utilizzato un parametro ($par1) che sarà impostato in fase di esecuzione dello script. Sia le properties sia i parameters sono per psake delle semplici hashtable, con l&#8217;unica differenza che i parameters vengono valutati, nell&#8217;ordine, prima delle properties e quindi possono essere referenziati e combinati da una property.

- 
alla riga 7, con l&#8217;istruzione &#8220;include&#8221; abbiamo la possibilità di includere, appunto, un file powershell esterno che contiene, per esempio, delle utility accessorie

- 
Alla riga 9 c&#8217;è l&#8217;entry point di default dello script: il task &#8220;default&#8221;. All&#8217;interno di uno script ci può essere un unico task denominato &#8220;default&#8221; che non deve contenere alcuna istruzione. Con la keyword &#8220;depends&#8221; identifichiamo quale altro task deve essere eseguito prima del task in oggetto.

- 
Alla riga 11, utilizzando la keyword &#8220;precondition&#8221;, stiamo identificando un task che verrà eseguito solo se la condizione proposta è verificata

- 
Alla riga 12, con la funzione &#8220;Assert&#8221; messa a disposizione dall&#8217;engine (contenuta nel modulo psake.psm1), abbiamo la possibilità di fare delle asserzioni vere e proprie che interrompono l&#8217;esecuzione dello script qualora risultassero non verificate.



Ora che lo script è pronto dobbiamo poterlo &#8220;runnare&#8221;&#8230;ma non c&#8217;è nulla di più semplice :-)
Aprendo la console powershell ecco le istruzioni da eseguire:

```
Import-Module .\psake.psm1
Invoke-psake .\test.ps1 -parameters @{'par1' = 'valore'}

```

- 
Alla riga 1 stiamo importando il modulo di psake, per poter usufruire di tutte le funzionalità dell&#8217; engine

- 
Alla riga 2 viene eseguito effettivamente lo script, passando il par1 con il valore indicato



Come detto prima, nel modulo psake.psm1, sono già inizializzate alcune proprietà di default. Tra queste c&#8217;è il nome dello script di build che psake cerca qualora non fosse diversamente specificato (default.ps1) e la versione di default del framework (la 4.0) con cui, eventualmente, compilare solution.
Oltre a queste ci sono altre proprietà, più o meno utili/importanti, che possono essere customizzate. Se si vuole fare in modo di cambiare in modo permanente il valore di default di tali property, un modo è quello di modificare il file  psake-config.ps1 che si trova nella stessa folder del file psake.psm1

Insomma, con pochissimo effort, abbiamo uno script engine molto versatile e che può sfruttare tutte le feature del .Net framework.
Per ora mi fermerei qui&#8230;poche semplici istruzioni per essere fin da subito operativi. Al prossimo giro vediamo lo script di build che ho prodotto per uno dei nostri progetti.

Buona build a tutti con powershell e psake :-)

