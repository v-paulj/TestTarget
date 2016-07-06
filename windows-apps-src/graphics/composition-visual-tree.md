---
author: scottmill
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: Komposition visueller Strukturen
description: "Visuelle Kompositionselemente bilden die visuelle Struktur, die die Grundlage für alle anderen Features der Composition-API bildet und von diesen verwendet wird. Die API ermöglicht es Entwicklern, visuelle Objekte zu definieren und zu erstellen, die jeweils für einen einzelnen Knoten in einer visuellen Struktur stehen."
ms.sourcegitcommit: 8a28765f5451e4303d6204070c38596773cb65b9
ms.openlocfilehash: 61adc6a894c56c6cfd292d89d4cd5c4ba6b0d017

---
# Visuelle Kompositionsstruktur

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Visuelle Kompositionselemente bilden die visuelle Struktur, die als Grundlage für alle anderen Features der Composition-API verwendet wird. Die API ermöglicht es Entwicklern, visuelle Objekte zu definieren und zu erstellen, die jeweils für einen einzelnen Knoten in einer visuellen Struktur stehen.

## Visuelle Elemente

Es gibt drei Arten visueller Elemente, aus denen sich die visuelle Struktur zusammensetzt, sowie eine grundlegende Pinselklasse mit mehreren Unterklassen, die Einfluss auf den Inhalt eines visuellen Elements hat:

-   [
              **Visual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706858) – Basisobjekt; umfasst den Großteil der Eigenschaften. Diese werden von anderen visuellen Objekten geerbt.
-   [
              **ContainerVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706810) – Abgeleitet von [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858). Fügt die Fähigkeit zum Erstellen von untergeordneten Elementen hinzu.
-   [
              **SpriteVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Mt589433) – Abgeleitet von [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810). Bietet zudem die Möglichkeit, einen Pinsel zuzuordnen, sodass das Visual-Element Pixel, einschließlich Bilder, Effekte oder Volltonfarbe, rendern kann.
-   [
              **CompositionBrush**
            ](https://msdn.microsoft.com/library/windows/apps/Mt589398) – Ermöglicht die Anwendung eines Effekts auf den Inhalt eines Visual-Elements. Es gibt eine Reihe von Unterklassen von „CompositionBrush“.

## Das CompositionVisual-Beispiel

Im Beispiel sehen Sie eine Reihe farbiger Quadrate, die Sie anklicken und über den Bildschirm ziehen können. Durch Klicken auf ein Quadrat gelangt dieses in den Vordergrund, dreht sich um 45 Grad und wird während der Bewegung undurchsichtig.

Es zeigt einige grundlegenden Konzepte für die Arbeit mit der API, einschließlich:

-   Erstellen eines Kompositors
-   Erstellen eines SpriteVisual-Elements mit „ColorBrush“
-   Beschneiden eines visuellen Elements
-   Drehen eines visuellen Elements
-   Festlegen der Deckkraft
-   Ändern der Position des visuellen Elements in der Auflistung.

Das Beispiel veranschaulicht die Nutzung dreier unterschiedlicher visueller Elemente:

-   [
              **Visual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706858) – Basisobjekt; umfasst den Großteil der Eigenschaften. Diese werden von anderen visuellen Objekten geerbt.
-   [
              **ContainerVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706810) – Abgeleitet von Visual. Fügt die Fähigkeit zum Erstellen von untergeordneten Elementen hinzu.
-   [
              **SpriteVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Mt589433) – Abgeleitet von Visual. Bietet zudem die Möglichkeit, einen Pinsel zuzuordnen, sodass das Visual-Element Pixel, einschließlich Bilder, Effekte oder Volltonfarbe, rendern kann.

Dieses Beispiel veranschaulicht keine Konzepte wie Animationen oder komplexere Effekte. Es enthält die Bausteine, die alle diese Systeme verwenden.

## Erstellen eines Kompositors

Es ist einfach, ein neues [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789)-Objekt zu erstellen und diese für die Verwendung als Factory in einer Variable zu speichern. Der folgende Codeausschnitt veranschaulicht das Erstellen eines neuen **Compositor**-Objekts:

```cs
_compositor = new Compositor();
```

## Erstellen eines „SpriteVisual“- und eines „ColorBrush“-Elements

Mit dem [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789)-Objekt können Sie auf einfache Weise Objekte erstellen, wenn Sie sie benötigen, wie etwa ein [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433)- und ein [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399)-Element:

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

Diese wenigen Zeilen Code veranschaulichen ein leistungsfähiges Konzept. [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433)-Objekte sind das Herzstück des Effektesystems. Das **SpriteVisual**-Element ermöglicht hohe Flexibilität und Interaktion bei der Farb-, Bild- und Effektgestaltung. **SpriteVisual** ist das einzige visuelle Element, das ein 2D-Rechteck mit einem Pinsel füllen kann, in diesem Fall mit einer Volltonfarbe.

## Beschneiden eines visuellen Elements

[
            **Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) kann auch zum Beschneiden eines [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekts verwendet werden. Im folgenden Beispiel werden mit [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) die Seiten des visuellen Elements gekürzt:

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

Hinweis: Auf die Eigenschaften von [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) können wie auch auf andere Objekte in der API Animationen angewendet werden.

## <span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>Drehen von Clips

Ein [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekt kann mit einer Drehung transformiert werden. Beachten Sie, dass [**RotationAngle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.rotationangle) Radianten und Grad unterstützt. Der Standardwert ist „Radianten“. Wie im folgenden Codeausschnitt dargestellt, ist es jedoch ganz einfach, einen Wert in Grad anzugeben:

```cs
child.RotationAngleInDegrees = 45.0f;
```

Drehung ist nur ein Beispiel für eine Reihe von Transformationskomponenten, die von der API bereitgestellt werden, um diese Aufgaben zu vereinfachen. Dazu gehören auch Offset, Skalieren, Ausrichtung, Drehachse und eine 4x4-Transformationsmatrix.

## Festlegen der Deckkraft

Das Festlegen der Deckkraft eines visuellen Elements ist mit einem Float-Wert unproblematisch. In diesem Beispiel haben alle Quadrate anfangs eine Deckkraft von 0,8:

```cs
visual.Opacity = 0.8f;
```

Wie die Drehung kann auch die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.opacity)-Eigenschaft animiert werden.

## Ändern der Position des visuellen Elements in der Auflistung

Die Composition-API ermöglicht es, dass die Position eines visuellen Elements in [**VisualCollection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection) auf vielfältige Weise geändert werden kann. Es kann mit [**InsertAbove**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertabove) über einem anderen visuellen Element und mit [**InsertBelow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertbelow) darunter platziert werden. An die oberste Position wird es mit [**InsertAtTop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertattop) und an die unterste mit [**InsertAtBottom**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertatbottom) verschoben.

In dem Beispiel wird ein [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekt, auf das geklickt wurde, oben platziert:

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## Vollständiges Beispiel

Im vollständigen Beispiel werden alle oben beschriebenen Konzepte zusammen verwendet, um eine einfache Struktur von [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekten zu erstellen und vorzuführen. Die Deckkraft wird ohne Verwendung von XAML, WWA oder DirectX geändert. Dieses Beispiel zeigt, wie untergeordnete **Visual**-Objekte erstellt und hinzugefügt und Eigenschaften geändert werden.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using Windows.ApplicationModel.Core;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Core;

namespace compositionvisual
{
    class VisualProperties : IFrameworkView
    {
        //------------------------------------------------------------------------------
        //
        // VisualProperties.Initialize
        //
        // This method is called during startup to associate the IFrameworkView with the
        // CoreApplicationView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Initialize(CoreApplicationView view)
        {
            _view = view;
            _random = new Random();
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.SetWindow
        //
        // This method is called when the CoreApplication has created a new CoreWindow,
        // allowing the application to configure the window and start producing content
        // to display.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.SetWindow(CoreWindow window)
        {
            _window = window;
            InitNewComposition();
            _window.PointerPressed += OnPointerPressed;
            _window.PointerMoved += OnPointerMoved;
            _window.PointerReleased += OnPointerReleased;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerPressed
        //
        // This method is called when the user touches the screen, taps it with a stylus
        // or clicks the mouse.
        //
        //------------------------------------------------------------------------------

        void OnPointerPressed(CoreWindow window, PointerEventArgs args)
        {
            Point position = args.CurrentPoint.Position;

            //
            // Walk our list of visuals to determine who, if anybody, was selected
            //
            foreach (var child in _root.Children)
            {
                //
                // Did we hit this child?
                //
                Vector3 offset = child.Offset;
                Vector2 size = child.Size;

                if ((position.X >= offset.X) &&
                    (position.X < offset.X + size.X) &&
                    (position.Y >= offset.Y) &&
                    (position.Y < offset.Y + size.Y))
                {
                    //
                    // This child was hit. Since the children are stored back to front,
                    // the last one hit is the front-most one so it wins
                    //
                    _currentVisual = child as ContainerVisual;
                    _offsetBias = new Vector2((float)(offset.X - position.X),
                                              (float)(offset.Y - position.Y));
                }
            }

            //
            // If a visual was hit, bring it to the front of the Z order
            //
            if (_currentVisual != null)
            {
                ContainerVisual parent = _currentVisual.Parent as ContainerVisual;
                parent.Children.Remove(_currentVisual);
                parent.Children.InsertAtTop(_currentVisual);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerMoved
        //
        // This method is called when the user moves their finger, stylus or mouse with
        // a button pressed over the screen.
        //
        //------------------------------------------------------------------------------

        void OnPointerMoved(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual is selected, drag it with the pointer position and
            // make it opaque while we drag it
            //
            if (_currentVisual != null)
            {
                //
                // Set up the properties of the visual the first time it is
                // dragged. This will last for the duration of the drag
                //
                if (!_dragging)
                {
                    _currentVisual.Opacity = 1.0f;

                    //
                    // Transform the first child of the current visual so that
                    // the image is rotated
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngleInDegrees = 45.0f;
                        child.CenterPoint = new Vector3(_currentVisual.Size.X / 2, _currentVisual.Size.Y / 2, 0);
                        break;
                    }

                    //
                    // Clip the visual to its original layout rect by using an inset
                    // clip with a one-pixel margin all around
                    //
                    var clip = _compositor.CreateInsetClip();
                    clip.LeftInset = 1.0f;
                    clip.RightInset = 1.0f;
                    clip.TopInset = 1.0f;
                    clip.BottomInset = 1.0f;
                    _currentVisual.Clip = clip;

                    _dragging = true;
                }

                Point position = args.CurrentPoint.Position;
                _currentVisual.Offset = new Vector3((float)(position.X + _offsetBias.X),
                                                    (float)(position.Y + _offsetBias.Y),
                                                    0.0f);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerReleased
        //
        // This method is called when the user lifts their finger or stylus from the
        // screen, or lifts the mouse button.
        //
        //------------------------------------------------------------------------------

        void OnPointerReleased(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual was selected, make it transparent again when it is
            // released and restore the transform and clip
            //
            if (_currentVisual != null)
            {
                if (_dragging)
                {
                    //
                    // Remove the transform from the first child
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngle = 0.0f;
                        child.CenterPoint = new Vector3(0.0f, 0.0f, 0.0f);
                        break;
                    }

                    _currentVisual.Opacity = 0.8f;
                    _currentVisual.Clip = null;
                    _dragging = false;
                }

                _currentVisual = null;
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Load
        //
        // This method is called when a specific page is being loaded in the
        // application.  It is not used for this application.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Load(string unused)
        {

        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Run
        //
        // This method is called by CoreApplication.Run() to actually run the
        // dispatcher's message pump.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Run()
        {
            _window.Activate();
            _window.Dispatcher.ProcessEvents(CoreProcessEventsOption.ProcessUntilQuit);
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Uninitialize
        //
        // This method is called during shutdown to disconnect the CoreApplicationView,
        // and CoreWindow from the IFrameworkView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Uninitialize()
        {
            _window = null;
            _view = null;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.InitNewComposition
        //
        // This method is called by SetWindow(), where we initialize Composition after
        // the CoreWindow has been created.
        //
        //------------------------------------------------------------------------------

        void InitNewComposition()
        {
            //
            // Set up Windows.UI.Composition Compositor, root ContainerVisual, and associate with
            // the CoreWindow.
            //

            _compositor = new Compositor();

            _root = _compositor.CreateContainerVisual();



            _compositionTarget = _compositor.CreateTargetForCurrentView();
            _compositionTarget.Root = _root;

            //
            // Create a few visuals for our window
            //
            for (int index = 0; index < 20; index++)
            {
                _root.Children.InsertAtTop(CreateChildElement());
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.CreateChildElement
        //
        // Creates a small sub-tree to represent a visible element in our application.
        //
        //------------------------------------------------------------------------------

        Visual CreateChildElement()
        {
            //
            // Each element consists of three visuals, which produce the appearance
            // of a framed rectangle
            //
            var element = _compositor.CreateContainerVisual();
            element.Size = new Vector2(100.0f, 100.0f);

            //
            // Position this visual randomly within our window
            //
            element.Offset = new Vector3((float)(_random.NextDouble() * 400), (float)(_random.NextDouble() * 400), 0.0f);

            //
            // The outer rectangle is always white
            //
            //Note to preview API users - SpriteVisual and Color Brush replace SolidColorVisual
            //for example instead of doing 
            //var visual = _compositor.CreateSolidColorVisual() and 
            //visual.Color = Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF);
            //we now use the below

            var visual = _compositor.CreateSpriteVisual();
            element.Children.InsertAtTop(visual);
            visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
            visual.Size = new Vector2(100.0f, 100.0f);

            //
            // The inner rectangle is inset from the outer by three pixels all around
            //
            var child = _compositor.CreateSpriteVisual();
            visual.Children.InsertAtTop(child);
            child.Offset = new Vector3(3.0f, 3.0f, 0.0f);
            child.Size = new Vector2(94.0f, 94.0f);

            //
            // Pick a random color for every rectangle
            //
            byte red = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte green = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte blue = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            child.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, red, green, blue));

            //
            // Make the subtree root visual partially transparent. This will cause each visual in the subtree
            // to render partially transparent, since a visual's opacity is multiplied with its parent's
            // opacity
            //
            element.Opacity = 0.8f;

            return element;
        }

        // CoreWindow / CoreApplicationView
        private CoreWindow _window;
        private CoreApplicationView _view;

        // Windows.UI.Composition
        private Compositor _compositor;
        private CompositionTarget _compositionTarget;
        private ContainerVisual _root;
        private ContainerVisual _currentVisual;
        private Vector2 _offsetBias;
        private bool _dragging;

        // Helpers
        private Random _random;
    }


    public sealed class VisualPropertiesFactory : IFrameworkViewSource
    {
        //------------------------------------------------------------------------------
        //
        // VisualPropertiesFactory.CreateView
        //
        // This method is called by CoreApplication to provide a new IFrameworkView for
        // a CoreWindow that is being created.
        //
        //------------------------------------------------------------------------------

        IFrameworkView IFrameworkViewSource.CreateView()
        {
            return new VisualProperties();
        }


        //------------------------------------------------------------------------------
        //
        // main
        //
        //------------------------------------------------------------------------------

        static int Main(string[] args)
        {
            CoreApplication.Run(new VisualPropertiesFactory());

            return 0;
        }
    }
}
```

 

 







<!--HONumber=Jun16_HO5-->


