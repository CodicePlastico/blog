---
layout: post
current: post
cover: 'assets/images/post-cover/2023-04-la-nostra-esperienza-con-questpdf.jpg'
socialcover: 'assets/images/post-cover/2023-04-la-nostra-esperienza-con-questpdf-s.jpg'
slug: la-nostra-esperienza-con-questpdf
title: 'Soluzioni per la creazione di PDF: la nostra esperienza con QuestPDF'
date: 2023-04-18 12:00:00
category : [tech]
tags: [library, pdf]
author: [luca]
---

Nella vita di un programmatore, che sia per creare un report di produzione, oppure per la stampa di una ricevuta, capita a tutti, presto o tardi, di scontrarsi con il dover realizzare un file PDF.
Quando è toccato a noi affrontare il problema ci siamo subito interrogati su quale potesse essere la soluzione migliore e siamo salpati alla ricerca di una libreria che potesse aiutarci.
Dopo varie ricerche e diversi tentativi, siamo approdati su QuestPDF.

## QuestPDF

QuestPDF è una libreria C# open source che, come suggerisce il nome, **aiuta a creare file PDF**.

Tra le varie funzionalità che hanno attirato la nostra attenzione abbiamo:

- Utilizzo **semplice** con **alte performance**

- Generazione di file PDF **code only**, senza uso di template HTML o programmi esterni

- Un previewer per visualizzare il nostro file PDF **in tempo reale**, senza dover ricompilare il progetto

Il tutto in modo completamente **gratuito**, senza licenze, o funzionalità nascoste dietro un paywall!

Sembra troppo bello per essere vero (e, spoiler, forse lo è), ma andiamo subito a vedere come funziona.

## L'utilizzo

Prima di andare a vedere come generare una pagina andiamo subito a setuppare il previewer, per aiutarci durante lo sviluppo.

Per installare il previewer è necessario usa il comando: 

`dotnet tool install QuestPDF.Previewer --global`.

Successivamente, dal codice, ci basta usare la funzione `ShowInPreviewer()` in questo modo:

```csharp
using QuestPDF.Previewer;

var document = new TestDocument();
document.ShowInPreviewer();
```

Dopo aver setuppato il previewer, andiamo velocemente a creare un documento vuoto e facciamo subito partire il nostro progetto.

**ATTENZIONE:** se state utilizzando un sistema operativo Linux, potrebbe essere necessario installare delle dipendenze aggiuntive tramite il comando: 

`dotnet add package SkiaSharp.NativeAssets.Linux.NoDependencies` 

per consentire il corretto funzionamento della libreria.

```csharp
using QuestPDF.Drawing;
using QuestPDF.Fluent;
using QuestPDF.Helpers;
using QuestPDF.Infrastructure;

public class TestDocument : IDocument
{
  public void Compose(IDocumentContainer container)
  {
    container.Page(page =>
    {
      page.Size(PageSizes.A4);
    });
  }

  public DocumentMetadata GetMetadata() => DocumentMetadata.Default;
}

```

Ora facciamo partire il progetto tramite il comando 

`dotnet watch` 

e, esattamente come ci aspettavamo, viene aperto un programma che visualizza il nostro file PDF, che per ora non è altro che una pagina vuota.

<figure style="text-align:center"><img src="/assets/images/post-content/questpdf-01.png" alt="la-nostra-esperienza-con-questpdf" /></figure>

Andiamo ad aggiungere del contenuto!

Le pagine di QuestPDF sono divise in 3 sezioni: **Header**, **Content** e **Footer**.
Come sicuramente avrete notato però, abbiamo anche la possibilità di agire direttamente sul componente della pagina ed andare quindi ad impostare dei parametri, che saranno automaticamente rispettati da tutte le sezioni.
Nell’esempio mostrato sopra, sono andato ad agire sulla dimensione della pagina specificando che si tratta di un foglio standard A4.
**Sono disponibili moltissimi formati**, anche A0, nel caso doveste stampare il vostro PDF da un plotter!

Procediamo ora con ordine e andiamo a creare un nostro Header.

```csharp 
container.Page(page =>
    {
      page.Size(PageSizes.A4);
      page.Margin(20);
      page.Header().Width(300).Image("images/codiceplastico.png");
    });

```

Grazie al previewer il risultato delle nostre modifiche è immediato, e questo ci permette di **risparmiare molto tempo** nell'effettuare piccole modifiche estetiche, ad esempio centrando la nostra immagine.

<figure style="text-align:center"><img src="/assets/images/post-content/questpdf-02.png" alt="la-nostra-esperienza-con-questpdf" /></figure>

Raggiunto un risultato soddisfacente è tempo di passare al Content.

Il content (come in realta' anche gli altri componenti, se lo doveste trovare necessario) può essere diviso in delle sottosezioni: **Columns** o **Rows**.

In questo caso, il nome può risultare abbastanza fuorviante, infatti Column è un container nel quale ogni elemento corrisponde ad una nuova riga, mentre Row è un container nel quale ogni elemento corrisponde ad una nuova colonna.

Dopo un po' di pratica questa nomenclatura inizierà ad avere senso, ma personalmente non posso fare a meno di pensare che forse invertirli sarebbe stato meglio.

Sporchiamoci le mani ed andiamo a creare qualche riga e colonna!

<figure style="text-align:center"><img src="/assets/images/post-content/questpdf-03.png" alt="la-nostra-esperienza-con-questpdf" /></figure>

Il nostro content inizia a prendere forma, ma sembra che manchi qualcosa per renderlo perfetto... **magari il font giusto può risolvere questo problema!**

QuestPDF ha accesso a tutti i font installati sul sistema, ma se installare il font sulla macchina non dovesse essere un'opzione, è possibile utilizzare direttamente il file del font.

<figure style="text-align:center"><img src="/assets/images/post-content/questpdf-04.png" alt="la-nostra-esperienza-con-questpdf" /></figure>

Dopo aver cambiato il font il nostro content è decisamente migliorato!

Per concludere quindi non ci resta che il footer.

Per il footer ci andrà bene un semplice contatore della pagina in basso a destra.

```csharp
      page.Footer().AlignRight().Text(text =>
      {
        text.Span("Pagina ");
        text.CurrentPageNumber();
        text.Span(" di ");
        text.TotalPages();
      });
```

E così **in pochissimo tempo** siamo riusciti a creare il nostro file PDF, con un font personalizzato, senza perdere tempo con la creazione di un template HTML e senza dover cercare su StackOverflow come centrare un'immagine all’interno di un div.

## L'elefante nella stanza

Una libreria come QuestPDF ad uso completamente gratuito **sembra troppo bello per essere vero**, ed infatti è così…

Ad inizio 2023, QuestPDF ha annunciato di stare cercando delle opzioni per creare delle licenze a pagamento e ad oggi la decisione si è finalizzata in questo modo:

- Per singole persone o aziende sotto un fatturato lordo di $1M, QuestPDF sarà **completamente gratuito**

- Per aziende con fatturato lordo superiore a $1M e fino a 10 developer, **il costo sarà di $500 annui**

- Per aziende con fatturato lordo superiore a $1M con più di 10 developer, **il costo sarà di $3000 annui**

Fortunatamente, questo tipo di licenza **si applica solo alle versioni di QuestPDF successive alla 2022.12** (ultima versione disponibile durante la scrittura di questo articolo), mentre tutte le versioni precedenti rimangono completamente gratuite.

Il mio consiglio è comunque quello di darci un'occhiata per capire se può fare al caso vostro, sono sicuro che rimarrete soddisfatti!

## Riferimenti

**Siti**

* QuestPDF: [http://questpdf.com/](http://questpdf.com/)
* Comic Mono: [https://dtinth.github.io/comic-mono-font/](https://dtinth.github.io/comic-mono-font/)
* Come centrare un’immagine all’interno di un div (un buon ripasso non fa mai male): [https://stackoverflow.com/questions/388180/how-to-make-an-image-center-vertically-horizontally-inside-a-bigger-div](https://stackoverflow.com/questions/388180/how-to-make-an-image-center-vertically-horizontally-inside-a-bigger-div)
* Repository Github: [https://github.com/lucaTorrianiCP/QuestArticle](https://github.com/lucaTorrianiCP/QuestArticle)
