---
author: TylerMSFT
title: "Starten einer App für Ergebnisse"
description: "Hier erfahren Sie, wie Sie eine App aus einer anderen App heraus starten und Daten zwischen den beiden Apps austauschen. Dieser Vorgang wird als Starten einer App für Ergebnisse bezeichnet."
ms.assetid: AFC53D75-B3DD-4FF6-9FC0-9335242EE327
ms.sourcegitcommit: 213384a194513a0f98a5f37e7f0e0849bf0a66e2
ms.openlocfilehash: 5826b370df3dccd1590e3f67c15126b4e78c2c32

---

# Starten einer App für Ergebnisse


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
-   [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

Hier erfahren Sie, wie Sie eine App aus einer anderen App heraus starten und Daten zwischen den beiden Apps austauschen. Dieser Vorgang wird als *Starten einer App für Ergebnisse* bezeichnet. Dieses Beispiel veranschaulicht die Verwendung von [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) zum Starten einer App für Ergebnisse.

Mit den neuen APIs für die App-zu-App-Kommunikation in Windows 10 können Windows-Apps (und Windows-Web-Apps) Apps starten und Daten und Dateien austauschen. Dies ermöglicht Ihnen das Erstellen kombinierter Lösungen aus mehreren Apps. Dank dieser neuen APIs können komplexe Aufgaben, für die Benutzer bisher mehrere Apps nutzen mussten, jetzt nahtlos durchgeführt werden. Ihrer App kann z. B. eine App für ein soziales Netzwerk starten, um einen Kontakt auszuwählen, oder eine Kassen-App, um einen Bezahlvorgang durchzuführen.

Die App, die für Ergebnisse geöffnet wird, wird als gestartete App bezeichnet. Die App, die die App startet, wird als aufrufende App bezeichnet. In diesem Beispiel schreiben Sie die aufrufende App und die gestartete App.

## Schritt 1: Registrieren des Protokolls, das in der App verarbeitet wird, die Sie für Ergebnisse starten


Fügen Sie in der Datei „Package.appxmanifest“ der gestarteten App dem Abschnitt **&lt;Application&gt;** eine Protokollerweiterung hinzu. Im folgenden Beispiel wird ein fiktives Protokoll mit dem Namen **test-app2app** verwendet.

Das **ReturnResults**-Attribut in der Protokollerweiterung akzeptiert einen der folgenden Werte:

-   **optional**: Die App kann mit der [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)-Methode für Ergebnisse oder mit der [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)-Methode nicht für Ergebnisse gestartet werden. Bei der Verwendung von **optional** muss die gestartete App ermitteln, ob sie für Ergebnisse gestartet wurde. Sie überprüft dazu das [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Ereignisargument. Wenn die [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728)-Eigenschaft des Arguments[**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693) zurückgibt oder der Typ des Ereignisarguments [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742) ist, wurde die App über **LaunchUriForResultsAsync** gestartet.
-   **always**: Die App kann nur für Ergebnisse gestartet werden, d. h. sie kann nur auf [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) reagieren.
-   **none**: Die App kann nicht für Ergebnisse gestartet werden, d. h. sie kann nur auf [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) reagieren.

Im folgenden Beispiel für eine Protokollerweiterung kann die App nur für Ergebnisse gestartet werden. Dadurch wird die im Folgenden erläuterte Logik innerhalb der **OnActivated**-Methode vereinfacht, da wir nur das „Starten für Ergebnisse“ behandeln müssen und nicht die anderen Möglichkeiten zum Aktivieren der App.

```xml
<Applications>
   <Application ...>

     <Extensions>
       <uap:Extension Category="windows.protocol">
         <uap:Protocol Name="test-app2app" ReturnResults="always">
           <uap:DisplayName>Test app-2-app</uap:DisplayName>
         </uap:Protocol>
       </uap:Extension>
     </Extensions>

   </Application>
</Applications>
```

## Schritt 2: Außerkraftsetzen von „Application.OnActivated“ in der App, die Sie für Ergebnisse starten


Wenn diese Methode in der gestarteten App noch nicht vorhanden ist, erstellen Sie sie innerhalb der in „App.xaml.cs“ definierten `App`-Klasse.

In einer App, in der Sie Freunde in einem sozialen Netzwerk auswählen können, wird mit dieser Funktion beispielsweise die Seite für die Kontaktauswahl geöffnet. Im folgenden Beispiel wird eine Seite mit dem Namen **LaunchedForResultsPage** angezeigt, wenn die App für Ergebnisse aktiviert wird. Stellen Sie sicher, dass die folgende **using**-Anweisung oben in der Datei vorhanden ist:

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnActivated(IActivatedEventArgs args)
{
    // Window management
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        rootFrame = new Frame();
        Window.Current.Content = rootFrame;
    }

    // Code specific to launch for results
    var protocolForResultsArgs = (ProtocolForResultsActivatedEventArgs)args;
    // Open the page that we created to handle activation for results.
    rootFrame.Navigate(typeof(LaunchedForResultsPage), protocolForResultsArgs);

    // Ensure the current window is active.
    Window.Current.Activate();
}
```

Da die Protokollerweiterung in der Datei „Package.appxmanifest“ **ReturnResults** als **always** angibt, kann der eben gezeigte Code `args` direkt in [**ProtocolForResultsActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn906905) umwandeln. Dabei wird sichergestellt, dass für diese App nur **ProtocolForResultsActivatedEventArgs** an **OnActivated** gesendet wird. Wenn für Ihre App andere Aktivierungsoptionen als „Starten für Ergebnisse“ verfügbar sind, können Sie überprüfen, ob die [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728)-Eigenschaft [**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693) zurückgibt, um festzustellen, ob die App für Ergebnisse gestartet wurde.

## Schritt 3: Hinzufügen eines ProtocolForResultsOperation-Felds zur App, die Sie für Ergebnisse starten


```cs
private Windows.System.ProtocolForResultsOperation _operation = null;
```

Sie verwenden das [**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913)-Feld, um zu signalisieren, wenn die gestartete App zur Zurückgabe des Ergebnisses an die aufrufende App bereit ist. In diesem Beispiel wird das Feld der **LaunchedForResultsPage**-Klasse hinzugefügt, da wir den Vorgang zum Starten für Ergebnisse auf dieser Seite durchführen und Zugriff darauf benötigen.

## Schritt 4: Außerkraftsetzen von „Override OnNavigatedTo()“ in der App, die Sie für Ergebnisse starten


Überschreiben Sie die [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508)-Methode auf der Seite, die Sie anzeigen, wenn die App für Ergebnisse gestartet wird. Wenn diese Methode in der App noch nicht vorhanden ist, erstellen Sie sie innerhalb der in &lt;Seitenname&gt;.xaml.cs“ definierten Klasse. Stellen Sie sicher, dass die folgende **using**-Anweisung oben in der Datei vorhanden ist:

```cs
using Windows.ApplicationModel.Activation
```

Das [**NavigationEventArgs**](https://msdn.microsoft.com/library/windows/apps/br243285)-Objekt in der [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508)-Methode enthält die von der aufrufenden Anwendung übergebenen Daten. Die Daten sind auf 100 KB begrenzt und werden in einem [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)-Objekt gespeichert.

Im folgenden Beispielcode erwartet die gestartete App, dass sich die von der aufrufenden Anwendung gesendeten Daten in einem [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) unter einem Schlüssel mit dem Namen **TestData** befinden, da das Startprogramm der Beispiel-App entsprechend codiert ist.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var protocolForResultsArgs = e.Parameter as ProtocolForResultsActivatedEventArgs;
    // Set the ProtocolForResultsOperation field.
    _operation = protocolForResultsArgs.ProtocolForResultsOperation;

    if (protocolForResultsArgs.Data.ContainsKey("TestData"))
    {
        string dataFromCaller = protocolForResultsArgs.Data["TestData"] as string;
    }
}
...
private Windows.System.ProtocolForResultsOperation _operation = null;
```

## Schritt 5: Schreiben von Code zum Zurückgeben von Daten an die aufrufende App


In der für Ergebnisse gestarteten App verwenden Sie [**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913), um Daten an die aufrufende App zurückzugeben. Im folgenden Beispielcode wird ein [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)-Objekt erstellt, das den Wert enthält, der an die aufrufende App zurückgegeben werden soll. Anschließend wird das **ProtocolForResultsOperation**-Feld verwendet, um den Wert an die aufrufende App zu senden.

```cs
    ValueSet result = new ValueSet();
    result["ReturnedData"] = "The returned result";
    _operation.ReportCompleted(result);
```

## Schritt 6: Schreiben von Code zum Starten der App für Ergebnisse und zum Abrufen der zurückgegebenen Daten


Starten Sie die App aus einer asynchronen Methode in der aufrufenden App, wie in diesem Beispielcode dargestellt. Beachten Sie die **using**-Anweisungen, die zum Kompilieren des Codes erforderlich sind:

```cs
using System.Threading.Tasks;
using Windows.System;
...

async Task<string> LaunchAppForResults()
{
    var testAppUri = new Uri("test-app2app:"); // The protocol handled by the launched app
    var options = new LauncherOptions();
    options.TargetApplicationPackageFamilyName = "67d987e1-e842-4229-9f7c-98cf13b5da45_yd7nk54bq29ra";

    var inputData = new ValueSet();
    inputData["TestData"] = "Test data";

    string theResult = "";
    LaunchUriResult result = await Windows.System.Launcher.LaunchUriForResultsAsync(testAppUri, options, inputData);
    if (result.Status == LaunchUriStatus.Success &&
        result.Result != null &&
        result.Result.ContainsKey("ReturnedData"))
    {
        ValueSet theValues = result.Result;
        theResult = theValues["ReturnedData"] as string;
    }
    return theResult;
}
```

In diesem Beispiel wird ein [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) mit dem Schlüssel **TestData** an die gestartete App übergeben. Die gestartete App erstellt einen **ValueSet** mit einem Schlüssel mit dem Namen **ReturnedData**, der das Ergebnis enthält, das an den Aufrufer zurückgegeben wird.

Sie müssen die App, die Sie für Ergebnisse starten, erstellen und bereitstellen, bevor Sie die aufrufende App ausführen können. Andernfalls wird von [**LaunchUriResult.Status**](https://msdn.microsoft.com/library/windows/apps/dn906892) der Status **LaunchUriStatus.AppUnavailable** gemeldet.

Sie benötigen den Familiennamen der gestarteten App beim Festlegen von [**TargetApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/dn893511). Eine Möglichkeit, den Familiennamen abzurufen besteht darin, folgenden Aufruf in der gestarteten App auszuführen:

```cs
string familyName = Windows.ApplicationModel.Package.Current.Id.FamilyName;
```

## Anmerkungen


Das Beispiel in dieser Anleitung bietet eine Einführung in das Starten einer App für Ergebnisse anhand von „Hello World“. Als wichtigster Punkt ist zu beachten, dass die neue [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)-API Ihnen das asynchrone Starten einer App und die Kommunikation über die [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)-Klasse ermöglicht. Das Übergeben von Daten über einen **ValueSet** ist auf 100 KB begrenzt. Wenn Sie größere Datenmengen übergeben müssen, können Sie Dateien mithilfe der [**SharedStorageAccessManager**](https://msdn.microsoft.com/library/windows/apps/dn889985)-Klasse freigeben, um Dateitoken zu erstellen, die zwischen Apps ausgetauscht werden können. Wenn Sie beispielsweise über **ValueSet** mit dem Namen `inputData` verfügen, könnten Sie das Token in einer Datei speichern, die Sie für die gestartete App freigeben möchten:

```cs
inputData["ImageFileToken"] = SharedStorageAccessManager.AddFile(myFile);
```

Anschließend übergeben Sie sie über **LaunchUriForResultsAsync** an die gestartete App.

## Verwandte Themen


* [**LaunchUri**](https://msdn.microsoft.com/library/windows/apps/hh701476)
* [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
* [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

 

 



<!--HONumber=Jun16_HO5-->


