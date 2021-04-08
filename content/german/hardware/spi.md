---
title: Serial Peripheral Interface (SPI)
tags: ["spi"]
---

## Das Wichtigste in Kürze

Das Serial Peripheral Interface, abgekürzt SPI, ist ein Bus-System welches eine Kommunikation zwischen einem Hauptgerät (englisch als
«Master» bezeichnet) und ein oder mehreren Nebengeräten (englisch als «Slave» bezeichnet) ermöglicht. Eine direkte Kommunikation zwischen
allen Teilnehmern ist hierbei nicht möglich, viel mehr kann der Master jederzeit auswählen mit welchem Slave er Daten austauschen möchte.

Um nur einen Slave anzusprechen werden insgesamt 3 Signalleitungen benötigt, wovon zwei für die bidirektionale Datenübertragung und eine als
Taktgeber für die serielle Übertragung genutzt wird. Wenn weitere Slaves angesprochen werden sollen, so werden zusätzliche Signalleitungen
je nach gewünschter Topologie benötigt.

## Verwendungen

SPI wird neben der Kommunikation zwischen Mikrocontrollern auch für das Ansprechen zahlreicher Sensoren und Aktoren verwendet. Ähnlich wie
bei I²C können hier über bereits 3 Leitungen eine grosse Anzahl von Steuerkommandos und Daten in beide Richtungen mit einer relativ hohen
Taktrate übermittelt werden. Ein besonderer Vorteil ist hier auch die Unterstützung von «Full Duplex», also dem gleichzeitigen 
Übertragen von Daten in beide Richtungen.

Die technische Implementation ist hierbei sehr einfach und wird beispielsweise auch zur Kommunikation mit SD-Karten verwendet. Auch der
Nintendo Game Boy nutzte dieses Protokoll bereits zur Verbindung von mehreren Spielekonsolen über das Game Boy Link Cable.

## Adressierung

Wie bereits im ersten Abschnitt erwähnt lassen sich auch bei SPI mehrere Slaves anschliessen. Die verfügbare Anzahl hängt hierbei von der
jeweils verwendeten Hardware ab. Auf dem Raspberry Pi lässt sich standardmässig `SPI0` mit zwei verschiedenen Slaves nutzen, was von den
sogenannten `Chip Select` Pins gesteuert wird.

## Weitere Informationen

- [Wikipedia SPI (Deutsch)](https://de.wikipedia.org/wiki/Serial_Peripheral_Interface)
- [Wikipedia SPI (Englisch mit weiteren Infos)](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface)
- [SPI Pinout für Raspberry Pi](https://pinout.xyz/pinout/spi#)
