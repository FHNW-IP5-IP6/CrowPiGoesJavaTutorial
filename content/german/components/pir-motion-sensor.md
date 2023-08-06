---
title: PIR Motion Sensor
tags: ["gpio"]
---

## Funktionsweise

Beim PIR Motion Sensor handelt es sich um einen Bewegungssensor, der mit passivem Infrarot arbeitet. Diese Komponente nutzt einen
pyroelektrischen Sensor, welcher Infrarot-Strahlung erkennen kann. Da jedes Lebewesen und Objekt abhängig von seiner jeweiligen Temperatur
eine unterschiedliche Menge an Infrarot-Strahlen emittiert, ist der Sensor in der Lage diese zu erkennen.

Um nun jedoch nicht das Grundrauschen zu messen, sondern effektiv Bewegung zu erkennen, ist der Sensor in zwei Hälften aufgeteilt, welche
sich gegenseitig bei Stillstand wieder aufheben. Findet nun aber eine Bewegung statt, sehen die beiden Hälften unterschiedlich viel
Strahlung, was anschliessend per GPIO an den Raspberry Pi als erkannte Bewegung übermittelt werden kann.

Mit dieser Komponente lässt sich zwar nicht feststellen wie viele Personen in der Nähe sind oder wie nahe sich diese am Sensor befinden,
jedoch ist die vorhandene Funktionalität meist ausreichend. Somit finden Bewegungssensoren von diesem Typ auch oft Verwendung im Alltag, zum
Beispiel für Aussenbeleuchtungen, die automatisch eingeschaltet werden sollen.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.PirMotionSensorComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                            | Bemerkung                                                                  |
|:-----------------------------------------------------------------------|:---------------------------------------------------------------------------|
| `PirMotionSensorComponent(com.pi4j.context.Context pi4j)`              | Initialisiert einen PIR Motion Sensor mit dem Standard-Pin für den CrowPi. |
| `PirMotionSensorComponent(com.pi4j.context.Context pi4j, int address)` | Initialisiert einen PIR Motion Sensor mit einem benutzerdefinierten Pin.   |

### Methoden

| Methode                                         | Bemerkung                                                                                                                  |
|:------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------|
| `MotionState getState()`                        | Gibt den aktuellen Zustand vom PIR Motion Sensor zurück.                                                                   |
| `boolean hasMovement()`                         | Überprüft ob der PIR Motion Sensor zurzeit Bewegung erkennt.                                                               |
| `boolean hasStillstand()`                       | Überprüft ob der PIR Motion Sensor zurzeit Stillstand erkennt.                                                             |
| `void onMovement(SimpleEventHandler handler)`   | Setzt den Event Handler welcher bei erkannter Bewegung aufgerufen werden soll. `null` deaktiviert diesen Event Listener.   |
| `void onStillstand(SimpleEventHandler handler)` | Setzt den Event Handler welcher bei erkanntem Stillstand aufgerufen werden soll. `null` deaktiviert diesen Event Listener. |

### Enumerationen

- {{< javadoc class="com.pi4j.crowpi.components.PirMotionSensorComponent" subclass="MotionState" >}} enthält alle möglichen Zustände, die
  vom PIR Motion Sensor zurückgegeben werden können.

## Beispielapplikation

Die nachfolgende Beispielapplikation stellt eine vereinfachte Alarmanlage dar, die zuerst darauf wartet, dass der Sensor keine Bewegung
mehr erkennt. Hierfür wird eine endlose `while`-Schleife verwendet, die zwischen jeder Überprüfung zuerst eine Sekunde wartet, um nicht
unnötige Rechenlast zu erzeugen.

Anschliessend wird die Alarmanlage aktiviert und überwacht für 30 Sekunden sämtliche auftretenden Zustandsänderungen vom Bewegungssensor.
Hierfür werden die Funktionen `onMovement()` und `onStillstand()` benötigt, welche Event Handler zur Protokollierung von Änderungen
registrieren. Nach 30 Sekunden wird die Anwendung automatisch beendet.

{{< code file="src/main/java/com/pi4j/crowpi/applications/PirMotionSensorApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Beispiel mit dem Buzzer kombinieren, um einen Alarmton abzuspielen, wenn der Zustand von `STILLSTAND` auf `MOVEMENT` ändert.


