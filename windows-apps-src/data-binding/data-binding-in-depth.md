---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: Datenbindung im Detail
description: Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann.
---
# Datenbindung im Detail

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** Wichtige APIs **

-   [**Binding-Klasse**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

**Hinweis**  In diesem Thema werden die Datenbindungsfeatures ausführlich beschrieben. Eine kurze und praktische Einführung finden Sie unter [Übersicht „Datenbindung“](data-binding-quickstart.md).

 

Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann. Mit der Datenbindung können Sie Datenaspekte von Benutzeroberflächenaspekten trennen, was zu einem einfacheren konzeptionellen Modell und besserer Lesbarkeit, Testbarkeit und Wartung Ihrer App führt.

Sie können die Datenbindung einfach verwenden, um nur Werte aus einer Datenquelle anzuzeigen, wenn die Benutzeroberfläche das erste Mal angezeigt wird, jedoch nicht auf Änderungen an diesen Werten reagieren. Dies wird als „einmalige Bindung“ bezeichnet und funktioniert gut für Daten, deren Werte während der Laufzeit nicht geändert werden. Darüber hinaus können Sie die Werte auch „feststellen“ und die Benutzeroberfläche bei einer Änderung aktualisieren. Dies wird als „unidirektionale Bindung“ bezeichnet und eignet sich gut für schreibgeschützte Daten. Letztendlich können Sie die Daten sowohl feststellen als auch aktualisieren, damit die vom Benutzer in der Benutzeroberfläche vorgenommenen Änderungen automatisch in die Datenquelle übernommen werden. Dies wird als „bidirektionale Bindung“ bezeichnet und eignet sich gut für Daten mit Lese-/Schreibberechtigung. Beispiele:

-   Sie können die einmalige Bindung verwenden, um ein [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) an das Foto des aktuellen Benutzers zu binden.
-   Sie können die unidirektionale Bindung verwenden, um ein [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)-Objekt an eine Sammlung von Nachrichtenartikeln in Echtzeit nach Zeitungsteil gruppiert zu binden.
-   Sie könnten eine bidirektionale Bindung verwenden, um ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)-Objekt an den Namen eines Kunden in einem Formular zu binden.

Es gibt zwei Arten von Bindungen. Diese werden in der Regel beide im Benutzeroberflächenmarkup deklariert. Sie können entweder die [{x:Bind}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204783) oder die [{Binding}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204782) verwenden. Sie können sogar eine Mischung aus beiden Erweiterungen in derselben App verwenden – sogar im gleichen Benutzeroberflächenelement. {x:Bind} ist neu in Windows 10 und bietet eine bessere Leistung. {Binding} bietet mehr Features. Alle in diesem Thema beschriebenen Details gelten für beide Arten von Bindungen, es sei denn, es wird ausdrücklich auf eine Abweichung hingewiesen.

**Beispiel-Apps zur Veranschaulichung von {x:Bind}**

-   [{x:Bind}-Beispiel](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame).
-   [Beispiel für XAML-Benutzeroberflächengrundlagen](http://go.microsoft.com/fwlink/p/?linkid=619992)

**Beispiel-Apps zur Veranschaulichung von {Binding}**

-   Laden Sie die App [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950) herunter.
-   Laden Sie die App [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) herunter.

Was zu jeder Bindung gehört
------------------------------------

-   Eine *Bindungsquelle*: Dies ist die Quelle der Daten für die Bindung und kann eine Instanz einer Klasse mit Membern sein, deren Werte Sie in der Benutzeroberfläche anzeigen möchten.
-   Ein *Bindungsziel*: Dies ist eine [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) des [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) in Ihrer Benutzeroberfläche, die die Daten anzeigt.
-   Ein *Bindungsobjekt*: Dies ist der Teil, der Datenwerte von der Quelle an das Ziel sowie optional vom Ziel zurück an die Quelle überträgt. Das Bindungsobjekt wird zur XAML-Ladezeit von der [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)- oder [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)-Markuperweiterung erstellt.

In den folgenden Abschnitten betrachten wird die Bindungsquelle, das Bindungsziel und das Bindungsobjekt genauer. Außerdem verknüpfen wir die Abschnitte durch ein Beispiel, bei dem der Inhalt einer Schaltfläche an eine Zeichenfolgeneigenschaft mit dem Namen **NextButtonText** gebunden wird, die zur Klasse **HostViewModel** gehört.

Bindungsquelle
--------------

Nachfolgend wird eine sehr einfache Implementierung einer Klasse gezeigt, die wir als Bindungsquelle verwenden könnten.

**Hinweis**  Bei Verwendung von [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) mit Komponentenerweiterungen für Visual C++ (C++/CX) müssen Sie das [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872)-Attribut zur Bindungsquellklasse hinzufügen. Bei Verwendung von [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ist dieses Attribut nicht erforderlich. Unter [Hinzufügen einer Detailansicht](data-binding-quickstart.md#adding-a-details-view) finden Sie einen Codeausschnitt.

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

Eine einfachere Möglichkeit, eine Klasse feststellbar zu machen – und eine Notwendigkeit für Klassen, die bereits über eine Basisklasse verfügen – besteht in der Implementierung von [**System.ComponentModel.INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged). Hierzu muss lediglich ein einzelnes Ereignis mit dem Namen **PropertyChanged** implementiert werden. Weiter unten finden Sie ein Beispiel für die Verwendung von **HostViewModel**.

**Hinweis**  Im Fall von C++/CX implementieren Sie [**Windows::UI::Xaml::Data::INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899). Die Bindungsquellenklasse muss entweder über das [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) verfügen oder [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) implementieren.

``` csharp
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

``` csharp
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

Das Auslösen des **PropertyChanged**-Ereignisses mit dem Argument [**String.Empty**](T:System.String) oder **null** gibt an, dass alle Eigenschaften, die keine Indexereigenschaften sind, für das Objekt erneut gelesen werden sollen. Sie können das Ereignis auslösen, um anzugeben, dass die Indexereigenschaften für das Objekt geändert wurden, indem Sie ein Argument von „Item [*indexer*\]“ für bestimmte Indexer (mit *indexer* als Indexwert) oder einen Wert von „Item\[\]“ für alle Indexer verwenden.

Eine Bindungsquelle kann entweder als einzelnes Objekt behandelt werden, dessen Eigenschaften Daten enthalten, oder als Sammlung von Objekten. In C#- und Visual Basic-Code können Sie eine einmalige Bindung an ein Objekt vornehmen, das [**List(Of T)**](T:System.Collections.Generic.List%601) zum Anzeigen einer Sammlung implementiert, die nicht zur Laufzeit geändert wird. Nehmen Sie für eine feststellbare Sammlung (es wird festgestellt, wann Elemente zur Sammlung hinzugefügt oder aus dieser entfernt werden) stattdessen eine unidirektionale Bindung an [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) vor. In C++-Code können Sie sowohl für feststellbare als auch für nicht feststellbare Sammlungen eine Bindung an [**Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/dn858385.aspx) vornehmen. Wenn Sie eine Bindung an Ihre eigenen Sammlungsklassen vornehmen möchten, verwenden Sie die Anleitung in der folgenden Tabelle.

| Szenario                                                        | C# und VB (CLR)                                                                                                                                                                                                                                                                                                                                                                                                                   | C++/CX                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| An ein Objekt binden                                              | Es kann sich um jedes Objekt handeln.                                                                                                                                                                                                                                                                                                                                                                                                                 | Das Objekt muss [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) besitzen oder [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) implementieren.                                                                                                                                                                                                                                                                                                             |
| Abrufen von Eigenschaftenänderungen eines gebundenen Objekts.                | Das Objekt muss [**System.ComponentModel. INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged) implementieren.                                                                                                                                                                                                                                                                                                         | Das Objekt muss [**Windows.UI.Xaml.Data. INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899) implementieren.                                                                                                                                                                                                                                                                                                                                                           |
| An eine Sammlung binden                                           | [**List(Of T)**](T:System.Collections.Generic.List%601)                                                                                                                                                                                                                                                                                                                                                                            | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Abrufen von Sammlungsänderungen einer gebundenen Sammlung          | [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)                                                                                                                                                                                                                                                                                                                                        | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Implementieren einer Sammlung, die Bindungen unterstützt                   | Erweitern von [**List(Of T)**](T:System.Collections.Generic.List%601) oder Implementieren von [**IList**](T:System.Collections.IList), [**IList**](T:System.Collections.Generic.IList%601)(Of [**Object**](T:System.Object)), [**IEnumerable**](T:System.Collections.IEnumerable) oder [**IEnumerable**](T:System.Collections.Generic.IEnumerable%601)(Of **Object**). Die Bindung an generische **IList(Of T)** und **IEnumerable(Of T)** wird nicht unterstützt. | Implementieren Sie [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957), [**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](T:System.Object)^&gt;, [**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; oder **IIterable**&lt;**IInspectable**\*&gt;. Die Bindung an generische **IVector&lt;T&gt;** und **IIterable&lt;T&gt;** wird nicht unterstützt. |
| Implementieren einer Sammlung, die Änderungen an der Sammlung unterstützt | Erweitern Sie [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601), oder implementieren Sie (nicht generische) [**IList**](T:System.Collections.IList) und [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged).                                                                                                                                                               | Implementieren Sie [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) und [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974).                                                                                                                                                                                                                                                                                                                       |
| Implementieren einer Sammlung, die inkrementelles Laden unterstützt       | Erweitern Sie [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601), oder implementieren Sie (nicht generische) [**IList**](T:System.Collections.IList) und [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged). Implementieren Sie zusätzlich [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                          | Implementieren Sie [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) und [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                                                                                                                                                                                                         |

 
Sie können mithilfe von inkrementellem Laden Listensteuerelemente an beliebig große Datenquellen binden und dennoch eine hohe Leistung erreichen. Zum Beispiel können Sie Listensteuerelemente an Ergebnisse von Bildabfragen in Bing binden, ohne alle Ergebnisse auf einmal laden zu müssen. Stattdessen laden Sie nur einige der Ergebnisse sofort und weitere Ergebnisse nach Bedarf. Sie müssen [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916) für eine Datenquelle umsetzen, die die Sammlungsänderungsbenachrichtigung unterstützt, um das inkrementelle Laden zu unterstützen. Wenn das Datenbindungsmodul weitere Daten anfordert, muss Ihre Datenquelle die entsprechenden Anforderungen senden, die Ergebnisse integrieren und anschließend die entsprechende Benachrichtigung senden, um die UI zu aktualisieren.

Bindungsziel
--------------

In den folgenden beiden Beispielen ist die **Button.Content**-Eigenschaft das Bindungsziel. Ihr Wert ist auf eine Markuperweiterung festgelegt, die das Bindungsobjekt deklariert. Es wird zuerst [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) angezeigt, dann [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Das Deklarieren von Bindungen im Markup wird häufig verwendet (es ist praktisch, lesbar und toolfreundlich). Sie können Markups jedoch auch vermeiden und eine Instanz der [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820)-Klasse stattdessen auch bei Bedarf imperativ (programmgesteuert) erstellen.

<!-- XAML lang specifier not yet supported in OP. Using XML for now. -->
``` xml
<Button Content="{x:Bind ...}" ... />
```

``` xml
<Button Content="{Binding ...}" ... />
```

Binding object declared using {x:Bind}
--------------------------------------

There's one step we need to do before we author our [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) markup. We need to expose our binding source class from the class that represents our page of markup. We do that by adding a property (of type **HostViewModel** in this case) to our **HostView** page class.

``` csharp
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

Nun können wir uns das Markup genauer ansehen, das das Bindungsobjekt deklariert. Das unten aufgeführte Beispiel verwendet das gleiche **Button.Content**-Bindungsziel, das wir weiter oben im Abschnitt „Bindungsziel“ verwendet haben, und zeigt seine Bindung an die **HostViewModel.NextButtonText**-Eigenschaft.

``` xml
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page itself, and in this case the path begins by referencing the **ViewModel** property that we just added to the **HostView** page. That property returns a **HostViewModel** instance, and so we can dot into that object to access the **HostViewModel.NextButtonText** property. And we specify **Mode**, to override the [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) default of one-time.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). For other settings, see [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783).

**Note**  Changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus, and not after every user keystroke.

**DataTemplate and x:DataType**

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (whether used as an item template, a content template, or a header template), the value of **Path** is not interpreted in the context of the page, but in the context of the data object being templated. So that its bindings can be validated (and efficient code generated for them) at compile-time, a **DataTemplate** needs to declare the type of its data object using **x:DataType**. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of **SampleDataGroup** objects.

``` xml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Weakly-typed objects in your Path**

Consider for example that you have a type named SampleDataGroup, which implements a string property named Title. And you have a property MainPage.SampleDataGroupAsObject, which is of type object but which actually returns an instance of SampleDataGroup. The binding `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` will result in a compile error because the Title property is not found on the type object. The remedy for this is to add a cast to your Path syntax like this: `<TextBlock Text="{x:Bind SampleDataGroupAsObject.(data:SampleDataGroup.Title)}"/>`. Here's another example where Element is declared as object but is actually a TextBlock: `<TextBlock Text="{x:Bind Element.Text}"/>`. And a cast remedies the issue: `<TextBlock Text="{x:Bind Element.(TextBlock.Text)}"/>`.

**Wenn Ihre Daten asynchron geladen werden**

Code zur Unterstützung von **{x:Bind}** wird während der Kompilierung in den partiellen Klassen für Ihre Seiten generiert. Diese Dateien befinden sich in Ihrem `obj`-Ordner und weisen (für C#) Namen auf wie<view name>`.g.cs`. The generated code includes a handler for your page's [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706-loading) event, and that handler calls the **Initialize** method on a generated class that represent's your page's bindings. **Initialize** in turn calls **Update** to begin moving data between the binding source and the target. **Loading** is raised just before the first measure pass of the page or user control. So if your data is loaded asynchronously it may not be ready by the time **Initialize** is called. So, after you've loaded data, you can force one-time bindings to be initialized by calling `this->Bindings->Update();`. Wenn Sie einmalige Bindungen nur für asynchron geladene Daten benötigen, ist es sehr viel kostengünstiger, sie auf diese Weise zu initialisieren, anstatt unidirektionale Bindungen zu verwenden und Änderungen zu überwachen. Wenn Ihre Daten keinen differenzierteren Änderungen unterliegen und wahrscheinlich im Rahmen einer bestimmten Aktion aktualisiert werden, können Sie einmalige Bindungen erstellen und durch Aufrufen von **Update** jederzeit ein manuelles Update erzwingen.

**Einschränkungen**

**{x:Bind}** ist nicht spät gebundene Szenarien, beispielsweise das Navigieren in der Wörterbuchstruktur eines JSON-Objekts, noch für Duck-Typing geeignet, eine schwache Form der Eingabe basierend auf lexikalischen Übereinstimmungen von Eigenschaftennamen („Wenn ich einen Vogel sehe, der wie eine Ente läuft, wie eine Ente schwimmt und wie eine Ente schnattert, dann nenne ich diesen Vogel eine Ente.“). Im Fall von Duck-Typing könnte eine Bindung zur Alter-Eigenschaft auch mit einem Objekt vom Typ „Person“ oder „Wein“ erfüllt werden. Verwenden Sie für diese Szenarien **{Binding}**.

Ein mittels {Binding} deklariertes Binding-Objekt
---------------------------------------

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) setzt standardmäßig voraus, dass Sie eine Bindung an [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) Ihrer Markupsprache ausführen. Daher legen wir den **DataContext** der Seite als eine Instanz der Bindungsquellenklasse fest (in diesem Fall des Typs **HostViewModel**). Das folgende Beispiel zeigt das Markup, mit dem das Bindungsobjekt deklariert wird. Wir verwenden das gleiche **Button.Content**-Bindungsziel, das wir bereits weiter oben im Abschnitt „Bindungsziel“ verwendet haben, und führen eine Bindung an die **HostViewModel.NextButtonText**-Eigenschaft vor.

``` xml
<Page xmlns:viewmodel="using:QuizGame.ViewModel" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page's [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), which in this example is set to an instance of **HostViewModel**. The path references the **HostViewModel.NextButtonText** property. We can omit **Mode**, because the [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) default of one-way works here.

The default value of [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) for a UI element is the inherited value of its parent. You can of course override that default by setting **DataContext** explicitly, which is in turn inherited by children by default. Setting **DataContext** explicitly on an element is useful when you want to have multiple bindings that use the same source.

A binding object has a **Source** property, which defaults to the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) of the UI element on which the binding is declared. You can override this default by setting **Source**, **RelativeSource**, or **ElementName** explicitly on the binding (see [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) for details).

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) is set to the data object being templated. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of any type that has string properties named **Title** and **Description**.

``` xml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

**Note**  By default, changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus. To cause changes to be sent after every user keystroke, set **UpdateSourceTrigger** to **PropertyChanged** on the binding in markup. You can also completely take control of when changes are sent to the source by setting **UpdateSourceTrigger** to **Explicit**. You then handle events on the text box (typically [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683-textchanged)), call [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR208706-getbindingexpression) on the target to get a [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR209820expression) object, and finally call [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/BR209820expression-updatesource) to programmatically update the data source.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). The [**ElementName**](https://msdn.microsoft.com/library/windows/apps/BR209820-elementname) property is useful for element-to-element binding. The [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/BR209820-relativesource) property has several uses, one of which is as a more powerful alternative to template binding inside a [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391). For other settings, see [{Binding} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204782) and the [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) class.

What if the source and the target are not the same type?
--------------------------------------------------------

If you want to control the visibility of a UI element based on the value of a boolean property, or if you want to render a UI element with a color that's a function of a numeric value's range or trend, or if you want to display a date and/or time value in a UI element property that expects a string, then you'll need to convert values from one type to another. There will be cases where the right solution is to expose another property of the right type from your binding source class, and keep the conversion logic encapsulated and testable there. But that isn't flexible nor scalable when you have large numbers, or large combinations, of source and target properties. In that case you'll want to use something known as a value converter. This section describes how to implement and consume a value converter.

Here's a value converter, suitable for a one-time or a one-way binding, that converts a [**DateTime**](T:System.DateTime) value to a string value containing the month. The class implements [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

``` csharp
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

``` vbnet
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

Hier sehen Sie, wie dieser Wertkonverter im Bindungsobjektmarkup verwendet wird.

``` xml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>

...

<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>

<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

The binding engine calls the [**Convert**](https://msdn.microsoft.com/library/windows/apps/BR209903-convert) and [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/BR209903-convertback) methods if the [**Converter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converter) parameter is defined for the binding. When data is passed from the source, the binding engine calls **Convert** and passes the returned data to the target. When data is passed from the target (for a two-way binding), the binding engine calls **ConvertBack** and passes the returned data to the source.

The converter also has optional parameters: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterlanguage), which allows specifying the language to be used in the conversion, and [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterparameter), which allows passing a parameter for the conversion logic. For an example that uses a converter parameter, see [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

**Note**  If there is an error in the conversion, do not throw an exception. Instead, return [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/BR242362-unsetvalue), which will stop the data transfer.

To display a default value to use whenever the binding source cannot be resolved, set the **FallbackValue** property on the binding object in markup. This is useful to handle conversion and formatting errors. It is also useful to bind to source properties that might not exist on all objects in a bound collection of heterogeneous types.

If you bind a text control to a value that is not a string, the data binding engine will convert the value to a string. If the value is a reference type, the data binding engine will retrieve the string value by calling [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/BR209878-getstringrepresentation) or [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) if available, and will otherwise call [**Object.ToString**](M:System.Object.ToString). Note, however, that the binding engine will ignore any **ToString** implementation that hides the base-class implementation. Subclass implementations should override the base class **ToString** method instead. Similarly, in native languages, all managed objects appear to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) and [**IStringable**](https://msdn.microsoft.com/library/Dn302135). However, all calls to **GetStringRepresentation** and **IStringable.ToString** are routed to **Object.ToString** or an override of that method, and never to a new **ToString** implementation that hides the base-class implementation.

Resource dictionaries with {x:Bind}
-----------------------------------

The [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783) depends on code generation, so it needs a code-behind file containing a constructor that calls **InitializeComponent** (to initialize the generated code). You re-use the resource dictionary by instantiating its type (so that **InitializeComponent** is called) instead of referencing its filename. Here's an example of what to do if you have an existing resource dictionary and you want to use {x:Bind} in it.

TemplatesResourceDictionary.xaml

``` xml
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

``` csharp
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

``` xml
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

Event binding and ICommand
--------------------------

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) supports a feature called event binding. With this feature, you can specify the handler for an event using a binding, which is an additional option on top of handling events with a method on the code-behind file. Let's say you have a **RootFrame** property on your **MainPage** class.

``` csharp
    public sealed partial class MainPage : Page
    {
        ....    
        public Frame RootFrame { get { return Window.Current.Content as Frame; } }
    }
```

Anschließend können Sie das **Click**-Ereignis einer Schaltfläche an das **Frame**-Objekt binden, das von der **RootFrame**-Eigenschaft zurückgegeben wird, wie hier gezeigt. Beachten Sie, dass die **IsEnabled**-Eigenschaft der Schaltfläche auch an einen anderen Member des gleichen **Frame** gebunden wird.

``` xml
    <AppBarButton Icon="Forward" IsCompact="True"
    IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
    Click="{x:Bind RootFrame.GoForward}"/>
```

Overloaded methods cannot be used to handle an event with this technique. Also, if the method that handles the event has parameters then they must all be assignable from the types of all of the event's parameters, respectively. In this case, [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) is not overloaded and it has no parameters (but it would still be valid even if it took two **object** parameters). [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) is overloaded, though, so we can't use that method with this technique.

The event binding technique is similar to implementing and consuming commands (a command is a property that returns an object that implements the [**ICommand**](T:System.Windows.Input.ICommand) interface). Both [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) work with commands. So that you don't have to implement the command pattern multiple times, you can use the **DelegateCommand** helper class that you'll find in the [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) sample (in the "Common" folder).

## Binding to a collection of folders or files

You can use the APIs in the [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) namespace to retrieve folder and file data. However, the various **GetFilesAsync**, **GetFoldersAsync**, and **GetItemsAsync** methods do not return values that are suitable for binding to list controls. Instead, you must bind to the return values of the [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422), [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428), and [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430) methods of the [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501) class. The following code example from the [StorageDataSource and GetVirtualizedFilesVector sample](http://go.microsoft.com/fwlink/p/?linkid=228621) shows the typical usage pattern. Remember to declare the **picturesLibrary** capability in your app package manifest, and confirm that there are pictures in your Pictures library folder.

``` csharp
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

In der Regel wird dieser Ansatz verwendet, um eine schreibgeschützte Ansicht der Datei- und Ordnerinformationen zu erstellen. Sie können Zwei-Wege-Bindungen an die Datei- und Ordnereigenschaften erstellen, zum Beispiel um Benutzern die Bewertung eines Titels in einer Musikansicht zu ermöglichen. Änderungen werden jedoch erst beibehalten, wenn Sie die entsprechende **SavePropertiesAsync**-Methode aufrufen (beispielsweise [**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)). Sie müssen für Änderungen einen Commit ausführen, wenn das Element den Fokus verliert, da dadurch das Zurücksetzen der Auswahl ausgelöst wird.

Beachten Sie, dass eine Zwei-Wege-Bindung mit dieser Technik nur für indizierte Orte wie "Musik" funktioniert. Sie können durch Aufrufen der [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627)-Methode feststellen, ob ein Ort indiziert ist.

Darüber hinaus kann der Wert **null** von einem virtualisierten Vektor für einige Elemente vor dem Füllen des Werts zurückgegeben werden. Beispielsweise sollten Sie nach **null** suchen, bevor Sie den [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770)-Wert eines Listensteuerelements verwenden, das an einen virtualisierten Vektor gebunden ist. Sie können stattdessen jedoch auch [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768) verwenden.

Bindung an mithilfe eines Schlüssels gruppierte Daten
--------------------------------

Wenn Sie eine flache Sammlung von Elementen aufnehmen – beispielsweise Bücher, dargestellt durch eine **booksku**-Klasse – und die Elemente mithilfe einer allgemeinen Eigenschaft als Schlüssel gruppieren – beispielsweise der Eigenschaft **BookSku.AuthorName** – werden als Ergebnis gruppierte Daten aufgerufen. Wenn Sie Daten gruppieren, handelt es sich nicht mehr um eine flache Sammlung. Gruppierte Daten sind eine Sammlung von Gruppenobjekten, wobei jedes Gruppenobjekt (a) über einen Schlüssel und (b) über eine Sammlung von Elementen verfügt, deren Eigenschaft dem Schlüssel entspricht. Im Fall des Beispiels mit den Büchern resultiert die Gruppierung der Bücher nach Autorennamen in einer Sammlung von Autorennamengruppen, wobei jede Gruppe a) über einen Schlüssel verfügt, bei dem es sich um den Autorennamen handelt, und b) über eine Sammlung der **BookSku**s verfügt, deren **AuthorName**-Eigenschaft dem Gruppenschlüssel entspricht.

In der Regel binden Sie zum Anzeigen einer Sammlung die [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) eines Elementsteuerelements (wie beispielsweise [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) oder [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)) direkt an eine Eigenschaft, die eine Sammlung zurückgibt. Wenn dies eine flache Sammlung von Elementen ist, müssen Sie nichts Besonderes tun. Wenn es sich jedoch um eine Sammlung von Gruppenobjekten handelt (wie bei der Bindung gruppierter Daten), benötigen Sie die Dienste eines Zwischenobjekts, das als [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) bezeichnet wird und sich zwischen dem Elementsteuerelement und der Bindungsquelle befindet. Sie binden die **CollectionViewSource** an die Eigenschaft, die gruppierte Daten zurückgibt, und das Elementsteuerelement an die **CollectionViewSource**. Ein weiterer Vorteil einer **CollectionViewSource** besteht darin, dass sie das aktuelle Element verfolgt, sodass Sie mehr als ein Elementsteuerelement synchron beibehalten können, indem Sie alle Steuerelemente an die gleiche **CollectionViewSource** binden. Sie können über die [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857)-Eigenschaft des Objekts, das von der [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/BR209833-view)-Eigenschaft zurückgegeben wird, auch programmgesteuert auf das aktuelle Element zugreifen.

Um die Gruppierungsfunktion einer [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) zu aktivieren, legen Sie [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/BR209833-issourcegrouped) auf **true** fest. Ob Sie auch die [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/BR209833-itemspath)-Eigenschaft festlegen müssen, hängt davon ab, wie Sie die Gruppenobjekte erstellen. Es gibt zwei Möglichkeiten zum Erstellen eines Gruppenobjekts: das Muster „is-a-group“ und das Muster „has-a-group“. Im Muster „is-a-group“ wird das Gruppenobjekt von einem Sammlungstyp abgeleitet (beispielsweise **List&lt;T&gt;**), sodass das Gruppenobjekt selbst die Gruppe von Elementen darstellt. Bei diesem Muster müssen Sie **ItemsPath** nicht festlegen. Im Muster „has-a-group“ hat das Gruppenobjekt eine oder mehrere Eigenschaften eines Sammlungstyps (beispielsweise **List&lt;T&gt;**), sodass die Gruppe über eine Gruppe von Elementen (engl. „has a group“) in Form einer Eigenschaft verfügt (oder über mehrere Gruppen von Elementen in der Form mehrerer Eigenschaften). Bei diesem Muster müssen Sie **ItemsPath** auf den Namen der Eigenschaft festlegen, die die Gruppe von Elementen enthält.

Das folgende Beispiel veranschaulicht das Muster „has-a-group“. Die Seitenklasse verfügt über eine Eigenschaft namens [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713), die eine Instanz des Ansichtsmodells zurückgibt. Die [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) bindet an die **Authors**-Eigenschaft des Ansichtsmodells (**Authors** ist die Sammlung von Gruppenobjekten) und gibt darüber hinaus an, dass die **Author.BookSkus**-Eigenschaft die gruppierten Elemente enthält. Zuletzt wird [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) an die **CollectionViewSource** gebunden und erhält eine Definition des Gruppenstils, damit die Ansicht die Elemente in den Gruppen rendern kann.

``` csharp
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

Note that the [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) must use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) (and not [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) because it needs to set the **Source** property to a resource. To see the above example in the context of the complete app, download the [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app. Unlike the markup shown above, [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) uses {Binding} exclusively.

You can implement the "is-a-group" pattern in one of two ways. One way is to author your own group class. Derive the class from **List&lt;T&gt;** (where *T* is the type of the items). For example, `public class Author : List<BookSku>`. Die zweite Möglichkeit besteht in der Verwendung eines [LINQ](http://msdn.microsoft.com/library/bb397926.aspx)-Ausdrucks zum dynamischen Erstellen von Gruppenobjekten (und einer Gruppenklasse) aus ähnlichen Eigenschaftswerten der **BookSku**-Elemente. Dieser Ansatz, bei dem nur eine flache Liste von Elementen beibehalten wird, die ad-hoc zusammen gruppiert werden, ist typisch für eine App, die über einen Clouddienst auf Daten zugreift. Sie können Bücher beispielsweise nach Autor oder Genre gruppieren, ohne dafür spezielle Gruppenklassen wie **Author** und **Genre** zu benötigen.

Das folgende Beispiel zeigt das Muster „is-a-group“ unter Verwendung von [LINQ](http://msdn.microsoft.com/library/bb397926.aspx). Dieses Mal werden die Bücher nach Genre gruppiert, wobei der Name des Genres in der Gruppenkopfzeile angezeigt wird. Dies wird durch den „Key“-Eigenschaftspfad als Verweis auf den [**Key**](P:System.Linq.IGrouping%602.Key)-Wert der Gruppe angezeigt.

``` csharp
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

Remember that when using [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) with data templates we need to indicate the type being bound to by setting an **x:DataType** value. If the type is generic then we can't express that in markup so we need to use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) instead in the group style header template.

``` xml
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

A [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) control is a great way for your users to view and navigate grouped data. The [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app illustrates how to use the **SemanticZoom**. In that app, you can view a list of books grouped by author (the zoomed-in view) or you can zoom out to see a jump list of authors (the zoomed-out view). The jump list affords much quicker navigation than scrolling through the list of books. The zoomed-in and zoomed-out views are actually **ListView** or **GridView** controls bound to the same **CollectionViewSource**.

![An illustration of a SemanticZoom](images/sezo.png)

When you bind to hierarchical data—such as subcategories within categories—you can choose to display the hierarchical levels in your UI with a series of items controls. A selection in one items control determines the contents of subsequent items controls. You can keep the lists synchronized by binding each list to its own [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) and binding the **CollectionViewSource** instances together in a chain. This is called a master/details (or list/details) view. For more info, see [How to bind to hierarchical data and create a master/details view](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

Diagnosing and debugging data binding problems
-----------------------------------------------

Your binding markup contains the names of properties (and, for C#, sometimes fields and methods). So when you rename a property, you'll also need to change any binding that references it. Forgetting to do that leads to a typical example of a data binding bug, and your app either won't compile or won't run correctly.

The binding objects created by [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) are largely functionally equivalent. But {x:Bind} has type information for the binding source, and it generates source code at compile-time. With {x:Bind} you get the same kind of problem detection that you get with the rest of your code. That includes compile-time validation of your binding expressions, and debugging by setting breakpoints in the source code generated as the partial class for your page. These classes can be found in the files in your `obj` folder, with names like (for C#) `<view name>.g.cs`). Wenn bei einer Bindung ein Problem auftritt, aktivieren Sie im Microsoft Visual Studio-Debugger die Option **Bei nicht behandelten Ausnahmen unterbrechen**. Der Debugger unterbricht die Ausführung an dieser Stelle, und Sie können den Fehler dann debuggen. Der von {x:Bind} generierte Code folgt für jeden Teil des Bindungsquellenknoten-Diagramms dem gleichen Muster. Anhand der Informationen im Fenster für die **Aufrufliste** können Sie die Reihenfolge der Aufrufe bis zum Auftreten des Problems ermitteln.

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) verfügt nicht über Typinformationen für die Bindungsquelle. Wenn Sie jedoch die App mit angefügtem Debugger ausführen, werden alle Bindungsfehler in Visual Studio im Fenster **Ausgabe** angezeigt.

Erstellen von Bindungen im Code
-------------------------

**Hinweis**  Dieser Abschnitt gilt nur für [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), da Sie keine [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)-Bindungen im Code erstellen können. Allerdings können einige der Vorteile von {x:Bind} auch mit [**DependencyProperty.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/BR242356-registerpropertychangedcallback) erzielt werden, das Ihnen die Registrierung von Änderungsbenachrichtigungen für Abhängigkeitseigenschaften ermöglicht.

Sie können darüber hinaus Benutzeroberflächenelemente mit Daten verbinden, indem Sie anstelle von XAML prozeduralen Code verwenden. Erstellen Sie hierzu ein neues [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820)-Objekt, legen Sie die entsprechenden Eigenschaften fest, und rufen Sie anschließend [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR208706-setbinding) oder [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR209820operations-setbinding) auf. Die programmgesteuerte Erstellung von Bindungen ist nützlich, wenn Sie die Bindungseigenschaftswerte zur Laufzeit auswählen oder eine einzelne Bindung für mehrere Steuerelemente gemeinsam verwenden möchten. Beachten Sie jedoch, dass Sie die Bindungseigenschaftswerte nach dem Aufrufen von **SetBinding** nicht mehr ändern können.

Im folgenden Beispiel wird gezeigt, wie Sie eine Bindung im Code implementieren.

``` xml
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

``` vbnet
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

Vergleich der Features von {x:Bind} und {Binding}
------------------------------------------

| Feature | {x:Bind} | {Binding} | Hinweise |
|---------|----------|-----------|-------|
| „Path“ ist die Standardeigenschaft | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Path-Eigenschaft | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | In x:Bind ist Path standardmäßig an Page als Stamm gebunden, nicht DataContext. | 
| Indexer | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | Bindet an das angegebene Element in der Sammlung. Es werden nur ganzzahlbasierte Indizes unterstützt. | 
| Angefügte Eigenschaften | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | Angefügte Eigenschaften werden mit Klammern angegeben. Wenn die Eigenschaft nicht in einem XAML-Namespace deklariert wird, stellen Sie ihr einen XML-Namespace als Präfix voran, der einem Codenamespace am Anfang des Dokuments zugeordnet werden sollte. | 
| Umwandeln | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | Nicht erforderlich< | Umwandlungen werden mit Klammern angegeben. Wenn die Eigenschaft nicht in einem XAML-Namespace deklariert wird, stellen Sie ihr einen XML-Namespace als Präfix voran, der einem Codenamespace am Anfang des Dokuments zugeordnet werden sollte. | 
| Konverter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Konverter müssen im Stamm von Page/ResourceDictionary oder in „App.xaml“ deklariert werden. | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Konverter müssen im Stamm von Page/ResourceDictionary oder in „App.xaml“ deklariert werden. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | Wird verwendet, wenn das Blatt des Bindungsausdrucks NULL ist. Verwenden Sie zum Angeben eines Zeichenfolgenwerts einfache Anführungszeichen. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | Wird verwendet, wenn ein Teil des Pfads für die Bindung (mit Ausnahme des Blatts) NULL ist. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | Mit {x:Bind} nehmen Sie eine Bindung an ein Feld vor; Path ist standardmäßig an Page als Stamm gebunden, damit über dessen Feld auf jedes benannte Element zugegriffen werden kann. | 
| RelativeSource: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | With {x:Bind}, name the element and use its name in Path. | 
| RelativeSource: TemplatedParent | Not supported | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | In den meisten Fällen kann eine reguläre Vorlagenbindung in Steuerelementvorlagen verwendet werden. Verwenden Sie jedoch „TemplatedParent“, wenn Sie einen Konverter oder bidirektionale Bindungen verwenden müssen.< | 
| Source | Nicht unterstützt | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | For {x:Bind} use a property or a static path instead. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode can be OneTime, OneWay, or TwoWay. {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | Not supported | `<Binding UpdateSourceTrigger="[Default | PropertyChanged | Explicit]"/>` | {x:Bind} verwendet stets das PropertyChanged-Verhalten, ausgenommen für „TextBox.Text“. Hier wartet es auf den Verlust des Fokus wartet, um die Quelle zu aktualisieren. | 




<!--HONumber=Mar16_HO1-->


