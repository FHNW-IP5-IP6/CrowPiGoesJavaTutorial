---
title: Debugging der Applikation
weight: 30
---

Auf dieser Seite werden die Grundlagen der Fehlersuche mit dem Setup aus [dem ersten Kapitel]({{< ref setup >}}) beschrieben. Dazu wird Schritt für Schritt erklärt, wie man den Debugger starten und verwenden kann.

---

### Applikation mit Debugger starten

Zum Starten der Applikation mit Debugger werden die beiden Run Konfigurationen `Debug on CrowPi` und `Attach to CrowPi Debugger` benötigt.

Diese sind nach dem Setup bereits korrekt eingestellt und können verwendet werden. Wichtig dabei ist die Reihenfolge, mit der die Konfigurationen gestartet werden. Dieses Vorgehen ist:

1. Start von `Debug on CrowPi` mit dem  *Run*-Knopf (*nicht* mit dem Debug-Knopf)
2. Warten bis die Konsolenausgabe meldet `Listening for transport dt_socket at address: 5005 (Attach debugger)`
3. Starten von `Attach to CrowPi Debugger`

Nachfolgend wird der Ablauf mit entsprechenden Abbildungen ergänzt. Als Erstes wird die Applikation im Debugmodus gestartet. Dazu muss die Konfiguration `Debug on CrowPi` angewählt werden. Gestartet wird die Konfiguration mit dem **Play Button**.
{{< img alt="Debugmodus wählen und starten" src="basics/intellij-select-and-start-debug.JPG" >}}

Als Zweites wird nun gewartet, bis die korrekte Ausgabe im Konsolenfenster erscheint:
{{< img alt="App wartet auf Debugger" src="basics/intellij-waiting-for-debugger.JPG" height="500px" >}}

Zum Starten des Remote Debugger die entsprechende Run Konfiguration `Attach to CrowPi Debugger` auswählen und mit einem Klick auf das Käfer-Icon (Debug) starten. Das Debugfenster meldet nun, ob eine Verbindung hergestellt wurde.
{{< img alt="Remote Debug starten" src="basics/intellij-start-remote-debug.JPG" >}}
{{< img alt="Remote Debug verbunden" src="basics/intellij-debug-connected.JPG" >}}

Nun kann zwischen den beiden Tabs `Run` und `Debug` hin und her gewechselt werden. 
- Im Tab `Run` wird die Ausgabe der Applikation angezeigt. 
- Bei `Debug` finden sich Knöpfe und Informationen zu Breakpoints und Variablen.  

Wenn nun ein Beispiel ausgeführt wird, startet dies ganz normal und läuft, bis das Programm beendet wird. Solange keine Breakpoints oder ähnliches im Programmablauf vorkommen, hat der Debugger keinen weiteren Einfluss auf die Applikation.

---

### Einsatz von Breakpoints

Wenn nun ein Problem auftritt, wird oft an der entsprechenden Codestelle ein Breakpoint gesetzt. Dieser Breakpoint weist den Debugger an, die laufende Applikation zu unterbrechen und den aktuellen Programmstatus auf dieser Zeile im Code auszugeben. An der
Beispielapplikation [7-Segment Anzeige]({{< ref "components/seven-segment"  >}}) wird demonstriert, wie ein Breakpoint verwendet werden kann. Untersucht soll die Anzeige der `- - - -` werden. Wie im Beispielcode ersichtlich passiert dies mit einem `for-loop`. 

#### Setzen des Breakpoints an Codestelle

Ein Breakpoint kann in IntelliJ sehr einfach durch Klicken neben die Zeilennummer in einem Programm eingefügt werden. Platziert wird der Breakpoint hier bewusst vor den zu untersuchenden `for-loop`. So kann sichergestellt werden, dass nichts verpasst wird. Wenn nicht genau bekannt ist, wo ein Problem verursacht wird, lohnt es sich fast immer den Breakpoint ein paar Zeilen über der kritischen Stelle zu
platzieren. So kann die Ausgangslage gut analysiert werden.
{{< img alt="Breakpoint vor Loop gesetzt" src="basics/intellij-setbreakpoint-before-loop.JPG" >}}

#### Starten der Applikation mit Debugger

Wie vorgängig erklärt, wird zuerst der Breakpoint platziert, die Applikation im Debugmodus gestartet und der Debugger angehängt. Es werden also diese 3 Schritte durchgeführt:

1. Start von `Debug on CrowPi` **ohne** Verwendung vom Debug-Knopf
2. Warten bis die Konsolenausgabe meldet `Listening for transport dt_socket at address: 5005 (Attach debugger)`
3. Starten von `Attach to CrowPi Debugger`

Da ein Fehler im 7-Segment Beispiel gesucht werden soll, wird das entsprechende Beispielprogramm nach Programmstart angewählt. Die Applikation wird ausgeführt, bis der Debugger am Breakpoint die Ausführung der Anwendung unterbricht.
{{< img alt="Start von 7-Segment Beispiel" src="basics/intellij-start-sevensegment.JPG" >}}

Nachfolgend die Ansicht in IntelliJ, wenn der Debugger das Programm unterbrochen hat. IntelliJ markiert die entsprechende aktuelle Zeile im Programm. Im Debug-Tab werden zudem die aktuellen Variablen mit Werten der Applikation ausgegeben.
{{< img alt="Example stoppt an Breakpoint" src="basics/intellij-debugger-stopped-at-breakpoint.JPG" >}}

#### Navigation am Breakpoint

Um nun die Programmausführung schrittweise fortzusetzen, verfügt die Entwicklungsumgebung über verschiedene Funktionen. Diese beinhalten Grundfunktionen wie das Fortsetzen der Applikation oder das komplette Stoppen der Applikation. Die wichtigsten kurz erklärt:

- `Resume Program` setzt das Programm fort bis zum nächsten Breakpoint oder Applikationsende
- `Stop Remote Debug` stoppt den Debugger. Da in diesem Setup jedoch die Applikation und der Debugger getrennt laufen, hat die Option
  keinen sinnvollen Effekt. Zum Stoppen des Programms sollte immer `Stop All` verwendet werden. Dies wird auf dem zweiten Bild gezeigt.
- `Mute Breakpoints / Disable all Breakpoints` der Debugger deaktiviert alle Breakpoints. So wird das Programm bei weiterer Ausführung nicht
  mehr unterbrochen.
  {{< img alt="Steuerung der Applikation" src="basics/intellij-application-controls.JPG" >}}
  {{< img alt="Stopp der Applikation" src="basics/intellij-stop-program.JPG" >}}

Für die schrittweise Ausführung des Programms stellt der Debugger noch weitere Tasten zur Verfügung:

- `Step Over` die nächste Anweisung im Code wird ausgeführt. Bei einem Methodenaufruf wird der Rückgabewert ermittelt, ohne die Methode zu
  betreten.
- `Step Into` die nächste Anweisung im Code wird ausgeführt. Bei einem Methodenaufruf wird der Code der Methode geöffnet und das Debugging
  geht in der Methode weiter.
- `Force Step Into` die nächste Anweisung im Code wird ausgeführt. Bei einem Methodenaufruf, welcher normalerweise bei Step Into ignoriert, wird
  das Eintauchen in die Methode erzwungen.
- `Step Out` die aktuelle Methode wird ausgeführt, bis der Code wieder am aufrufenden Ort angekommen ist. Wird zum vorzeitigen Rückkehren
  aus `Step Into` verwendet.
  {{< img alt="Navigation mit Debugger" src="basics/intellij-debug-navigations.JPG" >}}

Noch genauere Erklärungen zu den einzelnen Funktionen können auch direkt 
der [IntelliJ Anleitung](https://www.jetbrains.com/help/idea/stepping-through-the-program.html) entnommen werden.

---

### Analyse von Variablen
Inzwischen wurden einige Steps an der Codestelle des `for-loops` ausgeführt. Nun nochmals ein Blick auf die Variablenliste des Debuggers.

Auf dem Bild kann erkannt werden, wie der Debugger dem Benutzer anzeigt welche Variablen nun welche Werte haben. Aktuell ist es also:  `i=2` und `state="- "`

Da der `for-loop` erst bei `i > 5` stoppt, könnte nun mit dem Debugger jede weitere Iteration nachvollzogen werden.
{{< img alt="Variablen mit Debugger" src="basics/intellij-shows-variables.JPG" >}}

Auf diese Weise lassen sich auch Entscheidungen bei `if (this) {then that} else {other things}` nachvollziehen. Jede Variable kann auf ihren Zustand geprüft und somit bei Fehlentscheiden die Logik angepasst werden.
