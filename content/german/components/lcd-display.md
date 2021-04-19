---
title: LCD Display
tags: ["gpio", "i2c"]
---

## Funktionsweise
Beim LCD Display handelt es sich um eine der komplexesten Komponenten des CrowPi. Die betrifft sowohl die Ansteuerung in der Software als 
auch die Funktionalität des Displays an sich. Das LCD Display verfügt auf 2 Zeilen jeweils über 16 kleine Pixelfelder. Diese bestehen
aus je 5x8 Pixeln. Jedes dieser Pixel wird von den Mikrocontrollern des Displays einzeln angesteuert um so Buchstaben, Zahlen und 
Sonderzeichen zu zeigen. Auf diese Weise können also bis zu 32 Zeichen gleichzeitig angezeigt werden. Damit nun der Benutzer nicht jedes 
Pixel einzeln ansteuern muss, sind schon die wichtigsten Zeichen vorprogrammiert. Es können ´a-Z, 0–9 und einige Sonderzeichen´ einfach 
genutzt werden. Um dem Benutzer noch mehr Möglichkeiten zu offerieren, können zudem noch 8 benutzerdefinierte Zeichen erfasst werden.
Durch diese vielfältigkeit und doch schon grosse Menge an Platz für die Anzeige von Text und Zahlen bieten sich unzählige 
Anwendungsmöglichkeiten für ein solches LCD-Display.

Die Ansteuerung des Displays erfolgt über einfache digitale Signale. Da jedoch 8 Stück benutzt werden und der Raspberry Pi nur wenige 
zur Verfügung hat gibt es eine kleine Zwischenschaltung. Es wird ein Baustein verwendet welcher mittels [I²C]({{< ref "hardware/i2c" >}})
einige zusätzliche digitale Ein- und Ausgänge zur Verfügung stellt. Auf dem Bild ist diese Verschaltung nochmals skizziert:
{{< img alt="Schaltung des LCD-Display" src="components/lcd-display-wiring.jpg" >}}

Die Helligkeit des Displays wird bei dem Aufbau im CrowPi mit einem Potentiometer eingestellt. Mit dem Schraubenzieher kann dieses so 
eingestellt werden bis das Display einen optimalen Kontrast hat und alles gut lesbar ist. Das Potentiometer befindet sich direkt links 
neben dem Display. Es ist relativ klein und kann gut übersehen werden. Auf dem folgenden Bild wird es daher nochmals kurz gezeigt.
{{< img alt="Potentiometer des LCD-Display" src="components/potentiometer-lcd-display.png" height="500px" >}}

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="ch.fhnw.crowpi.components.LcdDisplayComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor | Bemerkung |
| --- | --- |
| `LcdDisplayComponent(com.pi4j.context.Context pi4j)` | Initialisiert ein LCD Display mit der Standard Bus- und Geräteadresse. |
| `LcdDisplayComponent(com.pi4j.context.Context pi4j, int address)` | Initialisiert ein LCD Display mit einer benutzerdefinierten Bus- und Geräteadresse. |

### Methoden

| Methode | Bemerkung |
| --- | --- |
| `void initialize()` | initialisiert das Display und bereitet alles für die Benutzung vor. |
| `void writeLine(String text, int line)` | Schreibt den maximal 16 Zeichen langen Text auf die in gewählte Zeile des Displays  |
| `void writeText(String text` | Schreibt einen 32 Zeichen langen Test auf beide Zeilen des Displays. Der Zeilenumbruch funktioniert automatisch oder kann mit ´\n´ eingefügt werden. |
| `void setCursorToLine(int number)` | Schiebt den aktuellen Zeiger auf die gewünschte Zeile des Displays. |
| `void setDisplayBacklight(boolean state)` | Schaltet die Hintergrundbeleuchtung des Displays ein oder aus. |d

### Enumerationen

????
- {{< javadoc class="ch.fhnw.crowpi.components.LcdDisplayComponent" subclass="Symbol" >}} beinhaltet die wichtigsten ASCII Zeichen 
  welche mit dem Display verwendet werden können. 

## Beispielapplikation

????
{{< code file="src/main/java/ch/fhnw/crowpi/applications/LcdDisplayApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Display mit anderen Komponenten kombinieren. Dabei bieten sich unzählige Möglichkeiten. Es könnten die aktuelle Temperatur oder 
  Helligkeit angezeigt werden.
- Eine Karaoke Maschine. Mittels des Buzzers einen Song abspielen und auf dem Display jeweils den aktuellen Text anzeigen. Viel Spass 
  beim Singen und Programmieren!