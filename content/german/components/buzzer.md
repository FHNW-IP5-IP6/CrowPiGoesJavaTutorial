---
title: Buzzer
tags: ["pwm"]
---

## Funktionsweise

Der Buzzer funktioniert wie ein Lautsprecher, jedoch mit dem Unterschied, dass dieser technisch gesehen nur einen einzigen Ton abspielen
kann. Dank der Verwendung von [PWM]({{< ref "hardware/pwm" >}}) ist es jedoch möglich diesen auch so anzusteuern, dass damit entsprechende
Melodien mit verschiedenen Tönen abgespielt werden können.

Die Klasse `BuzzerComponent` verwendet eine solche PWM-Implementation und bietet eine einfache Schnittstelle, mit welcher beliebige Töne
abgespielt werden können. Um eine zuverlässige Wiedergabe der gewünschten Frequenzen zu ermöglichen, verwendet diese Komponente
[Hardware PWM]({{< ref "hardware/pwm" >}}). Mehr Informationen dazu finden sich auf der verlinkten Seite.

Um das mühsame Abtippen der einzelnen Frequenzen zu erleichtern, wird zudem eine Hilfsklasse `Note` bereitgestellt. Dieser Aufzählungstyp
enthält alle bekannten Musiknoten von `B0` zu `DS8` und bildet diese auf die entsprechenden Tonfrequenzen ab, sodass diese direkt an
die `playTone()` Methode übergeben werden können.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.BuzzerComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor | Bemerkung |
| --- | --- |
| `BuzzerComponent(com.pi4j.context.Context pi4j)` | Initialisiert einen Buzzer mit dem Standard-Pin für den CrowPi. |
| `BuzzerComponent(com.pi4j.context.Context pi4j, int address)` | Initialisiert einen Buzzer mit einem benutzerdefinierten Pin. |

### Methoden
| Methode | Bemerkung |
| --- | --- |
| `void playTone(int frequency)` | Spielt dauerhaft einen Ton mit der gewünschten Frequenz. |
| `void playTone(int frequency, int duration)` | Spielt einen Ton mit der gewünschten Frequenz für die angegebene Zeit in Millisekunden. |
| `void playSilence()` | Schaltet den Buzzer dauerhaft aus. |
| `void playSilence(int duration)` | Schaltet den Buzzer dauerhaft aus und wartet für die angegebene Zeit in Millisekunden. |

## Beispielapplikation

Die nachfolgende Beispielapplikation steuert den Buzzer via PWM an, um eine bekannte Melodie abzuspielen. Hierfür wird ein Enum namens
`Note` verwendet, welches die entsprechenden Musiknoten auf eine Frequenz [Hz] für PWM abbildet, dies klingt dann wie der gesuchte Ton. Die Applikation spielt nacheinander alle
Noten ab, indem die Funktionen `playTone` sowie `playSilence` der Buzzer-Komponente verwendet werden.

Die Noten, die gespielt werden sollen, sind alle in der statischen Variable `NOTES` zu finden, welche durch die zusätzliche statische 
Variable mit dem Namen `TEMPO` um die Angabe der Notenlänge ergänzt wird. Die Applikation iteriert schliesslich mit einer `for`-Schleife 
über alle Noten und spielt diese mit der jeweiligen Länge ab.

Die effektive Länge in Millisekunden wird mit der Formel `1 / <tempo>` berechnet, sodass ein Tempo-Wert von 4 einer Viertelnote mit 0.25
Sekunden Länge entspricht. Damit sich die Noten besser voneinander unterscheiden lassen, wird zudem jeweils noch eine kleine Pause 
mit dem Faktor `1.3` eingefügt.

{{< code file="src/main/java/com/pi4j/crowpi/applications/BuzzerApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Beispiel kann einfach auf eine eigene Melodie angepasst werden, wofür jeweils lediglich die beiden Arrays `NOTES` und `TEMPO` 
angepasst werden müssen. Hier ist darauf zu achten, dass diese Arrays jeweils die gleiche Länge besitzen müssen, da jede Note auch eine 
dazugehörige Länge benötigt.

- Statt dem Abspielen einer Melodie könnte auch eine Sirene mit einer `for`-Schleife implementiert werden. Hierfür muss die Frequenz 
  manuell als Zahl angegeben werden statt der Verwendung der `Note` Klasse, welche sich beispielsweise mit dem Einsatz von 
  `for`-Schleifen hoch- und runterzählen lässt.

- Belegt man jeden Kopf der Button-Matrix mit einem eigenen Ton / Frequenz könnte man daraus seine eigene Melodie komponieren.  
