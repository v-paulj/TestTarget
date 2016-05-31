---
author: scottmill
ms.assetid: 6e9b9ff2-234b-6f63-0975-1afb2d86ba1a
title: Kompositionseffekte
description: Mithilfe von Effekt-APIs können Entwickler anpassen, wie ihre Benutzeroberfläche gerendert wird.
---
# Kompositionseffekte

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Mit der WinRT-API [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) können Echtzeiteffekte mithilfe animierbarer Effekteigenschaften auf Bilder und Benutzeroberflächen angewendet werden. In dieser Übersicht erläutern wir die Funktionen, über die Effekte auf visuelle Kompositionselemente angewendet werden können.

Um die Konsistenz der [Universellen Windows-Plattform (UWP)](https://msdn.microsoft.com/library/windows/apps/dn726767.aspx) für Entwickler zu gewährleisten, die Effektbeschreibungen in ihren Anwendungen verwenden, nutzen Kompositionseffekte die „IGraphicsEffect“-Schnittstelle von Win2D, um die Effektbeschreibungen über den [Microsoft.Graphics.Canvas.Effects](http://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.md)-Namespace anzuwenden.

Pinseleffekte werden verwendet, um Bereiche einer Anwendung farbig zu gestalten. Dabei werden Effekte auf eine Gruppe vorhandener Bilder angewendet. Die Kompositionseffekt-APIs in Windows 10 sind auf visuelle Sprite-Elemente ausgerichtet. Das SpriteVisual-Element ermöglicht hohe Flexibilität und Interaktion bei der Farb-, Bild- und Effektgestaltung. SpriteVisual ist ein visueller Kompositionstyp, der ein 2D-Rechteck mit einem Pinsel füllen kann. Das visuelle Element definiert die Grenzen des Rechtecks und der Pinsel die Pixel zum Zeichnen des Rechtecks.

Effektpinsel werden für visuelle Elemente von Kompositionsstrukturen verwendet, deren Inhalt der Ausgabe eines Effektgraphen entnommen wird. Effekte können auf vorhandene Oberflächen/Texturen verweisen, aber nicht auf die Ausgabe anderer Kompositionsstrukturen.

## Effektfeatures

-   [Effektbibliothek](./composition-effects.md#effect-library)
-   [Verketten von Effekten](./composition-effects.md#chaining-effects)
-   [Animationsunterstützung](./composition-effects.md#animation-support)
-   [Effekteigenschaften – Konstante und Animation](./composition-effects.md#effect-properties-constant-vs-animated)
-   [Mehrere Effektinstanzen mit unabhängigen Eigenschaften](./composition-effects.md#multiple-effect-instances-with-independent-properties)

### Effektbibliothek

Derzeit unterstützt die Komposition folgende Effekte:

| Effekt               | Beschreibung                                                                                                                                                                                                                |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2D-affine Transformation  | Wendet eine 2D-affine Transformationsmatrix auf ein Bild an. Dieser Effekt wurde verwendet, um die Alphamaske in unseren [Effektbeispielen](http://go.microsoft.com/fwlink/?LinkId=785341) zu animieren.       |
| Arithmetische Komposition | Kombiniert zwei Bilder mittels einer flexiblen Gleichung. Eine arithmetische Komposition wurde verwendet, um einen Überblendungseffekt in unseren [Beispielen](http://go.microsoft.com/fwlink/?LinkId=785341) zu erzeugen. |
| Fülleffekt         | Erzeugt einen Fülleffekt, der zwei Bilder kombiniert. Die Komposition stellt 21 der 26 in Win2D unterstützten [Füllmethoden](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_Effects_BlendEffectMode.md) bereit.        |
| Farbquelle         | Generiert ein Bild, das eine Volltonfarbe enthält.                                                                                                                                                                               |
| Komposition            | Kombiniert zwei Bilder. Die Komposition stellt alle 13 in Win2D unterstützten [Kompositionsmodi](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasComposite.md) bereit.                                              |
| Kontrast             | Erhöht oder verringert den Kontrast eines Bilds.                                                                                                                                                                           |
| Belichtung             | Erhöht oder verringert die Belichtung eines Bilds.                                                                                                                                                                           |
| Graustufen            | Konvertiert ein Bild in ein monochromes Graustufenbild.                                                                                                                                                                                   |
| Gammakorrektur       | Ändert die Farben eines Bilds, indem die Gammakorrekturfunktion pro Kanal angewendet wird.                                                                                                                                           |
| Farbtondrehung           | Ändert die Farbe eines Bilds durch Rotation der Farbtonwerte.                                                                                                                                                                   |
| Invertierung               | Invertiert die Farben eines Bilds.                                                                                                                                                                                            |
| Sättigung             | Ändert die Sättigung eines Bilds.                                                                                                                                                                                         |
| Sepia                | Konvertiert ein Bild in Sepiatöne.                                                                                                                                                                                          |
| Temperatur und Farbton | Passt die Temperatur und/oder den Farbton eines Bilds an.                                                                                                                                                                           |

 

Ausführliche Informationen finden Sie in der Beschreibung des Win2D-Namespaces [Microsoft.Graphics.Canvas.Effects](http://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.md). In der Komposition nicht unterstützte Effekte sind mit \[NoComposition\] gekennzeichnet.

### Verketten von Effekten

Effekte können verkettet werden, um einer Anwendung die gleichzeitige Nutzung mehrerer Effekte in einem Bild zu ermöglichen. Effektgraphen unterstützen mehrere Effekte, die aufeinander verweisen können. Bei der Beschreibung eines Effekts fügen Sie Ihrem Effekt einfach einen Effekt als Eingabe hinzu.

```cs
IGraphicsEffect graphicsEffect =
new Microsoft.Graphics.Canvas.Effects.ArithmeticCompositeEffect
{
  Source1 = new CompositionEffectSourceParameter("source1"),
  Source2 = new SaturationEffect
  {
    Saturation = 0,
    Source = new CompositionEffectSourceParameter("source2")
  },
  MultiplyAmount = 0,
  Source1Amount = 0.5f,
  Source2Amount = 0.5f,
  Offset = 0    
}
  
```

Im obigen Beispiel wird der Effekt einer arithmetischen Komposition mit zwei Eingaben beschrieben. Die zweite Eingabe hat einen Sättigungseffekt mit der Sättigungseigenschaft „0,5“.

### Animationsunterstützung

Effekteigenschaften bieten Unterstützung für Animationen. Während der Effektkompilierung können Sie angeben, welche Effekteigenschaften animiert und welche als Konstanten vorgegeben werden können. Die animierbaren Eigenschaften werden über Zeichenfolgen im Format „Effektname.Eigenschaftenname“ angegeben. Diese Eigenschaften können unabhängig über mehrere Instanziierungen des Effekts animiert werden.

### Effekteigenschaften – Konstante und Animation

Während der Effektkompilierung können Sie dynamische Effekteigenschaften oder als Konstanten vorgegebene Eigenschaften angeben. Die dynamischen Eigenschaften werden über Zeichenfolgen in folgendem Format angegeben: „<effect name>.<property name>“. Die dynamischen Eigenschaften können auf einen bestimmten Wert festgelegt oder mithilfe des Kompositionsanimationssystems animiert werden.

Beim Kompilieren der oben angegebenen Effektbeschreibung können Sie die Sättigung entweder als Konstante mit dem Wert „0,5“ vorgeben oder sie dynamisch gestalten (sie also dynamisch festlegen oder animieren).

Kompilieren eines Effekts mit als Konstante festgelegter Sättigung:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);              
```

Kompilieren eines Effekts mit dynamischer Sättigung:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect, new[]{SaturationEffect.Saturation});
_catEffect = effectFactory.CreateBrush();
_catEffect.SetSourceParameter("mySource", surfaceBrush);
_catEffect.Properties.InsertScalar("saturationEffect.Saturation", 0f);
```

Die Sättigungseigenschaft des oben genannten Effekts kann dann entweder auf einen statischen Wert festgelegt oder mithilfe der Expression- oder ScalarKeyFrame-Animation animiert werden.

Sie können ein ScalarKeyFrame-Element erstellen, das wie folgt zum Animieren der Sättigungseigenschaft eines Effekts verwendet wird:

```cs
ScalarKeyFrameAnimation effectAnimation = _compositor.CreateScalarKeyFrameAnimation();
            effectAnimation.InsertKeyFrame(0f, 0f);
            effectAnimation.InsertKeyFrame(0.50f, 1f);
            effectAnimation.InsertKeyFrame(1.0f, 0f);
            effectAnimation.Duration = TimeSpan.FromMilliseconds(2500);
            effectAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
```

Starten Sie wie folgt die Animation der Sättigungseigenschaft des Effekts:

```cs
catEffect.Properties.StartAnimation("saturationEffect.Saturation", effectAnimation);
```

Im Beispiel [Entsättigung – Animation](http://go.microsoft.com/fwlink/?LinkId=785342) finden Sie mithilfe von Keyframes animierte Effekteigenschaften. Das [AlphaMask-Beispiel](http://go.microsoft.com/fwlink/?LinkId=785343) enthält Informationen zur Verwendung von Effekten und Ausdrücken.

### Mehrere Effektinstanzen mit unabhängigen Eigenschaften

Wenn angegeben wird, dass ein Parameter während der Effektkompilierung dynamisch sein soll, kann der Parameter pro Effektinstanz geändert werden. Dadurch können zwei visuelle Elemente denselben Effekt verwenden, aber mit unterschiedlichen Effekteigenschaften gerendert werden. Weitere Informationen finden Sie im [Beispiel](http://go.microsoft.com/fwlink/?LinkId=785344) zum ColorSource- und Blend-Effekt.

## Erste Schritte mit Kompositionseffekten

In diesem Schnellstart-Lernprogramm erfahren Sie, wie Sie einige der grundlegenden Effektfunktionen nutzen.

-   [Installieren von Visual Studio](./composition-effects.md#installing-visual-studio)
-   [Erstellen eines neuen Projekts](./composition-effects.md#creating-a-new-project)
-   [Installieren von Win2D](./composition-effects.md#installing-win2d)
-   [Festlegen der Grundlagen für die Komposition](./composition-effects.md#setting-your-composition-basics)
-   [Erstellen eines CompositionSurface-Pinsels](./composition-effects.md#creating-a-compositionsurface-brush)
-   [Erstellen, Kompilieren und Anwenden von Effekten](./composition-effects.md#creating,-compiling-and-applying-effects)

### Installieren von Visual Studio

-   Wenn Sie keine unterstützte Version von Visual Studio installiert haben, wechseln Sie [hier](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) zur Visual Studio-Downloadseite.

### Erstellen eines neuen Projekts

-   Wählen Sie „Datei->Neu->Projekt“ aus.
-   Wählen Sie „Visual C#“ aus.
-   Erstellen Sie eine App vom Typ „(Leere App (Windows – universell)“ (Visual Studio 2015).
-   Geben Sie einen Projektnamen Ihrer Wahl ein.
-   Klicken Sie auf „OK“.

### Installieren von Win2D

Win2D wird als „Nuget.org“-Paket freigegeben und muss installiert werden, damit Sie Effekte nutzen können.

Es gibt zwei Paketversionen: eine für Windows 10 und eine für Windows 8.1. Für Kompositionseffekte verwenden Sie die Windows 10-Version.

-   Starten Sie den NuGet-Paket-Manager, indem Sie „Extras → NuGet-Paket-Manager → NuGet-Pakete für Projektmappe verwalten“ auswählen.
-   Suchen Sie nach „Win2D“, und wählen Sie das entsprechende Paket für die Zielversion von Windows aus. Da Windows.UI. Composition Windows 10 (aber nicht 8.1) unterstützt, wählen Sie „Win2D.uwp“ aus.
-   Akzeptieren Sie den Lizenzvertrag.
-   Klicken Sie auf „Schließen“.

In den nächsten Schritten verwenden wir Composition-APIs, um einen Sättigungseffekt auf dieses Katzenfoto anzuwenden, durch den die gesamte Sättigung entfernt wird. In diesem Modell wird der Effekt erstellt und dann auf ein Bild angewendet.

![Quellbild](images/composition-cat-source.png)
### Festlegen der Grundlagen für die Komposition

Anhand des [Beispiels für die visuelle Kompositionsstruktur](http://go.microsoft.com/fwlink/?LinkId=785345) auf GitHub erfahren Sie, wie Sie den „Windows.UI.Composition“-Kompositor einrichten, den „ContainerVisual“-Stamm angeben und diesen dem Hauptfenster zuordnen.

```cs
_compositor = new Compositor();
_root = _compositor.CreateContainerVisual();
_target = _compositor.CreateTargetForCurrentView();
_target.Root = _root;
_imageFactory = new CompositionImageFactory(_compositor)
Desaturate();
```

### Erstellen eines CompositionSurface-Pinsels

```cs
CompositionSurfaceBrush surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(surfaceBrush); 
```

### Erstellen, Kompilieren und Anwenden von Effekten

1.) Erstellen Sie den Grafikeffekt.
```cs
var graphicsEffect = new SaturationEffect
{
  Saturation = 0.0f,
  Source = new CompositionEffectSourceParameter("mySource")
};
```

2.) Kompilieren Sie den Effekt, und erstellen Sie einen Effektpinsel.
```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);

var catEffect = effectFactory.CreateBrush();
catEffect.SetSourceParameter("mySource", surfaceBrush);
```

3.) Erstellen Sie ein SpriteVisual-Element in der Kompositionsstruktur, und wenden Sie den Effekt an.
```cs
var catVisual = _compositor.CreateSpriteVisual();
  catVisual.Brush = catEffect;
  catVisual.Size = new Vector2(219, 300);
  _root.Children.InsertAtBottom(catVisual);
}
```

4.) Erstellen Sie die zu ladende Bildquelle.
```cs
CompositionImage imageSource = _imageFactory.CreateImageFromUri(new Uri("ms-appx:///Assets/cat.png"));
CompositionImageLoadResult result = await imageSource.CompleteLoadAsync();
if (result.Status == CompositionImageLoadStatus.Success)
```

5.) Passen Sie die Größe der SpriteVisual-Oberfläche an, und füllen Sie sie.
```cs
brush.Surface = imageSource.Surface;
```

6.) Führen Sie die App aus, um ein Katzenfoto ohne Sättigung zu erhalten:

![Bild ohne Sättigung](images/composition-cat-desaturated.png)
## Weitere Informationen

-   [Microsoft – GitHub-Seite zum Thema „Komposition“](https://github.com/Microsoft/composition)
-   [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878)
-   [Windows Composition-Team auf Twitter](https://twitter.com/wincomposition)
-   [Kompositionsübersicht](https://blogs.windows.com/buildingapps/2015/12/08/awaken-your-creativity-with-the-new-windows-ui-composition/)
-   [Grundlagen der visuellen Struktur](composition-visual-tree.md)
-   [Kompositionspinsel](composition-brushes.md)
-   [Übersicht über Animationen](composition-animation.md)
-   [Systemeigene DirectX- und Direct2D-Interoperabilität mit „BeginDraw“ und „EndDraw“](composition-native-interop.md)

 

 






<!--HONumber=May16_HO2-->


