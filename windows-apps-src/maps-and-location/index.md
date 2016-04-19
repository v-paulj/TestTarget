---
title: Übersicht über Karten und Position
description: In diesem Abschnitt wird erläutert, wie Sie in Ihrer App Karten anzeigen, Kartendienste verwenden, die Position suchen und einen Geofence einrichten. Außerdem erfahren Sie in diesem Abschnitt, wie die Windows-Karten-App mit einer bestimmten Karte, Route oder detaillierten Wegbeschreibung gestartet wird.
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
---

# Übersicht über Karten und Position


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Abschnitt wird erläutert, wie Sie in Ihrer App Karten anzeigen, Kartendienste verwenden, die Position suchen und einen Geofence einrichten. Außerdem erfahren Sie in diesem Abschnitt, wie die Windows-Karten-App mit einer bestimmten Karte, Route oder detaillierten Wegbeschreibung gestartet wird.

> **Tipp** Um mehr über das Verwenden von Karten und Positionen in Ihrer App zu erfahren, laden Sie die folgenden Beispiele aus dem [Repository Beispiele für Universelle Windows-Plattform](http://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub herunter:
-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)
-   [UWP-Geolocation-Beispiel](http://go.microsoft.com/fwlink/p/?linkid=533278)

 

## Anzeigen von Karten


Mit APIs aus dem [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751)-Namespace kann Ihre App Karten mit 2D-, 3D- oder Streetside-Ansichten anzeigen. Sie können interessante Orte (POI) auf der Karte mit Ortsmarken, Bildern, Formen oder XAML-UI-Elementen markieren. Außerdem können Sie nebeneinander angeordnete Bilder überlagern oder die Kartenbilder komplett ersetzen.

| Thema | Beschreibung |
|-------|-------------|
| [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md) | Ihre App muss authentifiziert werden, bevor sie die [MapControl](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse und Kartendienste im [Windows.Services.Maps](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace verwenden kann. Zum Authentifizieren Ihrer App müssen Sie einen Kartenauthentifizierungsschlüssel angeben. In diesem Artikel wird beschrieben, wie Sie einen Kartenauthentifizierungsschlüssel vom [Bing Maps Developer Center](https://www.bingmapsportal.com/) anfordern und Ihrer App hinzufügen. |
| [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md) | Sie können mit der [MapControl](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse anpassbare Karten in Ihrer App anzeigen. In diesem Thema werden auch 3D-Luftbilder und Streetside-Ansichten behandelt. |
| [Anzeigen von interessanten Orten (POI) auf einer Karte](display-poi.md) | Hinzufügen interessanter Orte (POI) mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen auf einer Karte. |
| [Überlagern von nebeneinander angeordneten Bildern in einer Karte](overlay-tiled-images.md) | Überlagern Sie Bilder von Drittanbietern oder benutzerdefinierte nebeneinander angeordnete Bilder in einer Karte mithilfe von Kachelquellen. Verwenden Sie Kachelquellen, um spezielle Infos wie Wetterdaten, Einwohnerzahlen oder seismische Daten zu überlagern oder die Standardkarte vollständig zu ersetzen. |
| [Entwurfsrichtlinien für Karten](controls-map.md) | Mithilfe des Kartensteuerelements können Straßenkarten und Luftansichten, Wegbeschreibungen, Suchergebnisse und Verkehr angezeigt werden. |
| [Entwurfsrichtlinien für Apps mit Standortbestimmung](guidelines-and-checklist-for-detecting-location.md) | Leistungsrichtlinien für Apps, für die der Zugriff auf den Standort eines Benutzers erforderlich ist. |


## Zugreifen auf Kartendienste

Fügen Sie Ihrer App Routen, Wegbeschreibungen und Geocodierungsfunktionen mithilfe der APIs aus dem [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace hinzu. Sie können außerdem die Offlineverwaltung von Karten für die Benutzer erleichtern, indem die Einstellungs-App direkt auf der entsprechenden Seite gestartet wird.

| Thema | Beschreibung |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Anzeigen von Routen und Wegbeschreibungen](routes-and-directions.md) | Fordern Sie Routen und Wegbeschreibungen an, und zeigen Sie sie in Ihrer App an. |
| [Durchführen der Geocodierung und umgekehrten Geocodierung](geocoding.md) | Sie konvertieren Adressen in geografische Standorte (Geocodierung) und geografische Standorte in Adressen (umgekehrte Geocodierung), indem Sie die Methoden der [MapLocationFinder](https://msdn.microsoft.com/library/windows/apps/dn627550)-Klasse im [Windows.Services.Maps](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace aufrufen. |


## Abrufen des Benutzerstandorts

Mit APIs aus dem [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603)-Namespace kann Ihre App die aktuelle Position des Benutzers abrufen und Sie über Positionsänderungen benachrichtigen. Diese API-Member werden auch häufig in Parametern der Karten-APIs verwendet. Mit APIs aus dem [**Windows.Devices.Geolocation.Geofencing**](https://msdn.microsoft.com/library/windows/apps/dn263744)-Namespace wird Ihre App benachrichtigt, wenn der Benutzer einen Geofence (einen vordefinierten geografischen Bereich) betritt oder verlässt.

| Thema | Beschreibung |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md) | Ihre App muss authentifiziert werden, bevor sie die [MapControl](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse und Kartendienste im [Windows.Services.Maps](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace verwenden kann. Zum Authentifizieren Ihrer App müssen Sie einen Kartenauthentifizierungsschlüssel angeben. In diesem Artikel wird beschrieben, wie Sie einen Kartenauthentifizierungsschlüssel vom [Bing Maps Developer Center](https://www.bingmapsportal.com/) anfordern und Ihrer App hinzufügen. |
| [Durchführen der Geocodierung und umgekehrten Geocodierung](geocoding.md) | Wandeln Sie Adressen in geografische Standorte (Geocodierung) und geografische Standorte in Adressen (umgekehrte Geocodierung) um, indem Sie die Methoden der MapLocationFinder-Klasse aufrufen. |
| [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md) | Sie können mit der [MapControl](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse anpassbare Karten in Ihrer App anzeigen. In diesem Thema werden auch 3D-Luftbilder und Streetside-Ansichten behandelt. |
| [Karten](controls-map.md) | Mithilfe des Kartensteuerelements können Straßenkarten und Luftansichten, Wegbeschreibungen, Suchergebnisse und Verkehr angezeigt werden. |
| [Anzeigen von interessanten Orten (POI) auf einer Karte](display-poi.md) | Hinzufügen interessanter Orte (POI) mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen auf einer Karte. |
| [Überlagern von nebeneinander angeordneten Bildern in einer Karte](overlay-tiled-images.md) | Überlagern Sie Bilder von Drittanbietern oder benutzerdefinierte nebeneinander angeordnete Bilder in einer Karte mithilfe von Kachelquellen. Verwenden Sie Kachelquellen, um spezielle Infos wie Wetterdaten, Einwohnerzahlen oder seismische Daten zu überlagern oder die Standardkarte vollständig zu ersetzen. |
| [Einrichten von Geofence-Bereichen](set-up-a-geofence.md) | Richten Sie einen Geofence-Bereich in Ihrer App ein, und erfahren Sie, wie Sie Benachrichtigungen im Vordergrund und Hintergrund behandeln. |

## Starten der Windows-Karten-App

Ihre App kann die Windows-Karten-App starten, wie hier veranschaulicht, um bestimmte Karten und detaillierte Wegbeschreibungen anzuzeigen. Anstatt die Kartenfunktionen direkt in Ihrer eigenen App bereitzustellen, können Sie sie auch über die Windows-Karten-App verfügbar machen. Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](https://msdn.microsoft.com/library/windows/apps/mt228341).

![Ein Beispiel der Windows-Karten-App.](images/mapnyc.png)

## Verwandte Themen

* [Beispiel für UWP-Karte](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP-Geolocation-Beispiel](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Abrufen der aktuellen Position](get-location.md)
* [Entwurfsrichtlinien für Apps mit Standortbestimmung](guidelines-and-checklist-for-detecting-location.md)
* [Entwurfsrichtlinien für Karten](controls-map.md)
* [Entwurfsrichtlinien für Apps mit Berücksichtigung von Datenschutz](https://msdn.microsoft.com/library/windows/apps/hh768223)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](http://go.microsoft.com/fwlink/p/?LinkId=619982)





<!--HONumber=Mar16_HO1-->

