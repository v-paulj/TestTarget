---
author: mcleblanc
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: "Übersicht Datenbindung"
description: "In diesem Thema erfahren Sie, wie Sie in einer UWP-App (Universelle Windows-Plattform) ein Steuerelement (oder ein anderes Benutzeroberflächenelement) an ein einzelnes Element oder ein Elementsteuerelement an eine Sammlung von Elementen binden."
ms.sourcegitcommit: d76ef6a87d6afad577f5f7bf5e8f18a8b0776094
ms.openlocfilehash: c30e048f450c062c6e0148e5040a58bfa47193bb

---
Übersicht Datenbindung
=====================

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Thema erfahren Sie, wie Sie in einer UWP-App (Universelle Windows-Plattform) ein Steuerelement (oder ein anderes Benutzeroberflächenelement) an ein einzelnes Element oder ein Elementsteuerelement an eine Sammlung von Elementen binden. Darüber hinaus wird erläutert, wie Sie die Anzeige von Elementen steuern, eine Detailansicht auf Grundlage einer Auswahl implementieren und Daten für die Anzeige umwandeln. Ausführliche Informationen finden Sie unter [Datenbindung im Detail](data-binding-in-depth.md).

Voraussetzungen
-------------------------------------------------------------------------------------------------------------

In diesem Thema wird vorausgesetzt, dass Sie eine UWP-App erstellen können. Anweisungen zum Erstellen Ihrer ersten UWP-App finden Sie unter [Erstellen Ihrer ersten UWP-App mit C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/Hh974581).

Erstellen des Projekts
---------------------------------------------------------------------------------------------------------------------------------

Erstellen Sie ein neues Projekt vom Typ **Leere Anwendung (Windows Universal)**. Nennen Sie sie „Schnellstart“.

Binden an ein einzelnes Element
---------------------------------------------------------------------------------------------------------------------------------------------------------

Jede Bindung besteht aus einem Bindungsziel und einer Bindungsquelle. In der Regel ist das Ziel eine Eigenschaft eines Steuerelements oder anderen Benutzeroberflächenelements, und die Quelle ist eine Eigenschaft einer Klasseninstanz (ein Datenmodell oder ein Ansichtsmodell). In diesem Beispiel wird veranschaulicht, wie Sie ein Steuerelement an ein einzelnes Element binden. Das Ziel ist die **Text**-Eigenschaft eines **TextBlock**. Die Quelle ist eine Instanz einer einfachen Klasse namens **Recording**, die eine Audioaufnahme darstellt. Befassen wir uns zuerst mit der Klasse.

Fügen Sie Ihrem Projekt eine neue Klasse hinzu, nennen Sie sie "Recording.cs" (bei Verwendung von C#), und fügen Sie ihr diesen Code hinzu.

``` csharp
namespace Quickstart
{
    public class Recording
    {
        public string ArtistName { get; set; }
        public string CompositionName { get; set; }
        public DateTime ReleaseDateTime { get; set; }

        public Recording()
        {
            this.ArtistName = "Wolfgang Amadeus Mozart";
            this.CompositionName = "Andante in C for Piano";
            this.ReleaseDateTime = new DateTime(1761, 1, 1);
        }

        public string OneLineSummary
        {
            get
            {
                return $"{this.CompositionName} by {this.ArtistName}, released: "
                    + this.ReleaseDateTime.ToString("d");
            }
        }
    }

    public class RecordingViewModel
    {
        private Recording defaultRecording = new Recording();
        public Recording DefaultRecording { get { return this.defaultRecording; } }
    }
}
```

``` cpp
#include <sstream>

namespace Quickstart
{
    public ref class Recording sealed
    {
    private:
        Platform::String^ artistName;
        Platform::String^ compositionName;
        Windows::Globalization::Calendar^ releaseDateTime;

    public:
        Recording(Platform::String^ artistName, Platform::String^ compositionName,
            Windows::Globalization::Calendar^ releaseDateTime) :
            artistName{ artistName },
            compositionName{ compositionName },
            releaseDateTime{ releaseDateTime } {}

        property Platform::String^ ArtistName
        {
            Platform::String^ get() { return this->artistName; }
        }

        property Platform::String^ CompositionName
        {
            Platform::String^ get() { return this->compositionName; }
        }

        property Windows::Globalization::Calendar^ ReleaseDateTime
        {
            Windows::Globalization::Calendar^ get() { return this->releaseDateTime; }
        }

        property Platform::String^ OneLineSummary
        {
            Platform::String^ get()
            {
                std::wstringstream wstringstream;
                wstringstream << this->CompositionName->Data();
                wstringstream << L" by " << this->ArtistName->Data();
                wstringstream << L", released: " << this->ReleaseDateTime->MonthAsNumericString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->DayAsString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->YearAsString()->Data();
                return ref new Platform::String(wstringstream.str().c-str());
            }
        }
    };

    public ref class RecordingViewModel sealed
    {
    private:
        Recording^ defaultRecording;

    public:
        RecordingViewModel()
        {
            Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 1;
            releaseDateTime->Day = 1;
            releaseDateTime->Year = 1761;
            this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
        }

        property Recording^ DefaultRecording
        {
            Recording^ get() { return this->defaultRecording; };
        }
    };
}
```

Machen Sie als Nächstes die Bindungsquellklasse aus der Klasse verfügbar, die die Markupseite darstellt. Zu diesem Zweck fügen Sie eine Eigenschaft vom Typ **RecordingViewModel** zu **MainPage** hinzu.

``` csharp
namespace Quickstart
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new RecordingViewModel();
        }

        public RecordingViewModel ViewModel { get; set; }
    }
}
```

``` cpp
namespace Quickstart
{
    public ref class MainPage sealed
    {
    private:
        RecordingViewModel^ viewModel;

    public:
        MainPage()
        {
            InitializeComponent();
            this->viewModel = ref new RecordingViewModel();
        }

        property RecordingViewModel^ ViewModel
        {
            RecordingViewModel^ get() { return this->viewModel; };
        }
    };
}
```

Der letzte Codeteil ist das Binden eines **TextBlock** an die **ViewModel.DefaultRecording.OneLiner**-Eigenschaft.

``` xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="{x:Bind ViewModel.DefaultRecording.OneLineSummary}"
        HorizontalAlignment="Center"
        VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Dies ist das Ergebnis.

![Binden eines Textblocks](images/xaml-databinding0.png)

Binden an eine Sammlung von Elementen
------------------------------------------------------------------------------------------------------------------

Ein häufiges Szenario ist das Binden an eine Sammlung von Geschäftsobjekten. In C# und Visual Basic stellt die generische [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/ms668604.aspx)-Klasse eine gute Wahl für die Datenbindung bei Sammlungen dar, da sie die  [**INotifyPropertyChanged**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx)-Schnittstelle und die [**INotifyCollectionChanged**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx)-Schnittstelle implementiert. Diese Schnittstellen bieten eine Änderungsbenachrichtigung für Bindungen, wenn Elemente hinzugefügt oder entfernt werden oder eine Eigenschaft der Liste selbst geändert wird. Wenn Ihre gebundenen Steuerelemente bei Änderungen an Eigenschaften von Objekten in der Sammlung aktualisiert werden sollen, muss das Geschäftsobjekt auch **INotifyPropertyChanged** implementieren. Weitere Informationen finden Sie unter [Datenbindung im Detail](data-binding-in-depth.md).

In diesem nächsten Beispiel wird eine [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) an eine Sammlung von `Recording`-Objekten gebunden. Beginnen wir, indem wir die Sammlung zum Ansichtsmodell hinzufügen. Fügen Sie einfach diese neuen Member zur **RecordingViewModel**-Klasse hinzu.

``` csharp
    public class RecordingViewModel
    {
        ...
        
        private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
        public ObservableCollection<Recording> Recordings { get { return this.recordings; } }

        public RecordingViewModel()
        {
            this.recordings.Add(new Recording() { ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
            this.recordings.Add(new Recording() { ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
            this.recordings.Add(new Recording() { ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
        }
    }
```

``` cpp
    public ref class RecordingViewModel sealed
    {
    private:
        ...
        Windows::Foundation::Collections::IVector<Recording^>^ recordings;

    public:
        RecordingViewModel()
        {
            ...

            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 7;
            releaseDateTime->Day = 8;
            releaseDateTime->Year = 1748;
            Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
            this->Recordings->Append(recording);

            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 2;
            releaseDateTime->Day = 11;
            releaseDateTime->Year = 1805;
            recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
            this->Recordings->Append(recording);

            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 12;
            releaseDateTime->Day = 3;
            releaseDateTime->Year = 1737;
            recording = ref new Recording{ L"George Frideric Handel", L"Serse", releaseDateTime };
            this->Recordings->Append(recording);
        }

        ...

        property Windows::Foundation::Collections::IVector<Recording^>^ Recordings
        {
            Windows::Foundation::Collections::IVector<Recording^>^ get()
            {
                if (this->recordings == nullptr)
                {
                    this->recordings = ref new Platform::Collections::Vector<Recording^>();
                }
                return this->recordings;
            };
        }
    };
    ```

And then bind a [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) to the **ViewModel.Recordings** property.

``` xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Wir haben noch keine Datenvorlage für die **Recording**-Klasse bereitgestellt. Daher kann das Benutzeroberflächenframework nur [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) für jedes Element in der [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) aufrufen. Die Standardimplementierung von **ToString** ist die Rückgabe des Typnamens.

![Binden einer Listenansicht](images/xaml-databinding1.png)

Um dies zu beheben, können wir entweder [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) so überschreiben, dass der Wert von **OneLineSummary** zurückgegeben wird, oder wir können eine Datenvorlage bereitstellen. Die Datenvorlagenoption wird häufiger verwendet ist auch deutlich flexibler. Sie legen eine Datenvorlage mithilfe der [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209369)-Eigenschaft eines Inhaltssteuerelements oder mit der [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242830)-Eigenschaft eines Elementsteuerelements fest. Nachfolgend sind zwei Möglichkeiten zum Entwerfen einer Datenvorlage für **Recording** dargestellt, zusammen mit einer Abbildung des Ergebnisses.

``` xml
    <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Recording">
                <TextBlock Text="{x:Bind OneLineSummary}"/>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    ```

![Binding a list view](images/xaml-databinding2.png)

``` xml
    <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
    HorizontalAlignment="Center" VerticalAlignment="Center">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Recording">
                <StackPanel Orientation="Horizontal" Margin="6">
                    <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                    <StackPanel>
                        <TextBlock Text="{x:Bind ArtistName}" FontWeight="Bold"/>
                        <TextBlock Text="{x:Bind CompositionName}"/>
                    </StackPanel>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
```

![Binden einer Listenansicht](images/xaml-databinding3.png)

Weitere Informationen zur XAML-Syntax finden Sie unter [Erstellen einer Benutzeroberfläche mit XAML](https://msdn.microsoft.com/library/windows/apps/Mt228349). Weitere Informationen zum Steuerelementlayout finden Sie unter [Definieren von Layouts mit XAML](https://msdn.microsoft.com/library/windows/apps/Mt228350).

Hinzufügen einer Detailansicht
-----------------------------------------------------------------------------------------------------

Sie können auch alle Details der **Recording**-Objekte in [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)-Elementen anzeigen. Dies nimmt jedoch sehr viel Platz in Anspruch. Stattdessen können Sie gerade so viele Daten im Element anzeigen, um es zu identifizieren, und wenn der Benutzer dann eine Auswahl vornimmt, können Sie alle Details des ausgewählten Elements in einem separaten Teil der Benutzeroberfläche anzeigen, der als „Detailansicht“ bezeichnet wird. Diese Anordnung wird auch als „Haupt-/Detailansicht“ oder „Listen-/Detailansicht“ bezeichnet.

Sie haben zwei Möglichkeiten, dieses Verhalten zu implementieren. Sie können die Detailansicht an die [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770)-Eigenschaft der [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) binden. Alternativ können Sie eine [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) verwenden: Binden Sie sowohl die **ListView** als auch die Detailansicht an die **CollectionViewSource** (dies behandelt das derzeit ausgewählte Elements für Sie). Die beiden Methoden sind unten aufgeführt und haben dieselben Ergebnisse, wie in der Abbildung dargestellt.

**Hinweis:**  Bisher haben wir in diesem Thema nur die [{x:Bind}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204783) verwendet. Für die beiden weiter unten aufgeführten Methoden wird jedoch die flexiblere (jedoch weniger leistungsfähige) [{Binding}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204782) benötigt.

Sehen wir uns zuerst die [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770)-Methode an. Bei Verwendung von Komponentenerweiterungen für Visual C++ (C++/CX) muss dann – da wir [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) verwenden – das [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872)-Attribut zur **Recording**-Klasse hinzugefügt werden.

``` cpp
    [Windows::UI::Xaml::Data::Bindable]
    public ref class Recording sealed
    {
        ...
    };
```

Darüber hinaus muss nur noch eine Änderung am Markup vorgenommen werden.

``` xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ListView x:Name="recordingsListView" ItemsSource="{x:Bind ViewModel.Recordings}">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="local:Recording">
                        <StackPanel Orientation="Horizontal" Margin="6">
                            <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                            <StackPanel>
                                <TextBlock Text="{x:Bind CompositionName}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
            <StackPanel DataContext="{Binding SelectedItem, ElementName=recordingsListView}"
            Margin="0,24,0,0">
                <TextBlock Text="{Binding ArtistName}"/>
                <TextBlock Text="{Binding CompositionName}"/>
                <TextBlock Text="{Binding ReleaseDateTime}"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Page>
```

Fügen Sie für die [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)-Methode zuerst eine **CollectionViewSource** als Seitenressource hinzu.

``` xml
    <Page.Resources>
        <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
    </Page.Resources>
```

Passen Sie dann die Bindungen in der [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) (die nicht mehr benannt werden muss) und in der Detailansicht so an, dass die [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) verwendet wird. Beachten Sie, dass Sie durch die direkte Bindung der Detailansicht an die **CollectionViewSource** implizieren, dass Sie in Bindungen, in denen der Pfad in der Sammlung selbst nicht gefunden werden kann, an das aktuelle Element binden möchten. Die **CurrentItem**-Eigenschaft muss nicht als Pfad für die Bindung angegeben werden, obwohl dies im Zweifelsfall möglich ist.

``` xml
    ...

    <ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">

    ...

    <StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
    ...
```

Nachfolgend ist das identische Ergebnis für die beiden Methoden dargestellt.

![Binden einer Listenansicht](images/xaml-databinding4.png)

Formatieren oder Konvertieren von Datenwerten für die Anzeige
--------------------------------------------------------------------------------------------------------------------------------------------

Es gibt ein kleines Problem mit dem oben gezeigten Rendering. Die **ReleaseDateTime**-Eigenschaft ist nicht nur ein Datum, sondern liegt als [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx) vor und wird daher mit einer Genauigkeit angezeigt, die nicht notwendig ist. Eine Lösung besteht darin, eine Zeichenfolgeneigenschaft zur **Recording**-Klasse hinzuzufügen, die `this.ReleaseDateTime.ToString("d")` zurückgibt. Indem wir diese Eigenschaft **ReleaseDate** nennen, geben wir an, dass sie ein Datum und nicht das Datum und die Uhrzeit zurückgibt. Die Benennung als **ReleaseDateAsString** gibt dann auch noch an, dass sie eine Zeichenfolge zurückgibt.

Eine flexiblere Lösung ist die Verwendung eines so genannten „Wertkonverters“. Nachfolgend ist ein Beispiel zum Erstellen Ihres eigenen Wertkonverter aufgeführt. Fügen Sie diesen Code zu Ihrer „Recording.cs“-Quellcodedatei hinzu.

``` csharp
public class StringFormatter : Windows.UI.Xaml.Data.IValueConverter
{
    // This converts the value object to the string to display.
    // This will work with most simple types.
    public object Convert(object value, Type targetType,
        object parameter, string language)
    {
        // Retrieve the format string and use it to format the value.
        string formatString = parameter as string;
        if (!string.IsNullOrEmpty(formatString))
        {
            return string.Format(formatString, value);
        }

        // If the format string is null or empty, simply
        // call ToString() on the value.
        return value.ToString();
    }

    // No need to implement converting back on a one-way binding
    public object ConvertBack(object value, Type targetType,
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

Jetzt können wir eine Instanz von **StringFormatter** als Seitenressource hinzufügen und in der Bindung verwenden. Wir übergeben die Formatzeichenfolge in den Konverter aus dem Markup, um eine möglichst hohe Flexibilität bei der Formatierung zu erhalten.

``` xml
    <Page.Resources>
        <local:StringFormatter x:Key="StringFormatterValueConverter"/>
    </Page.Resources>
    ...

    <TextBlock Text="{Binding ReleaseDateTime,
        Converter={StaticResource StringFormatterValueConverter},
        ConverterParameter=Released: \{0:d\}}"/>

    ...
```

Dies ist das Ergebnis.

![Anzeigen eines Datums mit benutzerdefinierter Formatierung](images/xaml-databinding5.png)





<!--HONumber=Jun16_HO3-->


