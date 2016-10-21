---
author: TylerMSFT
title: Behandeln der App-Fortsetzung
description: Hier erfahren Sie, wie Sie den angezeigten Inhalt aktualisieren, wenn das System die App fortsetzt.
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
translationtype: Human Translation
ms.sourcegitcommit: 231161ba576a140859952a7e9a4e8d3bd0ba4596
ms.openlocfilehash: 2813a112f9d60c5b133284903c98a152bd027bee

---

# Behandeln der App-Fortsetzung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

**Wichtige APIs**

-   [**Fortsetzen**](https://msdn.microsoft.com/library/windows/apps/br242339)

Erfahren Sie, wo Sie Ihre Benutzeroberfläche aktualisieren, wenn das System die App fortsetzt. Im Beispiel in diesem Thema wird ein Ereignishandler für das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis registriert.

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

## Aktualisieren des angezeigten Inhalts und erneutes Abrufen von Ressourcen

Das System hält Ihre App ein paar Sekunden an, nachdem der Benutzer zu einer anderen App oder zum Desktop wechselt. Wenn der Benutzer wieder zu Ihrer App wechselt, wird diese vom System fortgesetzt. Beim Fortsetzen der App haben die Variablen und Datenstrukturen den gleichen Inhalt wie vor der Unterbrechung. Das System stellt die App an der Stelle wieder her. Für den Benutzer sieht es so aus, als ob die App im Hintergrund ausgeführt wurde.

Wenn Ihre App das [**Resuming** ](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis handhabt, wird sie möglicherweise für Stunden oder Tage angehalten. Sie sollte alle Inhalte aktualisieren, die während des Anhaltens der App ggf. veraltet sind, z.B. Newsfeeds oder der Standort des Benutzers.

Dies ist auch ein guter Zeitpunkt, um alle exklusiven Ressourcen wiederzuherstellen, die Sie freigegeben haben, als Ihre App angehalten wurde, z.B. Dateihandles, Kameras, E/A-Geräte, externe Geräte und Netzwerkressourcen.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     private void App_Resuming(Object sender, Object e)
>     {
>         // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Resuming(sender As Object, e As Object)
>  
>         ' TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
>
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Resuming(Object^ sender, Object^ e)
> {
>     // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
> }
> ```

> **Hinweis:** Da das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis nicht vom UI-Thread ausgelöst wird, muss in Ihrem Handler ein Dispatcher verwendet werden, um Aufrufe für Ihre UI zu entsenden.

## Anmerkungen

Wenn Ihre App an den Visual Studio-Debugger gebunden ist, wird sie nicht angehalten. Sie können sie aber vom Debugger anhalten und dann ein **Resume**-Ereignis senden, damit Sie den Code debuggen können. Sorgen Sie dafür, dass die Symbolleiste **Debugspeicherort** angezeigt wird, und klicken Sie auf das Dropdownelement neben dem Symbol **Anhalten**. Wählen Sie dann **Fortsetzen** aus.

Für Windows Phone Store-Apps folgt auf das [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)-Ereignis immer [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), auch wenn Ihre App derzeit angehalten ist und der Benutzer Ihre App über eine primäre Kachel oder die App-Liste neu startet. Apps können die Initialisierung überspringen, wenn für das aktuelle Fenster bereits Inhalte festgelegt wurden. Überprüfen Sie die [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736)-Eigenschaft, um zu ermitteln, ob die App über eine primäre oder sekundäre Kachel gestartet wurde. Entscheiden Sie basierend auf dieser Information, ob die App neu gestartet oder fortgesetzt werden soll.

## Verwandte Themen

* [App-Lebenszyklus](app-lifecycle.md)
* [Behandeln der App-Aktivierung](activate-an-app.md)
* [Behandeln des Anhaltens von Apps](suspend-an-app.md)



<!--HONumber=Aug16_HO3-->


