---
title: Servomotor
tags: ["pwm"]
---

## Funktionsweise

Bei einem Servomotoren sind in einer vielzahl von Bauformen und Grössen erhältlich. Eine Eigenschaft welche einen Servomotor von einem
normalen Elektromotor unterscheidet ist die Existenz eines Regelkreises. Dieser Regelkreis ermöglicht also erst die Kontrolle über
Drehgeschwindigkeit, Beschleunigung und Winkelposition des Motors. Im Gegensatz zum Schrittmotor sorgt also nicht der physikalische Aufbau
für diese Kontrollmöglichkeiten, sondern eine Regelung. Die Regelung im Falle des beim CrowPi beiliegenden Servomotors wird durch ein
simples Potentiometer erreicht. Bei industriellen, hochwertigen Servomotoren werden für die Positionsrückmeldung spezielle Encoder
verwendet. Diese erlauben viel höhere Genauigkeiten als die Bauform mit Potentiometer. Verwendet wird diese vereinfachte Form häufig im
Modellbau für die Einstellung von Stellwinkeln bei Modellflugzeugen.

Der Anschluss der Servomotors erfolg so: {{< img alt="Anschluss für Schrittmotor" src="components/servo-motor.jpg" height="500px" >}}

## Voraussetzungen

### DIP Switches

Für diese Komponente müssen zwei DIP Switches des rechten Blockes gesetzt werden. Die Stellung der DIP Switches sollte anschliessend so
aussehen:

{{< dip-switches 15 16 >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.StepMotorComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                                                                                                              | Bemerkung                                                                                                                         |
|:---------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------|
| `ServoMotorComponent(com.pi4j.context.Context pi4j)`                                                                                                     | Initialisiert einen Servomotor mit den Standardeinstellungen für den CrowPi.                                                      |
| `ServoMotorComponent(com.pi4j.context.Context pi4j, float minAngle, float maxAngle, float minDutyCycle, float maxDutyCycle)`                             | Initialisiert einen Servomotor mit dem Standardpin. Hier sind jedoch benutzerdefinierte Winkelangaben sowie Tastgrad einstellbar. |
| `ServoMotorComponent(com.pi4j.context.Context pi4j, int address, int frequency, float minAngle, float maxAngle, float minDutyCycle, float maxDutyCycle)` | Initialisiert einen Servomotor mit frei definierbarem Pin. Zusätzlich sind ebenfalls Winkelangaben sowie Tastgrad einstellbar.    |

### Methoden

| Methode                                                         | Bemerkung                                                                                                                                                                                 |
|:----------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `void setAngle(float angle)`                                    | Rotiert den Motor auf den übergebenen Winkel in Grad [°].                                                                                                                                 |
| `void setPercent(float percent)`                                | Positioniert den Motor zwischen  `minAngle` und `maxAngle` prozentual.                                                                                                                    |
| `void moveOnRange(float value)`                                 | Positioniert den Motor entsprechend des übergebenen Werts und anhand der Kalkulation aus den mit `setRange` gesetzten Grenzwerten.                                                        |
| `void moveOnRange(float value, float minValue, float maxValue)` | Positioniert den Motor entsprechend des übergebenen Werts und anhand der Kalkulation aus den mit angegeben Grenzwerten. Hat keinen Einfluss auf die mit  `setRange` gesetzten Properties. |
| `void setRange(float minValue, float maxValue)`                 | Setzt die Unter- und Obergrenze der Werteskala. Wird für die Bewegungskalkulation mit  `moveOnRange(float value)` benutzt.                                                                |
| `float getMaxAngle()`                                           | Gibt den maximal positionierbaren Winkel des Servomotors zurück.                                                                                                                          |
| `float getMinAngle()`                                           | Gibt den minimal positionierbaren Winkel des Servomotors zurück.                                                                                                                          |


## Beispielapplikation

Die Beispielanwendung zeigt auf einfache Art und Weise wie die verschiedenen Methoden der `ServoMotorComponent` zu verwenden sind. Als
Erstes wird demonstriert wie einfach mit der prozentualen Positionierung gearbeitet werden kann. Dazu wird die Methode `setPercent`
verwendet. Als Zweites folgt dann ein Beispiel einer Positionierung mittels einer benutzerdefinierten Skalierung. Als Beispiel wird dabei
eine Temperaturanzeige benutzt. Mittels `setRange` wird der Messbereich des Sensors einprogrammiert. Danach mit `moveOnRange` der Messwert
in eine entsprechende Positionierung des Servomotors umgewandelt.

{{< code file="src/main/java/com/pi4j/crowpi/applications/ServoMotorApp.java" language="java">}}

## Weitere Möglichkeiten

- Anzeige von Messerwerten mittels eines an der Achse montierten Zeigers.

