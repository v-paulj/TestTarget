---
author: Jwmsft
Description: "Dient zum Aktivieren oder Deaktivieren von Aktionselementen. Kann für einzelne oder mehrere Listenelemente verwendet werden."
title: "Kontrollkästchen"
ms.assetid: 6231A806-287D-43EE-BD8D-39D2FF761914
label: Check boxes
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: f565acbebbee8b8fb88a72970c9dbe3202ba24df

---
# Kontrollkästchen

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Ein Kontrollkästchen dient zum Aktivieren oder Deaktivieren von Aktionselementen. Es kann für einzelne oder mehrere Listenelemente verwendet werden, die dem Benutzer zur Auswahl stehen. Das Steuerelement verfügt über drei Auswahlzustände: „Deaktiviert“, „Aktiviert“ und „Unbestimmt“. Verwenden Sie den unbestimmten Zustand, wenn für eine Sammlung von Unteroptionen eine Mischung aus deaktivierten als aktivierten Zuständen vorliegt.

![Bespiel für Kontrollkästchenzustände](images/templates-checkbox-states-default.png)

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/br209316"><strong>CheckBox-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx"><strong>Checked-Ereignis</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx"><strong>IsChecked-Eigenschaft</strong></a> </li>
</ul>

</div>
</div>






## Ist dies das richtige Steuerelement?

Verwenden Sie ein **einzelnes Kontrollkästchen** für eine binäre Ja/Nein-Auswahl – beispielsweise für ein Anmeldeszenario vom Typ „E-Mail-Adresse speichern?” oder für Vertragsbedingungen.

![Ein einzelnes Kontrollkästchen für eine einzelne Auswahl](images/checkbox1.png)

Bei einer binären Auswahl besteht der Hauptunterschied zwischen einem **Kontrollkästchen** und einem [**Umschalter**](toggles.md) darin, dass das Kontrollkästchen für einen Zustand und der Umschalter für eine Aktion verwendet wird. Sie können das Commit einer Kontrollkästcheninteraktion (etwa im Rahmen der Übermittlung eines Formulars) verzögern, während für die Interaktion eines Umschalters sofort ein Commit ausgeführt werden muss. Eine Mehrfachauswahl ist nur mit Kontrollkästchen möglich.

Verwenden Sie **mehrere Kontrollkästchen** für Mehrfachauswahlszenarien, in denen der Benutzer einzelne oder mehrere Elemente aus einer Gruppe von Optionen auswählt, die sich nicht gegenseitig ausschließen.

Erstellen Sie eine Kontrollkästchengruppe, wenn die Benutzer eine beliebige Kombination von Optionen auswählen können.

![Auswählen mehrerer Optionen mit Kontrollkästchen](images/checkbox2.png)

Bei gruppierbaren Optionen kann die gesamte Gruppe durch ein unbestimmtes Kontrollkästchen dargestellt werden. Verwenden Sie den unbestimmten Zustand des Kontrollkästchens, wenn ein Benutzer nur einige untergeordneten Elemente der Gruppe aktiviert.

![Kontrollkästchen für die Anzeige einer gemischten Auswahl](images/checkbox3.png)

Benutzer können sowohl über **Kontrollkästchen** als auch über **Optionsfelder** eine Auswahl in einer Liste von Optionen treffen. Mit Kontrollkästchen können Benutzer eine Kombination von Optionen auswählen. Bei Optionsfeldern kann der Benutzer dagegen nur eine der (sich gegenseitig ausschließenden) Optionen auswählen. Wenn mehrere Optionen verfügbar sind, aber nur eine ausgewählt werden kann, verwenden Sie ein Optionsfeld.

## Beispiele

Ein Kontrollkästchen in einem Dialogfeld im MicrosoftEdge-Browser:

![Kontrollkästchen in einem Dialogfeld im MicrosoftEdge-Browser](images/control-examples/check-box-edge.png)

Kontrollkästchen in der Alarm&Uhr-App in Windows:

![Kontrollkästchen in der Alarm&Uhr-App in Windows](images/control-examples/check-box-alarm.png)

## Erstellen eines Kontrollkästchens

Um dem Kontrollkästchen eine Beschriftung zuzuweisen, legen Sie die [**Content**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx)-Eigenschaft fest. Die Beschriftung wird neben dem Kontrollkästchen angezeigt.

Der folgende XAML-Code erstellt ein einzelnes Kontrollkästchen, mit dem vor dem Übermitteln eines Formulars den Servicebedingungen zugestimmt werden muss: 

```xaml
<CheckBox x:Name="termsOfServiceCheckBox" 
          Content="I agree to the terms of service."/>
```

Hier sehen Sie das gleiche Kontrollkästchen im Code:

```csharp
CheckBox checkBox1 = new CheckBox();
checkBox1.Content = "I agree to the terms of service.";
```

### Binden an „IsChecked“

Mit der [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx)-Eigenschaft können Sie den Aktivierungszustand des Kontrollkästchens ermitteln. Der Wert der IsChecked-Eigenschaft kann an einen anderen binären Wert gebunden werden. Da es sich bei „IsChecked“ aber um einen booleschen Wert vom Typ [Nullable](https://msdn.microsoft.com/library/windows/apps/b3h38hb0.aspx) handelt, müssen Sie einen Wertkonverter verwenden, um sie an einen booleschen Wert zu binden.

In diesem Beispiel wird die **IsChecked**-Eigenschaft des Kontrollkästchens zum Akzeptieren der Servicebedingungen an die [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled.aspx)-Eigenschaft der Schaltfläche zum Absenden gebunden. Die Schaltfläche zum Absenden wird nur aktiviert, wenn die Vertragsbedingungen akzeptiert werden.

> Hinweis&nbsp;&nbsp;Wir zeigen hier nur den relevanten Code. Weitere Informationen zu Datenbindungen und Wertkonvertern finden Sie unter [Übersicht "Datenbindung"](../data-binding/data-binding-quickstart.md).

```xaml
...
<Page.Resources>
    <local:NullableBooleanToBooleanConverter x:Key="NullableBooleanToBooleanConverter"/>
</Page.Resources>

...

<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind termsOfServiceCheckBox.IsChecked, 
                        Converter={StaticResource NullableBooleanToBooleanConverter}, Mode=OneWay}"/>
</StackPanel>
```

```csharp
public class NullableBooleanToBooleanConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, string language)
    {
        if (value is bool?)
        {
            return (bool)value;
        }
        return false;
    }

    public object ConvertBack(object value, Type targetType, object parameter, string language)
    {
        if (value is bool)
            return (bool)value;
        return false;
    }
}
```

### Behandeln von Click- und Checked-Ereignissen

Wenn bei einer Änderung des Kontrollkästchenzustands eine Aktion ausgeführt werden soll, behandeln Sie das [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)-Ereignis oder die Ereignisse [**Checked**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx) und [**deaktiviert**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.unchecked.aspx). 

Das **Click**-Ereignis tritt bei jeder Änderung des Aktivierungszustands auf. Verwenden Sie beim Behandeln des Click-Ereignisses die **IsChecked**-Eigenschaft, um den Zustand des Kontrollkästchens zu ermitteln.

Die Ereignisse **Checked** und **Unchecked** treten unabhängig voneinander auf. Daher müssen immer beide Ereignisse behandelt werden, um auf Zustandsänderungen des Kontrollkästchens zu reagieren.

In den folgenden Beispielen wird die Behandlung des Click-Ereignis und der Ereignisse „Checked“ und „Unchecked“ gezeigt. 

Mehrere Kontrollkästchen können den gleichen Ereignishandler verwenden. Im folgenden Beispiel werden vier Kontrollkästchen zum Auswählen von Pizzabelägen erstellt. Die vier Kontrollkästchen verwenden den gleichen **Click**-Ereignishandler, um die Liste mit den gewählten Belägen zu aktualisieren.

```XAML
<StackPanel Margin="40">
    <TextBlock Text="Pizza Toppings"/>
    <CheckBox Content="Pepperoni" x:Name="pepperoniCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Beef" x:Name="beefCheckbox" 
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Mushrooms" x:Name="mushroomsCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Onions" x:Name="onionsCheckbox"
              Click="toppingsCheckbox_Click"/>

    <!-- Display the selected toppings. -->
    <TextBlock Text="Toppings selected:"/>
    <TextBlock x:Name="toppingsList"/>
</StackPanel>
```

Hier sehen Sie den Ereignishandler für das Click-Ereignis. Bei jedem Klick auf ein Kontrollkästchen wird der Aktivierungszustand der Kontrollkästchen überprüft und die Liste mit den gewählten Belägen aktualisiert.

```csharp
private void toppingsCheckbox_Click(object sender, RoutedEventArgs e)
{
    string selectedToppingsText = string.Empty;
    CheckBox[] checkboxes = new CheckBox[] { pepperoniCheckbox, beefCheckbox,
                                             mushroomsCheckbox, onionsCheckbox };
    foreach (CheckBox c in checkboxes)
    {
        if (c.IsChecked == true)
        {
            if (selectedToppingsText.Length > 1)
            {
                selectedToppingsText += ", ";
            }
            selectedToppingsText += c.Content;
        }
    }
    toppingsList.Text = selectedToppingsText;
}
```

### Verwenden des unbestimmten Zustands

Das CheckBox-Steuerelement erbt von [ToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.aspx) und kann über drei Zustände verfügen: 

Zustand | Eigenschaft | Wert
------|----------|------
Aktiviert | IsChecked | **true** 
Deaktiviert | IsChecked | **false** 
Unbestimmt | IsChecked | **NULL** 

Damit das Kontrollkästchen einen unbestimmten Zustand meldet, müssen Sie die [**IsThreeState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.isthreestate.aspx)-Eigenschaft auf **true** festlegen. 

Bei gruppierbaren Optionen kann die gesamte Gruppe durch ein unbestimmtes Kontrollkästchen dargestellt werden. Verwenden Sie den unbestimmten Zustand des Kontrollkästchens, wenn ein Benutzer nur einige untergeordneten Elemente der Gruppe aktiviert.

Im folgenden Beispiel ist die IsThreeState-Eigenschaft des Kontrollkästchens „Alle auswählen“ auf **true** festgelegt. Das Kontrollkästchen „Alle auswählen“ ist aktiviert, wenn alle untergeordneten Elemente ausgewählt sind, und deaktiviert, wenn alle untergeordneten Elemente deaktiviert sind. Andernfalls ist der Zustand unbestimmt.

```xaml
<StackPanel>
    <CheckBox x:Name="OptionsAllCheckBox" Content="Select all" IsThreeState="True" 
              Checked="SelectAll_Checked" Unchecked="SelectAll_Unchecked" 
              Indeterminate="SelectAll_Indeterminate"/>
    <CheckBox x:Name="Option1CheckBox" Content="Option 1" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
    <CheckBox x:Name="Option2CheckBox" Content="Option 2" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" IsChecked="True"/>
    <CheckBox x:Name="Option3CheckBox" Content="Option 3" Margin="24,0,0,0"
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
</StackPanel>
```

```csharp
private void Option_Checked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void Option_Unchecked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void SelectAll_Checked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = true;
}

private void SelectAll_Unchecked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = false;
}

private void SelectAll_Indeterminate(object sender, RoutedEventArgs e)
{
    // If the SelectAll box is checked (all options are selected), 
    // clicking the box will change it to its indeterminate state.
    // Instead, we want to uncheck all the boxes,
    // so we do this programatically. The indeterminate state should
    // only be set programatically, not by the user.

    if (Option1CheckBox.IsChecked == true &&
        Option2CheckBox.IsChecked == true &&
        Option3CheckBox.IsChecked == true)
    {
        // This will cause SelectAll_Unchecked to be executed, so
        // we don't need to uncheck the other boxes here.
        OptionsAllCheckBox.IsChecked = false;
    }
}

private void SetCheckedState()
{
    // Controls are null the first time this is called, so we just 
    // need to perform a null check on any one of the controls.
    if (Option1CheckBox != null)
    {
        if (Option1CheckBox.IsChecked == true &&
            Option2CheckBox.IsChecked == true &&
            Option3CheckBox.IsChecked == true)
        {
            OptionsAllCheckBox.IsChecked = true;
        }
        else if (Option1CheckBox.IsChecked == false &&
            Option2CheckBox.IsChecked == false &&
            Option3CheckBox.IsChecked == false)
        {
            OptionsAllCheckBox.IsChecked = false;
        }
        else
        {
            // Set third state (indeterminate) by setting IsChecked to null.
            OptionsAllCheckBox.IsChecked = null;
        }
    }
}
```

## Empfohlene und nicht empfohlene Vorgehensweisen

-   Stellen Sie sicher, dass der Zweck und der aktuelle Zustand des Kontrollkästchens eindeutig sind.
-   Geben Sie nicht mehr als zwei Zeilen für den Text von Kontrollkästchen an.
-   Formulieren Sie die Beschriftung des Kontrollkästchens als Aussage, die bei aktiviertem Kontrollkästchen wahr ist und bei deaktiviertem Kontrollkästchen falsch.
-   Verwenden Sie die Standardschriftart – es sei denn, Sie müssen gemäß Ihren Markenrichtlinien eine andere Schriftart verwenden.
-   Bei dynamischem Text müssen Sie die Skalierung des Steuerelements und die Auswirkungen auf visuelle Elemente in der Nähe bedenken.
-   Bei mindestens zwei Auswahloptionen, die sich gegenseitig ausschließen, können Sie die Verwendung von [Optionsfeldern](radio-button.md) in Erwägung ziehen.
-   Zwei Kontrollkästchengruppen sollten nicht direkt nebeneinander platziert werden. Verwenden Sie Gruppenbeschriftungen zum Trennen der Gruppen.
-   Verwenden Sie kein Kontrollkästchen als Ein/Aus-Steuerelement oder zum Ausführen eines Befehls. Verwenden Sie stattdessen einen Umschalter.
-   Verwenden Sie kein Kontrollkästchen, um andere Steuerelemente (etwa ein Dialogfeld) anzuzeigen.
-   Mit dem unbestimmten Zustand können Sie angeben, dass eine Option für einige, aber nicht für alle Unteroptionen festgelegt ist.
-   Geben Sie bei Verwendung des unbestimmten Zustands mithilfe untergeordneter Kontrollkästchen an, welche Optionen aktiviert sind und welche nicht. Gestalten Sie die Benutzeroberfläche so, dass der Benutzer die Unteroptionen sehen kann.
-   Verwenden Sie den unbestimmten Zustand nicht, um einen dritten Zustand darzustellen. Mit dem unbestimmten Zustand wird angezeigt, dass eine Option für einige, aber nicht für alle Unteroptionen gilt. Benutzer dürfen daher nicht in der Lage sein, einen unbestimmten Zustand direkt festzulegen. In diesem Negativbeispiel wird der unbestimmte Zustand eines Kontrollkästchens verwendet, um eine mittlere Schärfe anzugeben:

    ![Ein unbestimmtes Kontrollkästchen](images/checkbox4_spicy.png)

    Verwenden Sie stattdessen eine Optionsfeldgruppe mit drei Optionen: Nicht scharf, Scharf, Besonders scharf.

    ![Optionsfeldgruppe mit drei Optionen: Nicht scharf, Scharf, Besonders scharf](images/spicyoptions.png)


## Verwandte Artikel

-   [**CheckBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209316) 
-   [Optionsfelder](radio-button.md)
-   [Umschalter](toggles.md)





<!--HONumber=Aug16_HO3-->


