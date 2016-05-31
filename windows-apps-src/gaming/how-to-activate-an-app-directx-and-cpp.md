---
author: mtoepke
title: So wird's gemacht: Aktivieren einer App (DirectX und C++)
description: In diesem Thema erfahren Sie, wie Sie die Aktivierungsbenutzeroberfläche für eine DirectX-App für die universelle Windows-Plattform (UWP) definieren.
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
---

# So wird's gemacht: Aktivieren einer App (DirectX und C++)


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In diesem Thema erfahren Sie, wie Sie die Aktivierungsbenutzeroberfläche für eine DirectX-App für die universelle Windows-Plattform (UWP) definieren.

## Registrieren des Ereignishandlers für die Aktivierung der App


Registrieren Sie zuerst die Behandlung des [**CoreApplicationView::Activated**](https://msdn.microsoft.com/library/windows/apps/br225018)-Ereignisses, das ausgelöst wird, wenn die App vom Betriebssystem gestartet und initialisiert wird.

Fügen Sie Ihrer Implementierung der [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)-Methode Ihres Ansichtsanbieters (in diesem Beispiel **MyViewProvider**) folgenden Code hinzu:

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
    // Register event handlers for the app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);
  
  //...

}
```

## Aktivieren der CoreWindow-Instanz für die App


Beim Start der App müssen Sie einen Verweis auf die [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Instanz für Ihre App abrufen. **CoreWindow** enthält den Meldungsverteiler für Fensterereignisse, mit dem die App Fensterereignisse verarbeitet. Rufen Sie diesen Verweis im Rückruf für das App-Aktivierungsereignis durch Aufrufen von [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) ab. Wenn Sie diesen Verweis abgerufen haben, aktivieren Sie das Hauptfenster der App durch Aufrufen von [**CoreWindow::Activate**](https://msdn.microsoft.com/library/windows/apps/br208254).

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## Beginn der Verarbeitung von Ereignismeldungen für das Hauptfenster der App


Ihre Rückrufe treten als Ereignismeldungen auf, die vom [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)-Element des [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Elements der App verarbeitet werden. Dieser Rückruf wird nicht aufgerufen, wenn Sie in der Hauptschleife der App (in der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode des Ansichtsanbieters implementiert) [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) nicht aufrufen.

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## Verwandte Themen


* [So wird's gemacht: Anhalten einer App (DirectX und C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [So wird's gemacht: Reaktivieren einer App (DirectX und C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 






<!--HONumber=May16_HO2-->


