---
author: mtoepke
title: Multisampling in UWP-Apps (Apps für die universelle Windows-Plattform)
description: Hier erfahren Sie, wie Sie Multisampling in UWP-Apps (Apps für die universelle Windows-Plattform) verwenden, die mit Direct3D erstellt wurden.
ms.assetid: 1cd482b8-32ff-1eb0-4c91-83eb52f08484
---

# <span id="dev_gaming.multisampling__multi-sample_anti_aliasing__in_windows_store_apps"></span> Multisampling in UWP-Apps (Apps für die universelle Windows-Plattform)


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Hier erfahren Sie, wie Sie Multisampling in Windows-Runtime-Apps verwenden, die mit Direct3D erstellt wurden. Das Multisampling, das auch als Multiple Sample Antialiasing bezeichnet wird, ist ein Grafikverfahren, das treppenförmige Kanten reduziert. Dazu werden mehr Pixel gezeichnet, als im endgültigen Renderziel tatsächlich enthalten sind, und anschließend wird der Mittelwert der Werte gebildet, um in bestimmten Pixeln die Darstellung einer "partiellen" Kante zu erreichen. Eine ausführliche Beschreibung der Funktionsweise des Multisamplings in Direct3D finden Sie unter [Regeln für die Rasterung beim Multiple Sample Antialiasing](https://msdn.microsoft.com/library/windows/desktop/cc627092#Multisample).

## Multisampling und die Flipmodell-Swapchain


Für UWP-Apps, die DirectX nutzen, müssen Flipmodell-Swapchains verwendet werden. Flipmodell-Swapchains verfügen über keine direkte Multisampling-Unterstützung. Das Multisampling kann aber auf andere Art und Weise angewendet werden. Hierzu wird die Szene in einer Renderzielansicht mit Multisampling gerendert und dieses Renderziel mit Multisampling vor dem Darstellen dann für den Hintergrundpuffer aufgelöst. Dieser Artikel beschreibt die Schritte, die erforderlich sind, um UWP-Apps mit Multisampling zu versehen.

### Verwenden von Multisampling

Direct3D-Featureebenen stellen die Unterstützung für spezielle Mindestfunktionen für die Beispielanzahl sicher, und es sind bestimmte Pufferformate verfügbar, die das Multisampling unterstützen. Grafikgeräte unterstützen häufig einen weiteren Bereich von Formaten und Beispielanzahlen als das erforderliche Minimum. Sie können die Multisampling-Unterstützung zur Laufzeit bestimmen. Prüfen Sie zu diesem Zweck die Featureunterstützung für das Multisampling mit bestimmten DXGI-Formaten und anschließend die Beispielanzahlen, die Sie für die einzelnen unterstützten Formate verwenden können.

1.  Ermitteln Sie per Aufruf von [**ID3D11Device::CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497), welche DXGI-Formate in Verbindung mit dem Multisampling verwendet werden können. Geben Sie die Renderzielformate an, die vom Spiel verwendet werden können. Sowohl für das Renderziel als auch das Auflösungsziel muss das gleiche Format genutzt werden. Prüfen Sie daher sowohl [**D3D11\_FORMAT\_SUPPORT\_MULTISAMPLE\_RENDERTARGET**](https://msdn.microsoft.com/library/windows/desktop/ff476134) als auch **D3D11\_FORMAT\_SUPPORT\_MULTISAMPLE\_RESOLVE**.

    **Featureebene 9:**  Auch wenn Geräte mit Featureebene 9 [die Unterstützung für Renderzielformate mit Multisampling sicherstellen](https://msdn.microsoft.com/library/windows/desktop/ff471324#MultiSample_RenderTarget), ist die Unterstützung für Auflösungsziele mit Multisampling nicht garantiert. Diese Überprüfung ist also erforderlich, bevor versucht wird, das in diesem Thema beschriebene Multisampling-Verfahren anzuwenden.

    Mit dem folgenden Code wird die Multisampling-Unterstützung für alle DXGI\_FORMAT-Werte überprüft:

    ```cpp
    // Determine the format support for multisampling.
    for (UINT i = 1; i < DXGI_FORMAT_MAX; i++)
    {
        DXGI_FORMAT inFormat = safe_cast<DXGI_FORMAT>(i);
        UINT formatSupport = 0;
        HRESULT hr = m_d3dDevice->CheckFormatSupport(inFormat, &formatSupport);

        if ((formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RESOLVE) &&
            (formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RENDERTARGET)
            )
        {
            m_supportInfo->SetFormatSupport(i, true);
        }
        else
        {
            m_supportInfo->SetFormatSupport(i, false);
        }
    }
    ```

2.  Fragen Sie für jedes unterstützte Format die Beispielanzahl ab, indem Sie [**ID3D11Device::CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499) aufrufen.

    Mit dem folgenden Code wird die Unterstützung der Beispielgröße für unterstützte DXGI-Formate überprüft:

    ```cpp
    // Find available sample sizes for each supported format.
    for (unsigned int i = 0; i < DXGI_FORMAT_MAX; i++)
    {
        for (unsigned int j = 1; j < MAX_SAMPLES_CHECK; j++)
        {
            UINT numQualityFlags;

            HRESULT test = m_d3dDevice->CheckMultisampleQualityLevels(
                (DXGI_FORMAT) i,
                j,
                &numQualityFlags
                );

            if (SUCCEEDED(test) && (numQualityFlags > 0))
            {
                m_supportInfo->SetSampleSize(i, j, 1);
                m_supportInfo->SetQualityFlagsAt(i, j, numQualityFlags);
            }
        }
    }
    ```

    > **Hinweis**   Verwenden Sie stattdessen [**ID3D11Device2::CheckMultisampleQualityLevels1**](https://msdn.microsoft.com/library/windows/desktop/dn280494), falls Sie die Multisampling-Unterstützung der Puffer für unterteilte Ressourcen überprüfen müssen.

     

3.  Erstellen Sie einen Puffer und eine Renderzielansicht mit der gewünschten Beispielanzahl. Verwenden Sie dieselbe DXGI\_FORMAT-Struktur, Breite und Höhe wie für die Swapchain, aber geben Sie eine Beispielanzahl an, die größer als 1 ist, und verwenden Sie eine Texturdimension mit Multisampling (z. B. **D3D11\_RTV\_DIMENSION\_TEXTURE2DMS**). Bei Bedarf können Sie die Swapchain mit für das Multisampling optimalen Einstellungen neu erstellen.

    Mit dem folgenden Code wird ein Renderziel mit Multisampling erstellt:

    ```cpp
    float widthMulti = m_d3dRenderTargetSize.Width;
    float heightMulti = m_d3dRenderTargetSize.Height;

    D3D11_TEXTURE2D_DESC offScreenSurfaceDesc;
    ZeroMemory(&offScreenSurfaceDesc, sizeof(D3D11_TEXTURE2D_DESC));

    offScreenSurfaceDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    offScreenSurfaceDesc.Width = static_cast<UINT>(widthMulti);
    offScreenSurfaceDesc.Height = static_cast<UINT>(heightMulti);
    offScreenSurfaceDesc.BindFlags = D3D11_BIND_RENDER_TARGET;
    offScreenSurfaceDesc.MipLevels = 1;
    offScreenSurfaceDesc.ArraySize = 1;
    offScreenSurfaceDesc.SampleDesc.Count = m_sampleSize;
    offScreenSurfaceDesc.SampleDesc.Quality = m_qualityFlags;

    // Create a surface that's multisampled.
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &offScreenSurfaceDesc,
        nullptr,
        &m_offScreenSurface)
        );

    // Create a render target view. 
    CD3D11_RENDER_TARGET_VIEW_DESC renderTargetViewDesc(D3D11_RTV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
        m_offScreenSurface.Get(),
        &renderTargetViewDesc,
        &m_d3dRenderTargetView
        )
        );
    ```

4.  Der Tiefenpuffer muss die gleiche Breite, Höhe, Beispielanzahl und Texturdimension haben, um mit dem Renderziel mit Multisampling übereinzustimmen.

    Mit dem folgenden Code wird ein Tiefenpuffer mit Multisampling erstellt:

    ```cpp
    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT,
        static_cast<UINT>(widthMulti),
        static_cast<UINT>(heightMulti),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL,
        D3D11_USAGE_DEFAULT,
        0,
        m_sampleSize,
        m_qualityFlags
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &depthStencilDesc,
        nullptr,
        &depthStencil
        )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
        depthStencil.Get(),
        &depthStencilViewDesc,
        &m_d3dDepthStencilView
        )
        );
    ```

5.  Dies ist ein guter Zeitpunkt zum Erstellen des Viewports, da die Breite und Höhe des Viewports ebenfalls mit dem Renderziel übereinstimmen muss.

    Mit dem folgenden Code wird ein Viewport erstellt:

    ```cpp
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        widthMulti / m_scalingFactor,
        heightMulti / m_scalingFactor
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

6.  Rendern Sie jeden Frame für das Renderziel mit Multisampling. Rufen Sie nach Abschluss des Renderings [**ID3D11DeviceContext::ResolveSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476474) auf, bevor der Frame dargestellt wird. Damit wird Direct3D angewiesen, den Multisampling-Vorgang durchzuführen, den Wert jedes Pixels für die Anzeige zu berechnen und das Ergebnis in den Hintergrundpuffer zu stellen. Der Hintergrundpuffer enthält dann das endgültige Antialiasing-Bild für die Darstellung.

    Mit dem folgenden Code wird die Unterressource aufgelöst, bevor der Frame dargestellt wird:

    ```cpp
    if (m_sampleSize > 1)
    {
        unsigned int sub = D3D11CalcSubresource(0, 0, 1);

        m_d3dContext->ResolveSubresource(
            m_backBuffer.Get(),
            sub,
            m_offScreenSurface.Get(),
            sub,
            DXGI_FORMAT_B8G8R8A8_UNORM
            );
    }

    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    hr = m_swapChain->Present(1, 0);
    ```

 

 






<!--HONumber=May16_HO2-->


