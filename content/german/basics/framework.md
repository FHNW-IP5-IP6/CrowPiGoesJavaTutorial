---
title: Application Framework
weight: 40
---

## Grundlagen
Um ein möglichst einfaches Arbeiten mit diesem Projekt zu gewährleisten, wurde ein einfaches Application Framework definiert. Dieses 
besteht aus einem regulären Java Interface und schon mit wenigen Zeilen Code kann eine neue Applikation mit eigenem Code gebaut werden. 
An jede Applikation wird hierbei jeweils der Pi4J `Context` übergeben, mit welchem alle Komponenten des CrowPi angesteuert werden können.

## Starten einer Applikation
Der integrierte Launcher kann zu jeder Zeit mit Maven gestartet werden. Falls der Code direkt auf dem Raspberry Pi zur Verfügung steht, 
kann zum Beispiel mit folgenden Befehlen eine Applikation gestartet werden:

```shell
# Führt den Launcher ohne Angabe einer Applikation aus, worauf ein Auswahlmenü erscheint.
# Nach Selektion der gewünschten Applikation wird diese schliesslich gestartet.
mvn install

# Führt direkt eine bestimmte Applikation mit dem Launcher aus ohne Umweg über das Auswahlmenü.
# Im Beispiel wird direkt die "BuzzerApp" gestartet.
mvn install -Dcrowpi.launcher.args=BuzzerApp
```

## Neue Applikation anlegen
Um eine neue Applikation anzulegen, welche anschliessend über den integrierten Launcher gestartet werden kann, sind nur wenige einfache 
Schritte erforderlich. Die nachfolgende Anleitung führt Schritt für Schritt zur ersten eigenen Applikation.

Zuerst muss eine Java-Klasse unterhalb von `src/main/java/ch/fhnw/crowpi/applications` erstellt werden mit einem beliebigen Namen, 
welche das Interface `Application` implementiert. Zum Beispiel könnte diese Klasse `ExampleApp` heissen und folgenden Code enthalten:

{{< code file="src/main/java/ch/fhnw/crowpi/applications/ExampleApp.java" language="java" >}}

Anschliessend muss die Applikation nur noch in der Klasse `ch.fhnw.crowpi.Launcher` zur Liste `APPLICATIONS` hinzugefügt werden. Die 
Datei ist unter `src/main/java/ch/fhnw/crowpi/Launcher.java` aufzufinden und muss an dieser Stelle erweitert werden:

```java
public final class Launcher implements Runnable {
    // ...
    private static final List<Application> APPLICATIONS = new ArrayList<>(Arrays.asList(
        new ExampleApp(),   // hier wird die neue Applikation eingefügt
        new BuzzerApp()
    ));
    // ...
}
```

Nun kann diese neue Applikation beliebig erweitert werden, indem weiterer Code innerhalb der `execute()` Methode hinzugefügt wird. Die 
Applikation kann jederzeit durch Starten des Launchers getestet werden und erscheint bei fehlender Angabe der gewünschten Applikation 
nun dort auch automatisch in der Auswahlliste.

Optional ist es auch möglich, den Namen oder die Beschreibung der Applikation selber anzupassen. Es ist hierbei wichtig, dass der Name 
unter allen eingehängten Applikationen jeweils eindeutig ist. Um diese Änderungen durchzuführen, können diese Methoden in der eigenen 
Applikationsklasse verwendet werden:

```java
public class ExampleApp implements Application {
    // ...
    @Override
    public String getName() {
        return "CoolApp";
    }

    @Override
    public String getDescription() {
        return "Meine coole Applikation";
    }
    // ...
}
```

Der Name entspricht hierbei auch dem Parameter, welcher an den Launcher übergeben werden muss, um die Applikation direkt zu starten. Im 
Normalfall ist es aber aufgrund potenzieller Namenskonflikte nicht empfohlen, diesen zu überschreiben.

## Das Interface
Zur Vollständigkeit ist hier noch das komplette `Application` Interface eingebunden. Dieses besteht aus exakt drei Methoden, wobei 
hiervon nur `execute()` implementiert werden muss da die anderen Methoden bereits über eine Standardimplementation verfügen.

{{< code file="/src/main/java/ch/fhnw/crowpi/Application.java" language="java" >}}
