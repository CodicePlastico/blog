---
layout: post
title:  "NuGet, le dll e il source control"
comments: true
categories: Technology
---


Fino a poco tempo fa, la struttura dei [nostri](http://www.codiceplastico.com) progetti, era quella &#8220;tradizionale&#8221;:

- 
source: folder in cui sono contenuti tutti i sorgenti della nostra soluzione

- 
libs: folder in cui sono contenute tutte le librerie di terze parti referenziate dai nostri progetti

- 
database: folder in cui sono contenuti tutti gli script per la generazione/aggiornamento del db

- 
docs: folder in cui è contenuta tutta la documentazione progettuale



Questa modo di organizzare la solution ci garantisce che, chiunque entri in corsa sul progetto, possa essere fin da subito operativo con un semplice checkout/update, senza bisogno di installare/configurare nulla di particolare.

Una cosa che non mi è mai piaciuta è quella di dover, per ogni progetto, dover committare tutte le volte, tutte le dll necessarie, duplicando, progetto dopo progetto, le stesse dll sul repository.

Ora però, con [NuGet](http://nuget.org/), abbiamo a disposizione un repository globale (e perchè no, anche locale e privato) per tutte le librerie di cui abbiamo bisogno, perfettamente integrato con Visual Studio e facilmente utilizzabile. Perchè non sfruttarlo?

Ma facciamo un passo indietro e ricapitoliamo velocemente come NuGet funziona: ogni volta che viene referenziata una nuova libreria in uno dei progetti di una solution, la dll e le relative dipendenze vengono scaricate in una folder (packages) creata a livello di solution e un nuovo file (packages.config) viene creato a livello di progetto. Quest&#8217;ultimo file è il &#8220;repository&#8221; di tutte le dll referenziate dal progetto.
Qualora un nuovo progetto della solution referenzi una dll precedentemente installata, il file packages.config del progetto viene aggiornato e viene aggiunto un riferimento alla dll già scaricata e presente nella folder packages.

Detto questo, non tutti sanno che tra i vari modi con cui possiamo interagire con NuGet, oltre che comodamente da VisualStudio, c&#8217;è anche la possibilità di sfruttarlo da riga di comando.

Si sta chiudendo il cerchio? La nebbia si sta diradando? :-)

La console a riga di comando ci da la possibilità, oltre all&#8217;utilizzo &#8220;tradizionale&#8221; (installare un package alla volta), di installare una serie di package utilizzando il file packages.config proprio come un vero repository del &#8220;necessario&#8221;.
Tramite l&#8217;istruzione:

```
nuget install packages.config -OutputDirectory "..\packages"

```

la versione console di NuGet legge il contenuto del file packages.config e scarica, solo il necessario (ovvero ciò che non è già presente), nella folder &#8220;..\packages&#8221;. Ottimo direi, no?

Ecco quindi la soluzione che abbiamo adottato:

- 
solo e soltanto NuGet per la gestione delle reference

- 
i file packages.config di ogni progetto vengono committati

- 
è stato inserito, per ogni progetto, un prebuild event che lancia il seguente comando:

nuget install packages.config -o &#8220;..\packages&#8221;

- 
è stata finalmente droppata la folder &#8220;libs&#8221; :-)



&#8230;che non vi venga in mente di committare la folder packages che viene creata!!!!

