---
author: QuinnRadich
title: Neuigkeiten in Windows 10, Version 1607 Preview
description: "Windows10, Version1607 Preview, und neue Entwicklertools stellen Werkzeuge, Features und Umgebungen zur Verfügung, die von der neuen universellen Windows-Plattform unterstützt werden."
keywords: Neuigkeiten, was neu ist, Aktualisierung, Updates, Features, neu, Windows 10 1607 Preview
translationtype: Human Translation
ms.sourcegitcommit: 5646bf7681b5b028031eab02f8dd5c352d4b9cc1
ms.openlocfilehash: 33c1888620d4e3c2d95cbf701e9128ce006961da

---

# Neuigkeiten in Windows

Windows10, Version 1607 Preview, und Updates für Windows-Entwicklertools stellen weiterhin Tools, Features und Umgebungen bereit, die von der universellen Windows-Plattform unterstützt werden. Nach der [Installation der Tools und des SDKs](http://go.microsoft.com/fwlink/?LinkId=821431) unter Windows10 können Sie entweder [eine neue universelle Windows-App erstellen](https://msdn.microsoft.com/library/windows/apps/bg124288) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](https://msdn.microsoft.com/library/windows/apps/mt238321) vertraut machen.

## Windows10, Version 1607 Preview

Feature | Beschreibung
 :---- | :----
Netzwerk | Sie können nun Ihre eigene angepasste Validierung von SSL/TLS-Serverzertifikaten bereitstellen, indem Sie das Ereignis [HttpBaseProtocolFilter.ServerCustomValidationRequest](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank) abonnieren. Außerdem können Sie das Lesen von HTTP-Antworten aus dem Cache vollständig deaktivieren, indem Sie in einer HTTP-Anforderung den Enumerationswert [HttpCacheReadBehavior.NoCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpcachereadbehavior.aspx#_blank) angeben. Sie können nun Authentifizierungsinformationen löschen, um ein „Abmeldeszenario“ zu ermöglichen, indem Sie die Methode [HttpBaseProtocolFilter.ClearAuthenticationCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank) aufrufen.
Erweiterungen | Neu in Microsoft Edge ist die Fähigkeit, Erweiterungen zu verwenden. Mithilfe von Erweiterungen können Benutzer die Funktionen von Microsoft Edge erweitern, was die Bereitstellung von Nischenfunktionen für bestimmte Zielgruppen ermöglicht. Sehen Sie sich die [Dokumentation zu Erweiterungen](https://developer.microsoft.com/microsoft-edge/platform/documentation/extensions/#_blank) an, um weitere Informationen zu erhalten.
Bluetooth-APIs | Apps können nun über [Windows.Devices.Bluetooth und Windows.Devices.Bluetooth.Rfcomm](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx#_blank) ohne vorherige Koppelung auf RFCOMM-Dienste von Bluetooth-Remoteperipheriegeräten zugreifen. Neue Methoden ermöglichen Apps das Durchsuchen von und den Zugriff auf RFCOMM-Dienste auf nichtgekoppelten Geräten.
Chat-APIs | Mithilfe der neuen [ChatSyncManager](https://msdn.microsoft.com/library/windows/apps/mt414181.aspx#_blank)-Klasse können Sie SMS-Nachrichten in und aus der Cloud synchronisieren.
[Windows-Apps-Konzeptzuordnung für Android- und iOS-Entwickler](https://msdn.microsoft.com/windows/uwp/porting/android-ios-uwp-map#_blank) | Wenn Sie Entwickler mit Android- oder iOS-Kenntnissen sind und/oder über den entsprechenden Code verfügen und zu Windows10 und zur universellen Windows-Plattform (UWP) wechseln möchten, bietet Ihnen dieser Artikel alle Informationen, die Sie für die Zuordnung der Plattformfeatures –und Ihrer Kenntnisse –zwischen den drei Plattformen benötigen.
[Unternehmensdatenschutz (Enterprise Data Protection, EDP)](https://msdn.microsoft.com/windows/uwp/enterprise/edp-hub?branch=build2016#_blank) | Der Unternehmensdatenschutz (Enterprise Data Protection, EDP) ist ein Satz von Features auf Desktops, Laptops, Tablets und Smartphones für das Mobile Device Management (MDM). EDP bietet Unternehmen eine bessere Kontrolle über die Behandlung von Daten (Unternehmensdateien und Datenblobs) auf Geräten, die das Unternehmen verwaltet.
[Windows.ApplicationModel.AppExtensions](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appextensions.aspx#_blank) | Der neue Namespace AppExtensions ermöglicht Ihrer Windows Store-App das Hosten von Inhalten, die von anderen Windows Store-Apps bereitgestellt werden. Sie können die Inhalte dieser Apps ermitteln und enumerieren und auf schreibgeschützte Inhalte aus diesen Apps zugreifen.
Windows IoT | Windows10 IoT Core ermöglicht Ihnen die Erstellung von IoT-Anwendungen in der vertrauten Windows-Umgebung und ist nun auf Raspberry Pi 3 verfügbar, dem neuesten Raspberry Pi-Board.
Medien-APIs | Mit den neuen MediaBreak-APIs im Windows.Media.Playback-Namespace können Sie auf einfache Weise Medienunterbrechungen planen und verwalten, wenn Sie Medien mit MediaSource und MediaPlaybackItem wiedergeben. Neue AudioGraph-APIs im Windows.Media.Audio-Namespace fügen eine räumliche Audioverarbeitung hinzu, mit der Sie 3D-positionierte Emitter und Listener zu Audiodiagrammknoten zuweisen können.
Karten-APIs | [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx#_blank) wurde verbessert, sodass Entwickler einen sichtbaren Bereich in der Nähe der Kamera abrufen können, wobei Regionen in großer Entfernung und nahe dem Horizont in besonders schrägen Ansichten ausgeschlossen werden. Die [MapLocationFinder](https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx#_blank)-Klasse wurde erweitert, sodass Entwickler den Netzwerkdatenverkehr für das umgekehrte Geocoding optimieren können, indem sie die gewünschte Genauigkeit angeben. Entwickler können nun die Vorteile des Downloads von Offlinekarten nutzen, indem sie die Methode [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx#_blank) verwenden und den Breiten- und Längengrad angeben. Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](https://msdn.microsoft.com/windows/uwp/launch-resume/launch-maps-app#_blank).



<!--HONumber=Aug16_HO4-->


