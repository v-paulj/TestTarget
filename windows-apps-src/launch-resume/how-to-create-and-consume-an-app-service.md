---
author: TylerMSFT
title: Erstellen und Verwenden eines App-Diensts
description: "Hier erfahren Sie, wie Sie eine App für die Universelle Windows-Plattform (UWP) erstellen, die Dienste für andere UWP-Apps bereitstellen kann, und wie Sie diese Dienste nutzen."
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: app to app
translationtype: Human Translation
ms.sourcegitcommit: d7d7edf8d1ed6ae1c4be504cd4827bb941f14380
ms.openlocfilehash: 13b9456d1f6ee2b592db0e5e38b9f9e7fe41764c

---

# Erstellen und Verwenden eines App-Diensts


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Hier erfahren Sie, wie Sie eine App für die Universelle Windows-Plattform (UWP) erstellen, die Dienste für andere UWP-Apps bereitstellen kann, und wie Sie diese Dienste nutzen.

## Erstellen eines neuen App-Dienstanbieterprojekts


In dieser Anleitung erstellen wir der Einfachheit halber alles in einer Projektmappe.

-   Erstellen Sie in Microsoft Visual Studio2015 ein neues UWP-App-Projekt mit dem Namen „AppServiceProvider”. (Wählen Sie im Dialogfeld **Neues Projekt** **Vorlagen &gt; Andere Sprachen &gt; Visual C# &gt; Windows &gt; Windows Universal &gt; Blank App (Windows Universal)** aus.) Dies ist die App, die den App-Dienst bereitstellt.

## Hinzufügen einer App-Diensterweiterung zu „package.appxmanifest”


Fügen Sie in der Datei „Package.appxmanifest” des Projekts „AppServiceProvider” die folgende AppService-Erweiterung zum **&lt;Application&gt;**-Element hinzu. Durch dieses Beispiel wird der `com.Microsoft.Inventory`-Dienst angekündigt, und die App wird als App-Dienstanbieter identifiziert. Der eigentliche Dienst wird als Hintergrundaufgabe implementiert. Die App, die den App-Dienst bereitstellt, macht den Dienst für andere Apps verfügbar. Wir empfehlen, einen umgekehrten Domänennamen für den Dienstnamen zu verwenden.

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


1.  Ein App-Dienst wird als Hintergrundaufgabe implementiert. Dadurch kann eine Vordergrund-App einen App-Dienst in einer anderen App aufrufen, um Aufgaben im Hintergrund auszuführen. Fügen Sie der Projektmappe ein neues Projekt vom Typ „Komponente für Windows-Runtime” (**Datei &gt; Hinzufügen &gt; Neues Projekt**) mit dem Namen „MyAppService” hinzu. (Wählen Sie im Dialogfeld **Neues Projekt hinzufügen** **Installiert &gt; Andere Sprachen &gt; Visual C# &gt; Windows &gt; Windows Universal &gt; Komponente für Windows-Runtime (Windows Universal)** aus.)
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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
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
    // and we don't want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &&
         inventoryIndex.Value >= 0 &&
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
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
}
```

**OnRequestedReceived()** ist **async**, da wir in diesem Beispiel einen await-fähigen Methodenaufruf von [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) verwenden.

Es wird eine Verzögerung implementiert, damit der Dienst **async**-Methoden im OnRequestReceived-Handler verwenden kann. Dadurch wird sichergestellt, dass der Aufruf von „OnRequestReceived“ erst abgeschlossen wird, wenn die Nachricht verarbeitet wurde. 
            [
              **SendResponseAsync**
            ](https://msdn.microsoft.com/library/windows/apps/dn921722) wird verwendet, um beim Abschluss eine Antwort zu senden. 
            **SendResponseAsync** signalisiert nicht den Abschluss des Aufrufs. [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) wird durch den Abschluss der Verzögerung signalisiert, dass „OnRequestReceived“ abgeschlossen wurde.

App-Dienste verwenden [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) zum Austauschen von Informationen. Die Größe der Daten, die übergeben werden können, ist nur durch die Systemressourcen begrenzt. Es gibt keine vordefinierten Schlüssel zur Verwendung in Ihrem **ValueSet**. Sie müssen bestimmen, welche Schlüsselwerte Sie zum Definieren des Protokolls für Ihren App-Dienst verwenden. Dieses Protokoll muss beim Schreiben des Aufrufers berücksichtigt werden. In diesem Beispiel haben wir einen Schlüssel namens „Command“ ausgewählt, dessen Wert angibt, ob der App-Dienst den Namen des Bestandselements oder seinen Preis bereitstellen soll. Der Index des Bestandsnamens wird unter dem Schlüssel „ID“ gespeichert. Der Rückgabewert wird unter dem Schlüssel „Result“ gespeichert.

An den Aufrufer wird eine [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703)-Enumeration zurückgegeben, um anzugeben, ob der Aufruf des App-Diensts erfolgreich war. Beim Aufruf des App-Diensts könnte z.B. ein Fehler auftreten, wenn das Betriebssystem den Dienstendpunkt abbricht, keine Ressourcen mehr verfügbar sind usw. Sie können über [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) zusätzliche Fehlerinformationen zurückgeben. In diesem Beispiel verwenden wir einen Schlüssel mit dem Namen „Status“, um detailliertere Fehlerinformationen an den Aufrufer zurückzugeben.

Der Aufruf von [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) gibt [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) an den Aufrufer zurück.

## Bereitstellen der Dienstanbieter-App und Abrufen des Paketfamiliennamens


Die Dienstanbieter-App muss bereitgestellt werden, bevor Sie sie von einem Client aus aufrufen können. Außerdem benötigen Sie den Paketfamiliennamen der Dienstanbieter-App, um sie aufrufen zu können.

-   Eine Möglichkeit zum Abrufen des Paketfamiliennamens der Dienstanbieter-App besteht darin, [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) im Projekt **AppServiceProvider** aufzurufen (z.B. in `public App()` in „App.xaml.cs”) und das Ergebnis zu notieren. Um das Projekt „AppServiceProvider” in Microsoft Visual Studio auszuführen, legen Sie es im Projektmappen-Explorer als Startprojekt fest, und führen Sie das Projekt aus.
-   Eine weitere Möglichkeit zum Abrufen des Paketfamiliennamens ist das Bereitstellen der Projektmappe (**Erstellen &gt; Projektmappe bereitstellen**) und Notieren des vollständigen Paketnamens im Ausgabefenster (**Ansicht &gt; Ausgabe**). Sie müssen die Plattforminformationen aus der Zeichenfolge im Ausgabefenster entfernen, um den Paketnamen abzuleiten. Wenn im Ausgabefenster z.B. der vollständige Paketname „9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra“ angezeigt wird, extrahieren Sie „1.0.0.0\_x86\_\_“, sodass Sie den Paketfamiliennamen „9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra“ erhalten.

## Schreiben eines Clients zum Aufrufen des App-Diensts


1.  Fügen Sie der Projektmappe ein neues leeres Projekt für eine Universelle Windows-App mit dem Namen „ClientApp” hinzu (**Datei &gt; Hinzufügen &gt; Neues Projekt**). (Wählen Sie im Dialogfeld **Neues Projekt hinzufügen** **Installiert &gt; Andere Sprachen &gt; Visual C# &gt; Windows &gt; Windows Universal &gt; Blank App (Windows Universal)** aus.)
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

            // Here, we use the app service name defined in the app service provider's Package.appxmanifest file in the <Extension> section.
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

    Ersetzen Sie den Paketfamiliennamen in der Zeile `this.inventoryService.PackageFamilyName = "replace with the package family name";` durch den Paketfamiliennamen des **AppServiceProvider**-Projekts, das Sie in \[Schritt5: Bereitstellen der Dienstanbieter-App und Abrufen des Paketfamiliennamens\] erhalten haben.

    Der Code richtet zunächst eine Verbindung mit dem App-Dienst ein. Die Verbindung bleibt offen, bis Sie **this.inventoryService** verwerfen. Der Name des App-Diensts muss mit dem Attribut **AppService-Name** übereinstimmen, das Sie der Package.appxmanifest-Datei des AppServiceProvider-Projekts hinzugefügt haben. In diesem Beispiel ist dies `<uap:AppService Name="com.microsoft.inventory"/>`.

    Es wird ein [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) mit dem Namen **message** erstellt, um den Befehl anzugeben, der an den App-Dienst gesendet werden soll. Der Beispiel-App-Dienst erwartet einen Befehl, der angibt, welche der beiden Aktionen ausgeführt werden soll. Sie erhalten den Index aus dem Textfeld in der ClientApp. Anschließend wird der Dienst mithilfe des Befehls „Item“ aufgerufen, um die Beschreibung des Elements zu erhalten. Anschließend wird der Aufruf mit dem Befehl „Price“ ausgeführt, um den Preis des Artikels zu erhalten. Der Text der Schaltfläche wird auf das Ergebnis festgelegt.

    Da [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) nur angibt, ob das Betriebssystem den Aufruf mit dem App-Dienst verknüpfen konnte, muss der Schlüssel „Status“ im [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) geprüft werden, den wir vom App-Dienst erhalten, um sicherzustellen, dass die Anforderung erfüllt werden konnte.

6.  Legen Sie das Projekt „ClientApp” in Visual Studio im Projektmappen-Explorer als Startprojekt fest, und führen Sie die Projektmappe aus. Geben Sie 1 in das Textfeld ein, und klicken Sie auf die Schaltfläche. Der Dienst sollte „Stuhl: Preis = 88,99“ zurückgeben.

    ![Beispiel-App, die Stuhlpreis = 88,99 anzeigt](images/appserviceclientapp.png)

Wenn beim Aufruf des App-Diensts ein Fehler auftritt, überprüfen Sie in „ClientApp” Folgendes:

1.  Stellen Sie sicher, dass der Paketfamilienname, der der Bestandsdienstverbindung zugewiesen ist, mit dem Paketfamiliennamen der App „AppServiceProvider” übereinstimmt. Weitere Informationen finden Sie unter **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  Überprüfen Sie in **button\_Click()**, ob der App-Dienstname, der der Bestandsdienstverbindung zugewiesen ist, dem in der Datei „Package.appxmanifest” von „AppServiceProvider” angegebenen Namen des App-Diensts entspricht. Weitere Informationen finden Sie unter `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Stellen Sie sicher, dass die App „AppServiceProvider” bereitgestellt wurde (klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und wählen Sie **Bereitstellen** aus).

## Debuggen des App-Diensts


1.  Stellen Sie vor dem Debuggen sicher, dass die gesamte Projektmappe bereitgestellt wurde. Die App Dienstanbieter-App muss bereitgestellt werden, bevor der Dienst aufgerufen werden kann. (Klicken Sie in Visual Studio auf **Erstellen &gt; Projektmappe bereitstellen**.)
2.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt „AppServiceProvider”, und wählen Sie **Eigenschaften**aus. Ändern Sie auf der Registerkarte **Debuggen** die **Startaktion** in **Eigenen Code zunächst nicht starten, sondern debuggen**.
3.  Legen Sie im Projekt „MyAppService” in der Datei „Class1.cs” einen Haltepunkt in „OnRequestReceived()” fest.
4.  Legen Sie das Projekt „AppServiceProvider” als Startprojekt fest, und drücken Sie F5.
5.  Starten Sie „ClientApp” über das Startmenü (nicht über Visual Studio).
6.  Geben Sie 1 in das Textfeld ein, und klicken Sie auf die Schaltfläche. Der Debugger stoppt im App-Dienstaufruf am Haltepunkt in Ihrem App-Dienst.

## Debuggen des Clients


1.  Folgen Sie zum Debuggen des App-Diensts den Anweisungen im vorherigen Schritt.
2.  Starten Sie „ClientApp” über das Startmenü.
3.  Hängen Sie den Debugger an den Prozess „ClientApp.exe” an (nicht an den Prozess „ApplicationFrameHost.exe”). (Klicken Sie in Visual Studio auf **Debuggen &gt; An den Prozess anhängen**.)
4.  Legen Sie im Projekt „ClientApp” einen Haltepunkt in **button\_Click()** fest.
5.  Die Haltepunkte in der Client-App und im App-Dienst werden jetzt erreicht, wenn Sie 1 in das Textfeld von „ClientApp” eingeben und auf die Schaltfläche klicken.

## Anmerkungen


Dieses Beispiel ist eine einfache Einführung in das Erstellen eines App-Diensts und Aufrufen des Diensts in einer anderen App. Die wichtigsten Punkte sind das Erstellen einer Hintergrundaufgabe zum Hosten des App-Diensts, das Hinzufügen der Erweiterung „windows.appservice” zur Datei „Package.appxmanifest” der App des App-Dienstanbieters, das Abrufen des Paketfamiliennamens der App des App-Dienstanbieters, sodass in der Client-App eine Verbindung damit hergestellt werden kann, und das Verwenden von [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) zum Aufrufen des Diensts.

## Vollständiger Code für „MyAppService”


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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
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
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
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

 

 



<!--HONumber=Jul16_HO1-->


