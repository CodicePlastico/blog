---
layout: post
current: post
cover: 'assets/images/post-cover/20200826-web-component.jpg'
socialcover: 'assets/images/post-cover/20200826-web-component-s.jpg'
slug: libri-estate-2020
title: 'Web Components Lifecycle'
date: 2020-08-23 09:00:00
category : [tech]
tags: [frontend]
author: claudio
---


## Cosa sono i Web Components
L’ecosistema Javascript ormai è ricco di librerie o framework che abbracciano i concetti di componenti e moduli per lo sviluppo di applicazioni. Questi elementi vengono generalmente considerati come porzioni di codice (_html / css / js_) indipendenti, che possono essere riutilizzate e personalizzate tramite un set di api messo a disposizione.

I Web Components, allo stesso modo, sono una suite di funzionalità che permettono di creare elementi personalizzati la cui logica viene **incapsulata e separata** dal resto del codice. Il grande vantaggio offerto dai Web Components consiste nell’utilizzo di API offerte dal browser, il che rende questi componenti indipendenti a livello tecnologico ed integrabili in tutte le librerie.

L’evoluzione di Javascript e la maturità dei moderni browser stanno alimentando questa nuova tecnologia che ormai influenza gli strumenti principali per lo sviluppo di web applications.

## Ciclo di vita dei componenti

I Web Components, come per i componenti creati tramite moderne librerie o framework javascript, possono essere gestiti tramite una serie di **metodi** che vengono chiamati da eventi corrispondenti alle diverse fasi del ciclo di vita di un componente. Il concetto di ciclo di vita offre agli sviluppatori di poter usufruire di un sistema standard per la gestione dei componenti, dalla loro creazione alla distruzione, senza doverne creare uno.

Di seguito i metodi che possono essere implementati:

*   [constructor()](#constructor) viene invocato quando un componente viene istanziato;
*   [connectedCallback()](#connectedCallback) viene invocato quando un componente viene aggiunto al DOM del documento;
*   [disconnectedCallback()](#disconnectedCallback) viene invocato quando un componente viene rimosso dal DOM del documento;
*   [adoptedCallback()](#adoptedCallback) viene invocato quando componente viene spostato in un’altro documento;
*   [attributeChangedCallback(name, oldValue, newValue)](#attributeChangedCallback) viene invocato ogni volta che un attributo del componente viene aggiunto, rimosso o modificato. Quali attributi devono essere monitorati sono definiti nel metodo static observedAttributes; 

Il codice seguente mostra l’esempio di un custom element con tutti i metodi invocati dal ciclo di vita:
<br/>

```javascript
class MyCustomElement extends HTMLElement {
	static get observedAttributes() {
    return ["attr-1", "attr-2", "attr-3"];
  }

  constructor() {
    super();
	  //...code
  }
  
  connectedCallback() {
    //...code
  }
  
  disconnectedCallback() {
    //...code
  }
  
  attributeChangedCallback(name, oldValue, newValue) {
    //...code
  }
  
  adoptedCallback() {
    //...code
  }
}
```
<br/>
<br/>
<p style="text-align:center"><img src="/assets/images/post-content/sep-1.png" alt="sep"/>
</p>
<br/><br/>

### constructor() {#constructor}

Il metodo ``constructor`` viene invocato quando l’oggetto, in questo caso un custom element, viene istanziato. In Javascript questa funzione non deve essere considerata una funzione come le altre ma come un metodo speciale che viene invocato per inizializzare un oggetto come classe.
<br/>

```javascript
const myCustomElement = document.createElement("my-custom-element");
```
<br/>
Quando si utilizza questo metodo è necessario richiamare come prima cosa il metodo super() senza passare alcun parametro, questo per permettere che la catena di ereditarietà delle proprietà e dei metodi con la classe padre funzioni correttamente.
<br/>

```javascript
class MyCustomElement extends HTMLElement {
	constructor() {
    super();
	  // ...code
  }
}
```

La cosa fondamentale da ricordare è che quando ci si trova all’interno di questo metodo il componente **non è ancora presente nel DOM del documento**, quindi non può ancora interagire con esso. Questo vuol dire che non si ha accesso a tutti quei metodi e quelle proprietà che sono legate in qualche modo al DOM. Se provassimo, ad esempio, a modificare l’attributo ``innerHTML`` otterremmo un errore che informa lo sviluppatore che l'elemento è stato creato ma che non è ancora pronto ad avere un contenuto.
<br/>

```shell
Uncaught DOMException: 
Failed to construct 'CustomElement': The result must not have children
```

<br/>
Esiste un caso nel quale possiamo inizializzare la visualizzazione grafica del componente nel metodo constructor ed è quando si utilizzano le **Shadow DOM API**. Con questa funzionalità viene creato un **mini DOM separato che vive esclusivamente all'interno del componente**, in questo caso lo Shadow DOM diviene disponibile non appena il componente viene creato.

Il metodo ``constructor`` può essere utilizzato per tutti quegli usi che non hanno impatto sulle proprietà di visualizzazione del componente. Si può ad esempio instanziare tutte le proprietà che si desidera utilizzare o recuperare preventivamente i dati necessari da sorgenti esterni per averle subito a disposizione una volta che il componente verrà inserito nel DOM.

Inoltre la sua posizione nel codice, generalmente in alto, aiuta a garantire un **buon grado di leggibilità** presentando immediatamente le proprietà del componente.
<br/>

<br/>
<br/>
<p style="text-align:center"><img src="/assets/images/post-content/sep-2.png" alt="sep"/>
</p>
<br/><br/>

### connectedCallback() {#connectedCallback}

_Il metodo connectedCallback dovrebbe contenere la logica necessaria alla presentazione visiva del componente._

Ogni volta che un elemento viene agganciato al DOM del documento, il metodo viene invocato: ora possiamo utilizzare tutti quei metodi e proprietà che ne definiscono il suo layout e i comportamenti. Con la certezza che il DOM sia disponibile si può definire il **template** attraverso la proprietà innerHTML, aggiungere **eventi** e recuperare **dati** da sorgenti esterne o dagli attributi del componente.<br/>


```javascript
class MyCustomElement extends HTMLElement {
  connectedCallback() {
    console.log("[MyCustomElement] connected");
  }
}

customElements.define("my-custom-element", MyCustomElement);

const myCustomElement = document.createElement("my-custom-element");
document.body.appendChild(myCustomElement);
// result: [MyCustomElement] connected
```

<br/>
Facendo uso del paradigma ad oggetti ``this`` si fa riferimento al componente stesso e quindi si può accedere velocemente a tutte le sue proprietà e metodi.


<br/>
<br/>
<p style="text-align:center"><img src="/assets/images/post-content/sep-3.png" alt="sep"/>
</p>
<br/><br/>

### disconnectedCallback() {#disconnectedCallback}

_Il metodo disconnectedCallback permette di fare pulizia nel componente quando giunge alla conclusione della sua vita._

Questo metodo, in opposizione a ``connectedCallback``, viene invocato appena prima che un componente venga rimosso dal DOM. L’utilizzo di questo metodo è **opzionale**, ad esempio potrebbe essere ignorato nel caso in cui sapessimo che un componente non sarà mai rimosso dal DOM.<br/>

```javascript
class MyCustomElement extends HTMLElement {
  disconnectedCallback() {
    console.log("[MyCustomElement] disconnected");
  }
}

customElements.define("my-custom-element", MyCustomElement);

document.querySelector("my-custom-element").remove();
// result: [MyCustomElement] disconnected
```

<br/>
Javascript dispone di un suo _Garbage Collector_ che potrebbe darci la **falsa illusione che questo metodo non sia utile**. Anche se questa procedura viene effettuata automaticamente questo metodo ci dà la possibilità di intervenire in maniera più accurata per ripristinare e valorizzare a ``null`` qualsiasi variabile che non sia più necessaria o che faccia riferimento ad altri oggetti.

Un esempio in cui il _Garbage Collector_ dimostra i suoi limiti è quella di un componente che utilizzi un contatore tramite un ``setInterval``. Quando il componente viene rimosso infatti il contatore persiste e prosegue nella sua attività benché l’elemento sia stato rimosso dal DOM. La soluzione è intervenire manualmente con ``disconnectedCallback`` ed eliminare la referenza al setInterval che è stata creata.<br/>

```javascript
class TimerCustomElement extends HTMLElement {
  constructor() {
    super();

    this.counter = 0;
  }

  connectedCallback() {
    this.timer = setInterval( () => this.update(), 1000);
  }

  update() {
    this.innerHTML = this.counter;
    this.counter++;
    console.log(this.counter);
  }
}

customElements.define("timer-custom-element", TimerCustomElement);
// result: 1
// result: 2
// result: 3

document.querySelector("timer-custom-element").remove();
// result: 4
// result: 5
// result: 6
// ...
```
<br/>
In questa situazione l’unica soluzione è aggiungere alla classe il metodo disconnectedCallback:
<br/>

```javascript
disconnectedCallback() {
  clearInterval(this.timer);
}
```

<br/>
<br/>
<p style="text-align:center"><img src="/assets/images/post-content/sep-1.png" alt="sep"/>
</p>
<br/><br/>


### adoptedCallback() {#adoptedCallback}

_Il metodo adoptedCallback() viene invocato quando un componente viene spostato in diverso documento._

A differenza del metodo ``disconnectedCallback``, il cui uso dovrebbe essere incentivato per migliorare i componenti ed il nostro codice, il metodo ``adoptedCallback ``è più criptico e prevede **un caso abbastanza insolito**.

Il metodo viene infatti invocato **quando il componente viene spostato da un documento ad un altro**. Si tratta di una situazione inusuale e che suscita qualche perplessità, ma proviamo ad utilizzare un esempio.

Generalmente i progetti sui quali siamo abituati a lavorare presentano un singolo documento, ma esiste la possibilità di avere più documenti in pagina lavorando con degli _iframe_. In questo caso, ogni iframe della pagina sarebbe da considerarsi un documento a se stante e sarebbe quindi possibile **spostare un componente da un iframe all’altro**.

Semplicemente utilizzando il riferimento ad un componente è possibile spostarlo da un documento all’altro:<br/>

```javascript
const frame = document.getElementsByTagName("iframe")[
const el = frame.contentWindow.document.getElementsByTagName("my-custom-element")[0];
const adopted = document.adoptNode(el);
```
<br/>
Anche se ormai gli applicativi o i layout costituiti da iframe sono sempre più rari se ci si trovasse in questa condizioni si potrebbe utilizzare questo metodo.

Bisogna tuttavia fare attenzione perché nel caso di spostamento di un componente in un nuovo documento il ciclo di vita sarà :

```javascript
disconnectedCallback() --> adoptedCallback() --> connectedCallback()
```
<br/>

<br/>
<br/>
<p style="text-align:center"><img src="/assets/images/post-content/sep-2.png" alt="sep"/>
</p>
<br/><br/>

### attributeChangedCallback() {#attributeChangedCallback}

_Il metodo attributeChangedCallback() viene invocato ogni volta che il valore di uno degli attributi del componente viene modificato._

Un **web component può essere parametrizzato** attraverso l’uso di proprietà che possono essere passate come attributi del tag html:<br/>

```html
<my-custom-element
  attr-1="xxx"
	attr-2="yyy"
	attr-3="zzz"
></my-custom-element>
```
<br/>
Ed al caricamento del componente possono essere lette in questo modo:
<br/>
```javascript
class MyCustomElement extends HTMLElement {
  connectedCallback() {
    const prop1 = this.getAttribute("attr-1"); // xxx
    const prop2 = this.getAttribute("attr-2"); // yyy
    const prop3 = this.getAttribute("attr-3"); // zzz
  }
}
```
<br/>
Il codice di esempio, però, ci permette di **leggere il valore dei parametri solo quando il componente viene aggiunto al DOM**, ``attributeChangedCallback`` invece viene invocato ogni volta che un attributo viene eliminato, aggiungo o modificato. Questo metodo ci restituisce tre parametri che nell’ordine sono: il **nome** dell’attributo, il **valore precedente** e il **nuovo valore**.<br/>

```javascript
class MyCustomElement extends HTMLElement {
  //...
  static get observedAttributes() {
    return ["attr-1", "attr-2", "attr-3"];
  }

  attributeChangedCallback(attrName, oldValue, newValue) {
    console.log(attrName, oldValue, newValue);
  }
}
```
<br/>
**Attenzione!** Il parametro del nome dell’attributo viene **convertito in minuscolo** pertanto, nel caso si fosse utilizzato un nome con lettere maiuscole (ad esempio in formato camel case), bisognerà prestare attenzione nel caso si effettui una **condizione di eguaglianza sul nome di quella variabile** poichè il contenuto potrebbe non essere quello che ci si aspetta. Per evitare confusione il consiglio è sempre quello di **utilizzare per gli attributi di un componente nomi in minuscolo e separati da trattini** (es. ``custom-attribute="value"``).

Un’altra caratteristica da ricordare è fatto che il metodo ``attributeChangedCallback`` **viene invocato anche al caricamento del componente se sono presenti degli attributi**. Questo perché il valore dell’attributo viene effettivamente modificato, passando da ``null`` al valore che viene impostato nell’html.

Un componente personalizzato può avere un **elevato numero di potenziali attributi personalizzati che lo definiscono**, ad esempio come minimo avrà un attributo class o un id. Rimanere in ascolto del cambiamento di ognuno di essi potrebbe causare l’esecuzione di **un'enorme quantità di codice non necessario**, ad ogni modifica verrebbe infatti invocato il metodo ``attributeChangedCallback``.

Nella prima versione delle api dei Web Componente (**v0**) accadeva proprio questo. Per ovviare il problema, nella seconda versione (**v1**) è necessario specificare di quali attributi si vuole rimanere in ascolto tramite il metodo statico ``observedAttributes``.

<br/>
<br/>
<p style="text-align:center"><img src="/assets/images/post-content/sep-2.png" alt="sep"/>
</p>
<br/><br/>

### observedAttributes() {#observedAttributes}

Per poter definire la lista degli attributi da monitorare è sufficiente definire un metodo static chiamato ``observedAttributes`` che restituisce un **array di stringhe corrispondente ai valori dei nomi degli attributi**.

Prima che il metodo ``attributeChangedCallback`` venga invocato viene controllato se il suo nome risulta nella lista restituita da ``observedAttributes`` , in caso positivo il metodo viene invocato, viceversa non accade nulla.

## Conslusioni

I metodi del ciclo di vita di un componente ci **semplificano lo sviluppo** perché ci forniscono un grande **controllo sui comportamenti dei nostri componenti** senza doverne creare uno manualmente. Infine, il grande vantaggio dei Web Components è di essere una tecnologia **già disponibile per la maggioranza dei browser** ed integrabile con le principali libreria javascript o progetti legacy. 

Non rimane che provare!


#### Riferimenti
 <ul>
 <li><a href="https://developer.mozilla.org/en-US/docs/Web/Web_Components" target="_blank">Web Components | MDN</a>
 <li><a href="https://www.manning.com/books/web-components-in-action" target="_blank">Manning | Web Components in Action</a>



