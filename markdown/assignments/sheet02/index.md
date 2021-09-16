---
title: "Blatt 02: Semantische Analyse"
author: "Carsten Gips (FH Bielefeld)"
hidden: true
weight: 2
---


Hier kommt der Inhalt für Blatt XYZ hin ... allgemeine einleitende Worte ...

## Aufgabe 1: XYZ (2P)

tbd

## Aufgabe 2: XYZ (3P)

tbd



{{% challenges %}}

## Allgemeines

Betrachten Sie folgenden Java-Code:

1. Umkreisen Sie alle Symbole.
2. Zeichen Sie Pfeile von Symbol-Referenzen zur jeweiligen Definition (falls vorhanden).
3. Identifizieren Sie alle benannten Scopes.
4. Identifizieren Sie alle anonymen Scopes.
5. Geben Sie die resultierende Symboltabelle an (Strukturen wie besprochen).

```java
package a.b;

import u.Y;

class X extends Y {
    int f(int x) {
        int x,y;
        { int x; x - y + 1; }
        x = y + 1;
    }
}

class Z {
    class W extends X {
        int x;
        void foo() { f(34); }
    }
    int x,z;
    int f(int x) {
        int y;
        y = x;
        z = x;
    }
}
```
<!-- nach https://github.com/parrt/cs652/blob/master/labs/def-ref.md -->


## Symboltabellen für Lox

Erweitern Sie die beiden Lox-Parser um die diskutierten Symboltabellen. Reichen die besprochenen
Strukturen? Falls nicht, was wurde nicht berücksichtigt und wie müssen die Änderungen aussehen?

Implementieren Sie auf Basis dieser Symboltabellen und des von den Parsern generierten AST erste
einfache semantische Prüfungen für Lox:

*   Namen müssen vor dem Benutzen deklariert sein (im Sinne von
    [Lox](https://www.craftinginterpreters.com/statements-and-state.html#global-variables): Deklaration == Definition)
*   Variablen dürfen mehrfach deklariert werden
*   Funktionen dürfen nur einmal definiert werden
*   Namen müssen beim (lesenden/schreibenden) Zugriff definiert und sichtbar sein (im aktuellen Scope oder
    auflösbar in einem der Elternscopes)

Implementieren Sie darüber hinaus Type-Checking für Lox (wie in einer späteren Sitzung diskutiert).

[Einsatz von Symboltabellen und Ansätze zum Type-Checking]{.thema}


## Symboltabellen in Python

Schauen Sie sich [Lib/symtable.py](https://github.com/python/cpython/blob/3.8/Lib/symtable.py) als
[Schnittstelle zur Python-internen Symboltabelle](https://docs.python.org/3/library/language.html)
an.

Wie sind die Symboltabellen in Python aufgebaut? Wie werden sie eingesetzt? Überlegen Sie sich
Beispiele, an denen Sie die Nutzung der Symboltabellen in Python demonstrieren können.

[Code-Analyse von Symboltabellen aus "echten" Projekten]{.thema}

{{% /challenges %}}
