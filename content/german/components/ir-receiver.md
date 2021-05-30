---
title: Infrarot Empfänger
tags: ["gpio"]
---

## Funktionsweise

Mithilfe von Infrarot können über einige Meter Distanz digitale Informationen übertragen werden, was zum Beispiel oft für die Fernbedienung
eines Fernsehers genutzt wird. Die Übertragungsart gleich hierbei etwas dem Prinzip des Morsens, da die Infrarot-LED im Sendegerät eine
Abfolge von Bits durch unterschiedlich lange Pausen zu dem Empfängergerät überträgt. Da jedoch auch viele andere Dinge in der Natur wie
beispielsweise unsere Sonne ein Infrarot-Licht emittieren, wird für die elektronische Verwendung nur eine Frequenz von 38kHz verwendet.

Um zu vermeiden, dass eine Fernbedienung über eine oder gar mehrere Sekunden komplett still gehalten werden muss, werden die einzelnen Bits
in sehr kurzen zeitlichen Abständen übertragen. Dies klingt im ersten Moment natürlich praktisch, bedeutet jedoch auch dass das empfangende
Gerät sehr präzise zeitliche Messungen durchführen können muss. Genau dies war jedoch beim CrowPi mit der Verwendung von Java in Kombination
mit Pi4J nicht möglich, da eine Genauigkeit von rund 60 Mikrosekunden (0.00006 Sekunden) benötigt wird, was sich mit dem Design der Java
Virtual Machine nicht umsetzen lässt.

Um diese Komponente jedoch trotzdem zur Verfügung zu stellen, nutzt diese Implementation eine vorinstallierte Applikation namens `mode2`
welche in C geschrieben wurde. Diese gibt die entsprechend erkannten Signale über die Konsole aus, welche dann automatisch in Java
ausgewertet, geprüft und weitergegeben werden. Ein grosser Teil der Logik ist somit weiterhin innerhalb dieser Komponenten-Klasse
aufzufinden, das Erkennen der Signale findet jedoch nicht mit Pi4J statt. Sollten sich hier in Zukunft neue Möglichkeiten ergeben, so würde
sich die Komponente regulär via GPIO auslesen lassen.

Eine saubere Alternative, welche sich weiterhin mit Pi4J umsetzen lässt, wäre einen dedizierten Mikrocontroller für das Empfangen der
Infrarot-Signale zu verwenden, welcher dann über einen anderen Bus wie I²C oder SPI die Signale an den Raspberry Pi sendet. Auf dem CrowPi
ist dies jedoch nicht vorhanden, sodass diese Möglichkeit nicht bestand.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.IrReceiverComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                  | Bemerkung                                                                                                           |
|:-------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------|
| `IrReceiverComponent()`                                      | Initialisiert einen Infrarot-Empfänger mit Standardeinstellungen für den CrowPi.                                    |
| `IrReceiverComponent(String mode2Binary, String devicePath)` | Initialisiert einen Infrarot-Empfänger mit benutzerdefiniertem Pfad zur `mode2` Applikation sowie einem Gerätepfad. |

### Methoden

|                                                | Methode                                                                                                                                                                                                | Bemerkung |
|:-----------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------|
| `void onKeyPressed(EventHandler<Key> handler)` | Setzt den Event Handler, welcher gedrückte Tasten auf der Infrarot-Fernbedienung als ersten Parameter erhält. Durch die Angabe von `null` kann das Empfangen von Infrarot-Signalen deaktiviert werden. |           |

### Enumerationen

- {{< javadoc class="com.pi4j.crowpi.components.IrReceiverComponent" subclass="Key" >}} enthält alle unterstützten Tasten, welche sich auf
  der beigelegten Infrarot-Fernbedienung des CrowPi befinden. Diese Enumeration kann genutzt werden, um innerhalb eines Events die erhaltene
  Taste zu vergleichen und unterschiedliche Aktionen durchzuführen.

## Beispielapplikation

Die Beispielapplikation initialisiert zuerst den Infrarot-Empfänger, wobei hier nicht einmal der übliche Pi4J-Kontext übergeben werden muss,
da wie bereits weiter oben beschrieben von dieser Komponente die Pi4J-Library nicht genutzt wird. Nach erfolgter Initialisierung wird ein
Event Handler registriert, welcher jeden Tastendruck auf der Konsole ausgibt sowie beim Drücken der "CH" Taste eine zusätzliche Ausgabe
vornimmt. Anschliessend wird 30 Sekunden gewartet, bevor der Event Handler wieder deaktiviert wird und die Applikation sich beendet.

{{< code file="src/main/java/com/pi4j/crowpi/applications/IrReceiverApp.java" language="java" >}}

## Weitere Möglichkeiten

- Mit der beigelegten Fernbedienung lassen sich einwandfrei numerische Eingaben tätigen, z.B. um einen einfachen Taschenrechner mit Addition
  und Subtraktion zu bauen, wofür sich die Zahlentasten sowie Plus und Minus nutzen lassen würden.
- Die Steuerungstasten für Previous, Next sowie Play/Pause könnten genutzt werden, um den Buzzer zu steuern und verschiedene Melodien damit
  abzuspielen.
