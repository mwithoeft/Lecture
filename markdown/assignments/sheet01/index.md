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
## $\operatorname{First}_2$ - und $\operatorname{Follow}_2$
Berechnen Sie die $\operatorname{First}_2$ - Mengen von allen rechten Seiten der Produktionen und die $\operatorname{Follow}_2$ - Mengen von allen Nichtterminalen der folgenden Grammatik:

$S\ \ \rightarrow \ \boldsymbol{if}\ \ E \ \ S\  \ \boldsymbol{else}\  \ S\ \ \ \vert \ \ \boldsymbol{\{} \ \ S \ \ R \ \ \vert \ \ \boldsymbol{id\ =} \ \ E$\phantom{m}

$R\ \ \rightarrow\ \ \boldsymbol{\}}\ \ \mid\ \ \boldsymbol{;}\ \ S\ \ R$

$E\ \ \rightarrow\ \ \boldsymbol{num}\ \ \mid\ \ \boldsymbol{id}$

[Berechnen von First- und Follow-Mengen]{.thema}

## $\operatorname{LL(1)}$-Sprachen
Ist die Sprache $L = {a^mcb^{m}}$ LL(1)?
Beweisen Sie Ihre Behauptung.

[Anwenden der Definitionen von First- und Follow-Mengen]{.thema}

## Reduzierte kontextfreie Grammatiken
Entwickeln Sie einen Algorithmus zur Reduzierung einer kontextfreien Grammatik.

[Verständnis reduzierter CFG]{.thema}

## $LL(1)$ - Grammatiken
Formen Sie die folgende Grammatik in eine äquivalente LL(1)-Grammatik um:

$S\ \rightarrow\ A\ T \mid T$

$A\ \rightarrow \ S\ \ \boldsymbol{+}\ \ \mid \ \boldsymbol{a}\ \boldsymbol{+}$

$B\ \rightarrow\ \boldsymbol{a}\ B\ \mid\ T$

$T\ \rightarrow\ T \ \ \boldsymbol{*}\ \ F\ \mid\ F$

$F\ \rightarrow\ L\ E\ \boldsymbol{)}\ \mid \ \boldsymbol{a}\ \mid\ L\ \boldsymbol{a}\ \boldsymbol{)}$

$L\ \rightarrow\ \boldsymbol{(}$

[Äquivalenzumformungen von Grammatiken]{.thema}

## Top-Down-Parsing
Erzeugen Sie die Parsertabelle für die Grammatik aus Aufgabe 4 und parsen Sie (auf dem Papier) das Wort $a + a + a$ mit einem PDA.

[Anwendung von Top-Down-Parsing-Verfahren]{.thema}

## Nicht-$LL(1)$ - Grammatiken
Entwickeln Sie eine Grammatik, die nicht $LL(1)$ ist. Ist sie $LL(k)$ für ein $k > 1$?

[Verständnis der $LL(k)$-Eigenschaft von Grammatiken]{.thema}
{{% /challenges %}}
