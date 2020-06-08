---
layout: post
title:  "Testare comportamenti multi-thread"
subtitle: "Quando la cosa difficile è far fallire i test ;-)..."
date: 2018-01-02 07:25:00 +0100
slug: testare-comportamenti-multi-thread
categories: tech
categories: tech
tags: [.net, testing, concurrency, multithreading, synchronization]
author: pietro
---
Mai scrivere codice di *produzione* se non a fronte di uno *unit test* che fallisce - e mai più dello stretto necessario a far passare il test: questo, fondamentalmente, lo spirito delle [*tre regole* del TDD](http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd).
Già: ma ci sono situazioni in cui scrivere uno unit test che fallisce è (a volte *molto*) più complicato di quanto non sia, poi, intervenire sul codice *di produzione* per farlo passare. È il caso, ad esempio, di componenti che devono esporre una semantica *thread-safe*: che si *comportano bene*, cioè, anche quando vengono utilizzati, concorrentemente, da più *thread* differenti.

Pensiamo ad esempio al caso, ovviamente banale, di una classe che implementa un contatore: qualcosa che, una volta completamente implementata, si presenterà in una forma simile a questa:

<pre>
public class Counter {
    public Counter(int initialValue) {
        Value = initialValue;
    }

    public Counter(): this(0) {            
    }

    public int Value { get; private set; }

    public void Increment() {
        <strong>lock (this) {</strong>
            Value++;
        <strong>}</strong>
    }
}
</pre>

Lo *statement* **`lock (this)`** risolve il problema dell'accesso concorrente da più *thread* alla stessa istanza di `Counter`, garantendo la serializzazione degli incrementi (`Value++`) e l'assenza di mancati aggiornamenti dovuti a [*race condition*](https://en.wikipedia.org/wiki/Race_condition) (`Value++` non è infatti un'operazione atomica, ma comporta la lettura del valore iniziale di `Value`, il suo incremento e la scrittura del risultato, sotto-operazioni che, in assenza di `lock`, potrebbero *mescolarsi* in modo diverso quando più *thread* le effettuano sulla stessa istanza di `Counter`).

Il problema dunque è noto, come nota (e semplice) è la soluzione.
Ma, in ottica TDD, tale soluzione non può essere implementata se non in presenza di uno *unit test* rosso: si tratta dunque di scrivere un test che eserciti la classe in questione utilizzando più *thread* e verifichi che il numero totale di incrementi sia quello atteso; un primo tentativo potrebbe essere questo (sia assumano valori alti, diciamo `100`, per `IncrementCount` e `ThreadCount`):

<pre>
[Fact]
public void CanUseTheSameCounterInMultipleConcurrentThreads() {
    Counter counter = new Counter();
    for (int i = 0; i < ThreadCount; i++) {
        new Thread(() => {
            for (int j = 0; j < IncrementCount; j++) {
                counter.Increment();                        
            }
        }).Start();
    }
    Assert.Equal(ThreadCount * IncrementCount, counter.Value);
}
</pre>

Il test in effetti fallisce (quasi sempre...) ma, cosa strana, fallisce sempre nello stesso modo: il valore del contatore al termine dell'elaborazione è infatti sempre lo stesso. Ragionando sulla cosa, si nota che in realtà il test non fallisce per il motivo giusto: fallisce perché il *thread* in cui gira il test si conclude prima che si siano concluse le esecuzioni di tutti gli altri *thread* (situazione in cui è corretto che non tutti gli incrementi attesi siano già avvenuti).
Si può risolvere il problema sincronizzando l'uscita dei *thread* mediante una *barriera*:
<pre>
[Fact]
public void CanUseTheSameCounterInMultipleConcurrentThreads() {
    Counter counter = new Counter();
    int threadCount = 100;
    int incrementCount = 100;
    <strong>Barrier barrier = new Barrier(threadCount + 1)</strong>;
    for (int i = 0; i < ThreadCount; i++) {
        new Thread(() => {
            for (int j = 0; j < IncrementCount; j++) {
                counter.Increment();                        
            }
            <strong>barrier.SignalAndWait()</strong>;
        }).Start();
    }
    <strong>barrier.SignalAndWait()</strong>;
    Assert.Equal(ThreadCount * IncrementCount, counter.Value);
}
</pre>

L'utilizzo di `barrier.SignalAndWait()` garantisce che l'asserzione finale non venga eseguita prima `ThreadCount + 1` *thread* (quello principale in cui gira il test e quelli che sollecitano la classe `Counter`) abbiano concluso la loro esecuzione. A questo punto, però, il test passa sempre: il che è sospetto (la *teoria* ci dice che il problema ci dovrebbe essere) e suggerisce di approfondire la questione.  Riflettendo un po' su quale può essere il motivo per cui, apparentemente, decine o centinaia di *thread* concorrenti si sincronizzano sempre nel modo "migliore" (dal punto di vista dell'utilizzo dell'istanza condivisa di `Counter`), notiamo una cosa: la logica che sollecita il sistema sotto test, `counter.Increment()`, è banale ed ha tempi di esecuzione piccolissimi - molto più piccoli dell'*overhead* comportato da inizializzazione (`new Thread( ... )`) ed avvio (`.Start()`) di un singolo *thread*; l'accesso all'istanza di `Counter` avviene dunque davvero in maniera serializzata, poiché quando termina l'inizializzazione del *thread* i-esimo l'esecuzione di quelli precedenti è già conclusa. Il *trucco*, di nuovo, consiste nell'utilizzo della classe `Barrier`:

<pre>
[Fact]
public void CanUseTheSameCounterInMultipleConcurrentThreads() {
    Counter counter = new Counter();
    <strong>Barrier startBarrier = new Barrier(ThreadCount);</strong>
    Barrier endBarrier = new Barrier(ThreadCount + 1);
    for (int i = 0; i < ThreadCount; i++) {
        new Thread(() => {
            <strong>startBarrier.SignalAndWait();</strong>
            for (int j = 0; j < IncrementCount; j++) {
                counter.Increment();                        
            }
            endBarrier.SignalAndWait();
        }).Start();
    }
    endBarrier.SignalAndWait();
    Assert.Equal(ThreadCount * incrementCount, counter.Value);
}	
</pre>

Lo *statement* **`startBarrier.SignalAndWait()`** rappresenta infatti un punto di sincronizzazione tra i *thread* che condividono l'accesso all'istanza di `Counter` sotto test, a valle di qualunque *overhead* di inizializzazione ed avvio del *thread* stesso e prima dell'utilizzo di `counter.Increment()`. A questo punto il test fallisce in modo sistematico (benché non sempre con lo stesso valore di `counter.Value`, come del resto ci aspettavamo) e siamo autorizzati, in ottica TDD, ad intervenire sul codice della classe `Counter` inserendo lo *statement* di `lock`.

Il nostro esempio è dunque completo; vale la pena tuttavia di aggiungere una piccola nota, a proposito di un semplice *trucco* che talvolta torna utile per ottenere l'agognato test rosso in fase di scrittura di *unit test* che coinvolgono concorrenza tra *thread*: in alcune situazioni la sincronizzazione iniziale non è sufficiente a far fallire il test, o non lo è a farlo fallire in modo sistematico; se il codice che si sta testando, o quello che, eseguito in *thread* concorrenti, lo esercita, esegue operazioni che, per loro stessa natura, introducono punti di sincronizzazione (penso alla scrittura in `Console` o su file di log, ad esempio), tale sincronizzazione tende poi a mantenersi, dato che tutti i *thread* si comportano allo stesso modo. In casi del genere, può essere utile introdurre un ritardo casuale tra un'esecuzione e la successiva del codice che costituisce il corpo del *thread* (ed eventualmente anche in avvio di ogni *thread*), in modo da disallineare (anche se di poco) i tempi di esecuzione dei diversi *thread*:

<pre>
[Fact]
public void CanUseTheSameCounterInMultipleConcurrentThreads() {
    <strong>Random random = new Random();</strong>
    Counter counter = new Counter();
    Barrier startBarrier = new Barrier(ThreadCount);
    Barrier endBarrier = new Barrier(ThreadCount + 1);
    for (int i = 0; i < ThreadCount; i++) {
        new Thread(() => {
            startBarrier.SignalAndWait();
            for (int j = 0; j < IncrementCount; j++) {
                <strong>Thread.Sleep(random.Next(10));</strong>
                counter.Increment();                        
            }
            endBarrier.SignalAndWait();
        }).Start();
    }
    endBarrier.SignalAndWait();
    Assert.Equal(ThreadCount * IncrementCount, counter.Value);
}
</pre>

Il codice completo dell'esempio è disponibile [qui](https://gitlab.com/pietrom_cp/ThreadSafeCounter).

**NOTA**: l'esempio è scritto in C# ed utilizza [xUnit.net](https://xunit.github.io) come framework di testing; con pochissimi cambiamenti, si può realizzare un esempio simile per la piattaforma Java, utilizzando [JUnit](http://junit.org/junit5) per la scrittura dei test e la classe [`CyclicBarrier`](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/CyclicBarrier.html) per la sincronizzazione dei *thread*.