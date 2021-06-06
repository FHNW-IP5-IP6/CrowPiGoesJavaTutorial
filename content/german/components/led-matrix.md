---
title: LED Matrix
tags: ["spi"]
---

## Funktionsweise

Bei der LED Matrix handelt es sich um eine Anzeige, die aus einem Gitter von LEDs besteht. Im konkreten Fall vom CrowPi steht eine
quadratische 8×8 LED Matrix zur Verfügung, sodass insgesamt 64 individuelle LEDs gesteuert werden können. Damit können neben der
statischen Anzeige von Buchstaben, Zahlen und Symbolen auch einfache Animationen realisiert werden.

Zur Ansteuerung der einzelnen LEDs wären individuell steuerbare Leitungen unpraktisch, weshalb der elektronische Baustein `MAX7219`
verwendet wird. Dieser ermöglicht die Steuerung der gesamten LED Matrix über die [SPI-Schnittstelle]({{< ref "hardware/spi" >}}), sodass
lediglich drei Pins benötigt werden.

Da das Übertragen jeder einzelnen Änderung an die Komponente sehr ineffizient ist, wird üblicherweise ein Buffer in der Software
implementiert. Alle gewünschten Anpassungen verändern somit nur den Buffer, der anschliessend mit nur einem einzigen Befehl übertragen
werden kann. Dieses Prinzip wird auch von der Komponenten-Klasse `LedMatrixComponent` verwendet.

## Voraussetzungen

### DIP Switches

Für diese Komponente werden keine spezifischen DIP-Switches benötigt, sodass diese in der Standardkonfiguration belassen werden können:

{{< dip-switches >}}

## Verwendung

Nachfolgend wird die Verwendung der Klasse {{< javadoc class="com.pi4j.crowpi.components.LedMatrixComponent" >}} beschrieben. Es handelt sich
hierbei um eine sehr mächtige und komplexe Klasse, da eine Vielzahl von Funktionalitäten angeboten wird. Ein Blick auf das Beispiel zeigt
jedoch, dass sich diese weiterhin sehr einfach anwenden lässt.

Es ist hierbei wichtig zu beachten, dass die beiden Methoden `setPixel` und `getPixel`, welche auch mit dem Wort "Buffer:" am Anfang
gekennzeichnet wurden, nicht direkt eine Auswirkung auf die Anzeige haben, sondern erst nach Aufruf von `void refresh()` aktiv werden. Somit
lassen sich viele einzelne Pixel setzen, welche dann mit einem einzelnen Aufruf an die Anzeige übermittelt werden. Alle anderen Befehle
werden jeweils sofort ausgeführt, da diese immer die ganze Anzeige betreffen.

### Konstruktoren

| Konstruktor | Bemerkung |
| --- | --- |
| `LedMatrixComponent(com.pi4j.context.Context pi4j)` | Initialisiert eine LED Matrix mit der Standard SPI Adresse und Bandbreite für den CrowPi. |
| `LedMatrixComponent(com.pi4j.context.Context pi4j, int channel, int baud)` | Initialisiert eine LED Matrix mit einer benutzerdefinierten SPI Adresse und Bandbreite. |

### Methoden

| Methode | Bemerkung |
| --- | --- |
| `void setEnabled(boolean enabled)` | Schaltet die Anzeige ein oder aus gemäss Boolean-Wert. |
| `void setTestMode(boolean enabled)` | Schaltet den Testmodus der Anzeige ein oder aus. Ist dieser eingeschaltet, leuchten alle LEDs bedingungslos auf und alle anderen Operationen werden **nicht** ausgeführt. |
| `void setBrightness(int brightness)` | Setzt die gewünschte Helligkeit der ganzen Anzeige zwischen 0 (am dunkelsten) und 15 (am hellsten). |
| `void setPixel(int x, int y, boolean enabled)` | *Buffer:* Schaltet einen einzelnen Pixel an der gewünschten Stelle ein (`true`) oder aus (`false`). |
| `boolean getPixel(int x, int y)` | *Buffer:* Gibt den aktuellen Zustand eines einzelnen Pixels an der gewünschten Stelle an. |
| `void clear()` | Löscht den internen Buffer ohne die Anzeige zu aktualisieren. |
| `void refresh()` | Aktualisiert die Anzeige mit dem aktuellen Inhalt des Buffers. |
| `void scroll(Direction direction)` | Scrollt die Anzeige und lässt die hierbei entstehende Lücke leer. |
| `void rotate(Direction direction)` | Scrollt die Anzeige und verschiebt die "herausgefallene" Zeile / Spalte ans andere Ende. |
| `void print(String string)` | Gibt den String auf der Anzeige mit einem Scroll-Effekt von rechts nach links aus. Die Anzeige wird hierbei vor und nach der Ausgabe automatisch gelöscht. Mit dem Muster `{SYMBOL-NAME}` innerhalb der Zeichenkette können Symbole in den Text eingebunden werden. |
| `void print(String string, Direction scrollDirection)` | Gleiche Funktion wie `print(String)`, jedoch mit einer benutzerdefinierten Richtung. |
| `void print(String string, Direction scrollDirection, long scrollDelay)` | Gleiche Funktion wie void `print(String, Direction)`, jedoch mit benutzerdefinierten zeitlichen Abständen in Millisekunden zwischen dem Scrollen. |
| `void print(char c)` | Gibt das gewünschte Zeichen sofort auf der Anzeige aus. |
| `void print(Symbol s)` | Gibt das gewünschte Symbol sofort auf der Anzeige aus. |
| `void transition(Symbol symbol)` | Schiebt das Symbol in die Anzeige von rechts nach links rein. |
| `void transition(Symbol symbol, Direction scrollDirection)` | Gleiche Funktion wie `transition(Symbol)`, jedoch mit einer benutzerdefinierten Richtung. |
| `void transition(Symbol symbol, Direction scrollDirection, long scrollDelay)` | Gleiche Funktion wie `transition(Symbol)`, jedoch mit benutzerdefinierten zeitlichen Abständen in Millisekunden zwischen dem Scrollen. |
| `void draw(Consumer<Graphics2D> drawer)` | Ruft die übergebene Lambda-Funktion mit einem `Graphics2D` Kontext auf, so dass ein Bild gezeichnet werden kann. Dieses wird anschliessend sofort auf der Anzeige dargestellt. |
| `void draw(BufferedImage image)` | Gibt das übergebene `BufferedImage` auf der Anzeige aus. Dieses muss den Typ `TYPE_BYTE_BINARY` (1bit Farbtiefe = nur schwarz/weiss) sowie eine Grösse von 8x8 Pixeln aufweisen. |
| `void draw(BufferedImage image, int x, int y)` | Gleiche Funktion wie `BufferedImage`, jedoch kann hier auch ein grösseres Bild angegeben werden, da automatisch ein Ausschnitt von 8x8 Pixeln ab der genannten X/Y Position erzeugt wird. |

### Enumerationen

- {{< javadoc class="com.pi4j.crowpi.components.LedMatrixComponent" subclass="Symbol" >}} enthält alle unterstützten Symbole welche von
  dieser Komponenten-Klasse dargestellt werden können. Es handelt sich hierbei um alle ASCII-Codes von 32 bis 127 sowie diversen Symbolen
  wie beispielsweise `HEART` für ein Herz. Die Werte sind jeweils als `byte[]` Array hinterlegt, was der internen Darstellung der LED Matrix
  Anzeige entspricht. Jedes Element ist eine Zeile und die Bits `0-7` entsprechen den jeweiligen Spalten in umgekehrter Reihenfolge, sprich
  das Bit `7` stellt die Spalte an der X-Position `0` dar.

## Beispielapplikation

Die nachfolgende Beispielapplikation zeigt nacheinander 4 verschiedene Beispiele für die LED Matrix, um dessen Vielfältigkeit und die
verschiedenen zur Verfügung gestellten Funktionen dieser Bibliothek zu demonstrieren. Zuerst wird die Komponente initialisiert, mit der
Funktion `setEnabled` aktiviert und eine mittlere Helligkeit mit `setBrightness` konfiguriert.

Das erste Beispiel zeigt nun die Verwendung von `draw(Consumer<Graphics2D>)` mit einer anonymen Lambda-Funktion. Dies klingt zuerst
kompliziert, jedoch ist dies nichts anderes als die Möglichkeit eine eigene Funktion zu übergeben, um darin beliebige Zeichenoperationen
durchzuführen. Der Parameter `graphics` entspricht dabei der Standard-Klasse `java.awt.Graphics2D`, wofür sich
[im Internet](https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/java/awt/Graphics2D.html) weitere Dokumentation finden lässt.
Hier wird nun ein Kreuz gezeichnet (zuerst eine Linie von oben-links nach unten-rechts, dann von oben-rechts nach unten-links) und
anschliessend ein Kreis darüber. Das Resultat wird dann auf der LED Matrix dargestellt.

Das zweite Beispiel zeigt nach einer kurzen Pause eine Schleife über eine Liste von vordefinierten Symbolen, in diesem Fall vier
verschiedenen Smileys. Diese werden jeweils mit einer kurzen Pause nacheinander dargestellt und demonstrieren somit einige der
vordefinierten Symbole. In der Enumeration `LedMatrixComponent.Symbol` lassen sich noch weitere Symbole zur freien Verwendung finden.
Konkret wird mit der `for`-Schleife über das Array von Symbolen iteriert und jedes wird nacheinander mit `print` sofort angezeigt.

Das dritte Beispiel zeigt ebenfalls vier verschiedene Symbole, dieses Mal jedoch unter Verwendung der `transition` Methode. Diese ersetzt
den Inhalt der LED Matrix nicht sofort, sondern schiebt das neue Symbol in die Anzeige rein mit der gewünschten Richtung. Wird also zum
Beispiel `Direction.UP` gewählt, so wird das neue Symbol von unten nach oben in die Anzeige reingeschoben.

Das vierte Beispiel gibt schliesslich einen längeren Text aus. Die Methode `print` stellt diesen automatisch so dar, dass nacheinander alle
Zeichen auf der Anzeige mit Scroll-Effekt dargestellt werden. Intern wird hierfür ebenfalls die `transition` Methode vom vorherigen Beispiel
genutzt. Um die `print` Methode zusammen mit Symbolen zu nutzen, können beliebig viele Symbole mit dem Muster `{NAME}`
eingebunden werden, sprich `{HEART}` wird durch das eigentliche `HEART` Zeichen von `LedMatrixComponent.Symbol` ersetzt.

{{< code file="src/main/java/com/pi4j/crowpi/applications/LedMatrixApp.java" language="java" >}}

## Weitere Möglichkeiten

- Statt nur einmal eine eigene Grafik mit `draw(Consumer<Graphics2D>)` zu zeichnen, könnten in einer Schleife verschiedene Grafiken
  gezeichnet werden, um eine kleine Animation zu realisieren.

- Das Beispiel könnte so verändert werden, dass auf der Standardeingabe ein Text entgegengenommen wird, welcher anschliessend mit
  `print(String)` auf der Anzeige ausgegeben wird.

- In Kombination mit einem nicht-binären Sensor wie der Distanzanzeige könnte auf der Anzeige ein Graph dargestellt werden. Hierfür könnte
  eine Kombination aus `rotate` und `setPixel` verwendet werden, um nacheinander den alten Messwert auf die Seite zu schieben und den neuen
  Messwert einzuzeichnen.
