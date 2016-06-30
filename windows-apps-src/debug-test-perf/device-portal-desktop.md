---
author: mcleblanc
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: "Device Portal für Desktop"
description: "Hier erfahren Sie, wie das Windows Device Portal Diagnose und Automatisierung auf dem Windows-Desktop öffnet."
ms.sourcegitcommit: f09f0233ec11b41989cf52da3c5e8cb37a97b607
ms.openlocfilehash: 7be27f5fb15676c5330f22995dd044899eddfd3d

---
# Device Portal für Desktop

Ab Windows 10, Version 1607, sind weitere Entwicklerfeatures für Desktop verfügbar. Diese Features sind nur verfügbar, wenn der Entwicklermodus aktiviert ist.

Informationen zum Aktivieren des Entwicklermodus finden Sie unter [Aktivieren Ihres Geräts für die Entwicklung](../get-started/enable-your-device-for-development.md).

Im Device Portal können Sie Diagnoseinformationen anzeigen und über HTTP aus dem Browser mit Ihrem Desktop interagieren. Sie haben im Device Portal folgende Möglichkeiten:
- Anzeigen und Bearbeiten einer Liste laufender Prozesse
- Installieren, Löschen, Starten und Beenden von Apps
- Ändern von WLAN-Profilen und Anzeigen der Signalstärke und der ipconfig
- Anzeigen von Live-Diagrammen zur Auslastung von CPU, Arbeitsspeicher, E/A, Netzwerk und GPU
- Sammeln von Prozesssicherungen
- Sammeln von ETW-Ablaufverfolgungen 
- Bearbeiten des isolierten Speichers quergeladener Apps

## Einrichten von Device Portal unter Windows-Desktop

### Aktivieren von Device Portal

Sie können Device Portal im Menü **Entwicklereinstellungen** mit aktiviertem Entwicklermodus aktivieren.  

Wenn Sie Device Portal aktivieren, müssen Sie einen Benutzernamen und ein Kennwort für Device Portal erstellen. Verwenden Sie nicht Ihr Microsoft-Konto oder andere Windows-Anmeldeinformationen.  

Nachdem Device Portal aktiviert wurde, werden unten im Abschnitt **Einstellungen** Links angezeigt. Beachten Sie die Portnummer am Ende der URL: Diese Portnummer wird nach dem Zufallsprinzip generiert, wenn Device Portal aktiviert wird, sollte jedoch zwischen Neustarts des Desktops konsistent bleiben. Wenn Sie die Portnummern manuell festlegen möchten, sodass sie erhalten bleiben, finden Sie unter [Portnummern setzen](device-portal-desktop.md#setting-port-numbers) weitere Informationen.

Sie haben zwei Möglichkeiten zum Herstellen einer Verbindung mit Device Portal: über einen lokalen Host und über das lokale Netzwerk (einschließlich VPN).

**So stellen Sie eine Verbindung mit dem Device Portal her**

1. Geben Sie in Ihrem Browser die hier angegebene Adresse für den verwendeten Verbindungstyp ein.

    - Lokaler Host: `http://127.0.0.1:PORT` oder `http://localhost:PORT`

    Verwenden Sie diese Adresse, um Device Portal lokal anzuzeigen.
    
    - Lokales Netzwerk: `https://<The IP address of the desktop>:PORT`

    Verwenden Sie diese Adresse, um die Verbindung über ein lokales Netzwerk herzustellen.

Für die Authentifizierung und sichere Kommunikation ist HTTPS erforderlich.

Sie können die Authentifizierung deaktivieren, wenn Sie Device Portal in einer geschützten Umgebung verwenden, z. B. in einem Testlabor, in dem Sie allen im lokalen Netzwerk vertrauen, keine persönlichen Informationen auf dem Gerät gespeichert haben und in dem spezielle Anforderungen bestehen. Dies ermöglicht eine unverschlüsselte Kommunikation, und jeder, der die IP-Adresse Ihres Computers kennt, kann es steuern.

## Seiten von Device Portal

Device Portal für Desktop enthält den Standardsatz der Seiten. Ausführliche Beschreibungen finden Sie unter [Übersicht über das Windows Device Portal](device-portal.md).

- Apps
- Prozesse
- Leistung
- Debuggen
- Ereignisablaufverfolgung für Windows (ETW)
- Leistungsüberwachung
- Geräte
- Netzwerk
- App-Datei-Explorer 

## Festlegen von Portnummern

Wenn Sie Portnummern für Device Portal auswählen möchten (z. B. 80 und 443), können Sie die folgenden Registrierungsschlüssel festlegen:

- Unter HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service
    - UseDynamicPorts: Ein erforderlicher DWORD-Wert. Legen Sie den Wert auf 0 fest, um die von Ihnen ausgewählten Portnummern beizubehalten.
    - HttpPort: Ein erforderlicher DWORD-Wert. Enthält die Portnummer, an der Device Portal nach HTTP-Verbindungen lauscht.  
    - HttpsPort: Ein erforderlicher DWORD-Wert. Enthält die Portnummer, an der Device Portal nach HTTPS-Verbindungen lauscht.

## Fehler beim Installieren des Entwicklermoduspakets
In manchen Fällen wird der Entwicklermodus aufgrund von Problemen mit dem Netzwerk oder der Kompatibilität nicht ordnungsgemäß installiert. Das Entwicklermoduspaket ist für Remote-Bereitstellung – Device Portal und SSH – aber nicht für lokale Entwicklung erforderlich.  

### Das Paket konnte nicht gefunden werden

"Developer Mode package couldn’t be located in Windows Update. Error Code 0x001234 Learn more"   

Dieser Fehler kann aufgrund eines Netzwerkverbindungsproblems, aufgrund von Enterprise-Einstellungen oder weil das Paket nicht vorhanden ist, auftreten. 

So beheben Sie dieses Problem:

1. Stellen Sie sicher, dass Ihr Computer mit dem Internet verbunden ist. 
2. Wenn Sie einen in eine Domäne eingebundenen Computer verwenden, sprechen Sie mit dem Netzwerkadministrator. 
3. Suchen nach Windows-Updates in den Einstellungen > Updates und Sicherheit > [Windows-Updates](ms-settings:windowsupdate).
4. Stellen Sie sicher, dass das Windows-Entwicklermoduspaket in den Einstellungen > System > Apps und Features > [Optionale Features verwalten](ms-settings:optionalfeatures) > Feature hinzufügen vorhanden ist. Wenn es nicht vorhanden ist, kann Windows das richtige Paket für Ihren Computer nicht finden. 

Nachdem Sie einen der oben genannten Schritte durchgeführt haben, deaktivieren Sie den Entwicklermodus, und aktivieren Sie ihn dann erneut, um die Korrektur zu überprüfen. 


### Fehler beim Installieren des Pakets

"Developer Mode package failed to install. Error code 0x001234  Learn more"

Dieser Fehler kann aufgrund von Inkompatibilitäten zwischen dem Build von Windows und dem Entwicklermoduspaket auftreten. 

So beheben Sie dieses Problem:

1. Suchen nach Windows-Updates in den Einstellungen > Updates und Sicherheit > [Windows-Updates](ms-settings:windowsupdate).
2. Starten Sie Ihren Computer neu, um sicherzustellen, dass alle Updates angewendet wurden.



<!--HONumber=Jun16_HO4-->


