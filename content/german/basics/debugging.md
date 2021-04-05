---
title: Debugging der Applikation
weight: 20
---

Auf dieser Seite werden die Grundlagen der Fehlersuche mit dem Setup aus [dem ersten Kapitel]({{< ref setup >}}) beschrieben. Es wird
Schritt für Schritt erklärt wie man den Debugger starten und verwenden kann.

---

### Applikation mit Debugger starten

Zum Starten der Applikation mit Debugger werden die beiden Run Konfigurationenen `crowpi-examples [debug]` und `Remote Debug` benötigt.
Diese sind nach dem Setup bereits korrekt eingestellt und können verwendet werden. Wichtig dabei ist die Reihenfolge mit der die
Konfigurationen gestartet werden. Dieses Vorgehen ist:

1. Start von `crowpi-examples [debug]` **ohne** Verwendung vom Debug-Knopf
2. Warten bis die Konsolenausgabe meldet `Listening for transport dt_socket at address: 5005 (Attach debugger)`
3. Starten von `Remote Debug`

Nun dieser Ablauf nochmals kurz mit entsprechenden Abbildungen. Als erstes wird die Applikation im Debugmodus gestartet. Dazu muss die
Konfiguration `crowpi-examples [debug]` angewählt werden. Gestartet wird die Konfiguration mit dem **Play Button**.
{{< img alt="Debugmodus wählen und starten" src="basics/intellij-select-and-start-debug.JPG" >}}

Als Schritt 2 wird nun gewartet bis die korrekte Augabe im Konsolenfenster erscheint. Das sieht dann so aus:
{{< img alt="App wartet auf Debugger" src="basics/intellij-waiting-for-debugger.JPG" height="500px" >}}

Jetzt ist alles bereit um den Remote Debugger zu starten. Dazu die entsprechende Run Konfiguration `Remote Debug` auswählen und mit einem
Klick auf das Käfer-Icon (Debug) starten. Das Debugfenster meldet nun ob die Verbindung geklappt hat.
{{< img alt="Remote Debug starten" src="basics/intellij-start-remote-debug.JPG" >}}
{{< img alt="Remote Debug verbunden" src="basics/intellij-debug-connected.JPG" >}}

Nun kann zwischen den beiden Tabs `Run` und `Debug` hin und her gewechselt werden. Im Tab `Run` wird zum Beispiel die Ausgabe der
Applikation angezeigt. Bei `Debug` finden sich Knöpfe und Informationen zu Breakpoints und Variablen. Wenn nun ein Beispiel ausgeführt wird,
startet dies ganz normal und läuft bis das Programm beendet wird. Solange keine Breakpoints oder ähnliches im Programmablauf vorkommen hat
der Debugger keinen weiteren Einfluss auf die Applikation.

---

### Einsatz von Breakpoints

Wenn nun ein Problem auftritt, wird oft an der entsprechenden Codestelle ein Breakpoint gesetzt. Dieser Breakpoint weist den Debugger an die
laufende Applikation zu unterbrechen und den aktuellen Programmstatus auf dieser Zeile im Code auszugeben. Nun wird an der
Beispielapplikation [7-Segment Anzeige]({{< ref "components/seven-segment"  >}}) demonstriert wie ein Breakpoint verwendet werden kann.
Untersucht soll die Anzeige der `- - - -` werden. Wie im Beispielcode ersichtlich passiert die mit einem `for-loop`. Dieser wird nun genauer
betrachtet.

#### Setzen des Breakpoints an Codestelle

Ein Breakpoint kann in IntelliJ sehr einfach durch Klicken neben die Zeilennummer in einem Programm eingefügt werden. Platziert wird der
Breakpoint hier bewusst vor den zu untersuchenden `for-loop`. So kann sichergestellt werden, dass nichts verpasst wird. Wenn nicht ganz
genau bekannt ist, wo ein Problem verursacht wird, lohnt es sich fast immer den Breakpoint ein paar Zeilen über der kritischen Stelle zu
platzieren. So kann auch die Ausgangslage gut analysiert werden.
{{< img alt="Breakpoint vor Loop gesetzt" src="basics/intellij-setbreakpoint-before-loop.JPG" >}}

#### Starten der Applikation mit Debugger

Wie bereits in den oberen Abschnitten erklärt wird, nachdem der Breakpoint platziert ist, die Applikation im Debugmodus gestartet und der
Debugger angehängt. Es werden also diese 3 Schritte durchgeführt:

1. Start von `crowpi-examples [debug]`` **ohne** Verwendung vom Debug-Knopf
2. Warten bis die Konsolenausgabe meldet `Listening for transport dt_socket at address: 5005 (Attach debugger)`
3. Starten von `Remote Debug`

Da ein Fehler im 7-Segment Beispiel gesucht werden soll, wird das entsprechende Beispielprogramm nach Programmstart angewählt. Die Applikation
läuft sofort bis der Debugger am Breakpoint die Ausführung der Anwendung unterbricht.
{{< img alt="Start von 7-Segment Beispiel" src="basics/intellij-start-sevensegment.JPG" >}}
Hier wird gezeigt wie nun die Ansicht in IntelliJ aussieht, wenn der Debugger das Programm unterbrochen hat und IntelliJ markiert die
entsprechende aktuelle Zeile im Programm. Im Debug-Tab werden zudem die aktuellen Variablen mit Werten der Applikation ausgegeben.
{{< img alt="Example stoppt an Breakpoint" src="basics/intellij-debugger-stopped-at-breakpoint.JPG" >}}

#### Navigation am Breakpoint

Um nun die Programmausführung schrittweise fortzusetzen, verfügt die Entwicklungsumgebung über verschiedene Funktionen. Zum einen gibt es
Grundfunktionen wie das Fortsetzen der Applikation oder das komplette Stoppen der Applikation. Die wichtigsten nun kurz erklärt:

- `Resume Program` setzt das Programm fort bis zum nächsten Breakpoint oder Applikationsende
- `Stop Remote Debug` stoppt den Debugger. Da in diesem Setup jedoch die Applikation und der Debugger getrennt laufen, hat diese Optionen
  keinen sinnvollen Effekt. Zum Stoppen des Programms sollte immer `Stop All` verwendet werden. Dies wird auf dem zweiten Bild gezeigt.
- `Mute Breakpoints / Disable all Breakpoints` der Debugger deaktiviert alle Breakpoints. So wird das Programm bei weiterer Ausführung nicht
  mehr unterbrochen.
  {{< img alt="Steuerung der Applikation" src="basics/intellij-application-controls.JPG" >}}
  {{< img alt="Stopp der Applikation" src="basics/intellij-stop-program.JPG" >}}

Für die schrittweise Ausführung des Programms stellt der Debugger noch weitere Tasten zur Verfügung. Diese sind:

- `Step Over` die nächste Anweisung im Code wird ausgeführt. Bei einem Methodenaufruf wird der Rückgabewert ermittelt ohne die Methode zu
  betreten.
- `Step Into` die nächste Anweisung im Code wird ausgeführt. Bei einem Methodenaufruf wird der Code der Methode geöffnet und das Debugging
  geht in der Methode weiter.
- `Force Step Into` die nächste Anweisung im Code wird ausgeführt. Bei einem Methodenaufruf welcher normalerweise Step Into ignoriert wird
  das eintauchen in die Methode erzwungen.
- `Step Out` die aktuelle Methode wird ausgeführt bis der Code wieder am aufrufenden Ort angekommen ist. Wird zum vorzeitigen Rückkehren
  aus `Step Into` verwendet.
  {{< img alt="Navigation mit Debugger" src="basics/intellij-debug-navigations.JPG" >}}

Noch genauere Erklärungen zu den einzelnen Funktionen können auch direkt in
der [IntelliJ Anleitung](https://www.jetbrains.com/help/idea/stepping-through-the-program.html) gefunden werden.

---

### Analyse von Variablen
Inzwischen wurden einige Steps an der Codestelle des `for-loops` ausgeführt. Nun nochmals ein Blick auf die Variablenliste des Debuggers.
Im auf dem Bild kann nun erkannt werden wie der Debugger dem Benutzer anzeigt welche Variablen nun welche Werte haben. Aktuell ist es 
also:  `i=2` und `state="- "`
Da der `for-loop` erst bei `i > 5` stoppt könnte nun mit dem Debugger jede weitere Iteration nachvollzogen werden.
{{< img alt="Variablen mit Debugger" src="basics/intellij-shows-variables.JPG" >}}
Auf genau diese Weise lassen sich auch Entscheidungen bei `if (this) {then that} else {other things}` nachvollziehen. Jede Variable kann 
auf ihren Zustand geprüft und somit bei Fehlentscheiden die Logik angepasst werden.
