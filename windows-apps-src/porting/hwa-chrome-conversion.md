---
author: seksenov
title: "Gehostete Web-Apps – Konvertieren Ihrer Chrome-App in eine App für die Universelle Windows-Plattform"
description: "Konvertieren Sie Ihre Chrome-App oder Chrome-Erweiterung in eine App für die Universelle Windows-Plattform (UWP) für den Windows Store."
kw: Package Chrome Extension for Windows Store tutorial, Port Chrome Extension to Windows 10, How to convert Chrome App to Windows, How to add Chrome Extension to Windows Store, hwa-cli, Hosted Web Apps Command Line Interface CLI Tool, Install Chrome Extension on Windows 10 Device, convert .crx to .AppX
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 7847f69c85708cb42b878253839b06929f837708

---

# Konvertieren Ihrer vorhandenen Chrome-App in eine App für die Universelle Windows-Plattform

Wir haben die Konvertierung vorhandener gehosteter Chrome-Apps in Apps für die Universelle Windows-Plattform (UWP) einfach gemacht. Es gibt zwei Möglichkeiten, Ihre Chrome-App zu konvertieren:

- Option Nr.1: [ManifoldJS](http://manifoldjs.com/) akzeptiert nun Chrome-Manifeste als eine Form der Eingabe. 

- Option Nr.2: Wir haben ein [CLI-Tool](https://github.com/MicrosoftEdge/hwa-cli) entwickelt, das ein `.appx`-Paket aus Ihren vorhandenen `.zip` oder `.crx` Dateien generiert.

## Konvertieren Sie Ihre vorhandene Chrome-App mittels der Befehlszeilenschnittstelle

1. Installieren Sie [NodeJS](https://nodejs.org/en/) und dessen Paket-Manager [npm](https://www.npmjs.com/). 


2. Öffnen ein Eingabeaufforderungsfenster zum Verzeichnis Ihrer Wahl.


3. Installieren Sie die Befehlszeilenschnittstelle (Command Line Interface, CLI) für gehostete Web-Apps (Hosted Web Apps, HWA), indem Sie den folgenden Befehl in der Befehlszeile eingeben: `npm i -g hwa-cli`

4. Konvertieren Sie Ihr Chrome-Paket (`.crx` und `.zip` werden als Paketformat unterstützt), indem Sie den folgenden Befehl in der Befehlszeile eingeben: `hwa convert path/to/chrome/app.crx`, oder `hwa convert path/to/chrome/app.zip`

    **Ersetzen Sie `path/to/chrome/app` durch die Pfadinformationen für Ihre Chrome-App.*
    
5. Die generierte `.appx` wird im gleichen Ordner wie das Chrome-Paket angezeigt. Sie können Ihre App nun zum Windows Store-hochladen. 

## Hochladen Ihrer App zum Windows Store

Um Ihre App hochzuladen, rufen Sie im [Windows Dev Center](https://developer.microsoft.com/windows) das Dashboard auf. Klicken Sie auf „[Erstellen einer neuen App](https://developer.microsoft.com/dashboard/Application/New)“, und reservieren Sie den App-Namen.
![Reservieren eines Namens im Windows Dev Center-Dashboard](images/hwa-to-uwp/reserve_a_name.png)


Laden Sie Ihr `AppX`-Paket hoch, indem Sie im Abschnitt „Übermittlungen“ zur Seite „Pakete“ navigieren.

Befolgen Sie die Windows Store-Anweisungen.

    During the conversion process, you will be prompted for an Identity Name, Publisher Identity, and Publisher Display Name. To retrieve these values, visit the Dashboard in the [Windows Dev Center](https://developer.microsoft.com/windows).
    - Click on "[Create a new app](https://developer.microsoft.com/dashboard/Application/New)" and reserve your app name.
![Reservieren eines Namens im Windows Dev Center-Dashboard](images/hwa-to-uwp/reserve_a_name.png)
    – Klicken Sie als Nächstes im Menü links unter dem Abschnitt „App-Verwaltung“ auf „App-Identität“.
    ![Windows Dev Center-Dashboard – App-Identität](images/hwa-to-uwp/app_identity.png)
    – Auf Seite 1 sollten Ihnen die drei Werte angezeigt werden, für die Sie eine Aufforderung erhalten haben. Identitätsname: `Package/Identity/Name`
        2. Veröffentlicher-ID: `Package/Identity/Publisher`
        3. Anzeigename des Veröffentlichers: `Package/Properties/PublisherDisplayName`


## Handbuch für die Migration Ihrer gehosteten Web-App

Passen Sie Ihre Web-App nach dem Verpacken für den Windows Store an, damit sie auf allen Windows-basierten Geräten gut funktioniert, darunter PCs, Tablets, Smartphones, HoloLens, Surface Hub, Xbox und Raspberry Pi.

### Regeln für den Anwendungsinhalt-URI

[Regeln für Anwendungsinhalt-URIs (Application Content URI Rules, ACURs)](/hwa-access-features.md#keep-your-app-secure-setting-application-content-uri-rules-acurs) bzw. Inhalts-URIs definieren den Bereich Ihrer gehosteten Web-App über eine URL-Zulassungsliste im Manifest für das App-Paket. Um die Kommunikation zu und von Remoteinhalten zu steuern, müssen Sie die URLs definieren, die in dieser Liste enthalten oder von dieser Liste ausgeschlossen werden. Wenn ein Benutzer auf eine URL klickt, die nicht explizit enthalten ist, öffnet Windows den Zielpfad im Standardbrowser. Mit ACURs können Sie einer URL auch Zugriff auf [universelle Windows-APIs](https://msdn.microsoft.com/library/windows/apps/br211377.aspx) gewähren.

Ihre Regeln sollten mindestens die Startseite Ihrer App enthalten. Das Konvertierungstool erstellt automatisch einen Satz von ACURs für Sie, basierend auf Ihrer Startseite und deren Domäne. Wenn es jedoch programmbasierte Umleitungen gibt, ob auf dem Server oder auf dem Client, müssen diese Ziele der Zulassungsliste hinzugefügt werden.

*Hinweis: ACURs gelten nur für die Seitennavigation. Bilder, JavaScript-Bibliotheken und andere vergleichbare Ressourcen sind von diesen Einschränkungen nicht betroffen.*

Viele Apps verwenden Drittanbieterwebsites für ihre Anmeldungsabläufe, z.B. Facebook und Google. Das Konvertierungstool erstellt automatisch einen Satz von ACURs für Sie, basierend auf den am häufigsten verwendeten Websites. Wenn Ihre Authentifizierungsmethode nicht in dieser Liste enthalten ist und es sich um eine Umleitung handelt, müssen Sie den Pfad/die Pfade als eine ACUR hinzufügen. Sie können auch die Verwendung eines [Webauthentifizierungsbrokers](/hwa-access-features.md#web-authentication-broker) in Betracht ziehen.

### Flash

Flash ist in Windows 10-Apps nicht zulässig. Sie müssen sicherstellen, dass dies keine Auswirkungen auf die App-Umgebung hat.

In Bezug auf Anzeigen müssen Sie sicherstellen, dass der Werbeanbieter über eine HTML5-Option verfügt. Sie können sich in [Bing Ads](https://bingads.microsoft.com/) und [Anzeigen in Apps](http://adsinapps.microsoft.com/) näher informieren.

YouTube-Videos sollten weiter funktionieren, da sie nun [standardmäßig HTML5 verwenden `<video>`,](http://youtube-eng.blogspot.com/2015/01/youtube-now-defaults-to-html5_27.html), solange Sie die [`<iframe>` embed-Methode](https://developers.google.com/youtube/iframe_api_reference) verwenden. Wenn Ihre App noch die Flash-API verwendet, müssen Sie zum oben genannten Einbettungsstil wechseln.

### Bildressourcen

Der Chrome-Webstore [erfordert bereits ein Symbolbild mit der Größe 128x128 für Ihre App](https://developer.chrome.com/webstore/images) in Ihrem App-Paket. In Bezug auf Windows10-Apps müssen Sie mindestens App-Symbolbilder mit den Größen 44x44, 50x50, 150x150 und 600x350 bereitstellen. Das Konvertierungstool erstellt diese Bilder automatisch für Sie, basierend auf dem Bild mit 128x128. Für eine umfassendere und hochwertigere App-Umgebung empfehlen wir nachdrücklich das Erstellen eigener Bilddateien. Im Folgenden finden Sie einige [Richtlinien für Kachel- und Symbolressourcen](https://msdn.microsoft.com/library/windows/apps/mt412102.aspx).

### Funktionen

Die App-Funktionen müssen im Paketmanifest [deklariert](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations) werden, um auf bestimmte APIs und Ressourcen zugreifen zu können. Das Konvertierungstool aktiviert automatisch drei häufig verwendete Gerätefunktionen für Sie: Standort, Mikrofon und Webcam. In Bezug auf die erste Funktion fordert das System den Benutzer nach wie vor zur Genehmigung auf, bevor der Zugriff gewährt wird.

*Hinweis: Benutzer werden über alle Funktionen benachrichtigt, die von einer App deklariert werden. Wir empfehlen, alle Funktionen zu entfernen, die Ihre App nicht benötigt.*

### Dateidownloads

Herkömmliche Dateidownloads wie im Browser werden zurzeit nicht unterstützt.

### Chrome-Plattform-APIs

Chrome stellt Apps [spezielle APIs](https://developer.chrome.com/apps/api_index) bereit, die als Hintergrundskript ausgeführt werden können. Diese werden nicht unterstützt. Sie finden unter den [Windows-Runtime-APIs](https://msdn.microsoft.com/library/windows/apps/br211377.aspx) gleichwertige und viele weitere Funktionen.

## Verwandte Themen

- [Optimieren Ihrer Web-App durch den Zugriff auf Features für die Universelle Windows-Plattform (UWP)](/hwa-access-features.md)
- [Anleitung für Apps für die Universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Herunterladen von Designressourcen für Windows Store-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)



<!--HONumber=Aug16_HO3-->


