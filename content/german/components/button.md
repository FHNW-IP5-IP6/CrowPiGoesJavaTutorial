---
title: Button
tags: ["gpio"]
---

## Funktionsweise

Bei einem Button handelt es sich schlicht um einen ganz einfachen Knopf, der entweder gedrückt wird oder eben nicht. Auf dem CrowPi
stehen neben der [Button Matrix]({{< ref "components/button-matrix" >}}) auch vier unabhängige Knöpfe zur Verfügung, die mit den
Richtungen Up (oben), Down (unten), Left (links), Right (rechts) bezeichnet sind.

Bei dieser Komponente wird der entsprechende GPIO-Pin direkt ausgelesen und ohne weitere Verarbeitung ausgewertet. Somit lässt
sich diese Komponente sehr einfach verwenden und kann dennoch für viele unterschiedliche Zwecke eingesetzt werden.

## Voraussetzungen

### DIP Switches

Für diese Komponente müssen die DIP Switches 1-5, 1-6, 1-7 sowie 1-8 aktiviert werden, da sich die Buttons sonst nicht oder nur eingeschränkt
nutzen lassen. Die Stellung der DIP Switches sollte anschliessend so aussehen:

{{< dip-switches 5 6 7 8 >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.ButtonComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                                                         | Bemerkung                                                                                                                                                                             |
|:----------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ButtonComponent(com.pi4j.context.Context pi4j pi4j, Button button)`                                | Initialisiert einen Button für den angegebenen Knopf vom CrowPi.                                                                                                                      |
| `ButtonComponent(com.pi4j.context.Context pi4j pi4j, int address, boolean inverted, long debounce)` | Initialisiert einen Button mit einer benutzerdefinierten Adresse und Entprellzeit. Der Parameter `inverted` kann gesetzt werden um einen Button mit Pull-Down Verfahren anzusprechen. |

### Methoden

| Methode                                   | Bemerkung                                                                                                                    |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------|
| `boolean isDown()`                        | Gibt `true` zurück falls der Knopf zurzeit gedrückt wird, ansonsten `false`.                                                 |
| `boolean isUp()`                          | Gibt `true` zurück falls der Knopf zurzeit **nicht** gedrückt wird, ansonsten `true`                                         |
| `ButtonState getState()`                  | Gibt den aktuellen Zustand vom Knopf zurück.                                                                                 |
| `void onDown(SimpleEventHandler handler)` | Setzt den Event Handler welcher beim Drücken des Knopfs aufgerufen werden soll. `null` deaktiviert diesen Event Listener.    |
| `void onUp(SimpleEventHandler handler)`   | Setzt den Event Handler welcher beim Loslassen des Sensors aufgerufen werden soll. `null` deaktiviert diesen Event Listener. |

### Enumerationen

- {{< javadoc class="com.pi4j.crowpi.components.ButtonComponent" subclass="ButtonState" >}} enthält alle möglichen Zustände, die vom Knopf
  zurückgegeben werden können.

- {{< javadoc class="com.pi4j.crowpi.components.definitions.Button" >}} enthält die Pins der vier verschiedenen Knöpfe auf dem CrowPi und
  wird im Konstruktor verwendet werden, um den gewünschten Knopf anzugeben.

## Beispielapplikation

Die Beispielapplikation ist sehr simpel gehalten und initialisiert alle 4 Knöpfe, um anschliessend auf jedem Knopf einen Event Handler für
gedrückt (onDown) sowie losgelassen (onUp) zu registrieren. Anschliessend schläft die Applikation für 15 Sekunden, um ein Testen der
verschiedenen Knöpfe zu ermöglichen.

{{< code file="src/main/java/com/pi4j/crowpi/applications/ButtonApp.java" language="java" >}}

## Weitere Möglichkeiten

- Mit den vier Knöpfen lassen sich leicht diverse Spiele realisieren, da jeder Knopf in eine bestimmte Richtung zeigt, was sich als Steuerung
  für einen Spieler beispielsweise verwenden lässt.

- Mit den vier Knöpfen lässt sich der Cursor auf dem LCD Display steuerbar machen, um an eine beliebige Stelle zu navigieren.

