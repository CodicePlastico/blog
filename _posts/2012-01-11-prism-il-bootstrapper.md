---
layout: post
title:  "Prism: il bootstrapper"
comments: true
categories: tech
tags: [wpf, mvvm, .net]
---


Il punto di ingresso di ogni applicazione che si basa su [Prism](http://compositewpf.codeplex.com/) è il bootstrapper.

Il bootstrapper deve contenere tutto quel codice di setup che permetterà alla nostra applicazione di funzionare. Questo è l&#8217;unico punto della nostra applicazione in cui mi concedo di mettere un po&#8217; di &#8220;sporcizia&#8221;: non pongo molta attenzione alla pulizia del codice in questo contesto. Ma è proprio questo contesto che mi permette di gestire, successivamente, in modo &#8220;pulito&#8221; la parte infrastrutturale della mia applicazione: logging, container di IoC&#8230;

Prism utilizza nativamente [Unity](http://unity.codeplex.com/) (o [MEF](http://mef.codeplex.com/)) come container di IoC. Qualora si decida di non utilizzare altro tipo di container, metà dell&#8217;opera è già &#8220;archiviata&#8221;. E quindi partiamo così, scegliendo Unity :-)

Un ottimo punto di partenza è, quindi, quello di sfruttare la classe [UnityBootstrapper](http://msdn.microsoft.com/en-us/library/microsoft.practices.composite.unityextensions.unitybootstrapper.aspx) e customizzare solo gli aspetti che &#8220;deviano&#8221; dal tradizionale. Creiamoci quindi il nostro bootstrapper, andando a ridefinire solo lo stretto indispensabile:

```
public class LittleJohnBootstrapper : UnityBootstrapper     
{
    protected override DependencyObject CreateShell()
    {
        return new Shell();
    }

    protected override void InitializeShell()
    {
        //base.InitializeShell();

        Application.Current.MainWindow = (Window) Shell;
        Application.Current.MainWindow.ShowDialog();
     }
}

```

&#8230;e inizializzando la nostra applicazione affinché utilizzi il bootstrapper per &#8220;setuppare&#8221; l&#8217;infrastruttura su cui si baserà il resto:

```
public partial class App     
{
    protected override void OnStartup(StartupEventArgs e)
    {
        base.OnStartup(e);

        var bootstrapper = new LittleJohnBootstrapper();
        bootstrapper.Run();
     }
}

```

Il setup minimale prevede di istruire il nostro bootstrapper su come creare ed inizializzare la shell, ovvero l&#8217;involucro della nostra applicazione. Fatto questo, il resto del setup è già compreso nella classe UnityBootstrapper.

Andiamo,però, a sbirciare il dietro le quinte di questa classe, giusto per capire cosa &#8220;combina&#8221;, nel caso volessimo customizzarla un po&#8217; o prevederne una basata, per esempio, su un container differente.
Il punto che da l&#8217;avvio a tutta la procedura di setup del bootstrapper e, quindi, dell&#8217;applicazione stessa è il metodo Run. Nell&#8217;ordine vengono eseguite le seguenti operazioni:

- 
Creazione del logger: di default prism utilizza un [TextLogger](http://msdn.microsoft.com/en-us/library/gg431439(v=PandP.38).aspx) per eseguire le operazioni di logging. Tutte le informazioni vengono &#8220;streammate&#8221; di default sulla console.

- 
Creazione e configurazione del module catalog: contiene le informazioni relative ai moduli che sono utilizzabili dall&#8217;applicazione. Prism contiene diverse implementazioni che permettono diverse sorgenti per il caricamento dei moduli (le vedremo in seguito)

- 
Creazione e configurazione del container: il container viene configurato con tutti i servizi trasversali all&#8217;applicazione (logger, module catalog, module initializer, module manager, region manager, event aggregator e altro)

- 
Creazione e configurazione della shell (invocando i metodi che abbiamo ridefinito nel nostro bootstrapper, visti in precedenza)

- 
Inizializzazione degli eventuali moduli dell&#8217;applicazione



Una cosa molto importante, che non dobbiamo dimenticarci di eseguire qualora decidessimo di crearci un nostro bootstrapper da zero (non utilizzando UnityBoostrapper, ma partendo direttamente da Bootstrapper), è quella di associare la nostra Shell al RegionManager di default, garantendoci tutte le inizializzazioni necessarie per sfruttare il meccanismo, che vedremo in seguito, dell&#8217;iniezione delle viste nelle regioni.

Il codice incriminato è molto semplice, ma indispensabile. Eccolo, copiato &#8220;as-is&#8221; dalla classe UnityBootstrapper:

```
this.Shell = this.CreateShell();      
if (this.Shell != null)
{
    // tolto il log per evitare inutile rumore
    RegionManager.SetRegionManager(
        this.Shell, 
        UnityContainerExtensions.Resolve<iregionmanager>(
            this.Container, 
            new ResolverOverride[0]));
    // tolto il log per evitare inutile rumore        
    RegionManager.UpdateRegions();

    // tolto il log per evitare inutile rumore        
    this.InitializeShell();
}

```

Be, per ora un po&#8217; di carne al fuoco ne abbiamo messa, ma&#8230;siamo solo agli inizi.

