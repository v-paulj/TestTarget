---
author: scottmill
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
title: "Systemeigene DirectX- und Direct2D-Interoperabilität mit „BeginDraw“ und „EndDraw“"
description: "Die Windows.UI.Composition-API bietet systemeigene Interoperabilitätsschnittstellen, mit deren Hilfe Inhalte direkt in den Kompositor verschoben werden können."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: c2086e703e3972d4dd38dc1b7147bfa5f01231cf

---
# Systemeigene DirectX- und Direct2D-Interoperabilität mit „BeginDraw“ und „EndDraw“

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Die Windows.UI.Composition-API bietet die systemeigenen Interoperabilitätsschnittstellen [**ICompositorInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620068), [**ICompositionDrawingSurfaceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620058) und [**ICompositionGraphicsDeviceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620065), mit deren Hilfe Inhalte direkt in den Kompositor verschoben werden können.

Systemeigene Interoperabilität ist um Oberflächenelemente herum strukturiert, die von DirectX-Texturen unterstützt werden. Die Oberflächen werden aus einem Factoryobjekt mit der Bezeichnung [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) erstellt. Dieses Objekt wird durch ein zugrunde liegendes Direct2D- oder Direct3D-Geräteobjekt unterstützt, mit dem Videospeicher für Oberflächen zugewiesen wird. Die Composition-API erstellt niemals das zugrunde liegende DirectX-Gerät. Es wird von der Anwendung erstellt und an das **CompositionGraphicsDevice**-Objekt übergeben. Eine Anwendung kann gleichzeitig mehrere **CompositionGraphicsDevice**-Objekte erstellen und das gleiche DirectX-Gerät als Renderinggerät für mehrere **CompositionGraphicsDevice**-Objekte verwenden.

## Erstellen einer Oberfläche

Jedes [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)-Objekt dient als Oberflächen-Factory. Jede Oberfläche wird mit einer vorgegebenen Größe (z.B. 0,0), aber ohne gültige Pixel erstellt. Eine Oberfläche kann in ihrem ursprünglichen Zustand sofort in einer visuellen Struktur verwendet werden, z. B. über ein [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415)- und ein [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433)-Element. Sie hat jedoch in ihrem ursprünglichen Zustand keine Auswirkung auf die Bildschirmausgabe. Sie wird immer vollständig transparent angezeigt, auch wenn der Alphamodus als „undurchsichtig“ angegeben ist.

Gelegentlich können DirectX-Geräte unbrauchbar werden. Dies geschieht beispielsweise, wenn die Anwendung bestimmten DirectX-APIs ungültige Argumente übergibt, der Grafikadapter vom System zurückgesetzt wird oder der Treiber aktualisiert wird. Direct3D verfügt über eine API, die von einer Anwendung asynchron zur Erkennung verwendet werden kann, wenn das Gerät aus irgendeinem Grund verloren geht. Wenn ein DirectX-Gerät verloren gegangen ist, muss die Anwendung es verwerfen, ein neues erstellen und es an ein beliebiges [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)-Objekt übergeben, das zuvor dem unbrauchbaren DirectX-Gerät zugeordnet war.

## Laden von Pixeln in eine Oberfläche

Um Pixel in eine Oberfläche zu laden, muss die Anwendung die [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx)-Methode aufrufen, die eine DirectX-Oberfläche zurückgibt. Abhängig von der Anforderung der Anwendung handelt es sich hierbei um eine Textur oder einen Direct2D-Kontext. Die Anwendung muss dann Pixel in die Textur rendern oder laden. Anschließend ruft sie die [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060)-Methode auf. Erst dann stehen die neuen Pixel für die Komposition zur Verfügung. Sie werden allerdings erst auf dem Bildschirm angezeigt, wenn alle Änderungen an der visuellen Struktur gespeichert wurden. Falls die visuelle Struktur vor dem Aufrufen von **EndDraw** gespeichert wird, ist das gerade ausgeführte Update nicht auf dem Bildschirm sichtbar. Auf der Benutzeroberfläche werden stattdessen die Inhalte angezeigt, die vor dem Aufruf von **BeginDraw** vorhanden waren. Wenn **EndDraw** aufgerufen wird, wird die von „BeginDraw“ zurückgegebene Textur oder der Direct2D-Kontextzeiger ungültig. Eine Anwendung sollte diesen Zeiger niemals über den Aufruf von **EndDraw** hinaus zwischenspeichern.

Die Anwendung kann „BeginDraw“ nur jeweils auf einer Oberfläche für ein bestimmtes [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749)-Element aufrufen. Nach dem Aufruf von [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) muss die Anwendung [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) auf dieser Oberfläche aufrufen, bevor sie auf einer anderen **BeginDraw** aufrufen kann. Da die API agil ist, muss die Anwendung diese Aufrufe synchronisiert durchführen, wenn das Rendering aus mehreren Workerthreads ausgeführt werden soll. Mit der [**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx) kann die Anwendung das Rendern einer Oberfläche unterbrechen und vorübergehend zu einer anderen wechseln. Dadurch kann eine weitere **BeginDraw**-Methode erfolgreich ausgeführt werden, während das erste Update der Oberfläche nicht für die Komposition auf dem Bildschirm bereitgestellt wird. So kann die Anwendung mehrere Transaktionsupdates ausführen. Sobald eine Oberfläche angehalten wird, kann die Anwendung das Update fortführen, indem sie die [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062)-Methode aufruft. Alternativ kann das Update durch Aufrufen von **EndDraw** als beendet erklärt werden. Dies bedeutet, dass für ein bestimmtes **CompositionGraphicsDevice**-Element jeweils nur eine Oberfläche gleichzeitig aktiv aktualisiert werden kann. Jedes Grafikgerät behält diesen Zustand unabhängig von den anderen bei, damit eine Anwendung zwei Oberflächen gleichzeitig rendern kann, wenn sie zu unterschiedlichen Grafikgeräten gehören. Allerdings kann der Videospeicher für diese beiden Oberflächen unter diesen Umständen nicht zusammengelegt werden, was eine weniger effiziente Speichernutzung zur Folge hat.

Die Methoden [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx), [**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx), [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) und [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) geben Fehler zurück, wenn die Anwendung einen falschen Vorgang ausführt (z.B. das Übergeben ungültiger Argumente oder das Aufrufen von **BeginDraw** auf einer Oberfläche, bevor auf einer anderen **EndDraw** aufgerufen wurde). Bei diesen Arten von Fehlern handelt es sich um Anwendungsfehler. Daher wird erwartet, dass sie mit der Fail-Fast-Funktion verarbeitet werden. 
              **BeginDraw** gibt möglicherweise auch einen Fehler zurück, wenn das zugrunde liegende DirectX-Gerät verloren geht. Dieser Fehler ist nicht schwerwiegend, da die Anwendung das DirectX-Gerät neu erstellen und den Vorgang wiederholen kann. Es wird also erwartet, dass die Anwendung verloren gegangene Geräte behandelt, indem Sie das Rendering einfach überspringt. Falls **BeginDraw** aus irgendeinem Grund fehlschlägt, sollte die Anwendung nicht **EndDraw** aufrufen, da „BeginDraw“ gar nicht erst erfolgreich ausgeführt wurde.

## Bildlauf

Wenn eine Anwendung [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) aufruft, entsprechen die Inhalte der zurückgegebenen Textur infolge der Leistungsoptimierung nicht zwangsläufig den vorherigen Inhalten der Oberfläche. Die Anwendung muss davon ausgehen, dass die Inhalte zufällig sind, und daher sicherstellen, dass alle Pixel berührt wurden. Löschen Sie zu diesem Zweck die Oberfläche vor dem Rendern, oder zeichnen Sie genügend undurchsichtigen Inhalt, um das gesamte aktualisierte Rechteck abzudecken. Aufgrund dessen und der Tatsache, dass der Texturzeiger nur zwischen **BeginDraw**- and [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060)-Aufrufen gültig ist, kann die Anwendung bereits vorhandenen Inhalt nicht aus der Oberfläche kopieren. Aus diesem steht eine [**Scroll**](https://msdn.microsoft.com/library/windows/apps/mt620063)-Methode zur Verfügung, mit der die Anwendung eine Kopie der Pixel der gleichen Oberfläche erstellen kann.

## Anwendungsbeispiel

Das folgende Beispiel zeigt ein sehr einfaches Szenario, in dem eine Anwendung Zeichenoberflächen erstellt und [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) sowie [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) verwendet, um die Oberflächen mit Text zu füllen. Die Anwendung verwendet DirectWrite für das Layout des Texts (Details nicht dargestellt) und dann Direct2D zum Rendern. Das Grafikgerät für die Komposition akzeptiert das Direct2D-Gerät direkt zum Zeitpunkt der Initialisierung. Dadurch kann **BeginDraw** einen ID2D1DeviceContext-Schnittstellenzeiger zurückzugeben. Dies ist wesentlich effizienter, als die Anwendung bei jedem Zeichenvorgang einen Direct2D-Kontext erstellen zu lassen, um eine zurückgegebene ID3D11Texture2D-Schnittstelle zu umschließen.

```cpp
//------------------------------------------------------------------------------
//
// Copyright (C) Microsoft. All rights reserved.
//
//------------------------------------------------------------------------------

#include "stdafx.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::Graphics::DirectX;
using namespace Windows::UI::Composition;

// This is an app-provided helper to render lines of text
class SampleText
{
private:
    // The text to draw
    ComPtr<IDWriteTextLayout> _text;

    // The composition surface that we use in the visual tree
    ComPtr<ICompositionDrawingSurfaceInterop> _drawingSurfaceInterop;

    // The device that owns the surface
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;

    // For managing our event notifier
    EventRegistrationToken _deviceReplacedEventToken;

public:
    SampleText(IDWriteTextLayout* text, ICompositionGraphicsDevice* compositionGraphicsDevice) :
        _text(text),
        _compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        FailFastOnFailure(text->GetMetrics(&metrics));
        Windows::Foundation::Size surfaceSize = { metrics.width, metrics.height };
        ComPtr<ICompositionDrawingSurface> drawingSurface;
        FailFastOnFailure(_compositionGraphicsDevice->CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::DirectXPixelFormat_B8G8R8A8UIntNormalized,
            DirectXAlphaMode::DirectXAlphaMode_Ignore,
            &drawingSurface));

        // Cache the interop pointer, since that's what we always use.
        FailFastOnFailure(drawingSurface.As(&_drawingSurfaceInterop));

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        FailFastOnFailure(_compositionGraphicsDevice->add_RenderingDeviceReplaced(
            Callback<RenderingDeviceReplacedEventHandler>([this](
                ICompositionGraphicsDevice* source, IRenderingDeviceReplacedEventArgs* args)
                -> HRESULT
            {
                // Draw the text again
                DrawText();
                return S_OK;
            }).Get(),
            &_deviceReplacedEventToken));
    }

    ~SampleText()
    {
        FailFastOnFailure(_compositionGraphicsDevice->remove_RenderingDeviceReplaced(
            _deviceReplacedEventToken));
    }

    // Return the underlying surface to the caller
    ComPtr<ICompositionSurface> get_Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead
        ComPtr<ICompositionSurface> surface;
        FailFastOnFailure(_drawingSurfaceInterop.As(&surface));
        return surface;
    }

private:
    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (SUCCEEDED(hr))
        {
            // Everything is fine -- go ahead and draw
            return true;
        }
        else if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated
            return false;
        }
        else
        {
            // Any other error is unexpected and, therefore, fatal
            FailFast();
        }
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        ComPtr<ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), &d2dDeviceContext, &offset)))
        {
            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            ComPtr<ID2D1SolidColorBrush> brush;
            FailFastOnFailure(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), &brush));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F(offset.x, offset.y), _text.Get(),
                brush.Get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            FailFastOnFailure(_drawingSurfaceInterop->EndDraw());
        }
    }
};

class SampleApp
{
    ComPtr<ICompositor> _compositor;
    ComPtr<ID2D1Device> _d2dDevice;
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;
    std::vector<ComPtr<SampleText>> _textSurfaces;

public:
    // Run once when the application starts up
    void Initialize(ICompositor* compositor)
    {
        // Cache the compositor (created outside of this method)
        _compositor = compositor;

        // Create a Direct2D device (helper implementation not shown here)
        FailFastOnFailure(CreateDirect2DDevice(&_d2dDevice));

        // To create a composition graphics device, we need to QI for another interface
        ComPtr<ICompositorInterop> compositorInterop;
        FailFastOnFailure(_compositor.As(&compositorInterop));

        // Create a graphics device backed by our D3D device
        FailFastOnFailure(compositorInterop->CreateGraphicsDevice(
            _d2dDevice.Get(),
            &_compositionGraphicsDevice));
    }

    // Called when Direct3D signals the device lost event
    void OnDirect3DDeviceLost()
    {
        // Create a new device
        FailFastOnFailure(CreateDirect2DDevice(_d2dDevice.ReleaseAndGetAddressOf()));

        // Restore our composition graphics device to good health
        ComPtr<ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop;
        FailFastOnFailure(_compositionGraphicsDevice.As(&compositionGraphicsDeviceInterop));
        FailFastOnFailure(compositionGraphicsDeviceInterop->SetRenderingDevice(_d2dDevice.Get()));
    }

    // Create a surface that is asynchronously filled with an image
    ComPtr<ICompositionSurface> CreateSurfaceFromTextLayout(IDWriteTextLayout* text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here)
        SampleText* textSurface = new SampleText(text, _compositionGraphicsDevice.Get());

        // Keep our image alive
        _textSurfaces.push_back(textSurface);

        // The caller is only interested in the underlying surface
        return textSurface->get_Surface();
    }

    // Create a visual that holds an image
    ComPtr<IVisual> CreateVisualFromTextLayout(IDWriteTextLayout* text)
    {
        // Create a sprite visual
        ComPtr<ISpriteVisual> spriteVisual;
        FailFastOnFailure(_compositor->CreateSpriteVisual(&spriteVisual));

        // The sprite visual needs a brush to hold the image
        ComPtr<ICompositionSurfaceBrush> surfaceBrush;
        FailFastOnFailure(_compositor->CreateSurfaceBrushWithSurface(
            CreateSurfaceFromTextLayout(text).Get(),
            &surfaceBrush));

        // Associate the brush with the visual
        ComPtr<ICompositionBrush> brush;
        FailFastOnFailure(surfaceBrush.As(&brush));
        FailFastOnFailure(spriteVisual->put_Brush(brush.Get()));

        // Return the visual to the caller as the base class
        ComPtr<IVisual> visual;
        FailFastOnFailure(spriteVisual.As(&visual));

        return visual;
    }

private:
    // This helper (implementation not shown here) creates a Direct2D device and registers
    // for a device loss notification on the underlying Direct3D device. When that notification is
    // raised, assume the OnDirect3DDeviceLost method is called.
    HRESULT CreateDirect2DDevice(ID2D1Device** ppDevice);
};
```

 

 







<!--HONumber=Jul16_HO2-->


