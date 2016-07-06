---
author: Karl-Bridge-Microsoft
Description: "Erstellen Sie UWP-Apps (Universelle Windows-Plattform), die benutzerdefinierte Interaktionen von Zeichen- und Eingabestiften unterstützen, einschließlich Freihandeingabe für das Schreiben und Zeichnen, wie Sie es von Papier gewohnt sind."
title: Zeichen- und Eingabestiftinteraktionen in UWP-Apps
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen and stylus interactions in UWP apps
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: e642e6ba5319dce2d78c243ab3c57a9ffcc6902f

---

# Zeichen- und Eingabestiftinteraktionen in UWP-Apps

Optimieren Sie Ihre UWP-Apps (Universelle Windows-Plattform) für Stifteingaben, um sowohl Standardfunktionalität für [**Zeigergeräte**](https://msdn.microsoft.com/library/windows/apps/br225633) als auch optimale Windows Ink-Funktionalität für Benutzer bereitzustellen.

> Hinweis: Der Schwerpunkt dieses Themas liegt auf der Windows Ink-Plattform. Informationen zur allgemeinen Behandlung von Zeigereingaben (ähnlich wie Maus-, Touch- und Touchpadeingaben) finden Sie unter [Behandeln von Zeigereingaben](handle-pointer-input.md).

![Touchpad](images/input-patterns/input-pen.jpg)

**Wichtige APIs**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
-   [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)

Zusammen mit einem Zeichenstift bietet die Windows Ink-Plattform eine Möglichkeit, digitale Notizen, Zeichnungen und Anmerkungen wie vom Papier her gewohnt zu erstellen. Sie können auf der Plattform Freihanddaten aus einem Eingabedigitalisierungsgerät erfassen, Freihanddaten generieren, verwalten und als letzte Striche auf dem Ausgabegerät rendern sowie über die Schrifterkennung in Text umwandeln.

Ihre App kann nicht nur die grundlegende Position und Bewegung des Stifts aufzeichnen, während der Benutzer schreibt oder zeichnet, sondern auch den variierenden Druck während des gesamten Strichs nachverfolgen und erfassen. Mit diesen Informationen, zusammen mit Einstellungen für Form und Größe der Stiftspitze, Drehung, Freihandfarbe und Zweck (einfache Freihandeingabe, Löschen, Hervorheben und Auswählen), können Sie dem Benutzer ermöglichen, auf ähnliche Wiese wie mit einem Stift, Bleistift oder Pinsel auf Papier zu arbeiten.

**Hinweis**  Ihre App kann auch Freihandeingaben von anderen zeigerbasierten Geräten wie Touchdigitalisierungs- und Mausgeräten unterstützen. 

Die Freihandplattform ist sehr flexibel. Sie unterstützt verschiedene Funktionalitätsgrade, abhängig von Ihren Anforderungen.

Die Freihandplattform enthält drei Komponenten:

-   [
              **InkCanvas**
            ](https://msdn.microsoft.com/library/windows/apps/dn858535): Ein XAML-UI-Plattformsteuerelement, das standardmäßig alle Eingaben von einem Stift als letzten Strich oder ausradierten Strich empfängt und anzeigt.

-   [
              **InkPresenter**
            ](https://msdn.microsoft.com/library/windows/apps/dn922011): Ein CodeBehind-Objekt, das zusammen mit einem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement instanziiert wird (über die [**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Eigenschaft verfügbar gemacht). Dieses Objekt stellt alle Standardfreihandfunktionen bereit, die vom **InkCanvas**-Steuerelement zur Verfügung gestellt werden, sowie einen umfassenden Satz von APIs für zusätzliche Anpassung und Personalisierung.

-   [
              **IInkD2DRenderer**
            ](https://msdn.microsoft.com/library/mt147263): Ermöglicht das Rendern von letzten Strichen im angegebenen Direct2D-Gerätekontext einer universellen Windows-App statt im standardmäßigen [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement. Dies ermöglicht die umfassende Anpassung der Freihandfunktionen.

## Einfaches Freihandzeichnen mit „InkCanvas“


Platzieren Sie für einfaches Freihandzeichen einfach an einer beliebigen Stelle auf einer Seite ein [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement.

Das [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement unterstützt nur Freihandeingaben mit einem Stift. Die Eingabe wird entweder als letzter Strich mit den Standardeinstellungen für Farbe und Stärke gerendert oder als Strichradierer behandelt (wenn die Eingabe von einer mit einer Löschschaltfläche geänderten Radiergummi- oder Stiftspitze stammt).

In diesem Beispiel überlagert ein [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement ein Hintergrundbild.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />            
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Diese Serie von Bildern zeigt, wie die Stifteingabe von diesem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement gerendert wird.

| ![Der leere „InkCanvas“ mit einem Hintergrundbild](images/ink_basic_1_small.png) | ![Der „InkCanvas“ mit letzten Strichen](images/ink_basic_2_small.png) | ![Der „InkCanvas“ mit einem ausradierten Strich](images/ink_basic_3_small.png) |
| --- | --- | ---|
| Der leere [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) mit einem Hintergrundbild | Der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) mit letzten Strichen | Der [**InkCanvas** ](https://msdn.microsoft.com/library/windows/apps/dn858535) mit einem ausradierten Strich (beachten Sie, dass jeweils der gesamte Strich und nicht nur auf einen Teil davon ausradiert wird). |

Die vom [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement unterstützte Freihandfunktion wird von einem CodeBehind-Objekt mit dem Namen [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) bereitgestellt.

Für die einfache Freihandeingabe müssen Sie sich nicht mit dem [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)-Objekt befassen. Wenn Sie jedoch das Freihandverhalten des [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements anpassen und konfigurieren möchten, müssen Sie auf das entsprechende **InkPresenter**-Objekt zugreifen.

## Einfache Anpassung mit „InkPresenter“


Für jedes [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement wird ein [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)-Objekt instanziiert.

Das [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)-Objekt stellt nicht nur das gesamte Standard-Freihandeingabeverhalten des entsprechenden [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements bereit, sondern bietet auch einen umfassenden Satz von APIs für die zusätzliche Strichanpassung. Hierzu zählen Stricheigenschaften, unterstützte Eingabegerätetypen und die Möglichkeit festzulegen, ob die Eingabe vom Objekt verarbeitet oder an die App übergeben wird.

**Hinweis**  
Das [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)-Objekt kann nicht direkt instanziiert werden. Stattdessen erfolgt der Zugriff darauf über die [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Eigenschaft des [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements.

 

Hier konfigurieren wir das [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt, damit es Eingabedaten von Stift und Maus als Freihandstriche interpretiert. Außerdem legen wir einige anfängliche Freihandstrichattribute fest, die zum Rendern von Strichen mit dem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement verwendet werden.

```CSharp
public MainPage()
{
    this.InitializeComponent();

    // Set supported inking device types.
    inkCanvas.InkPresenter.InputDeviceTypes = 
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;

    // Set initial ink stroke attributes.
    InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
    drawingAttributes.Color = Windows.UI.Colors.Black;
    drawingAttributes.IgnorePressure = false;
    drawingAttributes.FitToCurve = true;
    inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
}
```

Freihandstrichattribute können dynamisch entsprechend den Benutzereinstellungen oder App-Anforderungen festgelegt werden.

Hier kann der Benutzer aus einer Liste von Freihandfarben auswählen.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink customization sample" 
                   VerticalAlignment="Center"
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
        <TextBlock Text="Color:"
                   Style="{StaticResource SubheaderTextBlockStyle}"
                   VerticalAlignment="Center"
                   Margin="50,0,10,0"/>
        <ComboBox x:Name="PenColor"
                  VerticalAlignment="Center"
                  SelectedIndex="0"
                  SelectionChanged="OnPenColorChanged">
            <ComboBoxItem Content="Black"/>
            <ComboBoxItem Content="Red"/>
        </ComboBox>
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Anschließend behandeln wir Änderungen an der ausgewählten Farbe und aktualisieren die Attribute für letzte Striche entsprechend.

```CSharp
// Update ink stroke color for new strokes.
private void OnPenColorChanged(object sender, SelectionChangedEventArgs e)
{
    if (inkCanvas != null)
    {
        InkDrawingAttributes drawingAttributes = 
            inkCanvas.InkPresenter.CopyDefaultDrawingAttributes();

        string value = ((ComboBoxItem)PenColor.SelectedItem).Content.ToString();

        switch (value)
        {
            case "Black":
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
            case "Red":
                drawingAttributes.Color = Windows.UI.Colors.Red;
                break;
            default:
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
        };

        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
    }
}
```

Diese Bilder zeigen, wie die Stifteingabe vom [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt verarbeitet und angepasst wird.

| ![InkCanvas mit standardmäßigen schwarzen Freihandstrichen](images/ink-basic-custom-1-small.png) | ![InkCanvas mit vom Benutzer ausgewählten roten Freihandstrichen](images/ink-basic-custom-2-small.png) |
| --- | -- |
| Das [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement mit standardmäßigen schwarzen letzten Strichen | Das [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement mit vom Benutzer ausgewählten roten letzten Strichen |

 

Um zusätzlich zur Freihandeingabe und zum Löschen weitere Funktionen wie etwa die Strichauswahl bereitzustellen, muss die App bestimmte Eingaben identifizieren, die vom [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt ohne Verarbeitung zur Behandlung an die App weitergegeben werden.

## Weitergabe der Eingabe für die erweiterte Verarbeitung


Standardmäßig verarbeitet [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) sämtliche Eingaben als letzten Strich oder ausradierten Strich. Hierzu zählen auch Eingaben, die durch ein sekundäres Hardwareangebot wie etwa eine Zeichenstift-Drucktaste, eine rechte Maustaste oder ein ähnliches Element geändert werden.

Wenn Benutzer diese sekundären Angebote verwenden, erwarten sie in der Regel zusätzliche Funktionalität oder ein geändertes Verhalten.

In einigen Fällen müssen Sie eventuell basierend auf der Benutzerauswahl auf der App-UI grundlegende Freihandfunktionen für Stifte ohne sekundäre Angebote (Funktionalität, die in der Regel nicht der Stiftspitze zugeordnet ist), andere Eingabegerätetypen, zusätzliche Funktionalität oder geändertes Verhalten verfügbar machen.

Das [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt kann zu diesem Zweck so konfiguriert werden, dass bestimmte Eingaben unverarbeitet bleiben. Diese unverarbeiteten Eingaben werden dann zur Verarbeitung an die App weitergegeben.

Im folgenden Codebeispiel werden die Schritte zum Aktivieren der Strichauswahl gezeigt, wenn die Eingabe mit einer Zeichenstift-Drucktaste (oder rechten Maustaste) geändert wird.

In diesem Beispiel verwenden wir die Dateien „MainPage.xaml“ und „MainPage.Xaml.cs“, um den gesamten Code zu hosten.

1.  Zunächst richten wir in „MainPage.xaml“ die Benutzeroberfläche ein.

    Hier fügen wir einen Zeichenbereich (unterhalb des [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements) zum Zeichnen der Strichauswahl hinzu. Durch die Verwendung einer eigenen Ebene zum Zeichnen der Strichauswahl bleiben das **InkCanvas**-Steuerelement und dessen Inhalt unverändert.

    ![Das leere InkCanvas-Steuerelement mit einem darunterliegenden Auswahlzeichenbereich](images/ink-unprocessed-1-small.png)
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Advanced ink customization sample" 
                       VerticalAlignment="Center"
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
        </StackPanel>
        <Grid Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
        </Grid>
    </Grid>
```

2.  In „MainPage.xaml.cs“ deklarieren wir eine Reihe von globalen Variablen zum Speichern von Verweisen auf Aspekte der Auswahl-UI. Dies gilt insbesondere für den Auswahllassostrich und das umgebende Rechteck, das die ausgewählten Striche hervorhebt.
```    CSharp
// Stroke selection tool.
    private Polyline lasso;
    // Stroke selection area.
    private Rect boundingRect;
```

3.  Anschließend konfigurieren wir das [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt so, dass es Eingabedaten von Stift und Maus als Freihandstriche interpretiert. Zudem legen wir anfängliche Freihandstrichattribute zum Rendern von Freihandstrichen mit dem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement fest.

    Vor allem geben wir mithilfe der [**InputProcessingConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948764)-Eigenschaft des [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekts an, dass alle geänderten Eingaben von der App verarbeitet werden sollen. Geänderte Eingaben geben wir an, indem wir **InputProcessingConfiguration.RightDragAction** den Wert [**InkInputRightDragAction.LeaveUnprocessed**](https://msdn.microsoft.com/library/windows/apps/dn948760) zuweisen.

    Anschließend weisen wir Listener für die nicht verarbeiteten Ereignisse [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) und [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) zu, die vom [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt weitergegeben werden. Sämtliche Auswahlfunktionalität wird in den Handlern für diese Ereignisse implementiert.

    Schließlich weisen wir Listener für die Ereignisse [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) und [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) des [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekts zu. Wir verwenden die Handler für diese Ereignisse, um die Auswahl-UI zu bereinigen, wenn ein neuer Strich begonnen oder ein vorhandener Strich ausradiert wird.

    ![InkCanvas mit standardmäßigen schwarzen letzten Strichen](images/ink-unprocessed-2-small.png)
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // By default, the InkPresenter processes input modified by 
        // a secondary affordance (pen barrel button, right mouse 
        // button, or similar) as ink.
        // To pass through modified input to the app for custom processing 
        // on the app UI thread instead of the background ink thread, set 
        // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
        inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction = 
            InkInputRightDragAction.LeaveUnprocessed;

        // Listen for unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        inkCanvas.InkPresenter.UnprocessedInput.PointerPressed += 
            UnprocessedInput_PointerPressed;
        inkCanvas.InkPresenter.UnprocessedInput.PointerMoved += 
            UnprocessedInput_PointerMoved;
        inkCanvas.InkPresenter.UnprocessedInput.PointerReleased += 
            UnprocessedInput_PointerReleased;

        // Listen for new ink or erase strokes to clean up selection UI.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted += 
            StrokeInput_StrokeStarted;
        inkCanvas.InkPresenter.StrokesErased += 
            InkPresenter_StrokesErased;
    }
```

4.  Anschließend definieren wir Handler für die nicht verarbeiteten Ereignisse [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) und [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713), die vom [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt weitergegeben werden.

    Sämtliche Auswahlfunktionalität, einschließlich des Lassostrichs und des umgebenden Rechtecks, wird in diesen Handlern implementiert.

    ![Auswahllasso](images/ink-unprocessed-3-small.png)
```    CSharp
// Handle unprocessed pointer events from modifed input.
    // The input is used to provide selection functionality.
    // Selection UI is drawn on a canvas under the InkCanvas.
    private void UnprocessedInput_PointerPressed(
        InkUnprocessedInput sender, PointerEventArgs args)
    {
        // Initialize a selection lasso.
        lasso = new Polyline()
        {
            Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
            StrokeThickness = 1,
            StrokeDashArray = new DoubleCollection() { 5, 2 },
        };

        lasso.Points.Add(args.CurrentPoint.RawPosition);

        selectionCanvas.Children.Add(lasso);
    }

    private void UnprocessedInput_PointerMoved(
        InkUnprocessedInput sender, PointerEventArgs args)
    {
        // Add a point to the lasso Polyline object.
        lasso.Points.Add(args.CurrentPoint.RawPosition);
    }

    private void UnprocessedInput_PointerReleased(
        InkUnprocessedInput sender, PointerEventArgs args)
    {
        // Add the final point to the Polyline object and 
        // select strokes within the lasso area.
        // Draw a bounding box on the selection canvas 
        // around the selected ink strokes.
        lasso.Points.Add(args.CurrentPoint.RawPosition);

        boundingRect = 
            inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                lasso.Points);

        DrawBoundingRect();
    }
```

5.  Um den PointerReleased-Ereignishandler abzuschließen, löschen wir sämtlichen Inhalt (den Lassostrich) aus der Auswahlebene und zeichnen dann ein einzelnes umgebendes Rechteck um die letzten Striche, die sich im Lassobereich befinden.

    ![Das umgebende Auswahlrechteck](images/ink-unprocessed-4-small.png)
```    CSharp
// Draw a bounding rectangle, on the selection canvas, encompassing 
    // all ink strokes within the lasso area.
    private void DrawBoundingRect()
    {
        // Clear all existing content from the selection canvas.
        selectionCanvas.Children.Clear();

        // Draw a bounding rectangle only if there are ink strokes 
        // within the lasso area.
        if (!((boundingRect.Width == 0) || 
            (boundingRect.Height == 0) || 
            boundingRect.IsEmpty))
        {
            var rectangle = new Rectangle()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
                Width = boundingRect.Width,
                Height = boundingRect.Height
            };

            Canvas.SetLeft(rectangle, boundingRect.X);
            Canvas.SetTop(rectangle, boundingRect.Y);

            selectionCanvas.Children.Add(rectangle);
        }
    }
```

6.  Schließlich definieren wir Handler für die InkPresenter-Ereignisse [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) und [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767).

    Diese beiden rufen einfach die gleiche Bereinigungsfunktion auf, um bei jeder Erkennung eines neuen Strichs die aktuelle Auswahl zu löschen.
```    CSharp
// Handle new ink or erase strokes to clean up selection UI.
    private void StrokeInput_StrokeStarted(
        InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
    {
        ClearSelection();
    }

    private void InkPresenter_StrokesErased(
        InkPresenter sender, InkStrokesErasedEventArgs args)
    {
        ClearSelection();
    }
```

7.  Dies ist die Funktion zum Entfernen der gesamten Auswahl-UI aus dem Auswahlzeichenbereich, wenn ein neuer Strich begonnen oder ein vorhandener Strich ausradiert wird.
```    CSharp
// Clean up selection UI.
    private void ClearSelection()
    {
        var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        foreach (var stroke in strokes)
        {
            stroke.Selected = false;
        }
        ClearDrawnBoundingRect();
    }

    private void ClearDrawnBoundingRect()
    {
        if (selectionCanvas.Children.Any())
        {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
        }
    }
```

## Benutzerdefiniertes Rendern von Freihandeingaben


Standardmäßig werden Freihandeingaben in einem Hintergrundthread mit geringer Wartezeit verarbeitet und während des Zeichnens „nass“ gerendert. Wenn der Strich abgeschlossen ist (der Stift oder Finger wurde angehoben oder die Maustaste losgelassen), wird er im UI-Thread verarbeitet und auf der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Ebene „trocken“ gerendert (über dem Anwendungsinhalt, wo er die nasse Freihandeingabe ersetzt).

Die Freihandplattform ermöglicht es Ihnen, dieses Verhalten zu überschreiben und die Freihandfunktionen durch benutzerdefiniertes Trocknen der Freihandeingabe umfassend anzupassen.

Benutzerdefiniertes Trocknen erfordert anstelle des standardmäßigen [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements ein [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263)-Objekt, um die Freihandeingabe zu verwalten und im Direct2D-Gerätekontext der universellen Windows-App zu rendern.

Eine App erstellt durch Aufruf von [**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012) (vor dem Laden des [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements) ein [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979)-Objekt, um zu definieren, wie ein letzter Strich trocken in einer [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)- oder [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)-Klasse gerendert wird. Beispielsweise kann ein letzter Strich gerastert und in den Anwendungsinhalt integriert werden, statt auf einer separaten **InkCanvas**-Ebene gerendert zu werden.

Ein vollständiges Beispiel für diese Funktionalität finden Sie unter [Komplexes Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620314).


## Andere Artikel in diesem Abschnitt 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Erkennen von letzten Strichen](convert-ink-to-text.md)</p></td>
<td align="left"><p>Konvertieren Sie letzte Striche mit der Schrifterkennung in Text oder mit der benutzerdefinierten Erkennung in Formen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Speichern und Abrufen von letzten Strichen](save-and-load-ink.md)</p></td>
<td align="left"><p>Speichern Sie letzte Striche mithilfe eingebetteter serialisierter Freihandformat-Metadaten (Ink Serialized Format, ISF) in einer GIF-Datei (Graphics Interchange Format).</p></td>
</tr>
</tbody>
</table>

 


## Verwandte Artikel


* [Behandeln von Zeigereingaben](handle-pointer-input.md)
* [Identifizieren von Eingabegeräten](identify-input-devices.md)

**Beispiele**
* [Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Einfaches Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Komplexes Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Beispiel für grundlegende Eingabe](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Beispiel für Eingabe mit niedriger Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoom](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Gesten und Manipulationen mit GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 







<!--HONumber=Jun16_HO5-->


