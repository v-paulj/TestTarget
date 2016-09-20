---
author: mtoepke
title: Initialisieren von Direct3D11
description: "Hier wird veranschaulicht, wie Sie Direct3D 9-Initialisierungscode in Direct3D 11 konvertieren, und Sie erfahren, wie Sie Handles zum Direct3D-Gerät und zum Gerätekontext abrufen und DXGI zum Einrichten einer Swapchain verwenden."
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: f0e25e43633d895673d640f139af338f6f0713f2

---

# Initialisieren von Direct3D 11


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Zusammenfassung**

-   Teil 1: Initialisieren von Direct3D 11
-   [Teil 2: Konvertieren des Renderingframeworks](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [Teil 3: Portieren der Spielschleife](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Hier wird veranschaulicht, wie Sie Direct3D 9-Initialisierungscode in Direct3D 11 konvertieren, und Sie erfahren, wie Sie Handles zum Direct3D-Gerät und zum Gerätekontext abrufen und DXGI zum Einrichten einer Swapchain verwenden. Teil1 der exemplarischen Vorgehensweise [Portieren einer einfachen Direct3D9-App zu DirectX11 und UWP (Universelle Windows-Plattform)](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## Initialisieren des Direct3D-Geräts


In Direct3D9 wurde ein Handle zum Direct3D-Gerät erstellt, indem [**IDirect3D9::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174313) aufgerufen wurde. Zuerst wurde ein Zeiger auf [**IDirect3D9 interface**](https://msdn.microsoft.com/library/windows/desktop/bb174300) abgerufen und eine Anzahl von Parametern angegeben, um die Konfiguration des Direct3D-Geräts und der Swapchain zu steuern. Vorher wurde [**GetDeviceCaps**](https://msdn.microsoft.com/library/windows/desktop/dd144877) aufgerufen, um sicherzustellen, dass vom Gerät keine unmöglichen Vorgänge verlangt wurden.

Direct3D9

```cpp
UINT32 AdapterOrdinal = 0;
D3DDEVTYPE DeviceType = D3DDEVTYPE_HAL;
D3DCAPS9 caps;
m_pD3D->GetDeviceCaps(AdapterOrdinal, DeviceType, &caps); // caps bits

D3DPRESENT_PARAMETERS params;
ZeroMemory(&params, sizeof(D3DPRESENT_PARAMETERS));

// Swap chain parameters:
params.hDeviceWindow = m_hWnd;
params.AutoDepthStencilFormat = D3DFMT_D24X8;
params.BackBufferFormat = D3DFMT_X8R8G8B8;
params.MultiSampleQuality = D3DMULTISAMPLE_NONE;
params.MultiSampleType = D3DMULTISAMPLE_NONE;
params.SwapEffect = D3DSWAPEFFECT_DISCARD;
params.Windowed = true;
params.PresentationInterval = 0;
params.BackBufferCount = 2;
params.BackBufferWidth = 0;
params.BackBufferHeight = 0;
params.EnableAutoDepthStencil = true;
params.Flags = 2;

m_pD3D->CreateDevice(
    0,
    D3DDEVTYPE_HAL,
    m_hWnd,
    64,
    &params,
    &m_pd3dDevice
    );
```

In Direct3D11 werden der Gerätekontext und die Grafikinfrastruktur als separat vom eigentlichen Gerät angesehen. Die Initialisierung ist in mehrere Schritte unterteilt.

Zuerst wird das Gerät erstellt. Dazu rufen wir eine Liste der Featureebenen ab, die vom Gerät unterstützt werden. Dies sind bereits nahezu alle Informationen, die in Bezug auf die GPU benötigt werden. Allein für den Zugriff auf Direct3D muss auch keine Schnittstelle erstellt werden. Stattdessen wird die [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)-Kern-API verwendet. So erhalten wir einen Handle zum Gerät und den unmittelbaren Kontext des Geräts. Der Gerätekontext wird verwendet, um den Pipelinestatus festzulegen und Renderbefehle zu erzeugen.

Nach dem Erstellen des Direct3D11-Geräts und -Kontexts kann die COM-Zeigerfunktion zum Abrufen der jeweils aktuellen Version der Schnittstellen verwendet werden, die über zusätzliche Funktionen verfügt. Dies ist stets zu empfehlen.

> 
            **Hinweis**  D3D\_FEATURE\_LEVEL\_9\_1 (entspricht Shadermodell 2.0) ist die Mindestebene, die vom Windows Store-Spiel unterstützt werden muss. (Die ARM-Pakete des Spiels erhalten keine Zertifizierung, wenn 9\_1 nicht unterstützt wird.) Wenn das Spiel auch einen Renderpfad für die Features von Shadermodell3 enthält, sollten Sie D3D_FEATURE\_LEVEL\_9\_3 in das Array einbeziehen.

 

Direct3D11

```cpp
// This flag adds support for surfaces with a different color channel 
// ordering than the API default. It is required for compatibility with
// Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
// If the project is in a debug build, enable debugging via SDK Layers.
creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

// This example only uses feature level 9.1.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
D3D11CreateDevice(
    nullptr, // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,
    nullptr,
    creationFlags,
    featureLevels,
    ARRAYSIZE(featureLevels),
    D3D11_SDK_VERSION, // Windows Store apps must set this to D3D11_SDK_VERSION.
    &device, // Returns the Direct3D device created.
    nullptr,
    &context // Returns the device immediate context.
    );

// Store pointers to the Direct3D 11.2 API device and immediate context.
device.As(&m_d3dDevice);

context.As(&m_d3dContext);
```

## Erstellen einer Swapchain


Direct3D 11 enthält eine Geräte-API mit der Bezeichnung DirectX Graphics Infrastructure (DXGI). Mithilfe der DXGI-Schnittstelle können Sie (beispielsweise) steuern, wie die Swapchain konfiguriert wird, und freigegebene Geräte einrichten. Bei diesem Schritt der Initialisierung von Direct3D wird DXGI zum Erstellen einer Swapchain verwendet. Seit der Erstellung des Geräts können wir eine Schnittstellenkette zurück zum DXGI-Adapter verfolgen.

Vom Direct3D-Gerät wird eine COM-Schnittstelle für DXGI implementiert. Zuerst muss diese Schnittstelle abgerufen und verwendet werden, um den DXGI-Adapter anzufordern, mit dem das Gerät gehostet wird. Anschließend wird der DXGI-Adapter zum Erstellen einer DXGI-Factory genutzt.

> 
            **Hinweis**  Hierbei handelt es sich um COM-Schnittstellen, sodass Sie wahrscheinlich zuerst an die Verwendung von [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) denken. Stattdessen sollten Sie intelligente [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)-Zeiger nutzen. Rufen Sie anschließend einfach die [**As()**](https://msdn.microsoft.com/library/windows/apps/br230426.aspx)-Methode auf, und stellen Sie einen leeren COM-Zeiger mit dem passenden Schnittstellentyp bereit.

 

**Direct3D11**

```cpp
ComPtr<IDXGIDevice2> dxgiDevice;
m_d3dDevice.As(&dxgiDevice);

// Then, the adapter hosting the device;
ComPtr<IDXGIAdapter> dxgiAdapter;
dxgiDevice->GetAdapter(&dxgiAdapter);

// Then, the factory that created the adapter interface:
ComPtr<IDXGIFactory2> dxgiFactory;
dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2),
    &dxgiFactory
    );
```

Da die DXGI-Factory jetzt vorhanden ist, können wir sie zum Erstellen der Swapchain verwenden. Als Nächstes werden die Parameter der Swapchain definiert. Das Oberflächenformat muss angegeben werden, und wir wählen [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059), weil es mit Direct2D kompatibel ist. Die Anzeigeskalierung, Multisampling und das Stereorendering werden deaktiviert, weil diese Funktionen in diesem Beispiel nicht verwendet werden. Da die Ausführung direkt in einem CoreWindow-Objekt erfolgt, können wir die Breite und Höhe auf der Einstellung 0 belassen und automatisch Vollbildwerte erhalten.

> 
            **Hinweis**  Legen Sie den Parameter *SDKVersion* für UWP-Apps immer auf D3D11\_SDK\_VERSION fest.

 

**Direct3D11**

```cpp
ComPtr<IDXGISwapChain1> swapChain;
dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr,
    &swapChain
    );
swapChain.As(&m_swapChain);
```

Um sicherzustellen, dass nur so häufig Rendervorgänge ausgeführt werden, wie diese vom Bildschirm auch angezeigt werden können, legen wir die Framelatenz auf1 fest und verwenden [**DXGI\_SWAP\_EFFECT\_FLIP\_SEQUENTIAL**](https://msdn.microsoft.com/library/windows/desktop/bb173077). So kann Energie gespart werden. Außerdem ist dies eine Anforderung für die Store-Zertifizierung. Weitere Informationen zur Darstellung auf dem Bildschirm erhalten Sie in Teil 2 dieser exemplarischen Vorgehensweise.

> 
            **Hinweis**  Sie können das Multithreading verwenden (beispielsweise [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229642)-Arbeitsaufgaben), um weiterarbeiten zu können, während der Renderthread blockiert ist.

 

**Direct3D11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

Als Nächstes können wir den Hintergrundpuffer für das Rendern einrichten.

## Konfigurieren des Hintergrundpuffers als Renderziel


Zuerst müssen wir ein Handle zum Hintergrundpuffer abrufen. (Beachten Sie, dass der Hintergrundpuffer im Besitz der DXGI-Swapchain ist, während der Besitzer unter DirectX9 das Direct3D-Gerät war.) Anschließend wird das Direct3D-Gerät angewiesen, es als Renderziel zu verwenden, indem mithilfe des Hintergrundpuffers eine Renderziel*ansicht* erstellt wird.

**Direct3D11**

```cpp
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChain->GetBuffer(
    0,
    __uuidof(ID3D11Texture2D),
    &backBuffer
    );

// Create a render target view on the back buffer.
m_d3dDevice->CreateRenderTargetView(
    backBuffer.Get(),
    nullptr,
    &m_renderTargetView
    );
```

Jetzt kommt der Gerätekontext ins Spiel. Direct3D wird angewiesen, die neu erstellte Renderzielansicht zu nutzen, indem die Gerätekontextschnittstelle verwendet wird. Wir rufen die Breite und Höhe des Hintergrundpuffers ab, damit das gesamte Fenster als Viewport genutzt werden kann. Beachten Sie, dass der Hintergrundpuffer an die Swapchain angefügt ist. Wenn sich also die Fenstergröße ändert (z.B. wenn Benutzer das Spielfenster auf einen anderen Monitor ziehen), müssen die Größe des Hintergrundpuffers geändert und einige Setupschritte ausgeführt werden.

**Direct3D11**

```cpp
D3D11_TEXTURE2D_DESC backBufferDesc = {0};
backBuffer->GetDesc(&backBufferDesc);

CD3D11_VIEWPORT viewport(
    0.0f,
    0.0f,
    static_cast<float>(backBufferDesc.Width),
    static_cast<float>(backBufferDesc.Height)
    );

m_d3dContext->RSSetViewports(1, &viewport);
```

Da wir jetzt über ein Gerätehandle und ein Vollbild-Renderziel verfügen, sind wir bereit zum Laden und Zeichnen der Geometrie. Fahren Sie mit [Teil2: Rendern](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md) fort.

 

 







<!--HONumber=Jun16_HO4-->


