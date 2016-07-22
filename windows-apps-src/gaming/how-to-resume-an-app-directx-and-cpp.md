---
author: mtoepke
title: So wird's gemacht - Reaktivieren einer App (DirectX und C++)
description: "In diesem Thema erfahren Sie, wie wichtige Anwendungsdaten wiederhergestellt werden, wenn das System Ihre DirectX-App für die Universelle Windows-Plattform (UWP) fortsetzt."
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d5383da7332c80d4337f0e0b3eef0a6851fcd527

---

# So wird's gemacht - Reaktivieren einer App (DirectX und C++)


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In diesem Thema erfahren Sie, wie wichtige Anwendungsdaten wiederhergestellt werden, wenn das System Ihre DirectX-App für die Universelle Windows-Plattform (UWP) fortsetzt.

## Registrieren des resuming-Ereignishandlers


Registrieren Sie die Behandlung des [**CoreApplication::Resuming**](https://msdn.microsoft.com/library/windows/apps/br205859)-Ereignisses, mit dem angegeben wird, dass der Benutzer aus der App und wieder zurück gewechselt hat.

Fügen Sie Ihrer Implementierung der [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)-Methode Ihres Ansichtsanbieters folgenden Code hinzu:

```cpp
// The first method is called when the IFrameworkView is being created.
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);
    
  //...

}
```

## Aktualisieren der angezeigten Inhalte nach einer Unterbrechung


Wenn die App das Resuming-Ereignis behandelt, kann der angezeigte Inhalt aktualisiert werden. Stellen Sie alle mit dem Handler für [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) gespeicherten Apps wieder her, und setzen Sie die Verarbeitung fort. Für Spieleentwickler: Wenn Sie das Audiomodul angehalten haben, muss es jetzt neu gestartet werden.

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

Dieser Rückruf tritt als Ereignismeldung auf, die vom [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)-Element des [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Elements der App verarbeitet wird. Dieser Rückruf wird nicht aufgerufen, wenn Sie in der Hauptschleife der App (in der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode des Ansichtsanbieters implementiert) [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) nicht aufrufen.

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

## Anmerkungen


Das System hält Ihre App an, wenn der Benutzer zu einer anderen App oder zum Desktop wechselt. Wenn der Benutzer wieder zu Ihrer App wechselt, wird die App fortgesetzt. Beim Fortsetzen der App haben die Variablen und Datenstrukturen den gleichen Inhalt wie vor der Unterbrechung. Das System stellt die App exakt so wieder her, wie sie unterbrochen wurde. Dadurch entsteht für den Benutzer der Eindruck, die App wäre im Hintergrund weiter ausgeführt worden. Da die App jedoch unter Umständen längere Zeit angehalten war, müssen sämtliche angezeigten Inhalte, die sich möglicherweise in der Zwischenzeit geändert haben, aktualisiert werden, und alle Rendering- und Audioverarbeitungs-Threads müssen neu gestartet werden. Wenn Sie während eines vorherigen Anhalteereignisses Spielstände gespeichert haben, müssen Sie diese nun wiederherstellen.

## Verwandte Themen

* [So wird's gemacht: Anhalten einer App (DirectX und C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [So wird's gemacht: Aktivieren einer App (DirectX und C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 







<!--HONumber=Jun16_HO4-->


