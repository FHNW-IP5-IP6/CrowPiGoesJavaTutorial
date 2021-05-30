---
title: Schrittmotor
tags: ["gpio"]
---

## Funktionsweise

Der Schrittmotor, bestehend aus festem Stator und bewegenden Rotor, erhält sein Drehmoment durch unterschiedlich ausgerichtete Magnetfelder.
Der Rotor dreht sich dabei immer so, dass sich ein möglichst starker magnetischer Fluss ausbildet. Anders als bei anderen Motoren befinden
sich beim Schrittmotor jedoch nur im Stator Spulen. Durch gezieltes Ein- und Ausschalten dieser Spulen wird der Motor in Drehung versetzt.
Mit korrekter Abfolge der Steuerung lassen sich so Vorwärts und Rückwärtslauf implementieren. Der Name des Motors kommt aus der Tatsache das
sich ebenfalls durch Steuerung der Spulen einzelne Schritte mit dem Motor bewegen lassen. Die Schrittgrösse ist dabei abhängig vom
physikalischen Aufbau des Schrittmotors. Um die Position des Rotors zu bestimmen, genügt es, ausgehend von einer Ausgangslage die Schritte
im bzw. gegen den Uhrzeigersinn zu zählen und mit dem Schrittwinkel zu multiplizieren.

Die Schrittmotoren bieten folgende Vorteile:
- Genaue Positionierung, keine kumulierten Fehler
- Haltemoment in Ruhelage
- Günstige Antriebslösung mit hoher Genauigkeit
- Einfacher Aufbau des Treibers

{{< img alt="Anschluss für Schrittmotor" src="components/step-motor.jpg" height="500px" >}}

## Voraussetzungen

### DIP Switches

Für diese Komponente müssen vier DIP Switches des rechten Blockes gesetzt werden. Die Stellung der DIP Switches sollte anschliessend so
aussehen:

{{< dip-switches 11 12 13 14 >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.StepMotorComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                                                                 | Bemerkung                                                                                                                                                                                             |
|:------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `StepMotorComponent(com.pi4j.context.Context pi4j)`                                                         | Initialisiert einen Schrittmotor mit den Standardeinstellungen für den CrowPi.                                                                                                                        |
| `StepMotorComponent(com.pi4j.context.Context pi4j, int[] addresses, int[][] steps, long pulseMilliseconds)` | Initialisiert einen Schrittmotor mit frei wählbaren Pins. Diese werden mit dem Parameter `steps` an entsprechende Schritte des Motors gekoppelt. Zusätzlich kann noch die Pulslänge übergeben werden. |

### Methoden

| Methode                         | Bemerkung                                                                                                                |
|:--------------------------------|:-------------------------------------------------------------------------------------------------------------------------|
| `void turnDegrees(int degrees)` | Dreht die Achse des Schrittmotors um den übergebenen Winkel in Grad [°]. Ein negativer Winkel dreht den Motor rückwärts. |
| `void turnForward(int steps)`   | Dreht den Motor um die spezifizierte Anzahl Schritte vorwärts.                                                           |
| `void turnBackward(int steps)`  | Dreht den Motor um die spezifizierte Anzahl Schritte rückwärts.                                                          |


## Beispielapplikation

Die Beispielanwendung zeigt auf einfache Art und Weise wie die verschiedenen Methoden der `StepMotorComponent` zu verwenden sind. Als Erstes
wird nach einer kurzen Warnung der Motor um 50 Schritte vorwärts gedreht. Anschliessend dreht der Motor rückwärts ebenfalls um 50 Schritte.
Der Motor ist also wieder in Ausgangsposition. Als letztes werden nun mithilfe eines `for-loops` einige Male hin und her gedreht. Dazu wird
nun die `turnDegrees` Methode verwendet.

{{< code file="src/main/java/com/pi4j/crowpi/applications/StepMotorApp.java" language="java">}}

## Weitere Möglichkeiten

- Ein 3D-Drucker könnte aus der Kombination von mehreren Schrittmotoren gebaut werden.
- Automatisches Heizventil in Kombination mit einem Temperatursensor.
