---
Description: Layoutpanels dienen zum Anordnen und Gruppieren von UI-Elementen in Ihrer App.
title: Layoutpanels für Universelle-Windows-Plattform-(UWP)-Apps
ms.assetid: 07A7E022-EEE9-4C81-AF07-F80868665994
label: Layoutpanels
template: detail.hbs
---
# Layoutpanels

Layoutpanels dienen zum Anordnen und Gruppieren von UI-Elementen in Ihrer App. Zu den integrierten XAML-Layoutpanels gehören [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx), [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx), [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx), [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) und [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx). Hier werden die einzelnen Panels beschrieben und es wird erläutert, wie Sie sie zum Layouten von XAML-UI-Elementen einsetzen.

Bei der Wahl eines Layoutpanels müssen mehrere Dinge berücksichtigt werden:
- Wie das Panel seine untergeordneten Elemente positioniert
- Wie das Panel die Größe seiner untergeordneten Elemente bestimmt
- Wie sich überlappende untergeordnete Elemente übereinander geschichtet werden (Z-Reihenfolge)
- Anzahl und Komplexität geschachtelter Panelelemente, die erforderlich sind, um das gewünschte Layout zu erstellen


**Angefügte Paneleigenschaften**

Die meisten XAML-Layoutpanels verwenden angefügte Eigenschaften, mit denen die untergeordneten Elemente das übergeordnete Panel darüber informieren, wie sie in der Benutzeroberfläche positioniert werden sollen. Angefügte Eigenschaften verwenden die Syntax *AttachedPropertyProvider.PropertyName*. Wenn in anderen Panels geschachtelte Panels vorhanden sind, werden angefügte Eigenschaften von UI-Elementen, die Layoutmerkmale für ein übergeordnetes Element angeben, nur vom direkt übergeordneten Panel interpretiert.

Hier sehen Sie ein Beispiel für das Festlegen der angefügten Eigenschaft [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.left.aspx) auf einem Schaltflächen-Steuerelement in XAML. Hiermit wird das übergeordnete Canvas-Element darüber informiert, dass das Button-Element 50 effektive Pixel vom linken Rand des Canvas-Elements entfernt positioniert werden soll.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

Weitere Informationen zu angefügten Eigenschaften finden Sie unter [Übersicht über angefügte Eigenschaften](../xaml-platform/attached-properties-overview.md).

> **Hinweis**&nbsp;&nbsp;Eine angefügte Eigenschaft ist ein XAML-Konzept, das spezielle Syntax zum Abrufen aus oder Festlegen in Code erfordert. Informationen zur Verwendung angefügter Eigenschaften in Code finden Sie im Abschnitt *Angefügte Eigenschaften in Code* des Artikels *Übersicht über angefügte Eigenschaften*.

**Panelrahmen**

Die Panels „RelativePanel“, „StackPanel“ und „Grid“ definieren Rahmeneigenschaften, mit denen Sie einen Rahmen um das Panel zeichnen können, ohne dass ein zusätzliches umschließendes Rahmenelement erforderlich ist. Die Rahmeneigenschaften sind **BorderBrush**, **BorderThickness**, **CornerRadius** und **Padding**.

Hier sehen Sie ein Beispiel für das Festlegen von Rahmeneigenschaften für ein „Grid“.

```xaml
<Grid BorderBrush="Blue" BorderThickness="12" CornerRadius="12" Padding="12">
    <TextBlock Text="Hello World!"/>
</Grid>
```

![Ein Raster mit Rahmen](images/layout-panel-grid-border.png)

Durch Verwendung der integrierten Rahmeneigenschaften kann die Anzahl der XAML-Elemente reduziert und dadurch die UI-Leistung Ihrer App verbessert werden. Weitere Informationen zu Layoutpanels und zur UI-Leistung finden Sie unter [Optimieren des XAML-Layouts](https://msdn.microsoft.com/en-us/library/windows/apps/mt404609.aspx).

## RelativePanel

Mit [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) können Sie das Layout von UI-Elementen festlegen, indem Sie angeben, an welcher Position sie relativ zu anderen Elementen und dem Panel platziert werden sollen. Standardmäßig wird ein Element in der oberen linken Ecke des Panels positioniert. Sie können [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx) und [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) verwenden, um die Benutzeroberfläche für unterschiedliche Fenstergrößen neu anzuordnen.

Die folgende Tabelle zeigt die angefügten Eigenschaften, mit denen Sie ein Element auf die Kante oder die Mitte des Panels ausrichten und in Bezug auf andere Elemente ausrichten und positionieren können.

Panelausrichtung | Ausrichtung gleichgeordneter Elemente | Position gleichgeordneter Elemente
----------------|-------------------|-----------------
[**AlignTopWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aligntopwithpanel.aspx) | [**AlignTopWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aligntopwith.aspx) | [**Above**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.above.aspx)  
[**AlignBottomWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignbottomwithpanel.aspx) | [**AlignBottomWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignbottomwith.aspx) | [**Below**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.below.aspx)  
[**AlignLeftWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignleftwithpanel.aspx) | [**AlignLeftWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignleftwith.aspx) | [**LeftOf**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.leftof.aspx)  
[**AlignRightWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignrightwithpanel.aspx) | [**AlignRightWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignrightwith.aspx) | [**RightOf**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.rightof.aspx)  
[**AlignHorizontalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanel.aspx) | [**AlignHorizontalCenterWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwith.aspx) | &nbsp;   
[**AlignVerticalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithpanel.aspx) | [**AlignVerticalCenterWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignverticalcenterwith.aspx) | &nbsp;   

 
Dieser XAML-Code zeigt das Anordnen von Elementen in einem „RelativePanel“.

```xaml
<RelativePanel BorderBrush="Gray" BorderThickness="1">
    <Rectangle x:Name="RedRect" Fill="Red" Height="44" Width="44"/>
    <Rectangle x:Name="BlueRect" Fill="Blue"
               Height="44" Width="88"
               RelativePanel.RightOf="RedRect" />

    <Rectangle x:Name="GreenRect" Fill="Green" 
               Height="44"
               RelativePanel.Below="RedRect" 
               RelativePanel.AlignLeftWith="RedRect" 
               RelativePanel.AlignRightWith="BlueRect"/>
    <Rectangle Fill="Yellow"
               RelativePanel.Below="GreenRect" 
               RelativePanel.AlignLeftWith="BlueRect" 
               RelativePanel.AlignRightWithPanel="True"
               RelativePanel.AlignBottomWithPanel="True"/>
</RelativePanel>
```

Das Ergebnis sieht wie folgt aus. 

![RelativePanel](images/layout-panel-relative-panel.png)

Die folgenden Aspekte müssen in Bezug auf das Ändern der Rechtecksgrößen beachtet werden.
- Das rote Rechteck erhält eine explizite Größe von 44 x 44. Es wird in der oberen linken Ecke des Panels platziert; dies ist die Standardposition.
- Das grüne Rechteck erhält eine explizite Höhe von 44. Die linke Seite dieses Rechtecks wird auf das rote Rechteck ausgerichtet und die rechte Seite auf das blaue Rechteck, das die Breite bestimmt.
- Das gelbe Rechteck erhält keine explizite Größe. Seine linke Seite wird auf das blaue Rechteck ausgerichtet. Die rechte und untere Kante werden auf die Kante des Panels ausgerichtet. Die Größe wird durch diese Ausrichtungen bestimmt und ändert sich zusammen mit dem Panel.

## StackPanel

Das [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx) ist ein einfaches Layoutpanel, das seine untergeordneten Elemente in einer einzelnen Zeile anordnet. Die Zeile kann horizontal oder vertikal ausgerichtet werden. StackPanel-Steuerelemente werden i. d. R. verwendet, wenn Sie nur einen kleinen Teil der UI auf der Seite anordnen möchten.

Mit der [**Orientation**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.orientation.aspx)-Eigenschaft können Sie die Richtung der untergeordneten Elemente angeben. Die Standardausrichtung ist [**Vertikal**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.orientation.aspx).

Der folgende XAML-Code veranschaulicht das Erstellen eines vertikalen StackPanel von Elementen.

```xaml
<StackPanel>
    <Rectangle Fill="Red" Height="44"/>
    <Rectangle Fill="Blue" Height="44"/>
    <Rectangle Fill="Green" Height="44"/>
    <Rectangle Fill="Yellow" Height="44"/>
</StackPanel>
```


Das Ergebnis sieht wie folgt aus.

![StackPanel](images/layout-panel-stack-panel.png)

Wird in einem StackPanel die Größe eines untergeordneten Elements nicht explizit festgelegt, wird das Element gestreckt, sodass es die zur Verfügung stehende Breite (oder Höhe, falls die Ausrichtung auf **Horizontal** festgelegt ist) ausfüllt. In diesem Beispiel ist die Breite der Rechtecke nicht festgelegt. Die Rechtecke werden erweitert, bis Sie die gesamte Breite von StackPanel ausfüllen.

## Raster

Das [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)-Panel unterstützt die Anordnung von Steuerelementen in Layouts mit mehreren Zeilen und Spalten. Sie können die Zeilen und Spalten eines Rasterpanels mit den Eigenschaften [**RowDefinitions**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.rowdefinitions.aspx) und [**ColumnDefinitions**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.columndefinitions.aspx) angeben. Verwenden Sie in XAML die Eigenschaftselementsyntax, um die Zeilen und Spalten im Rasterelement zu deklarieren. Verteilen Sie den Platz innerhalb einer Spalte oder Zeile mit der automatischen Größenanpassung **Auto** oder per Größenanpassung mit Sternvariablen.

Mit den angefügten Eigenschaften [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.column.aspx) und [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.row.aspx) ordnen Sie Objekte in bestimmten Zellen an.

Wenn der Inhalt über mehrere Spalten und Zeilen angezeigt werden soll, können Sie die angefügten Eigenschaften [**Grid.RowSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.rowspan.aspx) und [**Grid.ColumnSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.columnspan.aspx) verwenden.

In diesem XAML-Beispiel wird veranschaulicht, wie Sie ein Rasterelement mit drei Zeilen und zwei Spalten erstellen. Die Höhe der ersten und dritten Zeile reicht gerade für den enthaltenen Text. Die zweite Zeile füllt die restliche Höhe aus. Die Breite der einzelnen Spalten wird gleichmäßig auf die verfügbare Breite des Containers aufgeteilt.

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="44"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red" Width="44"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Yellow" Grid.Row="1" Grid.Column="1"/>
</Grid>
```


Das Ergebnis sieht wie folgt aus.

![Raster](images/layout-panel-grid.png)

In diesem Beispiel funktioniert die Festlegung der Größe wie folgt: 
- Die zweite Zeile weist eine explizite Höhe von 44 effektiven Pixel auf. Standardmäßig füllt die Höhe der ersten Zeile den gesamten verbleibenden Platz.
- Die Breite der ersten Spalte ist auf **Auto** festgelegt, d. h. sie ist so breit, wie dies für ihre untergeordneten Elemente erforderlich ist. In diesem Fall ist die Spalte 44 effektive Pixel breit, um der Breite des roten Rechtecks zu entsprechen.
- Es sind keine anderen Größeneinschränkungen für die Rechtecke festgelegt, daher wird jedes gestreckt, um die Rasterzelle ausfüllen, in der es sich befindet.

## VariableSizedWrapGrid

[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) stellt ein Layoutpanel im Rasterstil zur Verfügung, in dem Elemente in Zeilen oder Spalten angeordnet werden, die automatisch in eine neue Zeile oder Spalte umbrochen werden, wenn der [**MaximumRowsOrColumns**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.maximumrowsorcolumns.aspx)-Wert erreicht ist. 

Mit der [**Orientation**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.orientation.aspx)-Eigenschaft wird angegeben, ob Elemente im Raster vor dem Umbrechen in Zeilen oder Spalten hinzugefügt werden. Die Standardausrichtung ist **Vertikal**, d. h., das Raster fügt Elemente von oben nach unten hinzu, bis eine Spalte voll ist, dann erfolgt ein Umbruch in eine neue Spalte. Wenn der Wert **Horizontal** lautet, fügt das Raster Elemente von links nach rechts hinzu und fügt dann einen Umbruch in eine neue Zeile ein.

Zellenabmessungen werden mit den Eigenschaften [**ItemHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.itemheight.aspx) und [**ItemWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.itemwidth.aspx) angegeben. Jede Zelle hat die gleiche Größe. Wenn „ItemHeight“ oder „ItemWidth“ nicht angegeben wird, wird die Größe der erste Zelle ihrem Inhalt entsprechend festgelegt, und alle anderen Zellen haben die Größe der ersten Zelle.

Sie können die angefügten Eigenschaften [**VariableSizedWrapGrid.ColumnSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.columnspan.aspx) und [**VariableSizedWrapGrid.RowSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.rowspan.aspx) verwenden, um anzugeben, wie viele benachbarte Zellen ein untergeordnetes Element füllen soll.

Hier sehen Sie die Verwendung eines VariableSizedWrapGrid-Elements in XAML.

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Yellow" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```


Das Ergebnis sieht wie folgt aus.

![Umbruchraster mit variabler Größe](images/layout-panel-variable-size-wrap-grid.png)

In diesem Beispiel beträgt die maximale Anzahl von Zeilen in jeder Spalte 3. Die erste Spalte enthält nur zwei Elemente (das rote und das blaue Rechteck), da sich das blaue Rechteck über zwei Zeilen erstreckt. Das grüne Rechteck wird dann an den Anfang der nächsten Spalte umbrochen.

## Canvas

Das [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)-Panel positioniert seine untergeordneten Elemente mithilfe von festen Koordinatenpunkten. Sie geben die Punkte in den einzelnen untergeordneten Elementen an, indem Sie für jedes Element die angefügten Eigenschaften [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.left.aspx) und [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.top.aspx) festlegen. Beim Erstellen des Layouts liest das übergeordnete Canvas-Element die Werte dieser angefügten Eigenschaften und verwendet diese Werte dann während der [Arrange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.arrange.aspx)-Phase der Layouterstellung.

Objekte in einem Canvas-Panel können sich überlappen, wobei ein Objekt über einem anderen Objekt gezeichnet wird. Standardmäßig rendert das Canvas-Element untergeordnete Objekte in der Reihenfolge, in der sie deklariert werden, d. h., das letzte untergeordnete Element wird im Vordergrund gerendert (jedes Element verfügt über einen standardmäßigen Z-Index von 0). Dies ist genauso wie bei anderen integrierten Panels. Canvas unterstützt jedoch auch die angefügte Eigenschaft [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx), die Sie für die einzelnen untergeordneten Elemente festlegen können. Sie können diese Eigenschaft im Code festlegen, um die Zeichnungsreihenfolge von Elementen zur Laufzeit zu ändern. Das Element mit dem höchsten Canvas.ZIndex-Wert wird zuletzt gezeichnet, also über allen anderen Elementen, die sich im gleichen Raum befinden oder sich auf irgendeine Weise überlappen. Beachten Sie, dass der Alpha-Wert (Transparenz) berücksichtigt wird. Auch wenn sich Elemente überlappen, werden die Inhalte in Überlappungsbereichen also ggf. vermischt, wenn das obere Element über einen Alpha-Wert verfügt, der nicht dem Maximalwert entspricht.

Das Canvas-Element legt die Größe seiner untergeordneten Elemente nicht fest. Jedes Element muss seine Größe selbst angeben.

Im Folgenden finden Sie ein Beispiel für ein Canvas-Element in XAML.

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red" Height="44" Width="44"/>
    <Rectangle Fill="Blue" Height="44" Width="44" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Height="44" Width="44" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Yellow" Height="44" Width="44" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```


Das Ergebnis sieht wie folgt aus.

![Canvas](images/layout-panel-canvas.png)

Verwenden Sie das Canvas-Panel mit Bedacht. Es ist zwar in einigen Szenarien hilfreich, die Positionen von UI-Elementen genau steuern zu können, aber eine feste Positionierung eines Layoutpanels bewirkt, dass dieser Bereich der UI weniger gut an allgemeine Änderungen der Größe von App-Fenstern angepasst werden kann. Ursachen für Größenänderungen von App-Fenstern können Änderungen der Geräteausrichtung, Teilungen von App-Fenstern, Änderungen von Monitoren und verschiedene andere Benutzerszenarien sein.

## Panels für ItemsControl

Es gibt verschiedene spezielle Panels, die nur als [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) zum Anzeigen von Elementen in einem [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)-Element verwendet werden können. Dies sind [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemsstackpanel.aspx), [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemswrapgrid.aspx), [**VirtualizingStackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.virtualizingstackpanel.aspx) und [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.wrapgrid.aspx). Diese Panel können nicht für das allgemeine UI-Layout verwendet werden.


<!--HONumber=Mar16_HO1-->


