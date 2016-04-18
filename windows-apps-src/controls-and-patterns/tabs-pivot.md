---
Description: Registerkarten und Pivots ermöglichen Benutzern die Navigation zwischen häufig verwendeten Inhalten.
title: Registerkarten und Pivots
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Registerkarten und Pivots
template: detail.hbs
---
# Registerkarten und Pivots

Registerkarten und Pivots werden für die Navigation zwischen häufig genutzten verschiedenen Inhaltskategorien verwendet. Das Registerkarten-/Pivotmuster besteht aus zwei oder mehr Inhaltsbereichen mit entsprechenden Kategorieheadern. Die Header werden dauerhaft auf dem Bildschirm angezeigt und verfügen über einen eindeutigen Auswahlstatus, damit Benutzer stets direkt erkennen, in welcher Kategorie sie sich befinden.
![Beispiele für Registerkarten](images/HIGSecOne_Tabs.png)

Registerkarten und Pivots werden mithilfe des [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx)-Steuerelements erstellt und folgen grundsätzlich dem gleichen Muster. Die grundlegenden Funktionen des Pivot-Steuerelements werden weiter unten in diesem Artikel beschrieben.

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**Pivot-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn608241)

## Das Registerkarten-/Pivotmuster

Beim Erstellen einer App mit dem Registerkarten-/Pivotmuster müssen basierend auf dem konfigurierbaren Featuresatz des Musters einige wichtige Designvariablen berücksichtigt werden.

- **Platzierung des Headers.**   Header können am oberen oder unteren Bildschirmrand platziert werden.
    
    **Hinweis**&nbsp;&nbsp;Wenn Header unten im Bildschirm platziert werden, muss die Vorlage für das Pivot-Steuerelement neu erstellt werden.
- **Headerbeschriftungen.**  Header können mit einem Symbol mit Text, nur mit Text oder nur mit einem Symbol versehen werden.
- **Headerausrichtung.**  Header können links ausgerichtet oder zentriert werden.
- **Navigation auf oberster oder auf einer untergeordneten Ebene.**  Registerkarten/Pivots können für alle Navigationsebenen verwendet und in einem Muster mit Navigation auf oberster Ebene/untergeordneter Ebene gestapelt werden. Wenn zwei Ebenen mit Registerkarten/Pivots vorhanden sind, sollten sich der Header der obersten Ebene und der Header der untergeordneten Ebene optisch unterscheiden, damit sie von Benutzern leicht auseinandergehalten werden können.
- **Unterstützung für Touchgesten**  Für Geräte, die Touchgesten unterstützen, können Sie für die Navigation zwischen Inhaltskategorien einen von zwei Interaktionssätzen verwenden:
    1. Tippen Sie auf einen Registerkarten-/Pivotheader, um zur gewünschten Kategorie zu navigieren, oder wischen Sie über den Inhaltsbereich, um zur benachbarten Kategorie zu navigieren.
    2. Tippen Sie auf einen Registerkarten-/Pivotheader, um zur gewünschten Kategorie zu navigieren (keine Wischgeste).

### Musterkonfigurationen

Die optimale Anordnung des Registerkarten-/Pivotmusters hängt vom Interaktionsszenario und den Geräten ab, auf denen Ihre App angezeigt wird. In dieser Tabelle werden einige der wichtigsten Szenarien und Musterkonfigurationen beschrieben.

Interaktionsszenario|Empfohlene Konfiguration
--------------------|-------------------------
Horizontales Wechseln zwischen 2 bis 5 Inhaltskategorien in einer Listen- oder Rasteransicht auf oberster Ebene auf einem Smartphone oder Phablet|Registerkarte/Pivots: am oberen Bildschirmrand, zentriert
|Headerbeschriftung: Symbole + Text
|Wischen auf dem Inhaltsbereich: aktiviert
Wechseln zwischen verschiedenen Inhaltskategorien auf einem Smartphone oder Phablet, bei dem sich das Wischen auf dem Inhaltsbereich nicht für die Navigation eignet|Registerkarte/Pivots: am unteren Bildschirmrand, zentriert
|Headerbeschriftung: Symbole + Text
|Wischen auf dem Inhaltsbereich: deaktiviert
Navigation auf oberster Ebene mit Maus und Tastatur|Registerkarte/Pivots: am oberen Bildschirmrand, links ausgerichtet
 *oder*|Headerbeschriftungen: nur Text
 Navigation auf Seitenebene auf einem Gerät mit Toucheingabe|Wischen auf dem Inhaltsbereich: deaktiviert

## Beispiele

Anhand dieses Entwurfs einer Food Truck-App wird das Platzieren von Registerkarten-/Pivotheadern am oberen oder unteren Bildschirmrand veranschaulicht. Das Platzieren von Registerkarten-/Pivotheadern am unteren Rand eignet sich auf Mobilgeräten gut für die Erreichbarkeit.

![Beispiel für Registerkarten auf einem mobilen Gerät](images/uap_foodtruck_phone_320_tabsboth.png)

Der Entwurf der Food Truck-App auf Laptops/Desktops enthält reine Textheader. Die Verwendung von Symbolen mit Text als Header eignet sich für Touchziele. Für Maus und Tastatur können jedoch auch reine Textheader verwendet werden.

![Beispiel für Registerkarten auf dem Desktop](images/uap_foodtruck_desktop_home_700.png)

## Erstellen eines Pivot-Steuerelements

Das Registerkarten-/Pivotnavigationsmuster wird mithilfe des [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx)-Steuerelements erstellt. Das Steuerelement enthält die in diesem Abschnitt beschriebenen grundlegenden Funktionen.

Dieser XAML-Code erzeugt ein grundlegendes Pivot-Steuerelement mit 3 Inhaltsbereichen.

```xaml
<Pivot x:Name="rootPivot" Title="PIVOT TITLE">
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

**Pivot-Elemente**

Das Pivot-Steuerelement ist ein [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx), daher kann es eine Sammlung von Elementen jeden Typs enthalten. Jedes zum Pivot-Steuerelement hinzugefügte Element, das nicht ausdrücklich ein [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx)-Element ist, wird implizit in ein PivotItem eingeschlossen. Da ein Pivot-Element häufig zum Navigieren zwischen Inhaltsseiten verwendet wird, ist es üblich, die Sammlung von [**Elementen**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) direkt mit XAML-UI-Elementen zu füllen. Alternativ können Sie die Eigenschaft [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) auf eine Datenquelle festlegen. In der ItemsSource gebundene Elemente können beliebigen Typs sein, wenn es sich jedoch nicht explizit um PivotItems handelt, müssen Sie eine [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)- und eine [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx)-Eigenschaft definieren, um festzulegen, wie die Elemente angezeigt werden sollen.

Mit der Eigenschaft [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) können Sie das aktive Element des Pivot-Steuerelements abrufen oder festlegen. Mit der Eigenschaft [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) können Sie den Index des aktiven Elements abrufen oder festlegen. 

**Pivotheader**

Sie können die Eigenschaften [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) und [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) verwenden, um weitere Steuerelemente zum Pivotheader hinzuzufügen. 

### Pivot-Interaktion

Das Steuerelement beinhaltet die folgenden Touchgesteninteraktionen:

-   Durch Tippen auf einen Header wird zum Abschnittsinhalt dieses Headers navigiert.
-   Durch Wischen nach links oder rechts auf einem Header wird zum benachbarten Header/Abschnitt navigiert.
-   Durch Wischen nach links oder rechts auf einem Abschnittsinhalt wird zum benachbarten Header/Abschnitt navigiert.

Das Steuerelement ist in zwei Modi verfügbar:

**Fest**

-   Pivots werden nicht verschoben, wenn alle Pivotheader in den zulässigen Bereich passen.
-   Durch Tippen auf eine Pivotbeschriftung wird zur entsprechenden Seite navigiert. Das Pivot selbst wird jedoch nicht verschoben. Das aktive Pivot wird hervorgehoben.

**Karussell**

-   Falls nicht alle Pivotheader in den verfügbaren Bereich passen, werden Pivots in einer Karussellansicht dargestellt.
-   Durch Tippen auf eine Pivotbeschriftung wird die entsprechende Seite aufgerufen, und die aktive Pivotbeschriftung rückt an die erste Position.

Das Steuerelement enthält integrierte Haltepunktfunktionen, die auf der Anzahl der Header und der Zeichenfolgenlänge der Beschriftungen basieren.

## Empfehlungen

-   Die Ausrichtung der Registerkarten-/Pivotheader sollte auf der Bildschirmgröße basieren. Bei Bildschirmbreiten unter 720 Epx ist die Zentrierung in der Regel besser geeignet. Bei Bildschirmbreiten über 720 Epx wird in den meisten Fällen die Linksausrichtung empfohlen.
-   Wenn Registerkarten-/Pivotheader beim Skalieren eines Fensters nicht auf die verfügbare Bildschirmfläche passen, empfiehlt es sich, sie in den Überlaufbereich auszulagern.
-   Registerkarten/Pivots können bei jeder Bildschirmausrichtung verwendet werden. Stellen Sie jedoch sicher, dass im Quer- und im Hochformat die gleiche Gesamtanzahl an Headern (sichtbar und ausgeblendet) angezeigt wird.
-   Verwenden Sie nicht mehr als 5 Header im Karussellmodus, da der Benutzer sonst leicht den Überblick verlieren kann.
-   Das Platzieren von Registerkarten am unteren Rand eignet sich auf Mobilgeräten gut für die Erreichbarkeit, falls Wischen in einem anderen Bereich der UI verwendet wird. Zudem befinden sich dann nicht zu viele UI-Elemente am oberen Rand.
-   Wenn eine Bildschirmtastatur bereitgestellt wird, können Header auf dem Bildschirm ausgeblendet werden, um Platz freizugeben.

\[Dieser Artikel enthält spezielle Informationen zu Apps für die universelle Windows-Plattform (UWP) und Windows 10. Laden Sie für Windows 8.1 die [PDF-Datei mit Windows 8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]

## Verwandte Themen

[Navigationsdesigngrundlagen](https://msdn.microsoft.com/library/windows/apps/dn958438)


<!--HONumber=Mar16_HO1-->


