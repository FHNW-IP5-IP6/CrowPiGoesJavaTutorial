---
title: RFID
tags: ["spi"]
---

## Funktionsweise

Bei RFID, abgekürzt für Radio Frequency Identification, handelt es sich um eine Technologie, welche es ermöglicht automatisch und kontaktlos
mit einer Karte entsprechende Informationen auszutauschen. Dies wird zum Beispiel bei weit bekannten NFC Tags verwendet, jedoch auch für
Anwendungen wie das kontaktlose Zahlen per Kreditkarte.

Ein grosser Vorteil ist hierbei, dass die jeweiligen Karten/Tags, auch Transponder genannt, so klein wie ein Reiskorn sein können und keine
eigene Stromversorgung benötigen. Im CrowPi ist die Komponente `MFRC522` verbaut, welche ein hochfrequentes elektronisches Wechselfeld bei
13.56 MHz erzeugt, um damit einerseits die Karte mit Strom zu versorgen und andererseits mit dieser zu kommunizieren. Somit können
beispielsweise die beiden beigelegten Tags des CrowPi ausgelesen und beschrieben werden. Dies funktioniert dabei mit beliebigen Daten.

Die Technologie dahinter ist sehr komplex und die entsprechenden Informationen dazu sind auf mehrere Standards aufgeteilt, hauptsächlich
`ISO-14443` für den allgemein Aufbau und die Kommunikation sowie die entsprechenden Spezifikationen von NXP Semiconductors, dem Hersteller
des Lese- und Schreibgeräts sowie von den meisten RFID Karten. Die Einhaltung dieser Standards ist hierbei sehr wichtig, da eine RFID Karte
bei falschen Schreiboperationen auch permanent beschädigt werden kann.

Es besteht hier jedoch kein Grund zur Sorge, da sich diese Komponente um all die komplexen Aufgaben im Hintergrund kümmert und eine einfache
sowie zuverlässige Schnittstelle bietet, um mit solchen RFID Karten zu interagieren. So bietet `RfidComponent` die Möglichkeit an, Karten zu
erkennen und ein beliebiges Java-Objekt darauf zu speichern oder wieder zu lesen.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.RfidComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                                                       | Bemerkung                                                                                                              |
|:--------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------|
| `RfidComponent(com.pi4j.context.Context pi4j)`                                                    | Initialisiert einen RFID Reader/Writer mit der Standard Reset-Pin und SPI Adresse sowie SPI Bandbreite für den CrowPi. |
| `RfidComponent(com.pi4j.context.Context pi4j, int spiChannel, int spiBaud)`                       | Initialisiert einen RFID Reader/Writer mit einer benutzerdefinierten SPI Adresse und Bandbreite ohne Reset-Pin.        |
| `RfidComponent(com.pi4j.context.Context pi4j, Integer gpioResetPin, int spiChannel, int spiBaud)` | Initialisiert einen RFID Reader/Writer mit einem benutzerdefinierten Reset-Pin, SPI-Adresse sowie Bandbreite.           |

### Methoden

| Methode                                               | Bemerkung                                                                                                                                                                                                                                               |
|:------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `boolean isNewCardPresent()`                          | Gibt einen boolschen Wert zurück, ob eine neue Karte erkannt wurde, welche noch nicht gelesen wurde.                                                                                                                                                    |
| `boolean isAnyCardPresent()`                          | Gibt einen boolschen Wert zurück, ob irgendeine Karte erkannt wurde. Diese Methode erkennt im Gegensatz zu `isNewCardPresent()` auch bereits gelesene Karten.                                                                                           |
| `RfidCard initializeCard()`                           | Erzeugt eine neue Instanz von `RfidCard`, welche anschliessend zur Interaktion mit der Karte verwendet werden kann. Erfordert dass `isNewCardPresent` oder `isAnyCardPresent` vorher aufgerufen wurde und `true` zurückgab.                             |
| `void uninitializeCard()`                             | Muss zwingend nach dem Abschliessen aller Aktionen mit einer Karte aufgerufen werden, um die Kommunikation mit der Karte sauber zu beenden und neue Karten erkennen zu können.                                                                          |
| `void onCardDetected(EventHandler<RfidCard> handler)` | Definiert einen Event Handler, welcher jedes Mal aufgerufen wird wenn eine neue Karte erkennt wird. Als erster Parameter wird hierbei die erkannte Karte übergeben.                                                                                     |
| `void waitForNewCard(EventHandler<RfidCard> handler)` | Gleiche Funktionsweise wie `onCardDetected`, blockiert jedoch den aktuellen Thread und wartet bis eine neue Karte erkannt wird, um dann einmalig einen Handler aufzurufen. Diese Methode deaktiviert den aktuellen Event Listener für `onCardDetected`. |
| `void waitForAnyCard(EventHandler<RfidCard> handler)` | Gleiche Funktionsweise wie `waitForNewCard`, akzeptiert jedoch auch eine Karte welche bereits gelesen oder beschrieben wurde und führt auf dieser die Aktion durch. Diese Methode deaktiviert den aktuellen Event Listener für `onCardDetected`.                                                                                     |
| `void reset()`                                        | Setzt die Komponente auf den Ausgangszustand zurück, was bei allfälligen Problemen Abhilfe schaffen kann. Normalerweise nicht erforderlich für den normalen Betrieb.                                                                                    |

### Karten-Methoden

Die Klasse {{< javadoc class="com.pi4j.crowpi.components.internal.rfid.RfidCard" >}} welche als Rückgabewert bei `initializeCard()` oder als
Parameter bei den Event Handlern `onCardDetected()`, `waitForNewCard()` sowie `waitForAnyCard` verwendet wird, bietet verschiedene Methoden
um mit der erkannten Karte zu interagieren. Wichtig zu beachten ist, dass nach Ausführung eines Event Handlers automatisch
`uninitializeCard()` aufgerufen wird, sodass sich die erkannte Karte nicht weiter nutzen lässt.

| Methode                         | Bemerkung                                                                                                                                                                                                                                                     |
|:--------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `String getSerial()`            | Gibt die Seriennummer der aktuellen Karte als String zurück.                                                                                                                                                                                                  |
| `int getCapacity()`             | Gibt die maximale Kapazität der aktuellen Karte in Bytes zurück.                                                                                                                                                                                              |
| `void writeObject(Object data)` | Schreibt das übergebene Objekt in die Karte. Wird dieser Befehl mehrfach aufgerufen, so wird dieses jeweils überschrieben. Das Objekt muss das Interface `Serializable` implementieren. Wirft eine `RfidException` falls die Operation nicht erfolgreich war. |
| `Object readObject()`           | Liest das auf der Karte gespeicherte Objekt aus und gibt dieses als generischen `Object`-Typ zurück. Wirft eine `RfidException` falls die Operation nicht erfolgreich war.                                                                                    |
| `T readObject(Class<T> type)`   | Gleiche Funktionsweise wie `readObject()`, aber gibt das Objekt gleich mit dem gewünschten Typ zurück. Wurde zum Beispiel mit `writeObject` vorher ein `Animal` gespeichert, so gibt `readObject(Animal.class)` direkt die `Animal`-Instanz zurück.           |

## Beispielapplikation

Die nachfolgende Beispielapplikation definiert eine Klasse namens `Person`, um von einer beliebigen Person den Vornamen und Nachnamen, die
Adresse sowie das Geburtsdatum zusammen mit einer zufällig generierten ID zu speichern. Nun werden zuerst zwei Personen generiert und in den
Variablen `personA` sowie `personB` gespeichert, welche beide über verschiedene Daten verfügen.

Anschliessend wird mit der `waitForNewCard` Methode die Anwendung so lange blockiert, bis eine neue Karte vom RFID Gerät erkannt wurde.
Sobald dies der Fall ist, wird die übergebene Funktion ausgeführt, welche die Instanz `personA` von der `Person`-Klasse auf der Karte mit
`writeObject` speichert. Es kann jedes beliebige Java-Objekt gespeichert werden, solange dieses das `Serializable` mit `implements
Serializable` implementiert. Wichtig ist hierbei zu beachten, dass diese Methode eine `RfidException` werfen kann, welche abgefangen werden
muss. Diese tritt auf, wenn die Karte nicht ordnungsgemäss beschrieben werden konnte.

Nachdem die erste Karte beschrieben wurde, wird der gleiche Prozess für die zweite Karte wiederholt. Da mit `waitForNewCard` grundsätzlich
nur neue Karten erkannt werden und eine Karte nach erfolgter Interaktion in einen Schlafzustand geht, kann hier garantiert werden dass nicht
sofort die zweite Person auf die gleiche Karte geschrieben wird, sondern sich eine neue Karte annähern oder die bestehende Karte kurzfristig
entfernt werden muss.

Sobald beide Karten beschrieben wurden, wird ein Event Handler mit `onCardDetected` registriert, welcher asynchron jedes Mal aufgerufen wird
wenn eine neue Karte erkannt wurde. Da der RFID-Standard ein Verfahren gegen Kollisionen besitzt, können sich sogar mehrere Karten
gleichzeitig auf dem Lese- und Schreibgerät befinden. Bei jeder ermittelten Karte wird mit `readObject(Person.class)` versucht, eine vorher
gespeicherte Instanz der `Person`-Klasse auszulesen und in die Variable `person` zu speichern. Wenn dies gelingt, so wird die Person auf der
Konsole ausgegeben. Auch hier muss eine allfällige `RfidException` aufgefangen werden, welche zum Beispiel auftritt, wenn die Daten nicht
gelesen werden können oder korrupt sind.

Nach der Registrierung des Event Handlers schläft die Applikation für 30 Sekunden, um genug Zeit zu geben, die zwei verwendeten Karten
auszuprobieren. Nach Ablauf der Zeit wird der Event Handler wieder sauber entfernt und die Applikation beendet sich.

{{< code file="src/main/java/com/pi4j/crowpi/applications/RfidApp.java" language="java" >}}

## Weitere Möglichkeiten

- Das Beispiel zu einer Art Zutrittskontrolle ausbauen und beispielsweise nur Personen mit einem bestimmten Attribut zulassen.

- Statt dem Speichern von Personen könnten auch andere Daten auf einer Karte abgelegt werden, zum Beispiel eine (nicht sehr sichere)
  Implementation einer Bank wo auf jeder Karte der Inhaber sowie der aktuelle Kontostand gespeichert wird. Hierfür wäre die Methode
  `waitForAnyCard` praktisch, um eine bereits aufgelegte Karte erneut zu beschreiben.
