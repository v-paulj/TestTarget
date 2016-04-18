---
Description: Stellen Sie Deep-Links aus dem Hintergrund-App-Dienst in Cortana bereit, um die App in einem bestimmten Zustand oder Kontext im Vordergrund zu starten.
title: Deep-Link von Cortana zu einer Hintergrund-App
ms.assetid: BE811A87-8821-476A-90E4-2E20D37E4043
label: Deep link to a background app
template: detail.hbs
---

# Deep-Link von Cortana zu einer Hintergrund-App


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD (Voice Command Definition)-Elemente und -Attribute v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Stellen Sie Deep-Links von einer Hintergrund-App in **Cortana** bereit, die die App in einem bestimmten Zustand oder Kontext im Vordergrund starten.

> **Hinweis**  
Sowohl **Cortana** als auch der Hintergrund-App-Dienst werden beendet, wenn die Vordergrund-App gestartet wird.

Standardmäßig wird auf dem **Cortana** Abschlussbildschirm wie hier dargestellt ein Deep-Link angezeigt („Zu AdventureWorks“), Sie können jedoch in verschiedenen anderen Bildschirmen Deep-Links anzeigen. 

![Cortana-Abschlussbildschirm für Hintergrund-App](images/cortana-completion-screen-upcomingtrip-small.png)

**Voraussetzungen:**

Dieses Thema baut auf [Interagieren mit einer Hintergrund-App in Cortana](interact-with-a-background-app-in-cortana.md) auf. Im Folgenden zeigen wir anhand einer Reiseplanungs- und Verwaltungs-App mit dem Namen **Adventure Works** verschiedene **Cortana**-Features.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

-   [Erstellen Ihrer ersten App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Richtlinien für die Benutzerfreundlichkeit:**

Unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233) finden Sie Informationen zur Integration Ihrer App mit **Cortana**. Unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121) finden Sie hilfreiche Tipps für den Entwurf einer nützlichen und interaktiven sprachaktivierten App.

## <span id="Overview"></span><span id="overview"></span><span id="OVERVIEW"></span>Übersicht


Benutzer können wie folgt über **Cortana** auf Ihre App zugreifen:

-   Aktivieren der App als Vordergrund-App (siehe [Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md)).
-   Verfügbarmachen bestimmter Funktionen als Hintergrund-App-Dienst (siehe [Aktivieren einer Hintergrund-App mit Sprachbefehlen über Cortana](launch-a-background-app-with-voice-commands-in-cortana.md)).
-   Erstellen von Deep-Links zu bestimmten Seiten, Inhalten, Zuständen oder Kontext.

In diesem Abschnitt wird das Erstellen von Deep-Links erläutert.

Das Erstellen von Deep-Links ist hilfreich, wenn Cortana und Ihr App-Dienst als Gateway zu Ihrer vollfunktionalen dienen (anstatt den Benutzer zum Starten der App über das Startmenü aufzufordern), oder um Zugriff auf umfangreichere Details und Funktionen in Ihrer App zu bieten, was über Cortana nicht möglich ist. Das Erstellen von Deep-Links ist eine weitere Möglichkeit zur Steigerung der Nutzbarkeit und zum Bewerben Ihrer App.

Es gibt drei Möglichkeiten zum Bereitstellen von Deep-Links:

-   Ein Link „Zur &lt;App&gt;“ in verschiedenen **Cortana**-Bildschirmen.
-   Ein in eine Inhaltskachel eingebetteter Link in verschiedenen **Cortana**-Bildschirmen.
-   Programmgesteuertes Starten der Vordergrund-App aus dem Hintergrund-App-Dienst.

## <span id="Go_to__app__deep_link"></span><span id="go_to__app__deep_link"></span><span id="GO_TO__APP__DEEP_LINK"></span>Link „Zur &lt;App&gt;“


**Cortana** zeigt in den meisten Bildschirmen einen Deep-Link „Zur &lt;App&gt;“ unterhalb der Inhaltskarte an.

![Cortana-Abschlussbildschirm für Hintergrund-App](images/cortana-completion-screen.png)

Sie können ein Start-Argument für diesen Link bereitstellen, mit dem Ihre App in einem ähnlichen Kontext wie der App-Dienst geöffnet wird. Wenn Sie kein Start-Argument angeben, wird der Hauptbildschirm der App gestartet.

In diesem Beispiel aus AdventureWorksVoiceCommandService.cs des Beispiels **AdventureWorks** übergeben wir das angegebene Ziel an die SendCompletionMessageForDestination-Methode, die alle übereinstimmenden Reisen abruft und einen Deep-Link zu der App bereitstellt.

Zunächst erstellen wir eine [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```), die von **Cortana** gesprochen und auf der **Cortana**-Canvas angezeigt wird. Anschließend wird ein [**VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx)-Listenobjekt zum Anzeigen der Sammlung von Ergebniskarten auf der Canvas erstellt. 

Diese beiden Objekte werden dann an die [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx)-Methode des [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182)-Objekts (```response```) übergeben. Anschließend legen wir als Wert für die [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183)-Eigenschaft den Wert des Ziels in dem Sprachbefehl fest.

Abschließend rufen wir die [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580)-Methode von [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) auf.

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```


## <span id="Content_tile_deep_link"></span><span id="content_tile_deep_link"></span><span id="CONTENT_TILE_DEEP_LINK"></span>Deep-Link-Inhaltskachel


Sie können Deep-Links zu Inhaltskarten auf verschiedenen **Cortana**-Bildschirmen hinzufügen.

![Cortana-Übergabebildschirm für Hintergrund-App ](images/cortana-backgroundapp-progress-result.png)

Wie bei den Links „Zur &lt;App&gt;“ können Sie ein Start-Argument angeben, um die App mit einem ähnlichen Kontext wie der App-Dienst zu öffnen. Wenn Sie kein Start-Argument angeben, wird die Inhaltskachel nicht mit Ihrer App verknüpft.

In diesem Beispiel aus AdventureWorksVoiceCommandService.cs des Beispiels **AdventureWorks** übergeben wir das angegebene Ziel an die SendCompletionMessageForDestination-Methode, die alle übereinstimmenden Reisen abruft und Inhaltskarten mit Deep-Links zu der App bereitstellt.

Zunächst erstellen wir eine [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```), die von **Cortana** gesprochen und auf der **Cortana**-Canvas angezeigt wird. Anschließend wird ein [**VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx)-Listenobbjekt zum Anzeigen der Sammlung von Ergebniskarten auf der Canvas erstellt. 

Diese beiden Objekte werden dann an die [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx)-Methode des [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182)-Objekts (```response```) übergeben. Anschließend legen wir als Wert für die [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183)-Eigenschaft den Wert des Ziels in dem Sprachbefehl fest.

Abschließend rufen wir die [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580)-Methode von [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) auf.
Hier fügen wir zwei Inhaltskacheln mit unterschiedlichen [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183)-Parameterwerten zu einer [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168)-Liste hinzu, die im [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580)-Aufruf des [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204)-Objekts verwendet wird.

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```
## <span id="Programmatic_deep_link"></span><span id="programmatic_deep_link"></span><span id="PROGRAMMATIC_DEEP_LINK"></span>Programmgesteuerter Deep-Link


Sie können Ihre App auch mit einem Start-Argument programmgesteuert starten, um die App mit einem ähnlichen Kontext wie der App-Dienst zu öffnen. Wenn Sie kein Start-Argument angeben, wird der Hauptbildschirm der App gestartet.

Hier fügen wir einen [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183)-Parameter mit dem Wert „Las Vegas“ zu einem [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182)-Objekt hinzu, das im [**RequestAppLaunchAsync**](https://msdn.microsoft.com/library/windows/apps/dn706581)-Aufruf des [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204)-Objekts verwendet wird.

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <span id="App_manifest"></span><span id="app_manifest"></span><span id="APP_MANIFEST"></span>App-Manifest


Um Deep-Links zu Ihrer App zu ermöglichen, müssen Sie die `windows.personalAssistantLaunch`-Erweiterung in der Datei „Package.appxmanifest“ Ihres App-Projekts deklarieren.

Hier deklarieren wir die `windows.personalAssistantLaunch`-Erweiterung für die **Adventure Works**-App.

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <span id="Protocol_contract"></span><span id="protocol_contract"></span><span id="PROTOCOL_CONTRACT"></span>Protokollvertrag


Ihre App wird über die URI (Uniform Resource Identifier)-Aktivierung mithilfe eines [**Protocol**](https://msdn.microsoft.com/library/windows/apps/br224693)-Vertrags im Vordergrund gestartet. Die App muss das [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Ereignis Ihrer App überschreiben und nach einem **ActivationKind**-Element mit dem Wert **Protocol** suchen. Weitere Informationen finden Sie unter [Behandeln der URI-Aktivierung](https://msdn.microsoft.com/library/windows/apps/mt228339).

Hier wird der von [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742) bereitgestellte URI decodiert, um auf das Start-Argument zuzugreifen. In diesem Beispiel wird für den [**Uri**](https://msdn.microsoft.com/library/windows/apps/br224746) „windows.personalassistantlaunch:? LaunchContext = Las Vegas“ festgelegt.

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <span id="related_topics"></span>Verwandte Artikel


**Entwickler**
* [Cortana-Interaktionen](cortana-interactions.md)
* [**VCD-Elemente und -Attribute v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designer**
* [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Beispiele**
* [Cortana-Sprachbefehlbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Apr16_HO3-->


