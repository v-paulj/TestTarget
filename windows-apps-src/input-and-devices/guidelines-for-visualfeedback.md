---
author: Karl-Bridge-Microsoft
Description: "Zeigen Sie Benutzern mit visuellem Feedback, wenn ihre Interaktionen mit einer Windows Store-App ermittelt, interpretiert und behandelt werden."
title: Visuelles Feedback
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 2bf873f35192c20f15c6cb445b6be6436354c8c2

---

# Richtlinien für visuelles Feedback

Zeigen Sie Benutzern durch visuelles Feedback, wenn ihre Interaktionen ermittelt, interpretiert und behandelt werden. Visuelles Feedback ist hilfreich für Benutzer und kann sie zur Interaktion ermutigen. Es weist auf erfolgreiche Interaktionen hin, was für den Benutzer das Gefühl der Kontrolle verstärkt. Darüber hinaus informiert es über den Systemstatus und verringert die Fehlerzahl.

**Wichtige APIs**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)

## Empfehlungen

-   Versuchen Sie, so eng wie möglich an der ursprünglichen Steuerelementvorlage zu bleiben, um eine optimale Steuerung und Leistung der Anwendung sicherzustellen.
-   Verwenden Sie keine Toucheingabevisualisierungen in Situationen, in denen diese die Verwendung der App beeinträchtigen könnten. Weitere Informationen finden Sie unter [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969).
-   Zeigen Sie nur dann Feedback an, wenn dies absolut notwendig ist. Sorgen Sie dafür, dass die Benutzeroberfläche übersichtlich bleibt. Zeigen Sie nur dann visuelles Feedback an, wenn die darin enthaltenen Informationen sonst nirgends verfügbar sind.
-   Versuchen Sie, die Verhaltensweisen der integrierten Windows-Gesten für visuelles Feedback nicht in erheblichem Umfang anzupassen, da dies eine inkonsistente und verwirrende Benutzerumgebung zur Folge haben kann.

## Weitere Hinweise zur Verwendung

Kontaktvisualisierungen sind besonders für Touchinteraktionen wichtig, bei denen Genauigkeit und Präzision gefragt sind. Ihre App sollte beispielsweise die Position, auf die getippt wurde, genau anzeigen, damit der Benutzer erkennen kann, ob er das Ziel verfehlt hat, wie weit er danebenliegt und welche Korrekturen erforderlich sind.

Durch die Verwendung der Standardsteuerelemente für die XAML-Plattform stellen Sie sicher, dass Ihre App auf allen Geräten und in allen Eingabesituationen ordnungsgemäß funktioniert. Wenn Ihre App benutzerdefinierte Interaktionen enthält, die angepasstes Feedback erfordern, müssen Sie sicherstellen, dass sich das Feedback für die jeweiligen Zwecke eignet, für alle unterstützten Eingabegeräte verfügbar ist und den Benutzer nicht von seiner eigentlichen Arbeit ablenkt. Dies kann insbesondere bei Spiel- oder Zeichnungs-Apps ein Problem sein, bei denen das visuelle Feedback mit wichtigen UI-Elementen kollidieren oder diese verdecken kann.

[!IMPORTANT] Das Interaktionsverhalten der integrierten Gesten sollte nicht geändert werden. 

**Feedback auf allen Geräten**

Das visuelle Feedback ist im Allgemeinen vom Eingabegerät abhängig (Toucheingabe, Touchpad, Maus, Stift, Tastatur usw.). Das integrierte Feedback für die Maus z. B. beinhaltet normalerweise eine Bewegung und Änderung des Cursors, während für Touch- und Stifteingabe Berührungsvisualisierungen erforderlich sind und für die Eingabe und Navigation per Tastatur Fokusrechtecke und Hervorhebung verwendet werden.

Verwenden Sie [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969), um das Feedbackverhalten für die Plattformgesten festzulegen.

Wenn Sie Anpassungen an der Feedback-UI vornehmen, muss das Feedback alle Eingabemodi unterstützen und für alle Modi geeignet sein.

Im Folgenden finden Sie einige Beispiele für integrierte Kontaktvisualisierungen in Windows.

| ![Touchfeedback](images/TouchFeedback.png) | ![Mausfeedback](images/MouseFeedback.png) | ![Stiftfeedback](images/PenFeedback.png) | ![Tastaturfeedback](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Touchvisualisierung | Maus-/Touchpadvisualisierung | Stiftvisualisierung | Tastaturvisualisierung |

## Visuelle Fokuselemente mit hoher Sichtbarkeit

Alle Windows-Apps zeigen ein stärker definiertes visuelles Fokuselement um interaktive Steuerelemente innerhalb der Anwendung herum. Diese neuen visuellen Fokuselemente können vollständig angepasst und auch gelöscht werden, wenn nötig.

## Farbbranding und -anpassung

**Rahmeneigenschaften**

Es gibt zwei Elemente bei den visuellen Fokuselementen mit hoher Sichtbarkeit: der primäre und der sekundäre Rahmen. Der primäre Rahmen ist **2 Pixel** breit und verläuft an der *Außenseite* des sekundären Rahmens. Der sekundäre Rahmen ist **1 Pixel** breit und verläuft an der *Innenseite* des primären Rahmens.
![Redlines visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusRectRedlines.png)

Um die Breite der beiden Rahmentypen (primär oder sekundär) zu ändern, verwenden Sie **FocusVisualPrimaryThickness** bzw. **FocusVisualSecondaryThickness**:
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Randbreiten visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusMargin.png)

Der Rand ist eine Eigenschaft des Typs [**Thickness**](https://msdn.microsoft.com/library/system.windows.thickness) und kann daher so angepasst werden, dass er nur an bestimmten Seiten des Steuerelements angezeigt wird. Weitere Informationen siehe unten: ![Randbreite visueller Fokuselemente mit hoher Sichtbarkeit nur unten](images/FocusThicknessSide.png)

Der Rand ist der Abstand zwischen den visuellen Grenzen des Steuerelements und dem Beginn des *sekundären Rahmens* der visuellen Fokuselemente. Der standardmäßige Rand hat eine Breite von **1 Pixel** außerhalb der Grenzen des Steuerelements. Sie können diesen Rand pro Steuerelement bearbeiten, indem Sie die Eigenschaft **FocusVisualMargin** ändern:
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![Randunterschiede visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusPlusMinusMargin.png)

*Ein negativer Rand verschiebt den Rahmen weiter weg von der Mitte des Steuerelements. Ein positiver Rand verschiebt den Rahmen näher zur Mitte des Steuerelements.*

Um die visuellen Fokuselemente für ein Steuerelement vollständig zu deaktivieren, deaktivieren Sie einfach **UseSystemFocusVisuals**:
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

Die Breite, der Rand oder die vollständige Entfernung der visuellen Fokuselemente durch den App-Entwickler werden pro Steuerelement festgelegt.

**Farbeigenschaften**

Es gibt nur zwei Farbeigenschaften für die visuellen Fokuselemente; die primäre Rahmenfarbe und die sekundäre Rahmenfarbe. Diese Rahmenfarben für visuelle Fokuselemente können pro Steuerelement auf Seitenebene und global auf App-Ebene geändert werden:

Um App-weites Branding visueller Fokuselemente durchzuführen, überschreiben Sie die Systempinsel:
```XAML
<SolidColorBrush x:Key="SystemControlFocusVisualPrimaryBrush" Color="DarkRed"/>
<SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="Pink"/>
```
![Änderungen der Farbe visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusRectColorChanges.png)

Um die Farben pro Steuerelement zu ändern, bearbeiten Sie einfach die Eigenschaften der visuellen Fokuselemente für das gewünschte Steuerelement:
```XAML
<Slider Width="200" FocusVisualPrimaryBrush="DarkRed" FocusVisualSecondaryBrush="Pink"/>
```

## Verwandte Artikel

**Für Designer**
* [Anleitungen für das Verschieben](guidelines-for-panning.md)

**Für Entwickler**
* [Benutzerdefinierte Benutzerinteraktionen](https://msdn.microsoft.com/library/windows/apps/mt185599)

**Beispiele**
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: Beispiel XAML-Benutzereingabeereignisse](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für Fingereingabe-Treffertests](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoomen](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: vereinfachtes Freihandbeispiel](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Eingabe: Beispiel für Windows 8-Bewegungen](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Eingabe: Beispiel für Manipulationen und Gesten (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Beispiel für die DirectX-Fingereingabe](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 



<!--HONumber=Jun16_HO4-->


