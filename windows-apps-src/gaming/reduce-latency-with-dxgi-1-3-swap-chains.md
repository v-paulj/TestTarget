---
author: mtoepke
title: Reduzieren der Latenz mit DXGI1.3-Swapchains
description: Verwenden Sie DXGI 1.3 zum Reduzieren der geltenden Framelatenz, indem Sie warten, bis die Swapchain den geeigneten Zeitpunkt signalisiert, um mit dem Rendern eines neuen Frames zu beginnen.
ms.assetid: c99b97ed-a757-879f-3d55-7ed77133f6ce
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 174e2918d54a2b03124752d009f43f0cb0c800ca

---

# Reduzieren der Latenz mit DXGI 1.3-Swapchains


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Verwenden Sie DXGI 1.3 zum Reduzieren der geltenden Framelatenz, indem Sie warten, bis die Swapchain den geeigneten Zeitpunkt zum Beginnen mit dem Rendern eines neuen Frames signalisiert. Spiele müssen in der Regel die geringstmögliche Latenz aufweisen, was den Zeitraum vom Eingang der Spielereingabe bis zur Reaktion des Spiels auf die Eingabe betrifft, indem die Anzeige aktualisiert wird. In diesem Thema wird ein Verfahren erläutert, das ab Version Direct3D11.2 verfügbar ist. Damit können Sie in Ihrem Spiel die geltende Framelatenz verringern.

## Wie kann mit dem Warten auf den Hintergrundpuffer die Latenz reduziert werden?


Bei der Flipmodell-Swapchain werden „Flips“ des Hintergrundpuffers jeweils in die Warteschlange eingereiht, wenn vom Spiel [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) aufgerufen wird. Wenn von der Renderschleife „Present()“ aufgerufen wird, blockiert das System den Thread, bis die Darstellung eines vorherigen Frames abgeschlossen ist. So wird in der Warteschlange Platz für den neuen Frame geschaffen, bevor dieser dargestellt wird. Dies verursacht zusätzliche Latenz zwischen dem Zeitpunkt, zu dem vom Spiel ein Frame gezeichnet wird, und dem Zeitpunkt, zu dem die Anzeige des Frames vom System zugelassen wird. In vielen Fällen wird vom System ein stabiles Gleichgewicht erreicht, bei dem vom Spiel zwischen dem Rendern und Darstellen des Frames immer nahezu einen ganzen zusätzlichen Frame lang abgewartet wird. Es ist besser zu warten, bis das System zum Akzeptieren eines neuen Frames bereit ist, als den Frame basierend auf den aktuellen Daten zu rendern und sofort in die Warteschlange einzureihen.

Erstellen Sie eine Swapchain mit Wartemöglichkeit, indem Sie das [**DXGI\_SWAP\_CHAIN\_FLAG\_FRAME\_LATENCY\_WAITABLE\_OBJECT**](https://msdn.microsoft.com/library/windows/desktop/bb173076)-Flag verwenden. Swapchains, die auf diese Art erstellt werden, können Ihre Renderschleife benachrichtigen, wenn das System zum Akzeptieren eines neuen Frames bereit ist. So kann das Spiel anhand der aktuellen Daten rendern und das Ergebnis sofort in die vorhandene Warteschlange einfügen.

## Schritt 1: Erstellen einer Swapchain mit Wartemöglichkeit


Geben Sie das [**DXGI\_SWAP\_CHAIN\_FLAG\_FRAME\_LATENCY\_WAITABLE\_OBJECT**](https://msdn.microsoft.com/library/windows/desktop/bb173076)-Flag an, wenn Sie [**CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) aufrufen.

```cpp
swapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT; // Enable GetFrameLatencyWaitableObject().
```

> 
            **Hinweis**  Dieses Flag kann nicht wie andere Flags mithilfe von [**ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577) hinzugefügt oder entfernt werden. Von DXGI wird ein Fehlercode zurückgegeben, wenn dieses Flag anders als bei der Erstellung der Swapchain festgelegt wird.

 

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT // Enable GetFrameLatencyWaitableObject().
    );
```

## Schritt 2: Festlegen der Framelatenz


Legen Sie die Framelatenz mit der [**IDXGISwapChain2::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/dn268313)-API fest, anstatt [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) aufzurufen.

Standardmäßig ist die Framelatenz für Swapchains mit Wartemöglichkeit auf 1 festgelegt. Dies führt zur geringstmöglichen Latenz, jedoch auch zu einer Reduzierung der CPU-GPU-Parallelität. Falls Sie eine höhere CPU-GPU-Parallelität benötigen, um 60 F/s zu erzielen – also wenn die CPU und GPU jeweils weniger als 16,7 ms pro Frame für die Verarbeitung des Renderns aufwendet, die Summe jedoch größer als 16,7 ms ist – legen Sie die Framelatenz auf 2 fest. Auf diese Weise können von der GPU Verarbeitungsschritte ausgeführt werden, die von der CPU während des vorherigen Frames in die Warteschlange eingereiht wurden, während die CPU unabhängig davon gleichzeitig Renderbefehle für den aktuellen Frame übermitteln kann.

```cpp
// Swapchains created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT flag use their
// own per-swapchain latency setting instead of the one associated with the DXGI device. The
// default per-swapchain latency is 1, which ensures that DXGI does not queue more than one frame
// at a time. This both reduces latency and ensures that the application will only render after
// each VSync, minimizing power consumption.
//DX::ThrowIfFailed(
//    swapChain2->SetMaximumFrameLatency(1)
//    );
```

## Schritt 3: Abrufen des Objekts mit Wartemöglichkeit aus der Swapchain


Rufen Sie [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://msdn.microsoft.com/library/windows/desktop/dn268309) auf, um das „wait“-Handle abzurufen. Das „wait“-Handle ist ein Zeiger auf das Objekt mit Wartemöglichkeit. Speichern Sie dieses Handle für die Verwendung durch die Renderschleife.

```cpp
// Get the frame latency waitable object, which is used by the WaitOnSwapChain method. This
// requires that swap chain be created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
// flag.
m_frameLatencyWaitableObject = swapChain2->GetFrameLatencyWaitableObject();
```

## Schritt4: Warten vor dem Rendern der einzelnen Frames


Die Renderschleife sollte warten, bis die Swapchain über das Objekt mit Wartemöglichkeit ein Signal sendet, bevor sie mit dem Rendern eines Frames beginnt. Dies gilt auch für den ersten Frame, der mit der Swapchain gerendert wird. Verwenden Sie [**WaitForSingleObjectEx**](https://msdn.microsoft.com/library/windows/desktop/ms687036), und stellen Sie das in Schritt 2 abgerufene „wait“-Handle bereit, um den Start eines Frames zu signalisieren.

Im folgenden Beispiel wird die Renderschleife aus dem DirectXLatency-Beispiel veranschaulicht:

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        // Block this thread until the swap chain is finished presenting. Note that it is
        // important to call this before the first Present in order to minimize the latency
        // of the swap chain.
        m_deviceResources->WaitOnSwapChain();

        // Process any UI events in the queue.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        // Update app state in response to any UI events that occurred.
        m_main->Update();

        // Render the scene.
        m_main->Render();

        // Present the scene.
        m_deviceResources->Present();
    }
    else
    {
        // The window is hidden. Block until a UI event occurs.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

Im folgenden Beispiel wird der WaitForSingleObjectEx-Aufruf aus dem DirectXLatency-Beispiel veranschaulicht:

```cpp
// Block the current thread until the swap chain has finished presenting.
void DX::DeviceResources::WaitOnSwapChain()
{
    DWORD result = WaitForSingleObjectEx(
        m_frameLatencyWaitableObject,
        1000, // 1 second timeout (shouldn't ever occur)
        true
        );
}
```

## Wie soll sich das Spiel verhalten, während es auf die Swapchain wartet?


Falls Ihr Spiel nicht über Aufgaben verfügt, die zu einer Blockierung der Renderschleife führen, kann die Nutzung des Wartens auf die Swapchain vorteilhaft sein, weil so Energie gespart wird. Dies ist besonders für mobile Geräte wichtig. Andernfalls können Sie das Multithreading nutzen, um Verarbeitungsschritte auszuführen, während das Spiel auf die Swapchain wartet. Unten sind einige Aufgaben aufgeführt, die vom Spiel ausgeführt werden können:

-   Verarbeiten von Netzwerkereignissen
-   Aktualisieren der künstlichen Intelligenz
-   CPU-basierte Physik
-   Rendern mit verzögertem Kontext (auf unterstützten Geräten)
-   Laden von Ressourcen

Weitere Informationen zur Programmierung mit Multithreading unter Windows finden Sie in den folgenden verwandten Themen.

## Verwandte Themen


* [DirectXLatency-Beispiel](http://go.microsoft.com/fwlink/p/?LinkID=317361)
* [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://msdn.microsoft.com/library/windows/desktop/dn268309)
* [**WaitForSingleObjectEx**](https://msdn.microsoft.com/library/windows/desktop/ms687036)
* [**Windows.System.Threading**](https://msdn.microsoft.com/library/windows/apps/br229642)
* [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334)
* [Prozesse und Threads](https://msdn.microsoft.com/library/windows/desktop/ms684841)
* [Synchronisierung](https://msdn.microsoft.com/library/windows/desktop/ms686353)
* [Verwenden von Ereignisobjekten (Windows)](https://msdn.microsoft.com/library/windows/desktop/ms686915)

 

 







<!--HONumber=Jun16_HO4-->


