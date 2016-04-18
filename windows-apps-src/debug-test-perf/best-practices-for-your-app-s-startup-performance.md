---
ms.assetid: 00ECF6C7-0970-4D5F-8055-47EA49F92C12
title: Bewährte Methoden für die Leistung Ihrer App beim Starten
description: Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit optimalen Startzeiten, indem Sie die Vorgehensweise bei Start und Aktivierung optimieren.
---
# Bewährte Methoden für die Leistung Ihrer App beim Starten

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit optimalen Startzeiten, indem Sie die Vorgehensweise bei Start und Aktivierung optimieren.

## Bewährte Methoden für die Leistung Ihrer App beim Starten

Benutzer nehmen die Schnelligkeit (oder die Langsamkeit) einer App teilweise auch anhand des Umstands wahr, wie lange der Start der App dauert. Für die Zwecke dieses Themas legen wir Folgendes fest: Die Startzeit einer App beginnt, wenn der Benutzer die Anwendung startet, und sie endet, wenn der Benutzer auf sinnvolle Weise mit der App interagieren kann. Dieser Abschnitt enthält Vorschläge, wie Sie beim Starten Ihrer App eine bessere Leistung erzielen können.

### Messen der Startzeit Ihrer App

Starten Sie die App einige Male, bevor Sie ihre Startzeit tatsächlich messen. Sie erhalten so einen Richtwert für Ihre Messung und können sicherstellen, dass der gemessene Startzeitraum möglichst kurz ist.

Wenn Ihre UWP-App auf den Computern Ihrer Kunden aktiviert wird, wurde sie im Rahmen der .NET Native-Toolkette kompiliert. .NET Native ist eine fortschrittliche Kompilierungstechnologie, mit der MSIL-Code in Computercode für die systemeigene Ausführung konvertiert wird. .NET Native-Apps starten schneller, verbrauchen weniger Arbeitsspeicher und benötigen weniger Akkuenergie als ihre MSIL-Gegenstücke. Anwendungen, die mit .NET Native erstellt werden, sind mit einer benutzerdefinierten Runtime und dem neuen konvergierten .NET Core verknüpft, der auf allen Geräten ausgeführt werden kann. Sie sind dann nicht mehr von der standardmäßigen .NET-Implementierung abhängig. Ihre App nutzt auf Ihrem Entwicklungscomputer standardmäßig .NET Native, wenn Sie sie im Modus „Release“ erstellen. CoreCLR wird verwendet, wenn Sie sie im Modus „Debug“ erstellen. Sie können dies in Visual Studio auf der Seite für die Erstellung unter „Eigenschaften“ (C#) konfigurieren oder „Kompilieren“ -> „Erweitert in ‚Mein Projekt‘“ (VB) verwenden. Suchen Sie nach einem Kontrollkästchen mit der Bezeichnung „Mit systemeigener .NET-Toolkette kompilieren“.

Sie sollten die Messungen natürlich so durchführen, dass die Messergebnisse der Wahrnehmung des Endbenutzers entsprechen. Wenn Sie sich nicht sicher sind, ob Sie die App auf Ihrem Entwicklungscomputer mit systemeigenem Code kompilieren, können Sie also das Tool „Native Image Generator“ (Ngen.exe) ausführen, um die App vor dem Messen der Startzeit vorab zu kompilieren.

Im folgenden Verfahren wird beschrieben, wie Sie „Ngen.exe“ zum Vorkompilieren Ihrer App ausführen.

**So führen Sie „Ngen.exe“ aus**

1.  Führen Sie Ihre App mindestens einmal aus, um sicherzustellen, dass Sie von „Ngen.exe“ erkannt wird.
2.  Öffnen Sie die **Aufgabenplanung**, indem Sie eine der folgenden Aktionen ausführen:
    -   Suchen Sie auf dem Startbildschirm nach „Aufgabenplanung“.
    -   Führen Sie „taskschd.msc“ aus.
3.  Erweitern Sie im linken Bereich der **Aufgabenplanung** das Element **Aufgabenplanungsbibliothek**.
4.  Erweitern Sie **Microsoft**.
5.  Erweitern Sie **Windows**.
6.  Wählen Sie **.NET Framework** aus.
7.  Wählen Sie aus der Aufgabenliste **.NET Framework NGEN 4.x**.

    Wenn Sie einen 64-Bit-Computer verwenden, gibt es auch **.NET Framework NGEN v4.x 64**. Wenn Sie eine 64-Bit-App erstellen, wählen Sie .**NET Framework NGEN v4.x 64** aus.

8.  Klicken Sie im Menü **Aktion** auf **Ausführen**.

Von „Ngen.exe“ werden alle Apps auf dem Computer vorkompiliert, die verwendet wurden und keine systemeigenen Bilder enthalten. Wenn viele Apps vorkompiliert werden müssen, kann dies einige Zeit in Anspruch nehmen. Nachfolgende Ausführungen erfolgen jedoch viel schneller.

Wenn Sie Ihre App erneut kompilieren, wird das systemeigene Bild nicht mehr verwendet. Stattdessen wird die App rechtzeitig kompiliert, was bedeutet, dass sie während ihrer Ausführung kompiliert wird. Sie müssen „Ngen.exe“ erneut ausführen, um eine neues systemeigenes Bild abzurufen.

### Verzögern Sie die Arbeit möglichst lange

Wenn Sie die Startzeit Ihrer App beschleunigen möchten, führen Sie nur die Arbeit aus, die unbedingt erforderlich ist, damit der Benutzer mit dem Interagieren mit der App beginnen kann. Dies kann besonders hilfreich sein, wenn Sie das Laden zusätzlicher Assemblys verzögern können. Die Common Language Runtime (CLR) lädt eine Assembly, wenn sie das erste Mal verwendet wird. Wenn Sie die Anzahl der geladenen Assemblys minimieren können, können Sie möglicherweise die Startzeit Ihrer App beschleunigen und ihren Speicherverbrauch optimieren.

### Separates Erledigen langer Abläufe

Ihre App kann interaktiv sein, selbst wenn Teile der App noch nicht voll funktionsfähig sind. Wenn von Ihrer App beispielsweise Daten angezeigt werden, deren Abruf einige Zeit in Anspruch nimmt, können Sie diesen Code separat vom Startcode der App ausführen, indem Sie die Daten asynchron abrufen. Wenn die Daten verfügbar sind, füllen Sie die Benutzeroberfläche der App mit den Daten auf.

Viele APIs für die Universelle Windows-Plattform (UWP), die Daten abrufen, sind asynchron, sodass Sie Daten wahrscheinlich sowieso asynchron abrufen. Weitere Informationen zu asynchronen APIs finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337). Wenn Sie Arbeit ausführen, für die keine asynchronen APIs verwendet werden, können Sie für lange ausgeführte Arbeit die Task-Klasse verwenden, damit der Benutzer mit der App interagieren kann. Dadurch reagiert Ihre App gegenüber dem Benutzer weiterhin, während die Daten geladen werden.

Wenn es sehr lange dauert, bis ein Teil der Benutzeroberfläche Ihrer App geladen wird, sollten Sie in diesem Bereich eine Zeichenfolge wie „Aktuelle Daten werden abgerufen“ hinzufügen, damit der Benutzer weiß, dass die App noch arbeitet.

## Minimieren der Startzeit

Mit Ausnahme wirklich sehr einfacher Apps benötigt jede App eine gewisse Zeit, um Ressourcen zu laden, XAML zu analysieren, Datenstrukturen einzurichten und bei Aktivierung Logik auszuführen. Diese Verzögerung ist für die Benutzer wahrnehmbar. In diesem Thema analysieren wir den Aktivierungsprozess, indem wir ihn in drei Phasen aufschlüsseln. Außerdem finden Sie hier Tipps zum Verringern der benötigten Zeit für die einzelnen Phasen sowie Techniken, mit denen Sie die Phasen beim Start Ihrer App für den Benutzer angenehmer gestalten können.

Die Aktivierungsperiode ist die Zeit zwischen dem Starten einer App durch einen Benutzer und dem Zeitpunkt, zu dem die App einsatzbereit ist. Da es sich bei dieser Zeit um den ersten Eindruck handelt, den der Benutzer von Ihrer App erhält, ist dieser Wert überaus wichtig. Der Benutzer erwartet sowohl vom System als auch von den Apps ein verzögerungs- und unterbrechungsfreies Feedback. Dauert der Start von Apps zu lange, kommen schnell Zweifel an der Qualität des Systems und der App auf. Schlimmer noch: Vergeht bis zur Aktivierung einer App zu viel Zeit, wird die App ggf. vom Process Lifetime Manager (PLM) beendet oder vom Benutzer deinstalliert.

### Einführung in die Phasen des Startvorgangs

Der Startvorgang umfasst mehrere „bewegliche Teile“, die alle richtig koordiniert werden müssen, um die höchste Benutzerfreundlichkeit zu erzielen. Zwischen dem Klicken auf die App-Kachel durch den Benutzer und dem Anzeigen des Anwendungsinhalts werden die unten angegebenen Schritte ausgeführt.

-   Die Windows-Shell startet den Prozess, und „Main“ wird aufgerufen.
-   Das Application-Objekt wird erstellt.
    -   (ProjectTemplate) Der Konstruktor ruft InitializeComponent auf, und dies führt dazu, dass „App.xaml“ analysiert wird und Objekte erstellt werden.
-   Das Application.OnLaunched-Ereignis wird ausgelöst.
    -   (ProjectTemplate) Der App-Code erstellt einen „Frame“ und navigiert zu „MainPage“.
    -   (ProjectTemplate) Der MainPage-Konstruktor ruft InitializeComponent auf, und dies führt dazu, dass „MainPage.xaml“ analysiert wird und Objekte erstellt werden.
    -   (ProjectTemplate) Window.Current.Activate() wird aufgerufen.
-   Die XAML-Plattform führt den Layoutdurchlauf durch, einschließlich der Messung und Anordnung.
    -   ApplyTemplate bewirkt, dass für jedes Steuerelement entsprechender Steuerelementinhalt erstellt wird. Hierfür wird beim Starten normalerweise die meiste Layoutzeit verbraucht.
-   „Render“ wird aufgerufen, um visuelle Elemente für alle Fensterinhalte zu erstellen.
-   Der Frame wird dem Desktopfenster-Manager präsentiert.

### Verringern der Schritte im Startvorgangspfad

Halten Sie Ihren Startcodepfad frei von allen Elementen, die für den ersten Frame nicht benötigt werden.

-   Falls Sie über DLLs mit Steuerelementen verfügen, die für den ersten Frame nicht benötigt werden, sollten Sie erwägen, das Laden später durchzuführen.
-   Wenn ein Teil der Benutzeroberfläche von Daten in der Cloud abhängig ist, sollten Sie die Benutzeroberfläche aufteilen. Zeigen Sie zuerst den Teil der Benutzeroberfläche an, der nicht von Clouddaten abhängig ist, und zeigen Sie asynchron die von der Cloud abhängige Benutzeroberfläche an. Erwägen Sie außerdem, Daten lokal zwischenzuspeichern, damit die Anwendung auch offline funktioniert und von einer langsamen Netzwerkverbindung nicht beeinträchtigt wird.
-   Blenden Sie eine Statusanzeige ein, wenn die Benutzeroberfläche auf Daten wartet.
-   Verwenden Sie App-Designs mit Bedacht, bei denen viele Analysen von Konfigurationsdateien durchgeführt werden oder bei denen die Benutzeroberfläche dynamisch per Code generiert wird.

### Verringern der Elementanzahl

Die Startleistung in einer XAML-App korreliert direkt mit der Anzahl von Elementen, die Sie während des Startvorgangs erstellen. Je weniger Elemente Sie erstellen, desto weniger Zeit benötigt Ihre App zum Starten. Als grobe Daumenregel gilt, dass die Erstellung jedes Elements 1 ms dauert.

-   In Steuerelementen von Elementen verwendete Vorlagen können die größte Auswirkung haben, da sie mehrere Male wiederholt werden. Weitere Informationen finden Sie unter [Optimieren der ListView- und GridView-Benutzeroberfläche](optimize-gridview-and-listview.md).
-   Da UserControls und Steuerelementvorlagen erweitert werden, sollten diese Elemente ebenfalls berücksichtigt werden.
-   Wenn Sie beliebige XAML-Daten erstellen, die nicht auf dem Bildschirm angezeigt werden, sollte es einen guten Grund dafür geben, dass diese XAML-Elemente während des Starts erstellt werden.

Im Fenster mit der [Visuellen Live-Struktur von Visual Studio](http://blogs.msdn.com/b/visualstudio/archive/2015/02/24/introducing-the-ui-debugging-tools-for-xaml.aspx) wird die Anzahl der untergeordneten Elemente für jeden Knoten der Struktur angezeigt.

![Visuelle Live-Struktur:](images/live-visual-tree.png)

**Verwenden Sie „x:DeferLoadStrategy“**. Das Reduzieren eines Elements oder das Festlegen der Deckkraft auf 0 verhindert nicht, dass das Element erstellt wird. Mit „x:DeferLoadStrategy“ können Sie das Laden eines UI-Bestandteils verschieben und es später laden, wenn es benötigt wird. Dies ist eine gute Möglichkeit, um die Verarbeitung von UI-Elementen aufzuschieben, die auf dem Startbildschirm nicht sichtbar sind. Sie können sie dann bei Bedarf oder im Rahmen einer Verzögerungslogik laden. Um das Laden auszulösen, müssen Sie für das Element nur FindName aufrufen. Ein Beispiel und weitere Informationen finden Sie unter [x:DeferLoadStrategy-Attribut](https://msdn.microsoft.com/library/windows/apps/Mt204785).

**Virtualisierung**. Falls Ihre Benutzeroberfläche Listen- oder Wiederholungsinhalte enthält, empfehlen wir Ihnen dringend, die UI-Virtualisierung zu nutzen. Wenn die Listen-UI nicht virtualisiert ist, werden alle Elemente vorher erstellt, und dies kann den Startvorgang verlängern. Weitere Informationen finden Sie unter [Optimieren der ListView- und GridView-Benutzeroberfläche](optimize-gridview-and-listview.md).

Bei der Anwendungsleistung geht es nicht nur um die reine Leistung, sondern auch um die Wahrnehmung. Wenn Sie die Reihenfolge der Vorgänge so ändern, dass visuelle Aspekte bevorzugt werden, erweckt dies bei Benutzern normalerweise den Eindruck, dass die Anwendung schneller startet. Benutzer sehen die Anwendung als geladen an, wenn der Inhalt auf dem Bildschirm zu sehen ist. In der Regel müssen Anwendungen im Rahmen des Startvorgangs mehrere Dinge erledigen. Da nicht alle diese Schritte erforderlich sind, um die Benutzeroberfläche anzuzeigen, sollten sie aufgeschoben oder mit einer niedrigeren Priorität als die Benutzeroberfläche versehen werden.

In diesem Thema geht es um das „erste Bild“. Dieser Begriff stammt aus dem Animations- und Fernsehbereich und ist ein Maß dafür, wie lange es dauert, bis der Endbenutzer Inhalte sieht.

### Verbessern des Eindrucks beim Start

Zur Veranschaulichung der einzelnen Startphasen und der unterschiedlichen Techniken, mit deren Hilfe der Benutzer während des gesamten Prozesses Feedback erhält, verwenden wir ein Beispiel mit einem einfachen Onlinespiel. In diesem Beispiel ist die erste Aktivierungsphase des Spiels die Zeit zwischen dem Moment, in dem der Benutzer auf die Kachel des Spiels tippt, und dem Moment, in dem das Spiel mit der Ausführung des Codes beginnt. Das System verfügt über keinerlei Inhalte, die es dem Benutzer während dieser Zeitspanne anzeigen könnte, um ihn darüber zu informieren, dass das richtige Spiel gestartet wurde. Einen solchen Inhalt können Sie dem System in Form eines Begrüßungsbildschirms zur Verfügung stellen. Das Spiel informiert dann den Benutzer über den Abschluss der ersten Aktivierungsphase, indem es den statischen Begrüßungsbildschirm durch die eigene Benutzeroberfläche ersetzt, sobald es mit der Ausführung des Codes beginnt.

Die zweite Aktivierungsphase umfasst das Erstellen und Initialisieren der unbedingt erforderlichen Strukturen für das Spiel. Wenn eine App die Anfangs-UI schnell mit den Daten erstellen kann, die nach der ersten Aktivierungsphase zur Verfügung stehen, kommt der zweiten Phase keine große Bedeutung zu, und Sie können die UI umgehend anzeigen. Andernfalls sollte die App während der Initialisierung eine Ladeseite anzeigen.

Wie Sie diese Ladeseite gestalten, liegt ganz bei Ihnen. Hier genügt unter Umständen bereits ein einfacher Statusbalken oder -ring. Wichtig ist, dass die App anzeigt, dass sie Aufgaben ausführt, bevor sie wieder reagiert. Im Falle des Spiels soll der Anfangsbildschirm angezeigt werden. Für diese UI müssen aber zunächst einige Bilder und Sounds vom Datenträger in den Arbeitsspeicher geladen werden. Da diese Aufgaben ein paar Sekunden dauern, wird der Benutzer von der App darüber informiert. Hierzu ersetzt sie den Begrüßungsbildschirm durch eine Ladeseite mit einer einfachen, zum Spiel passenden Animation.

Die dritte Phase beginnt, nachdem das Spiel über ein Mindestmaß an Informationen verfügt, um eine interaktive UI zu erstellen und die Ladeseite damit zu ersetzen. Bislang stehen dem Onlinespiel lediglich die Inhalte zur Verfügung, die die App vom Datenträger geladen hat. Das Spiel kann zwar bereits standardmäßig mit genügend Inhalten versehen werden, um eine interaktive UI zu erstellen, da es sich hierbei aber um ein Onlinespiel handelt, kann es erst verwendet werden, nachdem es eine Verbindung mit dem Internet hergestellt und einige Zusatzinfos heruntergeladen hat. Bis alle für die Verwendung benötigten Infos vorliegen, kann der Benutzer bereits mit der UI interagieren. Für Features, die zusätzliche Daten aus dem Web erfordern, muss allerdings deutlich gemacht werden, dass noch Inhalte geladen werden. Gegebenenfalls dauert es etwas, bis eine App vollständig funktionsfähig ist. Daher ist es wichtig, die Funktionen schnellstmöglich verfügbar zu machen.

Nach dieser Vorstellung der drei Aktivierungsphasen des Onlinespiels widmen wir uns nun dem eigentlichen Code.

### Phase 1

Vor dem Start der App muss diese dem System mitteilen, was als Begrüßungsbildschirm angezeigt werden soll. Hierzu werden für das SplashScreen-Element im App-Manifest ein Bild und eine Hintergrundfarbe angegeben (siehe Beispiel). Diese werden von Windows nach dem Aktivierungsbeginn der App angezeigt.

```xml
<Package ...>
  ...
  <Applications>
    <Application ...>
      <VisualElements ...>
        ...
        <SplashScreen Image="Images\splashscreen.png" BackgroundColor="#000000" />
        ...
      </VisualElements>
    </Application>
  </Applications>
</Package>
```

Weitere Informationen finden Sie unter [Hinzufügen eines Begrüßungsbildschirms](https://msdn.microsoft.com/library/windows/apps/Mt187306).

Initialisieren Sie mit dem Konstruktor der App lediglich Datenstrukturen, die für die App unbedingt erforderlich sind. Der Konstruktor wird nur beim ersten Ausführen der App aufgerufen, aber nicht unbedingt bei jeder Aktivierung der App. So wird der Konstruktor beispielsweise nicht für eine ausgeführte App aufgerufen, die in den Hintergrund versetzt wurde und anschließend mittels Vertrag für „Suche“ aktiviert wird.

### Phase 2

Eine App kann aus unterschiedlichen Gründen aktiviert werden, und diese müssen unter Umständen alle individuell behandelt werden. Durch Überschreiben der Methoden [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/BR242330), [**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701797), [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/BR242331), [**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701799), [**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701801), [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/BR242335), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/BR242336) und [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701806) können Sie jede Aktivierungsursache behandeln. In diesen Methoden muss die App unter anderem eine UI erstellen, sie [**Window.Content**](https://msdn.microsoft.com/library/windows/apps/BR209051) zuweisen und anschließend [**Window.Activate**](https://msdn.microsoft.com/library/windows/apps/BR209046) aufrufen. An diesem Punkt wird der Begrüßungsbildschirm durch die von der App erstellte UI ersetzt. Hierbei kann es sich entweder um einen Ladebildschirm oder bereits um die eigentliche UI der App handeln, sofern bei der Aktivierung genügend Informationen für deren Erstellung vorliegen.

> [!div class="tabbedCodeSnippets"]
```csharp
public partial class App : Application
{
    // A handler for regular activation.
    async protected override void OnLaunched(LaunchActivatedEventArgs args)
    {
        base.OnLaunched(args);

        // Asynchronously restore state based on generic launch.

        // Create the ExtendedSplash screen which serves as a loading page while the
        // reader downloads the section information.
        ExtendedSplash eSplash = new ExtendedSplash();

        // Set the content of the window to the extended splash screen.
        Window.Current.Content = eSplash;

        // Notify the Window that the process of activation is completed
        Window.Current.Activate();
    }

    // a different handler for activation via the search contract
    async protected override void OnSearchActivated(SearchActivatedEventArgs args)
    {
        base.OnSearchActivated(args);

        // Do an asynchronous restore based on Search activation

        // the rest of the code is the same as the OnLaunched method
    }
}

partial class ExtendedSplash : Page
{
    // This is the UIELement that's the game's home page.
    private GameHomePage homePage;

    public ExtendedSplash()
    {
        InitializeComponent();
        homePage = new GameHomePage();
    }

    // Shown for demonstration purposes only.
    // This is typically autogenerated by Visual Studio.
    private void InitializeComponent()
    {
    }
}
```
```vb
    Partial Public Class App
    Inherits Application

    ' A handler for regular activation.
    Protected Overrides Async Sub OnLaunched(ByVal args As LaunchActivatedEventArgs)
        MyBase.OnLaunched(args)

        ' Asynchronously restore state based on generic launch.

        ' Create the ExtendedSplash screen which serves as a loading page while the
        ' reader downloads the section information.
        Dim eSplash As New ExtendedSplash()

        ' Set the content of the window to the extended splash screen.
        Window.Current.Content = eSplash

        ' Notify the Window that the process of activation is completed
        Window.Current.Activate()
    End Sub

    ' a different handler for activation via the search contract
    Protected Overrides Async Sub OnSearchActivated(ByVal args As SearchActivatedEventArgs)
        MyBase.OnSearchActivated(args)

        ' Do an asynchronous restore based on Search activation

        ' the rest of the code is the same as the OnLaunched method
    End Sub
End Class

Partial Friend Class ExtendedSplash
    Inherits Page

    Public Sub New()
        InitializeComponent()

        ' Downloading the data necessary for 
        ' initial UI on a background thread.
        Task.Run(Sub() DownloadData())
    End Sub

    Private Sub DownloadData()
        ' Download data to populate the initial UI.

        ' Create the first page. 
        Dim firstPage As New MainPage()

        ' Add the data just downloaded to the first page

        ' Replace the loading page, which is currently 
        ' set as the window’s content, with the initial UI for the app
        Window.Current.Content = firstPage
    End Sub

    ' Shown for demonstration purposes only.
    ' This is typically autogenerated by Visual Studio.
    Private Sub InitializeComponent()
    End Sub
End Class 
```

Apps, die im Aktivierungshandler eine Ladeseite anzeigen, beginnen mit der UI-Erstellung im Hintergrund. Nach Erstellung dieses Elements tritt dessen [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723)-Ereignis auf. Im Ereignishandler ersetzen Sie den Fensterinhalt (also den derzeit angezeigten Ladebildschirm) durch die neu erstellte Startseite.

Bei Apps mit längerer Initialisierungsperiode ist das Anzeigen einer Ladeseite unverzichtbar. Abgesehen davon, dass der Benutzer Feedback zum Aktivierungsprozess erhält, wird der Prozess beendet, wenn [**Window.Activate**](https://msdn.microsoft.com/library/windows/apps/BR209046) nicht innerhalb von 15 Sekunden nach dem Start des Aktivierungsprozess aufgerufen wird.

> [!div class="tabbedCodeSnippets"]
```csharp
partial class GameHomePage : Page
{
    public GameHomePage()
    {
        InitializeComponent();

        // add a handler to be called when the home page has been loaded
        this.Loaded += ReaderHomePageLoaded;

        // load the minimal amount of image and sound data from disk necessary to create the home page.        
    }
    
    void ReaderHomePageLoaded(object sender, RoutedEventArgs e)
    {
        // set the content of the window to the home page now that it’s ready to be displayed.
        Window.Current.Content = this;
    }

    // Shown for demonstration purposes only.
    // This is typically autogenerated by Visual Studio.
    private void InitializeComponent()
    {
    }
}
```
```vb
    Partial Friend Class GameHomePage
    Inherits Page

    Public Sub New()
        InitializeComponent()

        ' add a handler to be called when the home page has been loaded
        AddHandler Me.Loaded, AddressOf ReaderHomePageLoaded

        ' load the minimal amount of image and sound data from disk necessary to create the home page.        
    End Sub

    Private Sub ReaderHomePageLoaded(ByVal sender As Object, ByVal e As RoutedEventArgs)
        ' set the content of the window to the home page now that it’s ready to be displayed.
        Window.Current.Content = Me
    End Sub

    ' Shown for demonstration purposes only.
    ' This is typically autogenerated by Visual Studio.
    Private Sub InitializeComponent()
    End Sub
End Class
```

Ein Beispiel für die Verwendung erweiterter Begrüßungsbildschirme finden Sie im [Beispiel für einen Begrüßungsbildschirm](http://go.microsoft.com/fwlink/p/?linkid=234889).

### Phase 3

Nur weil die App die UI anzeigt, heißt das noch nicht, dass sie auch vollständig verwendungsbereit ist. Im Falle unseres Spiels werden für Features, die Daten aus dem Internet benötigen, Platzhalter angezeigt. An diesem Punkt lädt das Spiel die zusätzlichen Daten herunter, die benötigt werden, um die App in vollem Umfang nutzen zu können, und aktiviert nach und nach die Features, für die bereits die erforderlichen Daten abgerufen wurden.

Manchmal können die für die Aktivierung erforderlichen Inhalte auch direkt in die App gepackt werden. Dies ist beispielsweise bei einem einfachen Spiel der Fall. Dadurch gestaltet sich der Aktivierungsprozess ziemlich einfach. Viele Programme (wie Newsreader-Apps, Fotoanzeige-Apps usw.) müssen allerdings Infos aus dem Web beziehen, damit sie verwendet werden können. Diese Daten können zum Teil recht umfangreich ausfallen, was das Herunterladen zu einer zeitaufwendigen Angelegenheit machen kann. Die Art und Weise, wie die App während des Aktivierungsprozesses an diese Daten gelangt, beeinflusst unter Umständen sehr stark den Eindruck, den der Benutzer von der Leistung der App bekommt.

Natürlich könnten Sie minutenlang eine Ladeseite (oder schlimmer noch: einen Begrüßungsbildschirm) anzeigen, während die App versucht, in der ersten oder zweiten Aktivierungsphase sämtliche für die Verwendung benötigte Daten herunterzuladen. Dies vermittelt jedoch den Eindruck, die App habe sich aufgehängt, oder führt dazu, dass sie vom System beendet wird. Wir empfehlen, eine App die Mindestmenge an Daten herunterladen zu lassen, die sie benötigt, um in Phase 2 eine interaktive UI mit Platzhalterelementen anzuzeigen. In Phase 3 können dann nach und nach weitere Daten geladen und die Platzhalter damit ersetzt werden. Weitere Informationen zur Verwendung von Daten finden Sie unter [Optimieren von ListView und GridView](optimize-gridview-and-listview.md).

Wie genau eine App auf die einzelnen Startphasen reagiert, liegt ganz bei Ihnen. Bedenken Sie allerdings, dass dem Benutzer durch möglichst viel Feedback (Begrüßungsbildschirm, Ladebildschirm und Verfügbarkeit der UI, während noch Daten geladen werden) der Eindruck einer schnellen App sowie eines insgesamt schnellen Systems vermittelt wird.

### Minimieren der verwalteten Assemblys im Startpfad

Wiederverwendbarer Code liegt oft in Gestalt von in das Projekt einbezogenen Modulen (DLL-Dateien) vor. Zum Laden dieser Module muss auf den Datenträger zugegriffen werden, was – wie Sie sich vorstellen können – schnell einen größeren Mehraufwand bedeuten kann. Dieser Aspekt wirkt sich am stärksten bei einem Kaltstart aus, kann aber auch Auswirkungen auf den Warmstart haben. Im Falle von C# und Visual Basic versucht die CLR, die Auswirkungen bestmöglich zu verzögern, indem sie die Assemblys nur bei Bedarf lädt. Mit anderen Worten: Die CLR lädt ein Modul erst, wenn in einer ausgeführten Methode darauf verwiesen wird. Verweisen Sie im Startcode also nur auf Assemblys, die für den Start Ihrer App erforderlich sind, damit die CLR keine überflüssigen Module lädt. Falls Ihr Startpfad nicht verwendete Codepfade mit unnötigen Verweisen enthält, können Sie diese Codepfade in andere Methoden auslagern, um unnötige Ladevorgänge zu vermeiden.

Eine weitere Möglichkeit zum Optimieren von Modulladevorgängen ist das Kombinieren von App-Modulen. Eine einzelne große Assembly wird in der Regel schneller geladen als zwei kleinere Assemblys. Dies ist allerdings nicht immer möglich. Außerdem sollten Sie Module nur dann kombinieren, wenn dieser Schritt keine großen Nachteile für die Entwicklerproduktivität oder die Wiederverwendbarkeit des Codes bedeutet. Welche Module beim Start geladen werden, können Sie mit Tools wie [PerfView](http://go.microsoft.com/fwlink/p/?linkid=251609) oder mit der [Windows-Leistungsanalyse](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/ff191077.aspx) ermitteln.

### Verwenden intelligenter Webanforderungen

Sie können die Ladezeit der App erheblich verbessern, wenn Sie ihren Inhalt (wie XAML, Bilder und andere wichtige App-Dateien) lokal packen. Datenträgervorgänge sind schneller als Netzwerkvorgänge. Falls eine App eine bestimmte Datei für die Initialisierung benötigt, können Sie die Startzeit insgesamt verringern, indem Sie die Datei vom Datenträger und nicht von einem Remoteserver laden.

## Effizienz beim Journaling und der Zwischenspeicherung von Seiten

Über das Frame-Steuerelement werden Navigationsfunktionen bereitgestellt. Dies umfasst das Navigieren auf eine Seite (Navigate-Methode), Navigationsjournaling (BackStack/ForwardStack-Eigenschaften, GoForward/GoBack-Methode), Zwischenspeichern von Seiten (Page.NavigationCacheMode) und Serialisierungsunterstützung (GetNavigationState-Methode).

Die Leistung, die für „Frame“ beachtet werden sollte, dreht sich vor allem um das Journaling und Zwischenspeichern von Seiten.

**Framejournaling**: Wenn Sie mit Frame.Navigate() zu einer Seite navigieren, wird der Frame.BackStack-Sammlung für die aktuelle Seite ein PageStackEntry-Element hinzugefügt. PageStackEntry ist relativ klein, aber es gibt keinen integrierten Grenzwert für die Größe der BackStack-Sammlung. Für Benutzer ist es potenziell möglich, eine Navigation in einer Schleife durchzuführen und diese Sammlung unendlich groß werden zu lassen.

Das PageStackEntry-Element enthält auch den Parameter, der an die Frame.Navigate()-Methode übergeben wurde. Es wird empfohlen, dass dieser Parameter einen primitiven, serialisierbaren Typ aufweist (z. B. „int“ oder „string“), damit die Frame.GetNavigationState()-Methode funktioniert. Mit diesem Parameter kann aber auch auf ein Objekt verwiesen werden, das mit einem erheblich umfangreicheren Arbeitssatz oder weiteren Ressourcen verbunden ist, sodass auch der Aufwand für jeden Eintrag im BackStack-Element deutlich ansteigt. Beispielsweise können Sie ein StorageFile-Element als Parameter verwenden, und dies bedeutet, dass das BackStack-Element eine unendliche Anzahl von Dateien geöffnet lässt.

Daher ist es ratsam, die Navigationsparameter klein zu halten und die Größe des BackStack-Elements zu beschränken. Das BackStack-Element ist ein standardmäßiger Vektor (IList in C#, Platform::Vector in C++/CX) und kann daher gekürzt werden, indem einfach Einträge entfernt werden.

**Zwischenspeichern von Seiten**: Wenn Sie mit der Frame.Navigate-Methode auf eine Seite navigieren, wird standardmäßig eine neue Instanz der Seite instanziiert. Wenn Sie mit Frame.GoBack dann wieder zurück auf die vorherige Seite navigieren, wird analog dazu eine neue Instanz der vorherigen Seite zugeordnet.

„Frame“ verfügt aber über die Möglichkeit einer optionalen Zwischenspeicherung von Seiten, mit der diese Instanziierungen vermieden werden können. Verwenden Sie die Page.NavigationCacheMode-Eigenschaft, um eine Seite in den Cache einzufügen. Wenn Sie diesen Modus auf „Required“ festlegen, wird das Zwischenspeichern der Seite erzwungen. Bei der Einstellung „Enabled“ ist die Zwischenspeicherung zulässig. Standardmäßig beträgt die Cachegröße zehn Seiten, aber dies kann mit der Frame.CacheSize-Eigenschaft überschrieben werden. Alle Seiten mit der Einstellung „Required“ werden zwischengespeichert, und wenn weniger Seiten als mit „CacheSize Required“ festgelegt vorhanden sind, können auch Seiten mit der Einstellung „Enabled“ zwischengespeichert werden.

Das Zwischenspeichern kann zu einer Verbesserung der Leistung beitragen, indem Instanziierungen vermieden werden. So wird die gesamte Navigationsleistung verbessert. Das Zwischenspeichern kann sich auch negativ auf die Leistung auswirken, indem zu viel zwischengespeichert und dadurch der Arbeitssatz beeinträchtigt wird.

Aus diesem Grund ist es zu empfehlen, das Zwischenspeichern von Seiten so einzusetzen, wie es für Ihre Anwendung am besten geeignet ist. Stellen Sie sich beispielsweise vor, dass Sie über eine App verfügen, bei der eine Liste mit Elementen in einem Frame angezeigt wird. Wenn Sie auf ein Element tippen, wird für den Frame die Navigation auf eine Detailseite des Elements durchgeführt. Für die Seite mit der Liste empfiehlt sich normalerweise eine Zwischenspeicherung. Falls die Detailseite für alle Elemente gleich ist, kann sie auch zwischengespeichert werden. Wenn die Detailseite eher abwechslungsreich gestaltet ist, kann es besser sein, sie nicht zwischenzuspeichern.



<!--HONumber=Mar16_HO1-->


