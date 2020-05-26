---
layout: post
title:  "Blob storage: nuove API"
categories: tech
---


Questo post è parte di una serie su windows azure. Ecco il [link](http://blog.codiceplastico.com/melkio/index.php/2012/10/15/azure-intro/) per la lista di tutti i post precedenti.

__Piccolo inframezzo nella serie sul Windows Azure e nello specifico del Blob Storage, per avvisarvi che pochi giorni fa è stata rilasciata la versione 2.0 della Windows Storage Client Library. Come per le altre versioni il codice sorgente è disponibile su [GitHub](https://github.com/WindowsAzure/azure-sdk-for-net) e la library è &#8220;agganciabile&#8221; anche tramite Nuget.

La nuova versione è ricca di novità: oltre ad una rivisitazione ed un &#8220;improving&#8221; delle API della libreria per il .Net framework è stata rilasciata una nuova versione della libreria (in CTP) ottimizzata per lo sviluppo delle applicazioni sia per Windows RT (ARM) sia per Windows8. La library ora supporta anche il .Net Client Profile e, in modo naturale, sia la programmazione sincrona (che prima era &#8220;forzata&#8221;) sia quella asincrona.

Tutte le altre novità le trovate in questo post: [Introducing Windows Azure Storage Client Library 2.0 for .NET and Windows Runtime](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/10/29/introducing-windows-azure-storage-client-library-2-0-for-net-and-windows-runtime.aspx)

Piccola nota di colore: la library passa dalla versione 1.7 alla versione 2.0. Come ogni major release che si rispetti anche questa porta con se delle breacking changes, la più importante delle quali è un refactoring importante della solution e dei suoi namespace: il &#8220;vecchio&#8221; root namespace Microsoft.WindowsAzure.StorageClient è stato sostituito da Microsoft.WindowsAzure.Storage e ogni componente dello storage (Blob, Table e Queue) è stato &#8220;spostato&#8221; nel relativo sub namespace di competenza.

Attenzione quindi ad aggiornare i vostri progetti!
Dev avvisato&#8230;

