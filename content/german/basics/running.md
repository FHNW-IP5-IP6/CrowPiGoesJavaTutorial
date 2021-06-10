---
title: Starten der Applikation
weight: 20
---
In diesem Kapitel wird genauer beschrieben, was alles beim Starten einer Applikation passiert und wie das Framework benutzt werden kann. 
Der genaue Aufbau des [Application Frameworks]({{< ref "basics/framework" >}}) welches dies alles auf diese Art und Weise ermöglicht, ist 
in einem separaten Kapitel zu finden. 

---

## Starten des Application Framework
Grundsätzlich wurde ein erster Start bereits zum Test am Ende des Setups ausgeführt. Benötigt wurden folgende 3 Schritte:
1. Anwählen der Run Config `crowpi-examples [install]`
2. Starten mit Run Button
3. Wählen der Applikation auf der Kommandozeile (falls keine Argumente verwendet werden)

{{< img alt="Start der Applikation" src="setup/intellij-start-firstapplication.JPG" height="500px" >}}
{{< img alt="Run Output von IntelliJ" src="setup/intellij-run-example.JPG" height="500px" >}}
---

## Was passiert beim Start
Durch die Startkonfiguration und das Maven Projekt werden automatisch verschiedene Schritte beim Starten der Applikation ausgeführt. 
Grundsätzlich folgt der Ablauf immer diesem Schema: 
{{<mermaid align="middle">}}
graph TB;
A[Resolve Dependencies] --> B
B[Compile Java Code] --> C
C[Run Unit Tests] --> D
D[Copy Code to Raspberry Pi] --> E
E[Start Application]
{{< /mermaid >}}

Bei jedem Start werden also sogar die Unit tests ausgeführt. So kann eine optimale Codequalität beim Entwickeln erreicht werden.

---

## Start mit Argument
Der Start mit einem Argument soll es ermöglichen direkt eine der Applikationen auszuwählen, ohne die Nummer in der Kommandozeile eintippen 
zu müssen. Dazu muss die `Run Konfiguration` angepasst werden. Dies funktioniert sowohl für `crowpi-examples [install]` als auch für 
`crowpi-examples [debug]` genau gleich. 
- Das Kontextmenü bei der Run Konfiguration `Edit Configurations ...` öffnen.
- Nun bei der gewünschten Konfiguration den Tab `Runner` öffnen und mit dem `+` ein neues Argument hinzufügen. Gewählt werden muss das `crowpi.
laucher.args` und als Wert wird der exakte Name einer Applikation wie zum Beispiel `BuzzerApp` eingetragen.
- Normales Starten der Applikation mit dem Play Button
{{< img alt="Start mit Argument" src="basics/intellij-runconfig-args.JPG" height="500px" >}}

Jetzt startet der übliche Ablauf. Anstelle der Auswahl einer ExampleApp wird jedoch direkt die angegebene Applikation gestartet, also in diesem Fall die BuzzerApp.
