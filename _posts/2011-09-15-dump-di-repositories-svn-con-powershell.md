---
layout: post
title:  "Dump di repositories svn con powershell"
categories: tech
tags: [powershell]
---



&#8230;mentre c&#8217;è chi sollazza in quel di Anaheim, con un nuovo tablet dotato di un windows 8 fiammante&#8230;


Ultimamente sto inserendo prepotentemente powershell nello stack tecnologico con cui ho a che fare, o meglio con cui voglio avere a che fare, nelle attività lavorative di tutti i giorni.

A &#8220;causa&#8221; di una ristrutturazione interna del [nostro](http://www.codiceplastico.com) sistema di gestione della codebase e del processo di build (di cui parleremo in un prossimo post) ho avuto la necessità di &#8220;dumpare&#8221; tutti i nostri repository svn e zipparli per permetterne una facile archiviazione. Perchè non farlo con powershell, mi sono detto&#8230;ecco come me la sono cavata:

```
$svnadmin = "C:\Program Files (x86)\VisualSVN Server\bin\svnadmin.exe"

Get-ChildItem "C:\Repositories" |
    Where-Object { $_.mode -match "d" } |
    ForEach-Object { &amp; $svnadmin dump $fullname |
        Out-File -Encoding ASCII "C:\temp\dump\$_.svndump" }

```

In queste poche righe di codice si capisce la potenza espressiva di powershell&#8230;si paga un po&#8217; in termini di verbosità, è innegabile, ma tant&#8217;è&#8230;non sono un po&#8217; di caratteri in più a spaventarci :-)

Un paio di commenti a margine:

- 
con l&#8217;operatore &#8220;|&#8221; concateniamo istruzioni successive: l&#8217;output della precedente viene &#8220;riversato&#8221; come input della successiva

- 
l&#8217;istruzione alla riga 4 permette di filtrare tutte e solo le directory risultanti dall&#8217;operazione precedente

- 
con l&#8217;operatore &#8220;&amp;&#8221; eseguiamo un comando (in questo caso svnadmin.exe)

- 
l&#8217;istruzione alla riga 6 fa il redirect dello standard output sul file indicato



Facile no?

La fase di &#8220;archiviazione&#8221; si è rivelata anche meno complicata e mostra esplicitamente l&#8217;integrazione tra powershell e ogni componente del .net framework. Ecco lo snippet:

```
[System.Reflection.Assembly]::LoadFrom("C:\temp\ICSharpCode.SharpZipLib.dll")

Get-ChildItem "C:\temp\dump" |
    ForEach-Object {
        $filename = "c:\temp\archive\$_.zip"
        $zip = [ICSharpCode.SharpZipLib.Zip.ZipFile]::Create($filename)
        $zip.BeginUpdate()
        $zip.Add($_.fullname)
        $zip.CommitUpdate()
        $zip.Close()
    }

```

