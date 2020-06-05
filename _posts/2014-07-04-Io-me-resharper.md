---
layout: post
title:  "Io, me e R#"
comments: true
categories: tech
tags: [c#]
author: ale
---

Sono sempre stato un grande fan di [R#](http://www.jetbrains.com/resharper/): il giusto completamento di un ottimo IDE come [Visual Studio](http://www.visualstudio.com/).  
E' vero, nei suoi primi anni di vita, le performance non erano delle migliori e l'utilizzo delle risorse particolarmente "esoso". Con il passare del tempo grazie alle ottimizzazioni apportate al tool e alla possibilita' di sfruttare macchine sempre piu' carrozzate (RAM e SSD in primis) il peso si avvertiva sempre meno.  

Circa un anno fa, pero', mi sono accorto di alcuni spiacevoli side-effects nell'utilizzo di R#:  
- facendo spesso mentoring, quando mi trovavo a lavorare con sviluppatori che non avevano/potevano installare il tool, la produttivita' ne risentiva pesantemente, rischiando di farmi fare la parte del "dislessico da tastiera"  
- lavorando su una soluzione piuttosto corposa, che utilizzava pesantemente pacchetti Nuget, per la gestione del versioning e delle reference cross-modulo, R# aveva (ha?) lo spiacevole vizietto di aggiungere le reference, con un semplice Alt+Enter, ma non attraverso il pacchetto nuget, bensi' agganciando la prima versione che trovava in una qualche folder di output di progetto creando non pochi problemi a catena...oltre a quelli che nuget genera di suo...ma questa e' un'altra storia :)

Ma la cosa piu' spiacevole di cui mi sono accorto e' che il tool, di cui continuo a ritenermi uno "sponsor", sembra spegnere la testa di (alcuni) dev: a colpi di Alt+Enter e Alt+Ins si creano classi e costruttori, si aggiungono reference o si "iniettano" dipendenze alla velocita' della luce, limitando qualsiasi pensiero di buon design.  
E' troppo semplice, con pochi shortcut, delegare al tool la scrittura di un "botto" di codice.   E' talmente veloce che non si ha quasi nemmeno il tempo di pensare...e poi ci si ritrova, per esempio, con classi con 10 dipendenze...  
Se ci pensiamo bene non veniamo pagati per quanto veloce pigiamo sulla tastiera, ma per la qualita' del lavoro che facciamo e quindi del codice che produciamo...o almeno cosi dovrebbe essere. Lo so, essendo in Italia, sono un inguarible idealista...

E allora mi sono deciso a disinstallarlo. Vi lascio solo immaginare cosa ha comportato, in termini di produttivita' all'inizio, ma poco a poco mi sono adattato ai (pochi) shortcut integrati nell'IDE, ma me li sono fatti bastare.  
Ogni singola operazione mi costa, potenzialmente, di piu', ma proprio per questo extra-time (si parla di 1 o 2 secondi) ho avuto modo di poter pensare una volta di piu' a quello che stavo facendo...e vi posso assicurare la qualita' del codice prodotto ne ha giovato...o almeno cosi' mi sembra

Ok, detto questo, credo che lo reinstallero' :) E sapete perche'?  
Perche' qualsiasi tentativo di trovare un "runner" per i test all'altezza di quello integrato in R# ha dato esito negativo: sia in termini di velocita' di esecuzione, sia nella fase di discovery dei test, sia nel livello di dettaglio dell'output e in tanti altri piccoli aspetti.  
Per intenderci, se ogni volta che lancio un test devo aspettare un paio di secondi per avere un feedback, oltre che della produttivita' ne va anche della mia salute mentale.  

Vi assicuro che l'unico motivo per cui, penso, reinstallero' R# e' proprio questo. Per il resto, si puo' fare (cit.), si puo' vivere senza R#: "Sono Alessandro, un dev, e da piu' di un anno lavoro senza R#"...pero' forse lo reinstallo :)