---
title: Buzzer
---

## Funktionsweise

Der Buzzer funktioniert wie ein Lautsprecher, jedoch mit dem Unterschied, dass dieser technisch gesehen nur einen einzigen Ton abspielen
kann. Dank der Verwendung von [PWM]({{< ref "hardware/pwm" >}}) ist es jedoch möglich diesen auch so anzusteuern, dass damit entsprechende
Melodien mit verschiedenen Tönen abgespielt werden können.

Die Klasse `BuzzerComponent` verwendet eine solche PWM-Implementation und bietet eine einfache Schnittstelle, mit welcher beliebige Töne
abgespielt werden können. Um eine zuverlässige Wiedergabe der gewünschten Frequenzen zu ermöglichen, verwendet diese Komponente
[Hardware PWM]({{< ref "hardware/pwm" >}}). Mehr Informationen dazu finden sich auf der verlinkten Seite.

Um das mühsame Abtippen der einzelnen Frequenzen zu erleichtern, wird zudem eine Hilfsklasse `Note` bereitgestellt. Dieser Aufzählungstyp
enthält alle bekannten Musiknoten von `B0` zu `DS8` und bildet diese auf die entsprechenden Tonfrequenzen ab, so dass diese direkt an
die `playTone()` Methode übergeben werden können.

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="ch.fhnw.crowpi.components.BuzzerComponent" >}} beschrieben.

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
`Note` verwendet, welches die entsprechenden Musiknoten auf eine Frequenz in Hertz (Hz) abbildet. Die Applikation spielt nacheinander alle
Noten ab, indem die Funktionen `playTone` sowie `playFrequency` der Buzzer-Komponente verwendet werden.

{{< code file="src/main/java/ch/fhnw/crowpi/applications/BuzzerApp.java" language="java" >}}
