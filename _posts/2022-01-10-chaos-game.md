---
layout: post
current: post
cover: 'assets/images/post-cover/2022-chaos-game.jpg'
socialcover: 'assets/images/post-cover/2021-chaos-game-s.jpg'
slug: chaos-game
title: 'Il Gioco del Caos'
date: 2022-01-10 09:00:00
category : [tech]
tags: [software, functional, programming, howto]
author: [claudiol]
customStyle: 'assets/webgl/TemplateData/style.css'
---

<cite>Io come Dio non gioco ai dadi e non credo nelle coincidenze.</cite>

Avete presente il film del 2005 **“V per Vendetta”**, tratto dal romanzo a fumetti di Alan Moore? Veramente un bel film, ricco di ideali e spunti di riflessione, ne rimasi molto colpito la prima volta che lo vidi e confesso che di tanto in tanto mi torna in mente. Proprio come un po' di tempo fa. Ricordo che stavo scorrendo tra i post di Reddit quando mi imbattei per puro caso (...o forse no?) in un video riguardante il cosiddetto **“Chaos Game”**, dove numeri a caso, seguendo una semplice regola, disegnano sempre la stessa figura, evitando al tempo stesso una parte specifica del disegno. Smisi di scorrere i post e, incuriosito, decisi di approfondire l’argomento, mentre la frase pronunciata da V ad Evey pian piano mi riecheggiò in mente: *“Io come Dio non gioco ai dadi e non credo nelle coincidenze”*, che a sua volta è la famosa citazione di Einstein *“Dio non gioca a dadi”*.

<figure style="text-align:center"><img src="/assets/images/post-content/chaos-game/chaos-game_s_001.png" alt="Chaos game" /></figure>

E fu così che mi misi comodo, scivolando gradualmente in uno dei miei soliti pacati deliri e pensai: si può parlare di “casualità” in un mondo basato sul **determinismo**? Per definizione il caso è “qualcosa di non prevedibile e che non segue alcuno schema”. Ma esiste davvero qualcosa che può accadere senza causa-effetto?

Il lancio della monetina è l’esempio più classico: con un 50% di probabilità uscirà o “testa” o “croce”, ma fattori come la forza e l’inclinazione del tiro decreteranno il risultato finale che a questo punto non è più considerabile “casuale”. Stesso discorso vale per i computer, che possono eseguire due tipi di generazione di numeri casuale: uno attraverso un **processo fisico (HRNG)** e uno attraverso un **algoritmo deterministico**; il primo si basa su fenomeni ambientali (il rumore della ventola, per esempio) ed è più difficile, ma non impossibile, da prevedere rispetto a quello basato su un algoritmo (Math.random() di JavaScript o la classe Random di C#).

<figure style="text-align:center"><img src="/assets/images/post-content/chaos-game/chaos-game_s_002.png" alt="Chaos game" /></figure>

Un altro esempio di determinismo è quello descritto dal fisico **Stephen Wolfram** nel **libro A New Kind of Science**, dove interpreta l’universo come un grande automa cellulare, cioè una griglia di quadrati ad altezza e larghezza fissi che possono contenere un numero finito di elementi e ognuna di queste celle cambia stato (vivo, morto, cambia colore, cambia forma) in base a una regola “locale”; per esempio lo stato di una cella in un determinato momento può dipendere dalle celle circostanti e/o dallo stato stesso della cella in quell’istante. Il “Gioco della Vita”, sviluppato dal matematico inglese **John Conway**, si rifà proprio a questa interpretazione e si basa su tre regole:

*   **Sopravvivenza**: ogni elemento avente due o tre elementi vicini sopravvive alla generazione successiva.
*   **Nascita**: all’interno di una cella adiacente a tre elementi, nascerà un nuovo elemento nella generazione successiva;
*   **Morte**: ogni elemento con quattro o più elementi adiacenti morirà per sovrappopolamento (non comparirà nella generazione successiva). Ogni elemento avente uno o zero elementi adiacenti morirà per isolamento.

Basta cercare su Google **“Conway's Game of Life”** per veder comparire questa griglia ai lati dello schermo oppure lo si può provare in maniera interattiva qui: [https://playgameoflife.com/](https://playgameoflife.com/). L’universo quindi potrebbe basarsi su un automa cellulare?

<figure style="text-align:center"><img src="/assets/images/post-content/chaos-game/chaos-game_s_003.png" alt="Chaos game" /></figure>

Infine, voglio mostrarvi “Il Gioco del Caos”: attraverso numeri generati in maniera “casuale” si arriva a ottenere un pattern ben preciso. Tutto ciò che serve è un foglio, un dado, una penna e molto tempo a disposizione, per cui i coraggiosi che vogliono cimentarsi possono tranquillamente procedere; gli altri possono usare un programma che ho scritto appositamente per questo “esperimento”.

Le regole sono semplici: segnare tre punti senza un particolare ordine che formino un triangolo e associare ad ogni vertice (A, B, C) due numeri, in modo tale da coprire tutte le facce del dado e, infine, segnare un “Tracepoint” che sarà il nostro punto di partenza e che cambierà continuamente posizione (salvando tutte le precedenti):
<figure style="text-align:center"><img src="/assets/images/post-content/chaos-game/chaos-game_s_005.png" alt="Chaos game" /></figure>

Ad ogni lancio, partendo dall’ultimo Tracepoint, la posizione da segnare sarà il punto medio tra il Tracepoint e uno dei vertici A, B o C; se per esempio uscisse “2” dovrei segnare il punto medio tra A e Tracepoint:
<figure style="text-align:center"><img src="/assets/images/post-content/chaos-game/chaos-game_s_006.png" alt="Chaos game" /></figure>

Dopo molteplici iterazioni tra l’ultimo Tracepoint e uno dei vertici del triangolo si otterrà il famoso **frattale di Sierpiński**:
<figure style="text-align:center"><img src="/assets/images/post-content/chaos-game/chaos-game_s_007.png" alt="Chaos game" /></figure>

Qualsiasi posizione dei vertici o del Tracepoint finirà per generare quasi sempre la stessa immagine. Infine, non posso non citare il famoso frattale chiamato **"Barnsley fern"**, dove, mediante semplici regole e numeri generati a caso, viene creata una felce.

# Qui sotto potete provare in maniera interattiva il "Chaos game".
<div id="unity-container" class="unity-desktop">
  <canvas id="unity-canvas" width="960" height="600"></canvas>
  <div id="unity-loading-bar">
    <div id="unity-logo"></div>
    <div id="unity-progress-bar-empty">
      <div id="unity-progress-bar-full"></div>
    </div>
  </div>
  <div id="unity-mobile-warning">
    WebGL builds are not supported on mobile devices.
  </div>
  <div id="unity-footer">
    <div id="unity-webgl-logo"></div>
    <div id="unity-fullscreen-button"></div>
    <div id="unity-build-title">Chaos Game</div>
  </div>
</div>

<script>
  console.log('Qui');
  var buildUrl = "/assets/webgl/Build";
  var loaderUrl = buildUrl + "/Build.loader.js";
  var config = {
    dataUrl: buildUrl + "/Build.data",
    frameworkUrl: buildUrl + "/Build.framework.js",
    codeUrl: buildUrl + "/Build.wasm",
    streamingAssetsUrl: "StreamingAssets",
    companyName: "DefaultCompany",
    productName: "Chaos Game",
    productVersion: "0.1",
  };

  var container = document.querySelector("#unity-container");
  var canvas = document.querySelector("#unity-canvas");
  var loadingBar = document.querySelector("#unity-loading-bar");
  var progressBarFull = document.querySelector("#unity-progress-bar-full");
  var fullscreenButton = document.querySelector("#unity-fullscreen-button");
  var mobileWarning = document.querySelector("#unity-mobile-warning");

  // By default Unity keeps WebGL canvas render target size matched with
  // the DOM size of the canvas element (scaled by window.devicePixelRatio)
  // Set this to false if you want to decouple this synchronization from
  // happening inside the engine, and you would instead like to size up
  // the canvas DOM size and WebGL render target sizes yourself.
  // config.matchWebGLToCanvasSize = false;

  if (/iPhone|iPad|iPod|Android/i.test(navigator.userAgent)) {
    container.className = "unity-mobile";
    // Avoid draining fillrate performance on mobile devices,
    // and default/override low DPI mode on mobile browsers.
    config.devicePixelRatio = 1;
    mobileWarning.style.display = "block";
    setTimeout(() => {
      mobileWarning.style.display = "none";
    }, 5000);
  } else {
    canvas.style.width = "960px";
    canvas.style.height = "600px";
  }
  loadingBar.style.display = "block";

  var script = document.createElement("script");
  script.src = loaderUrl;
  script.onload = () => {
    createUnityInstance(canvas, config, (progress) => {
      progressBarFull.style.width = 100 * progress + "%";
    }).then((unityInstance) => {
      loadingBar.style.display = "none";
      fullscreenButton.onclick = () => {
        unityInstance.SetFullscreen(1);
      };
    }).catch((message) => {
      alert(message);
    });
  };
  document.body.appendChild(script);
</script>

<figure style="text-align:center"><img src="/assets/images/post-content/chaos-game/chaos-game_s_004.png" alt="Chaos game" /></figure>