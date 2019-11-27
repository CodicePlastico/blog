---
layout: post
title:  "NuGet e la debug-experience"
comments: true
categories: Technology
---


Che cosa sia [NuGet](http://nuget.org/) è ormai noto a tutti, ma non tutte le sue potenzialità sono conosciute.

Una cosa di cui sento parlare troppo poco (eufemismo per dire mai) è la possibilità, con pochi semplici comandi, di &#8220;pushare&#8221;, oltre al tradizionale pacchetto di nuget, anche i simboli di debug delle dll corrispondenti, rendendo la debug-experience degli utenti del package di un livello superiore.

Vediamo i semplici passi da seguire:

Creiamo un progetto di tipo &#8216;class library&#8217; e chiamamolo StringUtils. Aggiungiamo un extension method per la classe string (giusto per avere del semplice codice da pacchettizzare con nuget)

```
public static class StringExtensionMethods
{
    public static String ToCamelCase(this String current)
    {
        if (String.IsNullOrEmpty(current)) return current;

    var result = new StringBuilder();
    for (var i=0; i<current.Length; i++)
    {
        if (i % 2 == 0)
            result.Append(current[i].ToString().ToUpper());
        else
        result.Append(current[i].ToString().ToLower());
    }
    return result.ToString();
    }
}

```

Passiamo ora alla console e creiamo per prima cosa i metadati da associare al pacchetto di nuget con il comando:

```
nuget spec

```

Se il precedente comando è eseguito nella stessa directory in cui è presente il file di progetto (*.csproj), nuget predispone i matadati, leggendo alcune informazioni dal file AssemblyInfo.cs del progetto stesso.
La precedente operazioni dovrebbe avere generato un file StringUtils.nuspec simile a questo (da completare manualmente in alcune sue voci):

```
<package>
  <metadata>
    <id>StringUtils</id>
    <version>1.4</version>
    <title>StringUtils</title>
    <authors>melkio</authors>
    <owners>CodicePlastico srl</owners>
    <requirelicenseacceptance>false</requirelicenseacceptance>
    <description>
        Semplice classe di utility per le stringhe
    </description>
    <releasenotes></releasenotes>
    <copyright>Copyright ©CodicePlastico srl  2012</copyright>
    <tags>Utils</tags>
  </metadata>
</package>

```

Perfetto! Ora siamo pronti a generare il package vero e proprio:

```
nuget pack StringUtils.csproj -Symbol

Attempting to build package from 'StringUtils.csproj'.
Packing files from '...\StringUtils\bin\Debug'.
Using 'StringUtils.nuspec' for metadata.
Successfully created package '...\StringUtils.1.4.nupkg'.

Attempting to build symbols package for 'StringUtils.csproj'.
Packing files from '...\StringUtils\bin\Debug'.
Using 'StringUtils.nuspec' for metadata.
Successfully created package '...\StringUtils.1.4.symbols.nupkg'.

```

Notiamo nel comando precedente due cose essenziali:
1) il package è creato a partire dal file di progetto (.csproj) e non da quello con i metadati (.nuspec)
2) è stata aggiunta l&#8217;opzione &#8216;Symbol&#8217;, che istruisce nuget alla creazione anche di un package con i simboli di debug

Se tutto è andato a buon fine dovremmo trovare due package: StringUtils.1.4.nupkg e StringUtils.1.4.symbols.nupkg

E&#8217; arrivato il momento di &#8220;pushare&#8221; i package sui rispettivi repository. Niente di più facile!
Per prima cosa, se ancora non l&#8217;abbiamo fatto, impostiamo l&#8217;apikey associata al nostro utente nuget, tramite il comando:

```
nuget setApiKey AAAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEE

The API Key 'AAAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEE' was saved 
for the NuGet gallery (https://www.nuget.org) and the symbol 
server (http://nuget.gw.symbolsource.org/Public/NuGet).

```

&#8230;e poi&#8230;&#8221;push&#8221;

```
nuget push StringUtils.1.4.nupkg

Pushing StringUtils 1.4 to the NuGet gallery (https://www.nuget.org)...
Your package was pushed.
Pushing StringUtils 1.4 to the symbol server (http://nuget.gw.symbolsource.org/Public/NuGet)...
Your package was pushed.

```

L&#8217;applicazione console di nuget si accorge della presenza di un symbols-package e, oltre all&#8217;upload del tradizionale package sul repository di nuget, si occupa dell&#8217;upload del package con i simboli sul repository pubblico di symbolsource.

Come possiamo notare, non abbiamo dovuto fare nulla per abilitare la scrittura sul repository dei simboli. Questo perché, tramite l&#8217;apikey impostata in precedenza, symbolsource determina l&#8217;ownership del package e ne conferma o meno l&#8217;upload.

NOTA: A causa di un problema con la nuova versione dell&#8217;applicazione console di nuget (versione 1.6) della gallery di NuGet potrebbero verificarsi problemi nella verifica della ownership del package, impedendo l&#8217;upload sul repository dei simboli, generando un errore simile al seguente:

&#8220;Failed to process request. &#8216;Failed to verify permissions for upload:  Project NuGet/StringUtils not found or inaccessible for Public/melkio. See http://www.symbolsource.org/Public/Home/Help for possible reasons. Fiddler may help diagnosing
this error if your client discards attached detailed information.&#8217;.
The remote server returned an error: (418) Failed to verify permissions for upload:  Project NuGet/StringUtils not found or inaccessible for Public/melkio. See
http://www.symbolsource.org/Public/Home/Help for possible reasons. Fiddler may help diagnosing this error if your client discards attached detailed information.&#8221;

Il problema è documentato [qui](https://github.com/NuGet/NuGetGallery/issues/341) e l&#8217;unico modo che, per ora, ho trovato per bypassare il problema è stato quello di scrivere sul [forum](http://groups.google.com/group/symbolsource) di [symbolsource](http://www.symbolsource.org/), richiedendo l&#8217;abilitazione manuale della ownership.

A questo punto, chi utilizza il nostro package, configurando opportunamente VisualStudio come descritto [qui](http://www.symbolsource.org/Public/Home/VisualStudio) avrà la possibilità di sfruttare anche i simboli di debug e, quindi, la possibilità di navigare nel nostro codice.

E allora, come direbbe il &#8220;mitico&#8221; Marzullo, la domanda sorge spontanea: perché se il &#8220;costo&#8221; di tale feature è praticamente nullo (fa tutto la console di nuget), non tutti i package presenti sul repository ufficiale di nuget, non hanno il loro corrispettivo in [symbolsource](http://www.symbolsource.org/) e rendono la mia (e la vostra) debug-experience molto complicata, se non impossibile?

