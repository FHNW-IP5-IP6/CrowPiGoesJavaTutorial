---
title: IntelliJ IDEA Setup
weight: 30  
disableToc: true
---

## Installieren von IntelliJ IDEA

In diesem Tutorial wird die Entwicklung mit Hilfe der Entwicklungsumgebung IntelliJ IDEA umgesetzt. Entsprechend sind auch die Artikel,
Anweisungen und Bilder jeweils damit erstellt. IntelliJ IDEA kann bei [Jetbrains](https://jetbrains.com) in verschiedenen Versionen bezogen
werden. Für die Entwicklung mit dem CrowPi verfügt die Community Version bereits über genügend Funktionalität. Für Studenten der
Fachhochschule Nordwestschweiz ist jedoch auch die Ultimate Version kostenfrei erhältlich. Die Entwicklungsumgebung ist sowohl für Windows,
macOS und auch Linux verfügbar. Der Download findet sich hier: [Download IntelliJ IDEA](https://www.jetbrains.com/idea/download/). Das 
anschliessende Einrichten von IntelliJ IDEA verhält sich ebenfalls auf allen Plattformen identisch.

---

### 1. Klonen des Repositories

Sobald wir nun die Entwicklungsumgebung installiert haben geht es an das Klonen des Einstiegsprojekts für den CrowPi. Dabei findet sich der
Sourcecode auf [GitHub]({{% siteparam "repoURL" %}}). Um nun das Repository zu klonen muss auf GitHub
der entsprechende Link kopiert werden und dann in IntelliJ IDEA importiert werden. Dazu nun schrittweise in Bildern eine etwas genauere
Anleitung.

Besuche [CrowPi]({{% siteparam "repoURL" %}}) auf GitHub und kopiere den Link zum Klonen wie auf dem Bild beschrieben.
kann.
![GitHub Clone Project](/fhnw-crowpi/images/setup/github-clone-project.JPG?height=500px)

---

### 2. Importieren des Projekts

Im Startfenster von IntelliJ ist die Option `Get from VCS` verfügbar. Diese muss angewählt werden damit direkt aus GitHub der Code geklont
werden kann. Danach kann einfach der Link welcher zuvor bei GitHub kopiert wurde eingefügt werden. Durch die Bestätigung mit
`Clone` wird der Vorgang gestartet und eine allenfalls nötige Authentifizierung des Benutzers wird ausgeführt. Dabei einfach den Anweisungen des
Tools folgen.
![Importieren von VCS](/fhnw-crowpi/images/setup/intellij-getfromvcs.JPG?height=500px)
![Importieren von VCS Link einfügen](/fhnw-crowpi/images/setup/intellj-insert-githublink.JPG?height=500px)

Sobald das Projekt vollständig auf den lokalen Computer geklont wurde, öffnet sich automatisch das Projekt in der Entwicklungsumgebung.
Dabei poppt unten rechts ein kleiner Dialog auf. Dieser muss mit `Import` bestätigt werden damit das Repository richtig initialisiert werden
kann. Maven ist ein Softwareprojektmanagement Tool. Näheres dazu kann direkt beim [Apache Maven Project](https://maven.apache.org/)
nachgelesen werden. Grundsätzlich muss daran jedoch nichts verändert werden. Das CrowPi Projekt bietet bereits ein vollständiges Setup und
kann einfach benutzt werden.
![Import Maven Project](/fhnw-crowpi/images/setup/intellij-import-maven.JPG?height=500px)

Das Importieren des Maven Projekts löst jeweils eine Sicherheitswarnung bei IntelliJ aus. Diese kann einfach mit `Trust project ...` 
bestätigt werden, solange der Code aus dem offiziellen FHNW CrowPi Repository stammt.
![Trust Project bestätigung](/fhnw-crowpi/images/setup/intellij-trust-project.JPG?height=500px)

Nun noch der letzte Import Schritt. Damit später besser Fehler gesucht werden können und auch die Funktionen der Komponenten ganz tief 
analysiert werden können, lohnt es sich bei Maven `Documentations and Sources` herunterzuladen. Das geht leicht mit einigen Klicks. Ganz 
rechts wird das Maven Projekt Menu geöffnet. Anschliessend durch einen Klick auf `Download Sources and/or Documentation`. Im Kontextmenü 
dann anschliessend `Download Sources and Documentation` wählen. So sind alle verwendeten Libraries lokal verfügbar und einsehbar. Nun 
fehlt nur noch die Startkonfiguration des Projekts welche in der nächsten Sektion beschrieben ist.
![IDownload Dependencies and Sources](/fhnw-crowpi/images/setup/intellij-download-deps-maven.JPG?height=500px)

---

### 3. Einstellung der Run Konfiguration
Das Projekt CrowPi der FHNW benutzt 3 Run Konfigurationen. In diesen ist definiert welche Teile des Codes auf welche Art und Weise 
ausgeführt werden. Es besteht jedoch kein Grund zur Sorge, denn der grösste Teil davon ist bereits vordefiniert und es ist nur noch die IP-Adresse des Raspberry Pi einzutragen. 
Verwendet werden die Konfigurationen: 
- crowpi-examples [install]
- crowpi-examples [debug]
- Remote Debug 

Dabei kopiert `crowpi-examples [install]` den aktuellen Code auf den Raspberry Pi. Dies funktioniert über eine Kombination aus `SSH/SCP`.
Anschliessend wird der kopierte Code auf dem Raspberry Pi gestartet. `crowpi-examples [debug]` macht im Prinzip nichts anderes. Jedoch 
werden andere Optionen bei der Verbindung ausgewählt und vor der eigentlichen Ausführung der Applikation wird auf eine Verbindung von einem Debugger gewartet. `Remote Debug` 
stellt genau diesen Debugger zur Verfügung. Dieser verbindet sich mit dem Raspberry Pi und die Fehlersuche kann beginnen. Näheres wie 
dies findet man hier: [Starten und Debuggen auf dem CrowPi](TODO)

Damit dies alles reibungslos funktioniert muss nun aber zunächst wie erwähnt die IP-Adresse des Raspberry Pi eingestellt werden. Dazu 
hier klicken und `Edit Configurations` wählen. Die IP-Adresse des Raspberry Pi wird, wie zuvor erklärt, auf dem Hintergrundbild des 
CrowPi angezeigt.
![Konfigurationsmenu auswählen](/fhnw-crowpi/images/setup/intellij-select-configuration.JPG?height=500px)

Nun öffnet sich der Dialog zum Einstellen der Konfigurationen. EIn kleiner Hinweis: Überall wo die IP-Adresse des Raspberry Pi hingehört 
steht bereits als Platzhalter `Add CrowPi IP here`. 
![Die drei Konfigurationen](/fhnw-crowpi/images/setup/intellj-three-configs.JPG?height=500px)

Als erstes wird `crowpi-examples [debug]` konfiuriert. Dazu wie auf dem Bild den Tab `Runner` öffnen und dann auf `Add CrowPi IP here` 
doppelklicken. Danach öffnet sich das Dialogfenster für das Eintippen der IP-Adresse. Kurz mit `OK` bestätigen und mit `Apply` speichern. 
![Einstellungen Debug](/fhnw-crowpi/images/setup/intellij-setup-debugconfig.JPG?height=500px)

Als zweites folgt die `crowpi-examples [install]` Konfiguration. Diese funktioniert genau gleich wie eben die `crowpi-examples [debug]` 
Konfiguration. Genau die gleiche Einstellung ist nötig. Wiederum mit `Apply` speichern.
![Einstellungen Install](/fhnw-crowpi/images/setup/intellij-setup-runconfig.JPG?height=500px)

Nun folgt auch schon die letzte Konfiguration `Remote Debug`. Hier stellt sich das Menü etwas anders dar. Jedoch ist es noch einfacher 
zu bedienen als die vorherigen. Kurz im Feld `Host` die IP-Adresse des Raspberry Pi eintragen. Mit `OK` beenden wird die Einstellung 
beendet.
![Einstellungen Remote Debug](/fhnw-crowpi/images/setup/intellij-remotedebug-config.JPG?height=500px)

---

### 4. Erster Testlauf
Schon ist es geschafft. Alles ist eingerichtet, um ein erstes Mal das CrowPi Projekt direkt aus der Entwicklungsumgebung zu starten. 
Dazu die Run Konfiguration `crowpi-examples [install]` auswählen. Danach durch Drücken von dem grünen Play Button die Applikation starten.
![Start der Applikation](/fhnw-crowpi/images/setup/intellij-start-firstapplication.JPG?height=500px)

Es öffnet sich sofort das `Run Fenster` von IntelliJ. Es dauert einen Moment und einiges an Text wird auf der Kommandozeile ausgegeben. 
Nach einigen Sekunden stoppt die Ausgabe und es sieht wie folgt aus:
![Run Output von IntelliJ](/fhnw-crowpi/images/setup/intellij-run-example.JPG?height=500px)

Hier kann nun eine Zahl entsprechend der Anweisung eingetippt werden und mit `Enter` bestätigt werden. Die entsprechende 
Beispielapplikation wird dann ausgeführt. Sollten dabei jetzt noch Fehlermeldungen in der Kommandozeile auftreten lohnt es sich nochmals 
die Netzwerkverbindung des Computers und des Raspberry Pi zu überprüfen. Auch in der Troubleshooting Sektion dieses Tutorials sind noch 
einige Tipps und Tricks zur Fehlerbehebung zu finden.
