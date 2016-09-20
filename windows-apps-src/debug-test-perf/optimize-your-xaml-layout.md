---
author: mcleblanc
ms.assetid: 79CF3927-25DE-43DD-B41A-87E6768D5C35
title: Optimieren des XAML-Layouts
description: "Das Layout kann einen aufwendigen Teil einer XAML-App darstellen, sowohl hinsichtlich der CPU-Nutzung als auch der Beanspruchung des Arbeitsspeichers. Hier sind einige einfache Schritte, mit denen Sie die Layoutleistung Ihrer XAML-App verbessern können."
ms.sourcegitcommit: afb508fcbc2d4ab75188a2d4f705ea0bee385ed6
ms.openlocfilehash: dbec176310896164ebc99c20aefca4c5b2b29ee9

---
# Optimieren des XAML-Layouts

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Wichtige APIs**

-   [**Panel**](https://msdn.microsoft.com/library/windows/apps/BR227511)

Unter Layout wird das Definieren der visuellen Struktur der Benutzeroberfläche verstanden. Der primäre Mechanismus zum Beschreiben des Layouts in XAML sind Bereiche, d. h. Containerobjekte, mit denen Sie UI-Elemente positionieren und anzuordnen können. Das Layout kann sowohl bezüglich der CPU-Auslastung als auch des Aufwands ein ressourcenintensiver Teil einer XAML-App sein. Hier sind einige einfache Schritte, mit denen Sie die Layoutleistung Ihrer XAML-App verbessern können.

## Reduzieren der Layout-Struktur

Die größte Verbesserung der Layoutleistung lässt sich durch die Vereinfachung der hierarchischen Struktur der UI-Elementstruktur erzielen. Bereiche sind in der visuellen Struktur vorhanden, aber es sind Strukturelemente, keine *pixelerzeugenden Elemente*, wie ein [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)- oder ein [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)-Objekt. Die Vereinfachung der Struktur durch die Verringerung der Anzahl der nicht pixelerzeugenden Elemente führt in der Regel zu einer deutlichen Leistungssteigerung.

Viele Benutzeroberflächen werden durch eine Schachtelung von Bereichen implementiert, was zu tiefen, komplexen Strukturen von Bereichen und Elementen führt. Es ist komfortabel, Bereiche zu schachteln, aber in vielen Fällen kann die gleiche Benutzeroberfläche mit einem komplexeren einzelnen Bereich erstellt werden. Die Verwendung eines einzelnen Bereichs sorgt für eine höhere Leistung.

### Gründe für die Reduzierung der Layoutstruktur

Eine einfache Reduzierung der Layoutstruktur – z. B. indem ein verschachtelter Bereich aus der Seite der obersten Ebene entfernt wird – hat keinen merklichen Effekt.

Die größten Leistungsgewinne werden durch die Reduzierung von Layoutstrukturen erzielt, die in der Benutzeroberfläche wiederholt vorkommen, z. B. ein [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)- oder [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)-Objekt. Diese [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803)-Elemente verwenden eine [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348)-Vorlage, welche eine häufig instanziierte Unterstruktur der UI-Elemente definiert. Wenn die gleiche Unterstruktur in Ihrer App oft repliziert wird, hat jede Leistungsverbesserung dieser Teilstruktur einen multiplikativen Effekt auf die allgemeine Leistung der App.

### Beispiele

Beachten Sie die folgende Benutzeroberfläche.

![Beispiel für ein Formularlayout](images/layout-perf-ex1.png)

In diesen Beispielen werden drei Methoden zur Implementierung der gleichen Benutzeroberfläche gezeigt. Die einzelnen Implementierungsoptionen resultieren in nahezu identischen Pixeln auf dem Bildschirm, unterscheidet sich jedoch grundlegend in den Implementierungsdetails.

Option 1: Geschachtelte [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635)-Elemente

Obwohl dieses Modell am einfachsten ist, werden darin 5 Bereichselemente verwendet, was zu erheblichem Mehraufwand führt.

```xml
  <StackPanel>
  <TextBlock Text="Options:" />
  <StackPanel Orientation="Horizontal">
      <CheckBox Content="Power User" />
      <CheckBox Content="Admin" Margin="20,0,0,0" />
  </StackPanel>
  <TextBlock Text="Basic information:" />
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Name:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Email:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Password:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <Button Content="Save" />
</StackPanel>
```

Option 2: Ein einzelnes [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Element

Das [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Element erhöht die Komplexität, jedoch wird nur ein einzelnes Bereichselement verwendet.

```xml
  <Grid>
  <Grid.RowDefinitions>
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
      <ColumnDefinition Width="Auto" />
      <ColumnDefinition Width="Auto" />
  </Grid.ColumnDefinitions>
  <TextBlock Text="Options:" Grid.ColumnSpan="2" />
  <CheckBox Content="Power User" Grid.Row="1" Grid.ColumnSpan="2" />
  <CheckBox Content="Admin" Margin="150,0,0,0" Grid.Row="1" Grid.ColumnSpan="2" />
  <TextBlock Text="Basic information:" Grid.Row="2" Grid.ColumnSpan="2" />
  <TextBlock Text="Name:" Width="75" Grid.Row="3" />
  <TextBox Width="200" Grid.Row="3" Grid.Column="1" />
  <TextBlock Text="Email:" Width="75" Grid.Row="4" />
  <TextBox Width="200" Grid.Row="4" Grid.Column="1" />
  <TextBlock Text="Password:" Width="75" Grid.Row="5" />
  <TextBox Width="200" Grid.Row="5" Grid.Column="1" />
  <Button Content="Save" Grid.Row="6" />
</Grid>
```

Option 3: Ein einzelnes [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/Dn879546)-Element:

Dieser einzelne Bereich ist etwas komplexer als die Verwendung der geschachtelten Bereiche, aber möglicherweise leichter zu verstehen und zu verwalten als ein [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Element.

```xml
<RelativePanel>
  <TextBlock Text="Options:" x:Name="Options" />
  <CheckBox Content="Power User" x:Name="PowerUser" RelativePanel.Below="Options" />
  <CheckBox Content="Admin" Margin="20,0,0,0" 
            RelativePanel.RightOf="PowerUser" RelativePanel.Below="Options" />
  <TextBlock Text="Basic information:" x:Name="BasicInformation"
           RelativePanel.Below="PowerUser" />
  <TextBlock Text="Name:" RelativePanel.AlignVerticalCenterWith="NameBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="NameBox"               
           RelativePanel.Below="BasicInformation" />
  <TextBlock Text="Email:"  RelativePanel.AlignVerticalCenterWith="EmailBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="EmailBox"
           RelativePanel.Below="NameBox" />
  <TextBlock Text="Password:" RelativePanel.AlignVerticalCenterWith="PasswordBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="PasswordBox"
           RelativePanel.Below="EmailBox" />
  <Button Content="Save" RelativePanel.Below="PasswordBox" />
</RelativePanel>
```

Wie die folgenden Beispiele zeigen, gibt es viele Möglichkeiten, die gleiche Benutzeroberfläche zu erzielen. Sie sollten bei der Wahl der Methode alle Nachteile, darunter die Aspekte Leistung, Lesbarkeit und Wartungsfreundlichkeit, sorgfältig prüfen.

## Verwenden von Rastern mit einzelnen Zellen für überlappende UI-Elemente

Eine allgemeine UI-Anforderung ist ein Layout, in dem Elemente einander überlappen. In der Regel werden Abstand, Rand, Ausrichtung und Transformationen verwendet, um Elementen in dieser Weise zu positionieren. Das [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Steuerelement in XAML wurde zur Verbesserung der Layoutleistung für sich überschneidende Elemente optimiert.


            **Wichtig**  Um eine Verbesserung festzustellen, verwenden Sie ein einzelliges [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)-Element. Definieren Sie weder [**RowDefinitions**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.rowdefinitions) noch [**ColumnDefinitions**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.columndefinitions).

### Beispiele

```xml
<Grid>
    <Ellipse Fill="Red" Width="200" Height="200" />
    <TextBlock Text="Test" 
               HorizontalAlignment="Center" 
               VerticalAlignment="Center" />
</Grid>
```

![Überlagerter Text auf einem Kreis](images/layout-perf-ex2.png)

```xml
<Grid Width="200" BorderBrush="Black" BorderThickness="1">
    <TextBlock Text="Test1" HorizontalAlignment="Left" />
    <TextBlock Text="Test2" HorizontalAlignment="Right" />
</Grid>
```

![Zwei Textblöcke in einem Raster](images/layout-perf-ex3.png)

## Verwenden der integrierten Rahmeneigenschaften von Bereichen


            [
              **Grid**
            ](https://msdn.microsoft.com/library/windows/apps/BR242704)-, [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635)-, [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/Dn879546)- und [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378)-Steuerelemente besitzen integrierte Rahmeneigenschaften, mit denen Sie einen Rahmen um sie herum zeichnen können, ohne XAML ein zusätzliches [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250)-Element hinzufügen zu müssen. Die neuen Eigenschaften, die den integrierten Rahmen unterstützen sind: **BorderBrush**, **BorderThickness**, **CornerRadius** und **Padding**. Jede davon ist eine [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362)-Eigenschaft, sodass sie mit Bindungen und Animationen verwendet werden können. Sie sind als vollständiger Ersatz für ein separates **Border**-Element konzipiert.

Wenn Ihre Benutzeroberfläche über [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250)-Elemente um Bereiche verfügt, verwenden Sie stattdessen den integrierten Rahmen. Dadurch wird ein zusätzliches Element in der Layoutstruktur Ihrer App eingespart. Wie bereits erwähnt, kann die Struktur dadurch deutlich vereinfacht werden, insbesondere bei einer UI mit sich wiederholenden Elementen.

### Beispiele

```xml
<RelativePanel BorderBrush="Red" BorderThickness="2" CornerRadius="10" Padding="12">
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

## Verwendung von **SizeChanged**-Ereignissen infolge von Layoutänderungen

Die [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706)-Klasse umfasst zwei ähnliche Ereignisse für die Reaktion auf Layoutänderungen: [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.layoutupdated) und [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.sizechanged). Möglicherweise verwenden Sie eines dieser Ereignisse, um Benachrichtigungen zu empfangen, wenn ein Element während des Layouts geändert wird. Die Semantik der beiden Ereignisse unterscheiden sich, und es gibt wichtige Leistungsaspekte, die bei der Auswahl zu berücksichtigen sind.

Wenn eine gute Leistung Priorität hat, ist [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.sizechanged) fast immer die richtige Wahl. 
            **SizeChanged** hat eine intuitive Semantik. Es wird während des Layouts ausgelöst, wenn die Größe des [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706)-Objekts aktualisiert wurde.


            [
              **LayoutUpdated**
            ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.layoutupdated) wird auch während des Layouts ausgelöst, hat aber eine globale Semantik. Es wird für jedes Element ausgelöst, sobald ein Element aktualisiert wird. Es ist üblich, Ereignishandler nur lokal zu verarbeiten, wodurch der Code häufiger als nötig ausgeführt wird. Verwenden Sie **LayoutUpdated** nur, wenn Sie wissen müssen, wann ein Element ohne Größenänderung neu angeordnet wird (was ungewöhnlich ist).

## Auswahl von Bereichen

Die Leistung wird bei der Wahl zwischen einzelnen Bereichen in der Regel nicht berücksichtigt. Die Auswahl erfolgt in der Regel erfolgt, indem betrachtet wird, welcher Bereich das Layoutverhalten bereitstellt, das der zu implementierenden UI am ähnlichsten ist. Wenn Sie z. B. zwischen [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704), [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635) und [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/Dn879546) wählen, sollten Sie den Bereich wählen, der am ehesten Ihrer Vorstellung von der Implementierung entspricht.

Jede XAML-Bereich ist hinsichtlich einer guten Leistung optimiert, und alle Bereiche stellen eine ähnliche Leistung für ähnliche UIs bereit.




<!--HONumber=Jun16_HO4-->


