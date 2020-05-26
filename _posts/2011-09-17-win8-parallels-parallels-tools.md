---
layout: post
title:  "Win8, parallels e i parallels tools"
comments: true
categories: tech
tags: [windows, microsoft]
---


Oggi sono riuscito finalmente a trovare un po&#8217; di tempo per installare [windows8](http://msdn.microsoft.com/en-us/windows/apps/br229516).

Leggendo blog, articoli e tweet sembra che su macosx, [virtual box](http://www.virtualbox.org/) sia la soluzione migliore, o meglio&#8230;una soluzione che non crea problemi. Utilizzando però [parallels](http://www.parallels.com/) sul mio mac per le virtualizzazioni di tutti i giorni, ho voluto evitare di installare un nuovo sistema di virtualizzazione, procedendo quindi a creare una nuova e fiammante macchina virtuale di win8 sfruttando parallels.

L&#8217;installazione è filata via liscia come l&#8217;olio, nessun problema, nessuna necessità particolare. Dopo pochi minuti avevo la mia VM pronta per l&#8217;uso.

I problemi hanno iniziato a verificarsi dopo l&#8217;installazione dei parallels tools. Problemi non da poco direi: black screen e impossibilità a fare qualsiasi cosa.
&#8220;Googlando&#8221; un po&#8217; sono riuscito a trovare sui forum di parallels la soluzione:

- 
shutdown della macchina virtuale

- 
configure > hardware > boot order

- 
nella sezione &#8216;boot flags&#8217; inserire: devices.video.pci_device_id=0x5005;

- 
riavviare la macchina virtuale



&#8230;good run on &#8216;metro&#8217;&#8230;

