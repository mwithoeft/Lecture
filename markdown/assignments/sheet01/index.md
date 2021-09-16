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
## LL-Parser

*   Wie kann man aus einer LL(1)-Grammatik einen LL(1)-Parser mit rekursivem
    Abstieg implementieren? Wie "übersetzt" man dabei Token und Regeln?
*   Wie geht man mit Alternativen um? Wie mit optionalen Subregeln?
*   Warum ist Linksrekursion i.A. bei LL-Parsern nicht erlaubt? Wie kann man
    Linksrekursion beseitigen?
*   Wie kann man Vorrangregeln und unterschiedliche Assoziativität implementieren?
*   Wann braucht man mehr als ein Token Lookahead? Geben Sie ein Beispiel an.


## ANTLR

Betrachten Sie folgenden Code-Schnipsel:

```
let fibonacci = fn(x) {
  if (x == 0) {
    return 0;
  } else {
    if (x == 1) {
      return 1;
    } else {
      fibonacci(x - 1) + fibonacci(x - 2);
    }
  }
};

let wuppie = fibonacci(4);
```

Erstellen Sie für diese fiktive Programmiersprache einen LL-Parser in ANTLR.


## Implementierung von LL-Parsern

<!-- XXX S. 68 Drachenbuch -->

Erstellen Sie für folgende Grammatiken **manuell** je einen passenden LL-Parser:

1.  `s : '+' s s | '-' s s | A ;`
2.  `s : '0' s '1' | '0' '1' ;`

::: showme
Tipp: `a : ax | y ;` (wobei `x` und `y` nicht mit `a` beginnen) wird umgeformt zu

```
a : yr;
r : xr | ;
```
:::

[Manuelle Implementierung von LL-Parsern]{.thema}


## Real-World-Parser mit ANTLR: Programmiersprache Lox

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

Erstellen Sie für diese fiktive Sprache einen Parser mit ANTLR. Die genauere Sprachdefinition finden Sie unter
[craftinginterpreters.com/the-lox-language.html](https://www.craftinginterpreters.com/the-lox-language.html).

[Erstellen eines Parsers für eine Programmiersprache mit ANTLR]{.thema}

{{% /challenges %}}
