---
title: Interoperabilität von DirectX und XAML
description: In Ihrem Spiel für die universelle Windows-Plattform (UWP) können Sie eine Kombination aus Extensible Application Markup Language (XAML) und Microsoft DirectX verwenden.
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
---

# Interoperabilität von DirectX und XAML


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In Ihrem Spiel für die universelle Windows-Plattform (UWP) können Sie eine Kombination aus Extensible Application Markup Language (XAML) und Microsoft DirectX verwenden. Dank der Kombination von XAML und DirectX können Sie flexible Frameworks für die Benutzeroberflächen erstellen, die mit den über DirectX gerenderten Inhalten kompatibel sind. Dies ist besonders für grafisch aufwendige Apps von Vorteil. In diesem Thema beschäftigen wir uns mit der Struktur einer UWP-App mit DirectX und gehen auf wichtige Typen ein, die beim Erstellen Ihrer UWP-App für die Zusammenarbeit mit DirectX erforderlich sind.

> **Hinweis**  DirectX-APIs sind nicht als Windows-Runtime-Typen definiert. Daher verwenden Sie in der Regel Visual C++-Komponentenerweiterungen (C++/CX), um mit DirectX kompatible XAMLUWP-Komponenten zu entwickeln. Sie können UWP-DirectX-Apps auch mit C# und XAML erstellen, indem Sie die DirectX-Aufrufe in eine separate Windows-Runtime-Metadatendatei einschließen.

 

## XAML und DirectX


DirectX bietet mit Direct2D und Microsoft Direct3D zwei leistungsstarke Bibliotheken für 2D- und 3D-Grafiken. Obwohl XAML grundlegende 2D-Grundtypen und -Effekte unterstützt, erfordern zahlreiche Apps, wie Modellierungs-Apps und Spiele, eine komplexere Grafikunterstützung. In diesen Fällen können Sie Grafiken teilweise oder vollständig mit Direct2D und Direct3D rendern und XAML für alles andere verwenden.

Für das Szenario mit gemeinsamer Verwendung von XAML und DirectX müssen Sie mit den folgenden beiden Begriffen vertraut sein:

-   Bei gemeinsam genutzten Flächen (Shared Surfaces) handelt es sich um Anzeigebereiche bestimmter Größe, die von XAML definiert werden. In diesen Bereichen können Sie mit DirectX und [**Windows::UI::Xaml::Media::Brush**](https://msdn.microsoft.com/library/windows/apps/br228076)-Typen indirekt zeichnen. Bei gemeinsam genutzten Flächen steuern Sie keine Aufrufe zum Präsentieren der Swapchain(s). Änderungen an den gemeinsam genutzten Flächen werden mit den Updates des XAML-Frameworks synchronisiert.
-   Der zweite Begriff ist die Swapchain selbst. Sie liefert den Hintergrundpuffer für die DirectX-Rendering-Pipeline, also den Speicherbereich, der für die Anzeige präsentiert wird, nachdem das Renderziel fertiggestellt wurde.

Wägen Sie ab, wofür Sie DirectX verwenden. Wird es für die Zusammenstellung und Animierung eines einzelnen Steuerelements verwendet, das in die Abmessungen des Anzeigefensters passt? Kann die zusammengefasste Oberfläche von anderen Oberflächen oder dem Bildschirmrand ausgeblendet werden? Wird eine Ausgabe enthalten sein, die wie in einem Spiel in Echtzeit gerendert und gesteuert werden muss?

Sobald Sie festgelegt haben, wie DirectX verwendet werden soll, können Sie einen dieser Windows-Runtime-Typen verwenden, um DirectX-Rendering in eine Windows Store-App zu integrieren:

-   Wenn Sie ein statisches Bild erstellen oder ein komplexes Bild in ereignisgesteuerten Intervallen zeichnen möchten, zeichnen Sie mit der Klasse [**Windows::UI::Xaml::Media::Imaging::SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) in eine gemeinsam genutzte Fläche. Dieser Typ ermöglicht die Behandlung einer DirectX-Zeichenoberfläche von bestimmter Größe. In der Regel wird dieser Typ beim Erstellen eines Bilds oder einer Textur als Bitmap verwendet, um in einem Dokument oder als UI-Element angezeigt zu werden. Der Typ ist in Szenarien mit Echtzeitinteraktivität wie hochleistungsfähige Spiele nicht gut geeignet. Dies liegt daran, dass Updates des **SurfaceImageSource**-Objekts mit Updates der XAML-Benutzeroberfläche synchronisiert werden. Somit kann es zu Verzögerungen beim visuellen Feedback kommen, z. B. in Form einer schwankenden Bildrate oder einer wahrgenommenen verzögerten Reaktion auf Echtzeiteingaben. Die Updates sind jedoch noch schnell genug für dynamische Steuerelemente oder Datensimulationen.

    [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)-Grafikobjekte können mit anderen XAML-Benutzeroberflächenelementen zusammengefasst werden. Sie können sie umwandeln oder projizieren, wobei das XAML-Framework alle Deckkraft- oder Z-Index-Werte berücksichtigt.

-   Wenn das Bild größer als der verfügbare Platz auf dem Bildschirm ist und es vom Benutzer geschwenkt und gezoomt werden kann, verwenden Sie die Klasse [**Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050). Dieser Typ ermöglicht die Behandlung einer DirectX-Zeichenoberfläche, die größer als der Bildschirm ist. Wie [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) wird diese Klasse für die dynamische Erstellung eines komplexen Bilds oder Steuerelements verwendet. Und wie **SurfaceImageSource** ist diese Klasse nicht gut für hochleistungsfähige Spiele geeignet. Beispielhafte XAML-Elemente, welche die Klasse **VirtualSurfaceImageSource** verwenden können, sind Kartensteuerelemente oder Viewer für große Dokumente mit hoher Bilddichte.

-   Wenn Sie DirectX verwenden, um Grafiken zu präsentieren, die in Echtzeit oder in regelmäßigen Intervallen mit niedriger Verzögerung aktualisiert werden müssen, verwenden Sie die Klasse [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834). So können Sie Grafiken aktualisieren, ohne dass eine Synchronisierung mit dem Aktualisierungstimer des XAML-Frameworks erforderlich ist. Mit dieser Klasse können Sie direkt auf die Swapchain ([**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)) der Grafikhardware zugreifen und XAML oberhalb des Renderziels anordnen. Dieser Typ ist hervorragend für Spiele und andere DirectX-Apps mit Vollbildansicht geeignet, in denen eine XAML-basierte Benutzeroberfläche erforderlich ist. Wenn Sie diese Vorgehensweise wählen, sollten Sie sich mit DirectX sehr gut auskennen, z. B. in Bezug auf die Bereiche Microsoft DirectX Graphics Infrastructure (DXGI), Direct2D und Direct3D. Weitere Informationen finden Sie unter [Programmieranleitung für Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345).

## SurfaceImageSource


[**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) bietet gemeinsam genutzte Flächen, in die mit DirectX gezeichnet werden kann, und setzt die einzelnen Bestandteile dann zu App-Inhalten zusammen.

Im Folgenden erfahren Sie mehr über die grundlegende Vorgehensweise zum Erstellen und Aktualisieren eines [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)-Objekts im CodeBehind:

1.  Legen Sie die Größe der gemeinsam genutzten Fläche fest, indem Sie die Werte für die Höhe und Breite an den Konstruktor der Klasse [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) übergeben. Sie können ebenfalls festlegen, ob für die gemeinsam genutzte Fläche Alpha-Unterstützung (Deckkraft) erforderlich ist.

    Beispiel:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Rufen Sie einen Zeiger zur Schnittstelle [**ISurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848322) ab. Wandeln Sie das [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)-Objekt als [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) (oder **IUnknown**) um, und rufen Sie **QueryInterface** für dieses auf, um die zugrundeliegende **ISurfaceImageSourceNative**-Implementierung abzurufen. Die für diese Implementierung festgelegten Methoden verwenden Sie dann, um das entsprechende Gerät festzulegen und die Zeichenoperationen auszuführen.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNative> m_sisNative;
    // ...
    IInspectable* sisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);
    sisInspectable->QueryInterface(__uuidof(ISurfaceImageSourceNative), (void **)&m_sisNative);
    ```

3.  Legen Sie das DXGI-Gerät fest, indem Sie die Methode [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) aufrufen und dann das Gerät und den Kontext an [**ISurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325) übergeben. Beispiel:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_sisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Stellen Sie einen Zeiger für das [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565)-Objekt zur Methode [**ISurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) bereit, und zeichnen Sie mit DirectX in dieser Fläche. Es wird nur in dem Bereich gezeichnet, der im Parameter *updateRect* für Updates festgelegt wurde.

    > **Hinweis**   Es kann jeweils nur ein ausstehender [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) Vorgang pro [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527) aktiv sein.

     

    This method returns the point (x,y) offset of the updated target rectangle in the *offset* parameter. You use this offset to determine where to draw into inside the [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565).

    ```cpp
    ComPtr<IDXGISurface> surface;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(updateRect, &surface, &offset);
    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
    {
              // Device changed
    }
    else
    {
        // Draw to IDXGISurface (the surface paramater)
    }
    ```

5.  Rufen Sie die [**ISurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324)-Methode auf, um die Bitmap fertigzustellen. Übergeben Sie die Bitmap an eine [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101)-Klasse.

    ```cpp
    m_sisNative->EndDraw();
    // ...
    // The SurfaceImageSource object's underlying ISurfaceImageSourceNative object contains the completed bitmap.
    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

6.  Verwenden Sie die [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101)-Klasse, um die Bitmap zu zeichnen.

> **Hinweis**   Das Aufrufen von [**SurfaceImageSource::SetSource**](https://msdn.microsoft.com/library/windows/apps/br243255) (geerbt von **IBitmapSource::SetSource**) löst zurzeit eine Ausnahme aus. Rufen Sie es nicht vom [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)-Objekt aus auf.

 

## VirtualSurfaceImageSource


[**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) erweitert [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041), wenn die Inhalte potenziell zu groß sind, um auf dem Bildschirm angezeigt zu werden, und die Inhalte virtualisiert werden müssen, um ein optimales Rendering sicherzustellen.

[**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) unterscheidet sich von [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) durch die Verwendung der Callback-Methode [**IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337). Die Callback-Methode wird implementiert, um bestimmte Bereiche der Fläche zu aktualisieren, sobald sie auf dem Bildschirm angezeigt werden. Somit müssen Sie keine ausgeblendeten Bereiche löschen, da das XAML-Framework diese Aufgabe für Sie übernimmt.

Im Folgenden erfahren Sie mehr über die grundlegende Vorgehensweise zum Erstellen und Aktualisieren eines [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)-Objekts im CodeBehind:

1.  Erstellen Sie eine Instanz der Klasse [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) in der gewünschten Größe. Beispiel:

    `VirtualSurfaceImageSource^ virtualSIS = ref new VirtualSurfaceImageSource(2000, 2000);`

2.  Rufen Sie einen Zeiger zur Schnittstelle [**IVirtualSurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848328) ab. Wandeln Sie das [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)-Objekt in [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) oder [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) um, und rufen Sie [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) für dieses auf, um die zugrundeliegende **IVirtualSurfaceImageSourceNative**-Implementierung abzurufen. Die für diese Implementierung festgelegten Methoden verwenden Sie dann, um das entsprechende Gerät festzulegen und die Zeichenoperationen auszuführen.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    // ...
    IInspectable* vsisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);
    vsisInspectable->QueryInterface(__uuidof(IVirtualSurfaceImageSourceNative), (void **)&m_vsisNative);
    ```

3.  Legen Sie das DXGI-Gerät fest, indem Sie die Methode [**IVirtualSurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325) aufrufen. Beispiel:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Rufen Sie die Methode [**IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334) auf, wodurch eine Referenz zu Ihrer Implementierung der Schnittstelle [**IVirtualSurfaceUpdatesCallbackNative**](https://msdn.microsoft.com/library/windows/desktop/hh848336) übergeben wird.

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }
    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    Daraufhin ruft das Framework Ihre Implementierung der Methode [**IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334) auf, wenn ein Bereich der Klasse [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) aktualisiert werden muss.

    Dieser Fall kann eintreten, wenn das Framework den Bereich bestimmt, der gezeichnet werden muss (etwa wenn der Benutzer die Ansicht der Fläche verschiebt oder zoomt), oder wenn die App die Methode [**IVirtualSurfaceImageSourceNative::Invalidate**](https://msdn.microsoft.com/library/windows/desktop/hh848332) in diesem Bereich aufgerufen hat.

5.  Verwenden Sie innerhalb der Methode [**IVirtualSurfaceImageSourceNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337) die Methoden [**IVirtualSurfaceImageSourceNative::GetUpdateRectCount**](https://msdn.microsoft.com/library/windows/desktop/hh848329) und [**IVirtualSurfaceImageSourceNative::GetUpdateRects**](https://msdn.microsoft.com/library/windows/desktop/hh848330), um zu bestimmen, welche Bereiche der Fläche gezeichnet werden müssen.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  

                  m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);
            std::unique_ptr<RECT[]> drawingBounds(new RECT[drawingBoundsCount]);
            m_vsisNative->GetUpdateRects(drawingBounds.get(), drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  Führen Sie schließlich für jeden Bereich, der aktualisiert werden soll, Folgendes aus:

    1.  Stellen Sie einen Zeiger für das Objekt [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) zur Methode [**IVirtualSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) bereit, und zeichnen Sie mit DirectX in dieser Fläche. Es wird nur in dem Bereich gezeichnet, der im Parameter *updateRect* für Updates festgelegt wurde.

        Wie bei [**IlSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) gibt diese Methode den X-Y-Punkt-Offset für das aktualisierte Zielrechteck im Parameter *offset* zurück. Anhand dieser Information bestimmen Sie die Stelle, an der Sie innerhalb der Schnittstelle [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) zeichnen.

        > **Hinweis**   Es kann jeweils nur ein ausstehender [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) Vorgang pro [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527) aktiv sein.

         

        ```cpp
        ComPtr<IDXGISurface> bigSurface;

        HRESULT beginDrawHR = m_vsisNative->BeginDraw(updateRect, &bigSurface, &offset);
        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
        {
                  // Device changed
        }
        else
        {
            // Draw to IDXGISurface
        }
        ```

    2.  Draw the specific content to that region, but constrain your drawing to the bounded regions for better performance.

    3.  Call [**IVirtualSurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324). The result is a bitmap.

## SwapChainPanel und Gaming


[**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) ist der Windows-Runtime-Typ, der für die Unterstützung von High-End-Grafiken und -Spielen mit direkter Swapchain-Verwaltung entwickelt wurde. Sie erstellen in diesem Fall Ihre eigene DirectX-Swapchain und verwalten die Präsentation Ihrer gerenderten Inhalte selbst. Anschließend können Sie dem **SwapChainPanel**-Objekt XAML-Elemente wie Menüs, Heads-up-Displays und andere UI-Überblendungen hinzufügen.

Es gibt gewisse Einschränkungen für die Klasse [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834), um die bestmögliche Leistungsfähigkeit sicherzustellen:

-   Es sind nicht mehr als 4 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)-Instanzen pro App vorhanden.
-   Die Eigenschaften **Opacity**, **RenderTransform**, **Projection** und **Clip**, die von [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) geerbt werden, werden nicht unterstützt.
-   Höhe und Breite der DirectX-Swapchain (in [**DXGI_SWAP_CHAIN_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) müssen auf die aktuellen Abmessungen des App-Fensters festgelegt werden. Wenn Sie dies nicht vornehmen, werden die Inhalte (unter Verwendung von **DXGI\_SCALING\_STRETCH**) passend skaliert.
-   Sie müssen den Skalierungsmodus der DirectX-Swapchain (in [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) als **DXGI\_SCALING\_STRETCH** festlegen.
-   Sie können den Alphamodus der DirectX-Swapchain (in [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) nicht als **DXGI\_ALPHA\_MODE\_PREMULTIPLIED** festlegen.
-   Sie müssen die DirectX-Swapchain erstellen, indem Sie die Methode [**IDXGIFactory2::CreateSwapChainForComposition**](https://msdn.microsoft.com/library/windows/desktop/hh404558) aufrufen.

Die Klasse [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) wird basierend auf den Anforderungen Ihrer App aktualisiert und nicht entsprechend den Updates des XAML-Frameworks. Wenn Sie die Updates der Klasse **SwapChainPanel** mit denen des XAML-Frameworks synchronisieren möchten, registrieren Sie die Klasse für das Ereignis [**Windows::UI::Xaml::Media::CompositionTarget::Rendering**](https://msdn.microsoft.com/library/windows/apps/br228127). Andernfalls müssen Sie sämtliche threadübergreifenden Probleme berücksichtigen, wenn Sie versuchen, die XAML-Elemente über einen Thread zu aktualisieren, bei dem es sich nicht um den Thread handelt, der die Klasse **SwapChainPanel** aktualisiert.

Es gibt auch eine Reihe von bewährten Methoden für die Verwendung der Klasse [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) bei der App-Entwicklung.

-   [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) erbt von der Klasse [**Windows::UI::Xaml::Controls::Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) und unterstützt ein ähnliches Layoutverhalten. Machen Sie sich mit dem Typ **Grid** und dessen Eigenschaften vertraut.

-   Nachdem eine DirectX-Swapchain festgelegt wurde, arbeiten alle Eingabeereignisse, die für die Klasse [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) ausgelöst wurden, auf die gleiche Weise wie bei allen anderen XAML-Elementen. Für die Klasse **SwapChainPanel** wird kein Hintergrundpinsel festgelegt, und Sie benötigen keine Eingabeereignisse vom [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt der App, wie es in DirectX-Apps der Fall ist, die **SwapChainPanel** nicht verwenden.

-   • Der gesamte Inhalt der visuellen XAML-Elementstruktur unter einem unmittelbar untergeordneten Element einer Klasse des Typs [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) wird auf die Layoutgröße des unmittelbar untergeordneten Elements des Objekts **SwapChainPanel** zugeschnitten. Inhalte, die außerhalb dieser Layoutgrenzen transformiert werden, werden nicht gerendert. Platzieren Sie daher alle animierten XAML-Inhalte, die Sie mit einem XAML-[**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) animieren, in der visuellen Struktur unterhalb eines Elements, dessen Layoutgrenzen groß genug für die vollständige Darstellung der Animation sind.

-   Begrenzen Sie die Zahl der unmittelbaren visuellen XAML-Elemente, die [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) untergeordnet sind. Wenn möglich, gruppieren Sie dicht nebeneinander liegende Elemente unter einem gemeinsamen übergeordneten Element. Allerdings gibt es leistungsbezogene Abhängigkeiten zwischen der Anzahl unmittelbar untergeordneter visueller Elemente und der Größe der untergeordneten Elemente: Zu viele oder unnötig große XAML-Elemente können die Gesamtleistung beeinträchtigen. Erstellen Sie außerdem keine einzelnen untergeordneten Vollbild-XAML-Elemente für die Klasse **SwapChainPanel** Ihrer App, da dies zu Überzeichnungen in der App und Leistungseinbußen führt. Erstellen Sie generell nicht mehr als 8 unmittelbar untergeordnete visuelle XAML-Elemente für die Klasse **SwapChainPanel** Ihrer App. Dabei darf das Layout jedes Elements nur so groß gewählt werden, wie es für die Darstellung der visuellen Inhalte erforderlich ist. Dennoch können Sie die visuelle Elementstruktur unter einem untergeordneten Element der Klasse **SwapChainPanel** ausreichend komplex gestalten, ohne dabei allzu große Leistungseinbußen hinnehmen zu müssen.

> **Hinweis**   Im Allgemeinen sollten Ihre DirectX-Apps Swapchains im Querformat und entsprechend der angezeigten Fenstergröße erstellen (in den meisten Windows Store-Spielen für gewöhnlich die systemeigene Bildschirmauflösung). Dadurch wird sichergestellt, dass Ihre App die optimale Swapchainimplementierung verwendet, wenn sie über keine sichtbaren XAML-Overlays verfügt. Wenn die App in das Hochformat gedreht wird, sollte sie [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) in der vorhandenen Swapchain aufrufen, eine Umwandlung des Inhalts anwenden, wenn erforderlich, und anschließend erneut [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144) auf der gleichen Swapchain aufrufen. Darüber hinaus sollte Ihre App jedes Mal erneut **SetSwapChain** auf der gleichen Swapchain aufrufen, wenn die Größe der Swapchain geändert wird, indem [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577) aufgerufen wird.

 

Im Folgenden erfahren Sie mehr über die grundlegende Vorgehensweise zum Erstellen und Aktualisieren eines [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)-Objekts im CodeBehind:

1.  Rufen Sie eine Instanz eines Swapchainbereichs für Ihre App ab. Die Instanzen sind in XAML mit dem Tag `<SwapChainPanel>` gekennzeichnet.

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    Unten finden Sie ein Beispiel für das Tag `<SwapChainPanel>`.

    ```xaml
    <SwapChainPanel x:Name=&quot;swapChainPanel&quot;>
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width=&quot;300*&quot;/>
            <ColumnDefinition Width=&quot;1069*&quot;/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  Rufen Sie einen Zeiger zur Schnittstelle [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143) ab. Wandeln Sie das [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)-Objekt in [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) (oder **IUnknown**) um, und rufen Sie **QueryInterface** für dieses auf, um die zugrundeliegende **ISwapChainPanelNative**-Implementierung abzurufen.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Erstellen Sie das DXGI-Gerät und die Swapchain, und legen Sie für die Swapchain die Schnittstelle [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143) fest, indem Sie diese an die [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144)-Methode übergeben.

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  Zeichnen Sie in die DirectX-Swapchain, und präsentieren Sie sie, um die Inhalte anzuzeigen.

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    Die XAML-Elemente werden aktualisiert, wenn das Windows-Runtime-Layout/die Rendering-Logik ein Update signalisiert.

## Verwandte Themen


* [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Programmieranleitung für Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 






<!--HONumber=Mar16_HO1-->


