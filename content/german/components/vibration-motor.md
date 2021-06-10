---
title: Vibrationsmotor
tags: ["gpio"]
---

## Funktionsweise

Beim Vibrationsmotor handelt es sich um einen sehr kleinen Motor, der über eine kleine Unwucht verfügt. Durch diesen Fehler im Rundlauf
entsteht die gewünschte Vibration. Aufgrund des elektrischen Anschlusses beim CrowPi ist jedoch nur ein Ein- und Ausschalten des Motors
möglich. Theoretisch wäre aber auch
[PWM]({{< ref "hardware/pwm" >}}) denkbar. Jedoch verfügt der verwendete Pin über keine
[PWM]({{< ref "hardware/pwm" >}}) Funktionalität.

## Voraussetzungen

### DIP Switches

Für diese Komponente muss einer der DIP-Switches auf die folgende Position eingestellt werden:

{{< dip-switches 9 >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.RelayComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                           | Bemerkung                                                                |
|:----------------------------------------------------------------------|:-------------------------------------------------------------------------|
| `VibrationMotorComponent(com.pi4j.context.Context pi4j)`              | Initialisiert einen Vibrationsmotor mit dem Standard-Pin für den CrowPi. |
| `VibrationMotorComponent(com.pi4j.context.Context pi4j, int address)` | Initialisiert einen Vibrationsmotor mit einem benutzerdefinierten Pin.   |

### Methoden

| Methode                     | Bemerkung                                                                                                              |
|:----------------------------|:-----------------------------------------------------------------------------------------------------------------------|
| `void on()`                 | Schaltet den Vibrationsmotor ein.                                                                                      |
| `void off()`                | Schaltet den Vibrationsmotor aus.                                                                                      |
| `boolean toggle()`          | Wechselt den Zustand des Vibrationsmotors abhängig vom aktuellen Zustand. Gibt den neuen Zustand als `boolean` zurück. |
| `void pulse(int interval)`  | Schaltet den Vibrationsmotor für die angegebene Zeit in Millisekunden ein.                                             |
| `void setState(boolean on)` | Setzt den aktuellen Zustand des Vibrationsmotors auf den Wert des `boolean on`.                                        |

## Beispielapplikation

Die Beispielapplikation führt erst einen kleinen Test mit Ein- und Ausschalten dem Vibrationsmotor durch. Anschliessend wird durch gezielte
Variation der Pulslänge versucht einen möglichst nervigen Ton zu erstellen. Dadurch soll der Benutzer aufgeweckt werden, falls er vor dem PC
eingeschlafen ist. Sobald der Benutzer das Aufwachen mit `YES` bestätigt wird die Applikation beendet.

{{< code file="src/main/java/com/pi4j/crowpi/applications/VibrationMotorApp.java" language="java" >}}

## Weitere Möglichkeiten

- Einen benutzerdefinierten Vibrationsalarm einrichten unter Verwendung eines spezifischen Pulsates.
- In Kombination mit Knöpfen und Sensoren kann der Vibrationsmotor verwendet werden, um den Benutzer auf etwas aufmerksam zu machen.

