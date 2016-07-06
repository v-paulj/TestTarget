---
author: jwmsft
title: xDeferLoadStrategy-Attribut
description: "„xDeferLoadStrategy“ verzögert die Erstellung eines Elements und seiner untergeordneten Elemente, verkürzt die Startzeit, erhöht aber leicht die Arbeitsspeicherauslastung. Jedes betroffene Element erhöht die Arbeitsspeicherauslastung um ca. 600 Bytes."
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: b989a31439444f06dacb86adb186f853d1637f6c

---

# x:DeferLoadStrategy-Attribut

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**x:DeferLoadStrategy="Lazy"** verzögert die Erstellung eines Elements und seiner untergeordneten Elemente, verkürzt die Startzeit, erhöht aber leicht die Arbeitsspeicherauslastung. Jedes betroffene Element erhöht die Arbeitsspeicherauslastung um ca. 600 Bytes. Je größer die verzögerte Elementstruktur, desto mehr Startzeit sparen Sie – aber auf Kosten eines größeren Arbeitsspeicherbedarfs. Daher ist es möglich, dass durch eine übermäßige Verwendung dieses Attributs die Leistung beeinträchtigt wird.

## XAML-Attributsyntax

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## Anmerkungen

Die Einschränkungen für die Verwendung von **x:DeferLoadStrategy** sind:

-   Erfordert eine Definition von [x:Name](x-name-attribute.md) , da es eine Methode geben muss, um das Element später zu suchen.
-   Nur ein [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) kann als verzögert markiert werden, mit Ausnahme der von [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249) abgeleiteten Typen.
-   Stammelemente können in [**Page**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page), [**UserControls**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.usercontrol) oder [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) nicht verzögert werden.
-   Elemente in einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) können nicht verzögert werden.
-   Funktioniert nicht mit lose mit [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) geladenem XAML-Code.
-   Durch das Verschieben eines übergeordneten Elements werden alle Elemente gelöscht, die nicht erkannt wurden.

Es gibt mehrere Möglichkeiten, verzögerte Elementen zu erkennen:

-   Rufen Sie [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) mit dem Namen auf, der für das Element definiert wurde.
-   Rufen Sie [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) mit dem Namen auf, der für das Element definiert wurde.
-   Verwenden Sie in [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) eine [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)- oder **Storyboard**-Animation, die auf das verzögerte Element ausgerichtet ist.
-   Legen Sie das verzögerte Elemente in allen **Storyboards** als Ziel fest.
-   Verwenden Sie eine Bindung, die auf das verzögerte Element ausgerichtet ist.
-   HINWEIS: Nach Start der Instanziierung eines Elements wird es im Benutzeroberflächen-Thread erstellt. Dies kann ggf. bewirken, dass die Oberfläche ruckelt, wenn zu viele auf einmal erstellt werden.

Wenn ein verzögertes Element mit einer der oben aufgeführten Methoden erstellt wurde, passiert Folgendes:

-   Das [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723)-Ereignis für das Element wird ausgelöst.
-   Alle Bindungen für das Element werden ausgewertet.
-   Wenn die Anwendung für den Empfang von Benachrichtigungen über Änderungen an der Eigenschaft, die die verzögerten Elemente enthält, registriert wurde, wird die Benachrichtigung ausgelöst.

Sie können verzögerte Elemente verschachteln, jedoch müssen Sie vom äußersten Element aus nach innen erkannt werden.  Wenn Sie versuchen, ein untergeordnetes Element zu erkennen, bevor das übergeordnete Element erkannt wurde, wird eine Ausnahme ausgelöst.

Allgemein empfiehlt es sich, Dinge zu verzögern, die nicht im ersten Frame angezeigt werden können.  Bei der Suche nach zu verzögernden Kandidaten empfiehlt es sich, nach Elementen zu suchen, die mit reduzierter [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) erstellt werden.  Auch eine zufällig entstandene Benutzeroberfläche (UI, die durch eine Benutzerinteraktion ausgelöst wird) ist ein guter Ort zum Suchen nach zu verzögernden Elementen.  

Seien Sie vorsichtig beim Verzögern von Elementen in einem [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)-Szenario, da so die Startzeit verringert, aber auch die Leistung beim Verschieben reduziert werden kann – je nachdem, was Sie erstellen.  Wenn Sie die Leistung beim Verschieben erhöhen möchten, finden Sie in den Dokumentationen [{x:Bind}-Markuperweiterung](x-bind-markup-extension.md) und [x:Phase-Attribut](x-phase-attribute.md) weitere Informationen.

Wenn das [x:Phase-Attribut](x-phase-attribute.md) zusammen mit **x:DeferLoadStrategy** verwendet wird und ein Element oder eine Elementstruktur realisiert wird, werden die Bindungen bis einschließlich zur aktuellen Phase angewendet. Die für **x:Phase** angegebene Phase wirkt sich nicht auf die Verzögerung des Elements aus und diese lässt sich darüber nicht steuern. Wenn ein Listenelement beim Schwenken wiederverwendet wird, verhalten sich realisierte Elemente wie andere Elemente, und kompilierte Bindungen (**{x:Bind}**-Bindungen) werden unter Verwendung der gleichen Regeln einschließlich Phasing verarbeitet.

Eine allgemeine Richtlinie besteht darin, Ihre Anwendung vorher und nachher zu messen, um sicherzustellen, das Sie die gewünschte Leistung erhalten.

## Beispiel

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    this.FindName("DeferredGrid"); // This will realize the deferred grid
}
```




<!--HONumber=Jun16_HO4-->


