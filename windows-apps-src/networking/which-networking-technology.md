---
ms.assetid: 2CC2E526-DACB-4008-9539-DA3D0C190290
description: Eine kurze Übersicht über die Netzwerktechnologien, die für UWP-Entwickler zur Verfügung stehen, mit Vorschlägen zum Auswählen der Technologien, die für Ihre App geeignet sind.
title: Welche Netzwerktechnologie?

---

# Welche Netzwerktechnologie?

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Eine kurze Übersicht über die Netzwerktechnologien, die für UWP-Entwickler zur Verfügung stehen, mit Vorschlägen zum Auswählen der Technologien, die für Ihre App geeignet sind.

## Sockets

Verwenden Sie [Sockets](sockets.md), wenn Sie mit einem anderen Gerät kommunizieren und Ihr eigenes Protokoll verwenden möchten.

Zwei Implementierungen von Sockets sind für UWP-Entwickler (Universelle Windows-Plattform) verfügbar: [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) und [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673). Wenn Sie neuen Code schreiben, hat Windows.Networking.Sockets den Vorteil, dass es sich um eine moderne API handelt, die für die Verwendung durch UWP-Entwickler vorgesehen ist. Wenn Sie plattformübergreifende Netzwerkbibliotheken oder anderen vorhandenen Winsock-Code verwenden oder die Winsock-API bevorzugen, verwenden Sie Winsock.

### Wann sollten Sockets verwenden werden?

-   Beide Socketimplementierungen ermöglichen Ihnen die Kommunikation mit anderen Geräten mithilfe von Protokollen Ihrer Wahl über TCP oder UDP.

-   Wählen Sie die Sockets-API aus, die am besten Ihre Anforderungen erfüllt; orientieren Sie sich dabei an Ihren Erfahrungen und vorhandenem Code, den Sie möglicherweise verwenden.

### Wann sollten Sockets nicht verwendet werden?

-   Implementieren Sie keine eigenen HTTP(S)-Stapel mit Sockets. Verwenden Sie stattdessen [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639).
-   Wenn WebSockets (die [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Klasse und [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)-Klasse) Ihren Kommunikationsanforderungen entsprechen (TCP zu/von einem Webserver), sollten Sie diese verwenden, anstatt Ihre Zeit und Entwicklungsressourcen für die Implementierung ähnlicher Funktionen mit Sockets aufzuwenden.

## WebSockets

Das [WebSockets](websockets.md)-Protokoll definiert einen Mechanismus für die schnelle und sichere bidirektionale Kommunikation zwischen einem Client und einem Server über das Web. Daten werden sofort über eine Vollduplex-Ein-Socket-Verbindung übertragen, sodass Nachrichten von beiden Endpunkten in Echtzeit gesendet und empfangen werden können. WebSockets eignen sich ideal für den Einsatz in Echtzeitspielen, bei denen Sofortbenachrichtigungen aus sozialen Netzwerken und die aktuelle Anzeige von Daten (z. B. Spielstatistiken) sicher sein und eine schnelle Datenübertragung verwenden müssen. UWP-Entwickler können die [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Klasse und [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)-Klasse zum Herstellen einer Verbindung mit Servern verwenden, die das WebSocket-Protokoll unterstützen.

### Wann sollten WebSockets verwendet werden?

-   Wenn Sie Daten fortlaufend zwischen einem Gerät und einem Server senden und empfangen möchten.

### Wann sollten WebSockets nicht verwendet werden?

-   Wenn Sie Daten selten senden oder empfangen, ist es für Sie unter Umständen einfacher, einzelne HTTP-Anforderungen vom Gerät an den Server zu senden, anstatt eine WebSocket-Verbindung herzustellen und aufrechtzuerhalten.
-   WebSockets sind möglicherweise nicht für Situationen mit großem Datenvolumen geeignet. Sie sollten Ihre Datenflüsse modellieren und Ihren Datenverkehr über WebSockets simulieren, bevor Sie sie in Ihrem Entwurf verwenden.

## HttpClient

Verwenden Sie [HttpClient](httpclient.md) (und den Rest der [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)-Namespace-API), wenn Sie HTTP(S) für die Kommunikation mit einem Webdienst oder Webserver verwenden.

### Wann sollte HttpClient verwendet werden?

-   Wenn Sie HTTP(S) zur Kommunikation mit Webdiensten verwenden.
-   Beim Hochladen oder Herunterladen einer geringen Anzahl von kleineren Dateien.
-   Wenn WebSockets (die [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Klasse und [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842))-Klasse Ihren Kommunikationsanforderungen (TCP zu/von einem Webserver) entsprechen und der fragliche Webserver WebSockets unterstützt, sollten Sie diese verwenden, anstatt Ihre Zeit und Entwicklungsressourcen für die Implementierung ähnlicher Funktionen mit HttpClient aufzuwenden.
-   Wenn Sie Inhalte über das Netzwerk streamen.

### Wann sollte HttpClient nicht verwendet werden?

-   Wenn Sie große Dateien oder eine große Anzahl von Dateien übertragen, sollten Sie stattdessen Hintergrundübertragungen verwenden.
-   Wenn Sie Upload-/Downloadlimits nach Verbindungstyp einschränken möchten oder den Upload-/Downloadstatus speichern und den Upload/Download nach einer Unterbrechung wiederaufnehmen möchten, müssen Sie Hintergrundübertragungen verwenden.
-   Wenn Sie zwischen zwei Geräten kommunizieren und keines als HTTP(S)-Server vorgesehen ist, sollten Sie Sockets verwenden. Versuchen Sie nicht, einen eigenen HTTP-Server zu implementieren und [HttpClient](httpclient.md) für die Kommunikation zu verwenden.

## Hintergrundübertragungen

Verwenden Sie die [Hintergrundübertragungs-API](background-transfers.md), wenn Sie zuverlässig Dateien über das Netzwerk übertragen möchten. Die Hintergrundübertragungs-API bietet erweiterte Upload- und Downloadfeatures, die bei angehaltener App im Hintergrund ausgeführt werden und auch nach Beendigung der App aktiv bleiben. Die API überwacht den Netzwerkstatus und kann Übertragungen automatisch anhalten und fortsetzen, wenn die Verbindung unterbrochen wird. Übertragungen sind außerdem daten- und akkuabhängig – die Downloadaktivität wird also basierend auf dem aktuellen Verbindungs- und Geräteakkustatus angepasst. Diese Funktionen sind wichtig, wenn Ihre App auf mobilen oder akkubetriebenen Geräten ausgeführt wird. Die API ist ideal für das Hoch- und Herunterladen von großen Dateien über HTTP(S) geeignet. FTP wird auch unterstützt, allerdings nur für Downloads.

Ein neues Feature für die Hintergrundübertragung in Windows 10 ist die Möglichkeit, eine Nachverarbeitung auszulösen, wenn eine Dateiübertragung abgeschlossen wurde, sodass Sie lokale Kataloge aktualisieren, andere Apps aktivieren oder den Benutzer benachrichtigen können, wenn ein Download abgeschlossen ist.

### Wann sollten Hintergrundübertragungen verwendet werden?

-   Verwenden Sie Hintergrundübertragungen zum zuverlässigen Übertragen großer Dateien oder einer großen Anzahl von Dateien.
-   Verwenden Sie Hintergrundübertragungen mit Abschlussgruppen für Hintergrundübertragungen, wenn Sie Dateiübertragungen im Nachhinein mit einer Hintergrundaufgabe verarbeiten möchten.
-   Verwenden Sie Hintergrundübertragungen, wenn Sie eine laufende Übertragung nach einer Netzwerkunterbrechung wiederaufnehmen möchten.
-   Verwenden Sie Hintergrundübertragungen, wenn Sie das Übertragungsverhalten auf Grundlage von Netzwerkbedingungen ändern möchten, z. B. beim Wechsel zu einem Volumentarif.

### Wann sollten Hintergrundübertragungen nicht verwendet werden?

-   Wenn Sie eine geringe Anzahl von kleinen Dateien übertragen und Sie keine Nachbearbeitung nach Abschluss der Übertragung durchführen müssen, sollten Sie die [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Methode PUT oder POST verwenden.
-   Wenn Sie Daten streamen und nach der Übertragung lokal verwenden möchten, verwenden Sie [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639).

## Zusätzliche netzwerkbezogene Technologien

### Verbindungsqualität

Mit der [**Windows.Networking.Connectivity**](https://msdn.microsoft.com/library/windows/apps/br207308)-API können Sie auf Informationen zu Netzwerkkonnektivität, Kosten und Nutzung zugreifen. Weitere Informationen zur Verwendung dieser API finden Sie unter [Zugreifen auf den Netzwerkverbindungsstatus und Verwalten von Netzwerkkosten](https://msdn.microsoft.com/library/windows/apps/hh452983).

### DNS Service Discovery (DNS-SD)

Mit der [**Windows.Networking.ServiceDiscovery.Dnssd**](https://msdn.microsoft.com/library/windows/apps/dn895183)-API können Sie einen Netzwerkdienst für andere Geräte im Netzwerk mithilfe des DNS-SD-Protokolls ankündigen, das in IETF [RFC 2782](http://go.microsoft.com/fwlink/?LinkId=524158) beschrieben wurde.

### Kommunikation über Bluetooth

Unter anderem ermöglicht Ihnen die [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/dn263413)-API die Verwendung von Bluetooth, um eine Verbindung mit anderen Geräten herzustellen und Daten zu übertragen. Weitere Informationen finden Sie unter [Senden und Empfangen von Dateien mit RFCOMM](https://msdn.microsoft.com/library/windows/apps/mt270289).

### Pushbenachrichtigungen (WNS)

Mit der [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307)-API können Sie den Windows-Benachrichtigungsdienst (Windows Notification Service, WNS) verwenden, um Pushbenachrichtigungen über das Netzwerk zu empfangen. Weitere Informationen zum Verwenden dieser API finden Sie unter [Übersicht über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203).

### Nahfeldkommunikation (Near Field Communication, NFC)

Mit der [**Windows.Networking.Proximity**](https://msdn.microsoft.com/library/windows/apps/br241250)-API können Sie Nahfeldkommunikation für Apps verwenden, die die Näherung oder das Koppeln mit Geräten nutzen, um eine einfache Datenübertragung zu ermöglichen. Weitere Informationen zum Verwenden dieser API finden Sie unter [Unterstützen von Näherung und Koppeln](https://msdn.microsoft.com/library/windows/apps/hh465229).

### RSS/Atom-Feeds

Mit der [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)-API können Sie Syndication-Feeds mit RSS- und Atom-Formaten verwalten. Weitere Informationen zur Verwendung dieser API finden Sie unter [RSS/Atom-Feeds](web-feeds.md).

### WLAN-Auflistung und Verbindungssteuerung

Mit der [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/dn975224)-API können Sie WLAN-Adapter auflisten, nach verfügbaren WLAN-Netzwerken suchen und einen Adapter mit einem Netzwerk verbinden.

### Funksteuerung

Mit der [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/dn996447)-API können Sie Funkempfänger auf dem lokalen Gerät suchen und steuern, einschließlich WLAN und Bluetooth.

### Wi-Fi Direct

Mit der [**Windows.Devices.WiFiDirect**](https://msdn.microsoft.com/library/windows/apps/dn297687)-API können Sie eine Verbindung mit anderen lokalen Geräten über Wi-Fi Direct herstellen und mit ihnen kommunizieren, um lokale Ad-hoc-Drahtlosnetzwerke zu erstellen.

### Wi-Fi Direct-Dienste

Mit der [**Windows.Devices.WiFiDirect.Services**](https://msdn.microsoft.com/library/windows/apps/dn996481)-API können Sie Wi-Fi Direct-Dienste bereitstellen und entsprechende Verbindungen herstellen. Wi-Fi Direct-Dienste ermöglichen einem Gerät in einem Wi-Fi Direct-Ad-hoc-Netzwerk (Dienstankündigung), einem anderen Gerät (Dienstsuche) über eine Wi-Fi Direct-Verbindung Funktionen bereitzustellen.

### Mobilfunkanbieter

Windows 10 stellt einer großen Entwicklergruppe einige APIs bereit, die bisher nur Geräteherstellern und Mobilfunkanbietern zur Verfügung standen. Beachten Sie, dass obwohl diese APIs jetzt verfügbar gemacht werden, sie auch bestimmten App-Funktionen unterliegen, die von Microsoft genehmigt werden müssen, bevor eine App veröffentlicht werden kann. Die tatsächliche Verwendung dieser APIs wird nach wie vor in erster Linie auf Gerätehersteller und Mobilfunkanbieter beschränkt.

### Netzwerkvorgänge

Die [**Windows.Networking.NetworkOperators**](https://msdn.microsoft.com/library/windows/apps/br241148)-API befasst sich hauptsächlich mit der Konfiguration und Bereitstellung von Telefonen. Darum ist die Berechtigung zum Verwenden der steuernden Funktionen auf Gerätehersteller und Telekommunikationsanbieter beschränkt.

### SMS

Der [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/br206567)-Namespace verarbeitet SMS- und verwandte Nachrichten als untergeordnete Elemente. Er wird für die Verwendung durch Mobilfunkanbieter für die App-gerichtete SMS-Verwendung bereitgestellt und wird durch eine Funktion gesteuert, die nicht zur Verwendung durch die meisten App-Entwickler genehmigt wird. Wenn Sie eine App für die Verarbeitung von Nachrichten schreiben, verwenden Sie stattdessen die [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/dn642321)-API, da sie nicht nur SMS-Nachrichten verarbeiten kann, sondern auch Nachrichten aus anderen Quellen, z. B. Echtzeit-Chat-Apps, sodass eine viel umfangreichere Chat-/Nachrichtenerfahrung möglich wird.



<!--HONumber=Mar16_HO1-->


