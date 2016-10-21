---
author: mtoepke
title: "Swapchainskalierung und Überlagerungen"
description: "Hier erfahren Sie, wie Sie skalierte Swapchains zum schnelleren Rendern auf mobilen Geräten erstellen und Überlagerungsswapchains (falls verfügbar) verwenden, um die visuelle Qualität zu steigern."
ms.assetid: 3e4d2d19-cac3-eebc-52dd-daa7a7bc30d1
translationtype: Human Translation
ms.sourcegitcommit: d403e78b775af0f842ba2172295a09e35015dcc8
ms.openlocfilehash: 1eea87b2175872e5a3bc7c41e82cda47bb555f82

---

# Swapchainskalierung und Überlagerungen


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Hier erfahren Sie, wie Sie skalierte Swapchains zum schnelleren Rendern auf mobilen Geräten erstellen und Überlagerungsswapchains (falls verfügbar) verwenden, um die visuelle Qualität zu steigern.

## Swapchains unter DirectX11.2


Unter Direct3D11.2 können Sie UWP-Apps (Universelle Windows-Plattform) mit Swapchains erstellen, die von nicht systemeigenen (reduzierten) Auflösungen skaliert werden, um höhere Füllraten zu ermöglichen. Außerdem enthält Direct3D11.2 APIs zum Rendern mit Hardwareüberlagerungen, damit Sie eine UI in einer anderen Swapchain mit systemeigener Auflösung darstellen können. So kann das Spiel die UI mit der höchsten systemeigenen Auflösung zeichnen und gleichzeitig eine hohe Framerate erzielen. Mobile Geräte und Anzeigen mit hohem DPI-Wert (z. B. 3840 x 2160) lassen sich auf diese Weise bestmöglich nutzen. In diesem Artikel wird erläutert, wie Sie sich überlappende Swapchains verwenden.

Mit Direct3D11.2 wird auch ein neues Feature zur Erzielung einer geringeren Latenz mithilfe von Flipmodell-Swapchains eingeführt. Weitere Informationen finden Sie unter [Reduzieren der Latenz mit DXGI1.3-Swapchains](reduce-latency-with-dxgi-1-3-swap-chains.md).

## Verwenden der Swapchainskalierung


Wenn Ihr Spiel nicht auf der neuesten Hardware ausgeführt wird – oder auf Hardware, die für sparsamen Betrieb optimiert ist –, kann es von Vorteil sein, Echtzeitinhalte des Spiels mit einer niedrigeren Auflösung als der Auflösung zu rendern, die das Display standardmäßig leisten kann. Dazu muss die zum Rendern der Spielinhalte verwendete Swapchain kleiner als die systemeigene Auflösung sein, oder es muss ein Unterbereich der Swapchain verwendet werden.

1.  Erstellen Sie zuerst eine Swapchain mit der höchstmöglichen systemeigenen Auflösung.

    ```cpp
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

    swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
    swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
    swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
    swapChainDesc.Stereo = false;
    swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
    swapChainDesc.SampleDesc.Quality = 0;
    swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
    swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All Windows Store apps must use this SwapEffect.
    swapChainDesc.Flags = 0;
    swapChainDesc.Scaling = DXGI_SCALING_STRETCH;

    // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
    ComPtr<IDXGIDevice3> dxgiDevice;
    DX::ThrowIfFailed(
        m_d3dDevice.As(&dxgiDevice)
        );

    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );

    ComPtr<IDXGIFactory2> dxgiFactory;
    DX::ThrowIfFailed(
        dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
        );

    ComPtr<IDXGISwapChain1> swapChain;
    DX::ThrowIfFailed(
        dxgiFactory->CreateSwapChainForCoreWindow(
            m_d3dDevice.Get(),
            reinterpret_cast<IUnknown*>(m_window.Get()),
            &swapChainDesc,
            nullptr,
            &swapChain
            )
        );

    DX::ThrowIfFailed(
        swapChain.As(&m_swapChain)
        );
    ```

2.  Wählen Sie anschließend einen Unterbereich der Swapchain für die Skalierung aus, indem Sie die Quellgröße auf eine niedrigere Auflösung festlegen.

    Im Beispiel zu DX-Vordergrund-Swapchains wird anhand eines Prozentsatzes eine reduzierte Größe berechnet:

    ```cpp
    m_d3dRenderSizePercentage = percentage;

    UINT renderWidth = static_cast<UINT>(m_d3dRenderTargetSize.Width * percentage + 0.5f);
    UINT renderHeight = static_cast<UINT>(m_d3dRenderTargetSize.Height * percentage + 0.5f);

    // Change the region of the swap chain that will be presented to the screen.
    DX::ThrowIfFailed(
        m_swapChain->SetSourceSize(
            renderWidth,
            renderHeight
            )
        );
    ```

3.  Erstellen Sie einen Viewport für den Unterbereich der Swapchain.

    ```cpp
    // In Direct3D, change the Viewport to match the region of the swap
    // chain that will now be presented from.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        static_cast<float>(renderWidth),
        static_cast<float>(renderHeight)
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

4.  Bei Verwendung von Direct2D muss die Drehungstransformation angepasst werden, um die Quellregion entsprechend zu berücksichtigen.

## Erstellen einer Hardware-ÜberlagerungsSwapchain für UI-Elemente


Mit der Verwendung der Swapchainskalierung ist der Nachteil verbunden, dass auch die UI herunterskaliert wird und ggf. verschwimmt und schwieriger zu nutzen ist. Auf Geräten mit Hardwareunterstützung für Überlagerungsswapchains kann dieses Problem vollständig vermieden werden, indem die UI mit der systemeigenen Auflösung in einer Swapchain gerendert wird, die von den Echtzeitinhalten des Spiels getrennt ist. Beachten Sie, dass dieses Verfahren nur für [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Swapchains gilt und nicht für die XAML-Interoperabilität verwendet werden kann.

Führen Sie die folgenden Schritte aus, um eine Vordergrund-Swapchain zu erstellen, bei der eine Hardwareüberlagerungsfunktion verwendet wird. Diese Schritte werden ausgeführt, nachdem wie oben beschrieben zuerst eine Swapchain für die Echtzeitinhalte des Spiels erstellt wurde.

1.  Ermitteln Sie zuerst, ob Überlagerungen vom DXGI-Adapter unterstützt werden. Rufen Sie den DXGI-Ausgabeadapter aus der Swapchain ab:

    ```cpp
    ComPtr<IDXGIAdapter> outputDxgiAdapter;
    DX::ThrowIfFailed(
        dxgiFactory->EnumAdapters(0, &outputDxgiAdapter)
        );

    ComPtr<IDXGIOutput> dxgiOutput;
    DX::ThrowIfFailed(
        outputDxgiAdapter->EnumOutputs(0, &dxgiOutput)
        );

    ComPtr<IDXGIOutput2> dxgiOutput2;
    DX::ThrowIfFailed(
        dxgiOutput.As(&dxgiOutput2)
        );
    ```

    Überlagerungen werden vom DXGI-Adapter unterstützt, wenn der Ausgabeadapter für [**SupportsOverlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411) "True" zurückgibt.

    ```cpp
    m_overlaySupportExists = dxgiOutput2->SupportsOverlays() ? true : false;
    ```
    
    > **Hinweis**   Wenn der DXGI-Adapter Überlagerungen unterstützt, fahren Sie mit dem nächsten Schritt fort. Wenn das Gerät Überlagerungen nicht unterstützt, ist das Rendern mit mehreren Swapchains nicht effizient. Rendern Sie die UI stattdessen mit reduzierter Auflösung in derselben Swapchain wie die Echtzeitinhalte des Spiels.

     

2.  Erstellen Sie die Vordergrund-Swapchain mit [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559). In der für den *pDesc*-Parameter bereitgestellten [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)-Struktur müssen folgende Optionen festgelegt werden:

    -   Legen Sie das [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076)-Swapchainflag fest, um eine Vordergrund-Swapchain anzugeben.
    -   Verwenden Sie das [**DXGI\_ALPHA\_MODE\_PREMULTIPLIED**](https://msdn.microsoft.com/library/windows/desktop/hh404496)-Alphamodusflag. Vordergrund-Swapchains sind immer prämultipliziert.
    -   Legen Sie das [**DXGI\_SCALING\_NONE**](https://msdn.microsoft.com/library/windows/desktop/hh404526)-Flag fest. Vordergrund-Swapchains werden immer mit systemeigener Auflösung ausgeführt.

    ```cpp
     foregroundSwapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER;
     foregroundSwapChainDesc.Scaling = DXGI_SCALING_NONE;
     foregroundSwapChainDesc.AlphaMode = DXGI_ALPHA_MODE_PREMULTIPLIED; // Foreground swap chain alpha values must be premultiplied.
    ```

    > **Hinweis**   Legen Sie das [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076)-Flag bei jeder Größenänderung der Swapchain neu fest.

    ```cpp
    HRESULT hr = m_foregroundSwapChain->ResizeBuffers(
        2, // Double-buffered swap chain.
        static_cast<UINT>(m_d3dRenderTargetSize.Width),
        static_cast<UINT>(m_d3dRenderTargetSize.Height),
        DXGI_FORMAT_B8G8R8A8_UNORM,
        DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER // The FOREGROUND_LAYER flag cannot be removed with ResizeBuffers.
        );
    ```

3.  Erhöhen Sie bei Verwendung von zwei Swapchains die maximale Framelatenz auf 2, damit der DXGI-Adapter genügend Zeit hat, um beide Swapchains gleichzeitig (innerhalb desselben VSync-Intervalls) darzustellen.

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

4.  Für Vordergrund-Swapchains wird immer prämultipliziertes Alpha verwendet. Es wird davon ausgegangen, dass die Farbwerte der einzelnen Pixel vor der Darstellung des Frames bereits mit dem Alphawert multipliziert wurden. Ein BGRA-Pixel mit 100% Weiß und einem Alpha von 50% wird beispielsweise auf (0,5, 0,5, 0,5, 0,5) festgelegt.

    Der Schritt der Alphaprämultiplikation kann in der Ausgabezusammenführungsphase ausgeführt werden, indem ein App-Blend-Status (siehe [**ID3D11BlendState**](https://msdn.microsoft.com/library/windows/desktop/ff476349)) angewendet wird, bei dem das **SrcBlend**-Feld der [**D3D11\_RENDER\_TARGET\_BLEND\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476200)-Struktur auf **D3D11\_SRC\_ALPHA** festgelegt ist. Es können auch Ressourcen mit prämultiplizierten Alphawerten verwendet werden.

    Wenn der Schritt der Alphaprämultiplikation nicht erfolgt, erscheinen Farben für die Vordergrund-Swapchain heller als erwartet.

5.  Je nachdem, ob die Vordergrund-Swapchain erstellt wurde, muss die Direct2D-Zeichenoberfläche für UI-Elemente möglicherweise der Vordergrund-Swapchain zugeordnet werden.

    Erstellen von Renderzielansichten:

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

    Erstellen der Direct2D-Zeichenoberfläche:

    ```cpp
    if (m_foregroundSwapChain)
    {
        // Create a Direct2D target bitmap for the foreground swap chain.
        ComPtr<IDXGISurface2> dxgiForegroundSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiForegroundSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiForegroundSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }
    else
    {
        // Create a Direct2D target bitmap for the swap chain.
        ComPtr<IDXGISurface2> dxgiSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
    ```

    ```cpp
    // Create a render target view of the swap chain's back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );
    ```

6.  Stellen Sie die Vordergrund-Swapchain zusammen mit der skalierten Swapchain dar, die für die Echtzeitinhalte des Spiels verwendet wird. Da die Framelatenz für beide Swapchains auf 2 festgelegt wurde, können beide von DXGI innerhalb desselben VSync-Intervalls dargestellt werden.

    ```cpp
    // Present the contents of the swap chain to the screen.
    void DX::DeviceResources::Present()
    {
        // The first argument instructs DXGI to block until VSync, putting the application
        // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
        // frames that will never be displayed to the screen.
        HRESULT hr = m_swapChain->Present(1, 0);

        if (SUCCEEDED(hr) && m_foregroundSwapChain)
        {
            m_foregroundSwapChain->Present(1, 0);
        }

        // Discard the contents of the render targets.
        // This is a valid operation only when the existing contents will be entirely
        // overwritten. If dirty or scroll rects are used, this call should be removed.
        m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());
        if (m_foregroundSwapChain)
        {
            m_d3dContext->DiscardView(m_d3dForegroundRenderTargetView.Get());
        }

        // Discard the contents of the depth stencil.
        m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

        // If the device was removed either by a disconnection or a driver upgrade, we
        // must recreate all device resources.
        if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            HandleDeviceLost();
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    ```

 

 







<!--HONumber=Aug16_HO3-->


