---
author: Karl-Bridge-Microsoft
Description: Zusätzlich zur Verwendung von Sprachbefehlen in Cortana zum Zugreifen auf Systemfeatures können Sie Cortana auch um Features und Funktionen einer Hintergrund-App erweitern, indem Sie Sprachbefehle verwenden, mit denen die Ausführung einer Aktion oder eines Befehls in der App angegeben wird.
title: Starten einer Hintergrund-App mit Sprachbefehlen in Cortana
ms.assetid: DF5B530C-57DD-4CA5-B3BE-1A0B3695C9C6
label: Launch a background app
template: detail.hbs
---

# Aktivieren einer Hintergrund-App mit Sprachbefehlen über Cortana

Zusätzlich zur Verwendung von Sprachbefehlen in **Cortana** zum Zugreifen auf Systemfeatures können Sie **Cortana** auch um Features und Funktionen Ihrer App (als Hintergrundaufgabe) erweitern, indem Sie Sprachbefehle verwenden, mit denen die Ausführung einer Aktion oder eines Befehls in der App angegeben wird. Wenn eine App einen Sprachbefehl im Hintergrund verarbeitet, steht sie nicht im Fokus. Stattdessen werden das gesamte Feedback und alle Ergebnisse über den **Cortana**-Canvas und die **Cortana**-Stimme zurückgegeben.

**Wichtige APIs**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD-Elemente und -Attribute v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)



Apps können im Vordergrund (App ist im Fokus) oder im Hintergrund (**Cortana** bleibt im Fokus) aktiviert werden, je nach Komplexität der Interaktion. Sprachbefehle, die zusätzlichen Kontext oder Benutzereingaben erfordern (z. B. das Senden einer Nachricht an einen bestimmten Kontakt), lassen sich beispielsweise am besten in einer Vordergrund-App verarbeiten, während einfache Befehle in **Cortana** durch eine Hintergrund-App verarbeitet werden können.

Wenn Sie eine App im Vordergrund mit Sprachbefehlen aktivieren möchten, informieren Sie sich unter [Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md).

> **Hinweis**  
> Ein Sprachbefehl ist eine einzelne, in einer Datei mit Sprachbefehlsdefinitionen (VCD) definierte Äußerung mit einem bestimmten Zweck, die über **Cortana** an eine installierte App gerichtet wird.

> Eine VCD-Datei definiert einen oder mehrere Sprachbefehle, die jeweils einem bestimmten Zweck dienen.

> Eine Sprachbefehlsdefinition kann unterschiedlich komplex sein. Sie kann beliebige Äußerungen unterstützen – einzelne eingeschränkte Äußerungen oder auch eine Collection flexiblerer Äußerungen in natürlicher Sprache, die alle dem gleichen spezifischen Zweck dienen.

Um Hintergrund-App-Features zu zeigen, verwenden wir eine Reiseplanungs- und Verwaltungs-App mit dem Namen **Adventure Works** aus dem [Cortana-Sprachbefehlsbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619899).

Hier sehen Sie eine Übersicht über die **Adventure Works**-App, die in die **Cortana**-Canvas integriert ist.

![Übersicht über die Cortana-Canvas ](images/cortana-overview.png)

Zum Anzeigen einer **Adventure Works**-Reise ohne **Cortana** startet ein Benutzer die App und navigiert auf die Seite **Bevorstehende Reisen**.

Mit den Sprachbefehlen über **Cortana** zum Starten Ihrer App im Hintergrund kann der Benutzer auch einfach Folgendes sagen: „Adventure Works, wann ist meine Reise nach Las Vegas?“. Ihre App verarbeitet den Befehl, und **Cortana** zeigt die Ergebnisse zusammen mit Ihrem App-Symbol und weiteren App-Informationen an, falls diese bereitgestellt wurden. Dies ist ein Beispiel für eine einfache Reiseabfrage und den Ergebnisbildschirm von **Cortana**. Darüber wird die Antwort „Ihre nächste Reise nach Las Vegas ist am 1. August“ sowohl angezeigt als auch per Sprachfunktion ausgegeben.

![Einfache Abfrage und Ergebnisbildschirm mit Verwendung der Adventure Works-App im Hintergrund](images/cortana-backgroundapp-result.png)

Die grundlegenden Schritte zum Hinzufügen der Sprachbefehlserkennung und zum Erweitern von **Cortana** um die Hintergrundfunktionen Ihrer App per Sprach- oder Tastatureingabe lauten wie folgt:

1.  Erstellen Sie einen App-Dienst (siehe [**Windows.ApplicationModel.AppService**](https://msdn.microsoft.com/library/windows/apps/dn921731)), der von **Cortana** im Hintergrund aufgerufen wird.
2.  Erstellen Sie eine VCD-Datei. Hierbei handelt es sich um ein XML-Dokument, das alle Sprachbefehle definiert, mit denen der Benutzer Aktionen initiieren oder Befehle aufrufen kann, wenn er Ihre App aktiviert. Informationen finden Sie unter [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).
3.  Registrieren Sie die Befehlssätze in der VCD-Datei, wenn die App gestartet wird.
4.  Behandeln Sie die Hintergrundaktivierung des App-Diensts und die Ausführung des Sprachbefehls.
5.  Zeigen Sie das entsprechende Feedback für den Sprachbefehl in **Cortana** an, und führen Sie die Sprachausgabe durch.

**Voraussetzungen:  **

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Apps für die universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

-   [Erstellen Ihrer ersten App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Richtlinien für die Benutzerfreundlichkeit:  **

Unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233) finden Sie Informationen zur Integration Ihrer App mit **Cortana**. Unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121) finden Sie hilfreiche Tipps für den Entwurf einer nützlichen und interaktiven sprachaktivierten App.

## <span id="Create_a_new_solution_with_a_primary_project_in_Visual_Studio"></span><span id="create_a_new_solution_with_a_primary_project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH_A_PRIMARY_PROJECT_IN_VISUAL_STUDIO"></span>Erstellen einer neuen Lösung mit einem primären Projekt in Visual Studio


1.  Starten Sie Microsoft Visual Studio 2015.

    Der Visual Studio 2015-Startbildschirm wird angezeigt.

2.  Klicken Sie im Menü **Datei** auf **Neu** > **Projekt**.

    Das Dialogfeld **Neues Projekt** wird geöffnet. Im linken Bereich des Dialogfelds können Sie die Art der anzuzeigenden Vorlagen auswählen.

3.  Erweitern Sie im linken Bereich die Option **Installiert > Vorlagen > Visual C\# > Windows** und wählen Sie anschließend die Vorlagengruppe **Universell**. Im mittleren Bereich des Dialogfelds sehen Sie eine Liste mit Projektvorlagen für universelle Windows-Plattform-Apps (UWP).
4.  Wählen Sie im mittleren Bereich die Projektvorlage **Leere App (universelle Windows-App)** aus.

    Die Vorlage **Leere App** stellt eine minimale UWP-App bereit, die kompiliert und ausgeführt wird, aber keine Steuerelemente oder Daten für die Benutzeroberfläche enthält. Die App wird später in diesem Lernprogramm noch mit Steuerelementen versehen.

5.  Geben Sie in das Textfeld **Name** Ihren Projektnamen ein. In diesem Beispiel verwenden wir „AdventureWorks“.
6.  Klicken Sie auf **OK**, um das Projekt zu erstellen.

    Microsoft Visual Studio erstellt Ihr Projekt und zeigt es im **Projektmappen-Explorer** an.


## <span id="Add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"></span><span id="add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"></span><span id="ADD_IMAGE_ASSETS_TO_PRIMARY_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"></span>Hinzufügen von Bildressourcen zum primären Projekt und deren Spezifizierung im App-Manifest
      
UWP-Apps können automatisch die am besten geeigneten Bilder basierend auf bestimmten Einstellungen und Gerätefunktionen (hoher Kontrast, effektive Pixel, Gebietsschema und so weiter) auswählen. Sie müssen lediglich die Bilder bereitstellen und sicherstellen, dass Sie die entsprechende Benennungskonvention und Ordnerstruktur innerhalb des App-Projekts für die verschiedenen Ressourcenversionen verwenden. Wenn Sie nicht die empfohlenen Ressourcenversionen bereitstellen können, werden unter Umständen je nach Voreinstellungen, Fähigkeiten, Gerätetyp und Standort des Benutzers die Eingabehilfen, Lokalisierung und Bildqualität beeinträchtigt.

Weitere Informationen zu Bildressourcen für hohen Kontrast und Skalierungsfaktoren finden Sie unter [Richtlinien für die Ressourcen für Kacheln und Symbole](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets).

Sie benennen Ressourcen mithilfe von Qualifizierern. Ressourcenqualifizierer sind Ordner- und Dateinamenmodifikatoren, die den Kontext angeben, in dem eine bestimmte Version einer Ressource verwendet werden soll.

Standardmäßig wird die folgende Benennungskonvention verwendet: `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`. Beispiel: Auf `images/en-US/logo.scale-100_contrast-white.png` wird im Code einfach durch Angabe des Stammordners und des Dateinamens verwiesen: `images/logo.png`. Weitere Informationen finden Sie unter [Benennen von Ressourcen mithilfe von Qualifizierern](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx).

Wir empfehlen, bei Zeichenfolgen-Ressourcendateien die Standardsprache (Beispiel: `en-US\resources.resw`) und bei Bildern den Standardskalierungsfaktor (Beispiel: `logo.scale-100.png`) selbst dann zu markieren, wenn diese Dateien derzeit nicht lokalisiert bzw. keine Ressourcen in unterschiedlichen Auflösungen bereitgestellt werden sollen. Es wird jedoch empfohlen, dass Sie mindestens Ressourcen für die Skalierungsfaktoren 100, 200 und 400 bereitstellen.

> *Wichtig

> Das im Titelbereich der **Cortana**-Canvas verwendete App-Symbol ist das in der Datei „Package.appxmanifest“ angegebene Symbol „Square44x44Logo“. 

> Sie können auch ein Symbol für die einzelnen Einträge im Inhaltsbereich der **Cortana**-Canvas angeben. Gültige Bildgrößen für die Ergebnissymbole sind:

> - 68 (B) x 68 (H) 
> - 68 (B) x 92 (H) 
> - 280 (B) x 140 (H) 

Die Inhaltskachel ist erst gültig, nachdem eine [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182)-Instanz an die [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204)-Verbindung übergeben wird. Wenn Sie ein **VoiceCommandResponse**-Objekt an **Cortana** übergeben, das eine Inhaltskachel mit einem Bild enthält, dessen Größe nicht diesen Seitenverhältnissen entspricht, kann eine Ausnahme auftreten. 

In diesem Beispiel aus der **Adventure Works**-App (VoiceCommandService\\AdventureWorksVoiceCommandService.cs) geben wir ein einfaches graues Quadrat („GreyTile.png“) auf [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) mithilfe der Kachelvorlage [**TitleWith68x68IconAndText**](https://msdn.microsoft.com/library/windows/apps/dn974169) an. Die Logovarianten befinden sich unter VoiceCommandService\\Images und werden mit der Methode [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) abgerufen.

```CSharp
var destinationTile = new VoiceCommandContentTile();

destinationTile.ContentTileType = 
  VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = 
  await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
```

## <span id="Create_an_app_service_project"></span><span id="create_an_app_service_project"></span><span id="CREATE_AN_APP_SERVICE_PROJECT"></span>Erstellen eines App-Dienstprojekts

<ol>
    <li>
    Klicken Sie mit der rechten Maustaste auf den Namen Ihrer Projektmappe, und wählen Sie **Neu > Projekt** aus.
    </li>
    <li>
    Wählen Sie unter **Installiert > Vorlagen > Visual C# > Windows > Universal** **Komponente für die Windows-Runtime** aus. Dies ist die Komponente, mit der der App-Dienst implementiert wird (**[Windows.ApplicationModel.AppService](https://msdn.microsoft.com/library/windows/apps/dn921731)**).
    </li>
    <li>
    Geben Sie einen Namen für das Projekt ein (z. B. „VoiceCommandService“), und klicken Sie auf **OK**.
    </li>
    <li>
    Wählen Sie im **Projektmappen-Explorer** das Projekt „VoiceCommandService“ aus, und benennen Sie die von Visual Studio generierte Datei „Class1.cs“ um. Für das Beispiel **Adventure Works** verwenden wir „AdventureWorksVoiceCommandService.cs“.
    </li>
    <li>
    Klicken Sie auf **Ja**, wenn Sie gefragt werden, ob Sie alle Vorkommen von „Class1.cs“ umbenennen möchten. 
    </li>
    <li>
    In der Datei „AdventureWorksVoiceCommandService.cs“:
        <ol type="i">
 <li>
 Fügen Sie Folgendes mithilfe einer Direktive hinzu.  
 ```using Windows.ApplicationModel.Background;```
 </li>
 <li>
 Wenn Sie ein neues Projekt erstellen, wird der Projektname als standardmäßiger Stamm-Namespace in allen Dateien verwendet. Benennen Sie den Namespace um, um den App-Dienstcode unter dem primären Projekt zu schachteln. Beispiel: `namespace AdventureWorks.VoiceCommands`. 
 </li>
 <li>
 Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das App-Dienstprojekt, und wählen Sie **Eigenschaften** aus. 
 </li>
 <li>
 Aktualisieren Sie auf der Registerkarte **Bibliothek** das Feld **Standard-Namespace** mit diesem Wert (für dieses Beispiel „AdventureWorks.VoiceCommands“). 
 </li>
 <li>
 Erstellen Sie eine neue Klasse zum Implementieren der Schnittstelle [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Diese Klasse erfordert eine [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode, die der Einstiegspunkt ist, wenn Cortana den Sprachbefehl erkennt. 
 </li>
        </ol>
    </li>
</ol>

Dies ist eine einfache Hintergrund-Aufgabenklasse aus der **Adventure Works**-App. Wir gehen darauf später ausführlicher ein.
> **Hinweis**    
> Die Hintergrund-Aufgabenklasse selbst sowie alle anderen Klassen im Hintergrund-Aufgabenprojekt müssen versiegelte öffentliche Klassen sein.
 
``` csharp
namespace AdventureWorks.VoiceCommands
{
    ...
    
    /// <summary>
    /// The VoiceCommandService implements the entry point for all voice commands.
    /// The individual commands supported are described in the VCD xml file. 
    /// The service entry point is defined in the appxmanifest.
    /// </summary>
    public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
    {
        ...
        
        /// <summary>
        /// The background task entrypoint. 
        /// 
        /// Background tasks must respond to activation by Cortana within 0.5 seconds, and must 
        /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
        /// input). There is no execution time limit on the background task managed by Cortana,
        /// but developers should use plmdebug (https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
        /// on the Cortana app package in order to prevent Cortana timing out the task during
        /// debugging.
        /// 
        /// The Cortana UI is dismissed if Cortana loses focus. 
        /// The background task is also dismissed even if being debugged. 
        /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
        /// Open the project properties for the app package (not the background task project), 
        /// and enable Debug -> "Do not launch, but debug my code when it starts". 
        /// Alternatively, add a long initial progress screen, and attach to the background task process while it executes.
        /// </summary>
        /// <param name="taskInstance">Connection to the hosting background service process.</param>
        public void Run(IBackgroundTaskInstance taskInstance)
        {
        
          //
          // TODO: Insert code 
          //
          //
        
    }        
  }
}
```

<ol start="7">
    <li>
    Deklarieren Sie Ihre Hintergrundaufgabe im App-Manifest als **AppService**.
    <ol type="i">
        <li>
        Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei „Package.appxmanifest“, und wählen Sie die Option **Code anzeigen** aus. 
        </li>
        <li>
        Suchen Sie das Element [**Anwendung**](https://msdn.microsoft.com/library/windows/apps/dn934738).
        </li>
        <li>
        Fügen Sie ein [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720)-Element zum [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738) hinzu.
        </li>
        <li>
        Fügen Sie ein [**Uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788)-Element zum [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720)-Element hinzu.
        </li>
        <li>Fügen Sie ein **Category**-Attribut zum Element **uap:Extension** hinzu, und legen Sie den Wert des Attributs **Category** auf „windows.appService“ fest.
        </li>
        <li>
        Fügen Sie ein **EntryPoint**-Attribut zum **uap:Extension**-Element hinzu, und legen Sie den Wert des **EntryPoint**-Attributs auf den Namen der Klasse fest, die [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) implementiert. In diesem Fall ist dies„AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService“.
        </li>
        <li>
        Fügen Sie ein [**Uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779)-Element zum **Uap:Extension**-Element hinzu.
        </li>
        <li>
        Fügen Sie ein **Name**-Attribut zum [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) hinzu, und legen Sie den Wert des **Name**-Attributs auf den Namen für den App-Dienst fest. In diesem Fall ist dies „AdventureWorksVoiceCommandService“.
        </li>
        <li>
        Fügen Sie ein zweites [**Uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788)-Element zu [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) hinzu.
        </li>
        <li>
        Fügen Sie ein **Category**-Attribut zu diesem [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788)-Element hinzu, und legen Sie den Wert des **Category**-Attributs auf „windows.personalAssistantLaunch“ fest.
        </li>
    </li> 
    </ol>
    </li>    
</ol>  

Dies ist das Manifest der Adventure Works-App:
```xml
<Package>
  <Applications>
    <Application>

      <Extensions>
        <uap:Extension Category="windows.appService" 
          EntryPoint="CortanaBack1.VoiceCommands.CortanaBack1VoiceCommandService">
          <uap:AppService Name="CortanaBack1VoiceCommandService"/>
        </uap:Extension>
        <uap:Extension Category="windows.personalAssistantLaunch"/>
      </Extensions>

    <Application>
  <Applications>
</Package>
```

<ol start="8">
    <li>
    Fügen Sie dieses App-Dienst-Projekt als Verweis im primären Projekt hinzu. 
    <ol type="i">
        <li>
        Klicken Sie mit der rechten Maustaste auf **Verweise**. 
        </li>
        <li>
        Wählen Sie **Verweis hinzufügen...** aus. 
        </li>
        <li>
        Erweitern Sie im Dialogfeld **Verweis-Manager** **Projekte**, und wählen Sie das App-Dienst-Projekt aus. 
        </li>
        <li>
        Klicken Sie auf „OK“. 
        </li>
    </ol>
    </li>
</ol>

## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>Erstellen einer VCD-Datei


1. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Namen Ihres primären Projekts, und wählen Sie **Hinzufügen > Neues Element** aus. Fügen Sie eine **XML-Datei** hinzu.
2. Geben Sie einen Namen für die [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593)-Datei ein (z. B. „AdventureWorksCommands.xml“), und klicken Sie auf „Hinzufügen“. 
3. Wählen Sie im **Projektmappen-Explorer** die [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593)-Datei aus.
4.  Legen Sie im Fenster **Eigenschaften** die Option **Buildvorgang** auf **Inhalt** fest, und legen Sie dann **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer** fest.

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>Bearbeiten der VCD-Datei

1. Fügen Sie ein **VoiceCommands**-Element mit einem **xmlns**-Attribut hinzu, das auf `http://schemas.microsoft.com/voicecommands/1.2` zeigt.

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

  Dieses [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt wird anschließend an [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205) übergeben.    
```CSharp
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
}
```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Behandeln der Aktivierung

Geben Sie an, wie Ihre App auf nachfolgende Sprachbefehlaktivierungen reagiert (nachdem sie mindestens einmal gestartet wurde und die Sätze mit den Sprachbefehlen installiert wurden).

1.  Überprüfen Sie, ob die App über einen Sprachbefehl aktiviert wurde.

    Überschreiben Sie das [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Ereignis, und überprüfen Sie, ob [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693) ist.

2.  Bestimmen Sie den Namen des Befehls und die Sprachausgabe.

    Rufen Sie einen Verweis auf ein [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755)-Objekt aus [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) ab, und fragen Sie die [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758)-Eigenschaft nach einem [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432)-Objekt ab.

    Um zu ermitteln, was der Benutzer gesagt hat, überprüfen Sie den Wert von [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) oder die semantischen Eigenschaften des erkannten Begriffs im Wörterbuch [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443).

3.  Führen Sie in Ihrer App die entsprechende Aktion aus; navigieren Sie beispielsweise zur gewünschten Seite.

In diesem Beispiel beziehen wir uns wieder auf die VCD-Datei aus Schritt 3: Bearbeiten der VCD-Datei.

Sobald das Spracherkennungsergebnis für den Sprachbefehl vorliegt, können wir den Befehlsnamen aus dem ersten Wert im [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438)-Array ableiten. Da in der VCD-Datei mehrere mögliche Sprachbefehle definiert wurden, müssen wir den Wert mit den Befehlsnamen in der VCD-Datei vergleichen und die entsprechende Aktion ausführen.

Die gängigste Aktion in einer Anwendung ist das Navigieren zu einer Seite mit Inhalt, der für den Kontext des Sprachbefehls relevant ist. In diesem Beispiel navigieren wir zur Seite **TripPage** und übergeben den Wert des Sprachbefehls, wie der Befehl eingegeben wurde, und den erkannten Ziel-Begriff (wenn zutreffend). Alternativ kann die App beim Navigieren zur Seite einen Navigationsparameter an [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) senden.

Sie können herausfinden, ob der Sprachbefehl, mit dem Ihre App gestartet wurde, tatsächlich gesprochen wurde oder ob er als Text eingegeben wurde. Dazu verwenden Sie das [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445)-Wörterbuch und den **commandMode**-Schlüssel. Der Wert des Schlüssels lautet „voice“ oder „text“. Ist der Wert des Schlüssels „voice“, erwägen Sie die Verwendung von Sprachsynthese ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) in der App, um für den Benutzer gesprochenes Feedback bereitzustellen.

Ermitteln Sie mithilfe von [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) die gesprochenen Inhalte in den Einschränkungen **PhraseList** oder **PhraseTopic** eines **ListenFor**-Elements. Der Wörterbuchschlüssel ist der Wert des **Label**-Attributs des **PhraseList**-Elements oder des **PhraseTopic**-Elements. Hier zeigen wir, wie auf den Wert des **{destination}**-Begriffs zugegriffen wird.

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

## <span id="Handle_the_voice_command_in_the_app_service"></span><span id="handle_the_voice_command_in_the_app_service"></span><span id="HANDLE_THE_VOICE_COMMAND_IN_THE_APP_SERVICE"></span>Behandeln des Sprachbefehls im App-Dienst


Verarbeiten Sie den Sprachbefehl im App-Dienst.


1.  Fügen Sie Folgendes mithilfe von Direktiven zu Ihrer Datei mit Sprachbefehlsdiensten hinzu, „AdventureWorksVoiceCommandService.cs“ für dieses Beispiel.
```csharp
using Windows.ApplicationModel.VoiceCommands;
using Windows.ApplicationModel.Resources.Core;
using Windows.ApplicationModel.AppService;
```

2.  Richten Sie eine Dienstverzögerung ein, damit Ihre App beim Verarbeiten des Sprachbefehls nicht beendet wird.
3.  Stellen Sie sicher, dass Ihre Hintergrundaufgabe als App-Dienst ausgeführt wird, der per Sprachbefehl aktiviert wird.

    1.  Wandeln Sie [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) in [**Windows.ApplicationModel.AppService.AppServiceTriggerDetails**](https://msdn.microsoft.com/library/windows/apps/dn921727) um.
    2.  Vergewissern Sie sich, dass [**IBackgroundTaskInstance.TriggerDetails.Name**](https://msdn.microsoft.com/library/windows/apps/br224807) der Name des App-Diensts in der Datei „Package.appxmanifest“ ist.

4.  Verwenden Sie [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802), um ein [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204)-Element für **Cortana** zum Abrufen des Sprachbefehls zu erstellen.
5.  Registrieren Sie einen Ereignishandler für das Element [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).[**VoiceCommandCompleted**](https://msdn.microsoft.com/library/windows/apps/dn706584), um eine Benachrichtigung empfangen zu können, wenn der App-Dienst aufgrund eines Abbruchs durch den Benutzer geschlossen wird.
6.  Registrieren Sie einen Ereignishandler für das Element [**IBackgroundTaskInstance.Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798), um eine Benachrichtigung empfangen zu können, wenn der App-Dienst aufgrund eines unerwarteten Fehlers geschlossen wird.
7.  Bestimmen Sie den Namen des Befehls und die Sprachausgabe.

    1.  Verwenden Sie die Eigenschaft [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/dn974162).[**CommandName**](https://msdn.microsoft.com/library/windows/apps/dn706589), um den Namen des Sprachbefehls zu ermitteln.
    2.  Um zu ermitteln, was der Benutzer gesagt hat, überprüfen Sie den Wert von [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) oder die semantischen Eigenschaften des erkannten Begriffs im Wörterbuch [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443).

7.  Führen Sie in Ihrem App-Dienst die entsprechende Aktion aus.
8.  Zeigen Sie das Feedback für den Sprachbefehl über **Cortana** an, und führen Sie die Sprachausgabe durch.

    1.  Bestimmen Sie die Zeichenfolgen, die von **Cortana** angezeigt und für Benutzer als Antwort auf den Sprachbefehl gesprochen werden sollen, und erstellen Sie ein [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182)-Objekt. Eine Anleitung, wie Sie die von **Cortana** angezeigten und gesprochenen Feedback-Zeichenfolgen auswählen, finden Sie unter [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233).
    2.  Verwenden Sie die Instanz [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204), um den Status bzw. Abschluss an **Cortana** zu melden, indem [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) oder [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) mit dem Objekt **VoiceCommandServiceConnection** aufgerufen wird.

    In diesem Beispiel beziehen wir uns wieder auf die VCD-Datei aus Schritt 3: Bearbeiten der VCD-Datei.

```csharp
public sealed class VoiceCommandService : IBackgroundTask
    {
      private BackgroundTaskDeferral serviceDeferral;
      VoiceCommandServiceConnection voiceServiceConnection;

      public async void Run(IBackgroundTaskInstance taskInstance)
      {
      //Take a service deferral so the service isn&#39;t terminated.
        this.serviceDeferral = taskInstance.GetDeferral();

        taskInstance.Canceled += OnTaskCanceled;

        var triggerDetails = 
          taskInstance.TriggerDetails as AppServiceTriggerDetails;

        if (triggerDetails != null &amp;&amp; 
          triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint")
        {
          try
          {
 voiceServiceConnection = 
   VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
     triggerDetails);

 voiceServiceConnection.VoiceCommandCompleted += 
   VoiceCommandCompleted;

 VoiceCommand voiceCommand = await
 voiceServiceConnection.GetVoiceCommandAsync();

 switch (voiceCommand.CommandName)
 {
   case "whenIsTripToDestination":
   {
     var destination = 
       voiceCommand.Properties["destination"][0];
     SendCompletionMessageForDestination(destination);
     break;
   }

   // As a last resort, launch the app in the foreground.
   default:
     LaunchAppInForeground();
     break;
 }
          }
          finally
          {
 if (this.serviceDeferral != null)
 {
   // Complete the service deferral.
   this.serviceDeferral.Complete();
 }
          }
        }
      }

      private void VoiceCommandCompleted(
        VoiceCommandServiceConnection sender, 
        VoiceCommandCompletedEventArgs args)
      {
        if (this.serviceDeferral != null)
        {
          // Insert your code here.
          // Complete the service deferral.
          this.serviceDeferral.Complete();
        }
      }

      private async void SendCompletionMessageForDestination(
        string destination)
      {
        // Take action and determine when the next trip to destination
        // Insert code here.
        
        // Replace the hardcoded strings used here with strings 
        // appropriate for your application.

        // First, create the VoiceCommandUserMessage with the strings 
        // that Cortana will show and speak.
        var userMessage = new VoiceCommandUserMessage();
        userMessage.DisplayMessage = "Here’s your trip.";
        userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";

        // Optionally, present visual information about the answer.
        // For this example, create a VoiceCommandContentTile with an 
        // icon and a string.
        var destinationsContentTiles = new List<VoiceCommandContentTile>();

        var destinationTile = new VoiceCommandContentTile();
        destinationTile.ContentTileType = 
          VoiceCommandContentTileType.TitleWith68x68IconAndText;
        // The user can tap on the visual content to launch the app. 
        // Pass in a launch argument to enable the app to deep link to a 
        // page relevant to the item displayed on the content tile.
        destinationTile.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        destinationTile.Title = "Las Vegas";
        destinationTile.TextLine1 = "August 3rd 2015";
        destinationsContentTiles.Add(destinationTile);

        // Create the VoiceCommandResponse from the userMessage and list    
        // of content tiles.
        var response = 
          VoiceCommandResponse.CreateResponse(
 userMessage, destinationsContentTiles);

        // Cortana will present a “Go to app_name” link that the user 
        // can tap to launch the app. 
        // Pass in a launch to enable the app to deep link to a page 
        // relevant to the voice command.
        response.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        
        // Ask Cortana to display the user message and content tile and 
        // also speak the user message.
        await voiceServiceConnection.ReportSuccessAsync(response);
      }

      private async void LaunchAppInForeground()
      {
        var userMessage = new VoiceCommandUserMessage();
        userMessage.SpokenMessage = "Launching Adventure Works";

        var response = VoiceCommandResponse.CreateResponse(userMessage);

        // When launching the app in the foreground, pass an app 
        // specific launch parameter to indicate what page to show.
        response.AppLaunchArgument = "showAllTrips=true";

        await voiceServiceConnection.RequestAppLaunchAsync(response);
      }
    }
```

Nach dem Aktivieren hat der App-Dienst 0,5 Sekunden Zeit, [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) aufzurufen. **Cortana** verwendet die von der App bereitgestellten Daten, um das in der VCD-Datei angegebene Feedback anzuzeigen und über Sprache auszugeben. Wenn die App länger als 0,5 Sekunden für den Aufruf benötigt, wird von **Cortana** ein Übergabebildschirm eingefügt (wie hier gezeigt). **Cortana** zeigt den Übergabebildschirm an, bis die Anwendung **ReportSuccessAsync** aufruft, oder für maximal 5 Sekunden. Falls der App-Dienst **ReportSuccessAsync** oder eine andere [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204)-Methode, über die **Cortana** mit Informationen versorgt wird, nicht aufruft, erhält der Benutzer eine Fehlermeldung, und der App-Dienst wird abgebrochen.

![Einfache Abfrage mit Status- und Ergebnisbildschirm und der Adventure Works-App im Hintergrund](images/cortana-backgroundapp-progress-result.png)

## <span id="related_topics"></span>Verwandte Artikel


**Entwickler**
* [Cortana-Interaktionen](cortana-interactions.md)
* [Definieren von benutzerdefinierten Erkennungseinschränkungen](define-custom-recognition-constraints.md)
* [Interagieren mit einer Hintergrund-App in Cortana](interact-with-a-background-app-in-cortana.md)
* [**VCD-Elemente und -Attribute v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)
* [Schnellstart: Verwenden von Datei- oder Bildressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325)
* [Benennen von Ressourcen mithilfe von Qualifizierern](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)

**Designer**
* [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Richtlinien für den Sprachentwurf](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Reaktionsfähiges Design – Grundlagen für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn958435)
* [Richtlinien für die Ressourcen für Kacheln und Symbole](https://msdn.microsoft.com/library/windows/apps/mt412102)

**Beispiele**
* [Cortana-Sprachbefehlbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


