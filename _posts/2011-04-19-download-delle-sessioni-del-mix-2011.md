---
layout: post
title:  "Download delle sessioni del MIX 2011"
comments: true
categories: tech

---


Perfetto&#8230;tutte le sessioni del [MIX](http://www.microsoft.com/events/mix/) sono online, ma come fare per farne il download, in modalità silent?

Partendo da questo [post](http://www.hanselman.com/blog/Mix11VideosDownloadThemAllWithRSS.aspx) di [Scott Hanselman](http://www.hanselman.com/blog/) mi sono scritto una piccola applicazione console per avere la possibilità di scaricare in modalità batch tutte le sessioni.

Trovate l&#8217;applicazione (bin e src) in allegato. Basta eseguirla dal prompt specificando la tipologia della risorsa che si vuole scaricare e la folder in cui si vuole scaricare il materiale. I possibili valori per il primo parametro sono:

- 
mp4

- 
mp4high

- 
wmv

- 
wmvhigh

- 
mp3



Esempio: MIX2011 mp4 &#8220;d:downloadsmix2011&#8221;

L&#8217;applicazione, partendo dalla folder specificata, crea una struttura con n sub-folder dove il nome è il codice della sessione. Ogni sub-folder contiene:

- 
abstract.html

- 
title.txt

- 
[codice sessione].wmv (o mp3 o mp4 a seconda della scelta fatta)



Il codice è scritto in poco più di dieci minuti, quindi non c&#8217;è nessun controllo del caso (ovvero specificate i parametri in modo corretto altrimenti &#8220;nisba&#8221;) e&#8230;non mi assumo nessuna responsabilità se vi si dovesse formattare il disco al termine del download :-)

[bin.zip (3.32 kb)](http://blog.codiceplastico.com/melkio/file.axd?file=2011%2f4%2fbin.zip)

[source.zip (2.62 kb)](http://blog.codiceplastico.com/melkio/file.axd?file=2011%2f4%2fsource.zip)

