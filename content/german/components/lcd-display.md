---
title: LCD Display
tags: ["gpio", "i2c"]
---

## Funktionsweise

Beim LCD Display handelt es sich um eine der komplexesten Komponenten des CrowPi. Die betrifft sowohl die Ansteuerung in der Software als
auch die Funktionalität des Displays an sich. Das LCD Display verfügt auf 2 Zeilen jeweils über 16 kleine Pixelfelder. Diese bestehen aus je
5x8 Pixeln. Jedes dieser Pixel wird von den Mikrocontrollern des Displays einzeln angesteuert um so Buchstaben, Zahlen und Sonderzeichen zu
zeigen. Auf diese Weise können also bis zu 32 Zeichen gleichzeitig angezeigt werden. Damit nun der Benutzer nicht jedes Pixel einzeln
ansteuern muss, sind schon die wichtigsten Zeichen vorprogrammiert. Es können `a-Z, 0–9 und einige Sonderzeichen` einfach genutzt werden. Um
dem Benutzer noch mehr Möglichkeiten zu offerieren, können zudem noch 8 benutzerdefinierte Zeichen erfasst werden. Durch diese
vielfältigkeit und doch schon eine grosse Menge an Platz für die Anzeige von Text und Zahlen bieten sich unzählige Anwendungsmöglichkeiten
für ein solches LCD-Display.

Die Ansteuerung des Displays erfolgt über einfache digitale Signale. Da jedoch 8 Stück benutzt werden und der Raspberry Pi nur wenige zur
Verfügung hat gibt es eine kleine Zwischenschaltung. Es wird ein Baustein verwendet welcher mittels
[I²C]({{< ref "hardware/i2c" >}}) einige zusätzliche digitale Ein- und Ausgänge zur Verfügung stellt. Auf dem Bild ist diese Verschaltung
nochmals skizziert: {{< img alt="Schaltung des LCD-Display" src="components/lcd-display-wiring.jpg" >}}

Die Helligkeit des Displays wird bei dem Aufbau im CrowPi mit einem Potentiometer eingestellt. Mit dem Schraubenzieher kann dieses so
eingestellt werden bis das Display einen optimalen Kontrast hat und alles gut lesbar ist. Das Potentiometer befindet sich direkt links neben
dem Display. Es ist relativ klein und kann gut übersehen werden. Auf dem folgenden Bild wird es daher nochmals kurz gezeigt. {{< img
alt="Potentiometer des LCD-Display" src="components/potentiometer-lcd-display.png" height="500px" >}}

Die verbaute Hardware beim CrowPi ist ein `MCP23008 I/O Expander` mit einem `1602A-1 LCD Display`.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="ch.fhnw.crowpi.components.LcdDisplayComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                       | Bemerkung                                                                           |
|:------------------------------------------------------------------|:------------------------------------------------------------------------------------|
| `LcdDisplayComponent(com.pi4j.context.Context pi4j)`              | Initialisiert ein LCD Display mit der Standard Bus- und Geräteadresse.              |
| `LcdDisplayComponent(com.pi4j.context.Context pi4j, int address)` | Initialisiert ein LCD Display mit einer benutzerdefinierten Bus- und Geräteadresse. |

### Methoden

| Methode                                                | Bemerkung                                                                                                                                            |
|:-------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------|
| `void initialize()`                                    | initialisiert das Display und bereitet alles für die Benutzung vor.                                                                                  |
| `void writeCharacter(char c)`                          | Schreibt einen einzelnen Character auf die aktuelle Cursorposition                                                                                   |
| `void writeCharacter(char c, int digit, int line)`     | Schreibt einen einzelnen Character auf die übergebene Displayposition                                                                                |
| `void writeLine(String text, int line)`                | Schreibt den maximal 16 Zeichen langen Text auf die in gewählte Zeile des Displays                                                                   |
| `void writeText(String text)`                          | Schreibt einen 32 Zeichen langen Test auf beide Zeilen des Displays. Der Zeilenumbruch funktioniert automatisch oder kann mit `\n` eingefügt werden. |
| `void moveCursorHome()`                                | Bewegt den Cursor an die Position erste Zeile erstes Zeichen.                                                                                        |
| `void moveCursorRight()`                               | Bewegt den Cursor eine Stelle nach rechts.                                                                                                           |
| `void moveCursorLeft()`                                | Bewegt den Cursor eine Stelle nach links.                                                                                                            |
| `void setCursorToPosition(int digit, int line)`        | Bewegt den Cursor auf angegebene Position.                                                                                                           |
| `void setCursorToLine(int line)`                       | Schiebt den aktuellen Zeiger auf die gewünschte Zeile des Displays.                                                                                  |
| `void setCursorVisibility(boolean show)`               | Hiermit kann der Cursor angezeigt (`true`) oder versteckt (`false`) werden.                                                                          |
| `void setCursorBlinking(boolean blink)`                | Das Feld, auf dem der Cursor sich aktuell befindet, blinkt (`true`) oder nicht (`false`)                                                             |
| `void moveDisplayRight()`                              | Bewegt alles auf dem Display um eine Stelle nach rechts.                                                                                             |
| `void moveDisplayLeft()`                               | Bewegt alles auf dem Display um eine Stelle nach links.                                                                                              |
| `void createCharacter(int location, byte[] character)` | Erstellt einen benutzerdefinierten Charakter auf der gewünschten Speicherstelle. Das Array muss genau der Länge acht entsprechen                     |
| `void setDisplayBacklight(boolean state)`              | Schaltet die Hintergrundbeleuchtung des Displays ein oder aus.                                                                                       |
| `void clearDisplay()`                                  | Löscht alle auf dem Display gezeigten Zeichen.                                                                                                       |
| `void clearLine(int line)`                             | Löscht alle Zeichen auf einer gewählten Zeile.                                                                                                       |

### Enumerationen

- {{< javadoc class="ch.fhnw.crowpi.components.LcdDisplayComponent" subclass="Symbol" >}} enthält alle unterstützten Symbole welche von
  dieser Komponenten-Klasse dargestellt werden können. Es handelt sich hierbei um eine Sammlung von ASCII-Codes sowie diversen Symbolen.
  Zusätzlich können mit `\1` bis `\7` die eigenen Zeichen aufgerufen und eingefügt werden. Die Werte sind jeweils als `byte` hinterlegt, was
  der internen Darstellung des LCD Displays

## Beispielapplikation

In der Beispielapplikation werden verschiedenste Varianten der Text- und Zeichenausgabe mit dem LCD Display demonstriert. Als erster Schritt
wird auf zwei Zeilen einzeln der Klassiker `Hello World!` ausgegeben. Anschliessend folgt eine sehr interessante Funktion. Diese erlaubt es
eigene Zeichen zu definieren. Bis zu 7 verschiedene Zeichen sind gleichzeitig speicherbar. Mit etwas Fantasie lassen sich so die
verschiedensten Symbole erstellen. Nun folgen nochmals einige Textausgaben. Während diesen Ausgaben wird kurz der `Cursor` angezeigt. Dieser
stellt jeweils die aktuelle Schreibposition des Displays dar. Er kann durch auch sehr einfach verschoben werden. Auch das wird kurz
demonstriert. Zum Abschluss folgen nochmals einige Textausgaben auch in kombination mit manuellen Zeilenumbrüchen. Dazu wird ein `\n` in den
String eingefügt. Dies erzeugt dann sofort einen Zeilenumbruch. {{< code
file="src/main/java/ch/fhnw/crowpi/applications/LcdDisplayApp.java"language="java" >}}

## Weitere Möglichkeiten

- Das Display mit anderen Komponenten kombinieren. Dabei bieten sich unzählige Möglichkeiten. Es könnten die aktuelle Temperatur oder
  Helligkeit angezeigt werden.
- Eine Karaoke Maschine. Mittels des Buzzers einen Song abspielen und auf dem Display jeweils den aktuellen Text anzeigen. Viel Spass beim
  Singen und Programmieren!

