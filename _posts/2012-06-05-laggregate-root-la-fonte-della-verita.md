---
layout: post
title:  "L'aggregate-root: la fonte della verità"
comments: true
categories: tech
tags: [metodologie, ddd, community]
---


Quando si parla di DDD ogni componente in gioco ha la sua importanza, ma la &#8220;visione d&#8217;insieme&#8221; e il contenuto informativo contenuto in un aggregate ricopre un ruolo fondamentale di tutto l&#8217;ecosistema.

&#8220;A collection of objects that are bound together by a root entity, otherwise known as an aggregate root. The aggregate root guarantees the consistency of changes being made within the aggregate by forbidding external objects from holding references to its members.&#8221; (fonte: wikipedia)

Partiamo da ciò che un aggregate non è, in modo da chiarire subito la strada da non intraprendere :-)
<div><script src='https://gist.github.com/2874669.js'></script>
<noscript><pre><code></code></pre></noscript></div>


Con un&#8217;implementazione di questo tipo, abbiamo in prima battuta una &#8220;piacevolissima&#8221; sensazione di navigabilità, che ci permette, partendo potenzialmente da ogni punto, di raggiungere qualsiasi foglia del grafo.

Questo però vìola le due caratteristiche che un aggregate-root deve avere:

- 
garantire la consistenza dell&#8217;aggregato

- 
evitare l&#8217;accesso a nodi figli



&#8230;e se ci pensate bene anche alcuni dei principi dell&#8217;OOP, per esempio l&#8217;incapsulamento. Con un implementazione di questo genere abbiamo potenzialmente scoperto il fianco ad &#8220;attacchi&#8221; di ogni tipo, nel senso che chiunque abbia accesso ai nostri oggetti ne può manipolare lo stato, partendo da ogni punto.

In base a quanto ci siamo detti nei post precedenti, un domain model senza behaviors è un domain model monco&#8230;o meglio non è un domain model :-)

Prendiamo uno use-case &#8220;reale&#8221; che ci permetta di capire, quando parlo di behaviors, a cosa mi riferisco e come viene modificata l&#8217;implementazione di conseguenza:

- quando un utente (fisico o virtuale) del sistema modifica la sede operativa di un&#8217;azienda deve essere verificata la corretta associazione alla filiale della banca di riferimento secondo algoritmi, potenzialmente modificabili dinamicamente. Fino a che la nuova banca non ha confermato l&#8217;autorizzazione ad operare, l&#8217;azienda deve entrare in un limbo che non permetta operatività sull&#8217;azienda.


Con un &#8220;domain model anemico&#8221; ed in totale assenza di aggregate-root non abbiamo modo di garantire la consistenza dello stato dell&#8217;oggetto azienda e la conoscenza dei processi di business che vengono veicolati dalla nostra applicazione sono sparpagliati qua e là in &#8220;strati&#8221; diversi rispetto a quello che per me è l&#8217;owner assoluto di questo genere di competenze: il domain layer.

Rifattorizziamo l&#8217;object model in modo da enfatizzare il comportamento e incapsulare lo stato (due piccioni con una fava):
<div><script src='https://gist.github.com/2875460.js'></script>
<noscript><pre><code></code></pre></noscript></div>


In questo modo chiunque voglia cambiare la sede operativa di una particolare azienda, deve passare per forza attraverso le nostre API e da quel punto in poi, il cambio di stato interno, l&#8217;interazione con altre componenti del sistema, la consistenza del &#8220;grafo&#8221; stesso è demandata all&#8217;aggregate-root in oggetto: l&#8217;unica fonte della verità del nostro dominio.

Attenzione, non è tutto oro quello che luccica, o meglio è &#8220;oro&#8221;, ma non è così facile da raggiungere: &#8220;chiudere&#8221; lo stato internamente comporta una pesante rivisitazione di diversi ambiti in cui, con il modello anemico, ci eravamo ormai assestati (vedi persistenza, unit test&#8230;).
Ma con un po&#8217; di pazienza (e tempo) vediamo tutto!

P.S. a tutti quelli che mi stanno mandando mail, interessati dai post precedenti, innanzitutto grazie per i complimenti :-) Vi chiedo di spostare/postare le vostre interessanti domande nel luogo più appropriato: [il forum di guisa](http://www.guisa.org/forums/). Non perchè io non voglia rispondervi, anzi&#8230;ma perchè così apriamo la conversazione ad altre menti geniali (e che sia chiaro: non è che io mi reputi tale&#8230;).

