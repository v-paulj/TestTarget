---
author: Jwmsft
Description: "Mithilfe eines Webansichtssteuerelements betten Sie eine Ansicht in Ihre App ein, die Webinhalte mit dem Microsoft Edge-Renderingmodul rendert. In einem Webansichtssteuerelement können auch Links angezeigt und verwendet werden."
title: Webansicht
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: dd947d0b55dad56fdd6c684ae236f1c31ac8da86

---

# Webansicht



Mithilfe eines Webansichtssteuerelements betten Sie eine Ansicht in Ihre App ein, die Webinhalte mit dem Microsoft Edge-Renderingmodul rendert. In einem Webansichtssteuerelement können auch Links angezeigt und verwendet werden.

**Wichtige APIs**

-   [**WebView-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227702)

## Ist dies das richtige Steuerelement?

Verwenden Sie ein Webansichtssteuerelement zum Anzeigen von grafisch ansprechenden HTML-Inhalten von einem Remotewebserver, dynamisch generiertem Code oder Inhaltsdateien in Ihrem App-Paket. Attraktive Inhalte können auch Skriptcode enthalten und zwischen dem Skript und dem Code Ihrer App kommunizieren.

## Erstellen einer Webansicht

**Ändern der Darstellung einer Webansicht**

[
              **WebView**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) ist keine [**Control**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.aspx)-Unterklasse und verfügt daher über keine Steuerelementvorlage. Sie können jedoch verschiedene Eigenschaften festlegen, um einige visuelle Aspekte der Webansicht zu steuern.
- Legen Sie zum Einschränken des Anzeigebereichs die [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx)-Eigenschaft und die [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx)-Eigenschaft fest. 
- Verwenden Sie zum Verschieben, Skalieren, Neigen und Drehen einer Webansicht die [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransform.aspx)-Eigenschaft.
- Legen Sie zum Steuern der Deckkraft der Webansicht die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx)-Eigenschaft fest.
- Legen Sie zum Angeben einer Farbe, die als Webseitenhintergrund verwendet wird, wenn der HTML-Inhalt keine Farbe vorgibt, die [**DefaultBackgroundColor**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultbackgroundcolor.aspx)-Eigenschaft fest. 

**Abrufen des Webseitentitels**

Sie können den Titel des momentan in der Webansicht angezeigten HTML-Dokuments mithilfe der [**DocumentTitle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.documenttitle.aspx)-Eigenschaft abrufen. 

**Eingabeereignisse und Aktivierreihenfolge**

Obwohl WebView keine Control-Unterklasse ist, erhält sie den Tastatureingabefokus und ist Teil der Aktivierreihenfolge. Sie stellt eine [**Focus**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.focus.aspx)-Methode sowie ein [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.gotfocus.aspx)-Ereignis und ein [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.lostfocus.aspx)-Ereignis bereit, enthält aber keine registerkartenbezogenen Eigenschaften. Ihre Position in der Aktivierreihenfolge ist identisch mit ihrer Position in der XAML-Dokumentreihenfolge. Die Aktivierreihenfolge enthält alle Elemente im Webansichtsinhalt, die den Eingabefokus erhalten können. 

Wie in der Events-Tabelle auf der Seite zur [**WebView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx)-Klasse angegeben, werden die meisten von [**UIElement**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx) geerbten Benutzereingabeereignisse (z. B. [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keydown.aspx), [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keyup.aspx) und [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx)) von der Webansicht nicht unterstützt. Stattdessen können Sie [**InvokeScriptAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) mit der JavaScript-Funktion **eval** verwenden, um die HTML-Ereignishandler zu nutzen, und **window.external.notify** aus dem HTML-Ereignishandler, um die Anwendung mithilfe von [**WebView.ScriptNotify**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx) zu benachrichtigen.

### Navigieren zum Inhalt

Die Webansicht stellt mehrere APIs zur grundlegenden Navigation zur Verfügung: [**GoBack**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goback.aspx), [**GoForward**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goforward.aspx), [**Stop**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.stop.aspx), [**Refresh**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.refresh.aspx), [**CanGoBack**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoback.aspx) und [**CanGoForward**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoforward.aspx). Mit diesen APIs können Sie Ihrer App typische Funktionen für das Webbrowsen hinzufügen. 

Legen Sie zum Einrichten des anfänglichen Inhalts der Webansicht die [**Source**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.source.aspx)-Eigenschaft in XAML fest. Der XAML-Parser konvertiert die Zeichenfolge automatisch in einen [**Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.uri.aspx). 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

Die Source-Eigenschaft kann theoretisch im Code festgelegt werden, üblicherweise verwenden Sie jedoch eine der **Navigate**-Methoden, um Inhalt in den Code zu laden. 

Verwenden Sie zum Laden des Webinhalts die [**Navigate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigate.aspx)-Methode mit einem **Uri**, der das HTTP- oder HTTPS-Schema verwendet. 

```csharp
webView1.Navigate("http://www.contoso.com");
```

Um zu einem URI mit POST-Anforderung und HTTP-Headern zu navigieren, verwenden Sie die [**NavigateWithHttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage.aspx)-Methode. Die Methode unterstützt nur [**HttpMethod.Post**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.post.aspx) und [**HttpMethod.Get**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.get.aspx) als Wert der [**HttpRequestMessage.Method**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httprequestmessage.method.aspx)-Eigenschaft. 

Um nicht komprimierten und unverschlüsselten Inhalt aus den Datenspeichern [**LocalFolder**]() oder [**TemporaryFolder**]() der App zu laden, verwenden Sie die **Navigate**-Methode mit einem **Uri**, der das [ms-appdata scheme]() verwendet. Damit die Webansicht für dieses Schema unterstützt wird, müssen Sie Ihren Inhalt in einem Unterordner des lokalen oder temporären Ordners platzieren. Dies ermöglicht die Navigation zu URIs wie „ms-appdata:///local/*folder*/*file*.html“ und „ms-appdata:///temp/*folder*/*file*.html“. (Informationen zum Laden komprimierter oder verschlüsselter Dateien finden Sie unter [**NavigateToLocalStreamUri**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx).) 

Jeder dieser Unterordner auf oberster Ebene ist vom Inhalt anderer Unterordner auf oberster Ebene isoliert. Beispielsweise können Sie zu „ms-appdata:///temp/Ordner1/Datei.html“ navigieren, aber keinen Link zu „ms-appdata:///temp/Ordner2/Datei.html“ in die Datei aufnehmen. Sie können aber trotzdem eine Verknüpfung mit HTML-Inhalt im App-Paket erstellen, indem Sie das **Schema „ms-appx-web“** verwenden, und mit Webinhalt, indem Sie die URI-Schemas **http** und **https** verwenden.

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

Verwenden Sie zum Laden von Inhalt aus Ihrem App-Paket die **Navigate**-Methode mit einem **Uri**, der das [**Schema „ms-appx-web“**](https://msdn.microsoft.com/library/windows/apps/xaml/jj655406.aspx#ms_appx_web) verwendet. 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

Sie können lokalen Inhalt über einen benutzerdefinierten Resolver laden, indem Sie die [**NavigateToLocalStreamUri**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx)-Methode verwenden. Diese Vorgehensweise ermöglicht erweiterte Szenarien wie das Herunterladen und Zwischenspeichern webbasierter Inhalte für die Offlineverwendung oder das Extrahieren von Inhalten aus einer komprimierten Datei.

### Reagieren auf Navigationsereignisse

Das Webansichtssteuerelement stellt mehrere Ereignisse bereit, mit denen Sie auf Zustände bei der Navigation und beim Laden von Inhalten reagieren können. Die Ereignisse treten in der folgenden Reihenfolge für den Webansichtsinhalt im Stammpfad auf: [**NavigationStarting**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationstarting.aspx), [**ContentLoading**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.contentloading.aspx), [**DOMContentLoaded**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.domcontentloaded.aspx), [**NavigationCompleted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationcompleted.aspx)


**NavigationStarting** – tritt ein, bevor die Webansicht zu neuem Inhalt navigiert. Sie können die Navigation in einem Handler für dieses Ereignis abbrechen, indem Sie die WebViewNavigationStartingEventArgs.Cancel-Eigenschaft auf „true“ festlegen. 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** – tritt ein, nachdem die Webansicht mit dem Laden neuer Inhalte begonnen hat. 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded** – tritt ein, nachdem die Webansicht die Analyse des aktuellen HTML-Inhalts beendet hat. 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted** – tritt ein, nachdem die Webansicht das Laden des aktuellen Inhalts beendet hat oder ein Navigationsfehler aufgetreten ist. Um festzustellen, ob ein Navigationsfehler aufgetreten ist, überprüfen Sie die [**IsSuccess**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess.aspx)-Eigenschaft und die [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus.aspx)-Eigenschaft der [**WebViewNavigationCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.aspx)-Klasse. 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

Ähnliche Ereignisse treten für jeden **iframe** im Webansichtsinhalt in der gleichen Reihenfolge auf: 
- [
              **FrameNavigationStarting**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationstarting.aspx) – tritt ein, bevor ein Frame in einer Webansicht zu neuem Inhalt navigiert. 
- [
              **FrameContentLoading**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framecontentloading.aspx) – tritt ein, nachdem ein Frame in der Webansicht mit dem Laden neuer Inhalte begonnen hat. 
- [
              **FrameDOMContentLoaded**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framedomcontentloaded.aspx) – tritt ein, nachdem ein Frame in der Webansicht die Analyse seines aktuellen HTML-Inhalts beendet hat. 
- [
              **FrameNavigationCompleted**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationcompleted.aspx) – tritt ein, nachdem ein Frame in der Webansicht das Laden seines Inhalts beendet hat. 

### Reagieren auf mögliche Probleme

Sie können auf potenzielle Inhaltsprobleme reagieren, z. B. auf Skripts mit langer Ausführungszeit, von der Webansicht nicht ladbare Inhalte und Warnungen vor unsicherem Inhalt. 

Bei der Ausführung von Skripts scheint die App nicht mehr zu reagieren. Das [**LongRunningScriptDetected**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.longrunningscriptdetected.aspx)-Ereignis tritt regelmäßig ein, während die Webansicht JavaScript ausführt, und bietet die Möglichkeit zur Unterbrechung des Skripts. Sie stellen die bisherige Ausführungszeit des Skripts mithilfe der [**ExecutionTime**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime.aspx)-Eigenschaft von [**WebViewLongRunningScriptDetectedEventArgs**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.aspx) fest. Legen Sie zum Anhalten der Skriptausführung die [**StopPageScriptExecution**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution.aspx)-Eigenschaft für die Ereignisargumente auf **true** fest. Das angehaltene Skript wird erst wieder ausgeführt, wenn es in einer späteren Navigation erneut in der Webansicht geladen wird. 

Das Webansichtssteuerelement kann keine beliebigen Dateitypen hosten. Wenn das Laden von Inhalten versucht wird, die von der Webansicht nicht gehostet werden können, tritt das [**UnviewableContentIdentified**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unviewablecontentidentified.aspx)-Ereignis ein. Sie können das Ereignis behandeln und den Benutzer benachrichtigen oder die Datei mithilfe der [**Launcher**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.system.launcher.aspx)-Klasse an einen externen Browser oder eine andere App umleiten.

Entsprechend tritt das [**UnsupportedUriSchemeIdentified**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsupportedurischemeidentified.aspx)-Ereignis ein, wenn ein nicht unterstütztes URI-Schema wie „fbconnect://“ oder „mailto://“ im Webinhalt aufgerufen wird. Sie können das Ereignis so behandeln, dass es ein benutzerdefiniertes Verhalten zeigt, anstatt dem Standard-Systemstartprogramm das Starten des URIs zu erlauben.

[
            **UnsafeContentWarningDisplayingevent**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying.aspx) tritt ein, wenn die Webansicht eine Warnungsseite für Inhalte anzeigt, die vom SmartScreen-Filter als unsicher gemeldet wurden. Wenn sich der Benutzer entscheidet, die Navigation fortzusetzen, wird bei einer späteren Navigation zu der Seite weder die Warnung angezeigt noch das Ereignis ausgelöst.

### Behandeln von Sonderfällen für Webansichtsinhalte

Sie können die [ **ContainsFullScreenElement** ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelement.aspx)-Eigenschaft und das [ **ContainsFullScreenElementChanged** ](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelementchanged.aspx)-Ereignis verwenden, um Vollbilddarstellungen von Webinhalt (z. B. Videowiedergabe im Vollbildmodus) zu erkennen, darauf zu reagieren und sie zu aktivieren. Mit dem ContainsFullScreenElementChanged-Ereignis können Sie die Größe der Webansicht beispielsweise so ändern, dass sie die gesamte App-Ansicht ausfüllt, oder eine App mit Fenstern in den Vollbildmodus versetzen, wenn die Webnutzung in Vollbild gewünscht wird (siehe das folgende Beispiel).

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

Sie können das [**NewWindowRequested**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.newwindowrequested.aspx)-Ereignis zur Handhabung von Situationen verwenden, in denen gehosteter Webinhalt die Anzeige eines neuen Fensters (z. B. ein Popupfenster) anfordert. Sie können ein anderes WebView-Steuerelement verwenden, um den Inhalt des angeforderten Fensters anzuzeigen.

Verwenden Sie das [**PermissionRequested**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx)-Ereignis zum Aktivieren von Webfeatures, die besondere Funktionen erfordern. Dazu gehören momentan Geolocation, IndexedDB-Speicher sowie Audio- und Videodaten des Benutzers (z. B. von einem Mikrofon oder einer Webcam). Wenn Ihre App auf den Standort oder Medien des Benutzers zugreift, müssen Sie diese Funktion trotzdem im App-Manifest deklarieren. Für eine App, die Geolocationdaten verwendet, müssen mindestens die folgenden Funktionen in „Package.appxmanifest“ deklariert werden:

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

Zusätzlich zur App, die das [**PermissionRequested**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx)-Ereignis behandelt, muss der Benutzer Standard-Systemdialogfelder für Apps genehmigen, die Standort- oder Medienfunktionen anfordern. Erst dann können diese Features aktiviert werden.

Das folgende Beispiel zeigt, wie eine App Geolocation in einer Bing-Karte aktivieren würde:

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

Wenn Ihre App eine Benutzereingabe oder andere asynchrone Vorgänge erfordert, um auf eine Berechtigungsanforderung zu reagieren, erstellen Sie mithilfe der [**Defer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx)-Methode von [**WebViewPermissionRequest**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.aspx) eine [**WebViewDeferredPermissionRequest**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewdeferredpermissionrequest.aspx), die zu einem späteren Zeitpunkt Gegenstand einer Aktion sein kann. Siehe [**WebViewPermissionRequest.Defer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx). 

Wenn sich die Benutzer sicher von einer Website abmelden müssen, die in einer Webansicht gehostet wird, oder immer dann, wenn Sicherheit Priorität hat, rufen Sie die statische Methode [**ClearTemporaryWebDataAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cleartemporarywebdataasync.aspx) auf, um den gesamten lokal zwischengespeicherten Inhalt aus einer Webansichtssitzung zu löschen. Dadurch werden böswillige Benutzer am Zugriff auf vertrauliche Daten gehindert. 

### Interaktion mit Webansichtsinhalten

Sie können mit dem Inhalt der Webansicht interagieren, indem Sie mithilfe der [**InvokeScriptAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx)-Methode Skripts aufrufen oder in den Webansichtsinhalt einfügen, und über das [**ScriptNotify**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx)-Ereignis Informationen aus dem Webansichtsinhalt zurückerhalten.

Verwenden Sie zum Aufrufen von JavaScript im Webansichtsinhalt die [**InvokeScriptAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx)-Methode. Das aufgerufene Skript kann nur Zeichenfolgenwerte zurückgeben. 

Wenn der Inhalt einer Webansicht namens `webView1` eine Funktion namens `setDate` enthält, die drei Parameter akzeptiert, können Sie sie wie folgt aufrufen. 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


Sie können **InvokeScriptAsync** mit der JavaScript-Funktion **eval** verwenden, um Inhalt in die Webseite einzufügen.

In diesem Fall wird der Text eines XAML-Textfeld (`nameTextBox.Text`) in ein div-Element einer in `webView1` gehosteten HTML-Seite geschrieben. 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

Skripts im Webansichtsinhalt können **window.external.notify** mit einem Zeichenfolgenparameter verwenden, um Informationen zurück an Ihre App zu senden. Behandeln Sie zum Empfangen dieser Nachrichten das [**ScriptNotify**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx)-Ereignis. 

Damit eine externe Webseite das **ScriptNotify**-Ereignis beim Aufrufen von „window.external.notify“ auslösen kann, müssen Sie den URI der Seite in den **ApplicationContentUriRules**-Abschnitt des App-Manifests einfügen. (Verwenden Sie dazu in Microsoft Visual Studio die Registerkarte „Inhalts-URIs“ im Designer „Package.appxmanifest“.) Die URIs in dieser Liste müssen HTTPS verwenden und dürfen Unterdomänenplatzhalter (z. B. `https://*.microsoft.com`) enthalten. Sie dürfen jedoch keine Domänenplatzhalter (z. B. `https://*.com` und `https://*.*`) enthalten. Die Manifestanforderung gilt nicht für Inhalte, die aus dem App-Paket stammen, die einen URI vom Typ „ms-local-stream:// URI“ verwenden, oder die mit [**NavigateToString**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetostring.aspx) geladen werden. 

### Zugreifen auf die Windows-Runtime in einer Webansicht

Sie können die [**AddWebAllowedObject**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx)-Methode verwenden, um eine Instanz einer systemeigenen Klasse aus einer Komponente für Windows-Runtime in den JavaScript-Kontext der Webansicht einzufügen. Dadurch wird der vollständige Zugriff auf die systemeigenen Methoden, Eigenschaften und Ereignisse des Objekts im JavaScript-Inhalt der betreffenden Webansicht gewährleistet. Die Klasse muss mit dem [**AllowForWeb**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.metadata.allowforwebattribute.aspx)-Attribut versehen werden. 

Durch den folgenden Code wird beispielsweise eine Instanz von `MyClass` eingefügt, die aus einer Komponente für Windows-Runtime in eine Webansicht importiert wurde.

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

Weitere Informationen finden Sie unter [**WebView.AddWebAllowedObject**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx). 

Zusätzlich kann vertrauenswürdigem JavaScript-Inhalt in einer Webansicht gestattet werden, direkt auf Windows-Runtime-APIs zuzugreifen. Dadurch werden leistungsfähige systemeigene Funktionen für in einer Webansicht gehostete Web-Apps bereitgestellt. Um dieses Feature zu aktivieren, muss der URI für vertrauenswürdigen Inhalt in der ApplicationContentUriRules-Whitelist der App in „Package.appxmanifest“ aufgeführt sein, wobei WindowsRuntimeAccess ausdrücklich auf „all“ festgelegt ist. 

Dieses Beispiel zeigt einen Abschnitt des App-Manifests. Hier wird einem lokalen URI Zugriff auf die Windows-Runtime gewährt. 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### Optionen für das Hosten von Webinhalten

Mit der [**WebView.Settings**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.settings.aspx)-Eigenschaft (des Typs [**WebViewSettings**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewsettings.aspx)) können Sie steuern, ob JavaScript und IndexedDB aktiviert werden. Wenn Sie beispielsweise eine Webansicht zur Anzeige streng statischen Inhalts verwenden, können Sie JavaScript deaktivieren, um die Leistung zu optimieren.

### Erfassen von Webansichtsinhalten

Um die Freigabe von Webansichtsinhalten für andere Apps zu aktivieren, verwenden Sie die [**CaptureSelectedContentToDataPackageAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync.aspx)-Methode, die den ausgewählten Inhalt als [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datapackage.aspx) zurückgibt. Diese Methode ist asynchron. Sie müssen also eine Verzögerung verwenden, um Rückgaben des [**DataRequested**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)-Ereignishandlers zu verhindern, solange der asynchrone Aufruf nicht abgeschlossen ist. 

Verwenden Sie zum Abrufen eines Vorschaubilds des aktuellen Webansichtsinhalts die [**CapturePreviewToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.capturepreviewtostreamasync.aspx)-Methode. Durch diese Methode wird ein Bild des aktuellen Inhalts erstellt und in den angegebenen Datenstrom geschrieben. 

### Threadingverhalten

Webansichtsinhalte werden auf Geräten der Desktopgerätefamilie standardmäßig im UI-Thread und auf allen anderen Geräten in anderen Bereichen gehostet. Sie können die statische [**WebView.DefaultExecutionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultexecutionmode.aspx)-Eigenschaft verwenden, um das Standardthreadingverhalten für den aktuellen Client abzufragen. Wenn erforderlich, können Sie dieses Verhalten mit dem [**WebView(WebViewExecutionMode)**](https://msdn.microsoft.com/library/windows/apps/xaml/dn932036.aspx)-Konstruktor überschreiben. 

> **Hinweis**
            &nbsp;&nbsp;Beim Hosten von Inhalten im UI-Thread mobiler Geräte können Leistungsprobleme auftreten. Wenn Sie DefaultExecutionMode ändern, sollten Sie die Leistung deshalb auf allen Zielgeräten testen.

Eine Webansicht, die Inhalte nicht im UI-Thread hostet, ist nicht mit übergeordneten Steuerelementen kompatibel, die Gesten zur Weitergabe des Webansichtssteuerelements an das übergeordnete Steuerelement (wie [**FlipView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx), [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx)) und andere verwandte Steuerelemente erfordern. Diese Steuerelemente können keine Gesten empfangen, die in der Hintergrundthread-Webansicht initiiert werden. Darüber hinaus wird der Ausdruck von Hintergrundthread-Webinhalten nicht direkt unterstützt. Sie sollten ein Element stattdessen mit der [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.aspx)-Füllung ausdrucken.

## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Empfehlungen


-   Stellen Sie sicher, dass die geladene Website für das Gerät korrekt formatiert wird und die verwendeten Farben sowie die verwendete Typografie und Navigation mit dem Rest der App konsistent sind.
-   Eingabefelder sollten in der Größe angepasst werden. Benutzern ist möglicherweise nicht bewusst, dass sie die Ansicht zum Eingeben von Text vergrößern können.
-   Wenn eine Webansicht nicht wie die restliche App aussieht, ziehen Sie alternative Steuerelemente oder Methoden in Erwägung, um relevante Aufgaben auszuführen. Wenn die Webansicht mit dem Rest der App übereinstimmt, nehmen Benutzer sie als einheitliche Benutzeroberfläche wahr.



## <span id="related_topics"></span>Verwandte Themen

* [**WebView-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227702)
 

 







<!--HONumber=Jun16_HO4-->


