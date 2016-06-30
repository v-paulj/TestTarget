---
author: Jwmsft
Description: "Ermöglicht dem Benutzer das Festlegen eines Werts in einem angegebenen Bereich."
title: Schieberegler
ms.assetid: 7EC7EA33-BE7E-4FD5-B205-B8FA7B729ACC
label: Sliders
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: 64ae11c8c7e46d973c13cacb69482fd957e0488d

---
# Schieberegler

Ein Schieberegler ist ein Steuerelement, über das der Benutzer aus einer Reihe von Werten auswählen kann, indem er ein Schiebereglersteuerelement über einen Bereich verschiebt.

![Schiebereglersteuerelement](images/controls/slider.png)

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**Slider-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx)
-   [**Value-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx)
-   [**ValueChanged-Ereignis**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx)

## Ist dies das richtige Steuerelement?

Verwenden Sie einen Schieberegler, wenn Sie Benutzern ermöglichen möchten, definierte, zusammenhängende Werte (z. B. Lautstärke oder Helligkeit) oder einen Bereich von separaten Werten (z. B. Einstellungen für die Bildschirmauflösung) festzulegen.

Eine Schieberegler ist eine gute Wahl, wenn Sie wissen, dass Benutzer den Wert als relative Anzahl betrachten und nicht als numerischen Wert. So denken Benutzer beispielsweise beim Festlegen der Audiolautstärke an "niedrig" oder "mittel", aber nicht an Werte wie "2" oder "5".

Verwenden Sie einen Schieberegler nicht für binäre Einstellungen. Verwenden Sie stattdessen einen [Umschalter](toggles.md).

Hier sind einige weitere Faktoren, die Sie bei der Entscheidung für oder gegen die Verwendung eines Schiebereglers berücksichtigen sollten:

-   **Handelt es sich bei der Einstellung um eine relative Anzahl?** Ist dies nicht der Fall, verwenden Sie stattdessen [Optionsfelder](radio-button.md) oder ein [Listenfeld](lists.md).
-   **Handelt es sich bei der Einstellung um einen exakten, bekannten numerischen Wert?** Wenn ja, verwenden Sie ein numerisches [Textfeld](text-box.md).
-   **Wäre es für Benutzer hilfreich, sofort Feedback zur Auswirkung von Einstellungsänderungen zu erhalten?** Wenn ja, verwenden Sie einen Schieberegler. Beispielsweise können Benutzer eine Farbe leichter auswählen, wenn sie sofort sehen, wie sich Änderungen an den Werten für Farbton, Sättigung oder Leuchtdichte auswirken.
-   **Weist die Einstellung einen Bereich aus vier oder mehr Werten auf?** Ist dies nicht der Fall, verwenden Sie stattdessen [Optionsfelder](radio-button.md).
-   **Können Benutzer den Wert ändern?** Schieberegler dienen zur Benutzerinteraktion. Wenn der Wert in keinem Fall von Benutzern geändert werden kann, verwenden Sie stattdessen schreibgeschützten Text.

Wenn Sie zwischen einem Schieberegler und einem numerischen Textfeld entscheiden, verwenden Sie in folgenden Fällen ein numerisches Textfeld:

-   Der auf dem Bildschirm verfügbare Platz ist knapp.
-   Benutzer möchten wahrscheinlich lieber die Tastatur verwenden.

Verwenden Sie in folgenden Fällen einen Schieberegler:

-   Benutzer profitieren von sofortigem Feedback.

## Beispiele

Ein Schieberegler zum Steuern der Lautstärke auf Windows Phone.

![Ein Schieberegler zum Steuern der Lautstärke auf Windows Phone](images/control-examples/slider-phone.png)

Ein Schieberegler zum Ändern der Textgröße in den Windows-Anzeigeeinstellungen.

![Ein Schieberegler zum Ändern der Textgröße in den Windows-Anzeigeeinstellungen](images/control-examples/slider-display-settings.png)

## Erstellen Sie einen Schieberegler

So erstellen Sie einen Schieberegler in XAML.

```xaml
<Slider x:Name="volumeSlider" Header="Volume" Width="200"
        ValueChanged="Slider_ValueChanged"/>
```

So erstellen Sie einen Schieberegler im Code.

```csharp
Slider volumeSlider = new Slider();
volumeSlider.Header = "Volume";
volumeSlider.Width = 200;
volumeSlider.ValueChanged += Slider_ValueChanged;

// Add the slider to a parent container in the visual tree.
stackPanel1.Children.Add(volumeSlider);
```

Den Wert des Schiebereglers können Sie aus der [**Value**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx)-Eigenschaft abrufen und festlegen. Zum Reagieren auf Werteänderungen können Sie die Value-Eigenschaft mithilfe der Datenbindung binden oder das [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx)-Ereignis behandeln.

```csharp
private void Slider_ValueChanged(object sender, RangeBaseValueChangedEventArgs e)
{
    Slider slider = sender as Slider;
    if (slider != null)
    {
        media.Volume = slider.Value;
    }
}
```

## Empfehlungen

-   Passen Sie das Steuerelement an, sodass Benutzer den Wert einfach anpassen können. Stellen Sie für Einstellungen mit separaten Werten sicher, dass der Benutzer jeden Wert einfach mithilfe der Maus auswählen kann. Stellen Sie sicher, dass die Endpunkte des Schiebereglers immer in die Bereiche einer Ansicht passen.
-   Geben Sie während oder nach der Benutzerauswahl ein sofortiges Feedback (sofern umsetzbar). Beispielsweise gibt die Windows-Lautstärkeregelung einen Signalton aus, um die ausgewählte Audiolautstärke anzuzeigen.
-   Verwenden Sie Bezeichnungen, um den Wertebereich anzuzeigen. Eine Ausnahme liegt vor, wenn der Schieberegler vertikal ausgerichtet ist und die obere Beschriftung "Maximum", "Hoch", "Mehr" oder ähnlich lautet, ist die Bedeutung klar, und Sie können die anderen Beschriftungen weglassen.
-   Deaktivieren Sie alle zugeordneten Beschriftungen und das visuelle Feedback, wenn Sie den Schieberegler deaktivieren.
-   Bedenken Sie beim Festlegen der Flussrichtung bzw. Ausrichtung Ihres Schiebereglers die Textrichtung. In einigen Sprachen fließt das Skript von links nach rechts, und in anderen von rechts nach links.
-   Verwenden Sie einen Schieberegler nicht als Statusanzeige.
-   Legen Sie für die Miniaturansicht des Schiebereglers keine andere Größe als die Standardgröße fest.
-   Erstellen Sie keinen fortlaufenden Schieberegler, wenn der Wertebereich groß ist und die Benutzer mit hoher Wahrscheinlichkeit einen von mehreren repräsentativen Werten aus dem Bereich auswählen. Verwenden Sie diese Werte stattdessen als einzige zulässige Schritte. Wenn der Höchstwert für einen Zeitwert beispielsweise 1 Monat ist, die Benutzer aber nur zwischen 1 Minute, 1 Stunde, 1 Tag oder 1 Monat auswählen sollen, erstellen Sie einen Schieberegler mit 4 Schrittpunkten.

## Weitere Hinweise zur Verwendung

### Auswählen des richtigen Layouts: horizontal oder vertikal

Sie können den Schieberegler horizontal oder vertikal ausrichten. Bestimmen Sie anhand dieser Richtlinien das geeignete Layout.

-   Verwenden Sie eine natürliche Ausrichtung. Wenn der Schieberegler beispielsweise einen echten Wert darstellt, der normalerweise vertikal angezeigt wird (z. B. eine Temperatur), verwenden Sie die vertikale Ausrichtung.
-   Wenn das Steuerelement für die Suche in Medien verwendet wird, beispielsweise in einer Video-App, verwenden Sie die horizontale Ausrichtung.
-   Wenn Sie einen Schieberegler auf einer Seite verwenden, die in eine Richtung geschwenkt werden kann (horizontal oder vertikal), verwenden Sie für den Schieberegler eine andere Ausrichtung als die Schwenkrichtung. Anderenfalls streifen Benutzer möglicherweise beim Schwenken der Seite den Schieberegler und ändern versehentlich den Wert.
-   Wenn Sie noch nicht sicher sind, welche Ausrichtung Sie verwenden sollen, nehmen Sie die, die am besten zum Seitenlayout passt.

### Bereichsrichtung

Die Bereichsrichtung ist die Richtung, in der Sie den Schieberegler bewegen, wenn Sie ihn vom aktuellen Wert zu seinem maximalen Wert verschieben.

-   Platzieren Sie bei einem vertikalen Schieberegler den höchsten Wert am oberen Ende des Schiebereglers, unabhängig von der Leserichtung. Platzieren Sie beispielsweise bei einem Schieberegler für die Lautstärke immer die Einstellung für die maximale Lautstärke am oberen Ende des Schiebereglers. Bei anderen Arten von Werten (beispielsweise bei Wochentagen) orientieren Sie sich an der Leserichtung der Seite.
-   Platzieren Sie bei horizontalen Stilen den niedrigeren Wert auf der linken Seite des Schiebereglers (bei einem Seitenlayout von links nach rechts ) bzw. auf der rechten Seite (bei einem Seitenlayout von rechts nach links).
-   Die einzige Ausnahme für die oben genannte Richtlinie sind Mediensuchleisten: Platzieren Sie immer den niedrigeren Wert auf der linken Seite des Schiebereglers.

### Schritte und Teilstriche

-   Verwenden Sie Schrittpunkte, wenn der Schieberegler keine beliebigen Werte zwischen minimalen und maximalen Werten zulassen soll. Wenn Sie beispielsweise einen Schieberegler verwenden, um die Anzahl der zu kaufenden Kinotickets anzugeben, lassen Sie keine Gleitkommawerte zu. Verwenden Sie den Schrittwert "1".
-   Wenn Sie Schritte (auch als Andockpunkte bezeichnet) angeben, stellen Sie sicher, dass der letzte Schritt am Maximalwert des Schiebereglers ausgerichtet ist.
-   Verwenden Sie Teilstriche, wenn Sie Benutzern die Position wichtiger Werte zeigen möchten. So kann beispielsweise ein Schieberegler für die Zoomsteuerung Teilstriche für 50 %, 100 % und 200 % haben.
-   Zeigen Sie Teilstriche an, wenn Benutzer den ungefähren Wert der Einstellung wissen müssen.
-   Zeigen Sie Teilstriche und eine Wertbeschriftung an, wenn die Benutzer den genauen Wert der ausgewählten Einstellung erfahren sollen, ohne mit dem Steuerelement zu interagieren. Andernfalls können Sie die QuickInfo für den Wert verwenden, um den genauen Wert anzuzeigen.
-   Zeigen Sie immer Teilstriche an, wenn Schrittpunkte nicht offensichtlich sind. Wenn der Schieberegler beispielsweise 200 Pixel breit ist und 200 Andockpunkte hat, können Sie die Teilstriche ausblenden, da die Benutzer das Andockverhalten nicht bemerken. Wenn aber nur zehn Andockpunkte vorhanden sind, zeigen Sie Teilstriche an.

### Beschriftungen

-   **Schiebereglerbeschriftungen**

    Aus der Schiebereglerbeschriftung geht hervor, wofür der Schieberegler verwendet wird.

    -   Verwenden Sie eine Beschriftung ohne Interpunktion am Ende (diese Konvention gilt für alle Beschriftungen für Steuerelemente).
    -   Platzieren Sie Beschriftungen über dem Schieberegler, wenn er sich in einem Formular befindet, in dem die meisten Beschriftungen über den Steuerelementen positioniert sind.
    -   Platzieren Sie Beschriftungen an der Seite, wenn sich der Schieberegler in einer Umgebung befindet, in der die meisten Beschriftungen neben den Steuerelementen positioniert sind.
    -   Platzieren Sie Beschriftungen nach Möglichkeit nicht unter dem Schieberegler, da der Benutzer die Beschriftung mit dem Finger verdecken könnte, wenn er den Schieberegler berührt.
-   **Bereichsbeschriftungen**

    Die Bereichs- oder Füllungsbeschriftungen beschreiben den minimalen und den maximalen Wert des Schiebereglers.

    -   Beschriften Sie die beiden Enden des Schiebereglerbereichs, sofern dies nicht aufgrund einer vertikalen Ausrichtung unnötig ist.
    -   Verwenden Sie nach Möglichkeit für jede Beschriftung nur ein Wort.
    -   Verwenden Sie keine Interpunktion am Ende.
    -   Stellen Sie sicher, dass die Beschriftungen aussagekräftig und parallel sind. Beispiele: "Maximum/Minimum", "Mehr/Weniger", "Niedrig/Hoch", "Leise/Laut".
-   **Wertbeschriftungen**

    Eine Wertbeschriftung zeigt den aktuellen Wert des Schiebereglers an.

    -   Wenn Sie eine Wertbeschriftung benötigen, zeigen Sie sie unter dem Schieberegler an.
    -   Zentrieren Sie den Text relativ zum Steuerelement, und schließen Sie die Einheiten ein (beispielsweise Pixel).
    -   Da der Schiebereglerziehpunkt beim Streichen verdeckt ist, sollten Sie in Erwägung ziehen, den aktuellen Wert auf andere Art und Weise anzuzeigen, und zwar mit einer Beschriftung oder einem anderen visuellen Effekt. Eine Schieberegler-Einstellungstextgröße könnte einigen Beispieltext auf die richtige Größe neben dem Schieberegler rendern.

### Erscheinungsbild und Interaktion

Eine Schieberegler besteht aus einer Spur und einem Ziehpunkt. Bei der Spur handelt es sich um eine Leiste (welche optional verschiedene Teilstrichstile anzeigen kann), die den Bereich der Werte darstellt, die eingegeben werden können. Der Ziehpunkt ist ein Wahlschalter, den der Benutzer positionieren kann, indem er entweder auf die Spur tippt oder zurück oder noch vorn streicht.

Ein Schieberegler weist ein großes Berührungsziel auf. Ein Schieberegler sollte möglichst weit genug vom Rand des Displays positioniert werden, um eine zugängliche Berührung zu gewährleisten.

Ziehen Sie beim Entwerfen eines benutzerdefinierten Schiebereglers Möglichkeiten zum übersichtlichen Darstellen aller für den Benutzer erforderlichen Informationen in Betracht. Verwenden Sie eine Wertbeschriftung, wenn der Benutzer die Einheiten verstehen muss, um die Einstellung nachvollziehen zu können. Suchen Sie nach kreativen Möglichkeiten, um diese Werte grafisch darzustellen. Beispielsweise könnte ein Schieberegler, über den die Lautstärke geregelt wird, eine Lautsprechergrafik anzeigen, und zwar ohne Schallwellen am minimalen Ende des Schiebereglers und einer Lautsprechergrafik mit Schallwellen am maximalen Ende.

## Verwandte Themen

**Für Designer**
- [Richtlinien für Umschalter](toggles.md)
            
          
            **Für Entwickler (XAML)**
- [**Slider-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209614)



<!--HONumber=Jun16_HO4-->


