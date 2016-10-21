---
author: jwmsft
title: xPhase-Attribut
description: Verwenden Sie xPhase mit der xBind-Markuperweiterung zum inkrementellen Rendern von ListView- und GridView-Elementen und zur Verbesserung des Verschiebens.
ms.assetid: BD17780E-6A34-4A38-8D11-9703107E247E
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: c6100f59bb91bc3c6451fc2167d914b0a4a36ded

---

# x:Phase-Attribut

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Verwenden Sie **x:Phase** mit der [{x:Bind}-Markuperweiterung](x-bind-markup-extension.md) zum inkrementellen Rendern von [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) und [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705)-Elementen und zur Verbesserung des Verschiebungserlebnisses. **x:Phase** bietet eine deklarative Möglichkeit, die gleiche Wirkung zu erzielen wie bei Verwendung des [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914)-Ereignisses zum manuellen Steuern des Renderns von Listenelementen. Weitere Informationen finden Sie auch unter [Inkrementelles Aktualisieren von ListView- und GridView-Elementen](../debug-test-perf/optimize-gridview-and-listview.md#update-items-incrementally).

## XAML-Attributsyntax


``` syntax
<object x:Phase="PhaseValue".../>
```

## XAML-Werte


| Benennung | Beschreibung |
|------|-------------|
| PhaseValue | Eine Zahl, die die Phase gibt an, in der das Element verarbeitet wird. Der Standardwert ist 0. | 

## Anmerkungen

Wenn eine Liste mit Toucheingabe oder mit dem Mausrad schnell verschoben wird, ist die Liste je nach Komplexität der Datenvorlage möglicherweise nicht in der Lage, Elemente schnell genug zu rendern, um mit der Geschwindigkeit des Bildlaufs Schritt zu halten. Das gilt besonders für ein tragbares Gerät mit einer energieeffizienten CPU, z. B. ein Telefon oder ein Tablet.

Phasing ermöglicht das inkrementelle Rendern der Datenvorlage, sodass der Inhalt priorisiert werden kann und die wichtigsten Elemente zuerst gerendert werden. Dadurch kann die Liste beim schnellen Verschieben für jedes Element einige Inhalte anzeigen und mehr Elemente für jede Vorlage rendern, falls die Zeit dies zulässt.

## Beispiel

```xml
<DataTemplate x:Key="PhasedFileTemplate" x:DataType="model:FileItem">
    <Grid Width="200" Height="80">
        <Grid.ColumnDefinitions>
           <ColumnDefinition Width="75" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Image Grid.RowSpan="4" Source="{x:Bind ImageData}" MaxWidth="70" MaxHeight="70" x:Phase="3"/>
        <TextBlock Text="{x:Bind DisplayName}" Grid.Column="1" FontSize="12"/>
        <TextBlock Text="{x:Bind prettyDate}"  Grid.Column="1"  Grid.Row="1" FontSize="12" x:Phase="1"/>
        <TextBlock Text="{x:Bind prettyFileSize}"  Grid.Column="1"  Grid.Row="2" FontSize="12" x:Phase="2"/>
        <TextBlock Text="{x:Bind prettyImageSize}"  Grid.Column="1"  Grid.Row="3" FontSize="12" x:Phase="2"/>
    </Grid>
</DataTemplate>
```

Die Datenvorlage beschreibt vier Phasen:

1.  Stellt den DisplayName-Textblock dar. Alle Steuerelemente ohne Phase werden implizit als zur Phase 0 zugehörig betrachtet.
2.  Zeigt den prettyDate-Textblock.
3.  Zeigt die prettyFileSize- und prettyImageSize-Textblöcke.
4.  Zeigt das Bild.

Phasing ist eine Funktion von [{x:Bind}](x-bind-markup-extension.md), die aus [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879) abgeleitete Steuerelemente verwendet und die Elementvorlage für die Datenbindung inkrementell verarbeitet. Beim Rendern von Listenelementen stellt **ListViewBase** eine einzelne Phase für alle Elemente in der Ansicht dar, bevor der Wechsel zur nächsten Phase erfolgt. Das Rendern wird in zeitlich unterteilten Stapeln ausgeführt, damit die erforderliche Arbeit bei einem Listenbildlauf neu analysiert und nicht für Elemente ausgeführt werden kann, die nicht mehr sichtbar sind.

Das **x:Phase**-Attribut kann für jedes Element in einer Datenvorlage, die [{x:Bind}](x-bind-markup-extension.md) verwendet, angegeben werden. Wenn ein Element eine andere Phase als 0 aufweist, wird das Element aus der Ansicht ausgeblendet (über **Opacity**, nicht über **Visibility**), bis diese Phase verarbeitet wird und Bindungen aktualisiert werden. Wenn für ein von [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879) abgeleitetes Steuerelement ein Bildlauf durchgeführt wird, verwendet es die Elementvorlagen von Elementen, die nicht mehr auf dem Bildschirm sind, erneut, um die neu sichtbaren Elemente zu rendern. UI-Elemente in der Vorlage behalten ihre alten Werte bei, bis sie erneut an Daten gebunden werden. Phasing verursacht das Verzögern dieses Datenbindungsschritts, sodass Phasing die UI-Elemente für den Fall, dass sie veraltet sind, ausblenden muss.

Für jedes UI-Element darf nur eine Phase angegeben werden. Diese gilt dann für alle Bindungen des Elements. Wenn keine Phase angegeben ist, wird die Phase 0 angenommen.

Phasennummern müssen nicht fortlaufend sein und sind mit den Wert der [**ContainerContentChangingEventArgs.Phase**](https://msdn.microsoft.com/library/windows/apps/dn298493) identisch. Das [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914)-Ereignis wird für jede Phase vor der Verarbeitung der **x:Phase**-Bindungen ausgelöst.

Phasing wirkt sich nur auf [{x:Bind}](x-bind-markup-extension.md) -Bindungen aus, nicht auf [{Binding}](binding-markup-extension.md)-Bindungen.

Phasing gilt nur, wenn die Elementvorlage mithilfe eines Steuerelements gerendert wird, das Phasing erkennt. Für Windows 10 bedeutet das [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) und [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705). Phasing gilt nicht für Datenvorlagen, die in anderen Elementsteuerelementen verwendet werden, oder für andere Szenarien, wie zum Beispiel die Abschnitte [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/br209369) oder [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) – in diesen Fällen werden alle UI-Elemente auf einmal an Daten gebunden.




<!--HONumber=Aug16_HO3-->


