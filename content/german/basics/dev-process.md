---
title: Entwicklungsprozess
weight: 50
---
Dieses Ablaufdiagramm zeigt, wie die Entwicklung einer neuen Applikation mit dem CrowPi Goes Java Framework ablaufen könnte.

---

{{<mermaid align="middle">}}
graph TB;
A[Idee] --> B
B[Benötigte Hardware] --> C{Neue Entwicklung oder mit Beispiel}
C --> | Beispiel erweitern | D[Passendes Example wählen] --> F
C --> | Neue Anwendung | E[App erstellen und registrieren] --> F
F[Lesen Dokumentation zu Hardware] --> G
G[Ablauf aufzeichnen] --> H 
H[App programmieren] --> I
I[Unit Tests ergänzen] --> H
I --> J[Testen der App]
J --> K[Dokumentation der App]
{{< /mermaid >}}
 
---

### Erklärung der einzelnen Schritte
#### Idee
Bevor auch nur im Ansatz an eine Entwicklung zu denken ist, muss eine Idee vorhanden sein. Der Kreativität sind keine Grenzen gesetzt. 
Jedoch sollten besonders Anfänger beachten, dass erste Anwendungen nicht zu kompliziert gestaltet werden. Umso wichtiger ist eine saubere Umsetzung.


#### Benötigte Hardware
Um sich einen Überblick zu verschaffen, sollte die Hardware, die zur Umsetzung der eigenen Idee benötigt wird, kurz umrissen werden.

#### Neue Entwicklung oder mit Beispiel
Wenn sich die Idee stark an einem der vorgegebenen Beispiele orientiert, ist es womöglich einfacher und schneller das Beispiel auszubauen 
und anzupassen. Vielleicht möchte man aber auch gerne auf der grünen Wiese mit dem Bauen starten. Dann wählt man eher die Variante einer 
komplett neuen Applikation im Rahmen von CrowPi Goes Java.

#### Passendes Example Auswählen
Wenn die Entscheidung gefällt wurde, auf einem Beispiel aufzubauen, wählt man das entsprechende Beispiel aus. Dies erspart uns die 
Schritte, welche bei einer neuen Applikation nötig wären.

#### App erstellen und registrieren
Gemäss Anleitung im [Application Framework]({{< ref "basics/framework" >}}) wird eine neue Applikation erstellt.

#### Lesen der Dokumentation zur Hardware
Um genauer zu erfahren, wie benötigte Hardware funktioniert, lohnt es sich die entsprechenden Kapitel hier im Tutorial zu betrachten.
So erfährt man welche Funktionen bereits zur Verfügung stehen und wo allenfalls noch Lücken vorhanden sind. Auch versteht man die 
physikalischen Vorgänge im Hintergrund etwas besser.

#### Ablauf aufzeichnen
Programmieren ohne Plan endet meist in einem unstrukturierten, chaotischen Code. Es ist also sinnvoll sich erst einen Schlachtplan zurechtzulegen. Sehr effizient und hilfreich sind dabei insbesondere Ablauf- oder Statusdiagramme.

#### App Programmieren
Entspricht der Abbildung der benötigten Funktion in Form von Code.

#### Unit Tests ergänzen
Eine gute Applikation wird immer automatisch getestet. Unit Tests stellen dabei eine einfache aber effektive Möglichkeit dar. Es 
besteht sogar das Konzept des Test-Code-Refactor, wo vor dem eigentlichen Code erst die Tests programmiert werden. So kann auch bei 
nachträglichen Änderungen sichergestellt werden, dass die ursprünglichen Funktionen erhalten bleiben.

#### Testen der App
Obwohl die Testabdeckung mit Unit Tests sehr zur Qualität beiträgt, handelt es sich bei CrowPi um ein Gerät mit Hardwarekomponenten. Das 
rein virtuelle Testen wird also nie jeden Fall abdecken können. Ein gründliches Testen mit der Hardware durch einen oder mehrere 
Benutzer ist immer von Vorteil.

#### Dokumentation der App
Zur späteren Nachvollziehbarkeit des Codes hilft eine kurzgehaltene aber vollständige Dokumentation der erstellen Applikation enorm.
