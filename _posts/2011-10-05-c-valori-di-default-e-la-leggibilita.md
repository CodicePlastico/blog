---
layout: post
title:  "C#, valori di default e la leggibilità"
comments: true
categories: tech
tags: [c#]
---


La possibilità di definire i [valori di default dei parametri di un metodo](http://msdn.microsoft.com/en-us/library/dd264739(VS.100).aspx) con C#, non è cosa recente, ma giusto ieri mi sono trovato ad utilizzare questa &#8220;feature&#8221; del linguaggio e a trarne alcune considerazioni.

Scenario:
descrivere uno snapshot della produttività di una macchina di un&#8217;automazione industriale dettagliandone, oltre al giorno a cui si riferisce lo snapshot stesso, tutti i relativi parametri:

- 
macchina accesa

- 
macchina pronta

- 
macchina in lavorazione automatica

- 
macchina in lavorazione manuale

- 
macchina in allarme

- 
macchina in assistenza



A fronte dei valori specificati da tali parametri viene di conseguenza calcolata l&#8217;efficienza &#8220;teorica&#8221; ed effettiva della macchina (logiche che non ci interessano per ora).

Utilizzare l&#8217;object initializer era fuori discussione: lo snapshot è un value-object del mio dominio e pertanto deve essere immutable. Non ho trovato altra strada che utilizzare il costruttore ed esporre i valori precedenti con delle properties in read-only.
L&#8217;implementazione &#8220;tradizionale&#8221; (pre default-parameter, per intenderci) non mi piaceva per nulla.

```
public class Snapshot
{ 
    public Snapshot(DateTime day, Int32 on, Int32 ready, Int32 working, 
                                  Int32 manual, Int32 alarm, Int32 service) 
    { 
        ... 
    } 
}

```

Dover specificare tutti i parametri, anche quelli con il valore 0, è molto prolisso, poco leggibile e, alla lunga, anche poco usabile: ogni volta mi devo ricordare (è vero ci sono l&#8217;intellisense e [resharper](http://www.jetbrains.com/resharper/) che mi danno una mano) l&#8217;ordine con cui inserire i parametri.

In questo contesto l&#8217;utilizzo dei valori di default è stato illuminante:

```
public class Snapshot
{
    public Snapshot(DateTime day, 
                    Int32 on=0, Int32 ready=0, Int32 working=0, 
                    Int32 manual=0, Int32 alarm=0, Int32 service=0)
    {
        ...
    }
}

```

in modo da poter scrivere:

```
Snapshot snapshot = new Snapshot(DateTime.Today, working: 4800, manual: 2400);

```

Questa sintassi è sicuramente più concisa e molto, ma molto più leggibile (ovviamente IMHO)

