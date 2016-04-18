---
title: Erstellen und Verwenden eines App-Diensts
description: Hier erfahren Sie, wie Sie eine App für die universelle Windows-Plattform (UWP) erstellen, die Dienste für andere UWP-Apps bereitstellen kann, und wie Sie diese Dienste nutzen.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
---

# Erstellen und Verwenden eines App-Diensts


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Hier erfahren Sie, wie Sie eine App für die universelle Windows-Plattform (UWP) erstellen, die Dienste für andere UWP-Apps bereitstellen kann, und wie Sie diese Dienste nutzen.

## Erstellen eines neuen App-Dienstanbieterprojekts


In dieser Anleitung erstellen wir der Einfachheit halber alles in einer Projektmappe.

-   Erstellen Sie in Microsoft Visual Studio 2015 ein neues UWP-App-Projekt mit dem Namen „AppServiceProvider”. Klicken Sie dazu im Dialogfeld **Neues Projekt** auf **Vorlagen &gt; Andere Sprachen &gt; Visual C# &gt; Windows &gt; Windows Universal &gt; Leere App (Windows Universal)**. Dies ist die App, die den App-Dienst bereitstellt.

## Hinzufügen einer App-Diensterweiterung zu „package.appxmanifest”


Fügen Sie in der Datei „Package.appxmanifest” des Projekts „AppServiceProvider” die folgende AppService-Erweiterung zum **&lt;Application&gt;**-Element hinzu. Durch dieses Beispiel wird der `com.Microsoft.Inventory`-Dienst angekündigt und die App als App-Dienstanbieter identifiziert. Der eigentliche Dienst wird als Hintergrundaufgabe implementiert. Die App, die den App-Dienst bereitstellt, macht den Dienst für andere Apps verfügbar. Wir empfehlen, einen umgekehrten Domänennamen für den Dienstnamen zu verwenden.

``` syntax
... 
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="AppServiceProvider.App">
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
          <uap:AppService Name="com.microsoft.inventory"/>
        </uap:Extension>
      </Extensions>
      ...
    </Application>
</Applications>
```

Das **Category**-Attribut identifiziert die App als App-Dienstanbieter.

Das **EntryPoint**-Attribut identifiziert die Klasse, die den Dienst implementiert. Den Dienst implementieren wir als Nächstes.

## Erstellen des App-Diensts


1.  Ein App-Dienst wird als Hintergrundaufgabe implementiert. Dadurch kann eine Vordergrund-App einen App-Dienst in einer anderen App aufrufen, um Aufgaben im Hintergrund auszuführen. Fügen Sie der Projektmappe ein neues Projekt vom Typ „Komponente für Windows-Runtime” (**Datei &gt; Hinzufügen &gt; Neues Projekt**) mit dem Namen „MyAppService” hinzu. Klicken Sie dazu im Dialogfeld **Neues Projekt hinzufügen** auf **Installiert &gt; Andere Sprachen &gt; Visual C# &gt; Windows &gt; Windows Universal &gt; Komponente für Windows-Runtime (Windows Universal)**.
2.  Fügen Sie im Projekt „AppServiceProvider” einen Verweis auf das Projekt „MyAppService” hinzu.
3.  Fügen Sie im Projekt „MyAppService” am Anfang von „Class1.cs” die folgenden **using**-Anweisungen hinzu:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Ersetzen Sie den Stubcode für **Class1** durch eine neue Hintergrundaufgabenklasse mit dem Namen **Inventory**:

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    In dieser Klasse wird der App-Dienst ausgeführt.

    **Run()** wird aufgerufen, wenn die Hintergrundaufgabe erstellt wird. Da Hintergrundaufgaben nach Abschluss von **Run** beendet werden, implementiert der Code eine Verzögerung, damit die Hintergrundaufgabe zum Verarbeiten von Anforderungen aktiv bleibt.

    **OnTaskCanceled()** wird aufgerufen, wenn die Aufgabe abgebrochen wird. Die Aufgabe wird abgebrochen, wenn die Client-App die [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) löscht, die Client-App angehalten wird, das Betriebssystem heruntergefahren oder in den Ruhezustand geschaltet wird oder im Betriebssystem nicht genügend Ressourcen zum Ausführen der Aufgabe verfügbar sind.

## Schreiben des Codes für den App-Dienst


Der Code für den App-Dienst wird in **OnRequestedReceived()** hinzugefügt. Ersetzen Sie den Stub **OnRequestedReceived()** in der Datei „Class1.cs” von „MyAppService” durch den Code aus dem folgenden Beispiel. Dieser Code ruft einen Index für ein Bestandselement ab und übergibt ihn zusammen mit einer Befehlszeichenfolge an den Dienst, um den Namen und Preis des angegebenen Bestandselements abzurufen. Der Code zur Fehlerbehandlung wurde aus Platzgründen entfernt.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don&#39;t want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &amp;&amp;
         inventoryIndex.Value >= 0 &amp;&amp;
         inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
}
```

**OnRequestedReceived()** ist **async**, da wir in diesem Beispiel einen await-fähigen Methodenaufruf von [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) verwenden.

Es wird eine Verzögerung implementiert, damit der Dienst **async**-Methoden im OnRequestReceived-Handler verwenden kann. Dadurch wird sichergestellt, dass der Aufruf von „OnRequestReceived“ erst abgeschlossen wird, wenn die Nachricht verarbeitet wurde. [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) wird verwendet, um beim Abschluss eine Antwort zu senden. **SendResponseAsync** signalisiert den Abschluss des Aufrufs nicht. [
            **SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) wird durch den Abschluss der Verzögerung signalisiert, dass „OnRequestReceived“ abgeschlossen wurde.

App-Dienste verwenden [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) zum Austauschen von Informationen. Die Größe der Daten, die übergeben werden können, ist nur durch die Systemressourcen begrenzt. Es gibt keine vordefinierten Schlüssel zur Verwendung in Ihrem **ValueSet**. Sie müssen bestimmen, welche Schlüsselwerte Sie zum Definieren des Protokolls für Ihren App-Dienst verwenden. Dieses Protokoll muss beim Schreiben des Aufrufers berücksichtigt werden. In diesem Beispiel haben wir einen Schlüssel namens „Command“ ausgewählt, dessen Wert angibt, ob der App-Dienst den Namen des Bestandselements oder seinen Preis bereitstellen soll. Der Index des Bestandsnamens wird unter dem Schlüssel „ID“ gespeichert. Der Rückgabewert wird unter dem Schlüssel „Result“ gespeichert.

An den Aufrufer wird eine [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703)-Enumeration zurückgegeben, um anzugeben, ob der Aufruf des App-Diensts erfolgreich war. Beim Aufruf des App-Diensts könnte z. B. ein Fehler auftreten, wenn das Betriebssystem den Dienstendpunkt abbricht, keine Ressourcen mehr verfügbar sind usw. Sie können über [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) zusätzliche Fehlerinformationen zurückgeben. In diesem Beispiel verwenden wir einen Schlüssel mit dem Namen „Status“, um detailliertere Fehlerinformationen an den Aufrufer zurückzugeben.

Der Aufruf von [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) gibt [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) an den Aufrufer zurück.

## Bereitstellen der Dienstanbieter-App und Abrufen des Paketfamiliennamens


Die Dienstanbieter-App muss bereitgestellt werden, bevor Sie sie von einem Client aus aufrufen können. Außerdem benötigen Sie den Paketfamiliennamen der Dienstanbieter-App, um sie aufrufen zu können.

-   Eine Möglichkeit zum Abrufen des Paketfamiliennamens der Dienstanbieter-App besteht darin, [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) im Projekt **AppServiceProvider** aufzurufen (z. B. in `public App()` in „App.xaml.cs”) und das Ergebnis zu notieren. Um das Projekt „AppServiceProvider” in Microsoft Visual Studio auszuführen, legen Sie es im Projektmappen-Explorer als Startprojekt fest, und führen Sie das Projekt aus.
-   Eine weitere Möglichkeit zum Abrufen des Paketfamiliennamens ist das Bereitstellen der Projektmappe (**Erstellen &gt; Projektmappe bereitstellen**) und Notieren des vollständigen Paketnamens im Ausgabefenster (**Ansicht &gt; Ausgabe**). Sie müssen die Plattforminformationen aus der Zeichenfolge im Ausgabefenster entfernen, um den Paketnamen abzuleiten. Wenn im Ausgabefenster z. B. der vollständige Paketname „9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra“ angezeigt wird, extrahieren Sie „1.0.0.0\_x86\_\_“, sodass Sie den Paketfamiliennamen „9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra“ erhalten.

## Schreiben eines Clients zum Aufrufen des App-Diensts


1.  Fügen Sie der Projektmappe ein neues leeres Projekt für eine Universelle Windows-App mit dem Namen „ClientApp” hinzu (**Datei &gt; Hinzufügen &gt; Neues Projekt**). Klicken Sie dazu im Dialogfeld **Neues Projekt hinzufügen** auf **Installiert &gt; Andere Sprachen &gt; Visual C# &gt; Windows &gt; Windows Universal &gt; Leere App (Windows Universal)**.
2.  Fügen Sie im Projekt „ClientApp” am Anfang von „MainPage.xaml.cs” die folgende **using**-Anweisung hinzu:
    ```cs
    >using Windows.ApplicationModel.AppService;
    ```

3.  Fügen Sie „MainPage.xaml” ein Textfeld und eine Schaltfläche hinzu.
4.  Fügen Sie einen Klickhandler für die Schaltfläche hinzu, und fügen Sie der Signatur des Schaltflächenhandlers das Schlüsselwort **async** hinzu.
5.  Ersetzen Sie den Stub des Klickhandlers für die Schaltfläche durch den folgenden Code. Vergessen Sie nicht die `inventoryService`-Felddeklaration.

   ```cs
   private AppServiceConnection inventoryService;
    private async void button_Click(object sender, RoutedEventArgs e)
    {
        // Add the connection.
        if (this.inventoryService == null)
        {
            this.inventoryService = new AppServiceConnection();

            // Here, we use the app service name defined in the app service provider&#39;s Package.appxmanifest file in the &lt;Extension&gt; section. 
            this.inventoryService.AppServiceName = "com.microsoft.inventory";

            // Use Windows.ApplicationModel.Package.Current.Id.FamilyName within the app service provider to get this value.
            this.inventoryService.PackageFamilyName = "replace with the package family name";

            var status = await this.inventoryService.OpenAsync();
            if (status != AppServiceConnectionStatus.Success)
            {
                button.Content = "Failed to connect";
                return;
            }
        }

        // Call the service.
        int idx = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("Command", "Item");
        message.Add("ID", idx);
        AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
        string result = "";

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data  that the service sent  to us.
            if (response.Message["Status"] as string == "OK")
            {
                result = response.Message["Result"] as string;
            }
        }

        message.Clear();
        message.Add("Command", "Price");
        message.Add("ID", idx);
        response = await this.inventoryService.SendMessageAsync(message);

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data that the service sent to us.
            if (response.Message["Status"] as string == "OK")
            {
                result += " : Price = " + response.Message["Result"] as string;
            }
        }

        button.Content = result;
    }
    ```

    Replace the package family name in the line `this.inventoryService.PackageFamilyName = "replace with the package family name";` with the package family name of the **AppServiceProvider** project that you obtained in \[Step 5: Deploy the service app and get the package family name\].

    The code first establishes a connection with the app service. The connection will remain open until you dispose **this.inventoryService**. The app service name must match the **AppService Name** attribute that you added to the AppServiceProvider project's Package.appxmanifest file. In this example, it is `<uap:AppService Name="com.microsoft.inventory"/>`.

    A [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) named **message** is created to specify the command that we want to send to the app service. The example app service expects a command to indicate which of two actions to take. We get the index from the textbox in the ClientApp, and then call the service with the "Item" command to get the description of the item. Then, we make the call with the "Price" command to get the item's price. The button text is set to the result.

    Because [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) only indicates whether the operating system was able to connect the call to the app service, we check the "Status" key in the [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) we receive from the app service to ensure that it was able to fulfill the request.

6.  In Visual Studio, set the ClientApp project to be the startup project in the Solution Explorer window and run the solution. Enter the number 1 into the text box and click the button. You should get "Chair : Price = 88.99" back from the service.

    ![sample app displaying chair price=88.99](images/appserviceclientapp.png)

If the app service call fails, check the following in the ClientApp:

1.  Verify that the package family name assigned to the inventory service connection matches the package family name of the AppServiceProvider app. See: **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  In **button\_Click()**, verify that the app service name that is assigned to the inventory service connection matches the app service name in the AppServiceProvider's Package.appxmanifest file. See: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Ensure that the AppServiceProvider app has been deployed (In the Solution Explorer, right-click the solution and choose **Deploy**).

## Debug the app service


1.  Ensure that the entire solution is deployed before debugging because the app service provider app must be deployed before the service can be called. (In Visual Studio, **Build &gt; Deploy Solution**).
2.  In the Solution Explorer, right-click the AppServiceProvider project and choose **Properties**. From the **Debug** tab, change the **Start action** to **Do not launch, but debug my code when it starts**.
3.  In the MyAppService project, in the Class1.cs file, set a breakpoint in OnRequestReceived().
4.  Set the AppServiceProvider project to be the startup project and press F5.
5.  Start ClientApp from the Start menu (not from Visual Studio).
6.  Enter the number 1 into the text box and press the button. The debugger will stop in the app service call on the breakpoint in your app service.

## Debug the client


1.  Follow the instructions in the preceding step to debug the app service.
2.  Launch ClientApp from the Start menu.
3.  Attach the debugger to the ClientApp.exe process (not the ApplicationFrameHost.exe process). (In Visual Studio, choose **Debug &gt; Attach to Process...**.)
4.  In the ClientApp project, set a breakpoint in **button\_Click()**.
5.  The breakpoints in both the client and the app service will now be hit when you enter the number 1 into the text box of the ClientApp and click the button.

## Remarks


This example provides a simple introduction to creating an app service and calling it from another app. The key things to note are the creation of a background task to host the app service, the addition of the windows.appservice extension to the app service provider app's Package.appxmanifest file, obtaining the package family name of the app service provider app so that we can connect to it from the client app, and using [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) to call the service.

## Full code for MyAppService


```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don&#39;t want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &amp;&amp;
                 inventoryIndex.Value >= 0 &amp;&amp;
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## Verwandte Themen


* [Unterstützen Ihrer App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md)

 

 





<!--HONumber=Mar16_HO1-->


