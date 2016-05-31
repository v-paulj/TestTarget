---
author: Jwmsft
Description: Registerkarten und Pivots ermöglichen Benutzern die Navigation zwischen häufig verwendeten Inhalten.
title: Registerkarten und Pivots
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
---
# Pivots und Registerkarten

Pivotsteuerelemente und Registerkarten werden für die Navigation zwischen häufig genutzten verschiedenen Inhaltskategorien verwendet. Das Registerkarten/Pivots bestehen aus zwei oder mehr Inhaltsbereichen mit entsprechenden Kategorieheadern. Die Header werden dauerhaft auf dem Bildschirm angezeigt und verfügen über einen eindeutigen Auswahlstatus, damit Benutzer stets direkt erkennen, in welcher Kategorie sie sich befinden.
![Beispiele für Registerkarten](images/HIGSecOne_Tabs.png)

Registerkarten sind eine visuelle Pivotvariante und werden mit dem [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx)-Steuerelement erstellt. [
              Ein **Codebeispiel**
            ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlPivot) für ein benutzerdefiniertes Pivot ist auf GitHub verfügbar.

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**Pivot-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn608241)

## Das Registerkarten-/Pivotmuster

Wenn Sie eine App mit dem Registerkarten-/Pivotmuster erstellen, sind einige wichtige Designelemente zu berücksichtigen.

- **Headerbeschriftungen.**  Header können ein Symbol mit Text oder nur Text enthalten.
- **Headerausrichtung.**  Header können links ausgerichtet oder zentriert werden.
- **Navigation auf oberster oder auf einer untergeordneten Ebene.**  Registerkarten/Pivots können für eine der Navigationsebenen verwendet werden. Optional kann der [Navigationsbereich](nav-pane.md) als primäre Ebene dienen und Registerkarten/Pivots als sekundäre.
- **Unterstützung für Touchgesten**  Für Geräte, die Touchgesten unterstützen, können Sie für die Navigation zwischen Inhaltskategorien einen von zwei Interaktionssätzen verwenden:
    1. Tippen Sie auf einen Registerkarten-/Pivotheader, um zur gewünschten Kategorie zu navigieren.
    2. Wischen Sie über dem Inhaltsbereich nach links oder rechts, um zur benachbarten Kategorie zu navigieren.

## Beispiele

Standardmäßig Pivotsteuerelement in Cortana-Erinnerungen.

![Ein Pivot-Beispiel in Cortana-Erinnerungen](images/pivot_cortana-reminders.png)

Registerkartenmuster in der App „Alarm & Uhr“

![Ein Beispiel für Registerkarten im „Alarm & Uhr“](images/tabs_alarms-and-clock.png)

## Erstellen eines Pivotsteuerelements

Das [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx)-Steuerelement enthält die in diesem Abschnitt beschriebenen grundlegenden Funktionen.

Dieser XAML-Code erzeugt ein grundlegendes Pivot-Steuerelement mit 3 Inhaltsbereichen.

```xaml
<Pivot x:Name="rootPivot" Title="Pivot Title">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

### Pivot-Elemente

Das Pivot-Steuerelement ist ein [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx), daher kann es eine Sammlung von Elementen jeden Typs enthalten. Jedes zum Pivot-Steuerelement hinzugefügte Element, das nicht ausdrücklich ein [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx)-Element ist, wird implizit in ein PivotItem eingeschlossen. Da ein Pivot-Element häufig zum Navigieren zwischen Inhaltsseiten verwendet wird, ist es üblich, die Sammlung von [**Elementen**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) direkt mit XAML-UI-Elementen zu füllen. Alternativ können Sie die Eigenschaft [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) auf eine Datenquelle festlegen. In der ItemsSource gebundene Elemente können beliebigen Typs sein, wenn es sich jedoch nicht explizit um PivotItems handelt, müssen Sie eine [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)- und eine [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx)-Eigenschaft definieren, um festzulegen, wie die Elemente angezeigt werden sollen.

Mit der Eigenschaft [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) können Sie das aktive Element des Pivot-Steuerelements abrufen oder festlegen. Mit der Eigenschaft [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) können Sie den Index des aktiven Elements abrufen oder festlegen.

### Pivotheader

Sie können die Eigenschaften [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) und [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) verwenden, um weitere Steuerelemente zum Pivotheader hinzuzufügen.

### Pivot-Interaktion

Das Steuerelement erkennt die folgenden Touchgesteninteraktionen:

-   Durch Tippen auf den Header eines Pivotelements wird zum Abschnittsinhalt dieses Headers navigiert.
-   Durch Wischen nach links oder rechts auf dem Header eines Pivotelements wird zum benachbarten Abschnitt navigiert.
-   Durch Wischen nach links oder rechts auf einem Abschnittsinhalt wird zum benachbarten Abschnitt navigiert.

Das Steuerelement ist in zwei Modi verfügbar:

**Fest**

-   Pivots werden nicht verschoben, wenn alle Pivotheader in den zulässigen Bereich passen.
-   Durch Tippen auf eine Pivotbeschriftung wird zur entsprechenden Seite navigiert. Das Pivot selbst wird jedoch nicht verschoben. Das aktive Pivot wird hervorgehoben.

**Karussell**

-   Falls nicht alle Pivotheader in den verfügbaren Bereich passen, werden Pivots in einer Karussellansicht dargestellt.
-   Durch Tippen auf eine Pivotbeschriftung wird die entsprechende Seite aufgerufen, und die aktive Pivotbeschriftung rückt an die erste Position.
-   Pivotelemente in einer Karussellschleife wechseln vom letzten zum ersten Pivotabschnitt.

## Empfehlungen

-   Die Ausrichtung der Registerkarten-/Pivotheader sollte auf der Bildschirmgröße basieren. Bei Bildschirmbreiten unter 720 epx ist die Zentrierung in der Regel besser geeignet. Bei Bildschirmbreiten über 720 epx ist in den meisten Fällen die Linksausrichtung empfehlenswert.
-   Vermeiden Sie mehr als 5 Header bei Verwendung des Karussell-Modus (Roundtrip), da mehr als Schleifen verwirrend sein können.
-   Verwenden Sie das Registerkartenmuster nur, wenn Ihre Pivotelemente unterschiedliche Symbole besitzen.
-   Fügen Sie Text in Pivotelementheader ein, damit Benutzer die Bedeutung der einzelnen Pivotabschnitte verstehen. Symbole sind nicht unbedingt für alle Benutzer selbsterklärend.



## Verwandte Themen

[Navigationsdesigngrundlagen](https://msdn.microsoft.com/library/windows/apps/dn958438)


<!--HONumber=May16_HO2-->


