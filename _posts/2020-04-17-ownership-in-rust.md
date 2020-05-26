---
layout: post
current: post
cover: 'assets/images/post-cover/20200417-rust.jpg'
socialcover: 'assets/images/post-cover/20200417-rust-social.jpg'
slug: ownership-in-rust
title: 'L&rsquo;ownership in Rust, spiegata con le torte'
date: 2020-04-17 00:00:00
category : tech
tags: [rust, kata, howto]
author: gio 
---


Rust è un linguaggio relativamente giovane e **molto innovativo** sotto vari punti di vista. Proponendosi inizialmente come un linguaggio "per sistemi" (definizione un po' vaga, che per semplicità tradurrei con "possibile sostituto del linguaggio C") negli ultimi anni ha avuto un'evoluzione molto rapida. 
I campi in cui questo linguaggio
eccelle sono molti e vari: dal **networking** alla **programmazione embedded**,
fino all'integrazione nel **frontend** di applicazioni web tramite **WebAssembly**.

Anche i colossi della tecnologia hanno espresso notevole interesse,
come Microsoft che prima ha iniziato ad
[introdurre codice Rust nel suo sistema operativo](https://msrc-blog.microsoft.com/2019/11/07/using-rust-in-windows/)
e poi ha deciso di
[crearne una copia](https://www.microsoft.com/en-us/research/news-and-awards/microsoft-opens-up-rust-inspired-project-verona-programming-language-on-github-2/),
certe [abitudini](https://en.wikipedia.org/wiki/Embrace,_extend,_and_extinguish)
sono dure a morire.

Ma cosa c'è di cosi' innovativo in questo linguaggio?
Rust ci promette **performance** paragonabili a quelle che si possono ottenere
con linguaggi come C e C++, consentendo una gestione molto raffinata
delle risorse della macchina su cui gira il codice, la memoria in primis;
ma, a differenza dei suddetti linguaggi, Rust ci fornisce un'altra
importante garanzia: la "**safety**".

## Ma cosa si intende per "safety"?

Chiunque abbia programmato qualcosa in C si è probabilmente scontrato
con questo simpatico errore:

```
Segmentation fault
```

che ci comunica che il nostro programma ha cercato di fare qualcosa
di "brutto" con la memoria, come ad esempio provare a leggere/scrivere una
zona di memoria non allocata.
Errori come questo sono particolarmente **difficili da debuggare** perchè si
presentano solo a tempo di esecuzione e, dato che l'errore non è propriamente "parlante", puo'
essere anche molto difficile replicarlo ed individuarne le cause.
<cite>
Il linguaggio C è pensato per **lasciare completa libertà al programmatore**,
ma "da grandi poteri derivano grandi responsabilità" e, aggiungerei, 
"da grandi responsabilità deriva immenso stress".
</cite>
I linguaggi di programmazione piu' moderni risolvono questo problema gestendo
**l'allocazione della memoria in modo automatico**, tipicamente tramite l'utilizzo di un
garbage collector (GC per gli amici). I classici esempi sono quelli ad oggetti, come Java o C#,
ma anche linguaggi puramente funzionali come Haskell sono dotati di un GC.
Questo toglie un problema dalla testa del programmatore, che puo' concentrarsi meglio
sul proprio compito e quindi essere piu' produttivo.
Inoltre viene pressoché **eliminata un'intera classe di errori** che potrebbero essere introdotti
nel programma: quelli relativi alla gestione della memoria. Per questo motivo i linguaggi
dotati di un GC vengono considerati piu' "safe" rispetto a quelli che non ne hanno uno.

Tuttavia la **perdita di controllo** sulla gestione della memoria rende piu' difficile,
se non impossibile, **ottimizzare il codice e migliorarne le performance** sia in termini
di memoria che di tempi d'esecuzione. Un GC è in sostanza un programma che ogni tanto
deve bloccare l'esecuzione del nostro codice e fare del lavoro per capire quali zone di
memoria possono essere liberate. Questo rende i tempi d'esecuzione del programma,
se non piu' lunghi, quantomeno piu' difficili da prevedere, visto che non ci è dato sapere quando
il GC entrerà in azione ne' quanto tempo impiegherà.

Esiste quindi un inevitabile tradeoff tra "safety" e "controllo/performance"?
Sembrerebbe proprio di si... se non fosse per Rust!

![Grafico](/assets/images/post-content/rust-graph.png)



**Rust riesce a fornire forti garanzie di sicurezza senza pero' utilizzare un GC.**
Il compilatore di Rust è in grado di identificare, a tempo di compilazione,
errori di gestione della memoria, impedendoci di compilare codice scorretto.
Questo è possibile grazie all'introduzione del concetto di **ownership** e di
una serie di regole che è necessario rispettare se vogliamo che il nostro
codice compili.
&Egrave; questo il prezzo da pagare se vogliamo scrivere codice che è sia performante
che "safe". La curva di apprendimento è piuttosto lenta e,
specialmente all'inizio, **programmare in Rust puo' sembrare un'eterna
lotta contro il compilatore**, inoltre i tempi di compilazione sono piuttosto lunghi
(il che è comprensibile dato che il compilatore Rust fa una complessa analisi statica
del codice).

<p style="text-align:center; mix-width:100%;"><img  src="https://media.giphy.com/media/M11UVCRrc0LUk/source.gif" ></p>

Tuttavia, a favore di Rust, non ho mai visto **messaggi di errore più
esplicativi** (spesso arrivano a dirti esattamente come modificare il
codice per farlo compilare!) ed il motto "If it compiles, it works"
non è mai stato cosi' azzeccato.

## Introduzione al meccanismo di ownership/borrowing

Vediamo insieme il concetto di ownership in un modo che sia comprensibile anche per chi non conosce
Rust ma è familiare con la programmazione. Per questo motivo eviteremo i dettagli tecnici della sintassi
e del typesystem di Rust per i quali rimando all'ottimo [libro](https://doc.rust-lang.org/book/).
Inoltre è possibile provare il linguaggio senza doverlo installare
sulla propria macchina [qui](https://play.rust-lang.org/), dove potrete
copincollare ed eseguire i vari esempi di codice che troverete di seguito.

### Le tre regole di ownership

Le principali regole del meccanismo di ownership sono le seguenti:
<ol>
<li>Per <strong>ogni dato esiste una variabile owner</strong> di tale dato.</li>
<li>Un dato può avere un <strong>unico owner</strong>, l'owner può cambiare nel tempo.</li>
<li>Quando un <strong>owner esce dallo scope, il suo dato viene eliminato</strong> (e quindi la memoria da esso occupata viene liberata).</li>
</ol>

Consideriamo questo semplice esempio:
```rs
fn main() {
  let jane = String::from("torta di mele");
  println!("Oggi zia Jane ci ha preparato: {}", jane);
}
```
<br/>
La variabile `jane` è owner della stringa `"torta di mele"`, questo implica
che sarà questa variabile ad essere responsabile della **deallocazione della
memoria occupata** dalla stringa.

Al termine della funzione `main`, dopo aver stampato il messaggio in console
`jane` uscirà dallo scope e la memoria occupata dalla sua `"torta di mele"`
verrà automaticamente liberata.

Fino a qui niente di strano, ma proviamo a modificare leggermente l'esempio:

```rs
fn main() {
  let jane = String::from("torta di mele");
  let tom = jane;
  println!("Oggi la zia Jane ci ha preparato: {}", jane);
}
```
<br/>
In questo secondo esempio, `tom` **sta "rubando" la torta alla
povera `jane`**, causando, tra l'altro, un errore in compilazione!
<br/>
```shell
error[E0382]: borrow of moved value: `jane`
 --> src/main.rs:4:52
  |
2 |   let jane = String::from("torta di mele");
  |       ---- move occurs because `jane` has type  `std::string::String`, 
  |              which does not implement the `Copy` trait
3 |   let tom = jane;
  |             ---- value moved here
4 |   println!("Oggi la zia 
  |         Jane ci ha preparato: {}", jane);
  |                                    ^^^^ value borrowed here after move
```
<br/>
Capiremo meglio tutti i dettagli di questo messaggio di errore piu' avanti, quello
che ci interessa al momento è che tramite l'istruzione `let tom = jane` stiamo
trasferendo l'ownership della `"torta di mele"` da `jane` a `tom`, operazione
che in "Rustiano" è chiamata **move**. 

**Una volta spostata l'ownership alla variabile `tom`, `jane` non è piu' utilizzabile.**


<p style="text-align:center; mix-width:100%;"><img  src="https://media.giphy.com/media/28BFzOGu9BeGHxKtsY/source.gif" ></p>

Questo codice invece compila tranquillamente:
```rs
fn main() {
  let jane = String::from("torta di mele");
  let tom = jane;
  println!("Tom si è sbafato tutta la {}", tom);
}
```
<br/>
Si puo' effettuare una move anche tramite il passaggio di argomenti ad una funzione.
<br/>

```rs
fn main() {
  let jane = String::from("torta di mele");
  eat_cake(jane);
  println!("Oggi la zia Jane ci ha preparato: {}", jane);
}

fn eat_cake(tom: String) { }
```
<br/>

La funzione `eat_cake`, non fa nulla se non **prendersi l'ownership**
della `"torta di mele"`, al termine di questa funzione la variabile `tom`
uscirà dallo scope e la `"torta di mele"` cesserà di esistere.
Anche in questo caso avremo un errore in compilazione:
<br/>
```shell
error[E0382]: borrow of moved value: `jane`
 --> src/main.rs:4:52
  |
2 |   let jane = String::from("torta di mele");
  |       ---- move occurs because `jane` has type `std::string::String`,
  |           which does not implement the `Copy` trait
3 |   eat_cake(jane);
  |            ---- value moved here
4 |   println!("Oggi la zia Jane ci ha preparato: 
  |              {}", jane);
  |                  ^^^^ value borrowed here after move
```
<br/>
A questo punto forse starete pensando:
<cite>Quindi ogni volta che passo un valore ad una funzione in Rust
non posso piu' utilizzarlo al di fuori di quella funzione?</cite>


**La risposta, fortunatamente, è no.** 

Ci sono vari modi per risolvere questo problema,
uno è il seguente:
<br/>

```rs
fn main() {
  let jane = String::from("torta di mele");
  let jane = do_not_eat_cake(jane);
  println!("Oggi la zia Jane ci ha preparato: {}", jane);
}

fn do_not_eat_cake(tom: String) -> String {
  return tom
}
```
<br/>
In questo caso il codice compila perchè la funzione `do_not_eat_cake`
sta **restituendo l'ownership della stringa alla funzione chiamante**.
Cerchiamo adesso di dare almeno un senso a questo esempio modificando la
stringa all'interno della funzione.
<br/>

```rs
fn main() {
  let jane = String::from("torta di mele");
  let jane = eat_piece_of_cake(jane);
  println!("Oggi la zia Jane ci ha preparato: {}", jane);
}

fn eat_piece_of_cake(mut tom: String) -> String {
  tom.pop();
  return tom;
}
```
<br/>
In questo esempio viene introdotto un altro concetto importante: quello di **mutabilità**.
Tutto in Rust **viene considerato immutabile, a meno che non venga dichiarato esplicitamente
il contrario** utilizzando la parola chiave `mut`. Nell'ultimo esempio, infatti, abbiamo
dovuto annotare la variabile `tom` come mutabile.

Si noti anche l'utilizzo dello **"shadowing"** che mi permette di ridefinire due volte la variabile
`jane`, cambiando eventualmente anche il tipo della variabile
(in questo caso il tipo rimane sempre `String`).

Questo modo di operare, spostando la ownership di un dato da una variabile ad un'altra,
ha pero' delle limitazioni. Potrei voler avere **accesso allo stesso dato da punti diversi
del codice senza necessariamente acquisirne la ownership**. 

Questo è possibile tramite il **borrowing**.

### Chiedi e ti sarà prestato

L'ultimo esempio di codice puo' essere riscritto anche in questo modo:
<br/>

```rs
fn main() {
  let mut jane = String::from("torta di mele");
  eat_piece_of_cake(&mut jane);
  println!("Oggi la zia Jane ci ha preparato: {}", jane);
}

fn eat_piece_of_cake(tom: &mut String) {
  tom.pop();
}
```
<br/>
La funzione `eat_piece_of_cake` prende come argomento una **reference** alla variabile,
il che implica che la ownership non cambia.
Il procedimento di passare un valore per riferimento viene anche detto **borrowing**, cioè prestare.
<p style="text-align:center; mix-width:100%;"><img  src="https://media.giphy.com/media/He4wudo59enf2/source.gif" ></p>

Si noti che il **passaggio per reference è necessariamente esplicito**, questo puo'
sembrare scomodo ma è necessario perché il compilatore possa analizzare
staticamente il codice, un piccolo prezzo da pagare per la "safety".
Come le variabili, **le reference possono essere mutabili o meno**, nell'esempio stiamo utilizzando
una reference mutabile `&mut`, una reference non mutabile viene dichiarata usando semplicemente
il carattere `&`.

Dobbiamo comunque rispettare alcune regole: ad esempio non possono esistere nello stesso
scope due riferimenti mutabili allo stesso dato.
<br/>
```rs
fn main() {
  let mut jane = String::from("torta di mele");
  let tom = &mut jane;
  let tim = &mut jane;
  tom.pop();
  tim.pop();
}
```
<br/>
In questo caso l'errore di compilazione è il seguente:
<br/>
```shell
error[E0499]: cannot borrow `jane` as mutable more than once at a time
 --> src/main.rs:4:13
  |
3 |   let tom = &mut jane;
  |             --------- first mutable borrow occurs here
4 |   let tim = &mut jane;
  |             ^^^^^^^^^ second mutable borrow occurs here
5 |   tom.pop();
  |   --- first borrow later used here
```
<br/>
Questo **puo' sembrare molto restrittivo** se paragoriamo Rust ad un qualsiasi
linguaggio ad oggetti che mi permette di avere un qualsiasi numero di riferimenti
(mutabili o no) allo stesso oggetto sparsi in giro per il codice, ma è anche cio'
che permette a Rust di individuare **race conditions** a tempo di compilazione.

Le race condition sono errori che si verificano quando, in modo concorrente,
**diverse parti del codice cercano di leggere/scrivere la stessa zona di memoria**.
Sono anche il motivo principale per cui scrivere codice parallelo è complesso e
prono ad errori, soprattutto quando si deve parallelizzare una base di codice già esistente.
Rust è in grado di identificare potenziali race condition a tempo di compilazione
e la standard library fornisce anche delle primitive molto comode per la programmazione concorrente.

## Limiti o opportunità?

Da qualsiasi background si provenga, imparare Rust non è facile,
ma non è nemmeno impossibile, una volta superato lo scoglio iniziale.
Le limitazioni imposte dal compilatore, che possono sembrare a volte
troppo stringenti, possono essere anche viste come **un'opportunità per
fermarci un attimo a riflettere**:
<cite>&Egrave; davvero questo il modo migliore
per strutturare il codice? Potrei provare altre strade?</cite>

Ad esempio, in un linguaggio ad oggetti spesso capita di avere
un oggetto referenziato da svariate classi, ciascuna delle quali
lo puo' modificare; man mano che l'applicazione si fa piu' complessa,
**diventa molto difficile seguire il ciclo di vita dell'oggetto**
(classico esempio: mi trovo un oggetto modificato
inaspettatamente, per poi scoprire che era modificato da una classe
di cui nemmeno sapevo l'esistenza).
In Rust una struttura dati del genere è difficile da implementare
(anche se non impossibile) e forse è giusto che lo sia.

Se approcciato con la mentalità giusta, sono convinto che Rust possa aiutarci ad
uscire dai nostri schemi e a diventare **programmatori migliori**.
