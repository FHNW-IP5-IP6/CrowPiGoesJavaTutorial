---
title: 7-Segment Anzeige
tags: ["i2c"]
---

## Funktionsweise

Die 7-Segment Anzeige auf dem CrowPi besteht aus 4 verschiedenen Ziffern, welche jeweils die Werte `0-9` sowie `A-F` darstellen können. 
Der Name ist dabei auch Programm, da jede Ziffer grundsätzlich aus 7 verschiedenen Segmenten besteht, welche je nach Zustand (an/aus) 
dann die verschiedenen Werte darstellen können. Dies bedeutet jedoch auch dass bei der Anzeige auf dem CrowPi insgesamt 33 verschiedene 
Segmente existieren (4x 7-Segment, 4x Dezimalpunkt, 1x Doppelpunkt), welche alle einzeln angeschlossen und gesteuert werden müssten.

Um dies zu vermeiden, verwendet die 7-Segment Anzeige den `HT16K33`, einen elektronischen Baustein zur Ansteuerung von mehreren LEDs 
über eine einzelne Schnittstelle. Hierfür wird der Bus [I2C]({{< ref "hardware/i2c" >}}) eingesetzt, über welchen verschiedene 
Steuerungsbefehle an die 7-Segment Anzeige gesendet werden können.

Da das Übertragen jeder einzelnen Änderung an die Komponente sehr ineffizient ist, wird üblicherweise ein Buffer in der Software 
implementiert. Alle gewünschten Anpassungen verändern somit nur den Buffer, welcher anschliessend mit nur einem einzigen Befehl übertragen 
werden kann. Dieses Prinzip wird auch von der Komponenten-Klasse `SevenSegmentComponent` verwendet.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, so dass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="ch.fhnw.crowpi.components.SevenSegmentComponent" >}} beschrieben. Es ist
hierbei wichtig zu beachten, dass alle Methoden mit dem Wort "Buffer:" am Anfang der Beschreibung nicht eine direkte Auswirkung haben,
sondern erst mit einem Aufruf von `void refresh()` auf der Anzeige sichtbar werden. Es ist somit zu empfehlen, alle gewünschten
Einstellungen zu tätigen und dann mit einem einzigen Aufruf die Änderungen auf dem Gerät darzustellen.

### Konstruktoren

| Konstruktor | Bemerkung |
| --- | --- |
| `SevenSegmentComponent(com.pi4j.context.Context pi4j)` | Initialisiert eine 7-Segment Anzeige mit der Standard Bus- und Geräteadresse für den CrowPi. |
| `SevenSegmentComponent(com.pi4j.context.Context pi4j, int bus, int device)` | Initialisiert eine 7-Segment Anzeige mit einer benutzerdefinierten Bus- und Geräteadresse. |

### Methoden

| Methode | Bemerkung |
| --- | --- |
| `void print(int i)` | Gibt eine Ganzzahl mit bis zu 4 Ziffern auf der Anzeige aus. Zu lange Zahlen werden nach hinten abgeschnitten. |
| `void print(double d)` | Gibt eine Kommazahl mit bis zu 4 Ziffern auf der Anzeige aus. Zu lange Zahlen werden nach hinten abgeschnitten. |
| `void print(java.time.LocalTime time)` | Gibt die übergebene Uhrzeit in Stunden und Minuten auf der Anzeige aus. Der Doppelpunkt auf der Anzeige wird bei ungerader Sekundenzahl aktiviert und ansonsten deaktiviert. |
| `void print(String s)` | Gibt die ersten 4 übergebenen Zeichen auf der Anzeige aus. Hierbei ist zu beachten dass die Anzeige nur wenige Zeichen unterstützt und ansonsten eine `IllegalArgumentException` wirft. |
| `void clear()` | Löscht den internen Buffer ohne die Anzeige zu aktualisieren. |
| `void refresh()` | Aktualisiert die Anzeige mit dem aktuellen Inhalt des Buffers. |
| `void setEnabled(boolean enabled)` | Schaltet die Anzeige ein oder aus gemäss Boolean-Wert. |
| `void setBlinkRate(int rate)` | Setzt die gewünschte Blink-Geschwindigkeit der ganzen Anzeige zwischen 0 (aus) und 3 (am schnellsten). |
| `void setBrightness(int brightness)` | Setzt die gewünschte Helligkeit der ganzen Anzeige zwischen 0 (am dunkelsten) und 15 (am hellsten). |
| `void setColon(boolean enabled)` | *Buffer:* Aktiviert/deaktiviert den Doppelpunkt auf der Anzeige. |
| `void setDecimalPoint(int position, boolean enabled)` | *Buffer:* Aktiviert/deaktiviert den entsprechenden Dezimalpunkt auf der Anzeige. Diese Einstellung wird durch Setzen einer Ziffer automatisch wieder deaktiviert. |
| `void setDigit(int position, int i)` | *Buffer:* Setzt die entsprechende Ziffer auf die übergebene Zahl zwischen 0 und 9. |
| `void setDigit(int position, char c)` | *Buffer:* Setzt die entsprechende Ziffer auf das übergebene Zeichen. |
| `void setDigit(int position, Segment... segments)` | *Buffer:* Setzt bei der entsprechenden Ziffer die übergebenen Segmente. |

## Beispielapplikation

Die nachfolgende Beispielapplikation besteht aus 3 verschiedenen Sektionen, welche die Vielfältigkeit der 7-Segment Anzeige veranschaulichen
sollen. Zuerst werden die 4 Ziffern auf der Anzeige auf verschiedene Werte gesetzt, wobei es sich bei der letzten Ziffer sogar um ein
komplett eigenes Symbol handelt.

Nach einer Wartezeit von 3 Sekunden wird eine kleine Ladeanimation auf der Anzeige dargestellt. Hierfür wird ein Array `states`
definiert, in welchem die Ladeposition mithilfe von Zeichenketten dargestellt wird. Es handelt sich hierbei um einen Strich, welcher sich
jeweils von links nach rechts bewegt. Hierfür wird eine verschachtelte `for`-Schleife eingesetzt, wobei mit `i` die ganze Animation
insgesamt fünfmal wiederholt wird, während die innere Schleife über alle möglichen Animationszustände iteriert und diese nacheinander mit
einer Verzögerung von 50 Millisekunden ausgibt.

Nachdem diese Ladeanimation abgespielt wurde, geht die Applikation in eine Endlosschleife und stellt jeweils die aktuelle Uhrzeit dar. Die
API-Methode `print(LocalTime time)` übernimmt hierbei den Grossteil der Arbeit und blinkt sogar automatisch wenn die Sekundenzahl zurzeit
ungerade ist. Durch das Abbrechen vom Maven-Prozess lässt sich diese Applikation anschliessend wieder beenden.

{{< code file="src/main/java/ch/fhnw/crowpi/applications/SevenSegmentApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Beispiel könnte so umgeschrieben werden, dass von einer vordefinierten Zeit heruntergezählt wird und die Anzeige nach Erreichen von 0
  entsprechend blinkt. In Kombination mit dem Buzzer wäre somit sogar eine Eieruhr möglich.

- Statt jeweils nur die aktuelle Zeit mit `LocalTime.now()` anzuzeigen, könnte auch die aktuelle Uhrzeit in verschiedenen Zeitzonen
  hintereinander dargestellt werden.

- In Kombination mit den vorhandenen Buttons lassen sich Eingaben für Zahlen realisieren, sodass beispielsweise eine Stoppuhr implementiert
  werden könnte.
