---
author: msatranjr
title: Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten
description: "Sie können mit der MapControl-Klasse anpassbare Karten in Ihrer App anzeigen. In diesem Thema werden auch 3D-Luftbilder und Streetside-Ansichten behandelt."
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: 249503f6a43ef8c38e76ed29aed4a1bfdb26e9fb

---

# Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Sie können mit der [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse anpassbare Karten in Ihrer App anzeigen. In diesem Thema werden auch 3D-Luftbilder und Streetside-Ansichten behandelt.

**Tipp** Um mehr über das Verwenden von Karten in Ihrer App zu erfahren, laden Sie das folgende Beispiel aus den [API-Beispielen für die Universelle Windows-Plattform](http://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub herunter.

-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## Hinzufügen des Kartensteuerelements zur App


Zeigen Sie eine Karte auf einer XAML-Seite durch Hinzufügen eines [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) an. Um **MapControl** zu verwenden, müssen Sie den Namespace [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) in der XAML-Seite oder im Code deklarieren. Wenn Sie das Steuerelement der Toolbox entnehmen, wird die Namespacedeklaration automatisch hinzugefügt. Wenn Sie **MapControl** manuell zur XAML-Seite hinzufügen, müssen Sie die Namespacedeklaration oben auf der Seite manuell hinzufügen.

Das folgende Beispiel zeigt ein einfaches Kartensteuerelement. Außerdem wird die Karte so konfiguriert, dass Toucheingaben möglich sind und gleichzeitig die Steuerelemente für Zoom und Neigung angezeigt werden. Weitere Informationen zum Anpassen der Darstellung der Karte finden Sie unter [Konfigurieren der Karte](#mapconfig).

```xml
<Page
    x:Class="MapsAndLocation1.DisplayMaps"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MapsAndLocation1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:Maps="using:Windows.UI.Xaml.Controls.Maps"
    mc:Ignorable="d">

 <Grid x:Name="pageGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Maps:MapControl
       x:Name="MapControl1"            
       ZoomInteractionMode="GestureAndControl"
       TiltInteractionMode="GestureAndControl"   
       MapServiceToken="EnterYourAuthenticationKeyHere"/>
  
 </Grid>
</Page>
```

Wenn Sie das Kartensteuerelement im Code hinzufügen, müssen Sie den Namespace oben in der Codedatei manuell deklarieren.

```csharp
using Windows.UI.Xaml.Controls.Maps;
...

// Add the MapControl and the specify maps authentication key.
MapControl MapControl2 = new MapControl();
MapControl2.ZoomInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.TiltInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.MapServiceToken = "EnterYourAuthenticationKeyHere";
pageGrid.Children.Add(MapControl2);
```

## Abrufen und Festlegen eines Kartenauthentifizierungsschlüssels


Bevor Sie [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) und Kartendienste verwenden können, müssen Sie den Kartenauthentifizierungsschlüssel als Wert für die [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036)-Eigenschaft angeben. Ersetzen Sie in den vorherigen Beispielen `EnterYourAuthenticationKeyHere` durch den Schlüssel, den Sie über das [Bing Maps Developer Center](https://www.bingmapsportal.com/) abgerufen haben. Bis Sie den Kartenauthentifizierungsschlüssel angeben, wird unterhalb des Steuerelements weiterhin der Text **Warnung: MapServiceToken wurde nicht angegeben** angezeigt. Weitere Informationen zum Abrufen und Festlegen eines Kartenauthentifizierungsschlüssels finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

## Festlegen einer Ausgangsposition für die Karte


Legen Sie die Position fest, die auf der Karte angezeigt werden soll, indem Sie die [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005)-Eigenschaft von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) im Code angeben oder die Eigenschaft im XAML-Markup binden. Im folgenden Beispiel wird eine Karte mit Seattle in der Mitte angezeigt.

**Tipp**  Da eine Zeichenfolge nicht in einen [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) konvertiert werden kann, können Sie nur dann einen Wert für die [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005)-Eigenschaft im XAML-Markup eingeben, wenn Sie die Datenbindung verwenden. (Diese Einschränkung gilt auch für die angefügte Eigenschaft [**MapControl.Location**](https://msdn.microsoft.com/library/windows/apps/dn653264).

 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Specify a known location.
   BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };
   Geopoint cityCenter = new Geopoint(cityPosition);

   // Set the map location.
   MapControl1.Center = cityCenter;
   MapControl1.ZoomLevel = 12;
   MapControl1.LandmarksVisible = true;
}
```

![Beispiel für das Kartensteuerelement](images/displaymapsexample1.png)

## Festlegen der aktuellen Kartenposition


Bevor die App auf die Position des Benutzers zugreifen kann, muss die App die [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)-Methode aufrufen. Zu diesem Zeitpunkt muss sich Ihre App im Vordergrund befinden, und **RequestAccessAsync** muss vom UI-Thread aufgerufen werden. Solange der Benutzer Ihrer App keinen Zugriff auf seine Position gewährt, kann Ihre App nicht auf Positionsdaten zugreifen.

Rufen Sie mit der [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)-Methode der Klasse [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) die aktuelle Position des Geräts ab (wenn die Position verfügbar ist). Zum Abrufen des entsprechenden [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) verwenden Sie die [**Point**](https://msdn.microsoft.com/library/windows/apps/dn263665)-Eigenschaft der Geoposition-Geokoordinate. Weitere Informationen finden Sie unter [Abrufen der aktuellen Position](get-location.md).

```csharp
// Set your current location.
var accessStatus = await Geolocator.RequestAccessAsync();
switch (accessStatus)
{
   case GeolocationAccessStatus.Allowed:

      // Get the current location.
      Geolocator geolocator = new Geolocator();
      Geoposition pos = await geolocator.GetGeopositionAsync();
      Geopoint myLocation = pos.Coordinate.Point;

      // Set the map location.
      MapControl1.Center = myLocation;
      MapControl1.ZoomLevel = 12;
      MapControl1.LandmarksVisible = true;
      break;

   case GeolocationAccessStatus.Denied:
      // Handle the case  if access to location is denied.
      break;

   case GeolocationAccessStatus.Unspecified:
      // Handle the case if  an unspecified error occurs.
      break;
}
```

Beim Anzeigen der Geräteposition auf einer Karte sollten Sie für das Anzeigen von Grafiken und Festlegen des Zoomfaktors die Genauigkeit der Positionsdaten berücksichtigen. Weitere Informationen finden Sie unter [Richtlinien für Apps mit Standortbestimmung](https://msdn.microsoft.com/library/windows/apps/hh465148).

## Ändern der Kartenposition


Rufen Sie zum Ändern der Position, die in einer 2D-Karte angezeigt wird, eine der Überladungen der [**TrySetViewAsync**](https://msdn.microsoft.com/library/windows/apps/dn637060)-Methode auf. Verwenden Sie diese Methode, um neue Werte für [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005), [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068), [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) und [**Pitch**](https://msdn.microsoft.com/library/windows/apps/dn637044) anzugeben. Darüber hinaus können Sie eine optionale Animation angeben, die beim Ändern der Ansicht angezeigt werden soll, indem Sie eine Konstante aus der Enumeration [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) angeben.

Verwenden Sie zum Ändern des Standorts einer 3D-Karte stattdessen die [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296)-Methode. Weitere Informationen finden Sie unter [Anzeigen von 3D-Ansichten](#display3d).

Rufen Sie die [**TrySetViewBoundsAsync**](https://msdn.microsoft.com/library/windows/apps/dn637065)-Methode zum Anzeigen der Inhalte eines [**GeoboundingBox**](https://msdn.microsoft.com/library/windows/apps/dn607949) auf der Karte auf. Diese Methode verwenden Sie beispielsweise, um eine Route oder einen Abschnitt einer Route auf der Karte anzuzeigen. Weitere Informationen finden Sie unter [Anzeigen von Routen und Wegbeschreibungen auf einer Karte](routes-and-directions.md).

## Konfigurieren der Karte


Konfigurieren Sie die Karte und ihre Darstellung, indem Sie die Werte der folgenden Eigenschaften von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) festlegen.

**Karteneinstellungen**

-   Legen Sie das **Zentrum** der Karte auf einen geografischen Punkt fest, indem Sie die Eigenschaft [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) festlegen.
-   Legen Sie die **Zoomstufe** der Karte fest, indem Sie die Eigenschaft [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) auf einen Wert zwischen 1 und 20 Grad festlegen.
-   Legen Sie die **Rotation** der Karte fest, indem Sie die Eigenschaft [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) festlegen, wobei 0 oder 360 Grad = Nord, 90 = Ost, 180 = Süd und 270 =West ist.
-   Legen Sie die **Neigung** der Karte fest, indem Sie die Eigenschaft [**DesiredPitch**](https://msdn.microsoft.com/library/windows/apps/dn637012) auf einen Wert zwischen 0 und 65 Grad festlegen.

**Darstellung der Karte**

-   Geben Sie den **Typ** der Karte an – z. B. Straßenkarte oder Luftbildansicht, indem Sie die Eigenschaft [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) auf eine der [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127)-Konstanten festlegen.
-   Legen Sie das **Farbschema** der Karte als hell oder dunkel fest, indem Sie die Eigenschaft [**ColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637010) auf eine der [**MapColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637003)-Konstanten festlegen.

Zeigen Sie Informationen auf der Karte an, indem Sie die Werte der folgenden Eigenschaften von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) festlegen.

-   Zeigen Sie **Gebäude und Sehenswürdigkeiten** auf der Karte an, indem Sie die Eigenschaft [**LandmarksVisible**](https://msdn.microsoft.com/library/windows/apps/dn637023) aktivieren oder deaktivieren.
-   Zeigen Sie **Features für Fußgänger** auf der Karte an, wie öffentliche Treppen, indem Sie die Eigenschaft [**PedestrianFeaturesVisible**](https://msdn.microsoft.com/library/windows/apps/dn637042) aktivieren oder deaktivieren.
-   Zeigen Sie den **Verkehr** auf der Seite an, indem Sie die Eigenschaft [**TrafficFlowVisible**](https://msdn.microsoft.com/library/windows/apps/dn637055) aktivieren oder deaktivieren.
-   Geben Sie an, ob das **Wasserzeichen** auf der Karte angezeigt wird, indem Sie die Eigenschaft [**WatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn637066) auf eine der [**MapWatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn610749)-Konstanten festlegen.
-   Zeigen Sie eine **Auto- oder Fußgängerroute** auf der Karte an, indem Sie [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) zur Sammlung [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) des Kartensteuerelements hinzufügen. Weitere Informationen und ein Beispiel finden Sie in [Anzeigen von Routen und Wegbeschreibungen auf einer Karte](routes-and-directions.md).

Informationen zum Anzeigen von Ortsmarken, Formen und XAML-Steuerelementen in [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) finden Sie unter [Anzeigen von interessanten Orten (POI) auf einer Karte](display-poi.md).

## Anzeigen von Streetside-Ansichten


Bei einer Streetside-Ansicht handelt es sich um die Straßenansicht eines Standorts, die über dem Kartensteuerelement angezeigt wird.

![Beispiel für eine Streetside-Ansicht des Kartensteuerelements](images/onlystreetside-730width.png)

Betrachten Sie die Vorgänge „innerhalb“ der Streetside-Ansicht getrennt von der ursprünglich im Kartensteuerelement angezeigten Karte. Wenn z. B. der Standort in der Streetside-Ansicht geändert wird, führt dies nicht zu einer Änderung der Position oder der Darstellung der Karte "unter" der Streetside-Ansicht. Nach dem Schließen der Streetside-Ansicht (durch Klicken auf **X** oben rechts im Steuerelement) bleibt die ursprüngliche Karte unverändert.

So zeigen Sie eine Streetside-Ansicht an

1.  Überprüfen Sie mittels [**IsStreetsideSupported**](https://msdn.microsoft.com/library/windows/apps/dn974271), ob Streetside-Ansichten auf dem Gerät unterstützt werden.
2.  Wenn Streetside-Ansichten unterstützt werden, erstellen Sie in der Nähe der angegebenen Position [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360), indem Sie [**FindNearbyAsync**](https://msdn.microsoft.com/library/windows/apps/dn974361) aufrufen.
3.  Bestimmen Sie, ob ein Panorama in der Nähe gefunden wurde, indem Sie überprüfen, ob [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360) ungleich NULL ist.
4.  Wenn ein Panorama in der Nähe gefunden wurde, erstellen Sie eine [**StreetsideExperience**](https://msdn.microsoft.com/library/windows/apps/dn974356) für die [**CustomExperience**](https://msdn.microsoft.com/library/windows/apps/dn974263)-Eigenschaft des Kartensteuerelements.

In diesem Beispiel wird gezeigt, wie Sie eine Streetside-Ansicht ähnlich wie in der Abbildung oben anzeigen.

**Hinweis**  Die Übersichtskarte wird nicht angezeigt, wenn die Größe des Kartensteuerelements zu klein festgelegt wird.

 

```csharp
private async void showStreetsideView()
{
   // Check if Streetside is supported.
   if (MapControl1.IsStreetsideSupported)
   {
      // Find a panorama near Avenue Gustave Eiffel.
      BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 48.858, Longitude = 2.295};
      Geopoint cityCenter = new Geopoint(cityPosition);
      StreetsidePanorama panoramaNearCity = await StreetsidePanorama.FindNearbyAsync(cityCenter);

      // Set the Streetside view if a panorama exists.
      if (panoramaNearCity != null)
      {
         // Create the Streetside view.
         StreetsideExperience ssView = new StreetsideExperience(panoramaNearCity);
         ssView.OverviewMapVisible = true;
         MapControl1.CustomExperience = ssView;
      }
   }
   else
   {
      // If Streetside is not supported
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "Streetside is not supported",
         Content ="\nStreetside views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();            
   }
}
```

## Anzeigen von 3D-Luftbildern


Mithilfe der Klasse [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) können Sie eine 3D-Perspektive der Karte angeben. Die Kartenszene stellt die 3D-Ansicht dar, die in der Karte angezeigt wird. Die Klasse [**MapCamera**](https://msdn.microsoft.com/library/windows/apps/dn974244) stellt die Position der Kamera dar, mit der eine solche Ansicht angezeigt würde.

![](images/mapcontrol-techdiagram.png)

Damit Gebäude und andere Merkmale auf der Kartenoberfläche in 3D angezeigt werden, legen Sie die [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051)-Eigenschaft des Kartensteuerelements auf [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127) fest. Dies ist ein Beispiel für eine 3D-Ansicht mit dem Stil **Aerial3DWithRoads**.

![Beispiel für eine 3D-Kartenansicht](images/only3d-730width.png)

So zeigen Sie eine 3D-Ansicht an

1.  Überprüfen Sie mittels [**Is3DSupported**](https://msdn.microsoft.com/library/windows/apps/dn974265), ob 3D-Ansichten auf dem Gerät unterstützt werden.
2.  Wenn 3D-Ansichten unterstützt werden, legen Sie die [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051)-Eigenschaft des Kartensteuerelements auf [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127) fest.
3.  Erstellen Sie ein [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329)-Objekt mithilfe einer der zahlreichen **CreateFrom**-Methoden, z. B. [**CreateFromLocationAndRadius**](https://msdn.microsoft.com/library/windows/apps/dn974336) und [**CreateFromCamera**](https://msdn.microsoft.com/library/windows/apps/dn974334).
4.  Rufen Sie [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) auf, um die 3D-Ansicht anzuzeigen. Darüber hinaus können Sie eine optionale Animation angeben, die beim Ändern der Ansicht angezeigt werden soll, indem Sie eine Konstante aus der Enumeration [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) angeben.

In diesem Beispiel wird das Anzeigen einer 3D-Ansicht gezeigt.

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 34.134, Longitude = -118.3216};
      Geopoint hwPoint = new Geopoint(hwGeoposition);

      // Create the map scene.
      MapScene hwScene = MapScene.CreateFromLocationAndRadius(hwPoint,
                                                                           80, /* show this many meters around */
                                                                           0, /* looking at it to the North*/
                                                                           60 /* degrees pitch */);
      // Set the 3D view with animation.
      await MapControl1.TrySetSceneAsync(hwScene,MapAnimationKind.Bow);
   }
   else
   {
      // If 3D views are not supported, display dialog.
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "3D is not supported",
         Content = "\n3D views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();
   }
}
```

## Abrufen von Informationen zu Standorten und Elementen


Rufen Sie Informationen zu Positionen auf der Karte ab, indem Sie die folgenden Methoden von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) aufrufen.

-   [
              **GetLocationFromOffset**
            ](https://msdn.microsoft.com/library/windows/apps/dn637016)-Methode – Ruft den geografischen Standort ab, der dem angegebenen Punkt im Viewport des Kartensteuerelements entspricht.
-   [
              **GetOffsetFromLocation**
            ](https://msdn.microsoft.com/library/windows/apps/dn637018)-Methode – Ruft den Punkt im Viewport des Kartensteuerelements ab, der dem angegebenen geografischen Standort entspricht.
-   [
              **IsLocationInView**
            ](https://msdn.microsoft.com/library/windows/apps/dn637022)-Methode – Bestimmt, ob der angegebene geografische Standort aktuell im Viewport des Kartensteuerelements sichtbar ist.
-   [
              **FindMapElementsAtOffset**
            ](https://msdn.microsoft.com/library/windows/apps/dn637014)-Methode – Ruft die Elemente auf der Karte ab, die sich am angegebenen Punkt im Viewport des Kartensteuerelements befinden.

## Behandeln von Benutzerinteraktionen und Änderungen


Sie behandeln Benutzereingabegesten auf der Karte, indem Sie die folgenden Ereignisse von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) behandeln. Rufen Sie Informationen zum geografischen Standort auf der Karte und der physischen Position im Viewport ab, an der die Geste ausgeführt wurde, indem Sie die Werte der [**Location**](https://msdn.microsoft.com/library/windows/apps/dn637091)-Eigenschaft und der [**Position**](https://msdn.microsoft.com/library/windows/apps/dn637093)-Eigenschaft von [**MapInputEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637090) überprüfen.

-   [**MapTapped**](https://msdn.microsoft.com/library/windows/apps/dn637038)
-   [**MapDoubleTapped**](https://msdn.microsoft.com/library/windows/apps/dn637032)
-   [**MapHolding**](https://msdn.microsoft.com/library/windows/apps/dn637035)

Sie stellen fest, ob die Karte geladen wird oder vollständig geladen wurde, indem Sie das [**LoadingStatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn637028)-Ereignis des Steuerelements behandeln.

Sie behandeln Änderungen, die durch Ändern der Karteneinstellungen durch den Benutzer oder die App entstehen, indem Sie die folgenden Ereignisse von [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) behandeln. [Richtlinien für Karten](https://msdn.microsoft.com/library/windows/apps/dn596102)

-   [**CenterChanged**](https://msdn.microsoft.com/library/windows/apps/dn637006)
-   [**HeadingChanged**](https://msdn.microsoft.com/library/windows/apps/dn637020)
-   [**PitchChanged**](https://msdn.microsoft.com/library/windows/apps/dn637045)
-   [**ZoomLevelChanged**](https://msdn.microsoft.com/library/windows/apps/dn637069)

## Verwandte Themen

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Abrufen der aktuellen Position](get-location.md)
* [Entwurfsrichtlinien für Apps mit Standortbestimmung](https://msdn.microsoft.com/library/windows/apps/hh465148)
* [Entwurfsrichtlinien für Karten](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)





<!--HONumber=Jun16_HO5-->


