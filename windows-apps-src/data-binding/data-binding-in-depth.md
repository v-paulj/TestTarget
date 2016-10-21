---
author: mcleblanc
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: Datenbindung im Detail
description: "Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann."
translationtype: Human Translation
ms.sourcegitcommit: ef5e2819a7fd18fb3ed3162fd8debe750f29c378
ms.openlocfilehash: fa1616c88d475393311055561a7bd219c2373f76

---
# Datenbindung im Detail

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**Binding-Klasse**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

> **Hinweis**&nbsp;&nbsp;In diesem Thema werden die Datenbindungsfeatures ausführlich beschrieben. Eine kurze und praktische Einführung finden Sie unter [Übersicht „Datenbindung“](data-binding-quickstart.md).


Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann. Mit der Datenbindung können Sie Datenaspekte von Benutzeroberflächenaspekten trennen, was zu einem einfacheren konzeptionellen Modell und besserer Lesbarkeit, Testbarkeit und Wartung Ihrer App führt.

Sie können die Datenbindung einfach verwenden, um nur Werte aus einer Datenquelle anzuzeigen, wenn die Benutzeroberfläche das erste Mal angezeigt wird, jedoch nicht auf Änderungen an diesen Werten reagieren. Dies wird als „einmalige Bindung“ bezeichnet und funktioniert gut für Daten, deren Werte während der Laufzeit nicht geändert werden. Darüber hinaus können Sie die Werte auch „feststellen“ und die Benutzeroberfläche bei einer Änderung aktualisieren. Dies wird als „unidirektionale Bindung“ bezeichnet und eignet sich gut für schreibgeschützte Daten. Letztendlich können Sie die Daten sowohl feststellen als auch aktualisieren, damit die vom Benutzer in der Benutzeroberfläche vorgenommenen Änderungen automatisch in die Datenquelle übernommen werden. Dies wird als „bidirektionale Bindung“ bezeichnet und eignet sich gut für Daten mit Lese-/Schreibberechtigung. Beispiele:

-   Sie können die einmalige Bindung verwenden, um ein [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) an das Foto des aktuellen Benutzers zu binden.
-   Sie können die unidirektionale Bindung verwenden, um ein [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)-Objekt an eine Sammlung von Nachrichtenartikeln in Echtzeit nach Zeitungsteil gruppiert zu binden.
-   Sie könnten eine bidirektionale Bindung verwenden, um ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Objekt an den Namen eines Kunden in einem Formular zu binden.

Es gibt zwei Arten von Bindungen. Diese werden in der Regel beide im Benutzeroberflächenmarkup deklariert. Sie können entweder die [{x:Bind}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204783) oder die [{Binding}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204782) verwenden. Sie können sogar eine Mischung aus den beiden in derselben App verwenden – sogar im gleichen Benutzeroberflächenelement. {x:Bind} ist neu in Windows10 und bietet eine bessere Leistung. Alle in diesem Thema beschriebenen Details gelten für beide Arten von Bindungen, es sei denn, es wird ausdrücklich auf eine Abweichung hingewiesen.

**Beispiel-Apps zur Veranschaulichung von {x:Bind}**

-   [{x:Bind}-Beispiel](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame).
-   [Beispiel für XAML-Benutzeroberflächengrundlagen](http://go.microsoft.com/fwlink/p/?linkid=619992).

**Beispiel-Apps zur Veranschaulichung von {Binding}**

-   Laden Sie die App [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950) herunter.
-   Laden Sie die App [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) herunter.

## Was zu jeder Bindung gehört

-   Eine *Bindungsquelle*: Dies ist die Quelle der Daten für die Bindung und kann eine Instanz einer Klasse mit Membern sein, deren Werte Sie in der Benutzeroberfläche anzeigen möchten.
-   Ein *Bindungsziel*: Dies ist eine [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) des [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) in Ihrer Benutzeroberfläche, die die Daten anzeigt.
-   Ein *Bindungsobjekt*: Dies ist der Teil, der Datenwerte von der Quelle an das Ziel sowie optional vom Ziel zurück an die Quelle überträgt. Das Bindungsobjekt wird zur XAML-Ladezeit von der [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)- oder [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)-Markuperweiterung erstellt.

In den folgenden Abschnitten betrachten wird die Bindungsquelle, das Bindungsziel und das Bindungsobjekt genauer. Außerdem verknüpfen wir die Abschnitte durch ein Beispiel, bei dem der Inhalt einer Schaltfläche an eine Zeichenfolgeneigenschaft mit dem Namen **NextButtonText** gebunden wird, die zur Klasse **HostViewModel** gehört.

### Bindungsquelle

Nachfolgend wird eine sehr einfache Implementierung einer Klasse gezeigt, die wir als Bindungsquelle verwenden könnten.

**Hinweis** Bei Verwendung von [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) mit Komponentenerweiterungen für Visual C++ (C++/CX) müssen Sie das [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872)-Attribut zur Bindungsquellklasse hinzufügen. Bei Verwendung von [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ist dieses Attribut nicht erforderlich. Unter [Hinzufügen einer Detailansicht](data-binding-quickstart.md#adding-a-details-view) finden Sie einen Codeausschnitt.

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

Diese Implementierung der **HostViewModel**-Klasse und der zugehörigen **NextButtonText**-Eigenschaft ist nur für eine einmalige Bindung geeignet. Unidirektionale und bidirektionale Bindungen sind jedoch sehr häufig, und bei diesen Arten von Bindungen wird die Benutzeroberfläche als Reaktion auf Änderungen bei den Datenwerten der Bindungsquelle automatisch aktualisiert. Damit diese Bindungen ordnungsgemäß funktionieren, muss die Bindungsquelle für das Bindungsobjekt „feststellbar“ sein. Wenn wir in unserem Beispiel also eine unidirektionale oder bidirektionale Bindung an die **NextButtonText**-Eigenschaft anwenden möchten, müssen alle Änderungen, die zur Laufzeit am Wert dieser Eigenschaft vorgenommen werden, für das Bindungsobjekt feststellbar sein.

Eine Möglichkeit für die Umsetzung besteht darin, die Klasse, die die Bindungsquelle darstellt, von [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356) abzuleiten und einen Datenwert über eine [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) verfügbar zu machen. Auf diese Weise wird ein [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) feststellbar. **FrameworkElements** sind gute Bindungsquellen, die sofort verwendet werden können.

Eine einfachere Möglichkeit, eine Klasse feststellbar zu machen – und eine Notwendigkeit für Klassen, die bereits über eine Basisklasse verfügen – besteht in der Implementierung von [**System.ComponentModel.INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx). Hierzu muss lediglich ein einzelnes Ereignis mit dem Namen **PropertyChanged** implementiert werden. Weiter unten finden Sie ein Beispiel für die Verwendung von **HostViewModel**.

**Hinweis** Im Fall von C++/CX implementieren Sie [**Windows::UI::Xaml::Data::INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899). Die Bindungsquellenklasse muss entweder über das [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) verfügen oder [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) implementieren.

```csharp
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Die **NextButtonText**-Eigenschaft ist jetzt feststellbar. Wenn Sie eine uni- oder bidirektionale Bindung an diese Eigenschaft erstellen (was weiter unten genauer gezeigt wird), abonniert das sich ergebende Bindungsobjekt das **PropertyChanged**-Ereignis. Wenn das Ereignis ausgelöst wird, erhält der Handler des Bindungsobjekts ein Argument mit dem Namen der Eigenschaft, die geändert wurde. Daher weiß das Bindungsobjekt, welchen Eigenschaftswert es erneut lesen muss.

Damit Sie das oben gezeigte Muster nicht mehrmals implementieren müssen, können Sie einfach eine Ableitung von der **BindableBase**-Basisklasse vornehmen, die im [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)-Beispiel (im Ordner „Common“) enthalten ist. Dies ist ein Beispiel hierfür.

```csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

Das Auslösen des **PropertyChanged**-Ereignisses mit dem Argument [**String.Empty**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.empty.aspx) oder **null** gibt an, dass alle Eigenschaften, die keine Indexereigenschaften sind, für das Objekt erneut gelesen werden sollen. Sie können das Ereignis auslösen, um anzugeben, dass die Indexereigenschaften für das Objekt geändert wurden, indem Sie ein Argument von „Item [*indexer*\]“ für bestimmte Indexer (mit *indexer* als Indexwert) oder einen Wert von „Item\[\]“ für alle Indexer verwenden.

Eine Bindungsquelle kann entweder als einzelnes Objekt behandelt werden, dessen Eigenschaften Daten enthalten, oder als Sammlung von Objekten. In C#- und Visual Basic-Code können Sie eine einmalige Bindung an ein Objekt vornehmen, das [**List(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx) zum Anzeigen einer Sammlung implementiert, die nicht zur Laufzeit geändert wird. Nehmen Sie für eine feststellbare Sammlung (es wird festgestellt, wann Elemente zur Sammlung hinzugefügt oder aus dieser entfernt werden) stattdessen eine unidirektionale Bindung an [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) vor. In C++-Code können Sie sowohl für feststellbare als auch für nicht feststellbare Sammlungen eine Bindung an [**Vector&lt;T&gt;**](https://msdn.microsoft.com/library/dn858385.aspx) vornehmen. Wenn Sie eine Bindung an Ihre eigenen Sammlungsklassen vornehmen möchten, verwenden Sie die Anleitung in der folgenden Tabelle.

| Szenario                                                        | C# und VB (CLR)                                                                                                                                                                                                                                                                                                                                                                                                                   | C++/CX                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| An ein Objekt binden                                              | Es kann sich um jedes Objekt handeln.                                                                                                                                                                                                                                                                                                                                                                                                                 | Das Objekt muss [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) besitzen oder [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) implementieren.                                                                                                                                                                                                                                                                                                             |
| Abrufen von Eigenschaftenänderungen eines gebundenen Objekts.                | Das Objekt muss [**System.ComponentModel. INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx) implementieren.                                                                                                                                                                                                                                                                                                         | Das Objekt muss [**Windows.UI.Xaml.Data. INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899) implementieren.                                                                                                                                                                                                                                                                                                                                                           |
| An eine Sammlung binden                                           | [**List(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx)                                                                                                                                                                                                                                                                                                                                                                            | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Abrufen von Sammlungsänderungen einer gebundenen Sammlung          | [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx)                                                                                                                                                                                                                                                                                                                                        | [**Windows::Foundation::Collections::IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/br226052.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Implementieren einer Sammlung, die Bindungen unterstützt                   | Erweitern von [**List(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx) oder Implementieren von [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx), [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/5y536ey6.aspx)(Of [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx)), [**IEnumerable**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ienumerable.aspx) oder [**IEnumerable**](https://msdn.microsoft.com/library/windows/apps/xaml/9eekhta0.aspx)(Of **Object**). Die Bindung an generische **IList(Of T)** und **IEnumerable(Of T)** wird nicht unterstützt. | Implementieren Sie [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957), [**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx)^&gt;, [**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; oder **IIterable**&lt;**IInspectable**\*&gt;. Die Bindung an generische **IVector&lt;T&gt;** und **IIterable&lt;T&gt;** wird nicht unterstützt. |
| Implementieren einer Sammlung, die Änderungen an der Sammlung unterstützt | Erweitern Sie [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx), oder implementieren Sie (nicht generische) [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) und [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx).                                                                                                                                                               | Implementieren Sie [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) und [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974).                                                                                                                                                                                                                                                                                                                       |
| Implementieren einer Sammlung, die inkrementelles Laden unterstützt       | Erweitern Sie [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx), oder implementieren Sie (nicht generische) [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) und [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx). Implementieren Sie zusätzlich [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                          | Implementieren Sie [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) und [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                                                                                                                                                                                                         |

Sie können mithilfe von inkrementellem Laden Listensteuerelemente an beliebig große Datenquellen binden und dennoch eine hohe Leistung erreichen. Zum Beispiel können Sie Listensteuerelemente an Ergebnisse von Bildabfragen in Bing binden, ohne alle Ergebnisse auf einmal laden zu müssen. Stattdessen laden Sie nur einige der Ergebnisse sofort und weitere Ergebnisse nach Bedarf. Sie müssen [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916) für eine Datenquelle umsetzen, die die Sammlungsänderungsbenachrichtigung unterstützt, um das inkrementelle Laden zu unterstützen. Wenn das Datenbindungsmodul weitere Daten anfordert, muss Ihre Datenquelle die entsprechenden Anforderungen senden, die Ergebnisse integrieren und anschließend die entsprechende Benachrichtigung senden, um die UI zu aktualisieren.

### Bindungsziel

In den folgenden beiden Beispielen ist die **Button.Content**-Eigenschaft das Bindungsziel. Ihr Wert ist auf eine Markuperweiterung festgelegt, die das Bindungsobjekt deklariert. Es wird zuerst [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) angezeigt, dann [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Das Deklarieren von Bindungen im Markup wird häufig verwendet (es ist praktisch, lesbar und toolfreundlich). Sie können Markups jedoch auch vermeiden und eine Instanz der [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820)-Klasse stattdessen auch bei Bedarf imperativ (programmgesteuert) erstellen.

<!-- XAML lang specifier not yet supported in OP. Using XML for now. -->
```xml
<Button Content="{x:Bind ...}" ... />
```

```xml
<Button Content="{Binding ...}" ... />
```

### Mit {x:Bind} deklariertes Bindungsobjekt

Vor dem Erstellen des [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)-Markups müssen wir jedoch noch einen Schritt durchführen. Wir müssen die Bindungsquellenklasse aus der Klasse verfügbar machen, die die Markupseite darstellt. Hierzu fügen wir eine Eigenschaft (in diesem Fall des Typs **HostViewModel**) zur **HostView**-Seitenklasse hinzu.

```csharp
namespace QuizGame.View
{
    public sealed partial class HostView : Page
    {
        public HostView()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

Wenn dies erledigt ist, können wir uns das Markup, das das Bindungsobjekt deklariert, genauer ansehen. Das unten aufgeführte Beispiel verwendet das gleiche **Button.Content**-Bindungsziel, das wir weiter oben im Abschnitt „Bindungsziel“ verwendet haben, und zeigt seine Bindung an die **HostViewModel.NextButtonText**-Eigenschaft.

```xml
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Beachten Sie den Wert, den wir für **Path** angeben. Dieser Wert wird im Kontext der Seite selbst interpretiert, und in diesem Fall beginnt der Pfad durch einen Verweis auf die **ViewModel**-Eigenschaft, die wir gerade der **HostView**-Seite hinzugefügt haben. Diese Eigenschaft gibt eine **HostViewModel**-Instanz zurück, und so können wir damit wir dieses Objekt mit einem Punkt verknüpfen, um auf die **HostViewModel.NextButtonText**-Eigenschaft zuzugreifen. Außerdem geben wir **Mode** an, um die Standardeinstellung für einmaliges Binden [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) zu überschreiben.

Die [**Path**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.path)-Eigenschaft unterstützt eine Vielzahl von Syntaxoptionen zum Binden geschachtelter Eigenschaften, angefügter Eigenschaften sowie von Ganzzahl- und Zeichenfolgenindexern. Weitere Informationen finden Sie unter [Property-path-Syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Die Bindung an Zeichenfolgenindexer hat dieselbe Wirkung wie die Bindung an dynamische Eigenschaften, ohne [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) implementieren zu müssen. Informationen zu weiteren Einstellungen finden Sie unter [{x:Bind}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204783).

**Hinweis** Änderungen an [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.text) werden an eine bidirektionale Bindungsquelle gesendet, wenn das [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Element den Fokus verliert, und nicht nach jedem Tastaturanschlag des Benutzers.

**DataTemplate und x:DataType**

Innerhalb einer [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (ob Elementvorlage, Inhaltsvorlage oder Kopfzeilenvorlage) wird der Wert von **Path** nicht im Kontext der Seite, sondern des Datenobjekts interpretiert, das als Vorlage verwendet werden soll. Um {x:Bind} in einer Datenvorlage so zu verwenden, dass die Bindungen zum Zeitpunkt des Kompilierens überprüft werden können(und effizienter Code für sie generiert wird), muss **DataTemplate** den Typ des Datenobjekts mithilfe von **x:DataType** deklarieren. Das unten aufgeführte Beispiel kann als **ItemTemplate** eines Elementsteuerelements verwendet werden, das an eine Sammlung von **SampleDataGroup**-Objekten gebunden ist.

```xml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Schwach typisierte Objekte im Pfad**

Angenommen, Sie verfügen über einen Typ mit dem Namen „SampleDataGroup“, der eine Zeichenfolgeneigenschaft mit dem Namen „Title“ implementiert. Zudem verfügen Sie über die MainPage.SampleDataGroupAsObject-Eigenschaft (Typobjekt), die jedoch tatsächlich eine Instanz von "SampleDataGroup" zurückgibt. Die `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>`-Bindung führt zu einem Kompilierungsfehler, da die Title-Eigenschaft nicht im Typobjekt gefunden wird. Die Lösung hierfür ist das Hinzufügen einer Umwandlung zur Path-Syntax: `<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>`. Hier ein weiteres Beispiel, in dem "Element" als Objekt deklariert ist, jedoch tatsächlich ein "TextBlock" ist: `<TextBlock Text="{x:Bind Element.Text}"/>`. Und eine Umwandlung behebt das Problem: `<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>`.

**Wenn die Daten asynchron geladen werden**

Code zur Unterstützung von **{X:Bind}** wird während der Kompilierung in den partiellen Klassen für Ihre Seiten generiert. Diese Dateien befinden sich in Ihrem `obj`-Ordner und weisen Namen wie `<view name>.g.cs` auf (für C#). Der generierte Code enthält einen Handler für das [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706)-Ereignis der Seite, der die **Initialize**-Methode für eine generierte Klasse aufruft, die die Bindungen Ihrer Seite darstellt. **Initialize** ruft im Gegenzug **Update** auf, um Daten zwischen der Bindungsquelle und dem Ziel zu verschieben. **Loading** wird genau vor dem ersten Messdurchlauf der Seite oder des Benutzersteuerelements ausgelöst. Wenn Ihre Daten asynchron geladen werden, sind sie zum Zeitpunkt des Aufrufs von **Initialize** möglicherweise nicht bereit. Nachdem Sie die Daten geladen haben, können Sie daher durch den Aufruf von `this.Bindings.Update();`einmalige Bindungen erzwingen. Wenn Sie einmalige Bindungen nur für asynchron geladene Daten benötigen, ist es sehr viel kostengünstiger, sie auf diese Weise zu initialisieren, anstatt unidirektionale Bindungen zu verwenden und Änderungen zu überwachen. Wenn Ihre Daten keinen differenzierteren Änderungen unterliegen und wahrscheinlich im Rahmen einer bestimmten Aktion aktualisiert werden, können Sie einmalige Bindungen erstellen und durch Aufrufen von **Update** jederzeit ein manuelles Update erzwingen.

**Einschränkungen**

**{x:Bind}** eignet sich weder für spät gebundene Szenarien, z.B. das Navigieren in der Wörterbuchstruktur eines JSON-Objekts, noch Duck-Typing, was eine schwache Form der Eingabe basierend auf lexikalischen Übereinstimmungen von Eigenschaftennamen ist („Wenn ich einen Vogel sehe, der wie eine Ente läuft, wie eine Ente schwimmt und wie eine Ente schnattert, dann nenne ich diesen Vogel eine Ente.“). Im Fall von Duck-Typing könnte eine Bindung zur Alter-Eigenschaft auch mit einem Objekt vom Typ „Person“ oder „Wein“ erfüllt werden. Verwenden Sie für diese Szenarien **{Binding}**.

### Mit {Binding} deklariertes Bindungsobjekt

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) geht standardmäßig davon aus, dass Sie eine Bindung an den [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) der Markupseite vornehmen. Daher legen wir den **DataContext** der Seite als eine Instanz der Bindungsquellenklasse fest (in diesem Fall des Typs **HostViewModel**). Das folgende Beispiel zeigt das Markup, mit dem das Bindungsobjekt deklariert wird. Wir verwenden das gleiche **Button.Content**-Bindungsziel, das wir bereits weiter oben im Abschnitt „Bindungsziel“ verwendet haben, und führen eine Bindung an die **HostViewModel.NextButtonText**-Eigenschaft vor.

```xml
<Page xmlns:viewmodel="using:QuizGame.ViewModel" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

Beachten Sie den Wert, den wir für **Path** angeben. Dieser Wert wird im Kontext der [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)-Eigenschaft der Seite interpretiert, die in diesem Beispiel auf eine Instanz von **HostViewModel** festgelegt ist. Der Pfad verweist auf die **HostViewModel.NextButtonText**-Eigenschaft. Wir können **Mode** weglassen, da der [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)-Standardwert einer unidirektionalen Bindung in diesem Fall funktioniert.

Der Standardwert von [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) für ein Benutzeroberflächenelement ist der von der übergeordneten Eigenschaft geerbte Wert. Sie können diesen Standardwert natürlich auch überschreiben, indem Sie **DataContext** explizit festlegen, und diese Eigenschaft wird wiederum standardmäßig von den untergeordneten Elementen geerbt. Das explizite Festlegen von **DataContext** für ein Element ist nützlich, wenn Sie mehrere Bindungen verwenden möchten, die dieselbe Quelle nutzen.

Ein Bindungsobjekt verfügt über eine **Source**-Eigenschaft, für die standardmäßig der [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) des Benutzeroberflächenelements festgelegt ist, für das die Bindung deklariert ist. Sie können diese Standardeinstellung überschreiben, indem Sie **Source**, **RelativeSource** oder **ElementName** explizit für die Bindung festlegen. (Ausführliche Informationen finden Sie unter [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782).)

Innerhalb von [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) wird der [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) auf das Datenobjekt festgelegt, das als Vorlage verwendet werden soll. Das unten aufgeführte Beispiel kann als **ItemTemplate** eines Elementsteuerelements verwendet werden, das eine Sammlung eines beliebigen Typs gebunden ist, der über Zeichenfolgeneigenschaften mit dem Namen **Title** und **Description** verfügt.

```xml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

**Hinweis** Standardmäßig werden Änderungen an [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.text) an eine bidirektionale Bindungsquelle gesendet, wenn das [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Element den Fokus verliert. Damit Änderungen nach jedem Tastaturanschlag des Benutzers gesendet werden, legen Sie **UpdateSourceTrigger** auf **PropertyChanged** für die Bindung im Markup fest. Sie können auch vollständig steuern, wann Änderungen an die Quelle gesendet werden, indem Sie **UpdateSourceTrigger** auf **Explicit** festlegen. Sie behandeln dann Ereignisse für das Textfeld (in der Regel [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683)), rufen [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.getbindingexpression) am Ziel auf, um ein [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.bindingexpression.aspx)-Objekt abzurufen, und rufen zum Schluss [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.bindingexpression.updatesource.aspx) auf, um die Datenquelle programmgesteuert zu aktualisieren.

Die [**Path**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.path)-Eigenschaft unterstützt eine Vielzahl von Syntaxoptionen zum Binden geschachtelter Eigenschaften, angefügter Eigenschaften sowie von Ganzzahl- und Zeichenfolgenindexern. Weitere Informationen finden Sie unter [Property-path-Syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Die Bindung an Zeichenfolgenindexer hat dieselbe Wirkung wie die Bindung an dynamische Eigenschaften, ohne [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) implementieren zu müssen. Die [**ElementName**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.elementname)-Eigenschaft ist hilfreich für eine Element-an-Element-Bindung. Die [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.relativesource)-Eigenschaft hat mehrere Funktionen. Eine davon ist, dass sie eine leistungsfähigere Alternative zur Vorlagenbindung innerhalb von [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391) ist. Informationen zu anderen Einstellungen finden Sie unter [{Binding}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204782) und der [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820)-Klasse.

## Was geschieht, wenn die Quelle und das Ziel nicht den gleichen Typ haben?

Wenn Sie die Sichtbarkeit eines Benutzeroberflächenelements basierend auf dem Wert einer booleschen Eigenschaft steuern möchten, oder wenn Sie ein Benutzeroberflächenelement mit einer Farbe rendern möchten, die eine Funktion des Bereichs oder Trends eines numerischen Werts ist, oder wenn Sie einen Datums- und/oder Uhrzeitwert in einer Benutzeroberflächen-Elementeigenschaft angezeigt möchten, die eine Zeichenfolge erwartet, müssen Sie Werte von einem Typ in einen anderen konvertieren. Es wird Fälle geben, bei denen die richtige Lösung ist, eine andere Eigenschaft des richtigen Typs über die Bindungsquellenklasse verfügbar zu machen und die Konvertierungslogik dort gekapselt und testfähig zu belassen. Dies ist jedoch weder eine flexible noch eine skalierbare Lösung, wenn Sie über eine große Anzahl bzw. umfangreiche Kombinationen von Quell- und Zieleigenschaften verfügen. In diesem Fall verfügen Sie über verschiedene Optionen:

* Bei Verwendung von {X:Bind} können Sie für diese Konvertierung direkt an eine Funktion binden.
* Sie können zudem einen Wertkonverter als Objekt zum Durchführen der Konvertierung angeben. 

## Wertkonverter

Nachfolgend ist ein Wertkonverter dargestellt, der sich für eine einmalige oder eine unidirektionale Bindung eignet und der einen [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx)-Wert in einen Zeichenfolgenwert mit dem Monat konvertiert. Die Klasse implementiert [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

```csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```vbnet
Public Class DateToStringConverter
    Implements IValueConverter

    ' Define the Convert method to change a DateTime object to
    ' a month string.
    Public Function Convert(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.Convert

        ' value is the data from the source object.
        Dim thisdate As DateTime = CType(value, DateTime)
        Dim monthnum As Integer = thisdate.Month
        Dim month As String
        Select Case (monthnum)
            Case 1
                month = "January"
            Case 2
                month = "February"
            Case Else
                month = "Month not found"
        End Select
        ' Return the value to pass to the target.
        Return month

    End Function

    ' ConvertBack is not implemented for a OneWay binding.
    Public Function ConvertBack(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.ConvertBack

        Throw New NotImplementedException

    End Function
End Class
```

Hier sehen Sie, wie Sie diesen Wertkonverter im Bindungsobjektmarkup nutzen.

```xml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>

...

<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>

<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

Das Bindungsmodul ruft die Methoden [**Convert**](https://msdn.microsoft.com/library/windows/apps/hh701934) und [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/hh701938) auf, wenn der [**Converter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converter)-Parameter für die Bindung definiert ist. Wenn Daten aus der Quelle übergeben werden, ruft das Bindungsmodul **Convert** auf und übergibt die zurückgegebenen Daten an das Ziel. Wenn Daten aus dem Ziel übergeben werden (bei einer bidirektionalen Bindung), ruft das Bindungsmodul **ConvertBack** auf und übergibt die zurückgegebenen Daten an die Quelle.

Der Konverter verfügt auch über optionale Parameter: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converterlanguage) ermöglicht das Angeben der für die Konvertierung zu verwendenden Sprache und [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converterparameter) ermöglicht das Übergeben eines Parameters für die Konvertierungslogik. Ein Beispiel, in dem ein Konverterparameter verwendet wird, finden Sie unter [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

**Hinweis** Lösen Sie keine Ausnahme aus, wenn bei der Konvertierung ein Fehler auftritt. Geben Sie stattdessen [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyproperty.unsetvalue) zurück, wodurch die Datenübertragung beendet wird.

Um immer dann einen Standardwert anzuzeigen, wenn die Bindungsquelle nicht aufgelöst werden kann, legen Sie die **FallbackValue** -Eigenschaft für das Bindungsobjekt im Markup fest. Dies ist hilfreich bei der Behandlung von Konvertierungs- und Formatierungsfehlern. Außerdem ist es nützlich zum Binden an Quelleigenschaften, die in einer gebundenen Auflistung von heterogenen Typen ggf. nicht für alle Objekte vorhanden sind.

Wenn Sie ein Textsteuerelement an einen Wert binden, bei dem es sich nicht um eine Zeichenfolge handelt, wird der Wert vom Datenbindungsmodul in eine Zeichenfolge konvertiert. Wenn der Wert ein Verweistyp ist, ruft das Datenbindungsmodul den Zeichenfolgenwert ab, indem [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) oder [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) abgerufen wird (falls verfügbar). Andernfalls wird [**Object.ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) aufgerufen. Beachten Sie jedoch, dass das Bindungsmodul alle **ToString**-Implementierungen ignoriert, bei denen die Basisklassenimplementierung ausgeblendet wird. Stattdessen sollte bei Implementierungen von Unterklassen die **ToString**-Basisklassenmethode überschrieben werden. Auf ähnliche Weise werden in systemeigenen Sprachen analog dazu scheinbar [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) und [**IStringable**](https://msdn.microsoft.com/library/Dn302135) implementiert. Alle Aufrufe von **GetStringRepresentation** und **IStringable.ToString** werden jedoch an **Object.ToString** oder eine Überschreibung dieser Methode weitergeleitet, und niemals an eine neue **ToString**-Implementierung, bei der die Basisklassenimplementierung ausgeblendet wird.

> [!NOTE]
> Ab Windows10, Version1607, wird über das XAML-Framework ein integrierter Konverter für die Konvertierung eines booleschen Operanden in einen Sichtbarkeitszustand bereitgestellt. Der Konverter verknüpft **true** mit dem Enumerationswert **Visible** und **false** mit dem Wert **Collapsed**, sodass Sie eine Visibility-Eigenschaft an einen booleschen Wert binden können, ohne einen Konverter zu erstellen. Für die Verwendung des integrierten Konverters muss die SDK-Zielversion der App mindestens 14393 lauten. Die Verwendung ist nicht möglich, wenn Ihre App für frühere Windows10-Versionen bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

## Funktionsbindung in {x:Bind}

{x:Bind} ermöglicht den letzten Schritt beim Umwandeln eines Bindungspfads in eine Funktion. Hiermit können Konvertierungen und Bindungen durchgeführt werden, die von mehreren Eigenschaften abhängen. Siehe [**{x:Bind}-Markuperweiterung**](https://msdn.microsoft.com/en-us/windows/uwp/xaml-platform/x-bind-markup-extension)

## Ressourcenwörterbücher mit {x:Bind}

Die [{x:Bind}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204783) hängt von der Codegenerierung ab. Sie benötigt daher eine CodeBehind-Datei mit einem Konstruktor, der **InitializeComponent** aufruft (um den generierten Code zu initialisieren). Sie verwenden das Ressourcenwörterbuch erneut, indem Sie seinen Typ instanziieren (sodass **InitializeComponent** aufgerufen wird), anstatt auf den Dateinamen zu verweisen. Nachfolgend ist ein Beispiel dafür aufgeführt, was Sie tun müssen, wenn Sie über ein vorhandenes Ressourcenwörterbuch verfügen und hierfür {x:Bind} verwenden möchten.

TemplatesResourceDictionary.xaml

```xml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

```csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
```

MainPage.xaml

```xml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

## Ereignisbindung und ICommand

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) unterstützt das Feature „Ereignisbindung“. Mit diesem Feature können Sie den Handler für ein Ereignis mit einer Bindung angeben, wobei es sich zusätzlich zum Behandeln von Ereignissen mit einer Methode in der CodeBehind-Datei um eine weitere Option handelt. Angenommen, Sie haben eine **RootFrame**-Eigenschaft für Ihre **MainPage**-Klasse.

```csharp
    public sealed partial class MainPage : Page
    {
        ....    
        public Frame RootFrame { get { return Window.Current.Content as Frame; } }
    }
```

Anschließend können Sie das **Click**-Ereignis einer Schaltfläche an das **Frame**-Objekt binden, das von der **RootFrame**-Eigenschaft zurückgegeben wird, wie hier gezeigt. Beachten Sie, dass die **IsEnabled**-Eigenschaft der Schaltfläche auch an einen anderen Member des gleichen **Frame** gebunden wird.

```xml
    <AppBarButton Icon="Forward" IsCompact="True"
    IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
    Click="{x:Bind RootFrame.GoForward}"/>
```

Überladene Methoden können nicht verwendet werden, um ein Ereignis mit diesem Verfahren zu behandeln. Wenn die Methode, die das Ereignis behandelt, darüber hinaus über Parameter verfügt, müssen diese alle von den Typen aller Ereignisparameter zugeordnet werden können. In diesem Fall ist [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) nicht überladen und hat keine Parameter (wäre aber weiterhin gültig, auch wenn es zwei **object**-Parameter verwendet). [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) ist jedoch überladen, sodass wir diese Methode nicht mit diesem Verfahren verwenden können.

Das Verfahren zu Ereignisbindung ist ähnlich der Implementierung und Nutzung von Befehlen (ein Befehl ist eine Eigenschaft, die ein Objekt zurückgibt, das die [**ICommand**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.icommand.aspx)-Schnittstelle implementiert). Sowohl [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) als auch [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) kann mit Befehlen verwendet werden. Damit Sie das Befehlsmuster nicht mehrmals implementieren müssen, können Sie die **DelegateCommand**-Hilfsklasse verwenden, die Sie im [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)-Beispiel (im Ordner „Common“) finden.


## Binden an eine Sammlung von Dateien oder Ordnern

Sie können die APIs im [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346)-Namespace verwenden, um Ordner- und Dateidaten abzurufen. Die verschiedenen **GetFilesAsync**-, **GetFoldersAsync**- und **GetItemsAsync**-Methoden geben jedoch keine Werte zurück, die zur Bindung an Listensteuerelemente geeignet sind. Stattdessen müssen Sie die Bindung an die Rückgabewerte der [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422)-, [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428)-, und [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430)-Methoden der [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501)-Klasse vornehmen. Das folgende Codebeispiel aus dem [Beispiel für StorageDataSource und GetVirtualizedFilesVector](http://go.microsoft.com/fwlink/p/?linkid=228621) veranschaulicht das typische Verwendungsmuster. Vergessen Sie nicht, die **picturesLibrary**-Funktion im App-Paketmanifest zu deklarieren, und bestätigen Sie, dass Bilder im Bildbibliotheksordner vorhanden sind.

```csharp
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var library = Windows.Storage.KnownFolders.PicturesLibrary;
            var queryOptions = new Windows.Storage.Search.QueryOptions();
            queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
            queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

            var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

            var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
                fileQuery,
                Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
                190,
                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
                false
                );

            var dataSource = fif.GetVirtualizedFilesVector();
            this.PicturesListView.ItemsSource = dataSource;
        }
```

In der Regel verwenden Sie diese Vorgehensweise, um eine schreibgeschützte Ansicht der Datei- und Ordnerinformationen zu erstellen. Sie können Zwei-Wege-Bindungen an die Datei- und Ordnereigenschaften erstellen, zum Beispiel um Benutzern die Bewertung eines Titels in einer Musikansicht zu ermöglichen. Änderungen werden jedoch erst beibehalten, wenn Sie die entsprechende **SavePropertiesAsync**-Methode aufrufen (beispielsweise [**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)). Sie müssen für Änderungen einen Commit ausführen, wenn das Element den Fokus verliert, da dadurch das Zurücksetzen der Auswahl ausgelöst wird.

Beachten Sie, dass eine Zwei-Wege-Bindung mit dieser Technik nur für indizierte Orte wie "Musik" funktioniert. Sie können durch Aufrufen der [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627)-Methode feststellen, ob ein Ort indiziert ist.

Darüber hinaus kann der Wert **null** von einem virtualisierten Vektor für einige Elemente vor dem Füllen des Werts zurückgegeben werden. Beispielsweise sollten Sie nach **null** suchen, bevor Sie den [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770)-Wert eines Listensteuerelements verwenden, das an einen virtualisierten Vektor gebunden ist. Sie können stattdessen jedoch auch [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768) verwenden.

## Binden an Daten, die durch einen Schlüssel gruppiert werden

Wenn Sie eine flache Sammlung von Elementen aufnehmen – beispielsweise Bücher, dargestellt durch eine **BookSku**-Klasse – und die Elemente mithilfe einer allgemeinen Eigenschaft als Schlüssel gruppieren – beispielsweise der Eigenschaft **BookSku.AuthorName** – werden als Ergebnis gruppierte Daten aufgerufen. Wenn Sie Daten gruppieren, handelt es sich nicht mehr um eine flache Sammlung. Gruppierte Daten sind eine Sammlung von Gruppenobjekten, wobei jedes Gruppenobjekt (a) über einen Schlüssel und (b) über eine Sammlung von Elementen verfügt, deren Eigenschaft dem Schlüssel entspricht. Im Fall des Beispiels mit den Büchern resultiert die Gruppierung der Bücher nach Autorennamen in einer Sammlung von Autorennamengruppen, wobei jede Gruppe a) über einen Schlüssel verfügt, bei dem es sich um den Autorennamen handelt, und b) über eine Sammlung der **BookSku**s verfügt, deren **AuthorName**-Eigenschaft dem Gruppenschlüssel entspricht.

In der Regel binden Sie zum Anzeigen einer Sammlung die [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) eines Elementsteuerelements (wie beispielsweise [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) oder [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)) direkt an eine Eigenschaft, die eine Sammlung zurückgibt. Wenn dies eine flache Sammlung von Elementen ist, müssen Sie nichts Besonderes tun. Wenn es sich jedoch um eine Sammlung von Gruppenobjekten handelt (wie bei der Bindung gruppierter Daten), benötigen Sie die Dienste eines Zwischenobjekts, das als [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) bezeichnet wird und sich zwischen dem Elementsteuerelement und der Bindungsquelle befindet. Sie binden die **CollectionViewSource** an die Eigenschaft, die gruppierte Daten zurückgibt, und das Elementsteuerelement an die **CollectionViewSource**. Ein weiterer Vorteil einer **CollectionViewSource** besteht darin, dass sie das aktuelle Element verfolgt, sodass Sie mehr als ein Elementsteuerelement synchron beibehalten können, indem Sie alle Steuerelemente an die gleiche **CollectionViewSource** binden. Sie können über die [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857)-Eigenschaft des Objekts, das von der [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.view)-Eigenschaft zurückgegeben wird, auch programmgesteuert auf das aktuelle Element zugreifen.

Um die Gruppierungsfunktion einer [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) zu aktivieren, legen Sie [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.issourcegrouped) auf **true** fest. Ob Sie auch die [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.itemspath)-Eigenschaft festlegen müssen, hängt davon ab, wie Sie die Gruppenobjekte erstellen. Es gibt zwei Möglichkeiten zum Erstellen eines Gruppenobjekts: das Muster „is-a-group“ und das Muster „has-a-group“. Im Muster „is-a-group“ wird das Gruppenobjekt von einem Sammlungstyp abgeleitet (beispielsweise **List&lt;T&gt;**), sodass das Gruppenobjekt selbst die Gruppe von Elementen darstellt. Bei diesem Muster müssen Sie **ItemsPath** nicht festlegen. Im Muster „has-a-group“ hat das Gruppenobjekt eine oder mehrere Eigenschaften eines Sammlungstyps (beispielsweise **List&lt;T&gt;**), sodass die Gruppe über eine Gruppe von Elementen (englisch „has a group“) in Form einer Eigenschaft verfügt (oder über mehrere Gruppen von Elementen in der Form mehrerer Eigenschaften). Bei diesem Muster müssen Sie **ItemsPath** auf den Namen der Eigenschaft festlegen, die die Gruppe von Elementen enthält.

Das folgende Beispiel veranschaulicht das Muster „has-a-group“. Die Seitenklasse verfügt über eine Eigenschaft namens [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713), die eine Instanz des Ansichtsmodells zurückgibt. Die [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) bindet an die **Authors**-Eigenschaft des Ansichtsmodells (**Authors** ist die Sammlung von Gruppenobjekten) und gibt darüber hinaus an, dass die **Author.BookSkus**-Eigenschaft die gruppierten Elemente enthält. Zuletzt wird [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) an die **CollectionViewSource** gebunden und erhält eine Definition des Gruppenstils, damit die Ansicht die Elemente in den Gruppen rendern kann.

```csharp
    <Page.Resources>
        <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{x:Bind ViewModel.Authors}"
        IsSourceGrouped="true"
        ItemsPath="BookSkus"/>
    </Page.Resources>
    ...

    <GridView
    ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}" ...>
        <GridView.GroupStyle>
            <GroupStyle
                HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
        </GridView.GroupStyle>
    </GridView>
```

Beachten Sie, dass die [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) verwenden muss (und nicht [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)), da sie die **Source**-Eigenschaft auf eine Ressource festlegen muss. Um das oben aufgeführte Beispiel im Kontext der vollständigen App zu sehen, laden Sie die Beispiel-App [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) herunter. Im Gegensatz zum oben angezeigten Markup verwendet [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) nur {Binding} allein.

Sie haben zwei Möglichkeiten zum Implementieren des Musters „is-a-group“. Eine Möglichkeit besteht darin, eine eigene Gruppenklasse zu erstellen. Leiten Sie die Klasse von **List&lt;T&gt;** ab (wobei *T* der Typ der Elemente ist). Beispiel: `public class Author : List<BookSku>`. Die zweite Möglichkeit besteht in der Verwendung eines [LINQ](http://msdn.microsoft.com/library/bb397926.aspx)-Ausdrucks zum dynamischen Erstellen von Gruppenobjekten (und einer Gruppenklasse) aus ähnlichen Eigenschaftswerten der **BookSku**-Elemente. Dieser Ansatz, bei dem nur eine flache Liste von Elementen beibehalten wird, die ad-hoc zusammen gruppiert werden, ist typisch für eine App, die über einen Clouddienst auf Daten zugreift. Sie können Bücher beispielsweise nach Autor oder Genre gruppieren, ohne dafür spezielle Gruppenklassen wie **Author** und **Genre** zu benötigen.

Das folgende Beispiel veranschaulicht das Muster „is-a-group“ unter Verwendung von [LINQ](http://msdn.microsoft.com/library/bb397926.aspx). Dieses Mal werden die Bücher nach Genre gruppiert, wobei der Name des Genres in der Gruppenkopfzeile angezeigt wird. Dies wird durch den „Key“-Eigenschaftspfad als Verweis auf den [**Key**](https://msdn.microsoft.com/library/windows/apps/bb343251.aspx)-Wert der Gruppe angezeigt.

```csharp
    using System.Linq;

    ...

    private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

    public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
    {
        get
        {
            if (this.genres == null)
            {
                this.genres = from book in this.bookSkus
                group book by book.genre into grp
                orderby grp.Key select grp;
            }
            return this.genres;
        }
    }
```

Vergessen Sie nicht, dass Sie bei der Verwendung von [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) mit Datenvorlagen den Typ angeben müssen, an den die Bindung erfolgt, indem Sie einen **x:DataType**-Wert festlegen. Wenn es sich um einen generischen Typ handelt, können wir dies nicht im Markup ausdrücken. Daher müssen wir stattdessen [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) in der Gruppenstil-Kopfzeilenvorlage verwenden.

```xml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{Binding Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{Binding Source={StaticResource GenreIsACollectionOfBookSku}}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

Mit einem [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601)-Steuerelement haben Benutzer eine sehr gute Möglichkeit, gruppierte Daten anzuzeigen und darin zu navigieren. Die Beispiel-App [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) veranschaulicht die Verwendung von **SemanticZoom**. In dieser App können Sie in der vergrößerten Ansicht eine Liste der Bücher gruppiert nach Autor anzeigen, oder Sie können die Ansicht verkleinern, um eine Sprungliste der Autoren anzuzeigen. Die Sprungliste ermöglicht eine wesentlich schnellere Navigation im Vergleich zum Blättern in der Bücherliste. Die vergrößerten und verkleinerten Ansichten sind eigentlich **ListView**- bzw. **GridView**-Steuerelemente, die an dieselbe **CollectionViewSource** gebunden sind.

![Abbildung zur Veranschaulichung eines semantischen Zooms](images/sezo.png)

Wenn Sie eine Bindung an hierarchische Daten vornehmen, wie z. B. Unterkategorien von Kategorien, können Sie die Hierarchieebenen in der Benutzeroberfläche mit einer Reihe von Elementsteuerelementen anzeigen. Eine Auswahl in einem Elementsteuerelement bestimmt den Inhalt der nachfolgenden Elementsteuerelemente. Die Listen können synchron gehalten werden, indem jede Liste jeweils an ihre eigene [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) und die **CollectionViewSource**-Instanzen in einer Kette gebunden werden. Dies wird als „Master-/Detailansicht“ (oder „Listen-/Detailansicht“) bezeichnet. Weitere Informationen finden Sie unter [Binden an hierarchische Daten und Erstellen einer Master-/Detailansicht](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

## Diagnose und Debuggen von Problemen bei der Datenbindung

Ihr Bindungsmarkup enthält die Namen der Eigenschaften (und für C# manchmal Felder und Methoden). Wenn Sie eine Eigenschaft umbenennen, müssen Sie daher außerdem alle Bindungen ändern, die darauf verweisen. Wenn Sie dies vergessen, führt dies zu einem typischen Beispiel eines Datenbindungsfehlers, und Ihre App kann nicht kompiliert oder ordnungsgemäß ausgeführt werden.

Die von [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) und [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) erstellten Bindungsobjekte sind von der Funktionsweise her größtenteils identisch. {x:Bind} verfügt jedoch über Typinformationen für die Bindungsquelle und generiert zum Zeitpunkt der Kompilierung Quellcode. Bei Verwendung von {x:Bind} erhalten Sie die gleiche Art von Problemerkennung wie bei dem restlichen Code. Dazu gehört die Überprüfung von Bindungsausdrücken während des Kompilierens sowie das Debuggen durch Setzen von Haltepunkten im Quellcode, der als Teilklasse für die Seite erstellt wird. Diese Klassen befinden sich in den Dateien in Ihrem `obj`-Ordner und weisen Namen wie `<view name>.g.cs` auf (in C#). Wenn bei einer Bindung ein Problem auftritt, aktivieren Sie im Microsoft Visual Studio-Debugger die Option **Bei nicht behandelten Ausnahmen unterbrechen**. Der Debugger unterbricht die Ausführung an dieser Stelle, und Sie können den Fehler dann debuggen. Der von {x:Bind} generierte Code folgt für jeden Teil des Bindungsquellenknoten-Diagramms dem gleichen Muster. Anhand der Informationen im Fenster für die **Aufrufliste** können Sie die Reihenfolge der Aufrufe bis zum Auftreten des Problems ermitteln.

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) verfügt nicht über Typinformationen für die Bindungsquelle. Wenn Sie jedoch die App mit angefügtem Debugger ausführen, werden alle Bindungsfehler in Visual Studio im Fenster **Ausgabe** angezeigt.

## Erstellen von Bindungen im Code

**Hinweis** Dieser Abschnitt gilt nur für [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), da Sie [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)-Bindungen nicht im Code erstellen können. Allerdings können einige der Vorteile von {x:Bind} auch mit [**DependencyObject.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.registerpropertychangedcallback.aspx) erzielt werden, das Ihnen die Registrierung von Änderungsbenachrichtigungen für Abhängigkeitseigenschaften ermöglicht.

Sie können darüber hinaus Benutzeroberflächenelemente mit Daten verbinden, indem Sie anstelle von XAML prozeduralen Code verwenden. Erstellen Sie hierzu ein neues [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820)-Objekt, legen Sie die entsprechenden Eigenschaften fest, und rufen Sie anschließend [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257.aspx) oder [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244376.aspx) auf. Die programmgesteuerte Erstellung von Bindungen ist nützlich, wenn Sie die Bindungseigenschaftswerte zur Laufzeit auswählen oder eine einzelne Bindung für mehrere Steuerelemente gemeinsam verwenden möchten. Beachten Sie jedoch, dass Sie die Bindungseigenschaftswerte nach dem Aufrufen von **SetBinding** nicht mehr ändern können.

Im folgenden Beispiel wird gezeigt, wie Sie eine Bindung im Code implementieren.

```xml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

```vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

## Vergleich der Features von {x:Bind} und {Binding}

| Feature | {X:Bind} | {Binding} | Hinweise |
|---------|----------|-----------|-------|
| „Path“ ist die Standardeigenschaft. | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Path-Eigenschaft | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | In x:Bind ist Path standardmäßig an Page als Stamm gebunden, nicht DataContext. | 
| Indexer | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | Bindung an das in der Sammlung angegebene Element. Es werden nur ganzzahlbasierte Indizes unterstützt. | 
| Angefügte Eigenschaften | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | Angefügte Eigenschaften werden in Klammern angegeben. Wenn die Eigenschaft nicht in einem XAML-Namespace deklariert wird, stellen Sie ihr einen XML-Namespace als Präfix voran, der einem Codenamespace am Anfang des Dokuments zugeordnet werden sollte. | 
| Umwandlung | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | Nicht erforderlich | Umwandlungen werden in Klammern angegeben. Wenn die Eigenschaft nicht in einem XAML-Namespace deklariert wird, stellen Sie ihr einen XML-Namespace als Präfix voran, der einem Codenamespace am Anfang des Dokuments zugeordnet werden sollte. | 
| Konverter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Konverter müssen im Stamm von Page/ResourceDictionary oder in „App.xaml“ deklariert werden. | 
| KonverterParameter, KonverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Konverter müssen im Stamm von Page/ResourceDictionary oder in „App.xaml“ deklariert werden. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | Wird verwendet, wenn das Blatt des Bindungsausdrucks NULL ist. Verwenden Sie zum Angeben eines Zeichenfolgenwerts einfache Anführungszeichen. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | Wird verwendet, wenn ein beliebiger Teil des Pfads für die Bindung (mit Ausnahme des Blatts) NULL ist. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | Mit {x:Bind} nehmen Sie eine Bindung an ein Feld vor; Path standardmäßig an Page als Stamm gebunden, damit auf jedes benannte Element über sein Feld zugegriffen werden kann. | 
| RelativeSource: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | Bei {x:Bind}: Benennen Sie das Element, und verwenden Sie den Namen in Path. | 
| RelativeSource: TemplatedParent | Nicht unterstützt | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | In den meisten Fällen kann eine reguläre Vorlagenbindung in Steuerelementvorlagen verwendet werden. Verwenden Sie jedoch „TemplatedParent“, wenn Sie einen Konverter oder bidirektionale Bindungen verwenden müssen. | 
| Source | Nicht unterstützt | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | Bei {x:Bind}: Verwenden Sie stattdessen eine Eigenschaft oder einen statischen Pfad. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | „Mode“ kann „OneTime“, „OneWay“ oder „TwoWay“ sein. Standardwert für {x:Bind} ist „OneTime“; Standardwert für {Binding} ist „OneWay“. | 
| UpdateSourceTrigger | Nicht unterstützt | `<Binding UpdateSourceTrigger="Default [or] PropertyChanged [or] Explicit"/>` | {x:Bind} verwendet das PropertyChanged-Verhalten in allen Fällen, außer bei „TextBox.Text“, wo es auf den Verlust des Fokus wartet, um die Quelle zu aktualisieren. | 





<!--HONumber=Aug16_HO3-->


