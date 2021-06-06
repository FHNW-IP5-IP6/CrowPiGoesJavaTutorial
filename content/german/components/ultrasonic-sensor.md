---

title: Ultraschall Distanzsensor
tags: ["gpio"]
--------------

## Funktionsweise

Der Ultraschall Distanzsensor misst mithilfe eines akustischen Signals die Distanz zu Objekten. Der Ton liegt dabei im Ultraschallbereich
und ist für den Menschen nicht hörbar. Um das Signal von Umgebungsgeräuschen unterscheiden zu können, wird nicht ein langer Ton, sondern
kurze pulsierendes Töne ausgeben. Beim im CrowPi verbauten Modell `HC-SR04` handelt es sich dabei 8 einzelne Impulse auf einer Frequenz von
40kHz. Wenn diese Töne nun auf ein Objekt treffen, werden sie reflektiert und zurück Richtung Sensor geworfen. Sobald der Sensor seinen
eigenen Ton wieder hört, schaltet er entsprechend seinen digitalen Ausgang. Zum Starten des Messvorgangs bietet der Sensor zusätzlich einen
digitalen Eingang.

Wenn nun mit einem Programm die Zeit zwischen Aussenden des Tons (Trigger) und dem Wiedereintreffen des Tons gemessen wird, kann daraus
mithilfe der Schallgeschwindigkeit die Distanz zum Objekt berechnet werden. Diese Funktion wird von der `UltrasonicDistanceSensorComponent`
übernommen. Sie liefert direkt einen Messwert in Zentimetern.

Mit dem `HC-SR04` können Distanzen zwischen von 2 bis 3000 Zentimetern gemessen werden. Dabei ist mit einer Toleranz von etwa 3 Millimetern
zu rechnen. Je nach Messdistanz variiert dieser Wert etwas. Genaueres dazu findet sich im
[Datenblatt](https://cdn.sparkfun.com/datasheets/Sensors/Proximity/HCSR04.pdf) des Sensors. Wichtig ist auch zu wissen, dass mehrere
Ultraschallsensoren sich gegenseitig stören können. Weiter ist die Schallgeschwindigkeit von der Umgebungstemperatur abhängig. Die Messwerte
können einige Prozent schwanken, wenn es kälter oder wärmer ist.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.UltrasonicDistanceSensorComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                                                             | Bemerkung                                                                                                                                             |
|:--------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------|
| `UltrasonicDistanceSensorComponent(com.pi4j.context.Context pi4j)`                                      | Initialisiert einen Ultraschall Distanz Sensor mit dem Standard-Pin für den CrowPi.                                                                   |
| `UltrasonicDistanceSensorComponent(com.pi4j.context.Context pi4j, int triggerAddress, int echoAddress)` | Initialisiert einen Ultraschall Distanz Sensor mit benutzerdefinierten Pins. Trigger ist dabei der digitale Eingang und Echo der Ausgang des Sensors. |

### Methoden

| Methode                                                    | Bemerkung                                                                                                                                                                                                 |
|:-----------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `double measure()`                                         | Führt eine Messung aus und gibt die aktuelle Distanz [cm] zurück. Nutzt dabei die aktuelle Temperatureinstellung.                                                                                         |
| `double measure(double temperature)`                       | Führt eine Messung aus und gibt die aktuelle Distanz [cm] zurück. Bei der Distanzberechnung wird zudem der Einfluss der Umgebungstemperatur auf die Schallgeschwindigkeit berücksichtigt und kompensiert. |
| `void onObjectFound(SimpleEventHandler handler)`           | Setzt den Event Handler, welcher bei Erkennung eines Objekts aufgerufen werden soll. `null` deaktiviert diesen Event Listener.                                                                            |
| `void onObjectDisappeared(SimpleEventHandler handler)`     | Setzt den Event Handler, welcher bei Verschwinden eines Objekts aufgerufen werden soll. `null` deaktiviert diesen Event Listener.                                                                         |
| `void setMeasurementTemperature(double temperature)`       | Definiert die Standardmesstemperatur, welche in die Distanzberechnung einbezogen wird.                                                                                                                    |
| `void setDetectionRange(double minRange, double maxRange)` | Definiert die Distanz welche gemessen werden muss, um als Objekt erkannt zu werden.                                                                                                                       |

## Beispielapplikation

Die nachfolgende Beispielapplikation führt als erstes unter Verwendung von `onObjectFound` eine Objekterkennung durch. Es wird jeweils
ausgegeben, ob ein Objekt gefunden wurde und ebenfalls, wenn es wieder verschwunden ist. Danach werden einige Messwerte mit verschiedenen
Temperatureinstellungen ausgegeben. So kann der Einfluss von Temperaturschwankungen auf den Messwert hervorragend analysiert werden.
Anschliessend wird eine kurze `for`-Schleife gestartet welche jede Sekunde einen neuen Messwert ausgibt. Dies verschafft etwas Zeit, um auch
einmal die Hand über den Sensor zu halten.

{{< code file="src/main/java/com/pi4j/crowpi/applications/UltrasonicDistanceSensorApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Beispiel um einen Temperatursensor erweitern, um immer korrekte Messwerte zu erhalten.

