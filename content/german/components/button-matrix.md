---
title: Button Matrix
tags: ["gpio"]
---

## Funktionsweise

Bei einer Button-Matrix handelt es sich einfach gesagt um ein Gitter von Knöpfen, also zum Beispiel 4 × 4 Knöpfe wie beim CrowPi. In der
simpelsten Form könnte jeder Knopf einzeln mit einem GPIO Pin verbunden werden, jedoch stösst man auf diese Art und Weise schnell an die
maximale Kapazität von GPIO Pins eines Raspberry Pi.

Bei einer anderen Methode, welche auch von der Button-Matrix auf dem CrowPi genutzt wird, wird die Kombination der beiden Achsen für das
Auslesen der einzelnen Knöpfe verwendet. In der Komponente werden hierbei sogenannte `Selector` und `Button` Pins definiert, die
dann zusammen das effiziente Anbinden einer Button-Matrix ermöglichen.

Der `Selector` Pin legt hierbei jeweils fest, welche Spalte (vertikal) ausgelesen wird. Nachdem der Pin der gewünschten Spalte
angesteuert wurde, können nun alle Zeilen (horizontal) dieser Spalte mithilfe der einzelnen `Button` Pins ausgelesen werden. Um den zweiten Knopf
von links in der ersten Reihe einzulesen, muss zuerst der zweite `Selector` Pin aktiviert werden (= Spalte 2), um anschliessend den ersten
`Button` Pin (= Zeile 1) auszulesen. Somit werden für ein 4 × 4 Gitter nur 8 statt 12 GPIO Pins benötigt.

Dies bedeutet jedoch auch, dass sich nun nicht mehr einfach so der Zustand eines Pins überwachen lässt, um Events für einen Knopf zu
erhalten. Stattdessen muss nun eine Polling-Methode verwendet werden, sprich es müssen immer wieder Spalte für Spalte alle Knöpfe abgefragt
werden. Diese Komplexität entfällt jedoch bei Nutzung der Komponente, da diese automatisch einen `Poller` im Hintergrund startet.

Die Buttons auf dem CrowPi sind auf der Platine beschriftet und von links nach rechts, oben nach unten von 1 bis 16 durchnummeriert. Die
Komponente nimmt bei allen Methoden jeweils diese Nummer entgegen, sprich `7` entspricht dem dritten Knopf von links in der zweiten Zeile.

{{< img alt="Nummerierung von Button Matrix" src="components/button-matrix.jpg" >}}

## Voraussetzungen

### DIP Switches

Für diese Komponente müssen alle DIP-Switches vom linken Block aktiviert werden, da sich die Button-Matrix sonst nicht oder nur teilweise
nutzen lässt. Die Stellung der DIP Switches sollte anschliessend so aussehen:

{{< dip-switches 1 2 3 4 5 6 7 8 >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.ButtonMatrixComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                                                                                            | Bemerkung                                                                                                                                            |
|:---------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------|
| `ButtonMatrixComponent(com.pi4j.context.Context pi4j)`                                                                                 | Initialisiert eine Button-Matrix mit den Standardeinstellungen für den CrowPi.                                                                       |
| `ButtonMatrixComponent(com.pi4j.context.Context pi4j, int[] selectorPins, int[] buttonPins, int[] stateMappings, long pollerPeriodMs)` | Initialisiert eine Button-Matrix mit frei definierbaren Selector / Button Pins, einem eigenen Mapping sowie einer benutzerdefinierten Polling-Dauer. |

### Methoden

| Methode                                               | Bemerkung                                                                                                                                                                             |
|:------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `void startPoller(long pollerPeriodMs)`               | Startet den Poller (siehe Funktionsweise) mit dem angegebenen Intervall. Diese Methode wird **automatisch** vom Konstruktor aufgerufen und muss normalerweise nicht verwendet werden. |
| `void stopPoller()`                                   | Stoppt den Poller sofort und aktualisiert hiermit den Zustand der Buttons nicht mehr.                                                                                                 |
| `int readBlocking()`                                  | Wartet endlos darauf dass ein Knopf gedrückt und wieder losgelassen wird, um anschliessend dessen Nummer zurückzugeben. Im Fehlerfall wird der Wert -1 zurückgegeben.                 |
| `int readBlocking(long timeoutMs)`                    | Wartet bis zu `timeoutMs` Millisekunden darauf, dass ein Knopf gedrückt und wieder losgelassen wird. Verhält sich ansonsten gleich wie `int readBlocking()`.                          |
| `int[] getPressedButtons()`                           | Gibt die Nummern aller Knöpfe zurück, die zurzeit aktiv gedrückt werden. Falls keine Knöpfe gedrückt werden, so wird eine leere Liste zurückgegeben.                                |
| `boolean isDown(int number)`                          | Gibt `true` zurück falls der Knopf mit der angegebenen Nummer zurzeit gedrückt wird.                                                                                                  |
| `boolean isUp(int number)`                            | Gibt `true` zurück falls der Knopf mit der angegebenen Nummer zurzeit **nicht** gedrückt wird.                                                                                        |
| `ButtonState getState(int number)`                    | Gibt den aktuellen Zustand vom Knopf mit der angegebenen Nummer zurück.                                                                                                               |
| `void onDown(int number, SimpleEventHandler handler)` | Setzt den Event Handler, welcher beim Drücken des angegebenen Knopfs aufgerufen werden soll. `null` deaktiviert diesen Event Listener.                                                 |
| `void onUp(int number, SimpleEventHandler handler)`   | Setzt den Event Handler, welcher beim Loslassen des angegebenen Knopfs aufgerufen werden soll. `null` deaktiviert diesen Event Listener.                                               |

### Enumerationen

- {{< javadoc class="com.pi4j.crowpi.components.ButtonComponent" subclass="ButtonState" >}} enthält alle möglichen Zustände, die von einem
  Knopf zurückgegeben werden können. Es wird hierbei absichtlich die Enumeration von der einfacheren `ButtonComponent` mitverwendet, um eine
  möglichst ähnliche Nutzung zu ermöglichen.

## Beispielapplikation

Die folgende Beispielapplikation ist etwas komplexer und stellt ein vollständiges Spiel auf Basis der Button-Matrix dar. Es gleicht dem
sogenannten Memory Game oder auch dem deutschen Spiel „Ich packe in meinen Koffer“. Zuerst werden mit der Hilfsmethode
`determinePlayers()` alle Spielernamen gesammelt, welche am Spiel teilnehmen sollen. Die Methode sorgt hierbei dafür, dass mindestens zwei
Spieler existieren, da dies eine Voraussetzung vom Spiel darstellt. Diese Spieler werden schliesslich in `players` gespeichert.

Anschliessend wird eine leere Liste namens `history` erzeugt, die jeweils alle vorherigen Knöpfe bzw. deren Nummer beinhaltet. Nun ist
das Spiel vollständig initialisiert und eine `while`-Schleife läuft bis nur noch ein aktiver Spieler existiert und alle anderen Spieler
aufgrund eines falschen Zugs verloren haben.

Innerhalb dieser Schleife wird mit einem Iterator jeweils der nächste Spieler ermittelt. Sobald der Iterator, welcher in der originalen
Reihenfolge über alle Spieler iteriert, am Ende angekommen ist, wird dieser neu erstellt und startet damit wieder am Anfang. So kommt ein
Spieler nach dem anderen zum Zug und dies lässt sich endlos wiederholen.

Nun erfolgen ein paar Ausgaben auf der Kommandozeile und danach eine `for`-Schleife, welche den Spieler auffordert, jeden vorherigen Zug der
in `history` gespeichert wurde, zu wiederholen. Wird hierbei ein Fehler erkannt, so wird die Schleife vorzeitig verlassen und das Flag
`hasFailed` auf `true` gesetzt. Falls dem Spieler keine Fehler passieren, endet die Schleife normal und der Wert von `hasFailed` bleibt auf
`false`.

Ist `hasFailed` nach der Schleife nun gesetzt, so wird der Spieler über seinen Fehler informiert, aus der aktiven Spielerliste gelöscht und
mit `continue` der nächste Spielzug forciert. War hingegen alles richtig, so kann der Spieler nun einen neuen Knopf wählen und der Zug des
nächsten Spielers beginnt.

Wenn irgendwann nur noch ein aktiver Spieler verbleibt, so wird eine Gewinnmeldung und die totale Punktzahl, welche der Anzahl
an wiederholten Elementen entspricht, ausgegeben. Bevor die Applikation endet, wird noch der Poller gestoppt um die Button Matrix nicht länger
abzufragen und somit Ressourcen freizugeben.

{{< code file="src/main/java/com/pi4j/crowpi/applications/ButtonMatrixApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das bestehende Spiel um LCD Display und/oder Buzzer erweitern, um gewisse Ausgaben direkt auf dem CrowPi zu erzeugen und je nach Knopfdruck
  einen anderen Ton / eine andere Frequenz abzuspielen.


