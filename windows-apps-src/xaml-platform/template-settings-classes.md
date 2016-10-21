---
author: jwmsft
description: Vorlageneinstellungsklassen
title: Vorlageneinstellungsklassen
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: 8a52535e54a321bab6b34b6a73c53222e88d2151

---

# Vorlageneinstellungsklassen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Voraussetzungen

Es wird davon ausgegangen, dass Sie Ihrer Benutzeroberfläche Steuerelemente hinzufügen, deren Eigenschaften festlegen und Ereignishandler anfügen können. Anweisungen zum Hinzufügen von Steuerelementen zu Ihrer App finden Sie unter [Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen](https://msdn.microsoft.com/library/windows/apps/mt228345). Wir gehen auch davon aus, dass Sie mit der grundlegenden Definition einer benutzerdefinierten Vorlage für Steuerelemente durch Erstellen und Bearbeiten einer Kopie der Standardvorlage vertraut sind. Weitere Informationen dazu finden Sie unter [Schnellstart: Steuerelementvorlagen](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374).

## Das Szenario für **TemplateSettings**-Klassen

**TemplateSettings**-Klassen bieten eine Reihe von Eigenschaften, die bei der Definition einer neuen Steuerelementvorlage für ein Steuerelement verwendet werden. Die Eigenschaften haben Werte wie Pixelmaße für die Größe bestimmter Teile des UI-Elements. Bei diesen Werten handelt es sich manchmal um berechnete Werte, die aus der Steuerelementlogik stammen; sie zu überschreiben oder darauf zuzugreifen, ist in der Regel nicht einfach. Einige der Eigenschaften dienen als **From**- und **To**-Werte, die Übergänge und Animationen von Teilen steuern; daher treten die relevanten **TemplateSettings**-Eigenschaften paarweise auf.

Es gibt mehrere **TemplateSettings**-Klassen. Alle sind im [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818)-Namespace enthalten. Im Anschluss finden Sie eine Liste der Klassen und einen Link zur **TemplateSettings**-Eigenschaft des entsprechenden Steuerelements. Mit dieser **TemplateSettings**-Eigenschaft greifen Sie auf die **TemplateSettings**-Werte für das Steuerelement zu und können Vorlagenbindungen an seine Eigenschaften festlegen:

-   [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752): Wert von [**ComboBox.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209364)
-   [**GridViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738499): Wert von [**GridViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738503)
-   [**ListViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh701948): Wert von [**ListViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br242923)
-   [**ProgressBarTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227856): Wert von [**ProgressBar.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227537)
-   [**ProgressRingTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702248): Wert von [**ProgressRing.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702581)
-   [**SettingsFlyoutTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn298721): Wert von [**SettingsFlyout.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn252826)
-   [**ToggleSwitchTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209804): Wert von [**ToggleSwitch.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209731)
-   [**ToolTipTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209813): Wert von [**ToolTip.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227629)

**TemplateSettings**-Eigenschaften sind zur Verwendung in XAML und nicht im Code bestimmt. Es handelt sich dabei um schreibgeschützte Untereigenschaften einer schreibgeschützten **TemplateSettings**-Eigenschaft eines übergeordneten Steuerelements. Für ein Szenario mit erweiterten benutzerdefinierten Steuerelementen, in dem Sie eine neue [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390)-basierte Klasse erstellen und daher die Steuerelementlogik beeinflussen können, empfiehlt es sich, eine benutzerdefinierte **TemplateSettings**-Eigenschaft für das Steuerelement zu definieren, um hilfreiche Informationen für das Ändern der Vorlage eines Steuerelements zu kommunizieren. Als schreibgeschützten Wert dieser Eigenschaft definieren Sie eine neue **TemplateSettings**-Klasse im Zusammenhang mit Ihrem Steuerelement, das schreibgeschützte Eigenschaften für alle Informationselemente aufweist, die für Vorlagenmaße, Animationspositionierung usw. relevant sind, und geben Aufrufern die Runtime-Instanz dieser Klasse, die mit Ihrer Steuerelementlogik initialisiert wird. **TemplateSettings**-Klassen werden von [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) abgeleitet, sodass die Eigenschaften das Abhängigkeitseigenschaftensystem für Rückrufe für Eigenschaftsänderungen verwenden können. Die Bezeichner von Abhängigkeitseigenschaften für die Eigenschaften sind jedoch nicht als öffentliche API verfügbar, da die **TemplateSettings**-Eigenschaften für Aufrufer schreibgeschützt sein sollen.

## Verwendung von **TemplateSettings** in einer Steuerelementvorlage

Hier sehen Sie ein Beispiel aus den ersten XAML-Standardvorlagen für Steuerelemente. Dieses Beispiel stammt aus der Standardvorlage von [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538):

```xml
<Ellipse
    x:Name="E1"
    Style="{StaticResource ProgressRingEllipseStyle}"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

Der vollständige XAML-Code für die [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)-Vorlage umfasst mehrere hundert Zeilen, hierbei handelt es sich also nur um einen sehr kleinen Auszug. Dieser XAML-Code definiert einen Teil des Steuerelements, das als eines von sechs [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/br243343)-Elementen die sich drehende Animation für einen unbestimmten Fortschritt darstellt. Ihnen als Entwickler gefallen die Kreise möglicherweise nicht, und Sie möchten einen anderen Grafikgrundtyp oder eine andere grundlegende Form für den Animationsverlauf verwenden. Sie können stattdessen z.B. ein **ProgressRing**-Element erstellen, das eine Reihe von in einem Quadrat angeordneten [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371)-Elementen enthält. Dabei kann jede einzelne **Rectangle**-Komponente Ihrer neuen Vorlage wie folgt aussehen:

```xml
<Rectangle
    x:Name="R1"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

Die **TemplateSettings**-Eigenschaften eignen sich hier gut, da es sich dabei um berechnete Werte aus der grundlegenden Steuerlogik von [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) handelt. Die Berechnung unterteilt das gesamte [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)- und [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)-Element des **ProgressRing**-Elements und weist eine berechnete Messung für jedes Bewegungselemente in den Vorlagen zu, sodass die Vorlagenteile die Größe an den Inhalt anpassen können.

Hier sehen Sie ein weiteres Beispiel für die Verwendung von standardmäßigen XAML-Steuerelementvorlagen, diesmal mit einem der Eigenschaftensätze, die die **From**- und **To**-Elemente einer Animation darstellen. Dieses Beispiel stammt aus der [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348)-Standardvorlage:

```xml
<VisualStateGroup x:Name="DropDownStates">
    <VisualState x:Name="Opened">
        <Storyboard>
            <SplitOpenThemeAnimation
               OpenedTargetName="PopupBorder"
               ContentTargetName="ScrollViewer"
               ClosedTargetName="ContentPresenter"
               ContentTranslationOffset="0"
               OffsetFromCenter="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOffset}"
               OpenedLength="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOpenedHeight}"
               ClosedLength="{Binding RelativeSource={RelativeSource TemplatedParent},
                 Path=TemplateSettings.DropDownClosedHeight}" />
        </Storyboard>
   </VisualState>
...
</VisualStateGroup>
```

Da auch diese Vorlage viel XAML-Code enthält, zeigen wir wieder nur einen Auszug. Dies ist nur ein Beispiel für die Zustände und Designanimationen, die dieselben [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752)-Eigenschaften verwenden. Für [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) erzwingt die Verwendung der **ComboBoxTemplateSettings**-Werte durch Bindungen, dass verwandte Animationen in der Vorlage beendet werden und an Positionen beginnen, die auf gemeinsamen Werten basieren. Dadurch ist der Übergang nahtlos.

**Hinweis**  
Wenn Sie **TemplateSettings**-Werte als Teil Ihrer Steuerelementvorlage verwenden, achten Sie darauf, Eigenschaften festzulegen, die dem Typ des Werts entsprechen. Andernfalls müssen Sie u.U. einen Wertkonverter für die Bindung erstellen, damit der Zieltyp der Bindung von einem anderen Quelltyp des **TemplateSettings**-Werts konvertiert werden kann. Weitere Informationen finden Sie unter [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903).

## Verwandte Themen

* [Schnellstart: Steuerelementvorlagen](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)




<!--HONumber=Aug16_HO3-->


