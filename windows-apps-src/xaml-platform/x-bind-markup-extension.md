---
author: jwmsft
description: "Die xBind-Markuperweiterung ist eine Alternative zur Bindung. xBind verfügt über weniger Features als die Bindungsfunktion, wird aber in kürzerer Zeit und mit weniger Arbeitsspeicher als die Bindungsfunktion ausgeführt und unterstützt ein besseres Debuggen."
title: xBind-Markuperweiterung
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
translationtype: Human Translation
ms.sourcegitcommit: 0f9955b897c626e7f6abb5557658e1b1e5937ffd
ms.openlocfilehash: 7380386a77338c1fce7a7184b558a06605ffdf33

---

# {x:Bind}-Markuperweiterung

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Hinweis**  Allgemeine Informationen zur Verwendung der Datenbindung in Ihrer App mit **{x:Bind}** (sowie einen Gesamtvergleich von **{x:Bind}** und **{Binding}**) finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

Die **{x:Bind}**-Markuperweiterung – neu in Windows10 – ist eine Alternative zu **{Binding}**. **{x:Bind}** verfügt über weniger Features als **{Binding}**, wird aber in kürzerer Zeit und mit weniger Arbeitsspeicher als **{Binding}** ausgeführt und unterstützt das Debuggen besser.

Zur XAML-Kompilierungszeit wird **{X: Bind}** in Code konvertiert, der den Wert aus einer Eigenschaft aus einer Datenquelle abruft und diesen in der im Markup angegeben Eigenschaft festlegt. Das Binding-Objekt kann optional konfiguriert werden, um Änderungen am Wert der Datenquelleneigenschaft zu beobachten und sich basierend auf diesen Änderungen zu aktualisieren. Es kann optional auch konfiguriert werden, um Änderungen am eigenen Wert per Push zurück an die Quelleigenschaft zu senden. Die von **{x:Bind}** und **{Binding}** erstellten Bindungsobjekte sind von der Funktionsweise her größtenteils identisch. **{x:Bind}** führt allerdings speziellen Code aus, der zur Kompilierzeit generiert wird, und **{Binding}** verwendet eine allgemeine Laufzeitobjektüberprüfung. Folglich bieten **{x:Bind}**-Bindungen (häufig als kompilierte Bindungen bezeichnet) eine hervorragende Leistung, stellen die Validierung Ihrer Bindungsausdrücke bei der Kompilierung bereit und unterstützen das Debuggen, indem Sie die Möglichkeit erhalten, Haltepunkte in den Codedateien festzulegen, die als Teilklasse für die Seite generiert werden. Diese Dateien befinden sich in Ihrem `obj`-Ordner und weisen Namen wie `<view name>.g.cs` auf (für C#).

**Beispiel-Apps zur Veranschaulichung von {x:Bind}**

-   [{x:Bind}-Beispiel](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [Beispiel für XAML-UI-Grundlagen](http://go.microsoft.com/fwlink/p/?linkid=619992)

## XAML-Attributsyntax

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
```

| Benennung | Beschreibung |
|------|-------------|
| _propertyPath_ | Eine Zeichenfolge, die den Eigenschaftspfad für die Bindung angibt. Weitere Informationen finden Sie unten im Abschnitt [Eigenschaftspfad](#property-path). |
| _bindingProperties_ |
| _propName_=_value_\[, _propName_=_value_\]* | Mindestens eine Bindungseigenschaft, die mithilfe einer Name-Wert-Paarsyntax angegeben wird. |
| _propName_ | Der Zeichenfolgenname der für das Binding-Objekt festzulegenden Eigenschaft. Beispiel: „Konverter“. |
| _value_ | Der für die Eigenschaft festzulegende Wert. Die Syntax des Arguments hängt von der festgelegten Eigenschaft ab. Hier ein Beispiel für eine _propName_=_value_-Syntax, bei der der Wert selbst eine Markuperweiterung ist: `Converter={StaticResource myConverterClass}`. Weitere Informationen finden Sie unten im Abschnitt [Mit {x:Bind} festzulegende Eigenschaften](#properties-you-can-set). | 

## Eigenschaftspfad

*PropertyPath* legt **Path** für einen **{x:Bind}**-Ausdruck fest. **Path** ist ein Eigenschaftspfad, der den Wert der Eigenschaft, der Untereigenschaft, des Felds oder der Methode angibt, an die bzw. das Sie binden (die Quelle). Sie können den Namen der **Path**-Eigenschaft explizit erwähnen: `{Binding Path=...}`. Sie können diesen aber auch weglassen: `{Binding ...}`.

### Auflösung des Eigenschaftspfads

**{x:Bind}** verwendet nicht den **DataContext** als Standardquelle. Stattdessen wird die Seite oder das Benutzersteuerelement selbst verwendet. So sieht der CodeBehind-Abschnitt der Seite oder des Benutzersteuerelements für Eigenschaften, Felder und Methoden aus. Um das Ansichtsmodell für **{x:Bind}** verfügbar zu machen, sollten Sie in der Regel neue Felder oder Eigenschaften zum CodeBehind-Abschnitt der Seite oder des Benutzersteuerelements hinzufügen. Die Schritte in einem Eigenschaftspfad werden durch Punkte getrennt (.), und Sie können mehrere Trennzeichen angeben, um aufeinander folgende untergeordnete Eigenschaften zu durchlaufen. Verwenden Sie unabhängig von der verwendeten Programmiersprache einen Punkt als Trennzeichen, um das Objekt zu implementieren, an das die Bindung erfolgt.

So sucht **Text="{x:Bind Employee.FirstName}"** z. B. auf einer Seite nach einem **Employee**-Mitglied auf der Seite und dann nach einem **FirstName**-Mitglied für das von **Employee** zurückgegebene Objekt. Wenn Sie ein ItemsControl-Element an eine Eigenschaft binden, die die abhängigen Elemente des Mitarbeiters enthält, kann der Eigenschaftspfad „Employee.Dependents“ lauten, und die Elementvorlage des ItemsControl-Elements wäre für die Anzeige der Elemente „Dependents“ verantwortlich.

Für C++/CX kann **{x:Bind}** keine Bindungen an private Felder und Eigenschaften im Seiten- oder Datenmodell durchführen – Sie benötigen eine öffentliche Eigenschaft, damit die Bindung möglich ist. Der Oberflächenbereich für die Bindung muss als CX-Klassen/-Schnittstellen verfügbar gemacht werden, damit die relevanten Metadaten abgerufen werden können. Das **\[Bindable\]**-Attribut sollte nicht erforderlich sein.

Mit **x:Bind** müssen Sie **ElementName=xxx** nicht als Teil des Bindungsausdrucks verwenden. Mit **x:Bind** können Sie den Namen des Elements als ersten Teil des Pfads für die Bindung verwenden, da benannte Elemente innerhalb der Seite oder des Benutzersteuerelements, die bzw. das die Stammbindungsquelle darstellt, zu Feldern werden

### Sammlungen

Wenn die Datenquelle eine Auflistung ist, kann der Eigenschaftspfad Elemente in der Auflistung anhand ihrer Position oder ihres Indexes angeben. In „Teams\[0\].Players“ schließt das Literal „\[\]“ z.B. die „0“ ein, die das erste Element einer nullindizierten Collection anfordert.

Um einen Indexer verwenden zu können, muss das Modell **IList&lt;T&gt;** oder **IVector&lt;T&gt;** für den Typ der Eigenschaft implementieren, die indiziert werden soll. Wenn der Typ der indizierten Eigenschaft **INotifyCollectionChanged** oder **IObservableVector** unterstützt und die Bindung OneWay oder TwoWay ist, registriert und lauscht er auf Änderungsbenachrichtigungen an diesen Schnittstellen. Die Änderungserkennungslogik wird basierend auf allen Auflistungsänderungen aktualisiert, auch wenn sie keine Auswirkungen auf den entsprechenden indizierten Wert hat. Dies geschieht, da die Überwachungslogik für alle Instanzen der Auflistung identisch ist.

Wenn die Datenquelle ein Wörterbuch oder eine Karte ist, kann der Eigenschaftspfad Elemente in der Auflistung anhand ihres Zeichenfolgennamens angeben. **&lt;TextBlock Text = "{X: Bind Players\ ["John Smith"\]" /&gt;** sucht z.B. nach einem Element mit dem Namen „John Smith“ im Wörterbuch. Der Name muss in Anführungszeichen gesetzt werden. Dabei können einfache oder doppelte Anführungszeichen verwendet werden. Das Caret-Symbol (^) kann als Escapezeichen für Anführungszeichen innerhalb von Zeichenfolgen verwendet werden. Es ist normalerweise am einfachsten, Anführungszeichen zu verwenden, die von denen für das XAML-Attribut verwendeten abweichen.

Um einen Zeichenfolgen-Indexer verwenden zu können, muss das Modell **IDictionary&lt;string, T&gt;** oder **IMap&lt;string T&gt;** für den Typ der Eigenschaft implementieren, die indiziert werden soll. Wenn der Typ der indizierten Eigenschaft **IObservableMap** unterstützt und die Bindung OneWay oder TwoWay ist, wird er registriert und überwacht Benachrichtigungen auf diesen Schnittstellen. Die Änderungserkennungslogik wird basierend auf allen Auflistungsänderungen aktualisiert, auch wenn sie keine Auswirkungen auf den entsprechenden indizierten Wert hat. Dies geschieht, da die Überwachungslogik für alle Instanzen der Auflistung identisch ist.

### Angefügte Eigenschaften

Um an angefügte Eigenschaften zu binden, müssen Sie den Klassen- und den Eigenschaftsnamen nach dem Punkt in Klammern setzen. Beispiel: **Text="{x:Bind Button22.(Grid.Row)}"**. Wenn die Eigenschaft nicht in einem Xaml-Namespace deklariert wird, müssen Sie ihr einen XML-Namespace als Präfix voranstellen, den Sie einem Codenamespace am Anfang des Dokuments zuordnen sollten.

### Umwandlung

Kompilierte Bindungen sind stark typisiert und lösen den Typ der einzelnen Schritte in einem Pfad auf. Wenn der zurückgegebene Typ nicht über den Member verfügt, erzeugt er zur Kompilierzeit einen Fehler. Sie können eine Umwandlung angeben, um der Bindung den tatsächlichen Typ des Objekts mitzuteilen. Im folgenden Fall ist **obj** eine Eigenschaft eines Typobjekts, enthält aber ein Textfeld, sodass wir **Text="{x:Bind ((TextBox)obj).Text}"** oder **Text="{x:Bind obj.(TextBox.Text)}"**verwenden können.
Das **groups3**-Feld in **Text="{x:Bind ((data:SampleDataGroup)groups3\[0\]).Title}"** ist ein Wörterbuch mit Objekten, das Sie in **data:SampleDataGroup** umwandeln müssen. Beachten Sie die Verwendung des **data:**-XML-Namespacepräfix für die Zuordnung des Objekttyps zu einem Code-Namespace, der nicht Teil des Standard-XAML-Namespace ist.

_Hinweis: Die C#-Umwandlungssyntax ist flexibler als die Syntax der angefügten Eigenschaft und wird künftig als Syntax empfohlen._

## Funktionen in Bindungspfaden

Ab Windows10, Version 1607, unterstützt **{x: Bind}** die Verwendung einer Funktion als blattbildenden Schrittdes Bindungspfades. Dadurch wird Folgendes ermöglicht:
- Eine einfachere Möglichkeit der Konvertierung von Werten
- Eine Möglichkeit, Bindungen von mehr als einem Parameter abhängig zu machen

> [!NOTE]
> Wenn Sie Funktionen für **{x: Bind}** verwenden möchten, muss die Ziel-SDK-Version 14393 oder höher sein. Sie können keine Funktionen verwenden, wenn Ihre App für frühere Versionen von Windows10 bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

Im folgenden Beispiel werden Hintergrund und Vordergrund des Elements an Funktionen gebunden, um eine Konvertierung basierend auf dem Farbparameter durchzuführen

``` Xamlmarkup
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind Brushify(Color)}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```
``` C#
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public static SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}

```
### Funktionssyntax
``` Syntax
Text="{x:Bind MyModel.Order.CalculateShipping(MyModel.Order.Weight, MyModel.Order.ShipAddr.Zip, 'Contoso'), Mode=OneTime}"
             |      Path to function         |    Path argument   |       Path argument       | Const arg |  Bind Props
```

### Pfad der Funktion
Der Pfad der Funktion wird wie jeder andere Eigenschaftspfad angegeben. Er kann Punkte (.), Indexer oder Umwandlungen für die Suche nach der Funktion enthalten.

Statische Funktionen können mithilfe der XMLNamespace:ClassName.MethodName-Syntax angegeben werden. **&lt;CalendarDatePicker Datum = "\ {X: Bind sys:DateTime.Parse (TextBlock1.Text) \}" /&gt;** wird zB. der DateTime.Parse-Funktion zugeordnet, vorausgesetzt, am Kopf der Seite ist **xmlns:sys="using:System"** angegeben.

Ist der Modus OneWay/TwoWay, wird auf den Pfad der Funktion eine Änderungserkennung angewendet, und die Bindung wird neu ausgewertet, wenn diese Objekte geändert wurden.

Für die zu bindende Funktion müssen folgende Voraussetzungen gelten:
- Code und die Metadaten müssen auf sie zugreifen können, d.h. interne/private Aktionen in C#, aber für C++/CX werden öffentliche WinRT-Methoden benötigt
- Überladung basiert auf der Anzahl der Argumente, nicht auf ihrem Typ, und es wird die erste Übereinstimmung mit dieser Anzahl von Argumenten gesucht
- Die Argumenttypen müssen den übergebenen Daten entsprechen. Es werden keine einschränkenden Konvertierungen durchgeführt
- Der Rückgabetyp der Funktion muss mit dem Typ der Eigenschaft übereinstimmen, für die die Bindung verwendet wird


### Funktionsargumente
Mehrere Argumente können durch Komma (,) voneinander getrennt angegeben werden
- Bindungspfad – dieselbe Syntax wie bei einer direkten Bindung an das Objekt.
  - Ist der Modus OneWay/TwoWay, wird eine Änderungserkennung angewendet, und die Bindung wird neu ausgewertet, wenn diese Objekte geändert wurden.
- Konstante Zeichenfolge in Anführungszeichen – Anführungszeichen müssen gesetzt werden, um Zeichenfolgen kenntlich zu machen Das Caret-Symbol (^) kann als Escapezeichen für Anführungszeichen innerhalb von Zeichenfolgen verwendet werden.
- Konstante Zahl – z. B. -123.456
- Boolean – als "x: True" oder "x: False" angeben

### Bidirektionale Funktionsbindung
In einem Szenario mit bidirektionaler Bindung muss eine zweite Funktion für die umgekehrte Bindungsrichtung angegeben werden. Dies geschieht mithilfe der **BindBack**-Bindungseigenschaft, z.B. **Text = "\ {Text="\{x:Bind a.MyFunc(b), BindBack=a.MyFunc2\}"**. Die Funktion muss ein Argument übernehmen: den Wert, der vom Modell übernommen werden muss.

## Ereignisbindung

Ereignisbindung ist ein besonderes Feature der kompilierte Bindung. Damit können Sie den Handler für ein Ereignis mit einer Bindung angeben. Es muss also keine Methode im CodeBehind-Abschnitt sein. Beispiel: **Click="{x:Bind rootFrame.GoForward}"**.

Bei Ereignissen muss die Zielmethode nicht überladen werden und muss Folgendes erfüllen:

-   Sie muss mit der Signatur des Ereignisses übereinstimmen
-   ODER keine Parameter enthalten
-   ODER die gleiche Anzahl von Parametern von Typen enthalten, die alle von den Typen der Ereignisparameter zugeordnet werden können.

Im generierten CodeBehind-Abschnitt verarbeitet die kompilierte Bindung das Ereignis und leitet es an die Methode für das Modell weiter. Dabei wird der Pfad des Bindungsausdrucks ausgewertet, wenn das Ereignis eintritt. Dies bedeutet, dass – im Gegensatz zu Eigenschaftsbindungen – keine Änderungen am Modell nachverfolgt werden.

Weitere Informationen zur Zeichenfolgensyntax für einen Eigenschaftspfad finden Sie unter [Property-path-Syntax](property-path-syntax.md). Beachten Sie die Unterschiede, die hier für **{x:Bind}** beschrieben werden.

##  Mit {x:Bind} festzulegende Eigenschaften


**{x:Bind}** wird mit der *bindingProperties*-Platzhaltersyntax angegeben, da in der Markuperweiterung mehrere Lese-/Schreibeigenschaften festgelegt werden können. Die Eigenschaften können in beliebiger Reihenfolge mithilfe von durch Kommas getrennten Paaren *propName*=*value* angegeben werden. Beachten Sie, dass Sie in den Bindungsausdruck keine Zeilenumbrüche einschließen können. Für einige Eigenschaften sind Typen erforderlich, die keine Typkonvertierung enthalten, sodass für diese eigene innerhalb von **{x:Bind}** geschachtelte Markuperweiterungen erforderlich sind.

Diese Eigenschaften funktionieren ähnlich wie die Eigenschaften der [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Klasse.

| Eigenschaft | Beschreibung |
|----------|-------------|
| **Pfad** | Weitere Informationen finden Sie weiter oben im Abschnitt [Eigenschaftspfad](#property-path). |
| **Converter** | Gibt das Konverterobjekt an, das vom Bindungsmodul aufgerufen wird. Der Konverter kann in XAML festgelegt werden, sofern Sie dabei auf eine Objektinstanz verweisen, die Sie in einer [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) zugewiesen haben. Verweisen Sie anschließend im Ressourcenwörterbuch auf dieses Objekt. |
| **ConverterLanguage** | Gibt die Kultur an, die vom Konverter verwendet werden soll. (Wenn Sie **ConverterLanguage** festlegen, sollten Sie auch **Converter** festlegen.) Die Kultur kann als standardbasierter Bezeichner festgelegt werden. Weitere Informationen finden Sie unter [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880). |
| **ConverterParameter** | Gibt den Konverterparameter an, der in der Konverterlogik verwendet werden kann. (Wenn Sie **ConverterParameter** festlegen, sollten Sie auch **Converter** festlegen.) Die meisten Konverter verwenden einfache Logik und rufen alle erforderlichen Informationen aus dem zu konvertierenden übergebenen Wert ab. Sie benötigen keinen **ConverterParameter**-Wert. Der **ConverterParameter**-Parameter ist für etwas weiter fortgeschrittene Konverterimplementierungen mit mehr als einer Logik gedacht, die überprüft, was in **ConverterParameter** übergeben wird. Sie können auch einen Konverter schreiben, der keine Zeichenfolgenwerte verwendet. Dies ist jedoch sehr ungewöhnlich. Weitere Informationen finden Sie in den „Anmerkungen“ zu [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827). |
| **FallbackValue** | Gibt einen Wert an, der angezeigt wird, wenn die Quelle oder der Pfad nicht aufgelöst werden können. |
| **Mode** | Gibt den Bindungsmodus als eine der folgenden Zeichenfolgen an: „OneTime“, „OneWay“ oder „TwoWay“. Der Standard lautet "OneTime". Beachten Sie, dass dieser vom Standardwert für **{Binding}** abweicht, der in den meisten Fällen "OneWay" lautet. |
| **TargetNullValue** | Gibt einen Wert an, der angezeigt wird, wenn der Quellwert aufgelöst werden kann, aber explizit **null** ist. |
| **BindBack** | Gibt eine Funktion an, die für die umgekehrter Richtung einer bidirektionalen Bindung verwendet wird. | 

**Hinweis**  Wenn Sie Markup von **{Binding}** in **{x:Bind}** konvertieren, beachten Sie die unterschiedlichen Standardwerte der **Mode**-Eigenschaft.
 
## Hinweise

Da **{x:Bind}** generierten Code für die optimale Nutzung verwendet, sind zur Kompilierzeit Typinformationen erforderlich. Dies bedeutet, dass Sie nur an Eigenschaften binden können, für die Sie den Typ vorab kennen. Aus diesem Grund können Sie **{x:Bind}** nicht mit der **DataContext**-Eigenschaft verwenden, die vom Typ **Object** ist und außerdem zur Laufzeit geändert werden kann.

Bei Verwendung von **{x:Bind}** mit Datenvorlagen müssen Sie den Typ angeben, an den gebunden wird, indem Sie einen **x:DataType**-Wert festlegen, wie im folgenden Beispiel gezeigt wird. Sie können den Typ auch auf eine Schnittstelle oder einen Basisklassentyp festlegen und dann ggf. Umwandlungen verwenden, um einen vollständigen Ausdruck zu formulieren.

Kompilierte Bindungen hängen von der Codegenerierung ab. Wenn Sie daher **{x:Bind}** in einem Ressourcenwörterbuch verwenden, muss das Ressourcenwörterbuch über eine CodeBehind-Klasse verfügen. Ein Codebeispiel finden Sie unter [Ressourcenwörterbücher mit {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind).

Bei Seiten und Benutzersteuerelementen, die kompilierte Bindungen umfassen, befindet sich im generierten Code eine "Bindings"-Eigenschaft. Dazu gehören folgende Methoden:
- **Update()** - Hiermit werden die Werte aller kompilierten Bindungen aktualisiert. Für alle unidirektionalen und bidirektionalen Bindungen werden zur Erkennung von Änderungen Listener eingehängt.
- **Initialize()** - Wenn die Bindungen nicht bereits initialisiert wurden, wird zur Initialisierung der Bindungen Update() aufgerufen
- **StopTracking()** - Hängt die für uni- und bidirektionale Bindungen erstellen Listener aus. Sie können mit der Methode Update() erneut initialisiert werden.

> [!NOTE]
> Ab Windows10, Version1607, wird über das XAML-Framework ein integrierter Konverter für die Konvertierung eines booleschen Operanden in einen Sichtbarkeitszustand bereitgestellt. Der Konverter verknüpft **true** mit dem Enumerationswert **Visible** und **false** mit dem Wert **Collapsed**, sodass Sie eine Visibility-Eigenschaft an einen booleschen Wert binden können, ohne einen Konverter zu erstellen. Für die Verwendung des integrierten Konverters muss die SDK-Zielversion der App mindestens 14393 lauten. Die Verwendung ist nicht möglich, wenn Ihre App für frühere Versionen von Windows10 bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Tipp**   Wenn Sie eine einzelne geschweifte Klammer für einen Wert angeben müssen wie beispielsweise in [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) oder [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), stellen Sie ihr einen umgekehrten Schrägstrich voran: `\{`. Setzen Sie alternativ die gesamte Zeichenfolge mit den geschweiften Klammern, für die Escapezeichen verwendet werden müssen, in weitere Anführungszeichen, z.B. `ConverterParameter='{Mix}'`.

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) und **ConverterLanguage** hängen mit der Konvertierung eines Werts oder Typs aus der Bindungsquelle in einen mit der Bindungszieleigenschaft kompatiblen Typ oder Wert zusammen. Weitere Informationen und Beispiele finden Sie im Abschnitt „Datenkonvertierungen“ unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

**{x:Bind}** ist nur eine Markuperweiterung ohne Methode zum programmgesteuerten Erstellen oder Ändern dieser Bindungen. Weitere Informationen zu Markuperweiterungen finden Sie in der [XAML-Übersicht](xaml-overview.md).

## Beispiele

```XML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Dieser beispielhafte XAML-Code verwendet **{x:Bind}** mit einer **ListView.ItemTemplate**-Eigenschaft. Beachten Sie die Deklaration eines **x:DataType**-Werts.

```XML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```



<!--HONumber=Aug16_HO3-->


