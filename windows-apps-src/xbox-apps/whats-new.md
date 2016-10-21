---
author: v-angraf
title: "Neuigkeiten für UWP auf Xbox One"
description: "Vorstellung neuer Features für UWP-Apps auf Xbox One."
translationtype: Human Translation
ms.sourcegitcommit: 044aac722180015586487dcc8738facccf209f5c
ms.openlocfilehash: 4cc1e0b495a80e019296b9c3be9e75a37c60224a

---

# Neuigkeiten für Entwickler in der neuesten Aktualisierung von UWP auf Xbox One

Die Developer Preview-Version vom Juli2016 für die Universelle Windows-Plattform (UWP) auf Xbox One enthält die folgenden neuen Features, Featureupdates und Fehlerkorrekturen.

## Netzwerke auf der Basis von TCP/UDP-Sockets sind jetzt verfügbar  
Der ein- und ausgehende Netzwerkzugriff von Konsolen, die herkömmliche TCP/UDP-Sockets (WinSock, Windows.Networking.Sockets) verwenden, ist nun verfügbar.

## Fiddler-Unterstützung
Sie können nun Fiddler als Proxy für eine Konsole aktivieren, für die die Universelle Windows Plattform (UWP) auf Xbox One aktiviert ist. Fiddler ermöglicht Ihnen die Anmeldung und Überprüfung des gesamten HTTP-/HTTPS-Datenverkehrs zu und von Xbox-Diensten und Webdiensten vertrauender Parteien. Weitere Informationen finden Sie unter [Verwendung von Fiddler mit Xbox One bei der Entwicklung für UWP](uwp-fiddler.md).

## Mausmodus nun standardmäßig aktiviert
Der Mausmodus ist jetzt standardmäßig für XAML- und gehostete Web-Apps aktiviert.
Es wird nachdrücklich empfohlen, diese Option zu deaktivieren und die direktionale Navigation über den Controller zu optimieren.
Informationen zum Deaktivieren des Mausmodus finden Sie unter [So wird's gemacht: Deaktivieren des Mausmodus](how-to-disable-mouse-mode.md).
Weitere Informationen dazu, wie Sie großartige Apps für die Xbox entwerfen, finden Sie unter [Entwerfen für Xbox und Fernsehgeräte](../input-and-devices/designing-for-tv.md#mouse-mode).

## Der erweiterte UWP-API-Oberflächenbereich ist jetzt in der Konsole verwendungsbereit.
Zusätzliche UWP-APIs sind jetzt in der Xbox-Konsole verwendungsbereit. Weitere Informationen zur UWP-API-Unterstützung finden Sie unter [UWP-Funktionen, die noch nicht auf Xbox One unterstützt werden](http://go.microsoft.com/fwlink/p/?LinkID=760755). 

## Funktionen für Hintergrundmusik und -audio
Sie können jetzt Musik und Audio einer im Hintergrund ausgeführten App wiedergeben.

## XAML-Verbesserungen
Die XAML-Plattform wurde wie folgt verbessert:
-   Das Fokusrechteck ist jetzt für ein 10-Fuß-TV-Erlebnis gestaltet.
-   Xbox-Sounds sind jetzt in die XAML-Plattform eingebettet.
-   Die XY-Fokusnavigation zwischen UI-Elementen wurde verbessert. 

## Sie können jetzt die Größe des zugewiesenen Entwicklerspeichers auf der Konsole ändern.
Mithilfe einer neuen Einstellung in der Dev Home-App können Sie den zugeordneten Entwicklerspeicher auf der Konsole vergrößern oder verkleinern. Weitere Informationen zum Ändern der Größe des zugeordneten Entwicklerspeichers finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).

## Verbesserungen am WDP-Tool
Am WDP (Windows Device Portal)-Tool für Xbox wurden folgende Verbesserungen vorgenommen:
 - Das Tool enthält zusätzliche Konsoleneinstellungen. Weitere Informationen zu Konsoleneinstellungen finden Sie im Referenzthema [/ext/settings](wdp-xboxsettings-api.md). 
 - Benutzer können auf der Konsole an- und abgemeldet werden. Weitere Informationen zu Benutzern finden Sie im Referenzthema [/ext/user](wdp-user-management.md).
 - Sie können jetzt einen Screenshot der Konsole erstellen. Weitere Informationen zum Erstellen eines Screenshots finden Sie im Referenzthema [/ext/screenshot](wdp-media-capture-api.md).
 - Das Tool kann einen losen Dateibuild Ihrer App bereitstellen. Weitere Informationen zu losen Dateibuilds finden Sie im Referenzthema [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Sie können auf Entwicklerdateien auf der Konsole über einen Datei-Explorer auf Ihrem Entwicklungscomputer zugreifen. Weitere Informationen zum Zugreifen auf Dateien über einen Datei-Explorer finden Sie im Referenzthema [/ext/smb/developerfolder](wdp-smb-api.md).

## Weitere Informationen
- [Bekannte Probleme](known-issues.md)
- [UWP auf XboxOne](index.md)



<!--HONumber=Aug16_HO3-->


