---
author: jaster
ms.assetid: 
title: Benutzung des Visual Layer mit XAML
description: "Lernen Sie Methoden für die Verwendung der Visual Layer APIs in Kombination mit existierendem XAML-Inhalten kennen, um Animationen und fortgeschrittene Effekte zu erzeugen."
translationtype: Human Translation
ms.sourcegitcommit: dfda33c70224f32d9c3e8877eabdfcd965521757
ms.openlocfilehash: 00d663b130202f4513cd1a9d82baed4068d909d3

---

# Benutzung des Visual Layer mit XAML

## Einführung

Die meisten Apps, die die Visual-Layer-Funktionen nutzen, verwenden XAML, um den Hauptinhalt der Benutzeroberfläche zu definieren. Im Rahmen des Windows 10 Anniversary Update sind neue Features im XAML-Framework und dem Visual Layer erschienen, die es einfacher machen, diese beiden Technologien zum Erstellen beeindruckender Erlebnisse für den Benutzer kombinieren.
Funktionen von XAML und Visual Layer "Interop" kann zum Erstellen von erweiterten Animationen und Effekte nicht allein mithilfe von XAML-API verwendet werden. Dazu zählen:

-              Durch Scrollen gesteuerte Animationen und Parallax-Bildlauf
-              Automatische Layout-Animationen
-              Pixelperfekte Schlagschatten
-              Unschärfe- und Milchglaseffekte

Diese Effekte und Animationen können auf vorhandenen XAML-Inhalt angewendet werden, damit Sie Ihre XAML-App nicht erheblich umstrukturieren müssen, um neue Funktionen nutzen.
Layoutanimationen, Schatten und Effekte für Unschärfe werden im folgenden Rezepteabschnitt behandelt. Ein Codebeispiel, das Parallax implementiert, finden Sie unter [ParallaxingListItems sample](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems). Die [WindowsUIDevLabs Repository](https://github.com/Microsoft/WindowsUIDevLabs) verfügt außerdem über weitere Beispiele für die Implementierung von Animationen, Schatten und Effekte.

## Die **ElementCompositionPreview** Klasse

[**ElementCompositionPreview**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.aspx) ist eine statische Klasse, die Kompatibilität mit XAML- und Visual Layer-Funktionalitäten bietet. Eine Übersicht über die visuelle Ebene und die zugehörige Funktionalität finden Sie unter [Visual Layer](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer). Die **ElementCompositionPreview** Klasse bietet die folgenden Methoden:

-   [**GetElementVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): Erhalten Sie eine „Handout“-Grafik, die verwendet wurde, um dieses Element zu rendern
-   [**SetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual.aspx): Legt ein "handin" Visual als untersten Ordner in der Struktur dieses Elements fest. Dieses Visual wird über den Rest des Elements gezeichnet. 
-   [**GetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): Rufen Sie das Visual Set ab, indem Sie **SetElementChildVisual** benutzen
-   [**GetScrollViewerManipulationPropertySet**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx): Erhalten Sie ein Objekt, das zur Erstellung von Animationen mit 60fps basierend auf der Scrollposition in einem **ScrollViewer** verwendet werden kann

## Hinweise auf **ElementCompositionPreview.GetElementVisual**

**ElementCompositionPreview.GetElementVisual** gibt ein "handout" Visual aus, das benutzt wird, um das gegebene **UIElement** zu rendern. Eigenschaften wie z. B. **Visual.Opacity**, **Visual.Offset** und **Visual.Size** werden durch das XAML-Framework, das auf den Zustand des UIElements basiert, festgelegt. Dies kann Techniken wie implizite Repositionierungsanimationen (siehe *Rezepte*) aktivieren.

Hinweis: Weil der **Offset** und die **Größe** als Ergebnis des Layouts des XAML-Frameworks festgelegt sind, Entwickler vorsichtig sein sollten, wenn sie diese Eigenschaften animieren oder ändern. Entwickler sollten den Offset ändern oder animieren, wenn die obere linke Ecke des Elements dieselbe Position hat, wie die des übergeordneten Elements im Layout. Die Größe sollte im Allgemeinen nicht geändert werden, jedoch kann der Zugriff auf die Eigenschaft hilfreich sein. Beispielsweise verwenden die folgenden Schlagschatten und Milchglas-Samples unten die Größe eines Handout Visuals als Eingabe für eine Animation.

Als weitere Sicherheit werden aktualisierte Eigenschaften des Handout Visuals nicht im entsprechenden UIElement widergespiegelt. Also wird beispielsweise das Einstellen der **UIElement.Opacity** auf 0,5 die Deckkraft des entsprechende Handout Visuals auf 0,5 festlegen. Das Festlegen der Deckkraft das Handout Visuals **Opacity** auf 0,5 bewirkt, dass der Inhalt mit 50 % Deckkraft angezeigt wird, aber es wird sich nicht auf die entsprechende UIElement Opacity-Eigenschaft auswirken.

### Beispiel für **Offset** Animation

#### Falsch

```xml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### Richtig

```xml
<Border>
    <Canvas Margin="5">
        <Image x:Name="MyImage" />
    </Canvas>
</Border>
```

```csharp
// This works because the Canvas parent doesn’t generate a layout offset.
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

## Die **ElementCompositionPreview.SetElementChildVisual** Methode

**ElementCompositionPreview.SetElementChildVisual** erlaubt es den Entwicklern, ein "Handin"-Visual zu einzubinden, welches als Teil des Visual Trees des Elements erscheinen wird. Dies ermöglicht Entwicklern das Erstellen einer "Kompositions-Insel", in denen Visual-basierter Inhalt innerhalb einer XAML-UI angezeigt werden kann. Entwickler sollten diese Technik jedoch vorsichtig einsetzen, da Visual-basierter Inhalt nicht dieselben Zugriffsmöglichkeiten und Garantien durch Benutzererfahrungen wie XAML-Inhalte haben. Daher ist es im Allgemeinen empfehlenswert, dieses Verfahren nur bei Bedarf zur Implementierung benutzerdefinierter Effekte genutzt wird, wie die nachfolgend im Rezeptabschnitt enthaltenen.

## **GetAlphaMask** Methoden

[**Image**](https://msdn.microsoft.com/library/windows/apps/br242752), [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652), und [**Shape**](https://msdn.microsoft.com/library/windows/apps/br243377) implementieren jede eine Methode, die **GetAlphaMask** genannt wird, welche eine **CompositionBrush** ausgibt, die ein Graustufenbild in der Form des Elements darstellt. Dieser **CompositionBrush** kann als Eingabe für eine Komposition **DropShadow** dienen, also kann der Schatten die Form des Elements anstelle eines Rechtecks haben. Das ermöglicht auf Pixel abgestimmte, Konturbasierte Schatten für Text, Bilder mit Alpha und Formen. Sie können unter *Schlagschatten* im Folgenden ein Beispiel für diese API finden.

## Rezepte

### Repositionierungsanimation

Indem er die Komposition impliziter Animationen nutzt, kann Entwickler automatisch Änderungen im Layout eines Elements relativ zum übergeordneten Element animieren. Wenn Sie z.B. den **Rand** von der Taste unten ändern, wird es automatisch an die neue Layoutposition animieren.

#### Übersicht über die Implementierung

1.            Rufen das Handout **Visual** für das Zielelement ab
2.            Erstellen einer **ImplicitAnimationCollection** , die automatisch Änderungen der **Offset** Eigenschaft animiert
3.            Ordnen Sie die **ImplicitAnimationCollection** dem zugrunde liegenden Visual zu.

#### XAML

```xml
<Button x:Name="RepositionTarget" Content="Click Me" />
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeRepositionAnimation(RepositionTarget);
}

private void InitializeRepositionAnimation(UIElement repositionTarget)
{
    var targetVisual = ElementCompositionPreview.GetElementVisual(repositionTarget);
    Compositor compositor = targetVisual.Compositor;

    // Create an animation to animate targetVisual's Offset property to its final value
    var repositionAnimation = compositor.CreateVector3KeyFrameAnimation();
    repositionAnimation.Duration = TimeSpan.FromSeconds(0.66);
    repositionAnimation.Target = "Offset";
    repositionAnimation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");

    // Run this animation when the Offset Property is changed
    var repositionAnimations = compositor.CreateImplicitAnimationCollection();
    repositionAnimations["Offset"] = repositionAnimation;

    targetVisual.ImplicitAnimations = repositionAnimations;
}
```

### Schlagschatten

Wenden Sie Pixelgenaue Schlagschatten auf ein **UIElement**, z. B. einer **Ellipse** an, die ein Bild enthält. Da der Schatten ein **SpriteVisual**, der durch die App erstellt wird, erfordert, müssen wir ein "Hostelement" erstellen, das die **SpriteVisual** enthält. Benutzt wird dazu **ElementCompositionPreview.SetElementChildVisual**.

#### Übersicht über die Implementierung

1.            Rufen Sie das Handout **Visual** für das Hostelement ab
2.            Erstellen einer Windows.UI.Composition **DropShadow**
3.            Konfigurieren Sie den **DropShadow**, damit er seine Form vom Zielelement über eine Maske bekommt
    - **DropShadow** ist in den Standarteinstellung rechteckig, sodass dies nicht nötig ist, wenn das Zielelement rechteckig ist
4.            Fügen Sie einen Schatten an einen neuen **SpriteVisual** an, und legen Sie das **SpriteVisual** als untergeordnetes Element des Hostelements fest
5.            Legen Sie die Größe des **SpriteVisual** auf die Größe des Hosts fest, mit einer **ExpressionAnimation**

#### XAML

```xml
<Grid Width="200" Height="200">
    <Canvas x:Name="ShadowHost" />
    <Ellipse x:Name="CircleImage">
        <Ellipse.Fill>
            <ImageBrush ImageSource="Assets/Images/2.jpg" Stretch="UniformToFill" />
        </Ellipse.Fill>
    </Ellipse>
</Grid>
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

private void InitializeDropShadow(UIElement shadowHost, Shape shadowTarget)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(shadowHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a drop shadow
    var dropShadow = compositor.CreateDropShadow();
    dropShadow.Color = Color.FromArgb(255, 75, 75, 80);
    dropShadow.BlurRadius = 15.0f;
    dropShadow.Offset = new Vector3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask = shadowTarget.GetAlphaMask();

    // Create a Visual to hold the shadow
    var shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
   ElementCompositionPreview.SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual.StartAnimation("Size", bindSizeAnimation);
}
```

### Milchglas

Erstellen Sie einen Effekt, der den Inhalt verwischt und den Hintergrund färbt. Beachten Sie, dass Entwickler das Win2D NuGet-Paket installieren müssen, um Effekte verwenden. Sie finden unter der [Win2D Homepage](http://microsoft.github.io/Win2D/html/Introduction.htm) Installationsanweisungen.

#### Übersicht über die Implementierung

1.            Rufen Sie das Handout **Visual** für das Hostelement ab
2.            Erstellen Sie einen Unschärfeeffektstruktur, indem Sie die Win2D und **CompositionEffectSourceParameter** nutzen
3.            Erstellen Sie **CompositionEffectBrush**, basierend auf der Effekt-Struktur
4.            Legen Sie die Eingabe von der **CompositionEffectBrush** zu einer **CompositionBackdropBrush**, was ihnen einen Effekt ermöglicht, der auf den Inhalt hinter einem **SpriteVisual** angewendet werden kann.
5.            Legen Sie die **CompositionEffectBrush** als Inhalt eines neuen **SpriteVisual** fest, und legen Sie das **SpriteVisual** als untergeordnetes Element des Hostelements fest.
6.            Legen Sie die Größe des **SpriteVisual** auf die Größe des Hosts fest, mit einer **ExpressionAnimation**

#### XAML

```xml
<Grid Width="300" Height="300" Grid.Column="1">
    <Image
        Source="Assets/Images/2.jpg"
        Width="200"
        Height="200" />
    <Canvas
        x:Name="GlassHost"
        Width="150"
        Height="300"
        HorizontalAlignment="Right" />
</Grid>
```

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeFrostedGlass(GlassHost);
}

private void InitializedFrostedGlass(UIElement glassHost)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(glassHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a glass effect, requires Win2D NuGet package
    var glassEffect = new GaussianBlurEffect
    { 
        BlurAmount = 15.0f,
        BorderMode = EffectBorderMode.Hard,
        Source = new ArithmeticCompositeEffect
        {
            MultiplyAmount = 0,
            Source1Amount = 0.5f,
            Source2Amount = 0.5f,
            Source1 = new CompositionEffectSourceParameter("backdropBrush"),
            Source2 = new ColorSourceEffect
            {
                Color = Color.FromArgb(255, 245, 245, 245)
            }
        }
    };

    //  Create an instance of the effect and set its source to a CompositionBackdropBrush
    var effectFactory = compositor.CreateEffectFactory(glassEffect);
    var backdropBrush = compositor.CreateBackdropBrush();
    var effectBrush = effectFactory.CreateBrush();

    effectBrush.SetSourceParameter("backdropBrush", backdropBrush);

    // Create a Visual to contain the frosted glass effect
    var glassVisual = compositor.CreateSpriteVisual();
    glassVisual.Brush = effectBrush;

    // Add the blur as a child of the host in the visual tree
    ElementCompositionPreview.SetElementChildVisual(glassHost, glassVisual);

    // Make sure size of glass host and glass visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    glassVisual.StartAnimation("Size", bindSizeAnimation);
}
```

## Weitere Ressourcen:

-   [Übersicht über die visuelle Ebene](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer)
-   [**ElementCompositionPreview** Klasse](https://msdn.microsoft.com/library/windows/apps/mt608976)
-   Erweiterte UI und Kompositionsbeispiele in dem [WindowsUIDevLabs-GitHub](https://github.com/microsoft/windowsuidevlabs).
-   [BasicXamlInterop-Beispiel](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
-   [ParallaxingListItems-Beispiel](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)



<!--HONumber=Aug16_HO3-->


