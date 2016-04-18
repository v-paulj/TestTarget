---
description: Die Binding-Markuperweiterung wird beim Laden von XAML in eine Instanz der Binding-Klasse konvertiert.
title: {Binding}-Markuperweiterung
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
---

# {Binding}-Markuperweiterung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Hinweis**  Es steht ein neuer Bindungsmechanismus für Windows 10 zur Verfügung, der für Leistung und Entwicklerproduktivität optimiert wurde. Weitere Informationen finden Sie unter [{x:Bind}-Markuperweiterung](x-bind-markup-extension.md)

**Note**  Allgemeine Informationen zur Verwendung der Datenbindung in Ihrer App mit **{Binding}** (sowie einen Gesamtvergleich von **{x:Bind}** und **{Binding}**) finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

Beschreibung: Die **{Binding}**-Markuperweiterung wird beim Laden von XAML in eine Instanz der [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Klasse konvertiert. Dieses Binding-Objekt ruft einen Wert aus einer Eigenschaft für eine Datenquelle ab. Das Binding-Objekt kann optional konfiguriert werden, um Änderungen am Wert der Datenquelleneigenschaft zu beobachten und den eigenen Wert anhand dieser Änderungen zu aktualisieren. Es kann optional auch konfiguriert werden, um Änderungen am eigenen Wert per Push zurück an die Quelleigenschaft zu senden. Die als Ziel einer Datenbindung verwendete Eigenschaft muss eine Abhängigkeitseigenschaft sein. Weitere Informationen finden Sie unter [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md).

**{Binding}** weist die gleiche Rangfolge für Abhängigkeitseigenschaften auf wie ein lokaler Wert. So wird beim Festlegen eines lokalen Werts im imperativen Code der Effekt aller im Markup festgelegten **{Binding}**-Objekte entfernt.

**Beispiel-Apps zur Veranschaulichung von {Binding}**

-   Laden Sie die App [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950) herunter.
-   Laden Sie die App [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) herunter.

## XAML-Attributsyntax


``` syntax
<object property="{Binding}" .../>
-or-
<object property="{Binding propertyPath}" .../>
-or-
<object property="{Binding bindingProperties}" .../>
-or-
<object property="{Binding propertyPath, bindingProperties}" .../>
```

| Benennung | Beschreibung |
|------|-------------|
| *propertyPath* | Eine Zeichenfolge, die den Eigenschaftspfad für die Bindung angibt. Weitere Informationen finden Sie unten im Abschnitt [Eigenschaftspfad](#property-path). |
| *bindingProperties* | *propName*=*value*\[, *propName*=*value*\]*<br/>Eine oder mehrere Bindungseigenschaften, die mithilfe einer Name-/Wert-Paarsyntax angegeben werden. |
| *propName* | Der Zeichenfolgenname der für das [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Objekt festzulegenden Eigenschaft. Beispiel: „Konverter“. | 
| *value* | Der für die Eigenschaft festzulegende Wert. Die Syntax des Arguments hängt von der Eigenschaft der festgelegten Binding-Klasse ab. Weitere Informationen finden Sie unten im Abschnitt [Mit {Binding} festlegbare Eigenschaften der Bindungsklasse](#properties-of-binding). |

## Eigenschaftspfad

*PropertyPath* legt den Wert der [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)-Eigenschaft fest. Dies ist die Quelleigenschaft, an die Sie binden. Sie können den Eigenschaftsnamen auch explizit erwähnen: `{Binding Path=...}`. Sie können diesen aber auch weglassen: `{Binding ...}`.

Der [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)-Typ ist ein Eigenschaftspfad. Es handelt sich dabei um eine Zeichenfolge, die als eine Eigenschaft oder Untereigenschaft des benutzerdefinierten Typs oder eines anderen Frameworktyps betrachtet wird. Der Typ kann auch eine [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)-Klasse sein. Die Schritte in einem Eigenschaftspfad werden durch Punkte (.) getrennt, und Sie können mehrere Trennzeichen angeben, um aufeinanderfolgende untergeordnete Eigenschaften zu durchlaufen. Verwenden Sie unabhängig von der verwendeten Programmiersprache einen Punkt als Trennzeichen, um das Objekt zu implementieren, an das die Bindung erfolgt.

Zum Binden der Benutzeroberfläche an die Vornameneigenschaft eines Mitarbeiterobjekts können Sie z. B. „Employee.FirstName“ als Eigenschaftspfad verwenden. Wenn Sie ein ItemsControl-Element an eine Eigenschaft binden, die die abhängigen Elemente des Mitarbeiters enthält, kann der Eigenschaftspfad „Employee.Dependents“ lauten, und die Elementvorlage des ItemsControl-Elements wäre für die Anzeige der Elemente „Dependents“ verantwortlich.

Wenn die Datenquelle eine Auflistung ist, kann der Eigenschaftspfad Elemente in der Auflistung anhand ihrer Position oder ihres Indexes angeben. In „Teams[0].Players“ schließt das Literal „\[\]“ beispielsweise „0“ ein. 0 gibt das erste Element einer Auflistung an.

Wenn Sie eine [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828)-Eigenschaft an eine vorhandene [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)Klasse binden, können Sie angefügte Eigenschaften als Teil des Eigenschaftspfads verwenden. Wenn Sie eine angefügte Eigenschaft eindeutig machen möchten, damit der Zwischenpunkt im Namen der angefügten Eigenschaften nicht als Schritt in einen Eigenschaftspfad angesehen wird, müssen Sie Klammern um den besitzerqualifizierten Namen der angefügten Eigenschaft setzen – z. B. `(AutomationProperties.Name)`.

Es wird ein Zwischenobjekt des Eigenschaftspfads als [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)-Objekt in einer Laufzeitdarstellung gespeichert, in den meisten Fällen ist jedoch keine Interaktion mit einem **PropertyPath**-Objekt im Code erforderlich. Sie können i. d. R. die Bindungsinformationen angeben, wenn Sie XAML verwenden möchten.

Weitere Informationen zur Zeichenfolgensyntax für einen Eigenschaftspfad, zu Eigenschaftspfaden in Animationsfeaturebereichen und zum Erstellen eines [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)-Objekts finden Sie unter [Property-path syntax](property-path-syntax.md).

## Mit {Binding} festlegbare Eigenschaften der Bindungsklasse


**{Binding}** wird mit der *bindingProperties*-Platzhaltersyntax angegeben, da in der Markuperweiterung mehrere Lese-/Schreibeigenschaften einer [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Klasse festgelegt werden können. Die Eigenschaften können in beliebiger Reihenfolge mithilfe von durch Kommas getrennten Paaren *propName*=*value* angegeben. Für einige Eigenschaften sind Typen erforderlich, die keine Typkonvertierung enthalten, sodass für diese eigene innerhalb von **{Binding}** geschachtelte Markuperweiterungen erforderlich sind.

| Eigenschaft | Beschreibung |
|----------|-------------|
| [**Pfad**](https://msdn.microsoft.com/library/windows/apps/br209830) | Weitere Informationen finden Sie weiter oben im Abschnitt [Eigenschaftspfad](#property-path). |
| [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) | Gibt das Konverterobjekt an, das vom Bindungsmodul aufgerufen wird. Der Konverter kann in XAML festgelegt werden, sofern Sie dabei auf eine Objektinstanz verweisen, die Sie in einer [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) zugewiesen haben. Verweisen Sie anschließend im Ressourcenwörterbuch auf dieses Objekt. |
| [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) | Gibt die Kultur an, die vom Konverter verwendet werden soll. (Wenn Sie eine [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)-Eigenschaft festlegen.) Die Kultur kann als standardbasierter Bezeichner festgelegt werden. Weitere Informationen finden Sie unter **ConverterLanguage**. | 
| [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) | Gibt den Konverterparameter an, der in der Konverterlogik verwendet werden kann. (Wenn Sie eine [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)-Eingeschaft festlegen.) Die meisten Konverter verwenden einfache Logik und rufen alle erforderlichen Informationen aus dem zu konvertierenden übergebenen Wert ab. Sie benötigen keinen **ConverterParameter**-Wert. Der **ConverterParameter**-Parameter ist für etwas weiter fortgeschrittene Konverterimplementierungen mit mehr als einer Logik gedacht, die überprüft, was in **ConverterParameter** übergeben wird. Sie können auch einen Konverter schreiben, der keine Zeichenfolgenwerte verwendet. Dies ist jedoch sehr ungewöhnlich. Weitere Informationen finden Sie in den „Anmerkungen“ zu **ConverterParameter**. |
| [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) | Gibt eine Datenquelle durch Verweis auf ein anderes Element in demselben XAML-Konstrukt an, das über eine **Name**-Eigenschaft oder ein [x:Name-Attribut](x-name-attribute.md) verfügt. Damit werden häufig zusammengehörige Werte oder Untereigenschaften eines Benutzeroberflächenelements gemeinsam verwendet, um einen bestimmten Wert für ein anderes Element bereitzustellen, z. B. in einer XAML-Steuerelementvorlage. |
| [**FallbackValue**](https://msdn.microsoft.com/library/windows/apps/dn279345) | Gibt einen Wert an, der angezeigt wird, wenn die Quelle oder der Pfad nicht aufgelöst werden können. | 
| [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) | Gibt den Bindungsmodus als eine der folgenden Zeichenfolgen an: „OneTime“, „OneWay“ oder „TwoWay“. Diese Zeichenfolgen entsprechen den Konstantennamen der [**BindingMode**](https://msdn.microsoft.com/library/windows/apps/br209822)-Enumeration. Der Standardwert hängt vom Bindungsziel ab, in den meisten Fällen ist es jedoch „OneWay“. Beachten Sie, dass dieser vom Standard für **{x:Bind}** abweicht, der „OneTime“ lautet. | 
| [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) | Gibt durch das Beschreiben der Position der Bindungsquelle relativ zur Position des Bindungsziels eine Datenquelle an. Die Angabe erfolgt über das Laufzeitobjektdiagramm, indem z. B. das übergeordnete Objekt angegeben wird. Legen Sie die [{RelativeSource}-Markuperweiterung](relativesource-markup-extension.md) fest. |
| [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) | Gibt die Objektdatenquelle an. ##In der **Binding**-Markuperweiterung die [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946). |
| [**TargetNullValue**](https://msdn.microsoft.com/library/windows/apps/dn279347) | Gibt einen Wert an, der angezeigt wird, wenn der Quellwert aufgelöst werden kann, aber explizit **null** ist. |
| [**UpdateSourceTrigger**](https://msdn.microsoft.com/library/windows/apps/dn279350) | Gibt den Zeitpunkt für Aktualisierungen von Bindungsquellen an. Wenn keine Angabe erfolgt, lautet der Standardwert **Default**. |

**Hinweis**  Wenn Sie Markup von **{x:Bind}** in **{Binding}** konvertieren, beachten Sie die unterschiedlichen Standardwerte der **Mode**-Eigenschaft.

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) und **ConverterLanguage** hängen mit der Konvertierung eines Werts oder Typs aus der Bindungsquelle in einen mit der Bindungszieleigenschaft kompatiblen Typ oder Wert zusammen. Weitere Informationen und Beispiele finden Sie im Abschnitt „Datenkonvertierungen“ unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

[**Source**](https://msdn.microsoft.com/library/windows/apps/br209832), [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) und [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) geben eine Bindungsquelle an und schließen sich somit gegenseitig aus.

**Tipp**  Wenn Sie eine einzelne geschweifte Klammer für einen Wert angeben müssen wie beispielsweise in [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) or [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), stellen Sie ihr einen umgekehrten Schrägstrich voran: `\{`. Setzen Sie alternativ die gesamte Zeichenfolge mit den geschweiften Klammern, für die Escapezeichen verwendet werden müssen, in weitere Anführungszeichen, z. B. `ConverterParameter='{Mix}'`.

## Beispiele

```XAML
<!-- binding a UI element to a view model -->    
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>

    <GridView ItemsSource="{Binding BookSkus}" SelectedItem="{Binding SelectedBookSku, Mode=TwoWay}" ... />
</Page>
```

```XAML
<!-- binding a UI element to another UI element -->
<Page ... >
    <Page.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </Page.Resources>

    <Slider x:Name="sliderValueConverter" ... />
    <TextBox Text="{Binding Path=Value, ElementName=sliderValueConverter,
        Mode=OneWay,
        Converter={StaticResource GradeConverter}}"/> 
</Page>
```

Im zweiten Beispiel werden vier unterschiedliche [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Eigenschaften festgelegt: [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830), [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) und [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826). **Path** wird in diesem Fall explizit als **Binding**-Eigenschaft benannt gezeigt. Die **Path**-Eigenschaft wird zu einer Datenbindungsquelle ausgewertet, bei der es sich um ein anderes Objekt in derselben Laufzeit-Objektstruktur handelt – ein [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) mit dem Namen `sliderValueConverter`.

Beachten Sie, dass der [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)-Eigenschaftswert eine weitere Markuperweiterung ([{StaticResource} markup extension](staticresource-markup-extension.md)) verwendet, sodass hier zwei geschachtelte Markuperweiterungssyntaxen zu sehen sind. Die innere Syntax wird zuerst ausgewertet. Nach dem Abrufen der Ressource ist daher ein [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) verfügbar (eine benutzerdefinierte Klasse, die vom `local:S2Formatter`-Element in Ressourcen instanziiert wird), den die Bindung verwenden kann.

## Unterstützung von Tools

Mit Microsoft IntelliSense in Microsoft Visual Studio werden die Eigenschaften des Datenkontexts beim Erstellen von **{Binding}** im XAML-Markup-Editor angezeigt. Sobald Sie „{Binding“ eingeben, werden für die [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)-Eigenschaft geeignete Datenkontexteigenschaften in der Dropdownliste angezeigt. IntelliSense bietet auch Unterstützung für andere Eigenschaften von [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Damit dies funktioniert muss der Datenkontext oder der Entwurfszeit-Datenkontext auf der Markupseite festgelegt werden. **Gehe zu Definition** (F12) funktioniert auch bei **{Binding}**. Alternativ können Sie das Dialogfeld „Datenbindung“ verwenden.

 



<!--HONumber=Mar16_HO1-->


