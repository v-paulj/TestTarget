---
author: Karl-Bridge-Microsoft
Description: Hier erfahren Sie, wie ein Benutzer während der Ausführung eines Sprachbefehls über die Spracheingabe und die Canvas von Cortana mit einer Hintergrund-App interagieren kann.
title: Interagieren mit einer Hintergrund-App
ms.assetid: 6C60F03C-A242-435D-96BB-736892CC1CA6
label: Interact with a background app
template: detail.hbs
---

# Interagieren mit einer Hintergrund-App in Cortana

Ermöglichen Sie Benutzerinteraktion mit einer Hintergrund-App über Sprach- und Texteingabe im **Cortana**-Canvas, während ein Sprachbefehl ausgeführt wird.



**Wichtige APIs**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Voice Command Definition (VCD) Elements and Attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Cortana unterstützt einen vollständigen Sprachnavigationsworkflow mit Ihrer App. Dieser Workflow wird von der App definiert und kann die folgenden Funktionen unterstützen: 

-   Erfolgreicher Abschluss
-   Übergabe
-   Status
-   Bestätigung
-   Mehrdeutigkeitsvermeidung
-   Fehler

**Voraussetzungen:**

Dieses Thema baut auf [Starten einer Hintergrund-App mit Sprachbefehlen in Cortana](launch-a-background-app-with-voice-commands-in-cortana.md) auf. Wir zeigen hier weitere Features anhand einer Reiseplanungs- und Verwaltungs-App mit dem Namen **Adventure Works**.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

-   [Erstellen Ihrer ersten App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Richtlinien für die Benutzerfreundlichkeit:  **

Unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233) finden Sie Informationen zur Integration Ihrer App mit **Cortana**. Unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121) finden Sie hilfreiche Tipps für den Entwurf einer nützlichen und interaktiven sprachaktivierten App.

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>Feedbackzeichenfolgen

Erstellen Sie Feedback-Zeichenfolgen, die sowohl auf dem Bildschirm angezeigt als auch von **Cortana** gesprochen werden.

Die [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233)enthalten Empfehlungen für das Erstellen von Zeichenfolgen für **Cortana**.

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>Feedbackzeichenfolgen

Inhaltskarten können zusätzlichen Kontext für den Benutzer bereitstellen und dazu beitragen, die Feedback-Zeichenfolgen möglichst kurz zu halten.

**Cortana** unterstützt die folgenden Inhaltskartenvorlagen. (Für den Abschlussbildschirm kann nur eine einzige Vorlage verwendet werden.)

    -   Title only
    -   Title with up to three lines of text
    -   Title with image
    -   Title with image and up to three lines of text

Das Image kann Folgendes sein:

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

Sie können es dem Benutzer auch ermöglichen, Ihre App durch Klicken auf eine Karte oder auf den Textlink zu Ihrer App im Vordergrund zu starten.

## <span id="Completion_screen"></span><span id="completion_screen"></span><span id="COMPLETION_SCREEN"></span>Abschlussbildschirm

Ein Abschlussbildschirm informiert den Benutzer über die abgeschlossene Sprachbefehlsaufgabe.

Aufgaben, auf die Ihre App innerhalb von weniger als 500 Millisekunden reagieren muss und keine weiteren Informationen vom Benutzer erfordern, werden ohne weitere Interaktion mit  **Cortana** abgeschlossen. Cortana zeigt den Abschlussbildschirm an.

Im Folgenden verwenden wir die **Adventure Works**-App, um den Abschlussbildschirm für eine Sprachbefehlsanforderung zum Anzeigen anstehender Reisen nach London darzustellen. 

![Cortana-Abschlussbildschirm für Hintergrund-App](images/cortana-completion-screen-upcomingtrip-small.png)

Der Sprachbefehl ist in der Datei „AdventureWorksCommands.xml“ definiert:
```
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs enthält die Methode für die Abschlussmeldung:

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

## <span id="Hand-off_screen"></span><span id="hand-off_screen"></span><span id="HAND-OFF_SCREEN"></span>Übergabebildschirm

Nachdem ein Sprachbefehl erkannt wurde, muss **Cortana** ReportSuccessAsync aufrufen und vorhandenes Feedback innerhalb von etwa 500 ms anzeigen. Kann der App-Dienst die durch den Sprachbefehl angeforderte Aktion nicht innerhalb von 500 Millisekunden abschließen, zeigt **Cortana** bis zu fünf Sekunden lang, oder bis Ihre App ReportSuccessAsync aufgerufen hat, einen Übergabebildschirm an.

Falls der App-Dienst ReportSuccessAsync oder eine andere VoiceCommandServiceConnection-Methode nicht aufruft, erhält der Benutzer eine Fehlermeldung, und der Aufruf des App-Diensts wird abgebrochen.

Hier sehen Sie einen Übergabebildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer **Cortana** nach anstehenden Reisen befragt. Der Übergabebildschirm enthält eine Meldung mit dem Namen des App-Diensts, einem Symbol und der **Feedback**-Zeichenfolge aus der VCD-Datei.

![Cortana-Übergabebildschirm für Hintergrund-App](images/cortana-backgroundapp-progress-result.png)


## <span id="Progress_screen"></span><span id="progress_screen"></span><span id="PROGRESS_SCREEN"></span>Statusbildschirm


Wenn der App-Dienst mehr als 500 ms benötigt, um ReportSuccessAsync aufzurufen, zeigt **Cortana** dem Benutzer einen Statusbildschirm an. Das App-Symbol wird angezeigt, und Sie müssen sowohl eine GUI- als auch eine TTS-Statuszeichenfolge bereitstellen, die deutlich macht, dass die Aufgabe aktiv bearbeitet wird.

**Cortana** zeigt einen Statusbildschirm maximal für 5 Sekunden an. Nach fünf Sekunden zeigt **Cortana** eine Fehlermeldung an, und der App-Dienst wird geschlossen. Sollte die Durchführung der Aktion länger als fünf Sekunden dauern, kann der App-Dienst **Cortana** mit weiteren Statusbildschirmen aktualisieren.

Hier sehen Sie einen Statusbildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer eine Reise nach Las Vegas storniert. Der Statusbildschirm enthält eine aktionsspezifische Meldung, ein Symbol und eine Inhaltskachel mit Informationen zur stornierten Reise.

![Cortana-Statusbildschirm für Hintergrund-App ](images/cortana-progress-screen.png)

AdventureWorksVoiceCommandService.cs enthält die folgende Statusmeldungsmethode, die [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) zum Anzeigen des Statusbildschirms in **Cortana** aufruft.


```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <span id="Confirmation_screen"></span><span id="confirmation_screen"></span><span id="CONFIRMATION_SCREEN"></span>Bestätigungsbildschirm


Wenn eine per Sprachbefehl angegebene Aktion nicht rückgängig gemacht werden kann, erhebliche Auswirkungen hat oder die Treffgenauigkeit gering ist, kann der App-Dienst eine Bestätigung anfordern.

Hier sehen Sie einen Bestätigungsbildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer den App-Dienst über **Cortana** angewiesen, eine Reise nach Las Vegas zu stornieren. Der App-Dienst hat für **Cortana** einen Bestätigungsbildschirm bereitgestellt, in dem der Benutzer die Reisestornierung bestätigen oder ablehnen kann.

Wenn der Benutzer nicht mit „Ja“ oder „Nein“ antwortet, kann **Cortana** die Antwort auf die Frage nicht ermitteln. In diesem Fall stellt **Cortana** dem Benutzer eine ähnliche, vom App-Dienst bereitgestellte Frage.

Falls der Benutzer auch die zweite Frage nicht mit „Ja“ oder „Nein“ beantwortet, fragt **Cortana** ein drittes Mal nach. Dabei wird nochmals die gleiche Frage verwendet und eine Entschuldigung vorangestellt. Antwortet der Benutzer wieder nicht mit „Ja“ oder „Nein“, lauscht **Cortana** nicht weiter auf Spracheingaben und fordert den Benutzer stattdessen auf, auf eine der Schaltflächen zu tippen.

Der Bestätigungsbildschirm enthält eine aktionsspezifische Meldung, ein Symbol und eine Inhaltskachel mit Informationen zur stornierten Reise.

![Cortana-Bestätigungsbildschirm für Hintergrund-App](images/cortana-confirmation-screen.png)

AdventureWorksVoiceCommandService.cs enthält die folgende Methode für die Stornierung der Reise, die [ **RequestConfirmationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706582) zum Anzeigen eines Bestätigungsbildschirms in **Cortana** aufruft.

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <span id="Disambiguation_screen"></span><span id="disambiguation_screen"></span><span id="DISAMBIGUATION_SCREEN"></span>Bildschirm zur Vermeidung von Mehrdeutigkeiten


Wenn für eine per Sprachbefehl angegebene Aktion mehrere Ergebnisse möglich sind, kann der App-Dienst vom Benutzer weitere Informationen anfordern.

Hier sehen Sie einen Mehrdeutigkeitsvermeidungsbildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer den App-Dienst über **Cortana** angewiesen, eine Reise nach Las Vegas zu stornieren. Für den Benutzer sind allerdings zwei Las Vegas-Reisen mit unterschiedlichen Reisedaten vorhanden, und der App-Dienst kann die Aktion nur abschließen, wenn der Benutzer die gewünschte Reise auswählt.

Der App-Dienst stellt für **Cortana** einen Mehrdeutigkeitsvermeidungsbildschirm bereit, über den der Benutzer vor der Stornierung aufgefordert wird, die gewünschte Reise in einer Liste mit passenden Reisen auszuwählen.

In diesem Fall stellt **Cortana** dem Benutzer eine ähnliche, vom App-Dienst bereitgestellte Frage.

Wenn der Benutzer auch bei der zweiten Aufforderung nichts sagt, wovon sich die gewünschte Auswahl ableiten lässt, fragt **Cortana** ein drittes Mal nach. Dabei wird nochmals die gleiche Frage verwendet und eine Entschuldigung vorangestellt. Sagt der Benutzer wieder nichts, wovon sich die gewünschte Auswahl ableiten lässt, lauscht **Cortana** nicht weiter auf Spracheingaben und fordert den Benutzer stattdessen auf, auf eine der Schaltflächen zu tippen.

Der Mehrdeutigkeitsvermeidungsbildschirm enthält eine aktionsspezifische Meldung, ein Symbol und eine Inhaltskachel mit Informationen zur stornierten Reise.

![Cortana-Mehrdeutigkeitsvermeidungsbildschirm für Hintergrund-App ](images/cortana-disambiguation-screen.png)

AdventureWorksVoiceCommandService.cs enthält die folgende Methode für die Stornierung der Reise, die [ **RequestDisambiguationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706583) zum Anzeigen eines Mehrdeutigkeitsvermeidungsbildschirms in **Cortana** aufruft.

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <span id="Error_screen"></span><span id="error_screen"></span><span id="ERROR_SCREEN"></span>Fehlerbildschirm


Wenn eine per Sprachbefehl angegebene Aktion nicht abgeschlossen werden kann, kann ein App-Dienst einen Fehlerbildschirm bereitstellen.

Hier sehen Sie einen Fehlerbildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer den App-Dienst über **Cortana** angewiesen, eine Reise nach Las Vegas zu stornieren. Für den Benutzer sind jedoch keine Reisen nach Las Vegas geplant.

Der App-Dienst stellt für **Cortana** einen Fehlerbildschirm mit einer aktionsspezifischen Meldung, einem Symbol und der entsprechenden Fehlermeldung bereit.

Rufen Sie [**ReportFailureAsync**](https://msdn.microsoft.com/library/windows/apps/dn706578) auf, um den Fehlerbildschirm in **Cortana** anzuzeigen.

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
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
 

 






<!--HONumber=May16_HO2-->


