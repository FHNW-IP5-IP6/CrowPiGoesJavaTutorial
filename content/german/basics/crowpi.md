---

title: Die CrowPi Plattform
weight: 10
----------

## Das Wichtigste in Kürze

Beim CrowPi handelt es sich um einen vielfältigen Baukasten der Firma
[Elecrow](https://www.elecrow.com/crowpi-compact-raspberry-pi-educational-kit.html), welcher in Verbindung mit einem Raspberry Pi den
Benutzer zahlreiche Komponenten zu Lern- und Weiterbildungszwecken nutzen lässt. Das Gerät entstand im Rahmen einer Kickstarter-Kampagne und
konnte schnell eine grosse Anzahl von Unterstützern finden.

Im Gegensatz zu anderen Elektronikbausätzen sind beim CrowPi alle Komponenten direkt über die Platine mit den jeweiligen Komponenten
verbunden, so dass kein manuelles und aufwändiges Verkabeln der einzelnen Bauteile mehr erforderlich ist. Stattdessen kann einfach ein
Raspberry Pi eingesetzt werden, welcher sofort über seine GPIO Pins (General-Purpose Input/Output) die jeweiligen Aktoren und Sensoren
ansprechen kann.

Dank dem integrierten Display vom CrowPi sowie der mitgelieferten Tastatur und Maus ist es sogar möglich, direkt auf dem entsprechenden
Gerät zu entwickeln oder andere grafische Applikationen darauf auszuführen. Da der Bildschirm für längere Arbeiten aber doch etwas klein
ist, wird im Tutorial eine Variante gewählt wo auf einem separaten PC oder Laptop entwickelt werden kann.

## Besonderheiten

### DIP Switches

Aufgrund der hohen Anzahl an verschiedenen Komponenten stösst der CrowPi an eine Limitation des Raspberry Pi, konkret die Anzahl der
verfügbaren GPIO Pins. Es ist nur möglich eine begrenzte Anzahl von Bauteilen mit dem Raspberry Pi zu verbinden bevor alle Ein- und Ausgänge
belegt sind. Um dieses Problem zu umgehen, verwendet der CrowPi sogenannte DIP Switches:

{{< img alt="CrowPi DIP Switches" src="basics/crowpi-dip-switches.jpg" height="500px" >}}

Es handelt sich hierbei um die beiden rot umrandeten Schaltergruppen, welche jeweils 8 kleine Schalter anbieten. In der Standardposition
sind diese alle ausgeschaltet und somit in der unteren Position, womit sich die meisten Komponenten vom CrowPi direkt einsetzen lassen. Bei
manchen Komponenten müssen aber noch einzelne Schalter eingeschaltet (= obere Position) werden, um eine andere verbundene Komponente vom
Raspberry Pi abzuhängen und stattdessen die neue Komponente anzusprechen.

Man kann sich diese Schalter beim CrowPi somit als eine Art Weiche vorstellen, welche entweder die eine oder die andere Komponente mit dem
Raspberry Pi verbindet. In diesem Tutorial wird jeweils auf die benötigte Schalterposition hingewiesen. Es lassen sich somit zwar eine
Vielzahl von Komponenten verbinden, aber einige Konfigurationen sind aufgrund dieser Limitation nicht möglich.

## Komponenten

Der CrowPi verfügt über eine grosse Anzahl von Sensoren und Aktoren, welche über die jeweiligen Pins auf dem Raspberry Pi mittels Pi4J
angesprochen werden können. Es kommen hierbei unterschiedliche Protokolle und Schnittstellen zum Einsatz, welche jedoch von den
entsprechenden Komponenten-Klassen dieses Tutorials abstrahiert und vereinfacht werden. Nachfolgend ist eine Übersicht aller vorhandenen
Komponenten aufgeführt:

| Komponente                                                                           | Einsatzzweck                                       | Schnittstelle | Position von DIP Switches            |
|:-------------------------------------------------------------------------------------|:---------------------------------------------------|:--------------|:-------------------------------------|
| [7-Segment Anzeige]({{< ref "components/seven-segment" >}})                          | Anzeigen von bis zu 4 Ziffern                      | I²C           | {{< dip-switches >}}                 |
| [Button]({{< ref "components/button" >}})                                            | Abfragen von vier unabhängigen Knöpfen             | GPIO          | {{< dip-switches 5 6 7 8 >}}         |
| [Button Matrix]({{< ref "components/button-matrix" >}})                              | Abfragen von Matrix aus 4 × 4 Knöpfen              | GPIO          | {{< dip-switches 1 2 3 4 5 6 7 8 >}} |
| [Buzzer]({{< ref "components/buzzer" >}})                                            | Abspielen von verschiedenen Tönen                  | PWM           | {{< dip-switches >}}                 |
| [Infrarot Empfänger]({{< ref "components/ir-receiver" >}})                           | Empfangen von Infrarot-Signalen                    | GPIO          | {{< dip-switches >}}                 |
| [LCD Display]({{< ref "components/lcd-display" >}})                                  | Anzeige von Text und Zahlen                        | I²C, GPIO     | {{< dip-switches >}}                 |
| [LED Matrix]({{< ref "components/led-matrix" >}})                                    | Darstellen von beliebigen Symbolen                 | SPI           | {{< dip-switches >}}                 |
| [Lichtsensor]({{< ref "components/light-sensor" >}})                                 | Erkennen von aktueller Lichtstärke                 | I²C           | {{< dip-switches >}}                 |
| [PIR Motion Sensor]({{< ref "components/pir-motion-sensor" >}})                      | Erkennen von Bewegung mit passivem Infrarot        | GPIO          | {{< dip-switches >}}                 |
| [Relais]({{< ref "components/relay" >}})                                             | Schaltung eines elektrischen Kontakts              | GPIO          | {{< dip-switches >}}                 |
| [RFID]({{< ref "components/rfid" >}})                                                | Kontaktloses Lesen und Schreiben von Karten        | SPI           | {{< dip-switches >}}                 |
| [Schrittmotor]({{< ref "components/step-motor" >}})                                  | Bewegen eines Schrittmotors                        | GPIO          | {{< dip-switches 11 12 13 14>}}      |
| [Servomotor]({{< ref "components/servo-motor" >}})                                   | Bewegen eines Servomotors                          | PWM           | {{< dip-switches 15 16 >}}           |
| [Sound Sensor]({{< ref "components/sound-sensor" >}})                                | Erkennen von Lärm oder Stille                      | GPIO          | {{< dip-switches >}}                 |
| [Temperatur- und Luftfeuchtigkeitssensor]({{< ref "components/humi-temp-sensor" >}}) | Messen von Temperatur und Luftfeuchtigkeit         | GPIO          | {{< dip-switches >}}                 |
| [Tilt Sensor]({{< ref "components/tilt-sensor" >}})                                  | Erkennt aktuelle Neigung (links/rechts) von CrowPi | GPIO          | {{< dip-switches 10 >}}              |
| [Touch Sensor]({{< ref "components/touch-sensor" >}})                                | Erkennen von Berührungen                           | GPIO          | {{< dip-switches >}}                 |
| [Ultraschall Distanz Sensor]({{< ref "components/ultrasonic-sensor" >}})             | Messung von Distanzen mit Ultraschall              | GPIO          | {{< dip-switches >}}                 |
| [Vibrationsmotor]({{< ref "components/vibration-motor" >}})                          | Erzeugen eines Vibrationsalarms                    | GPIO          | {{< dip-switches 9 >}}               |

## Weitere Ressourcen

- [Offizielle Anleitung von Hersteller (Englisch)](https://www.elecrow.com/download/product/SES14002K/CrowPi_User_Manual.pdf)
- [Ursprüngliche Kickstarter-Kampagne für CrowPi (Englisch)](https://www.kickstarter.com/projects/elecrow/crowpi-lead-you-go-from-zero-to-hero-with-raspberr)

