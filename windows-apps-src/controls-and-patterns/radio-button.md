---
author: Jwmsft
Description: "Mit Optionsfeldern können Benutzer eine oder mehrere Optionen auswählen."
title: "Richtlinien für Optionsfelder"
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 435a2a6f1b9707d1f64587a693bd9a60d587ca83

---
# Optionsfelder

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Mit Optionsfeldern können Benutzer eine oder mehrere Optionen auswählen. Jede Option wird durch ein Optionsfeld dargestellt. Benutzer können nur ein einziges Optionsfeld in einer Gruppe von Optionsfeldern auswählen.

(Falls Sie sich über die englische Bezeichnung „Radio Button” wundern: Optionsfelder sind im Englischen nach den Tasten mit voreingestellten Sendern an einem Radio benannt.)

![Optionsfelder](images/controls/radio-button.png)

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/br227544"><strong>RadioButton-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx"><strong>Checked-Ereignis</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx"><strong>IsChecked-Eigenschaft</strong></a></li>
</ul>

</div>
</div>






## Ist dies das richtige Steuerelement?

Verwenden Sie Optionsfelder, um den Benutzern zwei oder mehr Optionen bereitzustellen, die sich gegenseitig ausschließen, wie hier gezeigt.

![Eine Gruppe von Optionsfeldern](images/radiobutton_basic.png)

Optionsfelder verleihen wichtigen Optionen in Ihrer App Klarheit und Gewichtung. Verwenden Sie Optionsfelder, wenn die angezeigten Optionen wichtig genug sind, um mehr Bildschirmplatz in Anspruch zu nehmen, und in den Fällen, wenn die Klarheit der Auswahl äußerst explizite Optionen erfordert.

Optionsfelder heben alle Optionen gleichermaßen hervor, sodass Benutzer den Optionen möglicherweise mehr Aufmerksamkeit schenken, als eigentlich erforderlich ist. Ziehen Sie in Betracht, andere Steuerelemente zu verwenden, es sei denn, die Optionen erfordern zusätzliche Aufmerksamkeit seitens des Benutzers. Verwenden Sie beispielsweise demgegenüber eine [Dropdownliste](lists.md), wenn die standardmäßige Option für die meisten Benutzer in den meisten Situationen empfohlen ist.

Wenn nur zwei sich gegenseitig ausschließende Optionen vorhanden sind, kombinieren Sie sie in einem einzelnen [Kontrollkästchen](checkbox.md) oder [Umschalter](toggles.md). Verwenden Sie beispielsweise ein Kontrollkästchen für "Ich stimme zu" anstelle von zwei Optionsfeldern für "Ich stimme zu" und "Ich stimme nicht zu".

![Zwei Möglichkeiten für die Darstellung einer binären Auswahl](images/radiobutton_vs_checkbox.png)

Wenn der Benutzer mehrere Optionen auswählen kann, verwenden Sie stattdessen ein [Kontrollkästchen](checkbox.md) oder [Listenfeldsteuerelement](lists.md).

![Auswählen mehrerer Optionen mit Kontrollkästchen](images/checkbox2.png)

Verwenden Sie Optionsfelder nicht, wenn es sich bei den Optionen um Zahlen mit festen Schritten handelt, beispielsweise 10, 20, 30. Verwenden Sie stattdessen ein [Schiebereglersteuerelement](slider.md).

Wenn mehr als acht Optionen vorhanden sind, verwenden Sie stattdessen eine [Dropdownliste](lists.md), ein [Listenfeld](lists.md) für die Einfachauswahl oder eine [Listenfeld](lists.md).

Wenn die verfügbaren Optionen auf dem aktuellen Kontext der App basieren oder andernfalls dynamisch variieren können, verwenden Sie stattdessen ein [Listenfeld](lists.md) für die Einfachauswahl.

## Beispiel
Optionsfelder in den Microsoft Edge-Browsereinstellungen.

![Optionsfelder in den Microsoft Edge-Browsereinstellungen](images/control-examples/radio-buttons-edge.png)

## Erstellen eines Optionsfelds

Optionsfelder funktionieren in Gruppen. Es gibt zwei Arten zur Gruppierung von Optionsfeld-Steuerelementen:
- Platzieren Sie sie im gleichen übergeordneten Container.
- Legen Sie die [**GroupName**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.radiobutton.groupname.aspx)-Eigenschaft für jedes Optionsfeld auf denselben Wert fest.

> **Hinweis**
            &nbsp;&nbsp;Eine Gruppe von Optionsfeldern verhält sich wie ein einzelnes Steuerelement, wenn der Zugriff darauf über die Tastatur erfolgt. Der Benutzer kann mit der TAB-TASTE nur auf die jeweils ausgewählte Option zugreifen, mithilfe der Pfeiltasten kann der Benutzer jedoch in der Gruppe navigieren.

In diesem Beispiel wird die erste Gruppe von Optionsfeldern implizit gruppiert, da die Felder im selben StackPanel-Element enthalten sind. Die zweite Gruppe ist auf zwei StackPanel-Elemente aufgeteilt, damit sie explizit nach GroupName gruppiert werden.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

Die Optionsfeldgruppen sehen wie folgt aus.

![Optionsfelder in zwei Gruppen](images/radio-button-groups.png)

Ein Optionsfeld hat zwei Zustände: *aktiviert* und *deaktiviert*. Wenn ein Optionsfeld aktiviert ist, lautet die [ **IsChecked** ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx)- Eigenschaft **true**. Wenn ein Optionsfeld deaktiviert ist, lautet die **IsChecked**-Eigenschaft **false**. Ein Optionsfeld kann durch Klicken auf ein anderes Optionsfeld in derselben Gruppe deaktiviert werden, jedoch nicht durch erneutes Klicken auf das Optionsfeld selbst. Sie können ein Optionsfeld jedoch programmgesteuert durch Festlegen der IsChecked-Eigenschaft auf **false** deaktivieren.

## Empfehlungen

-   Stellen Sie sicher, dass der Zweck und der aktuelle Status eines Satzes an Optionsfeldern nachvollziehbar ist.
-   Sorgen Sie immer für ein visuelles Feedback, wenn der Benutzer auf ein Optionsfeld tippt.
-   Sorgen Sie für visuelles Feedback, sobald der Benutzer mit Optionsfeldern interagiert. "Normal", "pressed" "checked" und "disabled" sind Beispiele für Optionsfeldstatus. Ein Benutzer tippt auf ein Optionsfeld, um die zugehörige Option zu aktivieren. Durch das Tippen auf eine aktivierte Option wird sie nicht deaktiviert, aber das Tippen auf eine andere Option überträgt die Aktivierung auf diese Option.
-   Reservieren Sie visuelle Effekte und Animationen für Berührungsfeedback und für den aktivierten Status. Im nicht aktivierten Status sollten Optionsfeldsteuerelemente als nicht verwendet oder inaktiv (aber nicht deaktiviert) angezeigt werden.
-   Begrenzen Sie den Text des Optionsfelds auf eine einzelne Zeile. Sie können die visuellen Effekte des Optionsfelds anpassen, um eine Beschreibung der Option in kleinerer Schriftgröße unter dem Haupttext anzuzeigen.
-   Wenn der Textinhalt dynamisch ist, bedenken Sie die Größenänderung der Schaltfläche und die visuellen Effekte herum.
-   Verwenden Sie die standardmäßige Schriftart, es sei denn, Sie müssen gemäß Ihren Markenrichtlinien eine andere verwenden.
-   Schließen Sie das Optionsfeld in ein Bezeichnungselement ein, sodass das Optionsfeld durch Tippen auf die Bezeichnung ausgewählt wird.
-   Platzieren Sie den Bezeichnungstext hinter dem Optionsfeldsteuerelement und nicht davor oder darüber.
-   Ziehen Sie in Erwägung, Ihre Schaltflächen anzupassen. Standardmäßig besteht ein Optionsfeld aus zwei konzentrischen Kreisen– der innere ist ausgefüllt (und wird gezeigt, wenn das Optionsfeld aktiviert wird), und der äußere ist gestrichelt– und einigem Textinhalt. Wir ermutigen Sie jedoch, kreativ zu sein. Benutzer mögen es, direkt mit dem Inhalt einer App zu interagieren. Daher können Sie auswählen, den tatsächlichen Inhalt als Angebot anzuzeigen, unabhängig davon, ob er mit Grafiken oder als unauffälliger Textumschalter präsentiert wird.
-   Eine Optionsfeldgruppe sollte nicht mehr als acht Optionen beinhalten. Wenn Sie mehr Optionen verwenden müssen, verwenden Sie stattdessen eine [Dropdownliste](lists.md), ein [Listenfeld](lists.md)oder eine [Listenansicht](lists.md).
-   Zwei Optionsfeldgruppen sollten nicht direkt nebeneinander platziert werden. Wenn sich zwei Optionsfeldgruppen direkt nebeneinander befinden, ist es schwierig, festzustellen, welche Schaltflächen zu welcher Gruppe gehören. Verwenden Sie Gruppenbeschriftungen, um sie zu trennen.

## Weitere Hinweise zur Verwendung

Diese Abbildung zeigt die richtige Vorgehensweise zum Platzieren und Anordnen von Optionsfeldern in geeignetem Abstand.

![Gruppe von Optionsfeldern](images/radiobutton_layout1.png)
## Verwandte Themen

**Für Designer**
- [Richtlinien für Schaltflächen](buttons.md)
- [Richtlinien für Umschalter](toggles.md)
- [Richtlinien für Kontrollkästchen](checkbox.md)
- [Richtlinien für Dropdownlisten](lists.md)
- [Richtlinien für Listenansicht- und Rasteransichtsteuerelemente](lists.md)
- [Richtlinien für Schieberegler](slider.md)
- [Richtlinien für das Auswahlsteuerelement](lists.md)


**Für Entwickler (XAML)**
- [**Windows.UI.Xaml.Controls RadioButton-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227544)



<!--HONumber=Aug16_HO3-->


