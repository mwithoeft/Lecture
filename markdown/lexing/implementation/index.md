---
type: lecture-cg
title: "Lexer: Implementierungen"
author: "Carsten Gips (FH Bielefeld)"
weight: 2
readings:
  - key: "Aho2008"
    comment: " Abschnitt 2.6 und Kapitel 3"
  - key: "Torczon2012"
    comment: "Kapitel 2"
  - key: "Mogensen2017"
    comment: "Kapitel 1 (insbesondere Abschnitt 1.8)"
quizzes:
  - link: XYZ
    name: "Testquizz (URL from `'`{=markdown}Invite more Players`'`{=markdown})"
assignments:
  - topic: blatt01
youtube:
  - id: XYZ (ID)
  - link: https://youtu.be/XYZ
    name: "Use This As Link Text (Link from `'share'`{=markdown}-Button)"
fhmedia:
  - link: https://www.fh-bielefeld.de/medienportal/m/XYZ
    name: "Use This As Link Text (Link from `'share'`{=markdown}-Button)"
---


## Motivation
Lorem Ipsum. Starte mit H2-Level.
...

## Lexer: Erzeugen eines Token-Stroms aus einem Zeichenstrom

\vspace{-20mm}

[Aus dem Eingabe(-quell-)text]{.notes}

```c
/* demo */
a= [5  , 6]     ;
```

[erstellt der Lexer (oder auch Scanner genannt) eine Sequenz von Token:]{.notes}

\pause
\bigskip

```
<ID, "a"> <ASSIGN> <LBRACK> <NUM, 5> <COMMA> <NUM, 6> <RBRACK> <SEMICOL>
```

::: notes
*   Input: Zeichenstrom (Eingabedatei o.ä.)
*   Verarbeitung: Finden sinnvoller Sequenzen im Zeichenstrom ("Lexeme"),
    Einteilung in Kategorien und Erzeugen von Token (Paare: Typ/Name, Wert)
*   Ausgabe: Tokenstrom

Normalerweise werden für spätere Phasen unwichtige Elemente wie White-Space
oder Kommentare entfernt.

Durch diese Vorverarbeitung wird eine höhere Abstraktionsstufe erreicht und es
können erste grobe Fehler gefunden werden. Dadurch kann der Parser auf einer
abstrakteren Stufe arbeiten und muss nicht mehr den gesamten ursprünglichen
Zeichenstrom verarbeiten.


*Anmerkung*: In dieser Phase steht die Geschwindigkeit stark im Vordergrund:
Der Lexer "sieht" *alle* Zeichen im Input. Deshalb findet man häufig von
Hand kodierte Lexer, obwohl die Erstellung der Lexer auch durch Generatoren
erledigt werden könnte ...


*Anmerkung*: Die Token sind die Terminalsymbole in den Parserregeln (Grammatik).
:::


## Definition wichtiger Begriffe

*   **Token**: Tupel (Tokenname, optional: Wert)

    ::: notes
    Der Tokenname ist ein abstraktes Symbol, welches eine lexikalische
    Einheit repräsentiert (Kategorie). Die Tokennamen sind die Eingabesymbole
    für den Parser.

    Token werden i.d.R. einfach über ihren Namen referenziert. Token werden
    häufig zur Unterscheidung von anderen Symbolen in der Grammatik in
    Fettschrift oder mit großen Anfangsbuchstaben geschrieben.

    Ein Token kann einen Wert haben, etwa eine Zahl oder einen Bezeichner, der
    auf das zum Token gehörende Pattern gematcht hatte (also das Lexem). Wenn
    der Wert des Tokens eindeutig über den Namen bestimmt ist (im Beispiel oben
    beim Komma oder den Klammern), dann wird häufig auf den Wert verzichtet.
    :::

\smallskip

*   **Lexeme**: Sequenz von Zeichen im Eingabestrom, die auf ein Tokenpattern
    matcht und vom Lexer als Instanz dieses Tokens identifiziert wird.

\smallskip

*   **Pattern**: Beschreibung der Form eines Lexems

    ::: notes
    Bei Schlüsselwörtern oder Klammern etc. sind dies die Schlüsselwörter oder
    Klammern selbst. Bei Zahlen oder Bezeichnern (Namen) werden i.d.R.
    reguläre Ausdrücke zur Beschreibung der Form des Lexems formuliert.
    :::


## Erkennung mit RE und DFA

![Erkennung mit RE und DFA](images/lexer/lexer)\

::: notes
Die obige Skizze ist eine Kurzzusammenfassung der Theorie-Vorlesung in der
letzten Woche und stellt die Verbindung zur heutigen Vorlesung her:

Die Lexeme werden mit Hilfe von *DFA* bestimmt. Die Formulierung der DFA ist
eher komplex (zumindest sehr umständlich), weshalb man die Pattern für die
Lexeme ersatzweise mit Hilfe von *Regulären Ausdrücken* ("*RE*") formuliert.

Mit Hilfe der *Thompson's Construction* kann man diese in äquivalente *NFA*
umformen. Über die *Subset Construction* kann man daraus *DFA* erzeugen, die
wiederum mit Hilfe des *Hopcroft's Algorithm* minimiert werden.

Diese DFA erkennen die selbe Sprache wie die ursprünglichen REs. Man könnte
also durch Simulation der DFA die Lexeme erkennen und die Token bilden. Dabei
würde pro Eingabezeichen ein Übergang im DFA stattfinden und bei Erreichen
eines akzeptierenden Zustandes hätte man das durch diesen DFA (bzw. dessen
ursprünglichen RE) beschriebene Lexem identifiziert.

Falls mehrere REs matchen, muss man in geeigneter Weise entscheiden. I.d.R.
nimmt man den längsten Match. Zusätzlich wird eine Reihenfolge unter den REs
festgelegt, um bei mehreren gleich langen Matches ein Token bestimmen zu
können.

In der Praxis werden die DFA als Ausgangspunkt für die Implementierung des
Lexers genutzt (ob nun bei einer "handgeklöppelten" Implementierung oder beim
Einsatz eines Lexer-Generators). Als typische Implementierungsansätze sollen
nachfolgend die *tabellenbasierte Implementierung* sowie als etwas schnellere
Variante die *direkt codierte Implementierung* betrachtet werden. Während diese
beiden Varianten noch sehr nah an der Simulation eines DFA sind, ist die
*manuelle Implementierung* noch einfacher in bestehenden Code zu integrieren
(zum Preis einer erschwerten Änderbarkeit).

Über die *Kleene's Construction* könnte man aus den DFA wieder *RE* erzeugen
und damit den Kreis schließen :-)
:::


## Erkennen von Zeichenketten für Strickmuster: "10L"

**TODO Strickmuster**


## Tabellenbasierte Implementierung

**TODO Code** 

::: notes
Der dargestellte Code implementiert direkt den DFA zur Erkennung von
Register-Namen unter Nutzung der Tabellen aus dem letzten Abschnitt.

Nach einer Initialisierung wird in der Hauptschleife nach dem nächsten
Zeichen im Eingabestrom gefragt und das Lexem erweitert. Wenn wir bereits
im akzeptierenden Zustand "`s2`" sind, wird der Stack gelöscht (dies ist
für die zweite Schleife wichtig). Anschließend wird der aktuelle Zustand
auf dem Stack gesichert und mit Hilfe der Tabellen die Kategorie des aktuellen
Zeichens sowie der passende Folgezustand bestimmt. Sobald der Fehlerzustand
"`se`" erreicht wird, bricht die Schleife ab.

*Anmerkung*: Wenn wir in "`s2`" sind, wird so lange nach weiteren Ziffern
gesucht, bis im Strom irgendetwas anderes auftaucht und wir entsprechend in
"`se`" landen.

In der zweiten Schleife wird der Stack aufgerollt, um zu schauen, ob wir
früher bereits in "`s2`" waren oder nicht. Das erste Element wird vom Stack
genommen, das Lexem wird um das letzte Zeichen gekürzt und dieses letzte
Zeichen wird mit `rollBack()` in den Eingabestrom zurückgelegt. Falls wir
früher bereits in "`s2`" waren, wird dieser Zustand irgendwann vom Stack
genommen. Anderenfalls ist der Stack irgendwann leer. (Zur Verkürzung dieser
Suche wird deshalb in der ersten Schleife der Stack bei Erreichen von "`s2`"
immer geleert!)

Falls "`s2`" erreicht wurde, wird ein neues "`s2`"-Token generiert und das
Lexem wird als Attribut direkt gesetzt. Anderenfalls lag ein Fehler vor.


*Anmerkung*: Diese Implementierung ist generisch: Wenn man im Code die
direkte Nennung des akzeptierenden Zustands "`s2`" durch einen Vergleich
mit einer Menge aller akzeptierender Zustände ersetzt ("`state == s2`"
\blueArrow "`state in acceptedStates`"), bestimmen nur die Tabellen die
konkrete Funktionsweise.

Die Tabellen können allerdings schnell sehr groß werden, insbesondere die
Zustandsübergangstabelle!
:::

[[Hinweis: Direkt codierte Implementierung]{.bsp}]{.slides}


::: notes
## Direkt codierte Implementierung

Die Implementierung über die Tabellen ist sowohl generisch als auch effizient.
Allerdings kostet jeder Zugriff auf die Tabelle konstanten Aufwand (Erinnerung:
Zugriff auf Arrays, Pointerarithmetik), der sich in der Praxis deutlich
summieren kann.

Die Lösung: Umsetzung der Tabelle direkt in Code ...

**TODO Code** 

Durch die direkte Kodierung der Tabellen in Form von Sprungzielen für
`goto`-Befehle spart man sich die Formulierung der Tabellen und den Zugriff
auf die Inhalte. Allerdings ist der Code deutlich schwerer lesbar und auch
deutlich schwerer an eine andere Sprache anpassbar. Dies stellt aber keinen
echten Nachteil dar, wenn er durch einen Generator aus einer Grammatik o.ä.
erzeugt wird.
:::


## Manuelle Implementierung

```python
def nextToken():
    while (peek != EOF):  # globale Variable
        switch (peek):
            case ' ': case '\t': case '\n': WS(); continue
            case '[': consume(); return Token(LBRACK, '[')
            ...
            default:
                if isLetter(peek): return NAME()
                raise Error("invalid character: "+peek)
    return Token(EOF_Type, "<EOF>")

def WS():
    while (peek == ' ' || peek == '\t' || ...): consume()

def NAME():
    buf = StringBuilder()
    do { buf.append(peek); consume(); } while (isLetter(peek))
    return Token(NAME, buf.toString())
```

::: notes
Die manuelle Implementierung "denkt" nicht in den Zuständen des DFA, sondern
orientiert sich immer am aktuellen Zeichen "`peek`". Abhängig von dessen
Ausprägung wird entweder direkt ein Token erzeugt und das Zeichen aus dem
Eingabestrom entfernt sowie das nächste Zeichen eingelesen (mittels der
Funktion `consume()`, nicht dargestellt im Beispiel), oder man ruft weitere
Funktionen auf, die das Gewünschte erledigen, beispielsweise um White-Spaces
zu entfernen oder um einen Namen einzulesen: Nach einem Buchstaben werden
alle folgenden Buchstaben dem Namen (Bezeichner) hinzugefügt. Sobald ein
anderes Zeichen im Eingabestrom erscheint, wird das Namen-Token erzeugt.

Die Funktion `consume()` "verbraucht" das aktuelle Zeichen "`peek`" und holt
das nächste Zeichen aus dem Eingabestrom.


*Anmerkung*: Häufig findet man im Lexer keinen "schönen" objektorientierten
Ansatz. Dies ist i.d.R. Geschwindigkeitsgründen geschuldet ...
:::


## Read-Ahead: Unterscheiden von "*<*" und "*<=*"

```python
def nextToken():
    while (peek != EOF):  # globale Variable
        switch (peek):
            case '<':
                if match('='): consume(); return Token(LE, "<=")
                else: consume(); return Token(LESS, '<')
            ...
    return Token(EOF_Type, "<EOF>")

def match(c):   # lookahead one character
    consume()
    if (peek == c): return True
    else: rollBack(); return False
```

::: notes
Um die Token "`<`" und "`<=`" unterscheiden zu können, müssen wir ein Zeichen
vorausschauen: Wenn nach dem "`<`" noch ein "`=`" kommt, ist es "`<=`", sonst
"`<`".

Erinnerung: Die Funktion `consume()` liest das nächste Zeichen aus dem
Eingabestrom und speichert den Wert in der globalen Variable `peek`.

Für das Read-Ahead wird die Funktion `match()` definiert, die zunächst das
bereits bekannte Zeichen, in diesem Fall das "`<`" durch das nächste Zeichen
im Eingabestrom ersetzt (Aufruf von `consume()`). Falls der Vergleich des
Lookahead-Zeichens mit dem gesuchten Zeichen erfolgreich ist, liegt das
"größere" Token vor, also "`<=`". Dann wird noch das "`=`" durch das nächste
Zeichen ersetzt und das Token `LE` gebildet. Anderenfalls muss das zuviel
gelesene Zeichen wieder in den Eingabestrom zurückgelegt werden (`rollBack()`).
:::


## Puffern des Input-Stroms

::: notes
Das Einlesen einzelner Zeichen führt zwar zu eleganten algorithmischen
Lösungen, ist aber zur Laufzeit deutlich "teurer" als das Einlesen mit
gepufferten I/O-Operationen, die eine ganze Folge von Zeichen einlesen
(typischerweise einen ganzen Disk-Block, beispielsweise 4096 Zeichen).

Dazu nutzt man zwei `char`-Puffer mit jeweils der Länge $N$, wobei $N$ der
Länge eines Disk-Blocks entsprechen sollte.
:::

![Doppel-Puffer](images/doublebuffer)\


```python
Input = 0; Fence = 0; fill Buffer[0:n]

def nextChar():     # consume()
    peek = Buffer[Input]
    Input = (Input+1) mod 2n
    if (Input mod n == 0):
        fill Buffer[Input:Input+n-1]
        Fence = (Input+n) mod 2n
    return peek

def rollBack():
    if (Input == Fence): raise Error("roll back error")
    Input = (Input-1) mod 2n
```

::: notes
Zunächst wird nur der erste Puffer durch einen passenden Systemaufruf
gefüllt.

Beim Weiterschalten im simulierten DFA oder im manuell kodierten Lexer
(Funktionsaufrufe von `nextChar()` bzw. `consume()`) wird das nächste Zeichen
aus dem ersten Puffer zurückgeliefert. Über die Modulo-Operation bleibt
der Pointer `Input` immer im Speicherbereich der beiden Puffer.

Wenn man das Ende des ersten Puffers erreicht, wird der zweite Puffer mit
einem Systemaufruf gefüllt. Gleichzeitig wird ein Hilfspointer `Fence` auf
den Anfang des ersten Puffers gesetzt, um Fehler beim Roll-Back zu erkennen.

Wenn man das Ende des zweiten Puffers erreicht, wird der erste Puffer
nachgeladen und der Hilfspointer auf den Anfang des zweiten Puffers gesetzt.

Im Grunde ist also immer ein Puffer der "Arbeitspuffer" und der andere enthält
die bereits gelesene (verarbeitete) Zeichenkette. Wenn beim Nachladen weniger
als $N$ Zeichen gelesen werden, liefert der Systemaufruf als letztes "Zeichen"
ein `EOF`. Beim Verarbeiten wird `peek` entsprechend diesen Wert bekommen und
der Lexer muss diesen Wert abfragen und berücksichtigen.


Für das Roll-Back wird der `Input`-Pointer einfach dekrementiert (und mit einer
Modulo-Operation auf den Speicherbereich der beiden Puffer begrenzt). Falls
dabei der `Fence`-Pointer "eingeholt" wird, ist der `Input`-Pointer durch beide
Puffer zurückgelaufen und es gibt keinen früheren Input mehr. In diesem Fall
wird entsprechend ein Fehler gemeldet.


*Anmerkung*: In der Regel sind die Lexeme kurz und man muss man nur ein bis
zwei Zeichen im Voraus lesen (vgl. nächsten Abschnitt). Dann ist eine
Puffergröße von 4096 Zeichen mehr als ausreichend groß und man sollte nicht in
Probleme laufen. Wenn der nötige Look-Ahead aber beliebig groß werden kann,
etwa bei Sprachen ohne reservierte Schlüsselwörter, muss man andere Strategien
verwenden. ANTLR beispielsweise vergrößert in diesem Fall den Puffer dynamisch,
alternativ könnte man die Auflösung zwischen Schlüsselwörtern und Bezeichnern
dem Parser überlassen.
:::


## Vermeidung von (zuviel) Roll-Back

**TODO** 

## Typische Muster für Erstellung von Token

1.  Schlüsselwörter
    *   Ein eigenes Token (RE/DFA) für jedes Schlüsselwort, oder
    *   Erkennung als Name und Vergleich mit Wörterbuch
        [und nachträgliche Korrektur des Tokentyps (\blueArrow siehe B02 :-)]{.notes}

    ::: notes
    Wenn Schlüsselwörter über je ein eigenes Token abgebildet werden, benötigt
    man für jedes Schlüsselwort einen eigenen RE bzw. DFA. Die Erkennung als
    Bezeichner und das Nachschlagen in einem Wörterbuch (geeignete Hashtabelle)
    sowie die entsprechende nachträgliche Korrektur des Tokentyps kann die
    Anzahl der Zustände im Lexer signifikant reduzieren!
    :::

2.  Operatoren
    *   Ein eigenes Token für jeden Operator, oder
    *   Gemeinsames Token für jede Operatoren-Klasse

3.  Bezeichner: Ein gemeinsames Token für alle Namen

4.  Zahlen: Ein gemeinsames Token für alle numerischen Konstante  [(ggf. Integer und Float unterscheiden)]{.notes}

    ::: notes
    Für Zahlen führt man oft ein Token "`NUM`" ein. Als Attribut speichert man
    das Lexem i.d.R. als String. Alternativ kann man (zusätzlich) das Lexem in
    eine Zahl konvertieren und als (zusätzliches) Attribut speichern. Dies kann
    in späteren Stufen viel Arbeit sparen.
    :::

5.  String-Literale: Ein gemeinsames Token

6.  Komma, Semikolon, Klammern, ...: Je ein eigenes Token

\smallskip

7.  Regeln für White-Space und Kommentare etc. ...

    ::: notes
    Normalerweise benötigt man Kommentare und White-Spaces in den folgenden
    Stufen nicht und entfernt diese deshalb aus dem Eingabestrom. Dabei könnte
    man etwa White-Spaces in den Pattern der restlichen Token berücksichtigen,
    was die Pattern aber sehr komplex macht. Die Alternative sind zusätzliche
    Pattern, die auf die White-Space und anderen nicht benötigten Inhalt
    matchen und diesen "geräuschlos" entfernen. Mit diesen Pattern werden
    keine Token erzeugt, d.h. der Parser und die anderen Stufen bemerken nichts
    von diesem Inhalt.

    Gelegentlich benötigt man aber auch Informationen über White-Spaces,
    beispielsweise in Python. Dann müssen diese Token wie normale Token
    an den Parser weitergereicht werden.
    :::


::: notes
Jedes Token hat i.d.R. ein Attribut, in dem das Lexem gespeichert wird. Bei
eindeutigen Token (etwa bei eigenen Token je Schlüsselwort oder bei den
Interpunktions-Token) kann man sich das Attribut auch sparen, da das Lexem
durch den Tokennamen eindeutig rekonstruierbar ist.

| Token     | Beschreibung                                         | Beispiel-Lexeme      |
| :-------- | :--------------------------------------------------- | :------------------- |
| `if`      | Zeichen `i` und `f`                                  | `if`                 |
| `relop`   | `<` oder `>` oder `<=` oder `>=` oder `==` oder `!=` | `<`, `<=`            |
| `id`      | Buchstabe, gefolgt von Buchstaben oder Ziffern       | `pi`, `count`, `x3`  |
| `num`     | Numerische Konstante                                 | `42`, `3.14159`, `0` |
| `literal` | Alle Zeichen außer `"`, in `"` eingeschlossen        | `"core dumped"`      |

: Beispiele für Token


*Anmerkung*: Wenn es mehrere matchende REs gibt, wird in der Regel das längste
Lexem bevorzugt. Wenn es mehrere gleich lange Alternativen gibt, muss man mit
Vorrangregeln bzgl. der Token arbeiten.
:::


## Fehler bei der Lexikalischen Analyse

<!-- XXX auch in Interpreter1 -->

[Problem: Eingabestrom sieht so aus:]{.notes} `fi (a==42) { ... }`

::: notes
Der Lexer kann nicht erkennen, ob es sich bei `fi` um ein vertipptes
Schlüsselwort handelt oder um einen Bezeichner: Es könnte sich um einen
Funktionsaufruf der Funktion `fi()` handeln ...
Dieses Problem kann erst in der nächsten Stufe sinnvoll erkannt und behoben
werden.
:::

\bigskip

\blueArrow Was tun, wenn keines der Pattern auf den Anfang des Eingabestroms passt?

\bigskip
\pause

::: notes
Optionen:
:::

*   Aufgeben ...

    ::: notes
    Eventuell vielleicht sogar die beste und einfachste Variante :-)
    :::

\smallskip

*   "Panic Mode": Entferne so lange Zeichen, bis ein Pattern passt.

    ::: notes
    Das verwirrt u.U. den Parser, kann aber insbesondere in interaktiven
    Umgebungen hilfreich sein. Ggf. kann man dem Parser auch signalisieren,
    dass hier ein Problem vorlag.
    :::

\smallskip

*   Ein-Schritt-Transformationen:
    *   Füge fehlendes Zeichen in Eingabestrom ein.
    *   Entferne ein Zeichen aus Eingabestrom.
    *   Vertausche ein Zeichen:
        *   Ersetze ein Zeichen durch ein anderes.
        *   Vertausche zwei benachbarte Zeichen.

    ::: notes
    Diese Transformationen versuchen, den Input in einem Schritt zu reparieren.
    Das ist durchaus sinnvoll, da in der Praxis die meisten Fehler in dieser
    Stufe durch ein einzelnes Zeichen hervorgerufen werden: Es fehlt ein
    Zeichen oder es ist eines zuviel im Input. Es liegt ein falsches Zeichen
    vor (Tippfehler) oder zwei benachbarte Zeichen wurden verdreht ...

    Im Prinzip könnte man auch eine allgemeinere Strategie versuchen, indem man
    diejenige Transformation mit der *kleinsten Anzahl von Schritten* zur
    Fehlerbehebung bestimmt. Beispiele dafür finden sich im Bereich Natural
    Language Processing (*NLP*), etwa die Levenshtein-Distanz oder der
    SoundEx-Algorithmus oder sogar Hidden-Markov-Modelle. Allerdings muss
    man sich in Erinnerung rufen, dass gerade in dieser ersten Phase eines
    Compilers die Geschwindigkeit stark im Fokus steht und eine ausgefeilte
    Fehlerkorrekturstrategie die vielen kleinen Optimierungen schnell wieder
    zunichte machen kann.
    :::

\smallskip

*   Fehler-Regeln: Matche typische Typos

    ::: notes
    Gelegentlich findet man in den Grammatiken für den Lexer extra Regeln, die
    häufige bzw. typische Typos matchen und dann passend darauf reagieren.
    :::


## Wrap-Up

*   Zusammenhang DFA, RE und Lexer

\smallskip

*   Implementierungsansätze:
    *   Tabellenbasiert oder direkt codiert (DFA-Tabellen)
    *   Manuell codiert: Funktionen für Token
    *   Read-Ahead-Token

\smallskip

*   Optimierungen
    *   Puffern mit Doppel-Puffer-Strategie
    *   Bitmuster zum Vermeiden von zuviel Roll-Back

\smallskip

*   Typische Fehler beim Scannen







<!-- DO NOT REMOVE - THIS IS A LAST SLIDE TO INDICATE THE LICENSE AND POSSIBLE EXCEPTIONS (IMAGES, ...). -->
::: slides
## LICENSE
![](https://licensebuttons.net/l/by-sa/4.0/88x31.png)

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

### Exceptions
*   TODO (what, where, license)
:::
