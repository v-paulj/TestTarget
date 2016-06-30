---
author: TylerMSFT
title: Behandeln des Vorabstarts von Apps
description: "Erfahren Sie, wie Sie den Vorabstart von Apps durch Überschreiben der OnLaunched-Methode behandeln."
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
ms.sourcegitcommit: 213384a194513a0f98a5f37e7f0e0849bf0a66e2
ms.openlocfilehash: d9d3bdf86d858367008a32d9d6a06ec9fc13787d

---

# Behandeln des Vorabstarts von Apps


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

Erfahren Sie, wie Sie den Vorabstart von Apps durch Überschreiben der [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode behandeln.

## Einführung


Sofern die verfügbaren Systemressourcen dies zulassen, wird die Startleistung von Windows Store-Apps verbessert, indem die vom Benutzer am häufigsten verwendeten Apps im Hintergrund proaktiv gestartet werden. Eine vorab gestartete App wird kurz nach dem Start in den angehaltenen Zustand versetzt. Wenn der Benutzer die App aufruft, wird sie fortgesetzt, indem sie vom angehaltenen Zustand in den Ausführzustand versetzt wird, was schneller als ein Kaltstart der App ist.

Vor Windows 10 waren die Vorteile des Vorabstarts nicht automatisch für Apps verfügbar. Ab Windows 10 nutzen alle universelle Windows-Plattform-Apps (UWP) automatisch das Vorabstartfeature.

Die meisten Apps sollten den Vorabstart ohne jegliche Änderungen unterstützen. Einige App-Typen müssen jedoch u. U. ihr Startverhalten ändern, damit sie reibungslos mit dem Vorabstart funktionieren. Dazu gehört beispielsweise eine Nachrichten-App, die die Onlinesichtbarkeit des Benutzers beim Starten ändert, oder ein Spiel, das davon ausgeht, dass der Benutzer anwesend ist, und beim Starten der App aufwändige visuelle Elemente anzeigt.

## Vorabstart und App-Lebenszyklus


Nachdem eine App vorab gestartet wurde, tritt sie bald in den angehaltenen Zustand ein. (siehe [Behandeln des Anhaltens von Apps](suspend-an-app.md)).

## Erkennen und Behandeln des Vorabstarts


Apps empfangen das [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740)-Kennzeichen während der Aktivierung. Verwenden Sie dieses Kennzeichen, um zu bestimmen, ob Vorgänge ausgeführt werden müssen, die nur ausgeführt werden sollten, wenn die App explizit vom Benutzer gestartet wird, wie im folgenden Auszug aus [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) veranschaulicht.

```cs
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content - rather just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        if (!e.PrelaunchActivated)
        {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation parameter
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Tipp**  Wenn Sie Vorabstarts deaktivieren möchten, aktivieren Sie das [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740)-Kennzeichen. Wenn es festgelegt ist, kehren Sie von „OnLaunched()“ zurück, bevor Sie Aufgaben zum Erstellen eines Frames ausführen oder das Fenster aktivieren.

 

## Verwenden des VisibilityChanged-Ereignisses


Eine durch den Vorabstart aktivierte App ist für den Benutzer nicht sichtbar. Sie wird sichtbar, wenn der Benutzer zur App wechselt. Möglicherweise sollen bestimmte Vorgänge verzögert werden, bis das Hauptfenster der App sichtbar wird. Wenn Ihre App z. B. eine Liste mit Neuigkeiten aus einem Feed anzeigt, könnten Sie die Liste während des [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458)-Ereignisses aktualisieren, anstatt eine Liste zu verwenden, die beim Vorabstart der App erstellt wurde und bereits veraltet sein kann, wenn der Benutzer die App aktiviert. Der folgende Code behandelt das **VisibilityChanged**-Ereignis für **MainPage**:

```cs
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than
        // when it is prelaunched, such as building a what's new feed
    }
}
```

## Erläuterung


-   Apps sollten während des Vorabstarts keine Vorgänge mit langer Ausführungsdauer ausführen, da die App beendet wird, wenn sie nicht schnell angehalten werden kann.
-   Apps sollten keine Audiowiedergabe aus [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) initiieren, wenn die App vorab gestartet wird, da die App nicht angezeigt wird und daher nicht offensichtlich ist, warum eine Audiowiedergabe stattfindet.
-   Apps sollten während des Starts keine Vorgänge ausführen, die voraussetzen, dass die App für den Benutzer sichtbar ist, oder davon ausgehen, dass die App explizit vom Benutzer gestartet wurde. Da eine App jetzt ohne explizite Benutzeraktion im Hintergrund gestartet werden kann, sollten Entwickler die Auswirkungen auf den Datenschutz, die Benutzerfreundlichkeit und Leistung berücksichtigen.
    -   Beispielsweise wird der Datenschutz beeinträchtigt, wenn eine soziale App den Benutzerstatus in „Online“ ändert. Die App sollte warten, bis der Benutzer zur App wechselt, anstatt den Status beim Vorabstart der App zu ändern.
    -   Ein Negativbeispiel für die Benutzerfreundlichkeit bietet eine App, z. B. ein Spiel, die beim Start eine Einführungssequenz präsentiert. Diese Einführungssequenz sollte verzögert werden, bis der Benutzer zur App wechselt.
    -   Nachfolgend ein Beispiel für eine Leistungsauswirkung: Um die aktuelle Wetterlage abzurufen, sollte gewartet werden, bis der Benutzer zur App wechselt. Die Infos sollten nicht beim Vorabstart der App geladen werden, weil sie erneut geladen werden müssen, wenn die App sichtbar wird, um sicherzustellen, dass die Informationen aktuell sind.
-   Wenn die Live-Kachel Ihrer App beim Start gelöscht wird, stellen Sie diese Aktion bis zum VisibilityChanged-Ereignis zurück.
-   Die Telemetrie für Ihre App sollte zwischen normalen Kachelaktivierungen und Vorabstartaktivierungen unterscheiden, damit Sie das problematische Szenario ermitteln können.
-   Wenn Sie Microsoft Visual Studio 2015 Update 1 und Windows 10, Version 1511 verwenden, können Sie den Vorabstart Ihrer App in Visual Studio 2015 simulieren, indem Sie **Debuggen**&gt;**Andere Debugziele**&gt;**Vorabstart universeller Windows-Apps debuggen** auswählen.

## Verwandte Themen

* [App-Lebenszyklus](app-lifecycle.md)

 

 



<!--HONumber=Jun16_HO4-->


