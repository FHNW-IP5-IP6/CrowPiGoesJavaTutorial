---
title: JDK Setup
weight: 30
disableToc: true
---

## Installation auf dem Entwickler-Laptop
Im Pi4J-CrowPi-OS Image ist Java in der Version 17 (das sogenannte JDK) bereits vorinstalliert. Daher verwenden wir diesen JDK auch auf dem Laptop. Download: [Java 17](https://adoptium.net/?variant=openjdk17&jvmVariant=hotspot) 

Hinweis für Mac-Benutzer: Die Verwendung von SDKMAN (s.u.) für die Installation und die Verwaltung von JDKs ist sehr empfehlenswert.


### Empfehlung zur Installation des JDK für MacOS (und Linux)

Für MacOs und Linux gibt es ein sehr empfehlenswertes Tool zur Verwaltung unterschiedlicher Software Development Kits: [SDKMAN](https://sdkman.io)

Insbesondere wenn, wie üblich, mehrere Java JDKs verwendet werden, hilft SDKMAN.

#### Installation von SDKMAN
Folgenden Befehl in einem Terminal eingeben:
 ```shell
 export SDKMAN_DIR="$HOME/sdkman" && curl -s "https://get.sdkman.io" | bash
 ```

Falls Sie SDKMAN bereits früher installiert haben, müssen Sie SDKMAN auf den neuesten Stand bringen:

```shell
sdk update
```

#### Installation des JDK
In einem neuen Terminal-Window diesen Befehl eingeben:

```shell
sdk install java 17.0.8-tem
```

Danach liegt der JDK in ihrer Home-Directory im Folder `sdkman/candidates/java`. Von dort können Sie es dann in IntelliJ als neues SDK anlegen und im Projekt verwenden.

Mit:

```shell
sdk ls java
```

kann man sich auflisten lassen, welche anderen JDKs zur Installation zur Verfügung stehen.

### Java Version überprüfen

In einem Terminal-Window eingeben
```shell
java -version
```

Das sollte diese Ausgabe erzeugen
```shell
openjdk version "17.0.8" 2023-07-18
OpenJDK Runtime Environment Temurin-17.0.8+7 (build 17.0.8+7)
OpenJDK 64-Bit Server VM Temurin-17.0.8+7 (build 17.0.8+7, mixed mode)
```

Falls das nicht der Fall ist, muss der Default-JDK umgestellt werden. Mit SDKMAN geht das einfach:

```shell
sdk default java 17.0.8-tem
```
