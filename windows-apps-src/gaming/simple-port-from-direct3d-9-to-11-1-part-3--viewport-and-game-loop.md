---
title: Portieren der Spielschleife
description: So implementieren Sie ein Fenster für ein UWP-Spiel, übertragen die Spielschleife und erstellen ein IFrameworkView-Element zur CoreWindow-Vollbildsteuerung.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
---

# Portieren der Spielschleife


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Zusammenfassung**

-   [Teil 1: Initialisieren von Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Teil 2: Konvertieren des Renderingframeworks](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Teil 3: Portieren der Spielschleife


In diesem Thema wird veranschaulicht, wie Sie ein Fenster für ein UWP-Spiel (Universelle Windows-Plattform) implementieren und die Spielschleife übertragen. Außerdem wird die Erstellung eines [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)-Elements zum Steuern eines [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Vollbilds erläutert. Teil 3 der exemplarischen Vorgehensweise [Portieren einer einfachen Direct3D 9-App zu DirectX 11 und UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) .

## Erstellen eines Fensters


Zum Einrichten eines Desktopfensters mit einem Direct3D 9-Viewport musste das herkömmliche Fensterframework für Desktop-Apps implementiert werden. Es musste ein HWND-Element erstellt, die Fenstergröße festgelegt, ein Rückruf zur Fensterverarbeitung eingerichtet, die Sichtbarkeit hergestellt werden usw.

Dagegen verfügt die UWP-Umgebung über ein deutlich einfacheres System. Anstatt ein herkömmliches Fenster einzurichten, wird von einem Windows Store-Spiel, für das DirectX verwendet wird, das [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)-Element implementiert. Diese Schnittstelle ist für DirectX-Apps und -Spiele vorhanden, um die direkte Ausführung in einem [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) innerhalb des App-Containers zu ermöglichen.

> **Hinweis**   Von Windows werden verwaltete Zeiger auf Ressourcen bereitgestellt, z. B. das Quellanwendungsobjekt und das [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Informationen finden Sie unter [**Handle to Object Operator (^)**]https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx.

 

Ihre main-Klasse muss von [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) erben und die fünf **IFrameworkView**-Methoden implementieren: [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) und [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523). Zusätzlich zur Erstellung des **IFrameworkView**-Elements, in dem Ihr Spiel (im Wesentlichen) enthalten ist, müssen Sie eine Factoryklasse implementieren, über die eine Instanz des **IFrameworkView**-Elements erstellt wird. Das Spiel verfügt weiterhin über eine ausführbare Datei mit einer **main()**-Methode. Mithilfe von "main" kann jedoch lediglich die Factory verwendet werden, um die **IFrameworkView**-Instanz zu erstellen.

main-Funktion

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

IFrameworkView-Factory

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## Portieren der Spielschleife


Sehen wir uns die Spielschleife unserer Direct3D 9-Implementierung an. Dieser Code ist in der main-Funktion der App vorhanden. Mit jeder Iteration dieser Schleife wird eine Fenstermeldung verarbeitet oder ein Frame gerendert.

Spielschleife im Direct3D 9-Desktopspiel

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

Die Spielschleife in der UWP-Version unseres Spiels ist ähnlich, dabei jedoch einfacher aufgebaut:

Die Spielschleife befindet sich in der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode (anstatt **main()**), weil die Funktionen des Spiels über die [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)-Klasse abgewickelt werden.

Anstatt ein Framework zur Behandlung von Meldungen zu implementieren und [**PeekMessage**](https://msdn.microsoft.com/library/windows/desktop/ms644943) aufzurufen, können wir die [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)-Methode aufrufen, die in das [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)-Element des App-Fensters integriert ist. Es ist nicht erforderlich, dass die Spielschleife über Verzweigungen verfügt und Meldungen behandelt. Rufen Sie einfach **ProcessEvents** auf, und fahren Sie fort.

Spielschleife im Direct3D 11-Windows Store-Spiel

```cpp
// Windows Store apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

Nun verfügen wir über eine UWP-App, von der die gleiche grundlegende Grafikinfrastruktur eingerichtet und der gleiche farbige Würfel wie im DirectX 9-Beispiel gerendert wird.

## Wie geht es weiter?


Richten Sie ein Lesezeichen für [DirectX 11-Portierung – Häufig gestellte Fragen](directx-porting-faq.md) ein.

Die DirectX-UWP-Vorlagen enthalten eine stabile Direct3D-Geräteinfrastruktur, die bereit für die Nutzung mit Ihrem UWP-Spiel ist. Eine Anleitung zum Auswählen der richtigen Vorlage finden Sie unter [Erstellen eines DirectX-Spieleprojekts aus einer Vorlage](user-interface.md).

Lesen Sie sich die folgenden ausführlichen Artikel zur Entwicklung von Windows Store-Spielen durch:

-   [Exemplarische Vorgehensweise: Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-metro-style-directx-game.md)
-   [Audio für Spiele](working-with-audio-in-your-directx-game.md)
-   [Bewegungs-/Blicksteuerungen für Spiele](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 






<!--HONumber=Mar16_HO1-->


