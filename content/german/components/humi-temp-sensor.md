---
title: Temperatur- / Luftfeuchtigkeitssensor
tags: ["gpio"]
---

## Funktionsweise

Der im CrowPi benutzte DHT11 Temperatur- und Luftfeuchtigkeitssensor arbeitet für die Messung der Temperatur mit einem NTC-Temperatursensor.
Dies ist ein temperaturabhängiger elektrischer Widerstand. Je nach Umgebungstemperatur leitet dieser den Strom besser oder schlechter. Diese
Differenzen lassen sich messen und anhand von Referenzwertens kann die Temperatur ausgewertet werden. Das ist ein sehr einfaches Prinzip
welches häufig zum Einsatz kommt bei der Messung von Temperaturen. Für die Feuchtigkeit liegt zwischen zwei Elektroden ein spezielles
Granulat. Je mehr Feuchte in diesem Granulat festgehalten wird, desto einfacher kann der Strom fliessen. Es ändert sich also auch der
elektrische Widerstand. Die Übertragung der Messdaten erfolgt dann etwas speziell über einen einzelnen digitalen Ausgang am Sensor. Dieser
wird in kurzen Pulsen im Mikrosekundenbereich angesteuert.

Java mit Pi4J ist leider nicht in der Lage diese Pulse genügend schnell zu verarbeiten. Die Verzögerungen durch die vielen Schichten einer
Applikation bis zur Hardware benötigen einfach zu viel Zeit. Deshalb wurde in diesem Beispiel ein Linux Treiber verwendet, welcher die
Messwerte des Sensors in eine Datei schreibt. Diese wird von Java ausgelesen und so kann dennoch mit dem Sensor gearbeitet werden. Das ist
keine optimale Lösung aber es ermöglicht immerhin die Arbeit mit dem Sensor innerhalb des CrowPi. Besser wäre eine separate Ansteuerung
mittels eines Mikrocontrollers. Diese können aufgrund der einfachheit ihres Aufbaues viel schneller auf die eingehenden Impulse reagieren
und sind so in der Lage zuverlässigere Resultate als der Raspberry Pi zu liefern.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, so dass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.HumiTempComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                               | Bemerkung                                                                                                                                                                                               |
|:--------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `HumiTempComponent()`                                                     | Initialisiert einen Temperatur- und Luftfeuchtigkeitssensor mit den Standardeinstellungen für den CrowPi.                                                                                               |
| `HumiTempComponent(int pollingDelayMs)`                                   | Initialisiert einen Temperatur- und Luftfeuchtigkeitssensor mit den Standard Dateipfaden für den CrowPi. Die Polling-Zeit in welcher neue Sensordaten gelesen werden wird jedoch manuell überschrieben. |
| `HumiTempComponent(String humiPath, String tempPath, int pollingDelayMs)` | Initialisiert einen Temperatur- und Luftfeuchtigkeitssensor mit benutzerdefinierten Pfaden zu den Messwert Dateien. Die Polling-Zeit wird ebenfalls manuell überschrieben.                              |

### Methoden

| Methode                   | Bemerkung                                               |
|:--------------------------|:--------------------------------------------------------|
| `double getTemperature()` | Gibt den letzten Temperaturmesswert in °C zurück.       |
| `double getHumidity()`    | Gibt den letzten Luftfeuchtigkeitsmesswert in % zurück. |


## Beispielapplikation

Die sehr simple Beispielapplikation misst mithilfe eines `for-loops` einige Male die Temperatur und Luftfeuchtigkeit und gibt diese auf der
Konsole aus. Verwendet werden dazu die Methoden `getHumidity()` und `getTemperature()` welche jeweils den aktuellen Messwert als `Double`
retournieren. Dieser Messwert könnte nun in weiteren Schritte wie gewünscht verarbeitet werden.

{{< code file="src/main/java/com/pi4j/crowpi/applications/HumiTempApp.java" language="java">}}

## Weitere Möglichkeiten

- Anzeige der Messwerte auf einem Display
- Schaltung des Relais abhängig von gemessenen Temperaturen.

