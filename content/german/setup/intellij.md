---
title: IntelliJ IDEA Setup
weight: 40  
disableToc: true
---

## Installieren von IntelliJ IDEA

In diesem Tutorial wird die Entwicklung mit Hilfe der Entwicklungsumgebung IntelliJ IDEA umgesetzt. Entsprechend sind auch die Artikel, Anweisungen und Bilder jeweils damit erstellt. IntelliJ IDEA kann bei [Jetbrains](https://www.jetbrains.com/idea/) in verschiedenen Versionen bezogen werden. 

Für die Entwicklung mit dem CrowPi verfügt die Community Version bereits über genügend Funktionalität. Für Studierende der Fachhochschule Nordwestschweiz ist jedoch die Ultimate Version empfehlenswert, die ebenfalls kostenfrei erhältlich ist. Die Entwicklungsumgebung ist sowohl für Windows, macOS als auch Linux verfügbar. Der Download findet sich hier: [Download IntelliJ IDEA](https://www.jetbrains.com/idea/download/). Das anschliessende Einrichten von IntelliJ IDEA verhält sich ebenfalls auf allen Plattformen identisch.

Sehr empfehlenswert ist die Installation via die [Toolbox App](https://www.jetbrains.com/toolbox-app/).

---

### 1. Klonen des Repository

Sobald wir nun die Entwicklungsumgebung installiert haben geht es an das Klonen des Einstiegsprojekts für den CrowPi. Dabei findet sich der Sourcecode auf [GitHub]({{% siteparam "repoURL" %}}). Um das Repository zu klonen, muss auf GitHub der entsprechende Link kopiert werden und dann in IntelliJ IDEA importiert werden. Dazu schrittweise in Bildern eine etwas genauere Anleitung.

Um Konflikte mit dem offiziellen Repository zu vermeiden, ist es jedoch am besten seine eigene Kopie zu erstellen. Besuche hierfür zuerst 
[das CrowPi Repository]({{% siteparam "repoURL" %}}) auf GitHub und klicke auf den angezeigten Knopf `Use this template` um dein eigenes Repository mit dem gleichen Inhalt zu erhalten:

{{< img alt="Neues Repository erzeugen via Template" src="setup/github-template-step1.png" height="500px" >}}

Nun erscheint eine neue Seite, wo ein beliebiger Name für das eigene Repository vergeben werden muss. Die weiteren Optionen können nach Belieben angepasst werden, sind jedoch nicht erforderlich. Mit einem Klick auf `Create repository for this template` wird nun eine Kopie vom Repository erzeugt:

{{< img alt="Informationen für neues Repository angeben" src="setup/github-template-step2.png" height="500px" >}}

Nach einer kurzen Ladezeit steht das eigene Repository anschliessend zur Verfügung. Nun ist auf den angezeigten grünen Knopf `Code` zu klicken, um den Link zum eigenen Repository zu erhalten, welcher im nächsten Schritt im IntelliJ angegeben wird:

{{< img alt="URL für eigenes Repository finden" src="setup/github-template-step3.png" height="500px" >}}

---

### 2. Importieren des Projekts

Im Startfenster von IntelliJ ist die Option `Get from VCS` verfügbar. Diese muss angewählt werden damit direkt aus GitHub der Code geklont werden kann. Danach kann einfach der Link, welcher zuvor bei GitHub kopiert wurde, eingefügt werden. Durch die Bestätigung mit`Clone` wird der Vorgang gestartet und eine allenfalls nötige Authentifizierung des Benutzers wird ausgeführt. Dabei einfach den Anweisungen des Tools folgen.
{{< img alt="Importieren von VCS" src="setup/intellij-getfromvcs.JPG" height="500px" >}}
{{< img alt="Importieren von VCS Link einfügen" src="setup/intellj-insert-githublink.JPG" height="500px" >}}

Sobald das Projekt vollständig auf den lokalen Computer geklont wurde, öffnet sich automatisch das Projekt in der Entwicklungsumgebung. Dabei poppt unten rechts ein kleiner Dialog auf. Dieser muss mit `Import` bestätigt werden damit das Repository richtig initialisiert werden
kann. 

Maven ist ein Softwareprojektmanagement Tool. Näheres dazu kann direkt beim [Apache Maven Project](https://maven.apache.org/) nachgelesen werden. Grundsätzlich muss daran jedoch nichts verändert werden. Das CrowPi Projekt bietet bereits ein vollständiges Setup und kann einfach benutzt werden.
{{< img alt="Import Maven Project" src="setup/intellij-import-maven.JPG" height="500px" >}}

Das Importieren des Maven Projekts löst jeweils eine Sicherheitswarnung bei IntelliJ aus. Diese kann einfach mit `Trust project ...` bestätigt werden, solange der Code aus dem offiziellen FHNW CrowPi Repository stammt.
{{< img alt="Trust Project bestätigung" src="setup/intellij-trust-project.JPG" height="500px" >}}

Nun noch der letzte Import Schritt. Damit später besser Fehler gesucht werden können und auch die Funktionen der Komponenten ganz tief analysiert werden können, lohnt es sich bei Maven `Documentations and Sources` herunterzuladen. Das geht leicht mit einigen Klicks. Ganz rechts wird das Maven Projekt Menu geöffnet. Anschliessend durch einen Klick auf `Download Sources and/or Documentation`. Im Kontextmenü dann anschliessend `Download Sources and Documentation` wählen. So sind alle verwendeten Libraries lokal verfügbar und einsehbar. 

Nun fehlt nur noch die Startkonfiguration des Projekts welche in der nächsten Sektion beschrieben ist.
{{< img alt="IDownload Dependencies and Sources" src="setup/intellij-download-deps-maven.JPG" height="500px" >}}

---

### 3. Einstellung der Run Konfiguration
Das Projekt CrowPi der FHNW benutzt vier Run Konfigurationen. In diesen ist definiert welche Teile des Codes auf welche Art und Weise ausgeführt werden. Verwendet werden die Konfigurationen: 
- `Run on CrowPi`
- `Run on CrowPi(Demo Mode)`
- `Debug on CrowPi`
- `Attach to CrowPi Debugger`

Dabei kopiert `Run on CrowPi` den aktuellen Code auf den Raspberry Pi. Dies funktioniert über eine Kombination aus `SSH/SCP`. Anschliessend wird der kopierte Code auf dem Raspberry Pi gestartet. 

`Debug on CrowPi` macht im Prinzip nichts anderes. Jedoch werden andere Optionen bei der Verbindung ausgewählt und vor der eigentlichen Ausführung der Applikation wird auf eine Verbindung von einem Debugger gewartet.

`Attach to CrowPi Debugger` stellt genau diesen Debugger zur Verfügung. Dieser verbindet sich mit dem Raspberry Pi und die Fehlersuche kann beginnen. Näheres findet man hier: [Starten und Debuggen auf dem CrowPi]({{< ref "basics/debugging" >}})

Zusätzlich existiert auch noch eine Konfiguration `Run on CrowPi(Demo Mode)`, welche genau gleich wie `CrowPi Run` funktioniert und zusätzlich den Parameter `--demo` übergibt. Dies bewirkt, dass der Launcher sich nach Ausführung einer Applikation nicht mehr selber beendet, sondern mehrere Beispiele nacheinander ausgeführt werden können. Wie der Name schon sagt, ist dies besonders für Demozwecke praktisch.

---

### 4. Erster Testlauf
Schon ist es geschafft. Alles ist eingerichtet, um ein erstes Mal das CrowPi Projekt direkt aus der Entwicklungsumgebung zu starten. 

Dazu die Run Konfiguration `CrowPi Run` auswählen. Danach durch Drücken von dem grünen Play Button die Applikation starten.
{{< img alt="Start der Applikation" src="setup/intellij-start-firstapplication.JPG" height="500px" >}}

Es öffnet sich das `Run Fenster` von IntelliJ. Es dauert einen Moment und einiges an Text wird auf der Kommandozeile ausgegeben. Nach einigen Sekunden stoppt die Ausgabe und es sieht wie folgt aus:
{{< img alt="Run Output von IntelliJ" src="setup/intellij-run-example.JPG" height="500px" >}}

Hier kann nun eine Zahl entsprechend der Anweisung eingetippt werden und mit `Enter` bestätigt werden. Die entsprechende Beispielapplikation wird dann ausgeführt. Sollten dabei jetzt noch Fehlermeldungen in der Kommandozeile auftreten lohnt es sich nochmals die Netzwerkverbindung des Computers und des Raspberry Pi zu überprüfen. Auch in der [Troubleshooting Sektion]({{< ref "basics/troubleshooting" >}}) dieses Tutorials sind noch einige Tipps und Tricks zur Fehlerbehebung zu finden.
