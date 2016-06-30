---
author: jwmsft
description: "Die xBind-Markuperweiterung ist eine Alternative zur Bindung. xBind verfügt über weniger Features als die Bindungsfunktion, wird aber in kürzerer Zeit und mit weniger Arbeitsspeicher als die Bindungsfunktion ausgeführt und unterstützt ein besseres Debuggen."
title: xBind-Markuperweiterung
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: ceb5562ae08d7cc966f80fdb7e23f12afe040430

---

# {x:Bind}-Markuperweiterung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Hinweis** Allgemeine Informationen zur Verwendung der Datenbindung in Ihrer App mit **{x:Bind}** (sowie einen Gesamtvergleich von **{x:Bind}** und **{Binding}**) finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

Die **{x:Bind}**-Markuperweiterung – neu in Windows 10 – ist eine Alternative zu **{Binding}**. **{x:Bind}** verfügt über weniger Features als **{Binding}**, wird aber in kürzerer Zeit und mit weniger Arbeitsspeicher als **{Binding}** ausgeführt und unterstützt ein besseres Debuggen.

Zur XAML-Ladezeit wird **{x:Bind}** in eine Art von Binding-Objekt konvertiert, und dieses Objekt ruft einen Wert aus einer Eigenschaft für eine Datenquelle ab. Das Binding-Objekt kann optional konfiguriert werden, um Änderungen am Wert der Datenquelleneigenschaft zu beobachten und sich basierend auf diesen Änderungen zu aktualisieren. Es kann optional auch konfiguriert werden, um Änderungen am eigenen Wert per Push zurück an die Quelleigenschaft zu senden. Die von **{x:Bind}** und **{Binding}** erstellten Bindungsobjekte sind von der Funktionsweise her größtenteils identisch. **{x:Bind}** führt allerdings speziellen Code aus, der zur Kompilierzeit generiert wird, und **{Binding}** verwendet eine allgemeine Laufzeitobjektüberprüfung. Folglich bieten **{x:Bind}**-Bindungen (häufig als kompilierte Bindungen bezeichnet) eine hervorragende Leistung, stellen die Validierung Ihrer Bindungsausdrücke bei der Kompilierung bereit und unterstützen das Debuggen, indem Sie die Möglichkeit erhalten, Haltepunkte in den Codedateien festzulegen, die als Teilklasse für die Seite generiert werden. Diese Dateien befinden sich in Ihrem `obj`-Ordner und weisen Namen wie `<view name>.g.cs` auf (für C#).

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
| _propName_
            =
            _value_\[, _propName_=_value_\]* | Mindestens eine Bindungseigenschaft, die mithilfe einer Name-Wert-Paarsyntax angegeben wird. |
| _propName_ | Der Zeichenfolgenname der für das Binding-Objekt festzulegenden Eigenschaft. Beispiel: „Konverter“. | 
| _value_ | Der für die Eigenschaft festzulegende Wert. Die Syntax des Arguments hängt von der festgelegten Eigenschaft ab. Hier ein Beispiel für eine _propName_=_value_-Syntax, bei der der Wert selbst eine Markuperweiterung ist: `Converter={StaticResource myConverterClass}`. Weitere Informationen finden Sie unten im Abschnitt [Mit {x:Bind} festzulegende Eigenschaften](#properties-you-can-set). | 

## Eigenschaftspfad

*PropertyPath* legt **Path** für einen **{x:Bind}**-Ausdruck fest. **Path** ist ein Eigenschaftspfad, der den Wert der Eigenschaft, der Untereigenschaft, des Felds oder der Methode angibt, an die bzw. das Sie binden (die Quelle). Sie können den Namen der **Path**-Eigenschaft explizit erwähnen: `{Binding Path=...}`. Sie können diesen aber auch weglassen: `{Binding ...}`.

**{x:Bind}** verwendet nicht den **DataContext** als Standardquelle. Stattdessen wird die Seite oder das Benutzersteuerelement selbst verwendet. So sieht der CodeBehind-Abschnitt der Seite oder des Benutzersteuerelements für Eigenschaften, Felder und Methoden aus. Um das Ansichtsmodell für **{x:Bind}** verfügbar zu machen, sollten Sie in der Regel neue Felder oder Eigenschaften zum CodeBehind-Abschnitt der Seite oder des Benutzersteuerelements hinzufügen. Die Schritte in einem Eigenschaftspfad werden durch Punkte getrennt (.), und Sie können mehrere Trennzeichen angeben, um aufeinander folgende untergeordnete Eigenschaften zu durchlaufen. Verwenden Sie unabhängig von der verwendeten Programmiersprache einen Punkt als Trennzeichen, um das Objekt zu implementieren, an das die Bindung erfolgt.

So sucht **Text="{x:Bind Employee.FirstName}"** z. B. auf einer Seite nach einem **Employee**-Mitglied auf der Seite und dann nach einem **FirstName**-Mitglied für das von **Employee** zurückgegebene Objekt. Wenn Sie ein ItemsControl-Element an eine Eigenschaft binden, die die abhängigen Elemente des Mitarbeiters enthält, kann der Eigenschaftspfad „Employee.Dependents“ lauten, und die Elementvorlage des ItemsControl-Elements wäre für die Anzeige der Elemente „Dependents“ verantwortlich.

Für C++/CX kann **{x:Bind}** keine Bindungen an private Felder und Eigenschaften im Seiten- oder Datenmodell durchführen – Sie benötigen eine öffentliche Eigenschaft, damit die Bindung möglich ist. Der Oberflächenbereich für die Bindung muss als CX-Klassen/-Schnittstellen verfügbar gemacht werden, damit die relevanten Metadaten abgerufen werden können. Das **\[Bindable\]**-Attribut sollte nicht erforderlich sein.

Wenn die Datenquelle eine Auflistung ist, kann der Eigenschaftspfad Elemente in der Auflistung anhand ihrer Position oder ihres Indexes angeben. In „Teams\[0\].Players“ schließt das Literal „\[\]“ z. B. die „0“ ein, die das erste Element einer nullindizierten Collection anfordert.

Um einen Indexer verwenden zu können, muss das Modell **IList&lt;T&gt;** oder **IVector&lt;T&gt;** für den Typ der Eigenschaft implementieren, die indiziert werden soll. Wenn der Typ der indizierten Eigenschaft **INotifyCollectionChanged** oder **IObservableVector** unterstützt und die Bindung OneWay oder TwoWay ist, registriert und lauscht er auf Änderungsbenachrichtigungen an diesen Schnittstellen. Die Änderungserkennungslogik wird basierend auf allen Auflistungsänderungen aktualisiert, auch wenn sie keine Auswirkungen auf den entsprechenden indizierten Wert hat. Dies geschieht, da die Überwachungslogik für alle Instanzen der Auflistung identisch ist.

Um an angefügte Eigenschaften zu binden, müssen Sie den Klassen- und den Eigenschaftsnamen nach dem Punkt in Klammern setzen. Beispiel: **Text="{x:Bind Button22.(Grid.Row)}"**. Wenn die Eigenschaft nicht in einem Xaml-Namespace deklariert wird, müssen Sie ihr einen XML-Namespace als Präfix voranstellen, den Sie einem Codenamespace am Anfang des Dokuments zuordnen sollten.

Kompilierte Bindungen sind stark typisiert und lösen den Typ der einzelnen Schritte in einem Pfad auf. Wenn der zurückgegebene Typ nicht über den Member verfügt, erzeugt er zur Kompilierzeit einen Fehler. Sie können eine Umwandlung angeben, um der Bindung den tatsächlichen Typ des Objekts mitzuteilen. Im folgenden Fall ist **obj** eine Eigenschaft eines Typobjekts, enthält aber ein Textfeld, sodass wir **Text="{x:Bind obj.(TextBox.Text)}"** verwenden können.

Das **groups3**-Feld in **Text="{x:Bind groups3\[0\].(data:SampleDataGroup.Title)}"** ist ein Wörterbuch mit Objekten, das Sie zu **data:SampleDataGroup** umwandeln müssen. Beachten Sie die Verwendung des **data:**-Namespacepräfix für die Zuordnung des Objekttyps zu einem Namespace, der nicht Teil des Standard-XAML-Namespace ist.

Mit **x:Bind** müssen Sie **ElementName=xxx** nicht als Teil des Bindungsausdrucks verwenden. Mit **x:Bind** können Sie den Namen des Elements als ersten Teil des Pfads für die Bindung verwenden, da benannte Elemente innerhalb der Seite oder des Benutzersteuerelements, die bzw. das die Stammbindungsquelle darstellt, zu Feldern werden.

Ereignisbindung ist ein neues Feature für die kompilierte Bindung. Damit können Sie den Handler für ein Ereignis mit einer Bindung angeben. Es muss also keine Methode im CodeBehind-Abschnitt sein. Beispiel: **Click="{x:Bind rootFrame.GoForward}"**.

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

**Hinweis** Wenn Sie Markup von **{Binding}** in **{x:Bind}** konvertieren, beachten Sie die unterschiedlichen Standardwerte der **Mode**-Eigenschaft.
 
## Hinweise

Da **{x:Bind}** generierten Code für die optimale Nutzung verwendet, sind zur Kompilierzeit Typinformationen erforderlich. Dies bedeutet, dass Sie nur an Eigenschaften binden können, für die Sie den Typ vorab kennen. Aus diesem Grund können Sie **{x:Bind}** nicht mit der **DataContext**-Eigenschaft verwenden, die vom Typ **Object** ist und außerdem zur Laufzeit geändert werden kann.

Bei Verwendung von **{x:Bind}** mit Datenvorlagen müssen Sie den Typ angeben, an den gebunden wird, indem Sie einen **x:DataType**-Wert festlegen, wie im folgenden Beispiel gezeigt wird. Sie können den Typ auch auf eine Schnittstelle oder einen Basisklassentyp festlegen und dann ggf. Umwandlungen verwenden, um einen vollständigen Ausdruck zu formulieren.

Kompilierte Bindungen hängen von der Codegenerierung ab. Wenn Sie daher **{x:Bind}** in einem Ressourcenwörterbuch verwenden, muss das Ressourcenwörterbuch über eine CodeBehind-Klasse verfügen. Ein Codebeispiel finden Sie unter [Ressourcenwörterbücher mit {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) .

**Wichtig**   Wenn Sie einen lokalen Wert für eine Eigenschaft festlegen, die zuvor über eine **{x:Bind}**-Markuperweiterung zur Bereitstellung eines lokalen Werts verfügte, wird die Bindung vollständig entfernt.

**Tipp**   Wenn Sie eine einzelne geschweifte Klammer für einen Wert angeben müssen wie beispielsweise in [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) oder [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), stellen Sie ihr einen umgekehrten Schrägstrich voran: `\{`. Setzen Sie alternativ die gesamte Zeichenfolge mit den geschweiften Klammern, für die Escapezeichen verwendet werden müssen, in weitere Anführungszeichen, z. B. `ConverterParameter='{Mix}'`.

[
              **Converter**
            ](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) und **ConverterLanguage** hängen mit der Konvertierung eines Werts oder Typs aus der Bindungsquelle in einen mit der Bindungszieleigenschaft kompatiblen Typ oder Wert zusammen. Weitere Informationen und Beispiele finden Sie im Abschnitt „Datenkonvertierungen“ unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

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




<!--HONumber=Jun16_HO4-->


