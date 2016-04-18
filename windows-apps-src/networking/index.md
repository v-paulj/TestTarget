---
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: Technologien für den Zugriff auf das Netzwerk und Webdienste.
title: Netzwerk und Webdienste
---

# Netzwerk und Webdienste

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Die folgenden Netzwerk- und Webdiensttechnologien sind für Entwickler von UWP (Universelle Windows-Plattform)-Apps verfügbar.

| Thema                                                                                   | Beschreibung                                                                      |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| [Grundlagen zum Netzwerk](networking-basics.md)                                               | Aktionen, die Sie für eine netzwerkfähige App ausführen müssen.                     |
| [Welche Netzwerktechnologie?](which-networking-technology.md)                          | Eine kurze Übersicht über die Netzwerktechnologien, die für UWP-Entwickler zur Verfügung stehen, mit Vorschlägen zum Auswählen der Technologien, die für Ihre App geeignet sind.               |
| [Netzwerkkommunikation im Hintergrund](network-communications-in-the-background.md) | Apps verwenden Hintergrundaufgaben und zwei Hauptmechanismen, um die Kommunikation aufrechtzuerhalten, während sie nicht im Vordergrund ausgeführt werden: den Socketbroker und Steuerkanaltrigger.                  |
| [Sockets](sockets.md)                                                                   | Sie können als UWP-App-Entwickler [Windows.Networking.Sockets](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.networking.sockets.aspx) und [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) zur Kommunikation mit anderen Geräten verwenden. In diesem Thema finden Sie umfassende Anleitungen zur Verwendung des Windows.Networking.Sockets-Namespace, um Netzwerkvorgänge durchzuführen. |
| [WebSockets](websockets.md)                                                             | WebSockets stellt einen Mechanismus für die schnelle und sichere bidirektionale Kommunikation zwischen einem Client und einem Server über das Web mithilfe von HTTP(S) bereit.                 |
| [HttpClient](httpclient.md)                                                             | Verwenden Sie die [Windows.Web.Http](https://msdn.microsoft.com/library/windows/apps/dn279692)-Namespace-API zum Senden und Empfangen von Informationen über das HTTP-Protokoll 1.1 und 2.0.             |
| [RSS/Atom-Feeds](web-feeds.md)                                                          | Mithilfe von Fremdanbieterfeeds, die entsprechend den RSS- und Atom-Standards mit Features im [Windows.Web.Syndication](https://msdn.microsoft.com/library/windows/apps/br243632)-Namespace generiert werden, können Sie die aktuellsten und beliebtesten Webinhalte abrufen oder erstellen.                   |
| [Hintergrundübertragungen](background-transfers.md)                                         | Verwenden Sie die Hintergrundübertragungs-API zum zuverlässigen Kopieren von Dateien im Netzwerk.           |
| [Markieren von Netzwerkverbindungen mit EDP-Identität](tagging_network_connections_with_edp_identity.md) | In diesem Thema wird veranschaulicht, wie Sie einen geschützten Threadkontext erstellen, bevor Sie in einem Unternehmensdatenschutz-Szenario (Enterprise Data Protection, EDP) Netzwerkverbindungen erstellen. |

<!--HONumber=Mar16_HO5-->


