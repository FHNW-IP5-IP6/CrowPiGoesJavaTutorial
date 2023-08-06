---
title: Relais
tags: ["gpio"]
---

## Funktionsweise

Das Relais ist eine Komponente, welche es ermöglichen soll mit geringen Schaltströmen grosse Lasten zu schalten. So kann beispielsweise mit
dem Raspberry Pi eine grosse Lampe geschaltet werden, ohne den Raspberry Pi zu beschädigen. Um eine Last zu verkabeln, stellt das Relais
drei Anschlüsse zur Verfügung. Diese sind beim CrowPi mit ´NC, NO, COM´ beschriftet. Die Anschlüsse für die Ansteuerung durch den CrowPi
sind wie auch bei den anderen Komponenten bereits fertig verkabelt.

Auf dem Schaltbild des Relais ist sehr gut zu erkennen, wie das Relais zu benutzen ist. Dabei stellt das Schaltsymbol immer den
stromlosen Zustand einer Komponente dar. Also der digitale Ausgang im Status `LOW` ist.  
{{< img alt="Schaltung des Relais"src="components/relay-scheme.jpg" >}} Auf dem Bild stellt das Rechteck die Spule des Relais dar. Wenn die
Spule unter Strom gesetzt wird, bewegt sich der Kontakt entsprechend auf die andere Position. So kann ein Stromkreis jeweils zwischen `COM -
NC` oder `COM - NO` geschlossen werden.

{{% notice warning %}} Beim im CrowPi verbauten Relais handelt es sich um ein **24VDC/3A oder 120VAC/3A** Produkt. Diese technische
Spezifikation ist zwingend zu beachten. Überlastung des Relais kann den CrowPi beschädigen. **Es besteht Brandgefahr!** {{% /notice %}}

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.RelayComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor                                                  | Bemerkung                                                     |
|:-------------------------------------------------------------|:--------------------------------------------------------------|
| `RelayComponent(com.pi4j.context.Context pi4j)`              | Initialisiert ein Relais mit dem Standard-Pin für den CrowPi. |
| `RelayComponent(com.pi4j.context.Context pi4j, int address)` | Initialisiert eine Relais mit einem benutzerdefinierten Pin.  |

### Methoden

| Methode                     | Bemerkung                                                                                                      |
|:----------------------------|:---------------------------------------------------------------------------------------------------------------|
| `void setStateOn()`         | Schaltet das Relais ein.                                                                                       |
| `void setStateOff()`        | Schaltet das Relais aus.                                                                                       |
| `boolean toggleState()`     | Wechselt den Zustand des Relais abhängigen vom aktuellen Zustand. Gibt den neuen Zustand als `boolean` zurück. |
| `void setState(boolean on)` | Setzt den aktuellen Zustand des Relais auf den Wert des `boolean on`.                                          |

## Beispielapplikation

Die Beispielapplikation initialisiert kurz das Relais. Danach wird es einige Male hin und her geschaltet, um das `Toggle` zu demonstrieren.
Alles in allem eine kurze und kompakte Anwendung. Die Herausforderung bei einem Relais liegt mehr in der richtigen Verkabelung als in der
Software.

{{< code file="src/main/java/com/pi4j/crowpi/applications/RelayApp.java" language="java" >}}

## Weitere Möglichkeiten

- Eine eigene Komponente wie einen kleinen PC Lüfter oder ähnliches korrekt an das Relais verkabeln und dann eine Steuerung davon
  programmieren.
- Eine LED korrekt verkabeln und mit dem
  [Lichtsensor]({{< ref "components/light-sensor" >}}) kombinieren, damit diese sich bei Dunkelheit automatisch einschaltet. 

