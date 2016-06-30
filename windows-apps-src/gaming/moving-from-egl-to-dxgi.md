---
author: mtoepke
title: Vergleichen des EGL-Codes mit DXGI und Direct3D
description: "Die DirectX-Grafikschnittstelle (DXGI) und verschiedene Direct3D-APIs erfüllen die gleiche Rolle wie EGL. In diesem Thema werden die DXGI und Direct3D 11 aus Sicht von EGL erläutert."
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 461983b646148c21aba7da2adb703510d95b0343

---

# Vergleichen des EGL-Codes mit DXGI und Direct3D


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)
-   [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)

Die DirectX-Grafikschnittstelle (DXGI) und verschiedene Direct3D-APIs erfüllen die gleiche Rolle wie EGL. In diesem Thema werden die DXGI und Direct3D 11 aus Sicht von EGL erläutert.

Mit der DXGI und Direct3D werden, wie bei EGL, Methoden zum Konfigurieren von Grafikressourcen, Beschaffen eines Renderkontexts, in den von den Shadern gezeichnet werden kann, und Anzeigen der Ergebnisse in einem Fenster bereitgestellt. Für die DXGI und Direct3D sind jedoch deutlich mehr Optionen vorhanden. Zudem ist beim Portieren aus EGL das richtige Einrichten mit mehr Aufwand verbunden.

> **Hinweis**   Diese Anleitung basiert auf der offenen Spezifikation der Khronos Group für EGL 1.4 unter [Khronos Native Platform Graphics Interface (EGL Version 1.4 - April 6, 2011) \[PDF\]](http://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf). In dieser Anleitung wird nicht auf Unterschiede eingegangen, die sich auf die spezielle Syntax für andere Plattformen und Entwicklungssprachen beziehen.

 

## Welche Unterschiede bestehen zwischen der DXGI und Direct3D?


Der große Vorteil von EGL gegenüber DXGI und Direct3D besteht darin, dass das Zeichnen auf einer Fensterfläche relativ einfach ist. Dies liegt daran, dass OpenGL ES 2.0 – und damit auch EGL – eine Spezifikation ist, die von mehreren Plattformanbietern implementiert wird. DXGI und Direct3D sind hingegen Einzelverweise, nach denen sich die Treiber von Hardwareanbietern richten müssen. Dies bedeutet, dass Microsoft eine Reihe von APIs implementieren muss, mit denen ein möglichst großes Spektrum von Anbieterfeatures abgedeckt wird. Es reicht nicht aus, sich auf eine Teilmenge der Funktionen zu konzentrieren, die von einem bestimmten Anbieter angeboten werden, oder anbieterspezifische Setupbefehle in einfacheren APIs zu kombinieren. Andererseits wird von Direct3D eine einzelne Gruppe von APIs bereitgestellt, die einen sehr weiten Bereich von Grafikhardwareplattformen und Featureebenen abdecken und für Entwickler in Bezug auf die Plattform mehr Flexibilität bieten.

Wie EGL auch, verfügen DXGI und Direct3D über APIs für das folgende Verhalten:

-   Beschaffen und Lesen/Schreiben in einen Framepuffer (unter DXGI als "Swapchain" bezeichnet)
-   Zuordnen des Framepuffers zu einem UI-Fenster
-   Beschaffen und Konfigurieren von Renderkontexten, in die gezeichnet wird
-   Ausgeben von Befehlen in die Grafikpipeline für einen bestimmten Renderkontext
-   Erstellen und Verwalten von Shaderressourcen und Zuordnen von Renderinhalten
-   Rendern in bestimmte Renderziele (z. B. Texturen)
-   Aktualisieren der Anzeigefläche des Fensters mit den Ergebnissen des Rendervorgangs mit den Grafikressourcen

Sehen Sie sich die DirectX 11-Vorlage für UWP-Apps in Microsoft Visual Studio 2015 an. Sie finden darin Informationen zum grundlegenden Direct3D-Prozess für die Konfiguration der Grafikpipeline. Die darin enthaltene Renderklasse stellt eine gute Grundlage für die Einrichtung der Direct3D 11-Grafikinfrastruktur und die Konfiguration der dazugehörigen grundlegenden Ressourcen dar. Außerdem werden UWP-App-Features (Universelle Windows-Plattform) wie die Bildschirmdrehung unterstützt.

Im Vergleich zu Direct3D 11 verfügt EGL über sehr wenige APIs. Die Navigation in Direct3D 11 kann sich als schwierig erweisen, wenn Sie mit den Benennungen und der "Sprache" der jeweiligen Plattform nicht vertraut sind. Unten ist als Hilfe eine einfache Übersicht angegeben.

Sehen Sie sich zuerst an, wie die grundlegenden EGL-Objekte der Direct3D-Schnittstelle zugeordnet sind:

| EGL-Abstraktion | Ähnliche Direct3D-Darstellung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EGLDisplay**  | In Direct3D (für UWP-Apps) wird das Anzeigehandle über die [**Windows::UI::CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-API (oder die **ICoreWindowInterop**-Schnittstelle, die HWND zur Verfügung stellt) abgerufen. Die Adapter- und Hardwarekonfiguration wird mit der COM-Schnittstelle [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) bzw. [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543) festgelegt.                                                                                                                                                                                                                                                           |
| **EGLSurface**  | In Direct3D werden Puffer und andere Fensterressourcen (sichtbar oder außerhalb des Bildschirms) unter Verwendung spezieller DXGI-Schnittstellen erstellt und konfiguriert. Hierzu zählt die [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)-Schnittstelle (eine Factorymusterimplementierung zum Abrufen von DXGI-Ressourcen wie die [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)-Schnittstelle (Anzeigepuffer)). Die [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)-Schnittstelle zur Darstellung des Grafikgeräts und der dazugehörigen Ressourcen wird mit der [**D3D11Device::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)-Funktion angefordert. Verwenden Sie für Renderziele die [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)-Schnittstelle. |
| **EGLContext**  | In Direct3D verwenden Sie die [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Schnittstelle, um die Grafikpipeline zu konfigurieren und Befehle dafür auszugeben.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | In Direct3D 11 erstellen und konfigurieren Sie Grafikressourcen wie Puffer, Texturen, Schablonen und Shader mit den Methoden der [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)-Schnittstelle.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

Im Folgenden ist der grundlegende Prozess zum Einrichten einer einfachen Grafikanzeige in einer Windows Store-App, der Ressourcen und des Kontexts in DXGI und Direct3D für eine UWP-App dargestellt.

1.  Rufen Sie mit [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) ein Handle für das [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt für den UI-Kernthread der App ab.
2.  Rufen Sie für UWP-Apps mit [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) eine Swapchain von der [**IDXGIAdapter2**](https://msdn.microsoft.com/library/windows/desktop/hh404537)-Schnittstelle ab. Übergeben Sie dafür den [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Verweis, den Sie im ersten Schritt abgerufen haben. Sie erhalten dafür eine [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)-Instanz zurück. Passen Sie den Bereich an Ihr Rendererobjekt und den Renderthread an.
3.  Rufen Sie [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)- und [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Instanzen ab, indem Sie die [**D3D11Device::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)-Methode aufrufen. Passen Sie auch hierfür den Bereich an Ihr Rendererobjekt an.
4.  Erstellen Sie Shader, Texturen und andere Ressourcen mithilfe der Methoden des [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)-Objekts Ihres Renderers.
5.  Definieren Sie Puffer, führen Sie Shader aus, und verwalten Sie die Pipelinephasen mithilfe der Methoden des [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Objekts Ihres Renderers.
6.  Nachdem die Pipeline ausgeführt und ein Frame in den Hintergrundpuffer gezeichnet wurde, können Sie ihn mit der [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)-Methode auf dem Bildschirm darstellen.

Ausführlichere Informationen zu diesem Prozess finden Sie unter [Erste Schritte mit DirectX-Grafiken](https://msdn.microsoft.com/library/windows/desktop/hh309467). Die restlichen Informationen in diesem Artikel beziehen sich auf diverse allgemeine Schritte zur Einrichtung und Verwaltung der Grafikpipeline.
> **Hinweis**   Windows-Desktop-Apps verfügen über unterschiedliche APIs zum Abrufen einer Direct3D-Swapchain, wie beispielsweise [**D3D11Device::CreateDeviceAndSwapChain**](https://msdn.microsoft.com/library/windows/desktop/ff476083). Sie verwenden dabei kein [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt.

 

## Abrufen eines Fensters für die Anzeige


In diesem Beispiel wird eglGetDisplay ein HWND-Objekt für eine spezielle Fensterressource der Microsoft Windows-Plattform übergeben. Andere Plattformen, wie iOS (Cocoa) von Apple und Android von Google, verfügen über andere Handles oder Verweise auf Fensterressourcen und möglicherweise auch eine völlig andere Syntax für Aufrufe. Nach dem Beschaffen einer Anzeige initialisieren Sie diese, legen die bevorzugte Konfiguration fest und erstellen eine Oberfläche mit einem Hintergrundpuffer, in den Sie zeichnen können.

Aufrufen einer Anzeige und Konfigurieren der Anzeige mit EGL

``` syntax
// Obtain an EGL display object.
EGLDisplay display = eglGetDisplay(GetDC(hWnd));
if (display == EGL_NO_DISPLAY)
{
  return EGL_FALSE;
}

// Initialize the display
if (!eglInitialize(display, &majorVersion, &minorVersion))
{
  return EGL_FALSE;
}

// Obtain the display configs
if (!eglGetConfigs(display, NULL, 0, &numConfigs))
{
  return EGL_FALSE;
}

// Choose the display config
if (!eglChooseConfig(display, attribList, &config, 1, &numConfigs))
{
  return EGL_FALSE;
}

// Create a surface
surface = eglCreateWindowSurface(display, config, (EGLNativeWindowType)hWnd, NULL);
if (surface == EGL_NO_SURFACE)
{
  return EGL_FALSE;
}
```

In Direct3D wird das Hauptfenster einer UWP-App mit dem [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt dargestellt. Dieses Objekt kann aus dem App-Objekt abgerufen werden, indem das [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)-Element im Rahmen des Initialisierungsprozesses für den „Ansichtsanbieter“ aufgerufen wird, den Sie für Direct3D erstellen. (Bei Verwendung von Direct3D-XAML-Interoperabilität nutzen Sie den Ansichtsanbieter des XAML-Frameworks.) Der Prozess zum Erstellen eines Direct3D-Ansichtsanbieters wird unter [Einrichten Ihrer DirectX-Windows Store-App für das Anzeigen einer Ansicht](https://msdn.microsoft.com/library/windows/apps/hh465077) erläutert.

Anfordern eines CoreWindow-Elements für Direct3D

``` syntax
CoreWindow::GetForCurrentThread();
```

Nachdem der [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Verweis abgerufen wurde, muss das Fenster aktiviert werden. Dabei wird die **Run**-Methode des Hauptobjekts ausgeführt und mit der Verarbeitung des Fensterereignisses begonnen. Erstellen Sie anschließend eine [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)- und eine [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Schnittstelle. Verwenden Sie diese, um die zugrunde liegenden Schnittstellen [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/ff471331) und [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) abzurufen. Dadurch können Sie ein [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)-Objekt zum Erstellen einer Swapchainressource abrufen, die auf Ihrer [**DXGI_SWAP_CHAIN_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)-Konfiguration basiert.

Konfigurieren und Festlegen der DXGI-Swapchain im CoreWindow-Element für Direct3D

``` syntax
// Called when the CoreWindow object is created (or re-created).
void SimpleDirect3DApp::SetWindow(CoreWindow^ window)
{
  // Register event handlers with the CoreWindow object.
  // ...

  // Obtain your ID3D11Device1 and ID3D11DeviceContext1 objects
  // In this example, m_d3dDevice contains the scoped ID3D11Device1 object
  // ...

  ComPtr<IDXGIDevice1>  dxgiDevice;
  // Get the underlying DXGI device of the Direct3D device.
  m_d3dDevice.As(&dxgiDevice);

  ComPtr<IDXGIAdapter> dxgiAdapter;
  dxgiDevice->GetAdapter(&dxgiAdapter);

  ComPtr<IDXGIFactory2> dxgiFactory;
  dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2), 
    &dxgiFactory);

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

  // ...

  Windows::UI::Core::CoreWindow^ window = m_window.Get();
  dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr, // Allow on all displays.
    &m_swapChainCoreWindow);
}
```

Rufen Sie die [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)-Methode auf, nachdem Sie einen Frame vorbereitet haben, um diesen anzuzeigen.

Beachten Sie, dass in Direct3D 11 keine mit EGLSurface identische Abstraktion vorhanden ist. (Es gibt die [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343)-Schnittstelle, die jedoch anders verwendet wird.) Konzeptionell am ähnlichsten ist das [**ID3D11RenderTargetView** ](https://msdn.microsoft.com/library/windows/desktop/ff476582)-Objekt, mit dem wir eine Textur ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)) als Hintergrundpuffer zuweisen, die von der Shaderpipeline genutzt wird.

Einrichten des Hintergrundpuffers für die Swapchain in Direct3D 11

``` syntax
ComPtr<ID3D11RenderTargetView>    m_d3dRenderTargetViewWin; // scoped to renderer object

// ...

ComPtr<ID3D11Texture2D> backBuffer2;
    
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&backBuffer2));

m_d3dDevice->CreateRenderTargetView(
  backBuffer2.Get(),
  nullptr,
    &m_d3dRenderTargetViewWin);
```

Es ist ratsam, diesen Code jeweils aufzurufen, wenn das Fenster erstellt oder seine Größe geändert wird. Legen Sie die Renderzielansicht beim Rendern mit [**ID3D11DeviceContext1::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) fest, bevor Sie andere Unterressourcen wie Vertexpuffer oder -Shader einrichten.

``` syntax
// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

## Erstellen eines Renderkontexts


In EGL 1.4 steht eine "Anzeige" (Display) für eine Reihe von Fensterressourcen. Normalerweise konfigurieren Sie eine "Fläche" für die Anzeige, indem Sie Attribute für das Anzeigeobjekt angeben und als Rückgabe eine Fläche erhalten. Sie erstellen einen Kontext zum Anzeigen der Inhalte der Fläche, indem Sie diesen Kontext erstellen und an die Fläche und die Anzeige binden.

Der Aufruf läuft normalerweise wie folgt ab:

-   Aufrufen von eglGetDisplay mit dem Handle zu einer Anzeige oder Fensterressource und Beschaffen eines Anzeigeobjekts
-   Initialisieren der Anzeige mit eglInitialize
-   Abrufen der verfügbaren Anzeigekonfiguration und Auswählen einer Konfiguration mit eglGetConfigs und eglChooseConfig
-   Erstellen einer Fensterfläche mit eglCreateWindowSurface
-   Erstellen eines Anzeigekontexts zum Zeichnen mit eglCreateContext
-   Binden des Anzeigekontexts an die Ansicht und die Fläche mit eglMakeCurrent

Im vorherigen Abschnitt haben wir die Elemente EGLDisplay und EGLSurface erstellt. Nun verwenden wir das EGLDisplay-Element zum Erstellen eines Kontexts und ordnen diesen Kontext der Anzeige zu, indem wir das konfigurierte EGLSurface-Element zum Parametrisieren der Ausgabe nutzen.

Beschaffen eines Renderkontexts mit EGL 1.4

```cpp
// Configure your EGLDisplay and obtain an EGLSurface here ...
// ...

// Create a drawing context from the EGLDisplay
context = eglCreateContext(display, config, EGL_NO_CONTEXT, contextAttribs);
if (context == EGL_NO_CONTEXT)
{
  return EGL_FALSE;
}   
   
// Make the context current
if (!eglMakeCurrent(display, surface, surface, context))
{
  return EGL_FALSE;
}
```

In Direct3D 11 wird ein Renderkontext mithilfe eines [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)-Objekts dargestellt. Dieses Objekt steht für den Adapter und ermöglicht das Erstellen von Direct3D-Ressourcen wie Puffer und Shader. Außerdem wird der Kontext durch das [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Objekt dargestellt, mit dem Sie die Grafikpipeline verwalten und die Shader ausführen können.

Seien Sie sich der Direct3D-Featureebenen bewusst! Diese werden zum Unterstützen älterer Direct3D-Hardwareplattformen verwendet (von DirectX 9.1 bis DirectX 11). Viele Plattformen, für die stromsparende Grafikhardware verwendet wird, wie etwa Tablet PCs, haben nur Zugriff auf DirectX 9.1-Features. Bei der älteren Grafikhardware werden die Version 9.1 bis 11 unterstützt.

Erstellen eines Renderkontexts mit DXGI und Direct3D

```cpp

// ... 

UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;
ComPtr<IDXGIDevice> dxgiDevice;

D3D_FEATURE_LEVEL featureLevels[] = 
{
        D3D_FEATURE_LEVEL_11_1,
        D3D_FEATURE_LEVEL_11_0,
        D3D_FEATURE_LEVEL_10_1,
        D3D_FEATURE_LEVEL_10_0,
        D3D_FEATURE_LEVEL_9_3,
        D3D_FEATURE_LEVEL_9_2,
        D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> d3dContext;

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for Windows Store apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &d3dContext // Returns the device immediate context.
);
```

## Zeichnen in eine Textur oder pixmap-Ressource


Zum Zeichnen in eine Textur mit OpenGL ES 2.0 konfigurieren Sie einen Pixelpuffer (PBuffer). Nachdem Sie dafür ein EGLSurface-Element konfiguriert und erstellt haben, können Sie einen Renderkontext bereitstellen und die Shaderpipeline ausführen, um in die Textur zu zeichnen.

Zeichnen in einen Pixelpuffer mit OpenGL ES 2.0

``` syntax
// Create a pixel buffer surface to draw into
EGLConfig pBufConfig;
EGLint totalpBufAttrs;

const EGLint pBufConfigAttrs[] =
{
    // Configure the pBuffer here...
};
 
eglChooseConfig(eglDsplay, pBufConfigAttrs, &pBufConfig, 1, &totalpBufAttrs);
EGLSurface pBuffer = eglCreatePbufferSurface(eglDisplay, pBufConfig, EGL_TEXTURE_RGBA); 
```

In Direct3D 11 erstellen Sie eine [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)-Ressource und machen diese zu einem Renderziel. Konfigurieren Sie das Renderziel mithilfe von [**D3D11\_RENDER\_TARGET\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476201). Wenn Sie die [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407)-Methode (oder einen ähnlichen Draw\*-Vorgang im Gerätekontext) unter Verwendung dieses Renderziels aufrufen, werden die Ergebnisse in eine Textur gezeichnet.

Zeichnen in eine Textur mit Direct3D 11

``` syntax
ComPtr<ID3D11Texture2D> renderTarget1;

D3D11_RENDER_TARGET_VIEW_DESC renderTargetDesc = {0};
// Configure renderTargetDesc here ...

m_d3dDevice->CreateRenderTargetView(
  renderTarget1.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);

// Later, in your render loop...

// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

Diese Textur kann an einen Shader übergeben werden, wenn eine Zuordnung zu einer [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628)-Schnittstelle vorhanden ist.

## Zeichnen auf den Bildschirm


Nachdem Sie mit dem EGLContext-Element den Puffer konfiguriert und die Daten aktualisiert haben, führen Sie daran gebundenen Shader aus. Zeichnen Sie die Ergebnisse mithilfe von glDrawElements in den Hintergrundpuffer. Sie können den Hintergrundpuffer anzeigen, indem Sie das eglSwapBuffers-Element aufrufen.

OpenGL ES 2.0: Zeichnen auf den Bildschirm

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

In Direct3D 11 verwenden Sie die [**IDXGISwapChain::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)-Methode, um Puffer zu konfigurieren und Shader zu binden. Anschließend rufen Sie eine der [**ID3D11DeviceContext1::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407)\*-Methoden auf, um die Shader auszuführen. Die Ergebnisse werden in ein Renderziel gezeichnet, das als Hintergrundpuffer für die Swapchain konfiguriert ist. Danach stellen Sie den Hintergrundpuffer einfach für die Anzeige dar, indem Sie **IDXGISwapChain::Present1** aufrufen.

Direct3D 11: Zeichnen auf den Bildschirm

``` syntax

m_d3dContext->DrawIndexed(
        m_indexCount,
        0,
        0);

// ...

m_swapChainCoreWindow->Present1(1, 0, &parameters);
```

## Freigeben der Grafikressourcen


In EGL geben Sie die Fensterressourcen frei, indem Sie das EGLDisplay-Element an das eglTerminate-Element übergeben.

Beenden einer Anzeige mit EGL 1.4

```cpp
EGLBoolean eglTerminate(eglDisplay);
```

In einer UWP-App können Sie das CoreWindow-Element mit der [**CoreWindow::Close**](https://msdn.microsoft.com/library/windows/apps/br208260)-Methode schließen. Diese kann jedoch nur für sekundäre UI-Fenster verwendet werden. Der primäre UI-Thread und das dazugehörige CoreWindow-Element können nicht geschlossen werden. Ihr Ablauf wird vom Betriebssystem gesteuert. Zum Schließen eines sekundären CoreWindow-Element wird jedoch das [**CoreWindow::Closed**](https://msdn.microsoft.com/library/windows/apps/br208261)-Ereignis ausgelöst.

## API-Verweiszuordnung für EGL zu Direct3D 11


| EGL-API                          | Ähnliche Direct3D 11-API bzw. Verhaltensweise                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | Nicht verfügbar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglBindTexImage                  | Rufen Sie [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521) auf, um eine 2D-Textur festzulegen.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | Von Direct3D wird kein Satz mit standardmäßigen Framepufferkonfigurationen bereitgestellt. (Konfiguration der Swapchain)                                                                                                                                                                                                                                                                                                                                                                                           |
| eglCopyBuffers                   | Rufen Sie zum Kopieren von Pufferdaten die [**ID3D11DeviceContext::CopyStructureCount**](https://msdn.microsoft.com/library/windows/desktop/ff476393)-Methode auf. Rufen Sie zum Kopieren einer Ressource die [**ID3DDeviceCOntext::CopyResource**](https://msdn.microsoft.com/library/windows/desktop/ff476392)-Methode auf.                                                                                                                                                                                                                                                      |
| eglCreateContext                 | Erstellen Sie einen Direct3D-Gerätekontext, indem Sie [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) aufrufen. Von diesem Element wird sowohl ein Handle zu einem Direct3D-Gerät als auch ein unmittelbarer Direct3D-Standardkontext ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Objekt) zurückgegeben. Sie können auch einen verzögerten Direct3D-Kontext erstellen, indem Sie für das zurückgegebene [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)-Objekt die [**ID3D11Device2::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/dn280495)-Methode aufrufen. |
| eglCreatePbufferFromClientBuffer | Alle Puffer werden als Direct3D-Unterressource gelesen und geschrieben, z. B. als [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635). Verwenden Sie zum Kopieren von einem kompatiblen Unterressourcentyp zu einem anderen Methoden wie [**ID3D11DeviceContext1:CopyResource**](https://msdn.microsoft.com/library/windows/desktop/ff476392).                                                                                                                                                                                                     |
| eglCreatePbufferSurface          | Rufen Sie zum Erstellen eines Direct3D-Geräts ohne Swapchain die statische [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)-Methode auf. Rufen Sie für eine Direct3D-Renderzielansicht die [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517)-Methode auf.                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | Rufen Sie zum Erstellen eines Direct3D-Geräts ohne Swapchain die statische [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)-Methode auf. Rufen Sie für eine Direct3D-Renderzielansicht die [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517)-Methode auf.                                                                                                                                                                                                                               |
| eglCreateWindowSurface           | Fordern Sie eine [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)-Schnittstelle (für die Anzeigepuffer) und eine [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)-Schnittstelle (eine virtuelle Schnittstelle für das Grafikgerät und dessen Ressourcen) an. Definieren Sie über die **ID3D11Device1**-Schnittstelle eine [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)-Schnittstelle, die Sie zum Erstellen des Framepuffer für die **IDXGISwapChain1**-Schnittstelle erstellen können.                                                                                         |
| eglDestroyContext                | Nicht verfügbar. Verwenden Sie zum Verwerfen einer Renderzielansicht die [**ID3D11DeviceContext::DiscardView1**](https://msdn.microsoft.com/library/windows/desktop/jj247573)-Methode. Legen Sie die Instanz zum Schließen der übergeordneten [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Schnittstelle auf null fest, und warten Sie, bis die Ressourcen von der Plattform wieder übernommen wurden. Sie haben keine Möglichkeit, den Gerätekontext direkt zu zerstören.                                                                                                                                                |
| eglDestroySurface                | Nicht verfügbar. Grafikressourcen werden bereinigt, wenn das CoreWindow-Element der UWP-App von der Plattform geschlossen wird.                                                                                                                                                                                                                                                                                                                                                                                                 |
| eglGetCurrentDisplay             | Rufen Sie die [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)-Methode auf, um einen Verweis auf das aktuelle Hauptfenster der App zu erhalten.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetCurrentSurface             | Dies ist die aktuelle [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)-Schnittstelle. Normalerweise wird diese dem Rendererobjekt zugeordnet.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetError                      | Fehler gehen als HRESULTs ein. Sie werden von den meisten Methoden von DirectX-Schnittstellen zurückgegeben. Wenn die Methode kein HRESULT zurückgibt, rufen Sie die [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360)-Funktion auf. Verwenden Sie zum Konvertieren eines Systemfehlers in einen HRESULT-Wert das Makro [**HRESULT\_FROM\_WIN32**](https://msdn.microsoft.com/library/windows/desktop/ms680746).                                                                                                                                                                                                  |
| eglInitialize                    | Rufen Sie die [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)-Methode auf, um einen Verweis auf das aktuelle Hauptfenster der App zu erhalten.                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | Legen Sie mit der [**ID3D11DeviceContext1::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)-Methode ein Renderziel zum Zeichnen innerhalb des aktuellen Kontexts fest.                                                                                                                                                                                                                                                                                                                                  |
| eglQueryContext                  | Nicht verfügbar. Sie können Renderziele sowie einige Konfigurationsdaten jedoch von einer [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)-Instanz abrufen. (Unter dem Link finden Sie eine Liste der verfügbaren Methoden.)                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | Nicht verfügbar. Sie können Daten zu Viewports und zur aktuellen Grafikhardware jedoch mithilfe der Methoden einer [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)-Instanz abrufen. (Unter dem Link finden Sie eine Liste der verfügbaren Methoden.)                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | Nicht verfügbar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglReleaseThread                 | Informationen zum allgemeinen GPU-Multithreading finden Sie unter [Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | Konfigurieren Sie mit der [**D3D11\_RENDER\_TARGET\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476201)-Struktur eine Direct3D-Renderzielansicht.                                                                                                                                                                                                                                                                                                                                                               |
| eglSwapBuffers                   | Verwenden Sie die [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)-Methode.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| eglSwapInterval                  | Informationen finden Sie unter [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631).                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| eglTerminate                     | Das zum Anzeigen der Ausgabe der Grafikpipeline verwendete CoreWindow-Element wird vom Betriebssystem verwaltet.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglWaitClient                    | Verwenden Sie für gemeinsam genutzte Flächen das IDXGIKeyedMutex-Element. Informationen zum allgemeinen GPU-Multithreading finden Sie unter [Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | Verwenden Sie für gemeinsam genutzte Flächen das IDXGIKeyedMutex-Element. Informationen zum allgemeinen GPU-Multithreading finden Sie unter [Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitNative                    | Verwenden Sie für gemeinsam genutzte Flächen das IDXGIKeyedMutex-Element. Informationen zum allgemeinen GPU-Multithreading finden Sie unter [Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                    |

 

 

 







<!--HONumber=Jun16_HO4-->


