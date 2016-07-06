---
author: TylerMSFT
title: Behandeln der App-Fortsetzung
description: Hier erfahren Sie, wie Sie den angezeigten Inhalt aktualisieren, wenn das System die App fortsetzt.
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.sourcegitcommit: e6957dd44cdf6d474ae247ee0e9ba62bf17251da
ms.openlocfilehash: dd3d75c7f3dfe325324e1fe31c039cd207b68d0b

---

# Behandeln der App-Fortsetzung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**Fortsetzen**](https://msdn.microsoft.com/library/windows/apps/br242339)

Hier erfahren Sie, wie Sie den angezeigten Inhalt aktualisieren, wenn das System die App fortsetzt. Im Beispiel in diesem Thema wird ein Ereignishandler für das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis registriert.

**Roadmap:** Wie hängt dieses Thema mit anderen zusammen? Weitere Informationen:

-   [Roadmap für Windows-Runtime-Apps mit C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583)
-   [Roadmap für Windows-Runtime-Apps mit C++](https://msdn.microsoft.com/library/windows/apps/hh700360)

## Registrieren des „resuming“-Ereignishandlers

Registrieren Sie die Behandlung des [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignisses, mit dem angegeben wird, dass der Benutzer aus der App und wieder zurück gewechselt hat.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>    public MainPage()
>    {
>       InitializeComponent();
>       Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
>    }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>    Public Sub New()
>       InitializeComponent()
>       AddHandler Application.Current.Resuming, AddressOf App_Resuming
>    End Sub
>
> End Class
> ```
> ```cpp
> MainPage::MainPage()
> {
>     InitializeComponent();
>     Application::Current->Resuming +=
>         ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
> }
> ```

## [!div class="tabbedCodeSnippets"]

Aktualisieren der angezeigten Inhalte nach einer Unterbrechung

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     private void App_Resuming(Object sender, Object e)
>     {
>         // TODO: Refresh network data
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Resuming(sender As Object, e As Object)
>  
>         ' TODO: Refresh network data
>
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Resuming(Object^ sender, Object^ e)
> {
>     // TODO: Refresh network data
> }
> ```

> Wenn die App das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis behandelt, kann der angezeigte Inhalt aktualisiert werden.

## [!div class="tabbedCodeSnippets"]


**Hinweis**  Da das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis nicht im UI-Thread ausgelöst wird, muss mit einem Verteiler der UI-Thread aufgerufen und ein Update eingefügt werden (falls Sie dies in Ihrem Handler umsetzen möchten). Anmerkungen Das System hält Ihre App an, wenn der Benutzer zu einer anderen App oder zum Desktop wechselt. Wenn der Benutzer wieder zu Ihrer App wechselt, wird die App fortgesetzt. Beim Fortsetzen der App haben die Variablen und Datenstrukturen den gleichen Inhalt wie vor der Unterbrechung.

Das System stellt die App exakt so wieder her, wie sie unterbrochen wurde. Dadurch entsteht für den Benutzer der Eindruck, die App wäre im Hintergrund weiter ausgeführt worden.

> Da die App jedoch unter Umständen für einen längeren Zeitraum unterbrochen war, müssen sämtliche angezeigte Inhalte, die sich möglicherweise während der Unterbrechung geändert haben, aktualisiert werden. Zu solchen Inhalten zählen beispielsweise Newsfeeds oder der Standort des Benutzers. Wenn Ihre App keine Inhalte anzeigt, die aktualisiert werden müssen, muss das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis nicht behandelt werden. **Hinweis:** Wenn Ihre App an den Visual Studio-Debugger gebunden ist, können Sie ihr ein **Resume**-Ereignis senden.

> Sorgen Sie dafür, dass die Symbolleiste **Debugspeicherort** angezeigt wird, und klicken Sie auf das Dropdownelement neben dem Symbol **Anhalten**. Wählen Sie dann **Fortsetzen** aus. **Hinweis**  Für Windows Phone Store-Apps folgt auf das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis immer [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), auch wenn Ihre App derzeit angehalten ist und der Benutzer Ihre App über eine primäre Kachel oder die App-Liste neu startet.

## Apps können die Initialisierung überspringen, wenn für das aktuelle Fenster bereits Inhalte festgelegt wurden.

* [Überprüfen Sie die [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736)-Eigenschaft, um zu ermitteln, ob die App über eine primäre oder sekundäre Kachel gestartet wurde. Entscheiden Sie basierend auf dieser Information, ob die App neu gestartet oder fortgesetzt werden soll.](activate-an-app.md)
* [Verwandte Themen](suspend-an-app.md)
* [Behandeln der App-Aktivierung](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Behandeln des Anhaltens von Apps](app-lifecycle.md)



<!--HONumber=Jun16_HO5-->


