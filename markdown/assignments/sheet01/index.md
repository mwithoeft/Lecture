---
title: "Blatt 01: Scanner und Parser"
author: "BC George, Carsten Gips (FH Bielefeld)"
hidden: true
weight: 1
---


Hier kommt der Inhalt für Blatt 01 hin ... allgemeine einleitende Worte ...

## Aufgabe 1: XYZ (2P)

tbd

## Aufgabe 2: XYZ (3P)

tbd



{{% challenges %}}
## Fahrstuhlgrammatik

<!-- XXX S. 157, EAC -->

<!-- XXX `s : s '(' s ')' s | ;` -->

Stellen Sie sich einen Fahrstuhl vor, der mit Hilfe von zwei Kommandos gesteuert wird:
$\uparrow$ bewegt den Fahrstuhl ein Stockwerk nach oben, $\downarrow$ bewegt den Fahrstuhl um ein
Stockwerk nach unten.

Gehen Sie weiter davon aus, dass das Gebäude beliebig hoch ist und der Fahrstuhl in Etage
$x$ startet.

Geben Sie eine LL(1)-Grammatik an, die beliebige Steuersequenzen erzeugt, die folgende
Nebenbedingungen einhalten:

1.  Der Fahrstuhl fährt nie tiefer als Etage $x$.
2.  Am Ende der Sequenz wird der Fahrstuhl wieder zur Etage $x$ gebracht.

Die leere Sequenz sei erlaubt.

\smallskip

**Beispiele**:

*   $\uparrow\uparrow\downarrow\downarrow$ und $\uparrow\downarrow\uparrow\downarrow$ sind gültige Sequenzen
*   $\uparrow\downarrow\downarrow\uparrow$ und $\uparrow\downarrow\downarrow$ sind keine gültigen Sequenzen

[Formulierung von LL-Grammatiken]{.thema}

{{% /challenges %}}