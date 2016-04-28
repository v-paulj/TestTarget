---
ms.assetid: 089660A2-7CAE-4911-9994-F619C5D22287
title: für die Entwurfsoberfläche und Prototyperstellung
description: Möglicherweise ist es nicht möglich oder nicht erwünscht (z. B. aus Gründen des Datenschutzes oder der Leistung), dass Ihre App Livedaten auf der Entwurfsoberfläche von Microsoft Visual Studio oder Blend für Visual Studio anzeigt.
---
Beispieldaten für die Entwurfsoberfläche und Prototyperstellung
=============================================================================================

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Hinweis**  Der Bedarf an Beispieldaten und deren Nutzen für Sie hängen davon ab, ob für die Bindungen die [{Binding}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204782) oder die [{x:Bind}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204783) verwendet wird. Die in diesem Thema beschriebenen Verfahren basieren auf der Verwendung eines [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) und eignen sich deshalb nur für **{Binding}**. Wenn Sie jedoch **{x:Bind}** verwenden, zeigen die Bindungen zumindest Platzhalterwerte auf der Entwurfsoberfläche an (selbst für Elementsteuerelemente). Deshalb besteht ein geringerer Bedarf an Beispieldaten.

Möglicherweise ist es nicht möglich oder nicht erwünscht (z. B. aus Gründen des Datenschutzes oder der Leistung), dass Ihre App Livedaten auf der Entwurfsoberfläche von Microsoft Visual Studio oder Blend für Visual Studio anzeigt. Es gibt mehrere Möglichkeiten, Entwurfszeit-Beispieldaten zu verwenden, damit die Steuerelemente mit Daten aufgefüllt werden (sodass Sie das Layout, die Vorlagen und andere visuelle Eigenschaften der App bearbeiten können). Beispieldaten können auch hilfreich sein und Zeit sparen, wenn Sie eine App als Skizze (oder Prototyp) erstellen. Sie können zur Laufzeit Beispieldaten in der Skizze oder im Prototyp verwenden, um Ihre Ideen zu veranschaulichen, ohne echte Livedaten nutzen zu müssen.

Festlegen des DataContext im Markup
-----------------------------

Entwickler verwenden häufig imperativen Code (in CodeBehind), um den [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) einer Seite oder eines Benutzersteuerelements auf eine Ansichtsmodellinstanz festzulegen.

``` csharp
public MainPage()
{
    InitializeComponent();
    this.DataContext = new BookstoreViewModel();
}
```

Jedoch stehen Ihnen bei Verwendung dieses Verfahrens weniger Designmöglichkeiten für die Seite zur Verfügung. Der Grund dafür ist, dass beim Öffnen der XAML-Seite in Visual Studio oder Blend für Visual Studio der imperative Code, der den **DataContext**-Wert zuweist, nie ausgeführt wird (es wird nicht einmal der CodeBehind ausgeführt). Selbstverständlich wird von den XAML-Tools das Markup analysiert, und sie instanziieren alle in ihm deklarierten Objekte, sie instanziieren jedoch nicht den Typ der Seite. Deshalb werden in den Steuerelementen und im Dialogfeld **Datenbindung erstellen** keine Daten angezeigt, und es ist schwieriger, das Format und Layout der Seite zu erstellen.

![Benutzeroberfläche mit geringen Entwurfsmöglichkeiten](images/displaying-data-in-the-designer-01.png)

Als Abhilfe können Sie die **DataContext**-Zuweisung auskommentieren und den **DataContext** stattdessen im Seitenmarkup festlegen. Dies bewirkt, dass die Livedaten zur Entwurfszeit und zur Laufzeit angezeigt werden. Öffnen Sie hierzu zunächst die XAML-Seite. Klicken Sie dann im Fenster **Dokumentgliederung** auf das Stammentwurfselement (in der Regel mit der Bezeichnung **\[Page\]**), um es auszuwählen. Suchen Sie im **Eigenschaftenfenster** die **DataContext**-Eigenschaft (in der Kategorie „Allgemein“), und klicken Sie dann auf **Neu**. Klicken Sie im Dialogfeld **Objekt auswählen** auf den Ansichtsmodelltyp, und klicken Sie dann auf **OK**.

![Benutzeroberfläche zum Festlegen des „DataContext“](images/displaying-data-in-the-designer-02.png)

Das resultierende Markup sieht folgendermaßen aus.

``` xml
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>
```

Und so sieht die Benutzeroberfläche aus, wenn die Bindungen aufgelöst werden können. Beachten Sie, dass das Auswahlfeld **Pfad** im Dialogfeld **Datenbindung erstellen** jetzt aufgefüllt ist, basierend auf dem **DataContext**-Typ und den Eigenschaften, an die gebunden werden kann.

![Designfähige Benutzeroberfläche](images/displaying-data-in-the-designer-03.png)

Im Dialogfeld **Datenbindung erstellen** ist nur ein Typ als Grundlage erforderlich, die Bindungen erfordern jedoch, dass die Eigenschaften mit Werten initialisiert werden. Wenn Sie zur Entwurfszeit nicht den Clouddienst in Anspruch nehmen möchten (aus Gründen der Leistung, der Kosten der Datenübertragung, des Datenschutzes usw.), kann der Initialisierungscode überprüfen, ob die App in einem Entwicklungstool (z. B. Visual Studio oder Blend für Visual Studio) ausgeführt wird. Wenn dies der Fall ist, laden Sie Beispieldaten, die nur zur Entwurfszeit verwendet werden.

``` csharp
if (Windows.ApplicationModel.DesignMode.DesignModeEnabled)
{
    // Load design-time books.
}
else
{
    // Load books from a cloud service.
}
```

Sie können einen Ansichtsmodelllocator verwenden, wenn Sie Parameter an den Initialisierungscode übergeben müssen. Ein Ansichtsmodelllocator ist eine Klasse, die Sie in App-Ressourcen einfügen können. Er verfügt über eine Eigenschaft, die das Ansichtsmodell verfügbar macht, und der **DataContext** der Seite wird an diese Eigenschaft gebunden. Ein weiteres Muster, das der Locator oder das Ansichtsmodell verwenden können, ist das Einfügen von Abhängigkeiten. Damit kann ein Entwurfszeit- bzw. Laufzeitdatenanbieter (der jeweils eine gemeinsame Schnittstelle implementiert) erstellt werden.

„Beispieldaten aus Klasse erstellen“ und Entwurfszeitattribute
---------------------------------------------------------------------------------------

Wenn aus irgendeinem Grund keine der Optionen im vorherigen Abschnitt für Sie geeignet ist, stehen Ihnen immer noch über die Features der XAML-Tools und die Entwurfszeitattribute viele Optionen für Entwurfszeitdaten zur Verfügung. Eine gute Möglichkeit ist das Feature **Beispieldaten aus Klasse erstellen** in Blend für Visual Studio. Sie finden diesen Befehl auf einer der Schaltflächen am oberen Rand des Bereichs **Daten**.

Sie müssen lediglich eine Klasse für den zu verwendenden Befehl angeben. Der Befehl führt dann zwei wichtige Dinge für Sie aus. Erstens – er generiert eine XAML-Datei, die geeignete Beispieldaten zur rekursiven Hydration einer Instanz der ausgewählten Klasse und aller ihrer Member enthält (das Tool eignet sich für XAML-Dateien und JSON-Dateien gleichermaßen). Zweitens füllt er den Bereich **Daten** mit dem Schema der ausgewählten Klasse auf. Sie können dann Mitglieder aus dem Bereich **Daten** auf die Entwurfsoberfläche ziehen, um verschiedene Aufgaben auszuführen. Je nachdem, was Sie ziehen und wo Sie es ablegen, können Sie vorhandenen Steuerelementen Bindungen hinzufügen (mit **{Binding}**) oder neue Steuerelemente erstellen und gleichzeitig binden. In beiden Fällen wird außerdem automatisch im Layoutstamm der Seite ein Entwurfszeit-Datenkontext (**d:DataContext**) für Sie festgelegt (wenn dieser nicht bereits festgelegt ist). Der Entwurfszeit-Datenkontext verwendet das **d:DesignData**-Attribut zum Abrufen der Beispieldaten aus der generierten XAML-Datei (die Sie übrigens im Projekt suchen und bearbeiten können, damit sie die gewünschten Beispieldaten enthält).

``` xml
<Page ...
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Grid ... d:DataContext="{d:DesignData /SampleData/RecordingViewModelSampleData.xaml}"/>
        <ListView ItemsSource="{Binding Recordings}" ... />
        ...
    </Grid>
</Page>
```

Die verschiedenen xmlns-Deklarationen bedeuten, dass Attribute mit dem **d:**-Präfix nur zur Entwurfszeit interpretiert und während der Laufzeit ignoriert werden. Somit wirkt sich das **d:DataContext**-Attribut nur zur Entwurfszeit auf den Wert der Eigenschaft [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) aus und hat zur Laufzeit keine Auswirkungen. Sie können im Markup sogar sowohl **d:DataContext** als auch **DataContext** festlegen, wenn Sie dies wünschen. **d:DataContext** überschreibt zur Entwurfszeit, und **DataContext** und überschreibt zur Laufzeit. Diese Überschreibungsregeln werden auf alle Entwurfszeit- und Laufzeitattribute angewendet.

Das **d:DataContext**-Attribut und alle anderen Entwurfszeitattribute sind im Thema [Designzeitattribute](http://go.microsoft.com/fwlink/p/?LinkId=272504) dokumentiert, das für Universelle Windows-Plattform (UWP)-Apps) weiterhin Gültigkeit hat.

[**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) besitzt nicht die Eigenschaft **DataContext**, jedoch die Eigenschaft **Quelle**. Daher gibt es eine Eigenschaft **d:Source**, mit der Sie auf die Entwurfszeit beschränkte Beispieldaten in einer **CollectionViewSource** festlegen können.

``` xml
    <Page.Resources>
        <CollectionViewSource x:Name="RecordingsCollection" Source="{Binding Recordings}"
            d:Source="{d:DesignData /SampleData/RecordingsSampleData.xaml}"/>
    </Page.Resources>

    ...

        <ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}" ... />
    ...
```

Damit dies funktioniert, verwenden Sie eine Klasse mit dem Namen `Recordings : ObservableCollection<Recording>` und bearbeiten die XAML-Beispieldatendatei, damit sie nur ein **Recordings**-Objekt enthält (in dem sich **Recording**-Objekte befinden), wie hier dargestellt.

``` xml
<Quickstart:Recordings xmlns:Quickstart="using:Quickstart">
    <Quickstart:Recording ArtistName="Mollis massa" CompositionName="Cubilia metus"
        OneLineSummary="Morbi adipiscing sed" ReleaseDateTime="01/01/1800 15:53:17"/>
    <Quickstart:Recording ArtistName="Vulputate nunc" CompositionName="Parturient vestibulum"
        OneLineSummary="Dapibus praesent netus amet vestibulum" ReleaseDateTime="01/01/1800 15:53:17"/>
    <Quickstart:Recording ArtistName="Phasellus accumsan" CompositionName="Sit bibendum"
        OneLineSummary="Vestibulum egestas montes dictumst" ReleaseDateTime="01/01/1800 15:53:17"/>
</Quickstart:Recordings>
```

Wenn Sie eine JSON-Beispieldatendatei anstelle einer XAML-Beispieldatendatei verwenden, müssen Sie die **Type**-Eigenschaft festlegen.

``` xml
    d:Source="{d:DesignData /SampleData/RecordingsSampleData.json, Type=local:Recordings}"
```

Bisher haben wir zum Laden von Entwurfszeit-Beispieldaten aus einer XAML- oder JSON-Datei **d:DesignData** verwendet. Alternativ kann die Markuperweiterung **d:DesignInstance** verwendet werden. Diese gibt an, dass die Entwurfszeitquelle auf der von der **Type**-Eigenschaft angegebenen Klasse basiert. Im Folgenden finden Sie ein Beispiel hierfür.

``` xml
    <CollectionViewSource x:Name="RecordingsCollection" Source="{Binding Recordings}"
        d:Source="{d:DesignInstance Type=local:Recordings, IsDesignTimeCreatable=True}"/>
        ```

The **IsDesignTimeCreatable** property indicates that the design tool should actually create an instance of the class, which implies that the class has a public default constructor, and that it populates itself with data (either real or sample). If you don't set **IsDesignTimeCreatable** (or if you set it to **False**) then you won't get sample data displayed on the design surface. All the design tool does in that case is to parse the class for its bindable properties and display these in the the **Data** panel and in the **Create Data Binding** dialog.

Sample data for prototyping
--------------------------------------------------------

For prototyping, you want sample data at both design-time and at run-time. For that use case, Blend for Visual Studio has the **New Sample Data** feature. You can find that command on one of the buttons at the top of the **Data** panel.

Instead of specifying a class, you can actually design the schema of your sample data source right in the **Data** panel. You can also edit sample data values in the **Data** panel: there's no need to open and edit a file (although, you can still do that if you prefer).

The **New Sample Data** feature uses [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), and not **d:DataContext**, so that the sample data is available when you run your sketch or prototype as well as while you're designing it. And the **Data** panel really speeds up your designing and binding tasks. For example, simply dragging a collection property from the **Data** panel onto the design surface generates a data-bound items control and the necessary templates, all ready to build and run.

![Sample data for prototyping.](images/displaying-data-in-the-designer-04.png)

 

 






<!--HONumber=Mar16_HO1-->


