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
[Download CrowPi Image]({{% siteparam "repoURL" %}}/releases/latest)

Bei GitHub kann das Image dann hier heruntergeladen werden:
{{< img alt="GitHub Download CrowPi Image" src="setup/download-crowpi-image.JPG" >}}

Nach dem Download das .zip Archiv entpacken. Schon ist alles bereit für den nächsten Schritt.

---

### 3. Schreiben des Images auf eine SD Karte

{{% notice warning %}} Beim Schreiben des Images auf die SD-Karte werden sämtliche Daten welche sich noch darauf
befinden überschrieben!
{{% /notice %}}

Als erstes muss die SD-Karte auf dem Computer als Laufwerk verfügbar sein. Dazu gibt es verschiedene Möglichkeiten. Es
eignen sich Kartenlesegeräte genauso wie auch USB Adapter. Nun wird das Tool Raspberry Pi Imager gestartet. Als erstes
muss das Betriebsystem gewählt werden. Dazu den Knopf `OS WÄHLEN` drücken.
{{< img alt="Bild des Hauptmenüs vom Raspberry Pi Imager" src="setup/mainmenu-raspberrypi-imager.JPG" >}}
Nun findet sich ganz unten in der Auswahl `Eigenes Image`. Im Auswahldialog das vorher heruntergeladene und entpackte
`crowpi.img` auswählen.
{{< img alt="Betriebssystemauswahl vom Raspberry Pi Imager" src="setup/selectos-raspberrypi-imager.JPG" >}}

Als zweiter Schritt wird nun die SD-Karte ausgewählt. Dazu im Menu den Knopf `SD-KARTE WÄHLEN` drücken. Es zeigt einem
automatisch nur verfügbare Wechselmedien wie USB-Sticks oder SD-Karten. **Dabei ist es jetzt sehr wichtig den korrekten
Eintrag auszuwählen um ungewollten Datenverlust zu vermeiden**. Auf Windows wird unter dem Datenträger jeweils noch
angezeigt, welcher Laufwerksbuchstabe effektiv betroffen ist, so dass dieser einfach im Explorer überprüft werden kann.
Im Zweifelsfall aber einfach Datenträger mit wichtigen Medien vorher ausstecken, so dass sicher nichts passieren kann.

{{< img alt="Auswahl SD-Karte" src="setup/selectsdcard-raspberrypi-imager.JPG" >}}

Schon ist alles bereit um das Image auf die SD-Karte zu schreiben. Der Vorgang kann durch Betätigen vom `SCHREIBEN`
Knopf ausgelöst werden. Es folgt nochmals ein Bestätigungsdialog bevor dann endgültig sämtlicher Inhalt der SD-Karte
überschrieben wird. Das Schreiben des Image auf die SD-Karte kann einige Minuten dauern. Das ist völlig normal. Sobald
der Vorgang abgeschlossen ist, kommt die entsprechende Fertigmeldung. Die SD-Karte kann nun aus dem Computer entfernt
werden.

---

### 4. Einsetzten der SD Karte in den Raspberry Pi

Zum Einsetzen der vorbereiteten SD-Karte in den CrowPi müssen allenfalls die 4 Halteschrauben gelöst werden. Diese
finden sich hier:
{{< img alt="Raspberry im CrowPi mit markierten Schrauben" src="setup/crowpi-raspberrypi-screws.JPG" height="600px" >}}
Nach dem Lösen der Schrauben kann der Raspberry Pi angehoben werden. Allenfalls müssen noch einige Kabel ausgesteckt
werden. Auf der Unterseite findet sich nun der SD-Karten Einschub. Hier kann die SD-Karte mit den Kontaktflächen gegen
den Raspberry Pi eingesetzt werden. Auf dem Bild unten ist nochmals der Einschub markiert. Sobald die SD-Karte
eingesetzt ist, kann der Raspberry Pi wieder korrekt in den CrowPi eingebaut und die allenfalls gelösten Kabel wieder
eingesteckt werden. Sobald alles wieder an seinem Platz ist, kann der CrowPi mit dem Strom verbunden werden.
{{< img alt="Raspberry Pi SD-Karten Einschub" src="setup/crowpi-raspberrypi-sdslot.JPG" height="600px" >}}

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

Um mit einem WLAN Netzwerk zu Verbinden, auf dem Desktop des CrowPi oben rechts die beiden Pfeile drücken und das
gewünschte Um sich per WLANzu verbinden, muss auf dem Desktop des CrowPi auf die beiden Pfeile oben rechts gedrückt
werden, worauf sich anschliessend das gewünschte Netzwerk aussuchen lässt. Anschliessend im Dialog das entsprechende
WLAN Passwort bei gesicherten Verbindungen eintippen. Einige Sekunden nach der Verbindung wird sich das Hintergrundbild
des CrowPi automatisch aktualisieren und die zugewiesene IP-Adresse anzeigen. Auf den Bildern ist zusätzlich auch eine
Ethernet Adresse sichtbar. Das heisst es war im Moment der Aufnahme zusätzlich noch ein Ethernetkabel verbunden mit dem
Raspberry Pi. Nach dem wir nun eine Netzwerkverbindung zum CrowPi haben sind wir bereit für das Einrichten der
Entwicklungsumgebung als nächsten Schritt.

{{< img alt="CrowPi WLAN wählen" src="setup/crowpi-selectwlan.JPG?&width=865px&height=500px" >}}
{{< img alt="CrowPi WLAN Passwort eingeben" src="setup/crowpi-wlanpassword.JPG?height=500px&width=865px" >}}
{{< img alt="CrowPi IP Adresse erhalten" src="setup/crowpi-background-ipaddresses.JPG?height=500px&width=865px" >}}
