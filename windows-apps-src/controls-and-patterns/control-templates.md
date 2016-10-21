---
author: Jwmsft
Description: "Durch Erstellen einer Steuerelementvorlage im XAML-Framework können Sie die visuelle Struktur und das visuelle Verhalten eines Steuerelements anpassen."
MS-HAID: dev\_ctrl\_layout\_txt.control\_templates
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Steuerelementvorlagen
ms.assetid: 6E642626-A1D6-482F-9F7E-DBBA7A071DAD
label: Control templates
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 2aa257fa422ed954206dffb5ac68461e4e3a544f

---
# Steuerelementvorlagen

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/br209391"><strong>ControlTemplate-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.template.aspx"><strong>Control.Template-Eigenschaft</strong></a></li>
</ul>

</div>
</div>





Durch Erstellen einer Steuerelementvorlage im XAML-Framework können Sie die visuelle Struktur und das visuelle Verhalten eines Steuerelements anpassen. Steuerelemente besitzen zahlreiche Eigenschaften wie [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395), [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) und [**FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209404). Damit können Sie verschiedene Aspekte der Steuerelementdarstellung angeben. Die Anpassungen, die Sie mit diesen Eigenschaften vornehmen können, sind jedoch begrenzt. Weitere Anpassungen können Sie durch Erstellen einer Vorlage mit der [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)-Klasse angeben. Hier erfahren Sie, wie Sie eine **ControlTemplate** erstellen, um das Erscheinungsbild eines [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Steuerelements anzupassen.

## Beispiel für eine benutzerdefinierte Steuerelementvorlage


Standardmäßig wird der Inhalt eines [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Steuerelements (also die Zeichenfolge oder das Objekt neben dem **CheckBox**-Element) rechts neben dem Auswahlfeld platziert. Ein Häkchen gibt an, dass das **CheckBox**-Element von einem Benutzer aktiviert wurde. Diese Merkmale stellen die visuelle Struktur und das visuelle Verhalten des **CheckBox**-Elements dar.

Hier sehen Sie ein [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Element mit standardmäßiger [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)-Klasse und den Zuständen `Unchecked`, `Checked` und `Indeterminate`.

![Standardvorlage für CheckBox-Steuerelemente](images/templates-checkbox-states-default.png)

Sie können diese Merkmale ändern, indem Sie eine [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) für das [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Element erstellen. Ein Beispiel: Sie möchten den Inhalt des Kontrollkästchens unter dem Auswahlfeld platzieren und durch ein **X** angeben, dass das Kontrollkästchen vom Benutzer aktiviert wurde. Diese Merkmale geben Sie dann in der **ControlTemplate**-Klasse des **CheckBox**-Elements an.

Wenn Sie eine benutzerdefinierte Vorlage für ein Steuerelement verwenden möchten, weisen Sie die [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) der [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465)-Eigenschaft des Steuerelements zu. Hier sehen Sie ein [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Element mit der **ControlTemplate**-Klasse `CheckBoxTemplate1`. Den XAML (Extensible Application Markup Language)-Code für die **ControlTemplate** zeigen wir im nächsten Abschnitt.

```XAML
<CheckBox Content="CheckBox" Template="{StaticResource CheckBoxTemplate1}" IsThreeState="True" Margin="20"/>
```

So sieht dieses [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Element in den Zuständen `Unchecked`, `Checked`, and `Indeterminate` aus, nachdem die Vorlage angewendet wurde.

![Benutzerdefinierte Vorlage für CheckBox-Steuerelemente](images/templates-checkbox-states.png)

## Angeben der visuellen Struktur von Steuerelementen


Wenn Sie eine [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)-Klasse erstellen, kombinieren Sie [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Objekte, um ein einzelnes Steuerelement zu erstellen. Eine **ControlTemplate**-Klasse kann nur ein **FrameworkElement** als Stammelement besitzen. Das Stammelement enthält in der Regel weitere **FrameworkElement**-Objekte. Die Kombination aus Objekten ergibt die visuelle Struktur des Steuerelements.

Dieser XAML-Code erstellt eine [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)-Klasse für ein [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Element, die festlegt, dass sich das Steuerelement unter dem Auswahlfeld befinden soll. Das Stammelement ist eine [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250)-Klasse. Das Beispiel gibt einen [**Path**](https://msdn.microsoft.com/library/windows/apps/br243355)-Wert an, um das **X** zu erstellen, mit darauf hinweist, dass ein Benutzer das **CheckBox**-Element aktiviert hat. Eine [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/br243343)-Klasse kennzeichnet einen unbestimmten Zustand. Beachten Sie, dass [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br208962) für **Path** und **Ellipse** auf 0festgelegt ist, sodass beide standardmäßig nicht angezeigt werden.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}" 
            BorderThickness="{TemplateBinding BorderThickness}" 
            Background="{TemplateBinding Background}">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20" 
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}" 
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph" 
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z" 
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                  FlowDirection="LeftToRight" 
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph" 
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter" 
                              ContentTemplate="{TemplateBinding ContentTemplate}" 
                              Content="{TemplateBinding Content}" 
                              Margin="{TemplateBinding Padding}" Grid.Row="1" 
                              HorizontalAlignment="Center" 
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

## Angeben des visuellen Verhaltens von Steuerelementen


Das visuelle Verhalten gibt das Erscheinungsbild eines Steuerelements in verschiedenen Zuständen an. Das [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Steuerelement besitzt 3Aktivierungszustände: `Checked`, `Unchecked` und `Indeterminate`. Der Wert der [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798)-Eigenschaft bestimmt den Zustand des **CheckBox**, der wiederum festlegt, was im Feld angezeigt wird.

Diese Tabelle enthält die möglichen Werte von [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798), die zugehörigen Zustände von [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) und das Erscheinungsbild des **CheckBox**-Steuerelements.

|                     |                    |                         |
|---------------------|--------------------|-------------------------|
| **IsChecked**-Wert | **CheckBox**-Zustand | **CheckBox**-Darstellung |
| **true**            | `Checked`          | Enthält ein „X“.        |
| **false**           | `Unchecked`        | Leer.                  |
| **Null**            | `Indeterminate`    | Enthält einen Kreis.      |

 

Das Erscheinungsbild eines Steuerelements in verschiedenen Zuständen wird mithilfe von [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)-Objekten angegeben. Eine **VisualState**-Klasse enthält eine [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)- oder [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br243053)-Eigenschaft, mit der die Darstellung der Elementen in der [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)-Klasse geändert wird. Wenn das Steuerelement in den Zustand übergeht, den die [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031) angibt, werden die Änderungen in der **Setter**- oder [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)-Eigenschaft angewendet. Verlässt das Steuerelement den Zustand wieder, werden die Änderungen entfernt. **VisualState**-Objekte werden [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014)-Objekten hinzugefügt. **VisualStateGroup**-Objekte werden der angefügten [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505)-Eigenschaft hinzugefügt. Diesel legen Sie im [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Stammelement der **ControlTemplate**-Klasse fest.

Dieser XAML-Code zeigt die [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)-Objekte für die Zustände `Checked`, `Unchecked` und `Indeterminate`. Das Beispiel legt die angefügte [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505)-Eigenschaft in der [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250)-Klasse fest. Dies ist das Stammelement der [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)-Klasse. Der Zustand `Checked` der **VisualState**-Eigenschaft gibt an, dass die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br208962)-Eigenschaft des [**Path**](https://msdn.microsoft.com/library/windows/apps/br243355)-Werts namens `CheckGlyph` (den wir im vorherigen Beispiel gezeigt haben) auf1 gesetzt ist. Der Zustand `Indeterminate` der **VisualState**-Eigenschaft gibt an, dass die **Opacity**-Eigenschaft des [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/br243343)-Werts namens `IndeterminateGlyph` auf1 gesetzt ist. Da der Zustand `Unchecked` der **VisualState**-Eigenschaft keine [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)- oder [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)-Eigenschaft besitzt, kehrt [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) zur Standarddarstellung zurück.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}" 
            BorderThickness="{TemplateBinding BorderThickness}" 
            Background="{TemplateBinding Background}">
            
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CheckStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Target="CheckGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1" 
                         Storyboard.TargetName="CheckGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
                <VisualState x:Name="Unchecked"/>
                <VisualState x:Name="Indeterminate">
                    <VisualState.Setters>
                        <Setter Target="IndeterminateGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="IndeterminateGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20" 
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}" 
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph" 
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z" 
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                  FlowDirection="LeftToRight" 
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph" 
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter" 
                              ContentTemplate="{TemplateBinding ContentTemplate}" 
                              Content="{TemplateBinding Content}" 
                              Margin="{TemplateBinding Padding}" Grid.Row="1" 
                              HorizontalAlignment="Center" 
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

Zum besseren Verständnis der Funktionsweise von [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)-Objekten sollten Sie berücksichtigen, was passiert, wenn das [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) vom `Unchecked`-Zustand in den `Checked`-Zustand, dann in den `Indeterminate`-Zustand und anschließend zurück in den `Unchecked`-Zustand wechselt. Hier sehen Sie die Übergänge.

|                                      |                                                                                                                                                                                                                                                                                                                                                |                                                   |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| Zustandsübergänge                     | Folgendes passiert:                                                                                                                                                                                                                                                                                                                                   | CheckBox-Darstellung nach Abschluss des Übergangs |
| Von `Unchecked` in `Checked`.       | Der [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)-Wert des `Checked` [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) wird angewendet. Daher hat die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br208962)-Eigenschaft von `CheckGlyph` den Wert1.                                                                                                                                                         | Es wird ein „X“ angezeigt.                                |
| Von `Checked` in `Indeterminate`.   | Der [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)-Wert von `Indeterminate` [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) wird angewendet. Daher hat die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br208962)-Eigenschaft von `IndeterminateGlyph` den Wert1. Der **Setter**-Wert von `Checked` **VisualState** wird entfernt. Daher hat die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br228078)-Eigenschaft des `CheckGlyph`-Zustands den Wert0. | Ein Kreis wird angezeigt.                            |
| Von `Indeterminate` in `Unchecked`. | Der [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)-Wert von `Indeterminate` [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) wird entfernt. Daher hat die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br208962)-Eigenschaft des `IndeterminateGlyph`-Zustands den Wert0.                                                                                                                                           | Es wird nichts angezeigt.                             |

 
Weitere Informationen zum Erstellen visueller Zustände für Steuerelemente und insbesondere zum Verwenden der [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)-Klasse und der Animationstypen finden Sie unter [Storyboardanimationen für visuelle Zustände](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808).

## Verwenden Sie für die Arbeit mit Designs Tools.

Wenn Sie schnell Designs auf Ihre Steuerelemente anwenden möchten, klicken Sie in der Microsoft Visual Studio-**Dokumentgliederung** mit der rechten Maustaste auf ein Steuerelement und wählen **Design bearbeiten** oder **Stil bearbeiten** aus (je nach Steuerelement). Anschließend können Sie ein vorhandenes Design anwenden, indem Sie **Ressource übernehmen** auswählen, oder ein neues erstellen, indem Sie **Create Empty** auswählen.

## Steuerelemente und Barrierefreiheit

Beim Erstellen einer neuen Vorlage für ein Steuerelement können Sie zusätzlich zur eventuellen Änderung des Verhaltens und der visuellen Darstellung des Steuerelements auch die Art und Weise ändern, wie sich das Steuerelement selbst für Barrierefreiheits-Frameworks repräsentiert. Die Universelle Windows-Plattform (UWP) unterstützt das Benutzeroberflächenautomatisierungs-Framework von Microsoft für die Barrierefreiheit. Alle Standardsteuerelemente und zugehörigen Vorlagen bieten Unterstützung für allgemeine Steuerelementtypen zur Benutzeroberflächenautomatisierung und Muster, die für den Zweck und die Funktion des Steuerelements geeignet sind. Diese Steuerelementtypen und Muster werden von Benutzeroberflächenautomatisierungs-Clients interpretiert, beispielsweise von Hilfstechnologien, und dies ermöglicht die Zugriffsmöglichkeit auf ein Steuerelement als Bestandteil einer barrierefreien größeren App-UI.

Um die grundlegende Steuerlogik zu trennen und auch einige architekturbezogene Anforderungen der Benutzeroberflächenautomatisierung zu erfüllen, ist die Unterstützung der Barrierefreiheit von Steuerelementklassen in einer separaten Klasse in Form eines Automatisierungspeers enthalten. Die Automatisierungspeers interagieren mitunter mit den Steuerelementvorlagen, weil die Peers von der Existenz bestimmter benannter Teile in den Vorlagen ausgehen, sodass Funktionen wie die Aktivierung von Hilfstechnologien zum Aufrufen von Schaltflächenaktionen möglich sind.

Beim Erstellen eines vollständig neuen, benutzerdefinierten Steuerelements sollten Sie mitunter auch einen neuen Automatisierungspeer für das Steuerelement erstellen. Weitere Informationen finden Sie unter [Benutzerdefinierte Automatisierungspeers](../accessibility/custom-automation-peers.md).

## Weitere Informationen zur Standardvorlage eines Steuerelements

Die Themen, in denen die Stile und Vorlagen für XAML-Steuerelemente dokumentiert werden, enthalten Auszüge des XAML-Anfangscodes, der genutzt wird, wenn Sie die bereits erläuterten Verfahren zum **Bearbeiten von Designs** oder zum **Bearbeiten von Stilen** verwenden. In jedem Thema sind die Namen der Ansichtszustände, die Designressourcen und der vollständige XAML-Code für den Stil aufgeführt, der die Vorlage enthält. Die Themen können eine nützliche Hilfe darstellen, wenn Sie bereits mit der Änderung einer Vorlage begonnen haben und sehen möchten, wie die Vorlage im Originalzustand ausgesehen hat. Außerdem können Sie sicherstellen, dass die neue Vorlage über alle erforderlichen benannten Ansichtszustände verfügt.

## Designressourcen in Steuerelementvorlagen

Möglicherweise sind Ihnen bei einigen Attributen in den XAML-Beispielen Ressourcenverweise aufgefallen, für die die [{ThemeResource}-Markuperweiterung](../xaml-platform/themeresource-markup-extension.md) verwendet wird. Mit diesem Verfahren kann eine einzelne Steuerelementvorlage Ressourcen nutzen, bei denen es sich um unterschiedliche Werte handeln kann. Dies hängt davon ab, welches Design gerade aktiv ist. Besonders wichtig ist dies für Pinsel und Farben, da der Hauptzweck der Designs darin besteht, den Benutzern die Auswahl eines dunklen Designs, hellen Designs oder Designs mit hohem Kontrast zu ermöglichen, das auf das gesamte System angewendet wird. Apps, für die das XAML-Ressourcensystem verwendet wird, können einen für das jeweilige Design geeigneten Ressourcensatz nutzen. Die Designauswahl in der UI einer App spiegelt dann die systemweite Designauswahl des Benutzers wider.

**Hinweis**  
Dieser Artikel ist für Windows10-Entwickler gedacht, die UWP-Apps schreiben. Wenn Sie für Windows8.x oder Windows Phone8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 






<!--HONumber=Aug16_HO3-->


