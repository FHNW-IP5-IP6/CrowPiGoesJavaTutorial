---
title: Buzzer
---

## Funktionsweise
TODO

## Verwendung
TODO

## Beispielapplikation
Die nachfolgende Beispielapplikation steuert den Buzzer via PWM an, um eine bekannte Melodie abzuspielen. Hierfür wird ein Enum namens 
`Note` verwendet, welches die entsprechenden Musiknoten auf eine Frequenz in Hertz (Hz) abbildet. Die Applikation spielt nacheinander 
alle Noten ab, indem die Funktionen `playTone` sowie `playFrequency` der Buzzer-Komponente verwendet werden.

{{< code file="src/main/java/ch/fhnw/crowpi/applications/BuzzerApp.java" language="java" >}}
