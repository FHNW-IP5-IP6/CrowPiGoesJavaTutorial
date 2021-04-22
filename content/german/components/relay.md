---
title: Relais
tags: ["gpio"]
---

## Funktionsweise
???

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="ch.fhnw.crowpi.components.RelayComponent" >}} beschrieben.

### Konstruktoren

| Konstruktor | Bemerkung |
| --- | --- |
| `RelayComponent(com.pi4j.context.Context pi4j)` | Initialisiert ein Relais mit dem Standard-Pin für den CrowPi. |
| `RelayComponent(com.pi4j.context.Context pi4j, int address)` | Initialisiert eine Relais mit einem benutzerdefinierten Pin. |

### Methoden

| Methode | Bemerkung |
| --- | --- |
| `void ???()` | text text text  |

## Beispielapplikation

????

{{< code file="src/main/java/ch/fhnw/crowpi/applications/RelayApp.java"language="java" >}}

## Weitere Möglichkeiten

- ????
- ????
