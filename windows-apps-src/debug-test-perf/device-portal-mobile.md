---
author: mcleblanc
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: "Geräteportal für mobile Geräte"
description: "Hier erfahren Sie, wie Sie mit dem Windows Device Portal Ihr mobiles Gerät per Fernzugriff konfigurieren und verwalten können."
ms.sourcegitcommit: df6d42d6a91b8721e905fe9bc3a339dc33408459
ms.openlocfilehash: eeeb8f98d97468544cc30e3d9884cce15cb913a9

---
# Geräteportal für mobile Geräte

Ab Windows 10, Version 1511, sind weitere Entwicklerfeatures für Mobilgeräte verfügbar. Diese Features sind nur verfügbar, wenn der Entwicklermodus auf dem Gerät aktiviert ist.

Informationen zum Aktivieren des Entwicklermodus finden Sie unter [Aktivieren Ihres Geräts für die Entwicklung](../get-started/enable-your-device-for-development.md).

![Einstellungen im Device Portal](images/device-portal/mob-dev-mode-options.png)

## Einrichten des Device Portal unter Windows Phone

### Aktivieren der Geräteermittlung und -kopplung

Damit Sie eine Verbindung mit dem Device Portal herstellen können, müssen Sie die Geräteermittlung aktivieren. Auf diese Weise können Sie Ihr Smartphone mit einem PC oder einem anderen Windows10-Gerät koppeln. Beide Geräte müssen über eine kabelgebundene oder drahtlose Verbindung mit dem gleichen Subnetz des Netzwerks verbunden sein oder über USB verbunden werden.

Wenn Sie das erste Mal eine Verbindung mit dem Geräteportal herstellen, werden Sie aufgefordert, einen sechsstelligen Sicherheitscode einzugeben (mit Beachtung der Groß- und Kleinschreibung). Dadurch wird sichergestellt, dass Sie Zugriff auf das Smartphone haben, und Sie werden vor Angriffen geschützt. Tippen Sie auf Ihrem Smartphone auf die Schaltfläche „Koppeln“, um den Code zu generieren und anzuzeigen. Geben Sie dann die sechs Zeichen in das Textfeld im Browser ein.

![Einstellungen für die Geräteerkennung im Entwicklermodus](images/device-portal/mob-dev-mode-pairing.png)

Sie haben 3 Möglichkeiten zum Herstellen einer Verbindung mit dem Geräteportal: USB, über einen lokalen Host und über das lokale Netzwerk (einschließlich VPN und Tethering).

**So erstellen Sie eine Verbindung mit dem Geräteportal**

1. Geben Sie in Ihrem Browser die hier angegebene Adresse für den verwendeten Verbindungstyp ein.

    - USB:  `http://127.0.0.1:10080`

    Verwenden Sie diese Adresse, wenn das Smartphone über USB mit einem PC verbunden ist. Auf beiden Geräten muss Windows 10, Version 1511 oder höher, ausgeführt werden.
    
    - Lokaler Host:  `http://127.0.0.1`

    Verwenden Sie diese Adresse, um das Geräteportal lokal auf dem Smartphone in Microsoft Edge für Windows 10 Mobile anzuzeigen.
    
    - Lokales Netzwerk:  `https://<The IP address of the phone>`

    Verwenden Sie diese Adresse, um die Verbindung über ein lokales Netzwerk herzustellen.

    Die IP-Adresse des Smartphones wird in den Geräteportaleinstellungen auf dem Telefon angezeigt. Für die Authentifizierung und sichere Kommunikation ist HTTPS erforderlich. Der Hostname (kann in „Einstellungen“ > „System“ > „Info“ bearbeitet werden) kann auch für den Zugriff auf das Geräteportal im lokalen Netzwerk verwendet werden (Beispiel: „http://Phone360“). Dies ist hilfreich, wenn Netzwerke oder IP-Adressen von Geräten häufig geändert werden oder freigegeben werden müssen. 

2. Tippen Sie auf Ihrem Smartphone auf die Schaltfläche „Koppeln“, um den Code zu generieren und anzuzeigen.

3. Geben Sie den sechsstelligen Sicherheitscode im Browser in das Kennwortfeld des Geräteportals ein.

4. (Optional) Aktivieren Sie das Kontrollkästchen Computer merken in Ihrem Browser, um diese Kopplung für die spätere Verwendung zu speichern.

Dies ist der Geräteportalabschnitt auf der Seite mit den Entwicklerseiten in Windows Phone.

![Einstellungen im Geräteportal](images/device-portal/mob-dev-mode-portal.png)

Sie können die Authentifizierung deaktivieren, wenn Sie das Geräteportal in einer geschützten Umgebung verwenden, z.B. in einem Testlabor, in dem Sie allen im lokalen Netzwerk vertrauen, keine persönlichen Informationen auf dem Gerät gespeichert haben und in dem spezielle Anforderungen bestehen. Dies ermöglicht eine unverschlüsselte Kommunikation, und jeder, der die IP-Adresse Ihres Smartphones kennt, kann es steuern.

## Anmerkungen zu Tools

## Seiten des Geräteportals
### Prozesse

Im Windows Mobile Device Portal ist es nicht möglich, beliebige Prozesse zu beenden. 

Das Geräteportal für mobile Geräte enthält den Standardsatz der Seiten. Ausführliche Beschreibungen finden Sie unter [Übersicht über das Windows Device Portal](device-portal.md).

- Apps
- Prozesse
- Leistung
- Ereignisablaufverfolgung für Windows (ETW)
- Leistungsüberwachung
- Geräte
- Netzwerke



<!--HONumber=Jun16_HO4-->


