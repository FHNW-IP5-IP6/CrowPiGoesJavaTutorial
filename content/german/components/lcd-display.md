---
title: LCD-Display
tags: ["gpio", "i2c"]
---

## Funktionsweise
Beim LCD-Display handelt es sich um eine der komplexesten Komponenten des CrowPi. Die betrifft sowohl die Ansteuerung in der Software als 
auch die Funktionalität des Displays an sich. Das LCD-Display verfügt auf 2 Zeilen jeweils über 16 kleine Pixelfelder. Diese bestehen
aus je 5x8 Pixeln. Jedes dieser Pixel wird von den Mikrocontrollern des Displays einzeln angesteuert um so Buchstaben, Zahlen und 
Sonderzeichen zu zeigen. Auf diese Weise können also bis zu 32 Zeichen gleichzeitig angezeigt werden. Damit nun der Benutzer nicht jedes 
Pixel einzeln ansteuern muss, sind schon die wichtigsten Zeichen vorprogrammiert. Es können ´a-Z, 0–9 und einige Sonderzeichen´ einfach 
genutzt werden. Um dem Benutzer noch mehr Möglichkeiten zu offerieren, können zudem noch 8 benutzerdefinierte Zeichen erfasst werden.
Durch diese vielfältigkeit und doch schon grosse Menge an Platz für die Anzeige von Text und Zahlen bieten sich unzählige 
Anwendungsmöglichkeiten für ein solches LCD-Display.

Die Ansteuerung des Displays erfolgt über einfache digitale Signale. Da jedoch 8 Stück benutzt werden und der Raspberry Pi nur wenige 
zur Verfügung hat gibt es eine kleine Zwischenschaltung. Es wird ein Baustein verwendet welcher mittels [I²C]({{< ref "hardware/i2c" >}})
einige zusätzliche digitale Ein- und Ausgänge zur Verfügung stellt. Auf dem Bild ist diese Verschaltung nochmals skizziert:
{{< img alt="Schaltung des LCD-Display" src="components/lcd-display-wiring.jpg" >}}

Die Helligkeit des Displays wird bei dem Aufbau im CrowPi mit einem Potentiometer eingestellt. Mit dem Schraubenzieher kann dieses so 
eingestellt werden bis das Display einen optimalen Kontrast hat und alles gut lesbar ist. Das Potentiometer befindet sich direkt links 
neben dem Display. Es ist relativ klein und kann gut übersehen werden. Auf dem folgenden Bild wird es daher nochmals kurz gezeigt.
{{< img alt="Potentiometer des LCD-Display" src="components/potentiometer-lcd-display.png" height="500px" >}}

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, so dass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="ch.fhnw.crowpi.components.LcdDisplayComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor | Bemerkung |
| --- | --- |
| `TouchSensorComponent(com.pi4j.context.Context pi4j)` | Initialisiert einen Touch Sensor mit dem Standard-Pin für den CrowPi. |
| `TouchSensorComponent(com.pi4j.context.Context pi4j, int address, long debounce)` | Initialisiert einen Touch Sensor mit einem benutzerdefinierten Pin. Zusätzlich kann mit `debounce` noch eine Entprellzeit in Mikrosekunden angegeben werden  |

### Methoden

| Methode | Bemerkung |
| --- | --- |
| `boolean isTouched()` | Gibt `true` zurück falls der Berührungssensor gerade betätigt wird ansonsten `false`. |
| `TouchState getState()` | Gibt den aktuellen Zustand des Berührungssensor zurück. |
| `EventListener addListener(EventHandler<TouchState> handler)` | Registriert einen Event Listener, welcher bei jeder Veränderung aufgerufen wird. |
| `void removeListener(EventListener listener)` | Entfernt einen zuvor registrierten Event Listener. |

### Enumerationen

- {{< javadoc class="ch.fhnw.crowpi.components.TouchSensorComponent" subclass="TouchState" >}} enthält alle möglichen Zustände welche vom
  Touch Sensor zurückgegeben werden können.

## Beispielapplikation

Die nachfolgende Beispielapplikation registriert einen Listener auf dem Touch Sensor. Dieser Listener löst dann für 20 Sekunden ein
minimales Event aus. Es wird jedes Mal in dieser Zeit, wenn der Sensor berührt oder losgelassen wird ein kleiner Text mit dem Status
ausgegeben. Nach Ablauf der Zeit wird der Listener wieder vom Berührungssensor entfernt und mittels der `isTouched()` Methode gewartet bis
der Benutzer die Beispielapplikation beendet. Durch Verwendung des Konstruktors mit nur einem Argument wird in dieser Applikation die
Standardeinstellung für die Entprellung benutzt. Diese liegt bei 10'000 Mikrosekunden und passt ganz gut für die meisten menschlichen
Eingaben. 

{{< code file="src/main/java/ch/fhnw/crowpi/applications/LcdDisplayApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Beispiel um beliebige Abfolgen von Berührungen erweitern. So wäre zum Beispiel eine Kombination aus langen und kurzen Berührungen
  möglich um ein kleines Spiel zu entwickeln.

- Umsetzung des Morsealphabets mittels Touch Sensor und Events
