---
layout: post
current: post
title: "Testare comportamenti multi-thread (reprise)"
subtitle: "Una classe helper riutilizzabile"
date: 2018-02-01 07:00:00 +0100
categories: .net,testing,concurrency,multithreading,synchronization
slug: testare-componenti.multi-thread-reprise
tags: [Tech]
author: pietro

---
Riparto da dove ero arrivato nel [post precedente](.net,testing,concurrency,multithreading,synchronization/2018/01/02/testare-comportamenti-multi-thread.html), nel quale si è parlato di come testare comportamenti soggetti a concorrenza e si sono viste alcune tecniche utili ad evitare *falsi positivi*: 
<pre>
[Fact]
public void CanUseTheSameCounterInMultipleConcurrentThreads() {
    Random random = new Random();
    <strong>Counter counter = new Counter();</strong>
    Barrier startBarrier = new Barrier(ThreadCount);
    Barrier endBarrier = new Barrier(ThreadCount + 1);
    for (int i = 0; i < ThreadCount; i++) {
        new Thread(() => {
            startBarrier.SignalAndWait();
            for (int j = 0; j < IncrementCount; j++) {
                Thread.Sleep(random.Next(10));
                <strong>counter.Increment();</strong>
            }
            endBarrier.SignalAndWait();
        }).Start();
    }
    endBarrier.SignalAndWait();
    <strong>Assert.Equal(ThreadCount * IncrementCount, counter.Value);</strong>
}
</pre>

Il codice del test in questione si presta bene ad essere [rifattorizzato](https://en.wikipedia.org/wiki/Code_refactoring), separando la parte specifica del *sistema sotto test* (evidenziata in grassetto) da quella che affronta il generico problema del testing di comportamenti concorrenti.
Proviamo dunque ad immaginare come vorremmo scrivere il nostro test, utilizzando una classe helper `ConcurrentExecutor` che nasconda i dettagli di sincronizzazione (iniziale e finale) e gestione dei ritardi casuali tra un'esecuzione e l'altra:

<pre>
[Fact]
public void CanUseTheSameCounterInMultipleConcurrentThreads() {
    Counter counter = new Counter();
    executor.Execute(counter.Increment);
    Assert.Equal(executor.ThreadCount * executor.RepetitionsCount, counter.Value);
}
</pre>

Il codice del caso di test è molto più semplice di quello ottenuto nel post precedente: è focalizzato esclusivamente sul comportamento che si sta testando (`Counter.Increment`) ed utilizza, per gestire la complessità legata al contesto *multi-threading*, un esecutore così inizializzato (ad esempio, nel costruttore della classe che contiene il test):
<pre>
const int threadCount = 100;
const int repetitionsCount = 100;
executor = new ConcurrentExecutor {
    RepetitionsCount = repetitionsCount,
    ThreadCount = threadCount,
    RandomDelayBetweenExecutions = true
};  
</pre>
La classe `ConcurrentExecutor` espone tre proprietà, che permettono di scegliere
- il numero di *thread* da utilizzare concorrentemente
- il numero di esecuzioni del codice da testare
- se introdurre o meno un ritardo casuale ad ogni esecuzione del codice sotto test

L'implementazione dell'helper in questione è (sulla base di quanto visto in precedenza) molto semplice ed è riutilizzabile, alla bisogna, per scrivere test simili a quello proposto sin qui:
<pre>
public class ConcurrentExecutor {
    public int ThreadCount { get; set; } = 50;
    public int RepetitionsCount { get; set; } = 200;
    public bool RandomDelayBetweenExecutions { get; set; } = true;

    private readonly Random random = new Random();

    public void Execute(<strong>Action action</strong>) {
        Barrier startBarrier = new Barrier(ThreadCount);
        Barrier endBarrier = new Barrier(ThreadCount + 1);
        for (int i = 0; i < ThreadCount; i++) {
            new Thread(() => {
                startBarrier.SignalAndWait();
                for (int j = 0; j < RepetitionsCount; j++) {
                    if (RandomDelayBetweenExecutions) {
                        Thread.Sleep(random.Next(10));
                    }
                    <strong>action();</strong>
                }
                endBarrier.SignalAndWait();
            }).Start();
        }
        endBarrier.SignalAndWait();
    }
}
</pre>