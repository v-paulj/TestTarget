---
author: mijacobs
Description: "Typografie muss übersichtlich sein, da sie zur visuellen Darstellung von Sprache dient. Ihr Stil darf diesem Ziel nie im Wege stehen. Typografie spielt jedoch auch als Layoutkomponente eine wichtige Rolle und wirkt sich maßgeblich auf die Dichte und Komplexität des Designs und damit auf die Benutzerfreundlichkeit dieses Designs aus."
title: Typografie
ms.assetid: ca35f78a-e4da-423d-9f5b-75896e0b8f82
label: Typography
template: detail.hbs
extraBodyClass: style-typography
brief: "As the visual representation of language, typography’s main task is to be clear. Its style should never get in the way of that goal. But typography also has an important role as a layout component—with a powerful effect on the density and complexity of the design—and on the user’s experience of that design."
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 02d5149f945bb631e385e45a295fdfe08bd98fb0

---

# Typografie für UWP-Apps

Typografie muss übersichtlich sein, da sie zur visuellen Darstellung von Sprache dient. Ihr Stil darf diesem Ziel nie im Wege stehen. Typografie spielt jedoch auch als Layoutkomponente eine wichtige Rolle und wirkt sich maßgeblich auf die Dichte und Komplexität des Designs und damit auf die Benutzerfreundlichkeit dieses Designs aus.

## Schriftart

Wir haben uns dafür entschieden, für alle digitalen Microsoft-Designs „Segoe UI“ zu verwenden. „Segoe UI“ bietet eine breite Palette von Zeichen und zeichnet sich durch optimale Lesbarkeit bei unterschiedlichsten Größen und Pixeldichten aus. Die klare, ansprechende und offene Ästhetik passt perfekt zum Inhalt des Systems.

![Beispieltext für die Schriftart „Segoe UI“](images/segoe-sample.png)

## Breiten

Bei der Typografie legen wir unter anderem Wert auf Schlichtheit und Effizienz. Wir verwenden eine einzelne Schriftart, eine geringe Anzahl von Schriftbreiten und -graden sowie eine klare Hierarchie. Positionierung und Ausrichtung basieren auf dem Standardstil für die jeweilige Sprache. Im Deutschen wird Text von links nach rechts und von oben nach unten verwendet. Beziehungen zwischen Text und Bildern sind klar und einfach.

![Unterstützte Schriftbreiten: „Light“, „Semilight“, „Regular“, „Semibold“ und „Bold“](images/weights.png)

## Zeilenabstand

![Beispiel für einen Zeilenabstand von 125 Prozent](images/line-spacing.png)

Der Zeilenabstand muss mit 125 Prozent des Schriftgrads berechnet und bei Bedarf auf das nächste Vielfache von Vier gerundet werden. Bei Segoe UI mit 15px wären 125 Prozent beispielsweise 18,75px. Wir empfehlen, den Wert aufzurunden und den Zeilenabstand auf 20px festzulegen, um dem 4px-Raster zu entsprechen. Dadurch wird eine gute Lesbarkeit gewährleistet, und es steht ausreichend Platz für diakritische Zeichen zur Verfügung. Spezifische Beispiele finden Sie weiter unten im Abschnitt „Typhierarchie“.

Wird sich größere Schrift über kleinerer Schrift befindet, muss der Abstand zwischen der letzten Grundlinie der größeren Schrift und der ersten Grundlinie der kleineren Schrift dem Zeilenabstand der größeren Schrift entsprechen.

![Große Schrift über kleinerer Schrift](images/line-height-stacking.png)

Im XAML-Code wird dies durch Stapeln zweier [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)-Elemente sowie durch Festlegen des entsprechenden Rands erreicht.

```xaml
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

<!-- OP version -->

## Kerning und Laufweite

Segoe ist eine humanistische Schriftart mit weicher, ansprechender Optik und organischen, offenen Formen, die von handschriftlichen Texten inspiriert sind. Um eine optimale Lesbarkeit zu gewährleisten und den humanistischen Charakter zu bewahren, müssen für Kerning und Laufweite bestimmte Werte verwendet werden.

Kerning muss auf „Metrik“ und die Laufweite auf „0“ festgelegt werden.

<img src="images/kerning-tracking.png" alt="Shows the difference between kerning and tracking" />

## Wort- und Zeichenabstand

Ähnlich wie bei Kerning und Laufweite werden auch beim Wort- und Zeichenabstand bestimmte Einstellungen verwendet, um eine optimale Lesbarkeit und die Wahrung des humanistischen Charakters zu gewährleisten.

Der Wortabstand beträgt standardmäßig immer 100 Prozent. Der Zeichenabstand muss auf „0“ festgelegt werden.

<img src="images/word-letter.png" alt="Shows the difference between word and letter spacing" />

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
Verwenden Sie in einem XAML-Textsteuerelement [Typogrphy.Kerning](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.documents.typography.kerning.aspx), um das Kerning zu steuern, und [FontStretch](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.control.fontstretch.aspx). um die Nachverfolgung zu steuern. Typography.Kerning ist standardmäßig auf „true“ und FontStretch ist standardmäßig auf „Normal“ festgelegt. Dies sind die empfohlenen Werte.
    </div>
</aside>


<!-- OP version -->
## Ausrichtung

Im Allgemeinen wird die linksbündige Ausrichtung visueller Elemente und Spalten mit Schrift empfohlen. In den meisten Fällen sorgt das Konzept „links bündig, rechts mit Flatterrand“ für eine konsistente Verankerung des Inhalts und für ein einheitliches Layout.

<img src="images/alignment.png" alt="Shows flush-left text" />

## Zeilenenden

Wenn Typografie nicht linksbündig mit rechtem Flatterrand positioniert wird, versuchen Sie, gleichmäßige Zeilenenden zu erreichen, und vermeiden Sie die Verwendung von Silbentrennung.

<img src="images/line-endings.png" alt="Shows even line endings" />

## Absätze

Absätze müssen als übersprungene Zeile ohne Einzug dargestellt werden, um aufeinander abgestimmte Spaltenränder zu erhalten.

![Vollständige Leerzeile zwischen Absätzen](images/paragraphs.png)

## Zeichenanzahl

Ist eine Zeile zu kurz, muss das Auge zu oft hin und her bewegt werden, was den Lesefluss stört. Verwenden Sie daher möglichst 50 bis 60 Wörter pro Zeile. Dies bietet den höchsten Lesekomfort.

Segoe bietet eine breite Palette von Zeichen und zeichnet sich durch optimale Lesbarkeit bei unterschiedlichen Größen und Pixeldichten aus. Eine optimale Anzahl von Buchstaben in einer Textspaltenzeile erhöht den Lesekomfort in einer Anwendung.

Zu lange Zeilen können für den Benutzer anstrengend und verwirrend sein. Bei zu kurzen Zeilen muss das Auge zu viel hin und her bewegt werden, was sich als ermüdend erweisen kann.

![Drei Absätze mit unterschiedlichen Zeilenlängen](images/character-count.png)

## Ausrichtung von hängendem Text

Die horizontale Ausrichtung von Symbolen mit Text kann je nach Symbolgröße und Textmenge auf unterschiedliche Arten gehandhabt werden. Wenn der Text (einzelne Zeile oder mehrere Zeilen) die Höhe des Symbols nicht übersteigt, muss der Text vertikal zentriert werden.

Sobald die Höhe des Texts die Höhe des Symbols übersteigt, muss die erste Textzeile vertikal ausgerichtet werden, und der weitere Text muss dem natürlichen Textfluss folgen. Für höhere Großbuchstaben bzw. Zeichen mit höherer Ober- und Unterlänge gelten die gleichen Ausrichtungsrichtlinien.

![Mehrere Kombinationen aus Symbol und Text](images/hanging-text-alignment.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
Die XAML-Eigenschaft [TextBlock.TextLineBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.textlinebounds.aspx) stellt den Zugriff auf die Höhe von Großbuchstaben und die Basisschriftartmetrik bereit. Sie kann verwendet werden, um visuell vertikal zu zentrieren oder oben ausgerichtet einzugeben.
    </div>
</aside>

## Beschnitt und Ellipsen

Nutzen Sie standardmäßig das Beschnittkonzept, und gehen Sie davon aus, dass der Text umgebrochen wird – es sei denn, es ist etwas anderes angegeben. Bei Verwendung von nicht umgebrochenem Text empfiehlt es sich, anstelle von Ellipsen den Beschnitt zu verwenden. Der Beschnitt kann am Rand des Containers, am Rand des Geräts, am Rand der Bildlaufleiste usw. erfolgen.

Ausnahme: Bei Containern, die nicht klar definiert sind (also sich etwa nicht durch eine andere Hintergrundfarbe abheben), kann eine Ellipse (...) verwendet werden.

![Gerät mit abgeschnittenem Text](images/clipping.png)

# Typhierarchie

Mithilfe unterschiedlicher Größen von „Segoe UI“ muss eine Typhierarchie erstellt werden. Diese Hierarchie bildet eine Struktur, die Benutzern die Navigation durch schriftliche Kommunikation erleichtert.

<figure class="figure-img" >
    <img src="images/type-ramp.png" alt="Shows the type ramp"  />
        <figcaption>Die Größe wird jeweils in effektiven Pixeln angegeben. Ausführlichere Informationen finden Sie unter „TODO: link“.</figcaption>
</figure>

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
Die meisten Ebenen der Typhierarchie sind in XAML als [statische Ressourcen](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_type_ramp) verfügbar, die der `*TextBlockStyle`-Benennungskonvention folgen (z. B.: `HeaderTextBlockStyle`). 
    </div>
</aside>


## Primärer und sekundärer Text

Zur Erweiterung der Typhierarchie kann die Deckkraft des sekundären Texts auf 60 Prozent festgelegt werden. In der [Farbschemapalette](color.md#color-themes) wird dafür „BaseMedium“ verwendet. Der primäre Text muss immer eine Deckkraft von 100 Prozent (BaseHigh) besitzen.


## Titel in Großbuchstaben

Bestimmte Seitentitel sollten in GROSSBUCHSTABEN angegeben werden, um die Hierarchie um eine weitere Dimension zu erweitern. Diese Titel müssen „BaseAlt“ und einen Zeichenabstand von 0,075 em verwenden. Dies kann auch bei der App-Navigation hilfreich sein.

In bestimmten Sprachen ändert sich durch eine Großschreibung jedoch die Bedeutung von Eigennamen. Daher dürfen auf Namen oder Benutzereingaben basierende Seitentitel *nicht* in Großbuchstaben umgewandelt werden.


## Empfohlene und nicht empfohlene Vorgehensweisen
* Verwenden Sie „Body“ für die meisten Texte.
* Verwenden Sie „Base“ für Titel mit begrenztem Platz.
* Integrieren Sie „SubtitleAlt“, um durch Hervorhebung des übergeordneten Inhalts einen Kontrast und eine Hierarchie zu schaffen.
* Verwenden Sie „Caption“ nicht für lange Zeichenfolgen oder Hauptaktionen.
* Verwenden Sie „Header“ oder „Subheader“ nicht, wenn der Text umgebrochen werden muss.
* Verwenden Sie „Subtitle“ und „SubtitleAlt“ nicht auf der gleichen Seite.

## Verwandte Artikel

* [Textsteuerelemente](../controls-and-patterns/text-controls.md)



<!--HONumber=Jun16_HO4-->


