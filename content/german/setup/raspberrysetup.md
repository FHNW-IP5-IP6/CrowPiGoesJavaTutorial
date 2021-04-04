---
title: Raspberry PI Setup
weight: 20
disableToc: true
---

## Installieren des Betriebssystems

### 1. Installieren des Raspberry Pi Imager

Der offizielle Raspberry Pi Imager kann direkt bei [raspberry.org](https://www.raspberrypi.org/software/) in der
aktuellsten Version heruntergeladen werden. Das einfache Tool funktioniert auf allen gängigen Betriebssystemen und kann
mit wenigen Tastendrücken sehr simpel installiert werden. Genauere Anleitungen zur Installation sind ebenfalls auf der
genannten Homepage verfügbar.

---

### 2. Herunterladen des FHNW CrowPi Image

Das Image für den CrowPi welches das Betriebssystem für den Raspberry PI beinhaltet kann direkt aus dem Github
Repository des CrowPi Projekts bezogen werden. Das neustste Release des Betriebssystems findet unter diesem Link:
[Download CrowPi Image](https://github.com/ppmathis/fhnw-crowpi/releases/latest)

Bei GitHub kann das Image dann hier heruntergeladen werden:
![GitHub Download CrowPi Image](/fhnw-crowpi/images/setup/download-crowpi-image.JPG)

Nach dem Download das .zip Archiv entpacken. Schon ist alles bereit für den nächsten Schritt.

---

### 3. Schreiben des Images auf eine SD Karte

 <span style="color:red !important">**Warnung: Beim Schreiben des Images auf die SD-Karte werden sämtliche Daten
 welche sich
noch
darauf befinden überschrieben!**</span>

Als erstes muss die SD-Karte auf dem Computer als Laufwerk verfügbar sein. Dazu gibt es verschiedene Möglichkeiten. Es
eignen sich Kartenlesegeräte genauso wie auch USB Adapter.
Nun wird das Tool Raspberry Pi Imager gestartet. Als erstes muss das Betriebsystem gewählt werden. Dazu den Knopf `OS
WÄHLEN` drücken.
![Bild des Hauptmenüs vom Raspberry Pi Imager](/fhnw-crowpi/images/setup/mainmenu-raspberrypi-imager.JPG)
Nun findet sich ganz unten in der Auswahl `Eigenes Image`. Im Auswahldialog das vorher heruntergeladene und entpackte
 `crowpi.img` auswählen.
![Betriebssystemauswahl vom Raspberry Pi Imager](/fhnw-crowpi/images/setup/selectos-raspberrypi-imager.JPG)

Als zweiter Schritt wird nun die SD-Karte ausgewählt. Dazu im Menu den Knopf `SD-KARTE WÄHLEN` drücken. Es zeigt einem
automatisch nur verfügbare Wechselmedien wie USB-Sticks oder SD-Karten. **Dabei ist es jetzt sehr wichtig den
korrekten Eintrag auszuwählen um ungewollten Datenverlust zu vermeiden**. Als kleiner Tipp dazu. Es wird jeweils
angezeigt welche Laufwerksbuchstaben betroffen sind. Die können hervorragend im Dateisystem des Computers überprüft
werden.

![Auswahl SD-Karte](/fhnw-crowpi/images/setup/selectsdcard-raspberrypi-imager.JPG)

Schon ist alles bereit um das Image auf die SD-Karte zu schreiben. Der Vorgang kann durch betätigen der `SCHREIBEN`
Taste ausgelöst werden. Es folgt nochmals ein Bestätigungsdialog bevor dann endgültig sämtlicher Inhalt der SD-Karte
überschrieben wird. Das Schreiben des Image auf die SD-Karte kann einige Minuten dauern. Das ist völlig normal. Sobald
der Vorgang abgeschlossen ist kommt die entsprechende Fertigmeldung. Die SD-Karte kann nun aus dem Computer entfernt
werden.

---

### 4. Einsetzten der SD Karte in den Raspberry Pi

Zum Einsetzen der vorbereiteten SD-Karte in den CrowPi müssen allenfalls die 4 Halteschrauben gelöst werden. Diese
finden sich hier:
![Raspberry im CrowPi mit markierten Schrauben](/fhnw-crowpi/images/setup/crowpi-raspberrypi-screws.JPG?height=600px)
Danach kann der Raspberry Pi leicht abgehoben werden um am oberen Ende auf der Rückseite befindet sich der SD-Karten
einschub. Hier kann die Karte mit den Kontaktflächen gegen den Raspberry Pi eingesetzt werden. Auf dem Bild unten ist
nochmals der Einschub markiert. Sobald die SD-Karte eingesetzt ist kann der Raspberry Pi wieder korrekt in den CrowPi
eingebaut werden und die allenfalls gelösten Kabel wieder eingesteckt werden. Sobald alles weder an seinem Platz ist
kann der CrowPi mit dem Strom verbunden werden.
 ![Raspberry Pi SD-Karten Einschub](/fhnw-crowpi/images/setup/crowpi-raspberrypi-sdslot.JPG?height=600px)

---

### 5. Herstellen der Netzwerkverbindung
Einzig die Netzwerkverbindung des frisch gestarteten Raspberry Pi muss noch manuell getätigt werden. Ansonsten sind alle
 Einstellungen bereits optimal im FHNW CrowPi Image enthalten. Nun gibt es für die Netzwerkverbindung einige
 Möglichkeiten:

 - **Verbindung per WLAN (empfohlen für FHNW)**
 - Verbindung per Ethernetkabel (DHCP)
 - Verbindung per Ethernetkabel direkt zu Computer

Am einfachsten lassen sich die Einstellungen mittels am Raspberry Pi angeschlossener Maus und Tastatur vollziehen.

#### 5.1 Herstellen einer WLAN Verbindung
Um mit einem WLAN Netzwerk zu Verbinden auf Desktop des CrowPi oben recht die beiden Pfeile drücken und das gewünschte
WLAN aussuchen. Anschliessend im Dialog das entsprechende WLAN Passwort bei gesicherten Verbindungen eintippen. Ein Tipp
 es kann auch ein mobiler Hotspot eines Telefons verwendet werden um mit dem CrowPi zu arbeiten. Einige Sekunden nach
 der Verbindung wird sich das Hintergrundbild des CrowPi automatisch aktualisieren und die zugewiesene IP Adresse
 anzeigen.
 ![CrowPi WLAN wählen](/fhnw-crowpi/images/setup/crowpi-selectwlan.JPG?&width=865px&height=500px)
 ![CrowPi WLAN Passwort eingeben](/fhnw-crowpi/images/setup/crowpi-wlanpassword.JPG?height=500px&width=865px)
 ![CrowPi IP Adresse erhalten](/fhnw-crowpi/images/setup/crowpi-background-ipaddresses.JPG?height=500px&width=865px)
