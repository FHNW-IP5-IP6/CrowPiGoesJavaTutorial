---

title: Sound Sensor
tags: ["gpio"]
--------------

## Funktionsweise

Ein Sound Sensor funktioniert mittels eines Mikrofons. Die vom Mikrofon in elektrische Signale umgewandelten akustischen Schwingungen werden
gegen einen Schwellenwert verglichen. Sobald die elektrischen Signale in ihrer Stärke einen gewissen Schwellwert übersteigen, wird am Sensor
ein digitaler Ausgang geschaltet. Es wurde Lärm oder ein Geräusch erkannt.

Am CrowPi kann dieser Schwellwert mittels eines Potentiometers eingestellt werden. Am einfachsten geht das, wenn man auf das entsprechende
LED bei den Status LEDS achtet und den entsprechenden Lärm verursacht welcher erkannt werden soll. Wird das Potentiometer nach rechts
gedreht, muss das entsprechende akustische Signal lauter sein, um vom Sensor erkannt zu werden. Auf die linke Seite gedreht wird der Sensor viel empfindlicher gegenüber leisen Geräuschen. Für den ersten Test am besten ganz nach rechts drehen. Zu finden ist das Potentiometer wie auf diesem Bild gezeigt: {{< img
alt="Potentiometer des Sound Sensor" src="components/sound-sensor-potentiometer.jpg" >}}

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.SoundSensorComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                                       | Bemerkung                                                                                                                                                   |
|:----------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `SoundSensorComponent(com.pi4j.context.Context pi4j)`                             | Initialisiert einen Sound Sensor mit dem Standard-Pin für den CrowPi.                                                                                       |
| `SoundSensorComponent(com.pi4j.context.Context pi4j, int address, long debounce)` | Initialisiert einen Sound Sensor mit einem benutzerdefinierten Pin. Zusätzlich kann mit `debounce` noch eine Entprellzeit in Mikrosekunden angegeben werden |

### Methoden

| Methode                                      | Bemerkung                                                                                                                          |
|:---------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------|
| `boolean isNoisy()`                          | Gibt true zurück, wenn aktuell ein Geräusch vom Sensor erkannt wird                                                                |
| `boolean isSilent()`                         | Gibt true zurück, wenn aktuell Stille herrscht.                                                                                    |
| `SoundState getState()`                      | Gibt den aktuellen Zustand des Sensors in Form des SoundState zurück.                                                              |
| `void onNoise(SimpleEventHandler handler)`   | Setzt den Event Handler, welcher bei auftretendem Lärm am Sensor aufgerufen werden soll. null deaktiviert diesen Event Listener.   |
| `void onSilence(SimpleEventHandler handler)` | Setzt den Event Handler, welcher bei verschwundenem Lärm am Sensor aufgerufen werden soll. null deaktiviert diesen Event Listener. |

### Enumerationen

- {{< javadoc class="com.pi4j.crowpi.components.SoundSensorComponent" subclass="SoundState" >}} enthält alle möglichen Zustände welche vom
  Sound Sensor zurückgegeben werden können.

## Beispielapplikation

Bei dieser Komponente wurde ein sehr simples Beispiel gewählt. Damit die Applikation jedoch richtig funktioniert, muss erst der Sound Sensor
so eingestellt werden, dass ein Händeklatschen erkannt wird. Am besten wie in der Funktionsweise beschrieben kurz ausprobieren. Als erstes
wird mit einer simplen Statusabfrage geprüft, ob gerade stille im Raum herrscht. Falls es gerade schon zu laut wäre, würde das Programm
abbrechen. Ist es ruhig registriert das Programm einen `onNoise` Event Handler, welcher mittels einer Zählvariable zählt, wie oft schon Lärm
erkannt wurde. Nach 3x Händeklatschen beendet die Applikation wieder. Für das Zählen in einer Lambdafunktion in Java muss ein spezieller
Datentyp verwendet werden. Man sieht dies am `AtomicInteger count`. Der `AtomicInteger` ist eine spezielle Form eines normalen Integers,
welcher jedoch auch innerhalb einer Lambdafunktion benutzt werden kann.{{< code
file="src/main/java/com/pi4j/crowpi/applications/SoundSensorApp.java" language="java" >}}

## Weitere Möglichkeiten

- Mit der Relaiskomponente kombiniert könnte eine Lampe mittels Klatschen ein- und ausgeschaltet werden.
- Es könnte eine Alarmanlage gebaut werden, welche anhand von Lärm einen Eindringling erkennt.

