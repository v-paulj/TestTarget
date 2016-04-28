---
title: Einrichten von DirectX-Ressourcen und Darstellen eines Bilds
description: Hier zeigen wir Ihnen das Erstellen Direct3D-Geräts, Swapchain und Renderziel-Ansicht sowie die Darstellung des gerenderten Bilds auf dem Bildschirm.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
---

# Einrichten von DirectX-Ressourcen und Darstellen eines Bilds


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Hier zeigen wir Ihnen das Erstellen eines Direct3D-Geräts, einer Swapchain und einer Renderziel-Ansicht sowie die Darstellung des gerenderten Bilds auf dem Bildschirm.

**Ziel:** Sie richten DirectX-Ressourcen in einer UWP-App (Universelle Windows-Plattform) mit C++ ein und zeigen eine Volltonfarbe an.

## Voraussetzungen


Es wird davon ausgegangen, dass Sie mit C+ vertraut sind. Sie müssen außerdem mit den grundlegenden Konzepten der Grafikprogrammierung vertraut sein.

**Zeitaufwand:** 20 Minuten.

## Anweisungen

### 1. Deklarieren von Direct3D-Schnittstellenvariablen mit ComPtr

Wir deklarieren Direct3D-Schnittstellenvariablen mit der [intelligenten Zeigervorlage](https://msdn.microsoft.com/library/windows/apps/hh279674.aspx) ComPtr aus der C++-Vorlagenbibliothek für Windows-Runtime (Windows Runtime C++ Template Library, WRL), damit wir die Lebensdauer dieser Variablen ausnahmesicher verwalten können. Mithilfe dieser Variablen können wir auf die [**ComPtr class**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) und ihre Member zugreifen. Beispiel:

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

Wenn Sie [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) mit ComPtr deklarieren, können Sie anschließend die ComPtr-Methode **GetAddressOf** zum Abrufen der Adresse des Zeigers auf **ID3D11RenderTargetView** (\*\*ID3D11RenderTargetView) für die Übergabe an [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) verwenden. **OMSetRenderTargets** bindet das Renderziel an den [Ausgabezusammenführungsstatus](https://msdn.microsoft.com/library/windows/desktop/bb205120), um das Renderziel als Ausgabeziel anzugeben.

Nachdem die Beispiel-App gestartet wurde, wird sie initialisiert und geladen. Danach kann sie ausgeführt werden.

### 2. Erstellen des Direct3D-Geräts

Für die Verwendung der Direct3D-API zum Rendern einer Szene müssen wir zunächst ein Direct3D-Gerät erstellen, das die Grafikkarte darstellt. Zum Erstellen des Direct3D-Geräts rufen wir die [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)-Funktion auf. Im Array der [**D3D\_FEATURE\_LEVEL**](https://msdn.microsoft.com/library/windows/desktop/ff476329)-Werte geben wir die Ebenen 9.1 bis 11.1 an. Direct3D durchläuft das Array der Reihe nach und gibt die höchste Ebene des unterstützten Features zurück. Zum Abrufen der höchsten verfügbaren Featureebene listen wir also die **D3D\_FEATURE\_LEVEL**-Arrayeinträge vom höchsten zum niedrigsten Eintrag auf. Wir übergeben das [**D3D11\_CREATE\_DEVICE\_BGRA\_SUPPORT**](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_BGRA_SUPPORT)-Flag an den *Flags*-Parameter, damit Direct3D-Ressourcen mit Direct2D ausgeführt werden können. Bei Verwendung der Debugversion übergeben wir zudem das [**D3D11\_CREATE\_DEVICE\_DEBUG**](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_DEBUG)-Flag. Weitere Informationen zum Debuggen von Apps finden Sie unter [Verwenden der Debugschicht zum Debuggen von Apps](https://msdn.microsoft.com/library/windows/desktop/jj200584).

Wir rufen das Direct3D 11.1-Gerät ([**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)) und den Gerätekontext ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)) durch Abfragen des Direct3D 11-Geräts und Gerätekontexts ab, die von [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) zurückgegeben werden.

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### 3. Erstellen der Swapchain

Im nächsten Schritt erstellen wir eine Swapchain, die von dem Gerät zum Rendern und für die Anzeige verwendet wird. Wir deklarieren und initialisieren eine [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)-Struktur, um die Swapchain zu beschreiben. Anschließend richten wir die Swapchain als Flip-Modell ein (d. h. eine Swapchain, für die der [**DXGI\_SWAP\_EFFECT\_FLIP\_SEQUENTIAL**](https://msdn.microsoft.com/library/windows/desktop/bb173077#DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL)-Wert im **SwapEffect**-Member festgelegt ist), und wir legen das **Format**-Member auf [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM) fest. Wir legen den **Count**-Member der [**DXGI\_SAMPLE\_DESC**](https://msdn.microsoft.com/library/windows/desktop/bb173072)-Struktur, die vom **SampleDesc**-Member angegeben wird, auf 1 fest. Außerdem legen wir den **Quality**-Member von **DXGI\_SAMPLE\_DESC** auf null fest, da MSAA (Multiple Sample Antialiasing) vom Flip-Modell nicht unterstützt wird. Wir legen den **BufferCount**-Member auf 2 fest, sodass die Swapchain für die Darstellung auf dem Anzeigegerät einen Frontpuffer sowie einen Hintergrundpuffer verwenden kann, der als Renderziel dient.

Wir rufen das zugrunde liegende DXGI-Gerät durch Abfragen des Direct3D 11.1-Geräts ab. Zur Minimierung des Stromverbrauchs – einem wichtigen Aspekt für batteriebetriebene Geräte wie Laptops und Tablet-PCs – rufen wir die [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334)-Methode mit dem Wert 1 auf. Dieser Wert ist die maximale Anzahl an Hintergrundpufferframes, die von DXGI in die Warteschlange verschoben werden können. Dadurch wird sichergestellt, dass die App erst nach der vertikalen Austastung gerendert wird.

Für die endgültige Erstellung der Swapchain müssen wir die übergeordnete Factory vom DXGI-Gerät abrufen. Zum Abrufen des Adapters für das Gerät rufen wir [**IDXGIDevice::GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) auf. Anschließend rufen wir auf dem Adapter [**IDXGIObject::GetParent**](https://msdn.microsoft.com/library/windows/desktop/bb174542) auf, um die übergeordnete Factory ([**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)) zu erhalten. Zum Erstellen der Swapchain rufen wir [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) mit der Swapchainbeschreibung und dem Kernfenster der App auf.

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### 4. Erstellen der Renderziel-Ansicht

Zum Rendern von Grafik in dem Fenster müssen wir eine Renderziel-Ansicht erstellen. Wir rufen [**IDXGISwapChain::GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb174570) zum Abrufen des Swapchain-Hintergrundpuffers auf, der beim Erstellen der Renderziel-Ansicht verwendet werden soll. Wir geben den Hintergrundpuffer als 2D-Textur an ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)). Zum Erstellen der Renderziel-Ansicht rufen wir [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517) mit dem Hintergrundpuffer der Swapchain auf. Wir müssen das Zeichnen im gesamten Kernfenster festlegen, indem wir den Viewport ([**D3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/ff476260)) als vollständige Größe des Swapchain-Hintergrundpuffers angeben. Wir verwenden den Viewport in einem Aufruf von [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480), um den Viewport an die [Rasterprogrammstufe](https://msdn.microsoft.com/library/windows/desktop/bb205125) der Pipeline zu binden. In der Rasterprogrammstufe werden Vektorinformationen in ein Rasterbild konvertiert. In diesem Fall ist keine Konvertierung erforderlich, da wir nur eine Volltonfarbe anzeigen.

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### 5. Darstellen des gerenderten Bilds

Wir geben eine endlose Schleife zum fortlaufenden Rendern und Anzeigen der Szene ein.

In dieser Schleife wird Folgendes aufgerufen:

1.  [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464), um das Renderziel als Ausgabeziel anzugeben.
2.  [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388), um das Renderziel mit einer Volltonfarbe anzuzeigen.
3.  [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576), um das gerenderte Bild im Fenster darzustellen.

Da wir zuvor die maximale Framelatenz auf 1 festgelegt haben, reduziert Windows die Geschwindigkeit der Renderschleife generell auf die Bildschirmaktualisierungsrate, die normalerweise etwa 60 Hz beträgt. Windows reduziert die Geschwindigkeit der Renderschleife, indem die App in den Ruhezustand versetzt wird, wenn sie [**Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) aufruft. Windows versetzt die App so lange in den Ruhezustand, bis die Bildschirmanzeige aktualisiert wird.

```cpp
        // Enter the render loop.  Note that Windows Store apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### 6. Ändern der Größe des App-Fensters und des Swapchainpuffers

Wenn sich die Größe des App-Fensters ändert, muss die App die Größe der Swapchainpuffer ändern, die Renderziel-Ansicht neu erstellen und anschließend das gerenderte Bild in der geänderten Größe darstellen. Zum Ändern der Größe der Swapchainpuffer rufen wir [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577) auf. Bei diesem Aufruf lassen wir die Anzahl und das Format der Puffer unverändert (der *BufferCount*-Parameter ist auf den Wert zwei und der *NewFormat*-Parameter ist auf [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM) festgelegt). Wir legen für die Größe des Swapchain-Hintergrundpuffers denselben Wert wie für das Fenster mit geänderter Größe fest. Nachdem wir die Größe des Swapchainpuffers geändert haben, erstellen wir das neue Renderziel, und wir stellen das neue gerenderte Bild genauso wie beim Initialisieren der App dar.

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## Zusammenfassung und nächste Schritte


Wir haben ein Direct3D-Gerät, eine Swapchain und eine Renderziel-Ansicht erstellt und das gerenderte Bild auf dem Bildschirm dargestellt.

Als Nächstes zeichnen wir ein Dreieck auf dem Bildschirm.

[Erstellen von Shadern und Zeichnen von Primitiven](creating-shaders-and-drawing-primitives.md)

 

 






<!--HONumber=Mar16_HO1-->


