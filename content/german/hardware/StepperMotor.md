---
title: Stepper Motor
tags: ["spi"]
---

Schrittmotoren bestehen aus mehreren Spulen, welche um die in ihrer Mitte liegenden Welle angeordnet sind. Die Welle wird von einem Zylinder umschlossen, welcher abwechselnd mit positiven und negativen Polen versehen ist. Sobald durch eine Spule Strom fliesst, erzeugt diese ein Magnetfeld, nach welchem sich die Pole im Zylinder der Welle ausrichten. Somit kann die Welle gedreht werden. Siehe auch [Stepper Motor Control with the Raspberry Pi](https://www.youtube.com/watch?v=Dc16mKFA7Fo).

![Stepper Motor unter Single Stepping](https://upload.wikimedia.org/wikipedia/commons/6/67/StepperMotor.gif)
Stepper Motor unter Single Stepping

Bei den meisten Schrittmotoren kommt zusätzlich zur Welle noch ein Getriebe zum Einsatz, welches durch seine Übersetzung den Drehwinkel pro Schritt weiter verkleinert. Beim weit verbreiteten Stepper Motor 28BYJ-48 ist zum Beispiel ein 1/64 Getriebe verbaut. Jeder Schritt der Welle resultiert bei diesem Motor um eine um 64 verkleinerte Drehung.

Siehe: [DesignSpark: Stepper motors and drives, what is full step, half step and microstepping](https://www.rs-online.com/designspark/stepper-motors-and-drives-what-is-full-step-half-step-and-microstepping)

### Single Stepping
Beim Single Stepping wird jeweils nur eine Spule im Gehäuse des Motors unter Strom gesetzt. Die Welle des Motors richtet sich also immer genau nach der momentan aktiven Spule aus. 

### Double Stepping
Unter Verwendung des Double Stepping werden immer zwei Spulen simultan unter Strom gesetzt. Dies führt dazu, dass sich die Welle gemäss dem entstehenden Magnetfeld zwischen den zwei Spulen ausrichtet. Die sich überschneidenden Magnetfelder sind stärker als ein einzelnes Magnetfeld, weshalb der Schrittmotor unter diesem Modus mit grösserer Kraft arbeitet. Da allerdings zwei Spulen gleichzeitig mit Strom beliefert werden müssen, verdoppelt sich der Stromverbrauch im Vergleich zum Single Stepping. 

### Half Stepping
Im Gegensatz zu den anderen Modi agiert Half Stepping in acht Schritten. Somit kann die Position des Motors genauer bestimmt werden als in den anderen Modi. Half Stepping kombiniert Single Stepping und Double Stepping. Dies führt dazu, dass der Motor nicht immer gleich viel Kraft hat. Bei jedem zweiten Schritt ist jeweils nur eine Spule aktiv.

