---
title: Ausführen der Geocodierung und umgekehrten Geocodierung
description: Konvertieren Sie Adressen in geografische Standorte (Geocodierung) und geografische Standorte in Adressen (umgekehrte Geocodierung), indem Sie die Methoden der MapLocationFinder-Klasse im Windows.Services.Maps-Namespace aufrufen.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
---

# Ausführen der Geocodierung und umgekehrten Geocodierung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Sie konvertieren Adressen in geografische Standorte (Geocodierung) und geografische Standorte in Adressen (umgekehrte Geocodierung), indem Sie die Methoden der [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)-Klasse im [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace aufrufen.

**Tipp** Um mehr über das Verwenden von Karten in Ihrer App zu erfahren, laden Sie das folgende Beispiel aus den [API-Beispielen für die Universelle Windows-Plattform](http://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub herunter.

-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

Hier erfahren Sie, welchen Zusammenhang es zwischen den Klassen für Geocodierung und umgekehrte Geocodierung gibt:

-   Die [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)-Klasse verfügt über Methoden zur Geocodierung ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) und umgekehrten Geocodierung ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)).
-   Diese Methoden geben ein [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) zurück.
-   [
            **MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) enthält eine Sammlung von [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)-Objekten. Auf diese Sammlung greifen Sie über die [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552)-Eigenschaft von **MapLocationFinderResult** zu.
-   Jedes [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)-Objekt enthält ein [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533)-Objekt. Auf dieses Objekt greifen Sie über die [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929)-Eigenschaft des jeweiligen **MapLocation** zu.

**Wichtig**  Sie müssen einen Kartenauthentifizierungsschlüssel angeben, bevor Sie Kartendienste verwenden können. Weitere Informationen finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

 

## Abrufen eines Standorts (Geocode)


Sie konvertieren eine Adresse oder einen Ortsnamen in einen geografischen Standort (Geocodierung), indem Sie die folgenden Schritte ausführen.

1.  Rufen Sie eine der Überladungen der [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)-Methode der [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)-Klasse auf.
2.  Die [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)-Methode gibt ein [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)-Objekt zurück, das eine Sammlung übereinstimmender [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)-Objekte enthält.
3.  Auf diese Sammlung greifen Sie über die [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552)-Eigenschaft von [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) zu.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

Dieser Code zeigt die folgenden Ergebnisse im `tbOutputText`-Textfeld an:

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## Abrufen einer Adresse (umgekehrte Geocodierung)


Sie konvertieren einen geografischen Standort in eine Adresse (umgekehrte Geocodierung), indem Sie die folgenden Schritte ausführen.

1.  Rufen Sie die [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)-Methode der [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)-Klasse auf.
2.  Die [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)-Methode gibt ein [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)-Objekt zurück, das eine Sammlung übereinstimmender [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)-Objekte enthält.
3.  Auf diese Sammlung greifen Sie über die [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552)-Eigenschaft von [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) zu.
4.  Auf das [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533)-Objekt greifen Sie über die [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929)-Eigenschaft jedes [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)-Objekts zu.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

Dieser Code zeigt die folgenden Ergebnisse im `tbOutputText`-Textfeld an:

``` syntax
town = Redmond
```

## Verwandte Themen

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Entwurfsrichtlinien für Karten](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)




<!--HONumber=Mar16_HO1-->


