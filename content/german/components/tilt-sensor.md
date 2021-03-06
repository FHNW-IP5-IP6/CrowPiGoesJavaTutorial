---
title: Tilt Sensor
tags: ["gpio"]
---

## Funktionsweise

Ein Tilt Sensor, auf Deutsch Neigungs- oder Kippsensor, erkennt wenn dieser in eine bestimmte Richtung geneigt wird. Es existieren hier
einige verschiedene Ausführungen, die Komponente auf dem CrowPi basiert jedoch auf einem `SW-200D`, welcher nur eine Richtung kennt und
somit lediglich eine Neigung nach links (`HIGH`) oder rechts (`LOW`) erkennen kann.

Hierfür ist der silberne Draht auf der linken Seite der Komponente mit einer leitenden Innenhülle verbunden, in welcher sich zwei kleine
Metallkugeln befinden. Innerhalb dieser Hülle ist auf der rechten Seite ein Kontaktpunkt vorhanden, welcher mit dem goldenen Draht auf der
rechten Seite der Komponente verbunden ist. Abhängig von der Neigung berühren diese Kugeln nun den Kontaktpunkt oder eben nicht, sodass sich
der aktuelle Zustand jeweils einfach via GPIO erkennen lässt.

Diese Art von Sensor liefert somit zwar nicht viele Daten, wird aber trotzdem oft in der Robotik verwendet aufgrund der sehr einfachen
Einsatzweise und dem niedrigen Stückpreis.

## Voraussetzungen

### DIP Switches

Für diese Komponente muss der DIP Switch 2-2 aktiviert werden, da ansonsten der Tilt Sensor nicht mit dem CrowPi verbunden ist. Die Stellung
der DIP Switches sollte anschliessend so aussehen:

{{< dip-switches 10 >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.TiltSensorComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                                      | Bemerkung                                                                                                                                                   |
|:---------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `TiltSensorComponent(com.pi4j.context.Context pi4j)`                             | Initialisiert einen Tilt Sensor mit dem Standard-Pin für den CrowPi.                                                                                        |
| `TiltSensorComponent(com.pi4j.context.Context pi4j, int address, long debounce)` | Initialisiert einen Tilt Sensor mit einem benutzerdefinierten Pin. Zusätzlich kann mit `debounce` noch eine Entprellzeit in Mikrosekunden angegeben werden. |

### Methoden

| Methode                                                   | Bemerkung                                                                                                                                                                                           |
|:----------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `TiltState getState()`                                    | Gibt den aktuellen Zustand vom Tilt Sensor zurück.                                                                                                                                                  |
| `boolean hasLeftTilt()`                                   | Überprüft ob der Tilt Sensor nach links geneigt ist.                                                                                                                                                |
| `boolean hasRightTilt()`                                  | Überprüft ob der Tilt Sensor nach rechts geneigt ist.                                                                                                                                               |
| `void onTiltLeft(SimpleEventHandler handler)`             | Setzt den Event Handler welcher beim Neigen nach Links aufgerufen werden soll. `null` deaktiviert diesen Event Listener.                                                                            |
| `void onTiltRight(SimpleEventHandler handler)`            | Setzt den Event Handler welcher beim Neigen nach Rechts aufgerufen werden soll. `null` deaktiviert diesen Event Listener.                                                                           |
| `void onShake(SimpleEventHandler handler)`                | Setzt den Event Handler welcher beim Schütteln, sprich 3x Zustandswechsel in kurzer Zeit, aufgerufen werden soll. `null` deaktiviert diesen Event Listener.                                         |
| `void onShake(int threshold, SimpleEventHandler handler)` | Setzt den Event Handler welcher beim Schütteln aufgerufen werden soll. Der Parameter `threshold` definiert die Anzahl von Zustandswechseln. `null` als `handler` deaktiviert diesen Event Listener. |

### Enumerationen

- {{< javadoc class="com.pi4j.crowpi.components.TiltSensorComponent" subclass="TiltState" >}} enthält alle möglichen Zustände, welche vom Tilt
  Sensor zurückgegeben werden können.

## Beispielapplikation

Die nachfolgende Beispielapplikation gibt zuerst den aktuellen Zustand des Tilt Sensors aus, welcher je nach Lage / Positionierung des
CrowPi variieren kann. Danach werden zwei Event Listener mit `onTiltLeft()` bzw. `onTiltRight()` registriert, welche bei jedem
Zustandswechsel des Sensors eine Nachricht auf die Konsole ausgibt. Somit wird nun eine Nachricht ausgegeben, wenn der CrowPi
abwechslungsweise nach links oder rechts geneigt wird. Zudem wird noch ein Event Listener für `onShake()` registriert, welcher beim
Schütteln des CrowPi eine spezielle Meldung ausgibt.

Die Applikation wartet nun 20 Sekunden, um dem Anwender genug Zeit zu geben die Event Listener zu testen. Nach den 20 Sekunden werden die
Event Listener wieder entfernt und die Applikation beendet sich. Das Entfernen der Event Listeners ist nicht explizit notwendig, da dies
beim Beenden der JVM sowieso passiert, jedoch empfohlen.

{{< code file="src/main/java/com/pi4j/crowpi/applications/TiltSensorApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Beispiel um den Buzzer zu erweitern, sodass eine Alarmierung stattfindet, wenn der CrowPi in eine bestimmte Richtung geneigt ist.

- Automatisch aktualisierende Darstellung der aktuellen Neigung auf der LED Matrix oder der LCD Anzeige.

