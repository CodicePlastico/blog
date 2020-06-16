---
layout: post
current: post
cover: 'assets/images/post-cover/20200615-remote-pair-programming.jpg'
socialcover: 'assets/images/post-cover/20200615-remote-pair-programming-s.jpg'
slug: pair-programming-remoto
title: 'Pair Programming Remoto:  i consigli per renderlo efficace da chi lo sperimenta sul campo '
date: 2020-06-15 00:00:00
categories: [tech]
tags: [pair programming, metodologie]
author: [pietro, bonny, umby]
---

Il Pair Programming è una tecnica che prevede due programmatori che sviluppano insieme da una medesima postazione. I partecipanti, all’inizio di ogni sessione, stabiliscono due ruoli: il **driver**, che operativamente scrive il codice e ha l’onere (e l’onore!) di portare a termine la soluzione funzionante e l’**observer** che ha il compito di supervisionare il codice, indicare errori e discutere delle soluzioni attuate.

Nato come best practice normalmente riferita al mondo <i>Agile</i>, il Pair Programming ha sempre generato posizioni contrastanti: dagli **affiatati sostenitori** che ne sostengono l’efficacia, ai più **scettici detrattori** che non vedono benefici nell’allocare due persone, letteralmente, a fare il lavoro che potrebbe essere svolto da una.

CodicePlastico segue un modello misto: alcuni team adottano il Pair Programming e alcuni clienti richiedono di affiancare i loro sviluppatori nel Pair Programming, ma non mancano anche al nostro interno opinioni contrastanti. Inoltre, essendo un’azienda remote friendly, nel nostro caso non si tratta solo di Pair Programming, ma di **Pair Programming remoto**.

Visto il recente hype sul tema **smart working**, abbiamo pensato potesse essere utile condividere la nostra esperienza, andando ad intervistare <a href="https://blog.codiceplastico.com/authors/pietro-martinelli" target="_blank">Pietro</a>, <a href="https://blog.codiceplastico.com/authors/marco-bonera" target="_blank">Marco</a> e <a href="https://blog.codiceplastico.com/authors/umberto-franchini" target="_blank">Umberto</a> che hanno nel loro bagaglio professionale anni di Pair remoto. Per rendere l’argomento più scottante, abbiamo raccolto le domande dell’intervista tra tra gli **scettici del Pair**, andando a punteggiare i migliori luoghi comuni sull’argomento.
<br/>

## Il tempo 

<p style="text-align:center; min-width:100%;">
    <img src="https://media.giphy.com/media/4QX6y0fQT7yVO/source.gif" style=" min-width:100%;height: auto;">
</p>
<br/>
<cite>
Ma non si perde tempo a fare in due una cosa che potrebbe essere fatta da soli?
</cite>

<strong class="emph">Pietro</strong>: Anche acquistare tre prodotti (al prezzo di due) costa più dell'acquistare il solo necessario - ma nel medio periodo conviene. Da questo punto di vista, *fare Pair Programming è come fare regolarmente code review* - solo più divertente e sociale.

<strong class="emph">Marco</strong>: Il tempo è soggettivo se si considera che un software sviluppato con due teste è sicuramente fatto meglio. Il tempo di realizzazione di una feature è la somma del tempo di sviluppo e soprattutto di quello dedicato manutenzione e refactoring.  Grazie al pair le *operazioni di mantenimento del software risultano essere più facili*, proprio perchè il software è scritto meglio.

<strong class="emph">Umberto</strong>: Il Pair è una modalità di programmazione molto interessante. Sicuramente per chi è abituato a lavorare sempre da solo è facile considerare il lavorare in due sullo stesso task una perdita di tempo. Dopo anni d'esperienza in Pair, posso affermare che non è sempre così. Ad esempio *il Pair è molto efficace quando si svolge un lavoro particolarmente complesso* dove serve concentrazione sicuramente quattro occhi sono meglio che due  :-D 
In particolare il cliente per cui lavoriamo, lo richiede per 3 motivi principali: 

<ul>
<li><strong>Qualità del codice prodotto</strong> con una conseguente diminuzione del tempo di code review </li>
<li>Quando manca uno degli sviluppatori della coppia comunque <strong>il progetto non si ferma</strong></li>
<li>Facilita il <strong>passaggio di conoscenza</strong> del dominio e tecniche tra gli sviluppatori </li>
</ul>

Oggettivamente il Pair può diventare una *perdita di tempo quando le attività sono semplici*; in questi casi cerco sempre di parallelizzare le operazioni in modo da dividersi i task mantenendo un allineamento costante.  

<cite>
Non hai mai desiderio di lavorare da solo?<br/>
Non ti stufi a stare in call tutto il giorno?<br/>
Pensare a voce alta in continuazione non è estenuante?
</cite>

<strong class="emph">Pietro</strong>: Certo, a volte mi va di lavorare da solo e talvolta lo faccio: Pair Programming non significa non lavorare mai da soli, significa lavorare *prevalentemente in coppia*, il che per altro è un'abilità che non tutti hanno e che va affinata e coltivata. Effettivamente a volte risulta pesante essere in call in continuazione:  questo è il vero unico problema con il Pair Programming remoto, per come la vedo io. Non considero un problema pensare ad alta voce - anzi, trovo che sia *stimolante condividere il proprio flusso di pensiero ed assai interessante osservare quello altrui...* se poi il risultato è lo stesso - e spesso lo è - si ha la conferma della bontà della conclusione cui s'è giunti.

<strong class="emph">Marco</strong>: Certo a volte *risulta essere pesante portare la cuffia tutto il giorno...* vero è che le operazioni che non richiedono uno specifico studio dell’architettura o un confronto approfondito vengono svolte in autonomia. Inoltre durante la giornata è fondamentale *programmare delle pause* dal Pair: ricaricano il cervello e favoriscono la generazione di nuove idee. Per quanto riguarda il pensare ad alta voce lo trovo piacevole: avere una controparte fa riflettere, ti fa scoprire nuove strade e amplia la tua visione del progetto. 

<strong class="emph">Umberto</strong>: Il Pair remoto è *questione di abitudine* infatti stare in call tutto il giorno, pensare ad alta voce, non è facile nemmeno ora dopo anni, ma con il tempo ti abitui... anche se non posso negare che lavorare da soli spesso è più “semplice”.

<div class="emphasis">
<h4>L’angolo del consiglio pratico</h4>
<ul>
<li><strong>Sì al Pair, ma i task più semplici possono essere svolti in autonomia</strong>: in questo modo si alternano momenti di lavoro in coppia a momenti di lavoro in solitaria e si porta avanti il progetto in parallelo.</li>
<li><strong>Il Pair richiede una serie di soft skill</strong> che si maturano nel tempo: non scoraggiatevi se le prime sessioni risultano impegnative e ricordate di schedulare delle pause.</li>
</ul>
</div>


## La coppia
<p style="text-align:center; min-width:100%;">
    <img src="https://media.giphy.com/media/PjplWH49v1FS0/source.gif" style="min-width:100%; height: auto;">
</p>

<cite>
Quanto conta fare Pair con la persona giusta?<br/> Il rischio non è quello che il driver lavora per davvero e l’observer va a traino? <br/>Non è una sensazione fastidiosa essere costantemente controllati?
</cite>

<strong class="emph">Pietro</strong>: Fare Pair Programming con *la persona giusta conta moltissimo*, sia dal punto di vista *tecnico-metodologico* sia da quello *umano*. Non tutti possono fare Pair Programming con tutti: è questione di integrarsi bene dal punto di vista caratteriale e dei modi, sia per evitare l'effetto traino sia, direi soprattutto, per evitare di infastidirsi a vicenda. Per quanto riguarda il tema “controllo” è bene premettere che *non si tratta di controllare, bensì di collaborare* (e si dovrebbero scambiare spesso i ruoli). In secondo luogo, solo chi si sente in difetto è infastidito dal fare le cose in compagnia...

<strong class="emph">Marco</strong>: Sicuramente un affiatamento di coppia è necessario. C’è il rischio che uno prevarichi l’altro. *È buona pratica lasciare svolgere un’idea a chi scrive fino alla fine*, senza continue interruzioni. Se non si è d’accordo si può rivedere in seguito. Il tema del controllo è delicato e dipende sempre dalla persona con cui si lavora. Bisogna essere malleabili e recettivi per poter trovare la strada migliore, oltre che pronti a mettersi in discussione. Per certe persone può essere pesante.

<strong class="emph">Umberto</strong>: Lavorare con la persona giusta è molto importante... ti può cambiare la giornata. Fare Pair con delle persone con le quali c’è un perenne scontro comporta uscirne esausti e molto meno produttivi. 
Posso considerare *il Pair un po’ come un matrimonio*: un po’ tira uno e un po’ tira l’altro... se si va sempre e solo in un verso, la coppia scoppia.

<div class="emphasis">
<h4>L’angolo del consiglio pratico</h4>
<ul>
<li>Scegliete il <strong>partner giusto</strong> (e non è detto che quello giusto sia il primo) e ogni tanto prova a cambiare partner (ma solo se tutti sono d'accordo)</li>
<li>Se è possibile e la coppia è stabile, è importante fare le prime sessioni di Pair dal vivo giusto per conoscersi meglio e passare successivamente al Pair remoto. È fondamentale tenere presente sempre che non siamo automi: <strong>è necessario saper instaurare un buon rapporto</strong> con la persona con cui si lavora. Se c’è intesa, la giornata lavorativa sicuramente sarà più proficua! </li>
</ul>
</div>

<cite>
Non ti annoia dover spiegare ogni scelta architetturale/tecnica che fai?<br/>
Non rischi di perdere la concentrazione a causa delle continue interruzioni?
</cite>

<strong class="emph">Pietro</strong>: Per nulla: anzi, *una scelta non esiste finché non la spiego a qualcuno...* e per quanto riguarda le interruzioni valgono di più il confronto ed il risultato che emerge da esso di quanto non valga la concentrazione un po' alienante dello sviluppatore isolato dal mondo... Ciò che si perde in termini di concentrazione si guadagna in termini di condivisione delle scelte e della conoscenza - nonché di socialità.

<strong class="emph">Marco</strong>: *Parlare ad alta voce rivela i limiti e i pregi di una scelta.* Ricordo che non tutto viene sempre discusso: si lascia la libertà di prendere delle decisioni in autonomia. Inoltre interrompere continuamente è sintomo di un cattivo modo di fare Pair. Per questo *è fondamentale che la coppia sia composta da due persone con seniority simile* altrimenti il rischio è che si finisca a scrivere codice sotto dettatura.

<strong class="emph">Umberto</strong>: Nel nostro caso le scelte architetturali/tecniche vengono prese a livello di team, poi le coppie procedono con gli sviluppi. Anche in questo caso conta molto “l’affiatamento” e il livello della coppia, molto dipende dalla persona con cui lavori... deve *capire quando può intervenire* e quando invece lasciarti “andare” per discuterne alla fine.

<div class="emphasis">
<h4>L’angolo del consiglio pratico</h4>
<ul>
<li>Formate <strong>coppie con seniority simili</strong> e invertite spesso i ruoli di driver e observer</li>
<li>L’affiatamento è importante: è fondamentale avere la giusta <strong>empatia per dosare le interruzioni</strong></li>
</ul>
</div>

## Pair e Remote Working
<p style="text-align:center; min-width:100%;">
    <img src="https://media.giphy.com/media/4uGeJzUSCKKeQ/source.gif" style="min-width:100%; height: auto;">
</p>
<cite>Quali sono le differenze tra il Pair “dal vivo” e “da remoto”?</cite>

<strong class="emph">Pietro</strong>: Come per molte altre cose, la versione dal vivo è molto più divertente di quella online e permette di "scambiare i ruoli" di osservato ed osservatore in modo più sistematico, semplice e rapido. Facendo Pair Programming dal vivo, generalmente, è molto più frequente che *l'osservatore prenda il controllo della tastiera* per esemplificare un pensiero scrivendo qualche riga di codice; nel Pair Programming remoto questo è molto più difficile e, di conseguenza, assai raro. L'unico vero punto dolente del fare Pair Programming è, per quanto mi riguarda, la necessità di interminabili call conference per la versione remota...

<strong class="emph">Marco</strong>: Mmmh, secondo me sono abbastanza simili. Io trovo che da remoto sia *più pesante per i mezzi* (es .cuffia ) *ma meglio per quanto riguarda le interruzioni e la comodità di vedere bene* quello che si scrive. Dal “vivo” meglio per quanto riguarda il contatto umano fatto di tutta quella parte di comunicazione non verbale che da remoto si perde.

<strong class="emph">Umberto</strong>: il Pair remoto presenta numerosi vantaggi: primo su tutti la *logistica*, ti permette di lavorare da qualsiasi posto tu voglia generando *meno inquinamento per l’ambiente ed ore passate in macchina*. Con l’attuale qualità video degli strumenti utilizzati per la condivisione, è come lavorare sul proprio pc quindi c’è un maggior comfort di spazio e di postura rispetto al Pair dal “vivo”.<br/>
Dal punto di vista delle relazioni il Pair remoto è più astratto, di conseguenza il rapporto con l’altra persona (al fine solo lavorativo) potrebbe risultare anche più produttivo rispetto ad un Pair dal “vivo”. Sicuramente avere a che fare con una persona dal vivo non è come lavorare con una “voce” dove effettivamente viene a mancare tutta la parte di comunicazione non verbale e “contatto”, come la *cara vecchia pacca sulla spalla per complimentarsi*.


<div class="emphasis">
<h4>L’angolo del consiglio pratico</h4>
<ul>
<li>Disporre di una <strong>buona connessione</strong></li>
<li>Utilizzare <strong>strumenti che rendano il lavoro in Pair “fluido”</strong> e senza intoppi perché in caso contrario potrebbe risultare estremamente frustrante. </li>
</ul>
</div>