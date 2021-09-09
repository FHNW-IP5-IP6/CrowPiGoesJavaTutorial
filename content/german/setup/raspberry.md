---
title: Raspberry PI Setup 
weight: 20 
disableToc: true
---

## Installieren des Betriebssystems

### 1. Installieren des Raspberry Pi Imager

Der offizielle Raspberry Pi Imager kann direkt auf
[der offiziellen raspberry.org Website](https://www.raspberrypi.org/software/) in der aktuellsten Version heruntergeladen werden. Das
einfache Tool funktioniert auf allen gängigen Betriebssystemen und kann mit wenigen Tastendrücken sehr simpel installiert werden. Genauere
Anleitungen zur Installation sind ebenfalls auf der genannten Homepage verfügbar.

---

### 2. Herunterladen des FHNW CrowPi Image

Das Image für den CrowPi, welches das Betriebssystem für den Raspberry Pi beinhaltet, kann direkt aus dem [Github
Repository des Pi4J Projekts](https://github.com/Pi4J/pi4j-os) bezogen werden, das auch noch weitere spezialisierte Images bereitstellt.

Das neueste Release des Betriebssystems für den CrowPi findet ihr unter diesem Link: [Download CrowPi Image](https://pi4j-download.com/latest.php?flavor=crowpi)

Nach dem Download das .zip Archiv entpacken. Schon ist alles bereit für den nächsten Schritt.

---

### 3. Schreiben des Images auf eine SD Karte

{{% notice warning %}} Bei dem Schreiben des Images auf die SD-Karte werden sämtliche Daten, welche sich noch darauf
befinden, überschrieben!
{{% /notice %}}

Als Erstes muss die SD-Karte auf dem Computer als Laufwerk verfügbar sein. Dazu gibt es verschiedene Möglichkeiten. Es
eignen sich Kartenlesegeräte genauso wie auch USB Adapter. Nun wird das Tool Raspberry Pi Imager gestartet. Als Erstes
muss das Betriebssystem gewählt werden. Dazu den Knopf `OS WÄHLEN` drücken.
{{< img alt="Bild des Hauptmenüs vom Raspberry Pi Imager" src="setup/mainmenu-raspberrypi-imager.JPG" >}}
Nun findet sich ganz unten in der Auswahl `Eigenes Image`. Im Auswahldialog das vorher heruntergeladene und entpackte
`crowpi.img` auswählen.
{{< img alt="Betriebssystemauswahl vom Raspberry Pi Imager" src="setup/selectos-raspberrypi-imager.JPG" >}}

Als zweiter Schritt wird nun die SD-Karte ausgewählt. Dazu im Menu den Knopf `SD-KARTE WÄHLEN` drücken. Es zeigt einem
automatisch nur verfügbare Wechselmedien wie USB-Sticks oder SD-Karten. **Dabei ist es jetzt sehr wichtig den korrekten
Eintrag auszuwählen, um ungewollten Datenverlust zu vermeiden**. Auf Windows wird unter dem Datenträger jeweils noch
angezeigt, welcher Laufwerksbuchstabe effektiv betroffen ist, sodass dieser einfach im Explorer überprüft werden kann.
Im Zweifelsfall aber einfach Datenträger mit wichtigen Medien vorher ausstecken, sodass sicher nichts passieren kann.

{{< img alt="Auswahl SD-Karte" src="setup/selectsdcard-raspberrypi-imager.JPG" >}}

Schon ist alles bereit um das Image auf die SD-Karte zu schreiben. Der Vorgang kann durch Betätigen vom `SCHREIBEN`
Knopf ausgelöst werden. Es folgt nochmals ein Bestätigungsdialog, bevor dann endgültig sämtlicher Inhalt der SD-Karte
überschrieben wird. Das Schreiben des Image auf die SD-Karte kann einige Minuten dauern. Das ist völlig normal. Sobald
der Vorgang abgeschlossen ist, kommt die entsprechende Fertigmeldung. Die SD-Karte kann nun aus dem Computer entfernt
werden.

---

### 4. Einsetzen der SD Karte in den Raspberry Pi

Zum Einsetzen der vorbereiteten SD-Karte in den CrowPi müssen allenfalls die 4 Halteschrauben gelöst werden. Diese
finden sich hier:
{{< img alt="Raspberry im CrowPi mit markierten Schrauben" src="setup/crowpi-raspberrypi-screws.JPG" height="600px" >}}
Nach dem Lösen der Schrauben kann der Raspberry Pi angehoben werden. Allenfalls müssen noch einige Kabel ausgesteckt
werden. Auf der Unterseite findet sich nun der SD-Karten Einschub. Hier kann die SD-Karte mit den Kontaktflächen gegen
den Raspberry Pi eingesetzt werden. Auf dem Bild unten ist nochmals der Einschub markiert. Sobald die SD-Karte
eingesetzt ist, kann der Raspberry Pi wieder korrekt in den CrowPi eingebaut und die allenfalls gelösten Kabel wieder
eingesteckt werden. Sobald alles wieder an seinem Platz ist, kann der CrowPi mit dem Strom verbunden werden.

{{< img alt="Raspberry Pi SD-Karten Einschub" src="setup/crowpi-raspberrypi-sdslot.JPG" height="600px" >}}

Bevor der CrowPi nun in Betrieb genommen wird, sollte noch einmal überprüft werden, ob alle 3 Kabel mit dem Raspberry Pi verbunden sind. Es
sollte sowohl der HDMI-Adapter auf der linken Seite, das USB-Kabel auf der unteren Seite sowie das GPIO Flachbandkabel auf der rechten Seite
verbunden sein. Diese zwingend erforderlichen Kabel sind in der nächsten Grafik mit roten Kreisen umrahmt. Optional kann noch eine Tastatur
und Maus per USB verbunden werden, was nachfolgend mit einem pinken Kreis markiert wurde:

{{< img alt="Raspberry Pi SD-Karten Einschub" src="setup/crowpi-cables.jpg" height="600px" >}}

Nun kann der CrowPi eingeschaltet werden, worauf dieser nach einer kurzen Initialisierungszeit zur Verfügung steht. Die Zugangsdaten für den
CrowPi sind `pi` als Benutzername und `crowpi` als Passwort, welche **nicht** geändert werden dürfen da ansonsten die mitgelieferten
IntelliJ-Konfigurationen nicht mehr direkt lauffähig sind.

---

### 5. Herstellen der Netzwerkverbindung

Einzig die Netzwerkverbindung des frisch gestarteten Raspberry Pi muss noch manuell getätigt werden. Ansonsten sind alle
Einstellungen bereits optimal im FHNW CrowPi Image enthalten. Nun gibt es für die Netzwerkverbindung einige
Möglichkeiten:

- **Verbindung per WLAN (empfohlen für FHNW)**
- Verbindung per Ethernetkabel (DHCP)
- Verbindung per Ethernetkabel direkt zu Computer

Am einfachsten lassen sich die Einstellungen mittels am Raspberry Pi angeschlossener Maus und Tastatur vollziehen. Die
Einrichtung via WLAN wird explizit empfohlen, da sich diese in fast jeder Umgebung nutzen lässt und mittels
Hotspot-Funktion am eigenen Smartphone auch unterwegs prima funktioniert.

Nachfolgend wird nur die Verbindung per WLAN beschrieben, jedoch kann bei vorhandener Expertise in diesem Gebiet auch
eine Verbindung via Kabel aufgebaut werden.

#### 5.1 Herstellen einer WLAN Verbindung

Der Raspberry Pi und der Laptop müssen in der Regel mit dem gleichen WLAN verbunden sein. 

#####  Verwenden des Pi4J-Spot

Auf dem CrowPi-Image ist eine WLAN-Verbindung voreingestellt.

- ssid: ``Pi4J-Spot``
- password: ``MayTheCodeBeWithYou!``

Sobald ein dementsprechender Hotspot, zum Beispiel auf einem Smartphone, angelegt wird, verbindet sich der CrowPi mit diesem Hotspot und die aktuelle IP-Adresse des CrowPi wird als Hintergrundbild angezeigt. 

Dann noch den Laptop ebenfalls mit ``Pi4J-Spot`` verbinden.

##### Einstellen der WLAN-Verbindung auf dem CrowPi

Um mit einem WLAN Netzwerk zu verbinden, auf dem Desktop des CrowPi oben rechts das Icon mit den beiden Pfeile klicken und das
gewünschte WLAN auswählen.

Anschliessend im Dialog das entsprechende WLAN Passwort bei gesicherten Verbindungen eintippen. Einige Sekunden nach der Verbindung wird sich das Hintergrundbild des CrowPi automatisch aktualisieren und die zugewiesene IP-Adresse anzeigen. Auf den Bildern ist zusätzlich auch eine
Ethernet Adresse sichtbar. Das heisst es war im Moment der Aufnahme zusätzlich noch ein Ethernetkabel verbunden mit dem
Raspberry Pi. 

Nachdem wir nun eine Netzwerkverbindung zum CrowPi haben, sind wir bereit für das Einrichten der Entwicklungsumgebung als nächsten Schritt.

{{< img alt="CrowPi WLAN wählen" src="setup/crowpi-selectwlan.JPG?&width=865px&height=500px" >}}
{{< img alt="CrowPi WLAN Passwort eingeben" src="setup/crowpi-wlanpassword.JPG?height=500px&width=865px" >}}
{{< img alt="CrowPi IP Adresse erhalten" src="setup/crowpi-background-ipaddresses.JPG?height=500px&width=865px" >}}
