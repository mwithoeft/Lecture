---
title: "Blatt 03: Optimierung"
author: "BC George (FH Bielefeld)"
hidden: true
weight: 3
---


Hier kommt der Inhalt für Blatt XYZ hin ... allgemeine einleitende Worte ...

## Aufgabe 1: XYZ (2P)

tbd

## Aufgabe 2: XYZ (3P)

tbd



{{% challenges %}}
## Syntaxgerichtete Übersetzungsverfahren

*   Erstellen Sie ein SDT, das arithmetische Ausdrücke von der Infix- in die Präfix-Notation übersetzt.
*   Erstellen Sie ein SDT, das ganze Zahlen in römische Zahlen übersetzt und umgekehrt.
*   Betrachten Sie die Produktionsregel `a : b c d ;`. Jedes der Nicht-Terminalsymbole `a`, `b`, `c` und
    `d` hat zwei Attribute `s` und `i`, wobei `s` ein berechnetes und `i` ein geerbtes Attribut sei.
    Welche der folgenden semantischen Regeln sind mit einer S-attributierten SDD, mit einer L-attributierten
    SDD oder mit jeder beliebigen Auswertungsreihenfolge verträglich?
    1.  `a.s = b.i + c.s`
    2.  `a.s = b.i + c.s` und `d.i = a.i + b.s`
    3.  `a.s = b.s + d.s`
    4.  `a.s = d.i`, `b.i = a.s + c.s`, `c.i = b.s` und `d.i = b.i + c.i`
{{% /challenges %}}
