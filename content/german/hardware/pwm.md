---
title: Pulse Width Modulation (PWM)
tags: ["pwm"]
---

## Das Wichtigste in Kürze
Die Abkürzung PWM steht für Pulse Width Modulation und wird im Deutschen auch oft als Pulsbreitenmodulation oder Pulsweitenmodulation 
bezeichnet. Diese Technik wird unter anderem für die Steuerung von Servomotoren eingesetzt und findet beispielsweise auch Verwendung bei den 
Lüftern von einem regulären Computer.

Mit PWM ist es möglich, eine Komponente wie einen Motor nicht nur mehr rein binär zu steuern, sprich Aus (0% Leistung) oder An (100% 
Leistung), sondern diese fast beliebig zu steuern. Die Funktionsweise von PWM funktioniert hierbei so, dass die Komponente in einem 
bestimmten Zeitraum immer wieder aus- und angeschaltet wird.

Ein gutes Beispiel hierfür ist beim CrowPi die [Buzzer-Komponente]({{< ref "components/buzzer" >}}), welche über einen digitalen 
Anschluss verfügt und eigentlich nur einen einzigen Ton abspielen kann. Um damit nun trotzdem Melodien wie im verlinkten Beispiel zu 
erzeugen, wird der Buzzer mit PWM für die gewünschte Tonfrequenz konfiguriert und schaltet sich so beispielsweise bei der Note `C7` 
insgesamt 2093x pro Sekunde aus und wieder an, was somit von uns als C7 wahrgenommen wird.

## Software vs. Hardware
Auf dem Raspberry Pi und somit auch dem CrowPi stehen zwei verschiedene Arten von PWM zur Verfügung, konkret eine Software- sowie eine 
Hardware-Implementation. Beide bieten grundsätzlich die gleichen Möglichkeiten, jedoch können mit der Software-Version keine präzisen 
oder besonders schnellen Frequenzen erreicht werden.

Der Grund dafür ist dass bei der Software-Implementation für jeden einzelnen Zyklus (an/aus) wieder ein erneuter Steuerbefehl von der 
JVM (Java Virtual Machine) bis zur entsprechenden Komponente übertragen werden muss, während bei der Hardware-Implementation der 
Raspberry Pi sich die gewünschte Frequenz merkt und diese selbstständig direkt auf der Platine regelt.

## Technische Implementation
Zur technischen Ansteuerung einer Komponente mit PWM müssen zweit Werte definiert werden:

- **Puls-Pause-Verhältnis (englisch: Duty Cycle):** Dieser Wert definiert das Verhältnis zwischen dem ein- und ausgeschalteten Zustand 
  und wird durch eine Zahl zwischen 0% und 100% repräsentiert. Ein Wert von 50% bedeutet, dass innerhalb von einem Zyklus exakt die 
  Hälfte der Zeit die Komponente ein- und anschliessend ausgeschaltet ist. Ein Wert von 25% hingegen würde bedeuten, dass nur ein 
  Viertel der Zeit die Komponente eingeschaltet ist und die restlichen drei Viertel vom Zyklus die Komponente im ausgeschalteten Zustand 
  verweilt.
  
- **Frequenz (englisch: Frequency):** Dieser Wert definiert wie oft pro Sekunde ein Zyklus (an/aus) fur diese Komponente stattfindet und 
  wird üblicherweise in der Einheit Hertz (Hz) angegeben. Bei einem Wert von 10Hz würde die Komponente somit 10x zwischen dem ein- und 
  ausgeschalteten Zustand in einer Sekunde alternieren.

Diese beiden Werte lassen sich über die Pi4J-Bibliothek steuern und werden auch von diesem Projekt intern verwendet.

## Weitere Informationen
- [Wikipedia zu PWM](https://de.wikipedia.org/wiki/Pulsdauermodulation)
- [Wikipedia mit Tonfrequenzen](https://de.wikipedia.org/wiki/Frequenzen_der_gleichstufigen_Stimmung)
