---
title: Troubleshooting
weight: 60
---

## Fehlerhafte PiGPIO Initialisierung
Sollte beim Ausführen einer Applikation via Launcher eine Fehlermeldung erscheinen, dass PiGPIO nicht initialisiert werden kann, so sind
entweder nicht ausreichend Rechte vorhanden oder es läuft bereits eine andere Java-Applikation welche PiGPIO blockiert. Da mit dem CrowPi
Image und den beigelegten Run Konfigurationen die Rechte automatisch korrekt vergeben werden, handelt es sich im Normalfall um das zweite
Problem. Die Fehlerausgabe auf der Konsole sieht in etwa so aus:

```
[main] WARN com.pi4j.library.pigpio.impl.PiGpioNativeImpl - PIGPIO ERROR: PI_INIT_FAILED; pigpio initialisation failed
java.lang.reflect.UndeclaredThrowableException
	at jdk.proxy2/com.sun.proxy.jdk.proxy2.$Proxy6.create(Unknown Source)
	at com.pi4j@2.0-SNAPSHOT/com.pi4j.context.Context.create(Context.java:325)
	at com.pi4j@2.0-SNAPSHOT/com.pi4j.internal.IOCreator.create(IOCreator.java:58)
    ...
```

Neben einem Geräteneustart, welcher das Problem in jedem Fall beseitigen wird, lässt sich auch über das Terminal der nachfolgende Befehl
ausführen, welcher alle Java-Prozesse auf dem System beendet: `sudo killall -9 java`

{{< img alt="Beenden aller Java-Prozesse per Terminal" src="troubleshooting/terminal-killall.png" height="500px" >}}

Anschliessend sollte es wieder möglich sein, Applikationen auf dem CrowPi zu starten. Sollte diese Alternative nicht zum Erfolg führen und
weiterhin ein Fehler auftreten, so ist das Gerät stattdessen aus- und wieder einzuschalten. Hierbei ist zu beachten, dass das Gerät komplett
stromlos gemacht werden sollte, da ein normaler Reboot nicht alle Komponenten zurücksetzt.

## Überprüfung von I²C Bus
Um schnell zu überprüfen, ob der I²C Bus ordnungsgemäss initialisiert wurde und alle Geräte zur Verfügung stehen, lässt sich am einfachsten
der Befehl `i2cdetect -y 1` via Terminal ausführen:

{{< img alt="Auflisten von I²C Bus" src="troubleshooting/terminal-i2cdetect.png" height="500px" >}}

Die Ausgabe des Befehls sollte bei Ausführung auf einem CrowPi 1 die drei Adressen `21`, `5c` und `70` auflisten. Auf anderen Gerätetypen
oder einer neuen Hardwarerevision des CrowPi kann die Ausgabe entsprechend abweichen.
