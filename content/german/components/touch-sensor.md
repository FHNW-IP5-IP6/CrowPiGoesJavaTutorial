---
title: Touch Sensor
tags: ["gpio"]
---

## Funktionsweise

Der Berührungssensor funktioniert im Wesentlichen wie jeder andere Knopf auch. Der Unterschied liegt darin das der Berührungs- oder Touch
Sensor nicht gedrückt werden muss. Es reicht schon eine leichte Berührung, um den Sensor auszulösen. Beim CrowPi ist dieser Sensor am GPIO
Pin Nummer 17 angeschlossen. Dabei kennt der Sensor nur zwei Zustände. Er ist entweder gedrückt oder eben nicht. Dies entspricht im Programm
einem `HIGH` oder `LOW`. Der Sensor erkennt die Berührung durch die Veränderung des elektrischen Widerstandes durch den Kontakt mit der
Haut. Es gibt auch andere Varianten von Berührungserkennung. Diese sind jedoch viel komplexer und werden zum Beispiel in Mobiltelefonen
eingesetzt.

Eine oft verwendete Funktion bei digitalen Eingängen ist die Entprellung. Dies bedeutet das nach jeder Statusänderung des Eingangs jeweils
erst eine «Abkühlzeit» verstreichen muss. Solche Mehrfachauslösungen kommen fast bei jeder Art von Hardware-Knopf vor und sollten deshalb
immer berücksichtigt werden. Wird die Entprellzeit zu kurz gewählt, kann es vorkommen, dass durch 1x Betätigen mehrere Events ausgelöst
werden. Das wäre natürlich unerwünschtes Verhalten der Software.

Die boolesche Verhaltensweise des Sensors macht die Benutzung während des Programmierens sehr einfach. Die Herausforderung liegt
dabei meist mehr beim Gerüst rund um den Sensor herum. Zum Beispiel im Eventhandling, wenn der Knopf jederzeit funktionieren soll.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.TouchSensorComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                                       | Bemerkung                                                                                                                                                   |
|:----------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `TouchSensorComponent(com.pi4j.context.Context pi4j)`                             | Initialisiert einen Touch Sensor mit dem Standard-Pin für den CrowPi.                                                                                       |
| `TouchSensorComponent(com.pi4j.context.Context pi4j, int address, long debounce)` | Initialisiert einen Touch Sensor mit einem benutzerdefinierten Pin. Zusätzlich kann mit `debounce` noch eine Entprellzeit in Mikrosekunden angegeben werden |

### Methoden

| Methode                 | Bemerkung                                                                                                                    |
|:------------------------|:-----------------------------------------------------------------------------------------------------------------------------|
| `boolean isTouched()`   | Gibt `true` zurück falls der Berührungssensor gerade betätigt wird ansonsten `false`.                                        |
| `TouchState getState()` | Gibt den aktuellen Zustand des Berührungssensor zurück.                                                                      |
| `void onTouch()`        | Setzt den Event Handler welcher beim Berühren des Sensors aufgerufen werden soll. `null` deaktiviert diesen Event Listener.  |
| `void onRelease()`      | Setzt den Event Handler welcher beim Loslassen des Sensors aufgerufen werden soll. `null` deaktiviert diesen Event Listener. |

### Enumerationen

- {{< javadoc class="com.pi4j.crowpi.components.TouchSensorComponent" subclass="TouchState" >}} enthält alle möglichen Zustände, die vom
  Touch Sensor zurückgegeben werden können.

## Beispielapplikation

Die nachfolgende Beispielapplikation registriert zwei Event Listener auf dem Touch Sensor. Diese Listener lösen dann für 20 Sekunden jeweils
ein minimales Event aus. Es wird jedes Mal in dieser Zeit, wenn der Sensor berührt oder losgelassen wird ein kleiner Text mit dem Status
ausgegeben. Nach Ablauf der Zeit werden die Listener wieder vom Berührungssensor entfernt und mittels der `isTouched()` Methode gewartet bis
der Benutzer die Beispielapplikation beendet. Durch Verwendung des Konstruktors mit nur einem Argument wird in dieser Applikation die
Standardeinstellung für die Entprellung benutzt. Diese liegt bei 10'000 Mikrosekunden und passt ganz gut für die meisten menschlichen
Eingaben. {{< code file="src/main/java/com/pi4j/crowpi/applications/TouchSensorApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Beispiel um beliebige Abfolgen von Berührungen erweitern. So wäre zum Beispiel eine Kombination aus langen und kurzen Berührungen
  möglich um ein kleines Spiel zu entwickeln.

- Umsetzung des Morsealphabets mittels Touch Sensor und Events

