---
title: Lichtsensor
tags: ["i2c"]
---

## Funktionsweise
Der Lichtsensor nutzt photoelektronische Effekte, um Lichteinstrahlung in ein elektrisches Signal zu wandeln. Dieses Signal kann mittels Elektronik so skaliert werden, dass ein Messwert 
in der Einheit Lux entsteht. Beim CrowPi wird dazu der Umgebungslichtsensor BH1750 verwendet. Dieser arbeitet mit einer Photodiode. Zusätzlich erlaubt der verwendete Sensor eine Konfiguration 
der Messgenauigkeit. Er kann mit 3 verschiedenen Auflösungen betrieben werden. Diese sind: 0.5 Lux, 1 Lux oder 4 Lux. Die Zeit, welche für eine Messung benötigt wird, schwankt dabei massiv. Bei einer Auflösung von 4 Lux werden 
typischerweise 16 Millisekunden benötigt. Bei einer Auflösung 0.5 Lux hingegen bereits 120 Millisekunden. Diese Zeit ist natürlich auch bei der Programmierung des Sensors zu beachten. Zu schnelle
Lesevorgänge können zu ungültigen Messwerten führen.

Angesteuert wird der Lichtsensor BH1750 mittels des Bus I2C. Näheres zu diesem Bus kann unter [Hardware I2C]({{< ref "hardware/i2c" >}}) nachgeschlagen werden. Obwohl der Sensor nur wenige 
Einstellmöglichkeiten bietet, sind die Kombinationsmöglichkeiten eines Lichtsensors beinahe unendlich. Es gibt viele mögliche Szenarien wie der Sensor eingesetzt werden könnte.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, so dass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.LightSensorComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor | Bemerkung |
| --- | --- |
| `LightSensorComponent(com.pi4j.context.Context pi4j)` | Initialisiert einen Lichtsensor mit der Standard Bus- und Geräteadresse. |
| `LightSensorComponent(com.pi4j.context.Context pi4j, int address)` | Initialisiert einen Lichtsensor mit einer benutzerdefinierten Bus- und Geräteadresse. |

### Methoden
| Methode | Bemerkung |
| --- | --- |
| `double readLight()` | Liest einmalig die Helligkeit aus dem Lichtsensor. Es wird die Standardauflösung von 1 Lux genutzt.
| `double readLight(int resolution)` | Liest einmalig die Helligkeit aus dem Lichtsensor. Es können 3 verschiedene Auflösungen benutzt werden. Die gewünschte Auflösung wird als Integer gemäss folgender Definition übergeben: 0 = 4 Lux / 1 = 1 Lux / 2 = 0.5 Lux.  |

## Beispielapplikation

Die nachfolgende Beispielapplikation liest mit einem definierten Takt (`DELAY`) in Millisekunden den aktuellen Messwert des Lichtsensors aus. Die Anzahl
gelesener Werte und somit die Ausführungsdauer des Programms kann mit `NUMBER_OF_LOOPS` definiert werden. Die Applikation läuft also genau
`DELAY` mal `NUMBER_OF_LOOPS` Millisekunden. Mit den voreingestellten Werten wären das entsprechend genau 20 Sekunden. 
Nachdem der Messwert eingelesen ist, wird er mit einer einfachen Logik zu Textausgaben verarbeitet. Nach diesem Prinzip könnte zum Beispiel eine Lampe 
automatisch ein oder ausgeschaltet werden. Die Grenzwerte können dabei einfach über `DARK_VALUE` als unteren Schaltpunkt und `BRIGHT_VALUE` als oberen Schaltpunkt konfiguriert werden.
Die Wiederholung der Messung und Ausgabe wird dabei durch einen `for-loop` umgesetzt. Zu beachten gilt speziell: Da der Sensor eine kleine Zeit für eine Messung benötigt
kann eine zu kurze Taktrate zu ungültigen Messergebnissen führen. Weniger als 150 Millisekunden werden als Einstellung für `DELAY` nicht empfohlen.
{{< code file="src/main/java/com/pi4j/crowpi/applications/LightSensorApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Beispiel könnte um einen Eingang erweitert werden, mit dem die Schaltpunkte während des Betriebs verändert werden können. Dazu müsste nur die Deklaration des `DELAY` 
angepasst werden und eine Logik für einen Eingang ergänzt werden. 

- Statt die Zeitdauer über `DELAY` und `NUMBER_OF_LOOPS` zu regulieren könnte eine Benutzeraktion zum Beenden des Programms verwendet werden. Denkbar wäre dabei ein Knopf oder auch eine
Eingabe des Benutzers in der Konsole der Entwicklungsumgebung.
