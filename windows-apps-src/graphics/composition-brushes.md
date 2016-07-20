---
author: scottmill
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Kompositionspinsel
description: "Ein Pinsel zeichnet den Bereich eines Visual-Objekts mit der zugehörigen Ausgabe. Verschiedene Pinsel haben unterschiedliche Ausgabetypen."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: a9f30ca041d320798c7ace596bd9be37f9712129

---
# Kompositionspinsel

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Ein Pinsel zeichnet den Bereich eines [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekts mit der zugehörigen Ausgabe. Verschiedene Pinsel haben unterschiedliche Ausgabetypen. Die Composition-API bietet drei Pinseltypen:

-   
              [
              **CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) zeichnet ein Visual-Element mit einer Volltonfarbe.
-   
              [
              **CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) zeichnet ein Visual-Element mit den Inhalten einer Kompositionsoberfläche.
-   
              [
              **CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) zeichnet ein Visual-Element mit den Inhalten einer Kompositionseffekts.

Alle Pinsel erben von [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398). Die geräteabhängigen Ressourcen werden direkt oder indirekt durch [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) erstellt. Zwar sind Pinsel geräteunabhängig, aber [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) und [**CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) zeichnen ein [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekt mit geräteabhängigen Inhalten einer Kompositionsoberfläche.

-   [Voraussetzungen](./composition-brushes.md#prerequisites)
-   [Farbgrundlagen](./composition-brushes.md#color-basics)
    -   [Alpha-Modi](./composition-brushes.md#alpha-modes)
-   [Verwenden von Farbpinseln](./composition-brushes.md#using-color-brush)
-   [Verwenden von Oberflächenpinseln](./composition-brushes.md#using-surface-brush)
-   [Konfigurieren von Streckung und Ausrichtung](./composition-brushes.md#configuring-stretch-and-alignment)

## Voraussetzungen

In dieser Übersicht wird davon ausgegangen, dass Sie mit der Struktur einer einfachen Kompositionsanwendung, wie unter [Visuelle Ebene](visual-layer.md) beschrieben, vertraut sind.

## Farbgrundlagen

Vor dem Zeichnen mit [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) müssen Sie Farben auswählen. Die Composition-API verwendet die Windows-Runtime-Struktur „Color“, um eine Farbe darzustellen. Die Color-Struktur verwendet die sRGB-Codierung. Die sRGB-Codierung unterteilt Farben in vier Kanäle: Alpha, Rot, Grün und Blau. Jede Komponente wird durch einen Gleitkommawert mit einem normalen Bereich von 0,0 bis 1,0 dargestellt. Ein Wert von 0,0 gibt das vollständige Fehlen von Farbe an, während der Wert 1,0 angibt, dass die Farbe vollständig vorhanden ist. In der Alpha-Komponente steht0,0 für vollständig transparente Farbe und1,0 für vollständig deckende Farbe.

### Alpha-Modi

Farbwerte werden in [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) immer als nur als Alpha interpretiert.

## Verwenden von Farbpinseln

Um einen Pinsel zu erstellen, rufen Sie die Compositor.[**CreateColorBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositor.createcolorbrush.aspx)-Methode auf, die [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) zurückgibt. Die Standardfarbe für **CompositionColorBrush** ist „\#00000000“. Die Abbildung und der Code zeigen im Folgenden eine kleine visuelle Struktur. Es wird ein Rechteck erstellt, dessen Konturen mit einem schwarzen Pinsel gezeichnet sind und das mit einem Pinsel in Volltonfarbe ausgefüllt ist, die den Farbwert „0x9ACD32“ hat.

![CompositionColorBrush](images/composition-compositioncolorbrush.png)
```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual1, visual2;
CompositionColorBrush _blackBrush, _greenBrush; 

_compositor = new Compositor();
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
visual1 = _compositor.CreateSpriteVisual();
visual1.Brush = _blackBrush;
visual1.Size = new Vector2(156, 156);
visual1.Offset = new Vector3(0, 0, 0);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
Visual2 = _compositor.CreateSpriteVisual();
Visual2.Brush = _greenBrush;
Visual2.Size = new Vector2(150, 150);
Visual2.Offset = new Vector3(3, 3, 0);
```

Im Gegensatz zu anderen Pinseln ist das Erstellen von [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) relativ kostengünstig. Sie können **CompositionColorBrush**-Objekte bei jedem Rendern erstellen, ohne Leistungseinbußen in Kauf nehmen zu müssen.

## Verwenden von Oberflächenpinseln

[**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) zeichnet ein visuelles Element mit einer Kompositionsoberfläche (dargestellt durch ein [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819)-Objekt). Die folgende Abbildung zeigt ein quadratisches visuelles Element, das mit einer Lakritz-Bitmap gezeichnet und mithilfe von D2D auf einer **ICompositionSurface**-Schnittstelle gerendert wird.

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png) Im ersten Beispiel wird eine Kompositionsoberfläche für die Verwendung mit dem Pinsel initialisiert. Die Kompositionsoberfläche wird mithilfe der Hilfsmethode „LoadImage“ erstellt, die [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) und eine URL als Zeichenfolge akzeptiert. Sie lädt das Bild aus der URL, rendert das Bild auf einer [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819)-Schnittstelle und legt die Oberfläche als Inhalt von **CompositionSurfaceBrush** fest. Beachten Sie, dass die **ICompositionSurface**-Schnittstelle nur in systemeigenen Code verfügbar ist und daher die „LoadImage“-Methode in systemeigenem Code implementiert wird.

```cs
LoadImage(Brush,
          "ms-appx:///Assets/liqorice.png");
```

Um den Oberflächenpinsel zu erstellen, rufen Sie die Compositor.[**CreateSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositor.createsurfacebrush.aspx)-Methode auf. Die Methode gibt hier ein [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415)-Objekt zurück. Der folgende Code veranschaulicht den Code, mit dem ein visuelles Element mit Inhalten eines **CompositionSurfaceBrush**-Objekts gezeichnet werden kann.

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual;
CompositionSurfaceBrush _surfaceBrush;

_surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(_surfaceBrush, "ms-appx:///Assets/liqorice.png");
visual.Brush = _surfaceBrush;
```

## Konfigurieren von Strecken und Ausrichtung

Manchmal füllt der Inhalt der [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) für [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) die Bereiche des zu zeichnenden visuellen Elements nicht vollständig. In diesem Fall verwendet die Composition-API die Moduseinstellungen [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx), [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) und [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) des Pinsels, um den restlichen Bereich auszufüllen.

-   
              [
              **HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) und [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) sind vom Typ „float“ und können verwendet werden, um die Position des Pinsels innerhalb der visuellen Grenzen zu steuern.
    -   Der Wert 0,0 richtet die linke obere Ecke des Pinsels an der linken oberen Ecke des visuellen Elements aus.
    -   Der Wert 0,5 richtet die Mitte des Pinsels an der Mitte des visuellen Elements aus.
    -   Der Wert1,0 richtet die rechte untere Ecke des Pinselelements an der rechten unteren Ecke des visuellen Elements aus.
-   Die [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch)-Eigenschaft akzeptiert die folgenden Werte, die durch die [**CompositionStretch**](https://msdn.microsoft.com/library/windows/apps/Dn706786)-Aufzählung definiert werden:
    -   Keine: Das Pinsel-Element wird nicht gestreckt, um die Grenzen des visuellen Elements auszufüllen. Beachten Sie bei Verwendung der Einstellung für Strecken Folgendes: Wenn der Pinsel größer als die Grenzen des visuellen Elements ist, wird der Inhalt des Pinsels abgeschnitten. Der Teil des Pinsels, der zum Zeichnen der visuellen Grenzen verwendet wird, kann mit den Eigenschaften [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) und [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) gesteuert werden.
    -   Uniform: Der Pinsel wird skaliert und an die Grenzen des visuellen Elements angepasst. Das Seitenverhältnis des Pinsels wird beibehalten. Dies ist der Standardwert.
    -   UniformToFill: Der Pinsel wird skaliert, damit es die Grenzen des visuellen Elements vollständig ausfüllt. Das Seitenverhältnis des Pinsels wird beibehalten.
    -   Fill: Der Pinsel wird skaliert und an die Grenzen des visuellen Elements angepasst. Da die Höhe und Breite des Pinsels unabhängig voneinander skaliert werden, wird das ursprüngliche Seitenverhältnis des Pinsels möglicherweise nicht beibehalten. Das bedeutet, dass der Pinsel eventuell verzerrt wird, um die Grenzen des visuellen Elements vollständig auszufüllen.

 

 







<!--HONumber=Jul16_HO2-->


