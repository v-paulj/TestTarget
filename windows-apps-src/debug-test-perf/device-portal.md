---
author: mcleblanc
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: "Übersicht über das Windows Device Portal"
description: "Hier erfahren Sie, wie Sie mit dem Windows Device Portal Ihr Gerät per Remotezugriff über ein Netzwerk oder eine USB-Verbindung konfigurieren und verwalten."
ms.sourcegitcommit: c6f00006e656970e4a5bb11e3368faa92cbb8eca
ms.openlocfilehash: fe4945bf3048a0c38e844a74fa6fc46706085d6d

---
# Übersicht über das Windows Device Portal

Mit dem Windows Device Portal können Sie Ihr Gerät per Remotezugriff über ein Netzwerk oder eine USB-Verbindung konfigurieren und verwalten. Es bietet zudem erweiterte Diagnosetools, mit denen Sie Probleme beim Windows-Gerät behandeln und dessen Leistung in Echtzeit anzeigen können.

Das Geräteportal ist ein Webserver auf Ihrem Gerät, mit dem Sie über einen Webbrowser auf dem PC eine Verbindung herstellen können. Wenn Ihr Gerät über einen Webbrowser verfügt, können Sie auch mit dem Browser auf dem Gerät eine lokale Verbindung herstellen.

Das Windows Device Portal ist für jede Gerätefamilie verfügbar, die Features und die Einrichtung variieren jedoch abhängig von den Anforderungen des Geräts. Dieser Artikel enthält eine allgemeine Beschreibung des Device Portals und Links zu Artikeln mit ausführlicheren Informationen für jede Gerätefamilie.

Alle Komponenten im Windows Device Portal basieren auf [REST-APIs](device-portal-api-core.md), die Sie für den Zugriff auf die Daten und die programmatische Steuerung Ihres Geräts verwenden können.

## Setup

Für jedes Gerät gelten spezielle Anweisungen zum Herstellen der Verbindung mit dem Device Portal, diese allgemeinen Schritte sind jedoch für jedes Gerät erforderlich:
1. Aktivieren Sie den Entwicklermodus und das Device Portal auf Ihrem Gerät.
2. Verbinden Sie das Gerät und den PC über das lokale Netzwerk oder per USB.
3. Navigieren Sie im Browser zu der Seite für das Geräteportal. In dieser Tabelle sind die Ports und Protokolle aufgeführt, die von jeder Gerätefamilie verwendet werden.

Gerätefamilie | Standardmäßig aktiviert? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Ja, im Entwicklermodus | 80 (Standard) | 443 (Standard) | localhost:10080
IoT | Ja, im Entwicklermodus | 8080 | Über Registrierungsschlüssel aktivieren | N/V
Xbox | Im Entwicklermodus aktivieren | Deaktiviert | 11443 | N/V
Desktop| Im Entwicklermodus aktivieren | Zufallswert > 50.000 (xx080) | Zufallswert > 50.000 (xx443) | N/V
Phone | Im Entwicklermodus aktivieren | 80| 443 | localhost:10080

Gerätespezifische Anweisungen zum Einrichten finden Sie in folgenden Artikeln:
- [Device Portal für HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Device Portal für IoT](http://ms-iot.github.io/content/win10/tools/DevicePortal.htm)
- [Device Portal für Mobilgeräte](device-portal-mobile.md#set-up-device-portal-on-window-phone)
- [Device Portal für Xbox](device-portal-xbox.md)
- [Device Portal für Desktop](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## Features

### Symbolleiste und Navigation

Die Symbolleiste am oberen Rand der Seite ermöglicht den Zugriff auf häufig verwendete Status und Features.
- **Herunterfahren**: Schaltet das Gerät aus.
- **Neu starten**: Schaltet das Gerät aus und wieder ein.
- **Hilfe**: Öffnet die Hilfeseite.

Verwenden Sie die Links im Navigationsbereich am linken Rand der Seite, um zu den verfügbaren Verwaltungs- und Überwachungstools für Ihr Gerät zu navigieren.

Hier werden Tools beschrieben, die für alle Geräte verwendet werden können. Je nach Gerät sind möglicherweise andere Optionen verfügbar. Weitere Informationen finden Sie auf der entsprechenden Seite für Ihr Gerät.

### Startseite

Die Geräteportalsitzung beginnt auf der Startseite. Die Startseite enthält in der Regel Informationen über das Gerät, z. B. Name und Betriebssystemversion, sowie Einstellungen, die Sie für das Gerät festlegen können.

### Apps

Bietet Installations-/Deinstallations- und Verwaltungsfunktionen für AppX-Pakete und-Bündel auf Ihrem Gerät.

![Geräteportal für mobile Geräte](images/device-portal/mob-device-portal-apps.png)

- **Installierte Apps**: Hier können Sie Apps entfernen und starten.
- **Ausgeführte Apps**: Listet Apps auf, die derzeit ausgeführt werden.
- **App installieren**: Wählen Sie in einem Ordner auf dem Computer oder im Netzwerk App-Pakete für die Installation aus.
- **Abhängigkeit**: Hier fügen Sie Abhängigkeiten für die zu installierende App hinzu.
- **Bereitstellen**: Zum Bereitstellen der ausgewählten App und Abhängigkeiten auf Ihrem Gerät.

**So installieren Sie eine App**

1.  Wenn Sie [ein App-Paket erstellt haben](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx), können Sie es per Remotezugriff auf Ihrem Gerät installieren. Nachdem Sie es in Visual Studio erstellt haben, wird ein Ausgabeordner generiert.

    ![App-Installation](images/device-portal/iot-installapp0.png)
2.  Klicken Sie auf „Durchsuchen“, und suchen Sie das App-Paket („.appx“).
3.  Klicken Sie auf „Durchsuchen“, und suchen Sie die Zertifikatdatei („.cer“). (Nicht auf allen Geräten erforderlich.)
4.  Fügen Sie Abhängigkeiten hinzu. Wenn mehrere vorhanden sind, fügen Sie jede einzeln hinzu.     
5.  Klicken Sie unter **Bereitstellen** auf **Los**. 
6.  Wenn Sie eine weitere App installieren möchten, klicken Sie auf die Schaltfläche **Zurücksetzen**, um die Felder löschen.


**So deinstallieren Sie eine App**

1.  Stellen Sie sicher, dass die App nicht ausgeführt wird. 
2.  Wenn sie ausgeführt wird, wechseln Sie zu „Running apps“, und schließen Sie sie. Wenn Sie versuchen, eine App zu deinstallieren, die gerade ausgeführt wird, verursacht dies Probleme beim erneuten Installieren der App. 
3.  Sobald Sie bereit sind, klicken Sie auf **Deinstallieren**.

### Prozesse

Zeigt Details zu derzeit ausgeführten Prozessen an. Diese umfassen Apps und Systemprozesse.

Auf dieser Seite werden wie im Task-Manager auf dem PC die derzeit ausgeführten Prozesse sowie ihre Speicherauslastung angezeigt.  Auf manchen Plattformen (Desktop, IoT und HoloLens) können Sie Prozesse beenden.

![Geräteportal für mobile Geräte](images/device-portal/mob-device-portal-processes.png)

### Leistung

Zeigt Echtzeitgraphen mit Informationen zur Systemdiagnose an, z. B. Stromverbrauch, Bildfrequenz und CPU-Last.

Die folgenden Metriken sind verfügbar:
- **CPU**: Verfügbarkeit in Prozent
- **Arbeitsspeicher**: Gesamter Arbeitsspeicher, genutzter Arbeitsspeicher, verfügbarer zugesicherter Arbeitsspeicher, ausgelagerter Arbeitsspeicher und nicht ausgelagerter Arbeitsspeicher
- **GPU**: Auslastung des GPU-Moduls, gesamte Verfügbarkeit in Prozent
- **E/A**: Lese- und Schreibvorgänge
- **Netzwerk**: Empfangen und gesendet

![Geräteportal für mobile Geräte](images/device-portal/mob-device-portal-perf.png)

### Ereignisablaufverfolgung für Windows (ETW)

Verwaltet die Echtzeit-Ereignisablaufverfolgung für Windows (ETW) auf dem Gerät.

![Geräteportal für mobile Geräte](images/device-portal/mob-device-portal-etw.png)

Aktivieren Sie **Anbieter ausblenden**, um nur die Liste der Ereignisse anzuzeigen.
- **Registrierte Anbieter**: Wählen Sie den ETW-Anbieter und die Ablaufverfolgungsebene aus. Für die Ablaufverfolgungsebene wird einer der folgenden Werte festgelegt:
    1. Abnormal exit or termination
    2. Severe errors
    3. Warnungen
    4. Non-error warnings
    5. Detailed trace (*)

Klicken oder tippen Sie auf **Aktivieren**, um die Ablaufverfolgung zu starten. Der Anbieter wird der Liste **Aktivierte Anbieter** hinzugefügt.
- **Benutzerdefinierte Anbieter**: Wählen Sie einen benutzerdefinierten ETW-Anbieter und die Ablaufverfolgungsebene aus. Identifizieren Sie den Anbieter anhand seiner GUID. Fügen Sie keine Klammern in die GUID ein.
- **Aktivierte Anbieter**: Listet die aktivierten Anbieter auf. Wählen Sie einen Anbieter aus der Dropdownliste aus, und klicken oder tippen Sie auf **Deaktivieren**, um die Ablaufverfolgung zu beenden. Klicken oder tippen Sie auf **Beenden**, um sämtliche Ablaufverfolgung anzuhalten.
- **Anbieterverlauf**: Zeigt die ETW-Anbieter an, die während der aktuellen Sitzung aktiviert wurden. Klicken oder tippen Sie auf **Aktivieren**, um einen Anbieter zu aktivieren, der deaktiviert war. Klicken oder tippen Sie auf **Löschen**, um den Verlauf zu löschen.
- **Ereignisse**: Listet-ETW-Ereignisse der ausgewählten Anbieter im Tabellenformat auf. Diese Tabelle wird in Echtzeit aktualisiert. Klicken Sie unter der Tabelle auf die Schaltfläche **Löschen**, um alle ETW-Ereignisse aus der Tabelle zu löschen. Hierdurch werden keine Anbieter deaktiviert. Sie können auf **In Datei speichern** klicken, um die derzeit erfassten ETW-Ereignisse in eine lokale CSV-Datei zu exportieren.

Weitere Informationen zur Verwendung von ETW-Ablaufverfolgung finden Sie im [Blogbeitrag](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) zu deren Verwendung zum Sammeln von Echtzeitprotokollen in Ihrer App. 

### Leistungsüberwachung

Zeichnen Sie [Windows Performance Recorder](https://msdn.microsoft.com/library/windows/hardware/hh448205.aspx) (WPR)-Leistungsüberwachungen von Ihrem Gerät auf.

![Geräteportal für mobile Geräte](images/device-portal/mob-device-portal-perf-tracing.png)

- **Verfügbare Profile**: Wählen Sie in der Dropdownliste das WPR-Profil aus, und klicken oder tippen Sie auf **Starten**, um die Ablaufverfolgung zu starten.
- **Benutzerdefinierte Profile**: Klicken oder tippen Sie auf **Durchsuchen**, um ein WPR-Profil vom PC auszuwählen. Klicken oder tippen Sie auf **Hochladen und starten**, um die Ablaufverfolgung zu starten.

Klicken Sie auf **Beenden**, um die Ablaufverfolgung zu beenden. Lassen Sie diese Seite geöffnet, bis der Download der Ablaufverfolgungsdatei („.etl“) abgeschlossen ist.

Aufgezeichnete ETL-Dateien können in [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx) für die Analyse geöffnet werden.

### Geräte

Listet alle Peripheriegeräte auf, die an das Gerät angeschlossen sind.

![Geräteportal für mobile Geräte](images/device-portal/mob-device-portal-devices.png)

### Netzwerke

Verwaltet die Netzwerkverbindungen auf dem Gerät.  Durch das Ändern dieser Einstellungen wird das Gerät wahrscheinlich vom Geräteportal getrennt, sofern es nicht per USB mit dem Geräteportal verbunden ist.
- **Profile**: Wählen Sie ein anderes zu verwendendes WLAN-Profil aus.  
- **Verfügbare Netzwerke**: Die auf dem Gerät verfügbaren WLAN-Netzwerke. Durch Klicken oder Tippen auf ein Netzwerk können Sie eine Verbindung mit ihm herstellen und ggf. ein Kennwort eingeben. Hinweis: Das Geräteportal unterstützt noch keine Unternehmensauthentifizierung. 

![Geräteportal für mobile Geräte](images/device-portal/mob-device-portal-network.png)

## Service-Features und Hinweise

### DNS-SD

Das Geräteportal kündigt seine Präsenz im lokalen Netzwerk mithilfe von DNS-SD an.  Alle Geräteportalinstanzen, unabhängig von deren Gerätetyp, kündigen sich unter „WDP._wdp._tcp.local“ an. Die TXT-Datensätze für die Instanz des Dienstes liefern Folgendes:

Key | Typ | Beschreibung 
----|------|-------------
S | int | Sicherer Port für Geräteportal.  Wenn 0 (null), lauscht das Geräteportal nicht auf HTTPS-Verbindungen. 
D | Zeichenfolge | Typ des Geräts.  Dieser wird das Format „Windows.*“ aufweisen, z. B. Windows.Xbox oder Windows.Desktop
A | Zeichenfolge | Gerätearchitektur.  Diese ist ARM, x86 oder AMD64.  
T | Mit NULL-Zeichen getrennt Liste mit Zeichenfolgen | Vom Benutzer angewendete Tags für das Gerät. Informationen zur Verwendung finden Sie unter der Tags-REST-API. Liste wird durch Doppelnull beendet.  

Es wird vorgeschlagen, die Verbindung über den HTTPS-Anschluss herzustellen, da nicht alle Geräte auf dem vom DNS-SD-Datensatz angekündigten HTTP-Port lauschen. 

### CSRF-Schutz und -Skripting

Zum Schutz vor [CSRF-Angriffen](https://wikipedia.org/wiki/Cross-site_request_forgery) ist bei allen Nicht-GET-Anfragen ein eindeutiges Token erforderlich. Dieses Token, der X-CSFR-Token-Anforderungsheader, wird von einem Sitzungscookie CSRF-Token, abgeleitet. In der Web-Benutzeroberfläche des Geräteportals wird das CSRF-Token-Cookie bei jeder Anforderung in den X-CSRF-Token-Header kopiert.

**Wichtig** Dieser Schutz verhindert die Verwendung der REST-APIs auf einem eigenständigen Client (z. B. Befehlszeilenprogramme). Dies kann auf drei Arten gelöst werden: 

1. Verwendung des „Auto-“-Benutzernamens. Clients, die ihrem Benutzernamen „Auto-“ voranstellen, umgehen CSRF-Schutz. Es ist wichtig, dass dieser Benutzername nicht zur Anmeldung beim Geräteportal über den Browser verwendet wird, da dies den Dienst für CSRF-Angriffe öffnet. Beispiel: Wenn der Benutzername des Geräteportals „Admin“ lautet, sollte ```curl -u auto-admin:password <args>``` zum Umgehen des CSRF Schutzes verwendet werden. 

2. Implementieren des Cookie-zu-Header-Schemas in den Client. Dies erfordert eine GET-Anforderung zur Erstellung des Sitzungscookies und dann die Aufnahme von Header und Cookie in alle nachfolgenden Anforderungen. 
 
3. Deaktivieren der Authentifizierung und Verwenden von HTTP. CSRF-Schutz bezieht sich nur auf HTTPS-Endpunkte, sodass für Verbindungen auf HTTP-Endpunkten keine der oben genannten Schritte ausgeführt werden müssen. 

**Hinweis**: Ein Benutzername, der mit „Auto-“ beginnt, kann sich nicht am Geräteportal über den Browser anmelden.  



<!--HONumber=Jun16_HO4-->


