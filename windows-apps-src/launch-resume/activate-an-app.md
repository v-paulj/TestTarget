---
author: TylerMSFT
title: Behandeln der App-Aktivierung
description: "Erfahren Sie, wie Sie die App-Aktivierung durch Überschreiben der OnLaunched-Methode behandeln."
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
translationtype: Human Translation
ms.sourcegitcommit: a1bb0d5d24291fad1acab41c149dd9d763610907
ms.openlocfilehash: e41a683026a4543545556e98f6b4e9194099b362

---

# Behandeln der App-Aktivierung


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].


Erfahren Sie, wie Sie die App-Aktivierung durch Überschreiben der [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode behandeln.

## Überschreiben des Starthandlers

Beim Aktivieren einer App – gleich aus welchem Grund – wird vom System das [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018)-Ereignis gesendet. Eine Liste der Aktivierungstypen finden Sie in der [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693)-Enumeration.

Die [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)-Klasse definiert Methoden, die außer Kraft gesetzt werden können, um die verschiedenen Aktivierungstypen zu behandeln. Verschiedene Aktivierungstypen verfügen über eine spezifische Methode, die außer Kraft gesetzt werden kann. Setzen Sie für die übrigen Aktivierungstypen die [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Methode außer Kraft.

Definieren Sie die Klasse für Ihre Anwendung.

```xml
<Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AppName.App" >
```

Überschreiben Sie die [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode. Diese Methode wird immer dann aufgerufen, wenn der Benutzer die App startet. Der [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731)-Parameter enthält den vorherigen Status der App sowie die Aktivierungsargumente.

**Hinweis**  Für Windows Phone Store-Apps wird diese Methode jedes Mal aufgerufen, wenn der Benutzer die App über die Startkachel oder App-Liste startet. Dies ist auch dann der Fall, wenn die App derzeit im Arbeitsspeicher angehalten ist. Unter Windows wird beim Starten einer angehaltenen App über die Startkachel oder die App-Liste diese Methode nicht aufgerufen.

> [!div class="tabbedCodeSnippets"]
> ```cs
> using System;
> using Windows.ApplicationModel.Activation;
> using Windows.UI.Xaml;
>
> namespace AppName
> {
>    public partial class App
>    {
>       async protected override void OnLaunched(LaunchActivatedEventArgs args)
>       {
>          EnsurePageCreatedAndActivate();
>       }
>
>       // Creates the MainPage if it isn't already created.  Also activates
>       // the window so it takes foreground and input focus.
>       private MainPage EnsurePageCreatedAndActivate()
>       {
>          if (Window.Current.Content == null)
>          {
>              Window.Current.Content = new MainPage();
>          }
>
>          Window.Current.Activate();
>          return Window.Current.Content as MainPage;
>       }
>    }
> }
> ```
> ```vb
> Class App
>    Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
>       Window.Current.Content = New MainPage()
>       Window.Current.Activate()
>    End Sub
> End Class
> ```
> ```cpp
> using namespace Windows::ApplicationModel::Activation;
> using namespace Windows::Foundation;
> using namespace Windows::UI::Xaml;
> using namespace AppName;
> void App::OnLaunched(LaunchActivatedEventArgs^ args)
> {
>    EnsurePageCreatedAndActivate();
> }
>
> // Creates the MainPage if it isn't already created.  Also activates
> // the window so it takes foreground and input focus.
> void App::EnsurePageCreatedAndActivate()
> {
>     if (_mainPage == nullptr)
>     {
>         // Save the MainPage for use if we get activated later
>         _mainPage = ref new MainPage();
>     }
>     Window::Current->Content = _mainPage;
>     Window::Current->Activate();
> }
> ```

## Wiederherstellen von App-Daten, wenn die App angehalten und dann beendet wurde


Wenn der Benutzer zur beendeten App wechselt, sendet das System das [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018)-Ereignis, wobei [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) auf **Launch** und [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) auf **Terminated** oder **ClosedByUser** festgelegt ist. Von der App werden die gespeicherten Anwendungsdaten geladen, und der angezeigte Inhalt wird aktualisiert.

> [!div class="tabbedCodeSnippets"]
> ```cs
> async protected override void OnLaunched(LaunchActivatedEventArgs args)
> {
>    if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
>        args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```
> ```vb
> Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
>    Dim restoreState As Boolean = False
>
>    Select Case args.PreviousExecutionState
>       Case ApplicationExecutionState.Terminated
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case ApplicationExecutionState.ClosedByUser
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case Else
>          ' TODO: Populate the UI with defaults
>    End Select
>
>    Window.Current.Content = New MainPage(restoreState)
>    Window.Current.Activate()
> End Sub
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
> {
>    if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
>        args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```

Wenn der Wert von [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) gleich **NotRunning** ist, konnten die Anwendungsdaten von der App nicht erfolgreich gespeichert werden. Die App muss in diesem Fall neu gestartet werden, als ob sie erstmalig gestartet wird.

## Anmerkungen

> **Hinweis**  Für Windows Phone Store-Apps folgt auf das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis immer [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), auch wenn Ihre App derzeit angehalten ist und der Benutzer Ihre App über eine primäre Kachel oder die App-Liste neu startet. Apps können die Initialisierung überspringen, wenn für das aktuelle Fenster bereits Inhalte festgelegt wurden. Überprüfen Sie die [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736)-Eigenschaft, um zu ermitteln, ob die App über eine primäre oder sekundäre Kachel gestartet wurde. Entscheiden Sie basierend auf dieser Information, ob die App neu gestartet oder fortgesetzt werden soll.

## Verwandte Themen

* [Behandeln des Anhaltens von Apps](suspend-an-app.md)
* [Behandeln der App-Fortsetzung](resume-an-app.md)
* [Richtlinien für das Anhalten und Fortsetzen von Apps](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [App-Lebenszyklus](app-lifecycle.md)

**Referenzen**

* [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)

 

 



<!--HONumber=Aug16_HO3-->


