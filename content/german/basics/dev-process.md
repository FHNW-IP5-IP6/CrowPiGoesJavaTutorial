---
title: Entwicklungsprozess
weight: 50
---
Dieses Ablaufdiagramm zeigt wie eine Entwicklung einer neuen Applikation mit dem CrowPi Goes Java Framework ablaufen könnte.

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
Bevor eine Entwicklung nur ansatzweise denkbar ist, muss eine Idee vorhanden sein. Hier kann man seine Kreativität vollends ausleben. 
Jedoch sollten gerade Anfänger beachten, dass erste Anwendungen nicht zu kompliziert sind. Dafür sollte die Umsetzung sauber erfolgen 
und nicht in einem Chaos enden.

#### Benötigte Hardware
Um sich einen Überblick zu verschaffen, sollte die Hardware welche zur Umsetzung der eigenen Idee nötig ist, kurz zusammengefasst werden.

#### Neue Entwicklung oder mit Beispiel
Wenn sich die Idee stark an einem der vorgegebenen Beispiele orientiert, ist es vielleicht einfacher und schneller das Beispiel auszubauen 
und anzupassen. Vielleicht möchte man aber auch gerne auf der grünen Wiese mit dem Bauen starten. Dann nimmt man eher die Variante einer 
komplett neuen Applikation im Rahmen von CrowPi Goes Java.

#### Passendes Example Auswählen
Wenn die Entscheidung gefällt wurde, auf einem Beispiel aufzubauen. Wählt man das entsprechende Beispiel aus. Dies erspart und die 
Schritte welche bei einer neuen Applikation nötig wären.

#### App erstellen und registrieren
Gemäss Anleitung im [Application Framework]({{< ref "basics/framework" >}}) wir eine neue Applikation erstellt.

#### Lesen der Dokumentation zur Hardware
Um genauer zu erfahren, wie benötigte Hardware funktioniert, lohnt es sich die entsprechenden Kapitel hier im Tutorial zu betrachten.
So erfährt man welche Funktionen bereits zur Verfügung stehen und wo allenfalls noch Lücken vorhanden sind. Auch versteht man die 
physikalischen Vorgänge im Hintergrund etwas besser.

#### Ablauf aufzeichnen
Programmieren ohne Plan endet meist in unstrukturiertem, chaotischem Code. Es ist also sinnvoll sich erst einen Schlachtplan zurechtzulegen. Sehr effizient und hilfreich sind dabei insbesondere Ablauf oder Statusdiagramme.

#### App Programmieren
Entspricht der Abbildung der benötigten Funktion in Form von Code.

#### Unit Tests ergänzen
Eine gute Applikation wird immer automatisch getestet. Unit Tests stellen dabei eine einfache aber effektive Variante zur Verfügung. Es 
besteht sogar das Konzept des Test-Code-Refactor, wo vor dem eigentlichen Code erst die Tests programmiert werden. So kann auch bei 
nachträglichen Änderungen sichergestellt werden, dass die ursprünglichen Funktionen erhalten bleiben.

#### Testen der App
Obwohl die Testabdeckung mit Unit Tests sehr zur Qualität beiträgt, handelt es sich bei CrowPi um ein Gerät mit Hardwarekomponenten. Ein 
rein virtuelles Testen wird also nie jeden Fall abdecken können. Ein gründliches Testen mit der Hardware durch einen oder mehrere 
Benutzer ist immer von Vorteil.

#### Dokumentation der App
Zur späteren Nachvollziehbarkeit des Codes hilft eine kurz gehaltene aber vollständige Dokumentation der erstellen Applikation enorm.
