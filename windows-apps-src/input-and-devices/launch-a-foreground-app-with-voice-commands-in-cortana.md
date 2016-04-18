---
Description: Neben der Verwendung von Sprachbefehlen innerhalb von Cortana für den Zugriff auf Systemfeatures können Sie mithilfe von Sprachbefehlen über Cortana auch eine Vordergrund-App starten und eine Aktion oder einen Befehl angeben, der innerhalb der App ausgeführt wird.
title: Starten einer Vordergrund-App mit Sprachbefehlen in Cortana
ms.assetid: 8D3D1F66-7D17-4DD1-B426-DCCBD534EF00
label: Cortana-Launch a foreground app
template: detail.hbs
---

# Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD-Elemente und -Attribute v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Zusätzlich zur Verwendung von Sprachbefehlen in **Cortana** für den Zugriff auf Systemfeatures können Sie **Cortana** auch mit Features und Funktionalität von Ihrer App erweitern. Durch die Verwendung von Sprachbefehlen kann Ihre App im Vordergrund aktiviert und eine Aktion oder ein Befehl in der App ausgeführt werden. 

Wenn eine App im Vordergrund einen Sprachbefehl verarbeitet, steht sie im Fokus, und Cortana wird geschlossen. Falls gewünscht, können Sie Ihre App aktivieren und einen Befehl als Hintergrundaufgabe ausführen. In diesem Fall steht Cortana im Fokus, und Ihre App gibt das gesamte Feedback und alle Ergebnisse über die **Cortana**-Canvas und die **Cortana**-Stimme zurück.

Sprachbefehle, die zusätzlichen Kontext oder Benutzereingaben erfordern (z. B. das Senden einer Nachricht an einen bestimmten Kontakt), lassen sich am besten in einer Vordergrund-App verarbeiten, während einfache Befehle in **Cortana** durch eine Hintergrund-App verarbeitet werden können.

Wenn Sie eine App im Hintergrund mit Sprachbefehlen aktivieren möchten, informieren Sie sich unter [Aktivieren einer Hintergrund-App mit Sprachbefehlen über Cortana](launch-a-background-app-with-voice-commands-in-cortana.md).

> **Hinweis**  
> Ein Sprachbefehl ist eine einzelne, in einer Datei mit Sprachbefehldefinitionen (VCD) definierte Äußerung mit einem bestimmten Zweck, die über **Cortana** an eine installierte App gerichtet wird.

> Eine VCD-Datei definiert einen oder mehrere Sprachbefehle, die jeweils einem bestimmten Zweck dienen.

> Eine Sprachbefehlsdefinition kann unterschiedlich komplex sein. Sie kann beliebige Äußerungen unterstützen – einzelne eingeschränkte Äußerungen oder auch eine Collection flexiblerer Äußerungen in natürlicher Sprache, die alle dem gleichen spezifischen Zweck dienen.

Um Vordergrund-App-Features zu zeigen, verwenden wir eine Reiseplanungs- und Verwaltungs-App mit dem Namen **Adventure Works** aus dem [Cortana-Sprachbefehlsbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619899).

Zum Erstellen einer neuen Reise in **Adventure Works** ohne **Cortana** startet ein Benutzer die App und navigiert zur Seite **Neue Reise**. Zum Anzeigen einer vorhandenen Reise startet ein Benutzer die App, navigiert zur Seite **Bevorstehende Reisen** und wählt die Reise aus.

Mit den Sprachbefehlen über **Cortana** kann der Benutzer stattdessen einfach sagen „Adventure Works – Reise hinzufügen“ oder „Reise in Adventure Works hinzufügen“, um die App zu starten und zur Seite **Neue Reise** zu navigieren. Sagt der Benutzer dagegen „Adventure Works, zeige meine Reise nach London an“, wird die App mit der hier dargestellten Seite mit den Details zur **Reise** geöffnet.

![Cortana startet die Vordergrund-App](images/cortana-foreground-with-adventureworks.png)

Die grundlegenden Schritte zum Hinzufügen der Sprachbefehlserkennung und zum Integrieren von Cortana in Ihre App mithilfe der Sprach- oder Tastatureingabe lauten wie folgt:

1.  Erstellen einer VCD-Datei Hierbei handelt es sich um ein XML-Dokument, das alle Sprachbefehle definiert, mit denen der Benutzer Aktionen initiieren oder Befehle aufrufen kann, wenn er Ihre App aktiviert. Informationen finden Sie unter [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).
2.  Registrieren Sie die Befehlssätze in der VCD-Datei, wenn die App gestartet wird.
3.  Verarbeiten Sie die Aktivierung per Sprachbefehl, die Navigation innerhalb der App und die Befehlsausführung.

**Voraussetzungen:**

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

-   [Erstellen Ihrer ersten App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Richtlinien für die Benutzerfreundlichkeit:**

Unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233) finden Sie Informationen zur Integration Ihrer App mit **Cortana**. Unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121) finden Sie nützliche Tipps für den Entwurf einer nützlichen und interaktiven sprachaktivierten App.

## <span id="Create_a_new_solution_with_project_in_Visual_Studio"></span><span id="create_a_new_solution_with__project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH__PROJECT_IN_VISUAL_STUDIO"></span>Erstellen einer neuen Lösung mit einem Projekt in Visual Studio


1.  Starten Sie Microsoft Visual Studio 2015.

    Der Visual Studio 2015-Startbildschirm wird angezeigt.

2.  Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**.

    Das Dialogfeld **Neues Projekt** wird geöffnet. Im linken Bereich des Dialogfelds können Sie die Art der anzuzeigenden Vorlagen auswählen.

3.  Erweitern Sie im linken Bereich die Option **Installiert > Vorlagen > Visual C\# > Windows** und wählen Sie anschließend die Vorlagengruppe **Universell**. Im mittleren Bereich des Dialogfelds sehen Sie eine Liste mit Projektvorlagen für universelle Windows-Plattform-Apps (UWP).
4.  Wählen Sie im mittleren Bereich die Projektvorlage **Leere App (universelle Windows-App)** aus.

    Die Vorlage **Leere App** stellt eine minimale UWP-App bereit, die kompiliert und ausgeführt wird, aber keine Steuerelemente oder Daten für die Benutzeroberfläche enthält. Die App wird später in diesem Lernprogramm noch mit Steuerelementen versehen.

5.  Geben Sie in das Textfeld **Name** Ihren Projektnamen ein. In diesem Beispiel verwenden wir „AdventureWorks“.
6.  Klicken Sie auf **OK**, um das Projekt zu erstellen.

    Microsoft Visual Studio erstellt Ihr Projekt und zeigt es im **Projektmappen-Explorer** an.

## <span id="Add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="ADD_IMAGE_ASSETS_TO_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"></span>Hinzufügen von Bildressourcen zum Projekt und Angeben im App-Manifest
      
UWP-Apps können automatisch die am besten geeigneten Bilder basierend auf bestimmten Einstellungen und Gerätefunktionen (hoher Kontrast, effektive Pixel, Gebietsschema und so weiter) auswählen. Sie müssen lediglich die Bilder bereitstellen und sicherstellen, dass Sie die entsprechende Benennungskonvention und Ordnerstruktur innerhalb des App-Projekts für die verschiedenen Ressourcenversionen verwenden. Wenn Sie nicht die empfohlenen Ressourcenversionen bereitstellen können, werden unter Umständen je nach Voreinstellungen, Fähigkeiten, Gerätetyp und Standort des Benutzers die Eingabehilfen, Lokalisierung und Bildqualität beeinträchtigt.

Weitere Informationen zu Bildressourcen für hohen Kontrast und Skalierungsfaktoren finden Sie unter [Richtlinien für die Ressourcen für Kacheln und Symbole](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets).

Sie benennen Ressourcen mithilfe von Qualifizierern. Ressourcenqualifizierer sind Ordner- und Dateinamenmodifikatoren, die den Kontext angeben, in dem eine bestimmte Version einer Ressource verwendet werden soll.

Standardmäßig wird die folgende Benennungskonvention verwendet: `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`. Beispiel: Auf `images/en-US/logo.scale-100_contrast-white.png` wird im Code einfach durch Angabe des Stammordners und des Dateinamens verwiesen: `images/logo.png`. Weitere Informationen finden Sie unter [Benennen von Ressourcen mithilfe von Qualifizierern](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh965324.aspx).

Wir empfehlen, bei Zeichenfolgen-Ressourcendateien die Standardsprache (Beispiel: `en-US\resources.resw`) und bei Bildern den Standardskalierungsfaktor (Beispiel: `logo.scale-100.png`) selbst dann zu markieren, wenn diese Dateien derzeit nicht lokalisiert bzw. keine Ressourcen in unterschiedlichen Auflösungen bereitgestellt werden sollen. Es wird jedoch empfohlen, dass Sie mindestens Ressourcen für die Skalierungsfaktoren 100, 200 und 400 bereitstellen.

> *Wichtig

> Das im Titelbereich der **Cortana**-Canvas verwendete App-Symbol ist das in der Datei „Package.appxmanifest“ angegebene Symbol „Square44x44Logo“. 
    
## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>Erstellen einer VCD-Datei

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihren primären Projektnamen, und wählen Sie **Hinzufügen > Neues Element**. Fügen Sie eine **XML-Datei** hinzu.
2. Geben Sie einen Namen für die [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593)-Datei ein (z. B. „AdventureWorksCommands.xml“), und klicken Sie auf „Hinzufügen“. 
3. Wählen Sie im **Projektmappen-Explorer** die [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593)-Datei aus.
4.  Legen Sie im Fenster **Eigenschaften** die Option **Buildvorgang** auf **Inhalt** fest, und legen Sie dann **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer** fest.

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>Bearbeiten der VCD-Datei


. Fügen Sie ein **VoiceCommands**-Element mit einem **xmlns**-Attribut hinzu, das auf „http://schemas.microsoft.com/voicecommands/1.2“ verweist.

2. Erstellen Sie für jede von Ihrer App unterstützte Sprache ein [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)-Element, das die von Ihrer App unterstützten Sprachbefehle enthält.

  Sie können mehrere [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)-Elemente deklarieren, jedes mit einem anderen [**xml:lang**](https://msdn.microsoft.com/library/windows/apps/dn722331)-Attribut, sodass Ihre App in verschiedenen Märkten verwendet werden kann. So kann eine App für die Vereinigten Staaten beispielsweise einen [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) für Englisch und einen [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) für Spanisch umfassen.

  >  **Achtung**  
  Für die Aktivierung einer App und die Initiierung einer Aktion per Sprachbefehl muss die App eine VCD-Datei registrieren. Die VCD-Datei muss einen [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) mit einer Sprache enthalten, die der für das Gerät ausgewählten Sprache für die Spracherkennung entspricht. Die Spracherkennungssprache befindet sich unter **Einstellungen > System > Spracherkennung > Spracherkennungssprache**.

3. Fügen Sie ein **Command**-Element für jeden Befehl hinzu, den Sie unterstützen möchten.

  Jeder in einer [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593)-Datei deklarierte **Command** muss diese Informationen beinhalten:

  - Attribut **Name**, das Ihre Anwendung verwendet, um den Sprachbefehl zur Laufzeit zu identifizieren. 
  - Element **Example**, das eine Beschreibung enthält, wie ein Benutzer den Befehl aufrufen kann. **Cortana** zeigt folgendes Beispiel an, wenn der Benutzer „Was kann ich sagen?“ oder „Hilfe“ sagt oder auf **Mehr anzeigen** tippt.    
  -   Element **ListenFor**, das die Wörter oder Ausdrücke enthält, die Ihre App als Befehl erkennt. Jedes **ListenFor**-Element kann Referenzen zu einem oder mehreren **PhraseList**-Elementen enthalten, die spezielle für den Befehl relevante Wörter enthalten.
  > **Hinweis**  
  **ListenFor**-Elemente können nicht programmgesteuert geändert werden. **PhraseList-Elemente**, die **ListenFor**-Elementen zugeordnet sind, können dagegen programmgesteuert geändert werden. Anwendungen sollten den Inhalt der **PhraseList** zur Laufzeit basierend auf dem Datensatz ändern, der bei der Verwendung der App durch den Benutzer generiert wird. Weitere Informationen finden Sie unter [Dynamisches Ändern von VCD-PhraseLists (Sprachbefehlsdefinition)](dynamically-modify-voice-command-definition--vcd--phrase-lists.md).

  -   Element **Feedback**, das den Text für die Anzeige und Sprachausgabe von **Cortana** beim Starten der Anwendung enthält.

Ein **Navigate**-Element gibt an, dass der Sprachbefehl die App im Vordergrund aktiviert. In diesem Beispiel ist der Befehl ```showTripToDestination``` eine Vordergrundaufgabe.

Ein **VoiceCommandService**-Element gibt an, dass der Sprachbefehl die App im Hintergrund aktiviert. Der Wert des Attributs **Target** dieses Elements sollte dem Wert des Attributs **Name** des Elements [**Uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) in der Datei „package.appxmanifest“ entsprechen. In diesem Beispiel sind die Befehle ```whenIsTripToDestination``` und ```cancelTripToDestination``` Hintergrundaufgaben, die den Namen des App-Diensts als „AdventureWorksVoiceCommandService“ angeben.

Weitere Informationen finden Sie in der [**VCD-Elemente und -Attribute v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)-Referenz.

Im Folgenden finden Sie einen Teil der [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593)-Datei, der die en-us-Sprachbefehle für die App **Adventure Works** definiert.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <span id="Install_the_VCD_commands"></span><span id="install_the_vcd_commands"></span><span id="INSTALL_THE_VCD_COMMANDS"></span>Installieren der VCD-Befehle


Ihre App muss einmal ausgeführt werden, um die VCD-Datei zu installieren. 

>  **Hinweis**  
Sprachbefehlsdaten werden nicht übergreifend über App-Installationen beibehalten. Um sicherzustellen, dass die Sprachbefehlsdaten für Ihre App intakt bleiben, ziehen Sie die Initialisierung Ihrer VCD-Datei in Betracht, wenn Ihre App gestartet oder aktiviert wird, oder verwenden Sie eine Einstellung, die angibt, ob die VCD aktuell installiert ist.

In der Datei „app.xaml.cs“:

1. Fügen Sie Folgendes mithilfe einer Direktive hinzu:  
```csharp
using Windows.Storage;
```
2. Markieren Sie die Methode „OnLaunched“ mit dem asynchronen Modifizierer.  
```csharp
protected async override void OnLaunched(LaunchActivatedEventArgs e)
```
3. Rufen Sie [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205) im Handler [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) auf, um die Sprachbefehle zu registrieren, die das System erkennen soll.

  Im Adventure Works-Beispiel definieren wir zunächst ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt. 

  Anschließend rufen wir [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) auf, um es mit unserer Datei „AdventureWorksCommands.xml“ zu initialisieren.

  Dieses [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt wird dann an [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205) übergeben.    
```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
    await Package.Current.InstalledLocation.GetFileAsync(
      @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
    InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Handle activation and execute voice commands

Specify how your app responds to subsequent voice command activations (after it has been launched at least once and the voice command sets have been installed).

1.  Confirm that your app was activated by a voice command.

    Override the [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) event and check whether [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) is [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693).

2.  Determine the name of the command and what was spoken.

    Get a reference to a [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) object from the [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) and query the [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) property for a [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) object.

    To determine what the user said, check the value of [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) or the semantic properties of the recognized phrase in the [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) dictionary.

3.  Take the appropriate action in your app, such as navigating to the desired page.

For this example, we refer back to the VCD in Step 3: Edit the VCD file.

Once we get the speech-recognition result for the voice command, we get the command name from the first value in the [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438) array. As the VCD file defined more than one possible voice command, we need to compare the value against the command names in the VCD and take the appropriate action.

The most common action an application can take is to navigate to a page with content relevant to the context of the voice command. For this example, we navigate to a **TripPage** page and pass in the value of the voice command, how the command was input, and the recognized "destination" phrase (if applicable). Alternatively, the app could send a navigation parameter to the [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) when navigating to the page.

You can find out whether the voice command that launched your app was actually spoken, or whether it was typed in as text, from the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) dictionary using the **commandMode** key. The value of that key will be either "voice" or "text". If the value of the key is "voice", consider using speech synthesis ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) in your app to provide the user with spoken feedback.

Use the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) to find out the content spoken in the **PhraseList** or **PhraseTopic** constraints of a **ListenFor** element. The dictionary key is the value of the **Label** attribute of the **PhraseList** or **PhraseTopic** element. Here, we show how to access the value of **{destination}** phrase.

``` csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <span id="related_topics"></span>Verwandte Artikel


**Entwickler**
* [Cortana-Interaktionen](cortana-interactions.md)
* [Definieren von benutzerdefinierten Erkennungseinschränkungen](define-custom-recognition-constraints.md)
* [**VCD-Elemente und -Attribute v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designer**
* [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Beispiele**
* [Cortana-Sprachbefehlbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Apr16_HO3-->


