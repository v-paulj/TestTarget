---
author: TylerMSFT
title: Behandeln des Vorabstarts von Apps
description: "Erfahren Sie, wie Sie den Vorabstart von Apps durch Überschreiben der OnLaunched-Methode und Aufrufen von CoreApplication.EnablePrelaunch(true) behandeln."
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
translationtype: Human Translation
ms.sourcegitcommit: ea9aa37e15dbb6c977b0c0be4f91f77f3879e622
ms.openlocfilehash: cf7cb9f81207f4f25eb8e78283079df27f83d7dc

---

# Behandeln des Vorabstarts von Apps

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

Erfahren Sie, wie Sie den Vorabstart von Apps durch außer Kraft setzen der [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode behandeln.

## Einführung

Sofern die verfügbaren Systemressourcen dies zulassen, wird die Startleistung von Windows Store-Apps auf Desktopgeräten verbessert, indem die vom Benutzer am häufigsten verwendeten Apps im Hintergrund proaktiv gestartet werden. Eine vorab gestartete App wird kurz nach dem Start in den angehaltenen Zustand versetzt. Wenn der Benutzer die App dann aufruft, wird sie fortgesetzt, indem sie vom angehaltenen Zustand in den ausgeführten Zustand versetzt wird. Dies ist schneller als ein Kaltstart der App. Der Benutzer gewinnt den Eindruck, dass die App sehr schnell startet.

Vor Windows10 waren die Vorteile des Vorabstarts nicht automatisch für Apps verfügbar. In Windows10, Version1511, wurden alle UWP (Universelle Windows-Plattform)-Apps für den Vorabstart eingerichtet. In Windows 10, Version 1607, müssen Sie den Vorabstart aktivieren, in dem Sie [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx) aufrufen. Ein empfohlener Bereich für diesen Aufruf ist `OnLaunched()` in der Nähe der `if (e.PrelaunchActivated == false)`-Überprüfung.

Ob eine App vorab gestartet wird, ist von den Systemressourcen abhängig. Wenn die Systemressourcen ausgelastet sind, werden Apps nicht vorab gestartet.

Für einige App-Typen muss u.U. das Startverhalten geändert werden, damit der Vorabstart reibungslos funktioniert. Beispielsweise kann eine App, die beim Start Musik wiedergibt, ein Spiel, das dem Benutzer beim Start aufwändige visuelle Elemente anzeigt, eine Messaging-App, welche während des Starts die Onlinesichtbarkeit des Benutzers ändert, erkennen, ob die App vorab gestartet wurde und das Startverhalten wie in den folgenden Abschnitten beschrieben ändern.

Die Standardvorlagen für XAML-Projekte (C#, VB, C++) und WinJS in passen den Vorabstart in Visual Studio2015 Update3 an.

## Vorabstart und App-Lebenszyklus

Nach dem Vorabstart wechselt die App in den angehaltenen Zustand. (siehe [Behandeln des Anhaltens von Apps](suspend-an-app.md)).

## Erkennen und Behandeln des Vorabstarts

Apps empfangen das [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740)-Kennzeichen während der Aktivierung. Verwenden Sie dieses Flag, um Code auszuführen, der nur ausgeführt werden soll, wenn der Benutzer die App explizit startet, wie im folgenden Auszug aus [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) dargestellt.

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

**Tipp**  Wenn Sie Apps für Windows10 vor Version1607 erstellen und den Vorabstart deaktivieren möchten, aktivieren Sie das Flag [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740). Wenn es festgelegt ist, kehren Sie von „OnLaunched()“ zurück, bevor Sie Aufgaben zum Erstellen eines Frames ausführen oder das Fenster aktivieren.

## Verwenden des VisibilityChanged-Ereignisses

Eine durch den Vorabstart aktivierte App ist für den Benutzer nicht sichtbar. Sie wird sichtbar, wenn der Benutzer zur App wechselt. Möglicherweise sollen bestimmte Vorgänge verzögert werden, bis das Hauptfenster der App sichtbar wird. Wenn Ihre App z.B. eine Liste mit Neuigkeiten aus einem Feed anzeigt, können Sie die Liste während des [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458)-Ereignisses aktualisieren, anstatt eine Liste zu verwenden, die beim Vorabstart der App erstellt wurde. Denn diese kann bereits veraltet sein, wenn der Benutzer die App aktiviert. Der folgende Code behandelt das **VisibilityChanged**-Ereignis für **MainPage**:

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

## Leitfaden für DirectX-Spiele

Für DirectX-Spiele sollte der Vorabstart im Allgemeinen nicht aktiviert werden, da bei vielen DirectX-Spielen Initialisierungsvorgänge ausgeführt werden, bevor der Vorabstart erkannt wird. Ab Windows-Version 1607 (Anniversary-Edition) werden Spiele nicht standardmäßig vorab gestartet.  Wenn Sie den Vorabstart für Ihr Spiel verwenden möchten, rufen Sie [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx) auf.

Wenn Ihr Spiel für eine frühere Version von Windows10 geeignet ist, können Sie den Vorabstart zum Beenden der Anwendung verwenden:

```cs
void ViewProvider::OnActivated(CoreApplicationView^ appView,IActivatedEventArgs^ args)
{
    if (args->Kind == ActivationKind::Launch)
    {
        auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args);
        if (launchArgs->PrelaunchActivated)
        {
            // Opt-out of Prelaunch
            CoreApplication::Exit();
            return;
        }
    }
}
```

## WinJS-App-Leitfaden

Wenn Ihre WinJS-App für eine frühere Version von Windows10 geeignet ist, können Sie den Vorabstart im [onactivated](https://msdn.microsoft.com/en-us/library/windows/apps/br212679.aspx)-Handler behandeln:

```js
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## Allgemeiner Leitfaden

-   Apps sollten während des Vorabstarts keine Vorgänge mit langer Ausführungsdauer ausführen, da die App beendet wird, wenn sie nicht schnell angehalten werden kann.
-   Apps sollten keine Audiowiedergabe aus [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) initiieren, wenn die App vorab gestartet wird, da die App nicht angezeigt wird und daher nicht offensichtlich ist, warum eine Audiowiedergabe stattfindet.
-   Apps sollten während des Starts keine Vorgänge ausführen, die voraussetzen, dass die App für den Benutzer sichtbar ist, oder davon ausgehen, dass die App explizit vom Benutzer gestartet wurde. Da eine App jetzt ohne explizite Benutzeraktion im Hintergrund gestartet werden kann, sollten Entwickler die Auswirkungen auf den Datenschutz, die Benutzerfreundlichkeit und Leistung berücksichtigen.
    -   Beispielsweise wird der Datenschutz beeinträchtigt, wenn eine soziale App den Benutzerstatus in „Online“ ändert. Die App sollte warten, bis der Benutzer zur App wechselt, anstatt den Status beim Vorabstart der App zu ändern.
    -   Ein Negativbeispiel für die Benutzerfreundlichkeit bietet eine App, z.B. ein Spiel, die beim Start eine Einführungssequenz präsentiert. Diese Einführungssequenz sollte verzögert werden, bis der Benutzer zur App wechselt.
    -   Hier ein Beispiel für eine Leistungsbeeinträchtigung: Um die aktuelle Wetterlage abzurufen, sollte gewartet werden, bis der Benutzer zur App wechselt. Die Infos sollten nicht beim Vorabstart der App geladen werden, weil sie erneut geladen werden müssen, wenn die App sichtbar wird, um sicherzustellen, dass die Informationen aktuell sind.
-   Wenn die Live-Kachel Ihrer App beim Start gelöscht wird, stellen Sie diese Aktion bis zum VisibilityChanged-Ereignis zurück.
-   Die Telemetrie für Ihre App sollte zwischen normalen Kachelaktivierungen und Vorabstartaktivierungen unterscheiden, damit eventuell auftretende Probleme leichter zu identifizieren sind.
-   Wenn Sie Microsoft Visual Studio 2015 Update 1 und Windows10, Version1511, verwenden, können Sie den Vorabstart Ihrer App in Visual Studio2015 simulieren, indem Sie **Debuggen** &gt; **Andere Debugziele** &gt; **Vorabstart der universellen Windows-App debuggen**auswählen.

## Verwandte Themen

* [App-Lebenszyklus](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)



<!--HONumber=Aug16_HO4-->


