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
## Flex

*   Wie kann man in Flex Inselgrammatiken ausdrücken?
*   Wie kann man in Flex non-greedy Verhalten von Regeln erreichen?

## Real-World-Lexer mit Flex: Programmiersprache Lox

<!-- XXX Fibonacci aus Monkey übersetzt nach Lox -->

Betrachten Sie folgenden Code-Schnipsel in der Sprache
["Lox"](https://www.craftinginterpreters.com/the-lox-language.html):

```
fun fib(x) {
    if (x == 0) {
        return 0;
    } else {
        if (x == 1) {
            return 1;
        } else {
            fib(x - 1) + fib(x - 2);
        }
    }
}

var wuppie = fib(4);
```

Erstellen Sie für diese fiktive Sprache einen Lexer mit Flex. Die genauere Sprachdefinition finden Sie unter
[craftinginterpreters.com/the-lox-language.html](https://www.craftinginterpreters.com/the-lox-language.html).


## Flex Code-Analyse

Betrachten Sie die Flex-Definitionen zum Scannen der Sprache C im
[cdecl-Projekt](https://github.com/ridiculousfish/cdecl-blocks/blob/master/cdlex.l)
und im
[splint-Projekt](https://github.com/splintchecker/splint).

Erklären Sie jeweils die Flex-Definitionen.^[Möglicherweise verstehen Sie manche Konstrukte erst nach der
Sitzung ["Parser: Bison"](cb_bison.html), wo wir über Bison sprechen werden ...] Vergleichen Sie diese und
diskutieren Sie mögliche Unterschiede.

[Code-Analyse von Flex-Scannern aus "echten" Projekten]{.thema}

{{% /challenges %}}