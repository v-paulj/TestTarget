---
author: mijacobs
description: "Typografie muss übersichtlich sein, da sie zur visuellen Darstellung von Sprache dient. Ihr Stil darf diesem Ziel nie im Wege stehen. Typografie spielt jedoch auch als Layoutkomponente eine wichtige Rolle und wirkt sich maßgeblich auf die Dichte und Komplexität des Designs und damit auf die Benutzerfreundlichkeit des Designs aus."
title: Typografie
ms.assetid: ca35f78a-e4da-423d-9f5b-75896e0b8f82
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 8338b4ebcdd73f1b7ebf1dedafe68d861cd9d93b
ms.openlocfilehash: 481c66e3edd42722cfd59bf420fe5b6286706245

---

# Typografie

Typografie muss übersichtlich sein, da sie zur visuellen Darstellung von Sprache dient. Ihr Stil darf diesem Ziel nie im Wege stehen. Typografie spielt jedoch auch als Layoutkomponente eine wichtige Rolle und wirkt sich maßgeblich auf die Dichte und Komplexität des Designs und damit auf die Benutzerfreundlichkeit des Designs aus.

## Schriftart

Wir haben uns dafür entschieden, für alle digitalen Microsoft-Designs „SegoeUI“ zu verwenden. „SegoeUI“ bietet eine breite Palette von Zeichen und zeichnet sich durch optimale Lesbarkeit bei unterschiedlichsten Größen und Pixeldichten aus. Die klare, ansprechende und offene Ästhetik passt perfekt zum Inhalt des Systems.

![Beispieltext für die Schriftart „SegoeUI“](images/segoe-sample.png)

## Breiten

Bei der Typografie legen wir unter anderem Wert auf Schlichtheit und Effizienz. Wir verwenden eine einzelne Schriftart, eine geringe Anzahl von Schriftbreiten und -graden sowie eine klare Hierarchie. Positionierung und Ausrichtung basieren auf dem Standardstil für die jeweilige Sprache. Im Deutschen wird Text von links nach rechts und von oben nach unten verwendet. Beziehungen zwischen Text und Bildern sind klar und einfach.

![Unterstützte Schriftbreiten: „Light“, „Semilight“, „Regular“, „Semibold“ und „Bold“](images/weights.png)

## Zeilenabstand

![Beispiel für einen Zeilenabstand von 125Prozent](images/line-spacing.png)

Der Zeilenabstand muss mit 125Prozent des Schriftgrads berechnet und bei Bedarf auf das nächste Vielfache von Vier gerundet werden. Bei SegoeUI mit 15px wären 125Prozent beispielsweise 18,75px. Wir empfehlen, den Wert aufzurunden und den Zeilenabstand auf 20px festzulegen, um dem 4px-Raster zu entsprechen. Dadurch wird eine gute Lesbarkeit gewährleistet, und es steht ausreichend Platz für diakritische Zeichen zur Verfügung. Spezifische Beispiele finden Sie weiter unten im Abschnitt „Typhierarchie“.

Wird sich größere Schrift über kleinerer Schrift befindet, muss der Abstand zwischen der letzten Grundlinie der größeren Schrift und der ersten Grundlinie der kleineren Schrift dem Zeilenabstand der größeren Schrift entsprechen.

![Große Schrift über kleinerer Schrift](images/line-height-stacking.png)

Im XAML-Code wird dies durch Stapeln zweier [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)-Elemente sowie durch Festlegen des entsprechenden Rands erreicht.

```xml
<StackPanel Width="200">
    <!-- Setting a bottom margin of 3px on the header
         puts the baseline of the body text exactly 24px
         below the baseline of the header. 24px is the
         recommended line height for a 20px font size,
         which is what's set in SubtitleTextBlockStyle.
         The bottom margin will be different for
         different font size pairings. -->
    <TextBlock
        Style="{StaticResource SubtitleTextBlockStyle}"
        Margin="0,0,0,3"
        Text="Header text" />
    <TextBlock
        Style="{StaticResource BodyTextBlockStyle}"
        TextWrapping="Wrap"
        Text="This line of text should be positioned where the above header would have wrapped." />
</StackPanel>
```



## Kerning und Laufweite

Segoe ist eine humanistische Schriftart mit weicher, ansprechender Optik und organischen, offenen Formen, die von handschriftlichen Texten inspiriert sind. Um eine optimale Lesbarkeit zu gewährleisten und den humanistischen Charakter zu bewahren, müssen für Kerning und Laufweite bestimmte Werte verwendet werden.

Kerning muss auf „Metrik“ und die Laufweite auf„0“ festgelegt werden.


![Zeigt den Unterschied zwischen Kerning und Laufweite](images/kerning-tracking.png)



## Wort- und Zeichenabstand

Ähnlich wie bei Kerning und Laufweite werden auch beim Wort- und Zeichenabstand bestimmte Einstellungen verwendet, um eine optimale Lesbarkeit und die Wahrung des humanistischen Charakters zu gewährleisten.

Der Wortabstand beträgt standardmäßig immer 100Prozent. Der Zeichenabstand muss auf„0“ festgelegt werden.


![Zeigt den Unterschied zwischen Wort- und Zeichenabstand](images/word-letter.png)

**Hinweis**&nbsp;&nbsp;Verwenden Sie in einem XAML-Textsteuerelement [Typogrphy.Kerning](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.documents.typography.kerning.aspx) zur Steuerung des Kernings und [FontStretch](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.control.fontstretch.aspx) zur Steuerung der Laufweite. Typography.Kerning ist standardmäßig auf „true“ und FontStretch ist standardmäßig auf „Normal“ festgelegt. Dies sind die empfohlenen Werte.




## Ausrichtung

Im Allgemeinen wird die linksbündige Ausrichtung visueller Elemente und Spalten mit Schrift empfohlen. In den meisten Fällen sorgt das Konzept „links bündig, rechts mit Flatterrand“ für eine konsistente Verankerung des Inhalts und für ein einheitliches Layout.


![Zeigt linksbündigen Text](images/alignment.png)



## Zeilenenden

Wenn Typografie nicht linksbündig mit rechtem Flatterrand positioniert wird, versuchen Sie, gleichmäßige Zeilenenden zu erreichen, und vermeiden Sie die Verwendung von Silbentrennung.


![Zeigt gleichmäßige Zeilenenden](images/line-endings.png)

## Absätze

Absätze müssen als übersprungene Zeile ohne Einzug dargestellt werden, um aufeinander abgestimmte Spaltenränder zu erhalten.

![Vollständige Leerzeile zwischen Absätzen](images/paragraphs.png)

## Zeichenanzahl

Ist eine Zeile zu kurz, muss das Auge zu oft hin und her bewegt werden, was den Lesefluss stört. Verwenden Sie daher möglichst50 bis 60Wörter pro Zeile. Dies bietet den höchsten Lesekomfort.

Segoe bietet eine breite Palette von Zeichen und zeichnet sich durch optimale Lesbarkeit bei unterschiedlichen Größen und Pixeldichten aus. Eine optimale Anzahl von Buchstaben in einer Textspaltenzeile erhöht den Lesekomfort in einer Anwendung.

Zu lange Zeilen können für den Benutzer anstrengend und verwirrend sein. Bei zu kurzen Zeilen muss das Auge zu viel hin und her bewegt werden, was sich als ermüdend erweisen kann.

![Drei Absätze mit unterschiedlichen Zeilenlängen](images/character-count.png)

## Ausrichtung von hängendem Text

Die horizontale Ausrichtung von Symbolen mit Text kann je nach Symbolgröße und Textmenge auf unterschiedliche Arten gehandhabt werden. Wenn der Text (einzelne Zeile oder mehrere Zeilen) die Höhe des Symbols nicht übersteigt, muss der Text vertikal zentriert werden.

Sobald die Höhe des Texts die Höhe des Symbols übersteigt, muss die erste Textzeile vertikal ausgerichtet werden, und der weitere Text muss dem natürlichen Textfluss folgen. Für höhere Großbuchstaben bzw. Zeichen mit höherer Ober- und Unterlänge gelten die gleichen Ausrichtungsrichtlinien.

![Mehrere Kombinationen aus Symbol und Text](images/hanging-text-alignment.png)

**Hinweis**&nbsp;&nbsp;Die XAML-Eigenschaft [TextBlock.TextLineBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.textlinebounds.aspx) stellt den Zugriff auf die Höhe von Großbuchstaben und die Basisschriftartmetrik bereit. Sie kann verwendet werden, um visuell vertikal zu zentrieren oder oben ausgerichtet einzugeben.

## Beschnitt und Ellipsen

Nutzen Sie standardmäßig das Beschnittkonzept, und gehen Sie davon aus, dass der Text umgebrochen wird – es sei denn, es ist etwas anderes angegeben. Bei Verwendung von nicht umgebrochenem Text empfiehlt es sich, anstelle von Ellipsen den Beschnitt zu verwenden. Der Beschnitt kann am Rand des Containers, am Rand des Geräts, am Rand der Bildlaufleiste usw. erfolgen.

Ausnahme: Bei Containern, die nicht klar definiert sind (also sich etwa nicht durch eine andere Hintergrundfarbe abheben), kann eine Ellipse(...) verwendet werden.

![Gerät mit abgeschnittenem Text](images/clipping.png)

## Typhierarchie
Die Typhierarchie stellt eine wichtige gestalterische Beziehung zwischen Überschrift und Textkörper her und schafft damit eine klare und verständliche Hierarchie zwischen den verschiedenen Ebenen. Diese Hierarchie bildet eine Struktur, die Benutzern die Navigation durch schriftliche Kommunikation erleichtert.

![Zeigt die Typhierarchie](images/type-ramp.png) Die Größen sind in effektiven Pixeln angegeben. 


**Hinweis**&nbsp;&nbsp;Die meisten Ebenen der Typhierarchie sind in XAML als [statische Ressourcen](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_type_ramp) verfügbar, die der `*TextBlockStyle`-Namenskonvention folgen (z.B. `HeaderTextBlockStyle`).


## Primärer und sekundärer Text

Zur Erweiterung der Typhierarchie kann die Deckkraft des sekundären Texts auf 60Prozent festgelegt werden. In der [Farbschemapalette](color.md#color-themes) wird dafür „BaseMedium“ verwendet. Der primäre Text muss immer eine Deckkraft von 100Prozent (BaseHigh) besitzen.


## Titel in Großbuchstaben

Bestimmte Seitentitel sollten in GROSSBUCHSTABEN angegeben werden, um die Hierarchie um eine weitere Dimension zu erweitern. Diese Titel müssen „BaseAlt“ und einen Zeichenabstand von 0,075em verwenden. Dies kann auch bei der App-Navigation hilfreich sein.

In bestimmten Sprachen ändert sich durch eine Großschreibung jedoch die Bedeutung von Eigennamen. Daher dürfen auf Namen oder Benutzereingaben basierende Seitentitel *nicht* in Großbuchstaben umgewandelt werden.


**Empfohlen**



* Verwenden Sie „Body“ für die meisten Texte.
* Verwenden Sie „Base“ für Titel mit begrenztem Platz.
* Integrieren Sie „SubtitleAlt“, um durch Hervorhebung des übergeordneten Inhalts einen Kontrast und eine Hierarchie zu schaffen.



**Nicht empfohlen**



* Verwenden Sie „Caption“ nicht für lange Zeichenfolgen oder Hauptaktionen.
* Verwenden Sie „Header“ oder „Subheader“ nicht, wenn der Text umbrochen werden muss.
* Verwenden Sie „Subtitle“ und „SubtitleAlt“ nicht auf der gleichen Seite.



## Verwandte Artikel

* [Textsteuerelemente](../controls-and-patterns/text-controls.md)



<!--HONumber=Aug16_HO3-->


