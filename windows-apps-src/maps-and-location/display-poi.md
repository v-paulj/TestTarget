---
author: PatrickFarley
title: 'Anzeigen von interessanten Orten (POI) auf einer Karte'
description: Mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen können Sie interessante Orte (Points of Interest, POI) auf einer Karte hinzufügen.
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
---

# Anzeigen von interessanten Orten (POI) auf einer Karte


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen können Sie interessante Orte (Points of Interest, POI) auf einer Karte hinzufügen. Ein POI ist ein Punkt auf der Karte, der Orte angibt, die von Interesse sind. Beispiele sind die Position eines Geschäfts, eines Orts oder eines Freundes.

**Tipp:** Um mehr über das Anzeigen von interessanten Orten in Ihrer App zu erfahren, laden Sie das folgende Beispiel aus dem [Repository Beispiele für Universelle Windows-Plattform](http://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub herunter.

-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

Zeigen Sie Ortsmarken, Bilder und Formen auf der Karte an, indem Sie [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)-, [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)- und [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)-Objekte zur Sammlung [**MapElements**](https://msdn.microsoft.com/library/windows/apps/dn637033) der Kartensteuerung hinzufügen. Verwenden Sie Datenbindung, oder fügen Sie Elemente programmgesteuert hinzu. Eine deklarative Bindung an die Sammlung **MapElements** im XAML-Markup ist nicht möglich.

Zeigen Sie XAML-UI-Elemente wie [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265), [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) oder [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) auf der Karte an, indem Sie diese als [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) hinzufügen. Sie können sie auch zu [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) hinzufügen oder **MapItemsControl** an eine Sammlung von Elementen binden.

Zusammenfassung:

-   [Fügen Sie der Karte ein MapIcon hinzu](#mapicon), um ein Bild anzuzeigen, z. B. eine Ortsmarke mit optionalem Text.
-   [Fügen Sie der Karte ein MapPolygon hinzu](#mappolygon), um eine Form mit mehreren Punkten anzuzeigen.
-   [Fügen Sie der Karte eine MapPolyline hinzu](#mappolyline), um Linien auf der Karte anzuzeigen.
-   [Fügen Sie der Karte XAML hinzu](#mapxaml), um benutzerdefinierte UI-Elemente anzuzeigen.

Wenn Sie eine große Anzahl von Elementen auf der Karte platzieren möchten, sollten Sie [nebeneinander angeordnete Bilder auf der Karte überlagern](overlay-tiled-images.md). Informationen zum Anzeigen von Straßen auf der Karte finden Sie unter [Anzeigen von Routen und Wegbeschreibungen](routes-and-directions.md).

## Hinzufügen eines MapIcon


Verwenden Sie die Klasse [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077), um Bilder wie eine Ortsmarke mit optionalem Text auf der Karte anzuzeigen. Sie können das Standardbild akzeptieren oder mit der [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078)-Eigenschaft ein benutzerdefiniertes Bild bereitstellen. Die folgende Abbildung zeigt das Standardbild für ein **MapIcon** ohne festgelegten Wert für die Eigenschaft [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) mit einem kurzen Titel, einem langen Titel und einem sehr langen Titel.

![Beispiel für MapIcon mit Titeln unterschiedlicher Länge](images/mapctrl-mapicons.png)

Im folgenden Beispiel wird einer Karte von Seattle ein [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) mit Standardbild und optionalem Titel hinzugefügt, um den Standort der Space Needle anzugeben. Außerdem wird die Karte auf dem Symbol zentriert und vergrößert. Allgemeine Informationen zur Verwendung des Kartensteuerelements finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md).

```csharp
      private void displayPOIButton_Click(object sender, RoutedEventArgs e)
      {
         // Specify a known location.
         BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
         Geopoint snPoint = new Geopoint(snPosition);

         // Create a MapIcon.
         MapIcon mapIcon1 = new MapIcon();
         mapIcon1.Location = snPoint;
         mapIcon1.NormalizedAnchorPoint = new Point(0.5, 1.0);
         mapIcon1.Title = "Space Needle";
         mapIcon1.ZIndex = 0;

         // Add the MapIcon to the map.
         MapControl1.MapElements.Add(mapIcon1);

         // Center the map over the POI.
         MapControl1.Center = snPoint;
         MapControl1.ZoomLevel = 14;
      }
```

In diesem Beispiel wird der folgende interessante Ort (POI) auf der Karte angezeigt (mit dem Standardbild in der Mitte).

![Karte mit MapIcon](images/displaypoidefault.png)

Die folgende Codezeile zeigt [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) mit einem benutzerdefinierten Bild an, das im Ordner „Assets“ (Ressourcen) des Projekts gespeichert wurde. Die Eigenschaft [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) von **MapIcon** erwartet einen Wert vom Typ [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813). Dieser Typ erfordert die Anweisung **using** für den Namespace [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791).

**Tipp** Wenn Sie dasselbe Bild für mehrere Kartensymbole verwenden, deklarieren Sie [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) auf Seiten- oder App-Ebene, um eine optimale Leistung zu erzielen.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

Berücksichtigen Sie beim Arbeiten mit der [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)-Klasse Folgendes:

-   Die [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078)-Eigenschaft unterstützt eine maximale Bildgröße von 2048 x 2048 Pixeln.
-   Standardmäßig wird die Anzeige des Bilds für das Kartensymbol nicht garantiert. Es wird möglicherweise ausgeblendet, wenn es andere Elemente oder Bezeichnungen auf der Karte verdeckt. Damit es sichtbar bleibt, legen Sie die [**CollisionBehaviorDesired**](https://msdn.microsoft.com/library/windows/apps/dn974327)-Eigenschaft des Kartensymbols auf [**MapElementCollisionBehavior.RemainVisible**](https://msdn.microsoft.com/library/windows/apps/dn974314) fest.
-   Die Anzeige des optionalen [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) für [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) wird nicht garantiert. Wenn der Text nicht angezeigt wird, verkleinern Sie die Ansicht, indem Sie den Wert der [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068)-Eigenschaft von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) verringern.
-   Wenn Sie ein [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)-Bild anzeigen, das auf eine bestimmte Position auf der Karte hinweist, z. B. eine Ortsmarkierung oder ein Pfeil, sollten Sie in Erwägung ziehen, den Wert der [**NormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637082)-Eigenschaft auf den ungefähren Standort des Zeigers auf dem Bild festzulegen. Wenn Sie den Wert von **NormalizedAnchorPoint** beim Standardwert (0, 0) belassen, der die obere linke Ecke des Bilds darstellt, führen Änderungen am [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) der Karte möglicherweise dazu, dass das Bild auf eine andere Position zeigt.

## Hinzufügen eines MapPolygon


Mithilfe der Klasse [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) können Sie eine Form mit mehreren Punkten auf der Karte anzeigen. Im folgenden Beispiel (aus dem [Beispiel zu UWP-Karten](http://go.microsoft.com/fwlink/p/?LinkId=619977)) wird ein rotes Feld mit blauem Rahmen auf der Karte angezeigt.

```csharp
private void mapPolygonAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
   double centerLatitude = myMap.Center.Position.Latitude;
   double centerLongitude = myMap.Center.Position.Longitude;
   MapPolygon mapPolygon = new MapPolygon();
   mapPolygon.Path = new Geopath(new List<BasicGeoposition>() {
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },

   });
           
   mapPolygon.ZIndex = 1;
   mapPolygon.FillColor = Colors.Red;
   mapPolygon.StrokeColor = Colors.Blue;
   mapPolygon.StrokeThickness = 3;
   mapPolygon.StrokeDashed = false;
   myMap.MapElements.Add(mapPolygon);
}
```

## Hinzufügen einer MapPolyline


Mithilfe der Klasse [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) können Sie eine Linie auf der Karte anzeigen. Im folgenden Beispiel (aus dem [Beispiel zu UWP-Karten](http://go.microsoft.com/fwlink/p/?LinkId=619977)) wird eine gestrichelte Linie auf der Karte angezeigt.

```csharp
private void mapPolylineAddButton_Click(object sender, Windows.UI.Xaml.RoutedEventArgs e)
{
   double centerLatitude = myMap.Center.Position.Latitude;
   double centerLongitude = myMap.Center.Position.Longitude;
   MapPolyline mapPolyline = new MapPolyline();
   mapPolyline.Path = new Geopath(new List<BasicGeoposition>() {                
         new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },                
         new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
   });
              
   mapPolyline.StrokeColor = Colors.Black;
   mapPolyline.StrokeThickness = 3;
   mapPolyline.StrokeDashed = true;
   myMap.MapElements.Add(mapPolyline);
}
```

## Hinzufügen von XAML


Zeigen Sie benutzerdefinierte UI-Elemente mit XAML auf der Karte an. Positionieren Sie XAML auf der Karte, indem Sie die Position und den normalisierten Ankerpunkt für den XAML-Code angeben.

-   Rufen Sie [**SetLocation**](https://msdn.microsoft.com/library/windows/desktop/ms704369) auf, um die Position auf der Karte festzulegen, an der der XAML-Code platziert werden soll.
-   Rufen Sie [**SetNormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637050) auf, um die relative Position des XAML-Codes festzulegen, die der angegebenen Position entspricht.

Im folgenden Beispiel wird einer Karte von Seattle das XAML-Steuerelement [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) hinzugefügt, um den Standort der Space Needle anzugeben. Außerdem wird die Karte in diesem Bereich zentriert und vergrößert. Allgemeine Informationen zur Verwendung des Kartensteuerelements finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md).

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition() { Latitude = 47.620, Longitude = -122.349 };
   Geopoint snPoint = new Geopoint(snPosition);

   // Create a XAML border.
   Border border = new Border
   {
      Height = 100,
      Width = 100,
      BorderBrush = new SolidColorBrush(Windows.UI.Colors.Blue),
      BorderThickness = new Thickness(5),
   };

   // Center the map over the POI.
   MapControl1.Center = snPoint;
   MapControl1.ZoomLevel = 14;

   // Add XAML to the map.
   MapControl1.Children.Add(border);
   MapControl.SetLocation(border, snPoint);
   MapControl.SetNormalizedAnchorPoint(border, new Point(0.5, 0.5));
}
```

In diesem Beispiel wird ein blauer Rahmen auf der Karte angezeigt.

![](images/displaypoixaml.png)

Die nächsten Beispiele zeigen, wie XAML-UI-Elemente mithilfe der Datenbindung direkt im XAML-Markup der Seite hinzugefügt werden. Wie bei anderen XAML-Elementen zur Anzeige von Inhalten auch, stellt [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) die standardmäßige Inhaltseigenschaft von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) dar und muss nicht explizit im XAML-Markup angegeben werden.

In diesem Beispiel wird gezeigt, wie zwei XAML-Steuerelemente als implizite untergeordnete Objekte von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) angezeigt werden.

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
</maps:MapControl>
```

In diesem Beispiel wird gezeigt, wie zwei XAML-Steuerelemente in einem [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) angezeigt werden.

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{Binding SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{Binding BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

In diesem Beispiel wird eine Sammlung von XAML-Elementen angezeigt, die an [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) gebunden sind.

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{Binding StateOverlays}">
    <maps:MapItemsControl.ItemTemplate>
      <DataTemplate>
        <StackPanel Background="Black" Tapped="Overlay_Tapped">
          <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Name}" maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
        </StackPanel>
      </DataTemplate>
    </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

## Verwandte Themen

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Entwurfsrichtlinien für Karten](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)
* [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)
* [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)




<!--HONumber=May16_HO2-->


