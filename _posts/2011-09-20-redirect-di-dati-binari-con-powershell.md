---
layout: post
title:  "Redirect di dati binari con powershell"
comments: true
categories: Technology
---


In questo [post](http://blog.codiceplastico.com/melkio/index.php/2011/09/15/dump-di-repositories-svn-con-powershell/) abbiamo visto come è stato possibile, con powershell, eseguire il dump dei [nostri](http://www.codiceplastico.com) repositories svn e zipparli per una più facile archiviazione. Non è però tutto oro quello che luccica.

Eseguendo il dump di un repository e tentando di reimportarlo sul nuovo server, la procedura veniva sempre interrota con la segnalazione di &#8220;formato errato&#8221;, come se il dump eseguito fosse corrotto o non corretto. Provando con il command prompt tutto invece filava liscio come l&#8217;olio.

Buttando l&#8217;occhio sui dump che venivano generati con le due modalità mi sono però accorto di una differenza macroscopica: la dimensione del dump ottenuto con powershell era quasi il doppio di quello ottenuto attraverso il command prompt &#8220;tradizionale&#8221;&#8230;no bello!!!

A naso ho circoscritto il problema al solo redirect verso lo stdout di contenuto binario, perchè quando si tratta di testo PowerShell fa degnamente il suo sporco lavoro. Ho provato quindi a &#8220;travasare&#8221; il contenuto di un&#8217;immagine in un&#8217;altra:

```
Get-Content .\image.png |
    Out-File .\image-postprocess.png

```

e analizzato il risultato

```
ls image*.png |
    select name, length |
    ft -AutoSize

Name                    Length
----                    ------
image.png                98869
image-postprocess.png   199184

```

Andando più nel dettaglio:

```
(Get-Content .\image-postprocess.png).GeType() |
    select name

Name
----
Object[]




Get-Content .\image-postprocess-.png |
    %{ $_.GetType() } |
    group name |
    select count, name |
    ft -AutoSize




Count Name
----- ----
  327 String

```

Ma come? Allora powershell &#8220;trasforma&#8221; l&#8217;output binario in un array di stringhe convertendo il tutto in Unicode. Ecco il perchè della dimensione quasi doppia :-(

L&#8217;unica soluzione che ho trovato, a dir la verità grazie al solito san google, è stata quella di utilizzare il seguente script per evitare che powershell si &#8220;intrometta&#8221; nello stream decodificando i caratteri:

```
Function Get-ProcessOutputAsBinary([string] $processname, [string] $arguments)

$processStartInfo = New-Object System.Diagnostics.ProcessStartInfo
$processStartInfo.FileName = $processname
$processStartInfo.WorkingDirectory = (Get-Location).Path
if($arguments) { $processStartInfo.Arguments = $arguments }
$processStartInfo.UseShellExecute = $false
$processStartInfo.RedirectStandardOutput = $true 

$process = [System.Diagnostics.Process]::Start($processStartInfo)
$process.StandardOutput.ReadToEnd()
$process.WaitForExit()

```

&#8230;facendo diventare il nostro script per eseguire il dump dei repository un po&#8217; più complesso:

```
$svnadmin = "C:\Program Files (x86)\VisualSVN Server\bin\svnadmin.exe"

Function Get-ProcessOutputAsBinary([string] $processname, [string] $arguments)
{
    $processStartInfo = New-Object System.Diagnostics.ProcessStartInfo
    $processStartInfo.FileName = $processname
    $processStartInfo.WorkingDirectory = (Get-Location).Path
    if($arguments) { $processStartInfo.Arguments = $arguments }
    $processStartInfo.UseShellExecute = $false
    $processStartInfo.RedirectStandardOutput = $true 

    $process = [System.Diagnostics.Process]::Start($processStartInfo)
    $process.StandardOutput.ReadToEnd()
    $process.WaitForExit()
}

Get-ChildItem "C:\Repositories" |
    Where-Object { $_.mode -match "d" } |
    ForEach-Object {
        $fullname = $_.fullname
        Get-ProcessOutputAsBinary $svnadmin "dump $fullname"  |
            Out-File -Encoding OEM "C:\temp\dump\$_.svndump" }

```

