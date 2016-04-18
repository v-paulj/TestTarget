---
Description: Der Umschalter stellt einen physischen Schalter dar, mit dem Benutzer Dinge ein- oder ausschalten können.
title: Richtlinien für Umschaltersteuerelemente
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Umschalter
template: detail.hbs
---
# Umschalter

Der Umschalter stellt einen physischen Schalter dar, mit dem Benutzer Dinge ein- oder ausschalten können. Mit **ToggleSwitch**-Steuerelementen können Sie Benutzern zwei Optionen anbieten, die sich gegenseitig ausschließen (z. B. Ein/Aus), wenn eine Option mit der Auswahl unmittelbar bestätigt wird.

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**ToggleSwitch-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.aspx)
-   [**IsOn-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.ison.aspx)
-   [**Toggled-Ereignis**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.toggled.aspx)

## Ist dies das richtige Steuerelement?

Verwenden Sie einen Umschalter für binäre Vorgänge, die wirksam werden, sobald der Benutzer den Umschalter umstellt. Verwenden Sie beispielsweise einen Umschalter, um Dienste oder Hardwarekomponenten wie WLAN ein- bzw. auszuschalten.

![WLAN-Umschalter ein- und ausgeschaltet](images/toggleswitches01.png)

Wenn ein physischer Schalter für die Aktion geeignet wäre, ist ein Umschalter wahrscheinlich das am besten geeignete Steuerelement.

Nachdem der Benutzer den Schalter ein- oder ausgeschaltet hat, sollte die entsprechende Aktion sofort ausgeführt werden.

### Wählen zwischen Umschalter und Kontrollkästchen

Für einige Aktionen kann entweder ein Umschalter oder ein Kontrollkästchen verwendet werden. Berücksichtigen Sie bei der Entscheidung zwischen den beiden Steuerelementen folgende Überlegungen:

-   Verwenden Sie einen Umschalter für binäre Einstellungen, wenn Änderungen direkt wirksam werden.

    ![Umschalter im Vergleich zum Kontrollkästchen](images/toggleswitches02.png)

    Im Beispiel oben ist durch den Umschalter klar, dass das WLAN aktiviert ist. Im Falle eines Kontrollkästchens muss der Benutzer erst überlegen, ob das WLAN derzeit aktiviert ist oder ob er das Kontrollkästchen aktivieren muss, um es zu aktivieren.

-   Verwenden Sie ein Kontrollkästchen, wenn der Benutzer zusätzliche Schritte ausführen muss, damit die Änderungen wirksam werden. Verwenden Sie beispielsweise ein Kontrollkästchen, wenn der Benutzer auf die Schaltfläche „Übermitteln“ oder „Weiter“ klicken muss, damit Änderungen übernommen werden.

    ![Ein Kontrollkästchen und eine Schaltfläche zum Senden](images/submitcheckbox.png)

-   Verwenden Sie Kontrollkästchen oder ein [Listenfeld](lists.md), wenn der Benutzer mehrere Elemente auswählen kann:

    ![Kontrollkästchen mit mehreren ausgewählten Elementen](images/guidelines_and_checklist_for_toggle_switches_checkbox_multi_select.png)

## Beispiele

Umschalter in den allgemeinen Einstellungen der Nachrichten-App.

![Umschalter in den allgemeinen Einstellungen der Nachrichten-App](images/control-examples/toggle-switch-news.png)

Umschalter in den Einstellungen für das Menü „Start” in Windows.

![Umschalter in den Einstellungen für das Menü „Start” in Windows](images/control-examples/toggle-switch-start-settings.png)

## Erstellen von Umschaltern

Hier sehen Sie, wie Sie einen einfachen Umschalter erstellen. Mit diesem XAML-Code wird der oben gezeigte WLAN-Umschalter erstellt.

```xaml
<ToggleSwitch x:Name="wiFiToggle" Header="Wifi"/>
```
An dieser Stelle wird beschrieben, wie derselbe Umschalter im Code erstellt wird.

```csharp
ToggleSwitch wiFiToggle = new ToggleSwitch();
wiFiToggle.Header = "WiFi";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(wiFiToggle);
```

### IsOn

Der Schalter kann entweder ein- oder ausgeschaltet sein. Verwenden Sie die [**IsOn**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.ison.aspx)-Eigenschaft, um den Zustand des Schalters zu ermitteln. Wenn der Schalter zur Steuerung des Zustands einer anderen binären Eigenschaft verwendet wird, können Sie wie hier gezeigt eine Bindung verwenden.

```
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### Toggled

In anderen Fällen können Sie das [**Toggled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.toggled.aspx)-Ereignis zur Reaktion auf Zustandsänderungen behandeln.

In diesem Beispiel wird veranschaulicht, wie in XAML und im Code ein Toggled-Ereignis hinzugefügt wird. Das Toggled-Ereignis wird behandelt, um einen Statusring ein- oder auszuschalten oder seine Sichtbarkeit zu ändern.

```xaml
<ToggleSwitch x:Name="toggleSwitch1" IsOn="True" 
              Toggled="ToggleSwitch_Toggled"/>
```

An dieser Stelle wird beschrieben, wie derselbe Umschalter im Code erstellt wird.

```csharp
// Create a new toggle switch and add a Toggled event handler.
ToggleSwitch toggleSwitch1 = new ToggleSwitch();
toggleSwitch1.Toggled += ToggleSwitch_Toggled;

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(toggleSwitch1);
```

Hier sehen Sie den Handler für das Toggled-Ereignis.

```csharp
private void ToggleSwitch_Toggled(object sender, RoutedEventArgs e)
{
    ToggleSwitch toggleSwitch = sender as ToggleSwitch;
    if (toggleSwitch != null)
    {
        if (toggleSwitch.IsOn == true)
        {
            progress1.IsActive = true;
            progress1.Visibility = Visibility.Visible;
        }
        else
        {
            progress1.IsActive = false;
            progress1.Visibility = Visibility.Collapsed;
        }
    }
}
```

### Beschriftungen „Ein”/„Aus”

Standardmäßig beinhaltet der Umschalter literalen Beschriftungen „Ein” und „Aus”, die automatisch lokalisiert werden. Sie können diese Beschriftungen durch Festlegen der Eigenschaften [**OnContent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.oncontent.aspx) und [**OffContent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.offcontent.aspx) ersetzen.

In diesem Beispiel werden die Beschriftungen „Ein”/„Aus” durch die Beschriftungen „Einblenden”/„Ausblenden” ersetzt.  

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide" 
              Toggled="ToggleSwitch_Toggled"/>
```

Sie können auch komplexeren Inhalt verwenden, indem Sie die Eigenschaften [**OnContentTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.oncontenttemplate.aspx) und [**OffContentTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.offcontenttemplate.aspx) verwenden.

## Empfehlungen

-   Ersetzen Sie die Beschriftungen „Ein“ und „Aus“, wenn es spezifischere Beschriftungen für die Einstellung gibt. Wenn Sie mit kurzen Beschriftungen (3 bis 4 Zeichen) einen besser geeigneten binären Gegensatz für eine bestimmte Einstellung darstellen können, verwenden Sie diese Beschriftungen. Wenn die Einstellung „Bilder anzeigen” lautet, könnten Sie beispielsweise „Einblenden”/„Ausblenden” verwenden. Die Verwendung spezifischerer Beschriftungen kann bei der Lokalisierung hilfreich sein.
-   Die Beschriftungen „Ein“ und „Aus“ sollten nur ersetzt werden, falls unbedingt nötig. Behalten Sie die Standardbeschriftungen bei, sofern keine benutzerdefinierten Beschriftungen erforderlich sind.
-   Beschriftungen sollten maximal 4 Zeichen lang sein.

## Verwandte Artikel

[**ToggleSwitch**](https://msdn.microsoft.com/library/windows/apps/hh701411)
- [Optionsfelder](radio-button.md)
- [Umschalter](toggles.md)
- [Kontrollkästchen](checkbox.md)

**Für Entwickler (XAML)**
- [**ToggleSwitch-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209712)


<!--HONumber=Mar16_HO1-->


