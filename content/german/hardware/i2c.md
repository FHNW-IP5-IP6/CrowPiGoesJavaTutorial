---
title: I²C
---

## Das Wichtigste in Kürze
I²C (gesprochen Englisch als I-Squared-C) ist ein ursprünglich von Philips erfundener Bus. Er ist als klassischer Master-Slave-Bus konzipiert. Wobei 
eine Datenübertragung immer durch einen Master initiiert wird. Auch ein Aufbau in einem Multi-Master System ist möglich. 
Angeschlossen wird I²C über zwei Signalleitungen (Datenleitung und Taktleitung).
Die Übertragungsrate des Buses kann je nach Taktrate zwischen 0.1 Mbit/s bis zu 3.4 Mbit/s liegen. Sofern nur eine unidirektionale Verbindung nötig ist,
wäre sogar 5.0 Mbit/s möglich. Zu beachten ist dabei: Je höher die Taktrate, desto störanfälliger wird das Gesamtsystem. Die niedrige Betriebsspannung
von nur 3.3V trägt ebenfalls nicht zur Störresistenz bei. 

## Verwendungen
I²C wird vor allem zur Kommunikation zwischen Mikrocontrollern eingesetzt. Der Vorteil dass dabei über nur 2 Leitungen eine ganze Reihe von Mikrocontrollern
gesteuert werden kann, ist natürlich sehr interessant für das Platinenlayout. Die Vorteile von I²C liegen vor allem in der Einfachheit. Es gibt sehr wohl 
neuere Bussysteme mit besseren Übertragungsraten. Kaum ein Bussystem ist jedoch so einfach zu benutzen wie I²C. Sogar ein "Hot-Plugging" also ein ein- und
ausstecken der Geräte während dem Betrieb ist mit I²C möglich.

## Adressierung
I²C nutzt einen Adressraum von 7 Bit. Das erlaubt bis zu 112 Knoten auf einem Bus. Die restlichen 16 Adressen sind für spezielle Anwendungen reserviert.
Normalerweise wird die Adresse eines Geräts direkt vom Hersteller definiert. Sie kann also in den entsprechenden Datenblättern gefunden werden. Zusätzlich 
gibt es aufgrund der Adressknappheit eine Variante mit 10 Bit Adressraum. So sind bis zu 1136 Knoten möglich und das Protokoll ist kompatibel mit dem kleineren
7 Bit Adressraum. 

## Übertragungsraten

| Modus | Max. Übertragungsrate | Richtung |
| --- | --- | --- |
| Standard Mode | 0.1 Mbit/s | bidirektional |
| Fast Mode | 0.4 Mbit/s | bidirektional |
| Fast Mode Plus | 1.0 Mbit/s | bidirektional |
| High Speed Mode | 3.4 Mbit/s | bidirektional |
| Ultra Fast-mode | 5.0 Mbit/s | unidirektional |

## Weitere Informationen
- [Wikipedia I²C](https://de.wikipedia.org/wiki/I%C2%B2C)
- [I²C Bus](https://i2c-bus.org)
