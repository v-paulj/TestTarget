---
author: Jwmsft
Description: "Ein Steuerelement eines semantischen Zooms ermöglicht Benutzern, zwischen zwei verschiedenen semantischen Ansichten des gleichen Datensatzes zu zoomen."
title: Semantischer Zoom
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 125cb6e45defe3213af3f5cd20f524a5311241af

---
# Semantischer Zoom

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Mit dem semantischen Zoom können Benutzer zwischen zwei unterschiedlichen Ansichten desselben Inhalts zoomen, um schnell durch große Mengen gruppierter Daten zu navigieren.
 
- Die vergrößerte Ansicht ist die Hauptansicht des Inhalts. Dies ist die Hauptansicht, in der Sie einzelne Datenelemente anzeigen. 
- Die verkleinerte Ansicht zeigt den gleichen Inhalt auf höherer Ebene. In dieser Ansicht werden normalerweise die Gruppenköpfe für gruppierte Daten angezeigt. 

Bei der Anzeige eines Adressbuchs kann der Benutzer beispielsweise schnell zum Buchstaben „W” springen und diesen vergrößern, um die zu dem betreffenden Buchstaben gehörenden Namen anzuzeigen. 

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/hh702601"><strong>SemanticZoom-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx"><strong>ListView-Klasse</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx"><strong>GridView-Klasse</strong></a></li>
</ul>

</div>
</div>






**Features**:

-   Die Größe der verkleinerten Ansicht ist durch die Grenzen des Steuerelements für semantischen Zoom beschränkt.
-   Durch Tippen auf einen Gruppenkopf wird zwischen Ansichten gewechselt. Auch Zusammendrücken kann als Möglichkeit zum Wechseln zwischen den Ansichten aktiviert werden.
-   Aktive Kopfzeilen wechseln zwischen Ansichten.

## Ist dies das richtige Steuerelement?

Verwenden Sie das Steuerelement **SemanticZoom**, wenn Sie einen gruppierten Datensatz anzeigen müssen, der so groß ist, dass er auf einer oder zwei Seiten nicht ganz angezeigt werden kann.

Der semantische Zoom ist nicht mit dem optischen Zoom zu verwechseln. Sie zeigen zwar das gleiche Interaktions- und Grundverhalten (d.h. sie zeigen je nach Zoomfaktor mehr oder weniger Details an), der optische Zoom betrifft jedoch die Größenanpassung für einen Inhaltsbereich oder ein Objekt wie etwa ein Foto. Informationen zu einem Steuerelement, das optisches Zooming durchführt, finden Sie im Artikel über das Steuerelement [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx).

## Beispiele

**Fotos-App**

Hier ist ein in der Fotos-App verwendeter semantischer Zoom. Fotos werden nach Monat gruppiert. Durch Auswahl einer Monatskopfzeile in der standardmäßigen Rasteransicht wird die Ansicht der Monatsliste verkleinert, um schneller navigieren zu können.

![In der Fotos-App verwendeter semantischer Zoom](images/control-examples/semantic-zoom-photos.png)

**Adressbuch**

Ein Adressbuch ist ein weiteres Beispiel für einen Datensatz, der sich mit einem Steuerelement für semantisches Zoomen sehr viel einfacher durchblättern lässt. Sie können die verkleinerte Ansicht verwenden, um schnell zu dem gewünschten Buchstaben zu springen (linkes Bild), während die vergrößerte Ansicht die einzelnen Datenelemente zeigt (rechtes Bild).

![Beispiel für semantischen Zoom in einer Kontaktliste](images/semanticzoom-win10.png)

## Erstellen eines semantischen Zooms

Das Steuerelement **SemanticZoom** verfügt über keine visuelle Darstellung. Es handelt sich um ein Hoststeuerelement, das den Übergang zwischen zwei anderen Steuerelementen steuert, die die Ansichten für Ihre Inhalte bereitstellen, in der Regel die Steuerelemente **ListView** oder **GridView**.  Sie legen die Ansicht-Steuerelemente auf die [**ZoomedInView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedinview.aspx)- und [**ZoomedOutView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedoutview.aspx)-Eigenschaften von SemanticZoom fest.

Sie benötigen für einen semantischen Zoom folgende drei Elemente:
- Eine gruppierte Datenquelle
- Eine vergrößerte Ansicht, die Daten auf Elementebene anzeigt
- Eine verkleinerte Ansicht, die die Daten auf Gruppenebene anzeigt

Bevor Sie einen semantischen Zoom verwenden, sollten Sie wissen, wie Sie eine Listenansicht mit gruppierten Daten verwenden. Weitere Informationen finden Sie unter [Listenansicht und Rasteransicht](listview-and-gridview.md) und [Gruppieren von Elementen in einer Liste](). 

> **Hinweis**:&nbsp;&nbsp;Um die vergrößerte und verkleinerte Ansicht des SemanticZoom-Steuerelements zu definieren, können Sie zwei beliebige Steuerelemente verwenden, die die Benutzeroberfläche [**ISemanticZoomInformation**]() implementieren. Das XAML-Framework bietet drei Steuerelemente, die diese Schnittstelle implementieren: ListView, GridView und Hub.
 
 Dieser XAML-Code zeigt die Struktur des SemanticZoom-Steuerelements. Sie weisen weitere Steuerelemente den Eigenschaften „ZoomedInView” und „ZoomedOutView” zu.
 
 **XAML**
 ```xaml
<SemanticZoom>
    <SemanticZoom.ZoomedInView>
        <!-- Put the GridView for the zoomed out view here. -->   
    </SemanticZoom.ZoomedInView>

    <SemanticZoom.ZoomedOutView>
        <!-- Put the ListView for the zoomed in view here. -->       
    </SemanticZoom.ZoomedOutView>
</SemanticZoom>
 ```
 
Die folgenden Beispiele stammen von der Seite „SemanticZoom” des [Beispiels für XAML-UI-Grundlagen](http://go.microsoft.com/fwlink/p/?LinkId=619992). Sie können das Beispiel herunterladen, um den kompletten Code anzuzeigen, einschließlich der Datenquelle. Dieser semantische Zoom verwendet eine GridView, um die vergrößerte Ansicht und eine ListView für die verkleinerte Ansicht bereitzustellen.
  
**Definieren der vergrößerten Ansicht**

Hier sehen Sie das GridView-Steuerelement für die vergrößerte Ansicht. Die vergrößerte Ansicht sollte die einzelnen Datenelemente in Gruppen anzeigen. Dieses Beispiel zeigt, wie die Elemente in einem Raster mit einem Bild und Text angezeigt werden sollten. 

**XAML**
```xaml
<SemanticZoom.ZoomedInView>
    <GridView ItemsSource="{x:Bind cvsGroups.View}" 
              ScrollViewer.IsHorizontalScrollChainingEnabled="False" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedInTemplate}">
        <GridView.GroupStyle>
            <GroupStyle HeaderTemplate="{StaticResource ZoomedInGroupHeaderTemplate}"/>
        </GridView.GroupStyle>
    </GridView>
</SemanticZoom.ZoomedInView>
```
 
Die Darstellung der Gruppenkopfzeilen wird in der Ressource `ZoomedInGroupHeaderTemplate` definiert. Die Darstellung der Elemente wird in der Ressource `ZoomedInTemplate` definiert. 

**XAML**   
```xaml
<DataTemplate x:Key="" x:DataType="data:ControlInfoDataGroup">
    <TextBlock Text="{x:Bind Title}" 
               Foreground="{ThemeResource ApplicationForegroundThemeBrush}" 
               Style="{StaticResource SubtitleTextBlockStyle}"/>
</DataTemplate>

<DataTemplate x:Key="ZoomedInTemplate" x:DataType="data:ControlInfoDataItem">
    <StackPanel Orientation="Horizontal" MinWidth="200" Margin="12,6,0,6">
        <Image Source="{x:Bind ImagePath}" Height="80" Width="80"/>
        <StackPanel Margin="20,0,0,0">
            <TextBlock Text="{x:Bind Title}" 
                       Style="{StaticResource BaseTextBlockStyle}"/>
            <TextBlock Text="{x:Bind Subtitle}" 
                       TextWrapping="Wrap" HorizontalAlignment="Left" 
                       Width="300" Style="{StaticResource BodyTextBlockStyle}"/>
        </StackPanel>
    </StackPanel>
</DataTemplate>
```

**Definieren der verkleinerten Ansicht**

Dieser XAML-Code definiert ein ListView-Steuerelement für die verkleinerte Ansicht. Dieses Beispiel zeigt, wie die Gruppenkopfzeilen als Text in einer Liste angezeigt werden sollten.

**XAML**
```xaml
<SemanticZoom.ZoomedOutView>
    <ListView ItemsSource="{x:Bind cvsGroups.View.CollectionGroups}" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedOutTemplate}" />
</SemanticZoom.ZoomedOutView>
```

 Die Darstellung wird in der Ressource `ZoomedOutTemplate` definiert.
 
 **XAML**   
```xaml    
<DataTemplate x:Key="ZoomedOutTemplate" x:DataType="wuxdata:ICollectionViewGroup">
    <TextBlock Text="{x:Bind Group.(data:ControlInfoDataGroup.Title)}" 
               Style="{StaticResource SubtitleTextBlockStyle}" TextWrapping="Wrap"/>
</DataTemplate>
```

**Synchronisieren der Ansichten**

Die vergrößerte und verkleinerte Ansicht sollten synchronisiert werden, sodass für den Fall, dass ein Benutzer eine Gruppe in der verkleinerten Ansicht auswählt, die Details dieser Gruppe in der vergrößerten Ansicht angezeigt werden. Sie können eine [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) verwenden oder Code hinzufügen, um die Ansichten zu synchronisieren.

Alle Steuerelemente, die Sie an die gleiche CollectionViewSource binden, verfügen immer über das gleiche aktuelle Element. Wenn beide Ansichten die gleiche CollectionViewSource als Datenquelle verwenden, synchronisiert die CollectionViewSource die Ansichten automatisch. Weitere Informationen finden Sie unter [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx).

Wenn Sie keine CollectionViewSource verwenden, um die Ansichten zu synchronisieren, sollten Sie das [**ViewChangeStarted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.viewchangestarted.aspx)-Ereignis behandeln und die Elemente wie hier gezeigt im Ereignishandler synchronisieren.

**XAML**
```xaml
<SemanticZoom x:Name="semanticZoom" ViewChangeStarted="SemanticZoom_ViewChangeStarted">
```

**C#**
```csharp
private void SemanticZoom_ViewChangeStarted(object sender, SemanticZoomViewChangedEventArgs e)
{
    if (e.IsSourceZoomedInView == false)
    {
        e.DestinationItem.Item = e.SourceItem.Item;
    }
}
```

## Empfehlungen

-   Stellen Sie bei Verwendung des semantischen Zooms in Ihrer App sicher, dass sich das Elementlayout und die Verschieberichtung nicht basierend auf der Zoomstufe ändern. Layouts und Verschiebeinteraktionen sollten auf allen Zoomstufen konsistent und vorhersehbar sein.
-   Der semantische Zoom ermöglicht Benutzern das schnelle Wechseln zu Inhalten; beschränken Sie daher die Anzahl der Seiten oder Bildschirme in der verkleinerten Ansicht auf drei. Zu viel Verschiebung beeinträchtigt die praktische Nutzung des semantischen Zooms.
-   Vermeiden Sie das Verwenden des semantischen Zooms, um den Umfang der Inhalte zu ändern. So sollte beispielsweise ein Fotoalbum nicht zu einer Ordneransicht im Datei-Explorer wechseln.
-   Verwenden Sie eine Struktur und Semantik, die für die Ansicht wichtig sind.
-   Verwenden Sie Gruppennamen für Elemente in einer gruppierten Auflistung.
-   Verwenden Sie die Sortierreihenfolge für eine nicht gruppierte, aber sortierte Sammlung; sortieren Sie beispielsweise Datumsangaben chronologisch und Namenslisten alphabetisch.

## Verwandte Artikel

- [Navigationsdesigngrundlagen](../layout/navigation-basics.md)
- [Listenansicht und Rasteransicht](listview-and-gridview.md)
- [Vorlagen für Listenansichtselemente](listview-item-templates.md)

**Beispiele**

- [Beispiel für XAML-UI-Grundlagen](http://go.microsoft.com/fwlink/p/?LinkId=619992)

 







<!--HONumber=Aug16_HO3-->


