---
author: TylerMSFT
title: Behandeln des Anhaltens von Apps
description: "Hier erfahren Sie, wie Sie wichtige Anwendungsdaten speichern, wenn das System die App anhält."
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.sourcegitcommit: fb83213a4ce58285dae94da97fa20d397468bdc9
ms.openlocfilehash: 3ad58dc20a660d89622d215c46d263adf27a0542

---

# Behandeln des Anhaltens von Apps


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**Anhalten**](https://msdn.microsoft.com/library/windows/apps/br242341)

Hier erfahren Sie, wie Sie wichtige Anwendungsdaten speichern, wenn das System die App anhält. Im Beispiel wird ein Ereignishandler für das [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341)-Ereignis registriert und eine Zeichenfolge in einer Datei gespeichert.

## Registrieren des Suspending-Ereignishandlers


Registrieren Sie die Behandlung des [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341)-Ereignisses, das angibt, dass die App ihre Anwendungsdaten speichern sollte, bevor sie vom System angehalten wird.

> [!div class="tabbedCodeSnippets"]
> ```cs
> using System;
> using Windows.ApplicationModel;
> using Windows.ApplicationModel.Activation;
> using Windows.UI.Xaml;
>
> partial class MainPage
> {
>    public MainPage()
>    {
>       InitializeComponent();
>       Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
>    }
> }
> ```
> ```vb
> Public NotInheritable Class MainPage
>
>    Public Sub New()
>       InitializeComponent()
>       AddHandler Application.Current.Suspending, AddressOf App_Suspending
>    End Sub
>    
> End Class
> ```
> ```cpp
> using namespace Windows::ApplicationModel;
> using namespace Windows::ApplicationModel::Activation;
> using namespace Windows::Foundation;
> using namespace Windows::UI::Xaml;
> using namespace AppName;
>
> MainPage::MainPage()
> {
>    InitializeComponent();
>    Application::Current->Suspending +=
>        ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
> }
> ```

## [!div class="tabbedCodeSnippets"]


Speichern von App-Daten vor dem Anhalten Wenn die App das [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341)-Ereignis behandelt, kann sie die wichtigen App-Daten in der Handlerfunktion speichern.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     async void App_Suspending(
>         Object sender,
>         Windows.ApplicationModel.SuspendingEventArgs e)
>     {
>         // TODO: This is the time to save app data in case the process is terminated
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Suspending(
>         sender As Object,
>         e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending
>
>         ' TODO: This is the time to save app data in case the process is terminated
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
> {
>     // TODO: This is the time to save app data in case the process is terminated
> }
> ```

## Die App sollte die [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622)-Speicher-API verwenden, um einfache Anwendungsdaten synchron zu speichern.


[!div class="tabbedCodeSnippets"] Hinweise Das System hält Ihre App an, wenn der Benutzer zu einer anderen App oder zum Desktop bzw. auf den Startbildschirm wechselt. Wenn der Benutzer wieder zu Ihrer App wechselt, wird diese vom System fortgesetzt.

Beim Fortsetzen der App haben die Variablen und Datenstrukturen den gleichen Inhalt wie vor der Unterbrechung. Das System stellt die App exakt so wieder her, wie sie unterbrochen wurde. Dadurch entsteht für den Benutzer der Eindruck, die App wäre im Hintergrund weiter ausgeführt worden. Das System versucht, die App und die dazugehörigen Daten zu speichern, während sie angehalten ist.

Wenn das System aber nicht über die notwendigen Ressourcen verfügt, um die App im Arbeitsspeicher zu behalten, beendet es die App.

> Wenn der Benutzer wieder zu einer angehaltenen App wechselt, die beendet wurde, sendet das System ein [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018)-Ereignis, und es sollte die App-Daten in der [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode wiederherstellen. Das System benachrichtigt eine App nicht, wenn sie beendet wird. Wenn Ihre App angehalten wird, muss sie daher die App-Daten speichern und die exklusiven Ressourcen und Dateihandles freigeben, damit diese beim erneuten Aktivieren der App nach der Beendigung wiederhergestellt werden können.

> 
            **Hinweis**  Wenn Sie asynchrone Arbeiten erledigen müssen, während die App angehalten wird, müssen Sie den Abschluss des Anhaltens bis zur Fertigstellung Ihrer Arbeit verzögern. Mithilfe der [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690)-Methode im [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688)-Objekt (verfügbar über die Ereignisargumente) können Sie den Abschluss des Anhaltens verzögern, bis Sie die [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685)-Methode für das zurückgegebene [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684)-Objekt aufgerufen haben. 
            **Hinweis**  Zum Verbessern der Reaktionsfähigkeit des Systems in Windows 8.1 erhalten angehaltene Apps nur eine geringe Priorität beim Zugriff auf Ressourcen.

> Zur Unterstützung dieser neuen Priorität wird das Timeout für den Anhaltevorgang ausgedehnt, sodass die App für normale Priorität unter Windows über einen 5-Sekunden-Timeout und unter WindowsPhone über einen Timeout von 1 bis 10Sekunden verfügt. Dieses Timeout-Fenster kann weder verlängert noch geändert werden. 
            **Hinweis zum Debuggen mit Visual Studio:**  Visual Studio verhindert, dass Windows eine an den Debugger angeschlossene App anhält. Dies hat den Zweck, dem Benutzer das Anzeigen der Debugging-Benutzeroberfläche von Visual Studio zu ermöglichen, während die App ausgeführt wird.

## Beim Debuggen einer App können Sie mit VisualStudio ein Anhalteereignis an die App senden.


* [Stellen Sie sicher, dass die Symbolleiste **Debugspeicherort** angezeigt wird, und klicken Sie dann auf das Symbol **Anhalten**.](activate-an-app.md)
* [Verwandte Themen](resume-an-app.md)
* [Behandeln der App-Aktivierung](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [Behandeln der App-Fortsetzung](app-lifecycle.md)

 

 



<!--HONumber=Jun16_HO5-->


