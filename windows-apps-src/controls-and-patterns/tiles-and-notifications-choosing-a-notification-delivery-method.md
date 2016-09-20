---
author: mijacobs
Description: In diesem Artikel werden die vier Benachrichtigungsoptionen&\#8212;lokal, geplant, periodisch und Push&\#8212;behandelt, die Kachel- und Signalaktualisierungen sowie Popupbenachrichtigungsinhalte bereitstellen.
title: "Auswählen einer Methode für die Übermittlung von Benachrichtigungen"
ms.assetid: FDB43EDE-C5F2-493F-952C-55401EC5172B
label: Choose a notification delivery method
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: b79a6f771afe63c9a42567875db5ef8107611afc

---

# Auswählen einer Methode für die Übermittlung von Benachrichtigungen





In diesem Artikel werden die vier Benachrichtigungsoptionen – lokal, geplant, periodisch und Push – behandelt, die Kachel- und Signalaktualisierungen sowie Popupbenachrichtigungsinhalte bereitstellen. Eine Kachel oder eine Popupbenachrichtigung kann dem Benutzer Informationen anzeigen, auch wenn der Benutzer nicht direkt mit Ihrer App beschäftigt ist. Basierend auf der Art und dem Inhalt Ihrer App sowie den Informationen, die Sie bereitstellen möchten, können Sie die am besten geeignete Benachrichtigungsmethode bzw. -methoden für Ihr Szenario bestimmen.

## <span id="Notification_delivery_methods__overview"></span><span id="notification_delivery_methods__overview"></span><span id="NOTIFICATION_DELIVERY_METHODS__OVERVIEW"></span>Übersicht über Methoden für die Benachrichtigungsübermittlung


Es gibt vier Mechanismen, die von einer App zum Übermitteln einer Benachrichtigung verwendet werden können:

-   **Lokal**
-   **Geplant**
-   **Periodisch**
-   **Push**

In dieser Tabelle sind die Benachrichtigungsübermittlungstypen zusammengefasst.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Übermittlungsmethode</th>
<th align="left">Verwendungsart</th>
<th align="left">Beschreibung</th>
<th align="left">Beispiele</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Lokal</td>
<td align="left">Kachel, Signal, Popup</td>
<td align="left">Eine Reihe von API-Aufrufen, die Benachrichtigungen senden, während Ihre App ausgeführt wird, und die Kachel oder das Signal direkt aktualisieren oder eine Popupbenachrichtigung senden.</td>
<td align="left"><ul>
<li>Eine Musik-App aktualisiert ihre Kachel so, dass &quot;Aktuelle Wiedergabe&quot; angezeigt wird.</li>
<li>Eine Spiele-App aktualisiert ihre Kachel mit der Bestwertung des Benutzers, wenn dieser das Spiel verlässt.</li>
<li>Ein Signal, dessen Symbol anzeigt, dass neue Informationen in der App gelöscht werden, wenn die App aktiviert wird.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Geplant</td>
<td align="left">Kachel, Popup</td>
<td align="left">Eine Reihe von API-Aufrufen, die eine Benachrichtigung im Voraus so planen, dass zum von Ihnen angegebenen Zeitpunkt eine Aktualisierung vorgenommen wird.</td>
<td align="left"><ul>
<li>Eine Kalender-App verwendet für eine anstehende Besprechung eine Erinnerung in Form einer Popupbenachrichtigung.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Periodisch</td>
<td align="left">Kachel, Signal</td>
<td align="left">Benachrichtigungen, mit denen Kacheln und Signale regelmäßig in einem festen Zeitintervall durch Abrufen von neuem Inhalt aus einem Clouddienst aktualisiert werden.</td>
<td align="left"><ul>
<li>Eine Wetter-App aktualisiert ihre Kachel, in der die Vorschau angezeigt wird, in Intervallen von 30 Minuten.</li>
<li>Eine Website für &quot;Tägliche Deals&quot; aktualisiert ihren Deal des Tages jeden Morgen.</li>
<li>Eine Kachel, welche die Anzahl der Tage bis zu einem Ereignis anzeigt, aktualisiert den angezeigten Countdown jeden Tag um Mitternacht.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Push</td>
<td align="left">Kachel, Signal, Popup, Raw</td>
<td align="left">Von einem Cloudserver gesendete Benachrichtigungen, selbst wenn die App nicht ausgeführt wird.</td>
<td align="left"><ul>
<li>Eine Shopping-App sendet eine Popupbenachrichtigung, um einen Benutzer über den Rabatt eines Artikels zu informieren, den er sich gerade ansieht.</li>
<li>Eine Nachrichten-App aktualisiert ihre Kachel mit Eilmeldungen, sobald sich die entsprechenden Geschehnisse ereignen.</li>
<li>Eine Sport-App hält ihre Kachel während eines laufenden Spiels auf dem neuesten Stand.</li>
<li>Eine Kommunikations-App bietet Alarme zu eingehenden Nachrichten oder Telefonanrufen.</li>
</ul></td>
</tr>
</tbody>
</table>

 

## <span id="Local_notifications"></span><span id="local_notifications"></span><span id="LOCAL_NOTIFICATIONS"></span>Lokale Benachrichtigungen


Das Aktualisieren der App-Kachel oder des Signals oder das Auslösen einer Popupbenachrichtigung, während die App ausgeführt wird, stellt den einfachsten der Mechanismen zur Benachrichtigungsübermittlung dar. Dafür werden nur lokale API-Aufrufe benötigt. Für jede App kann es hilfreiche oder interessante Informationen zum Anzeigen auf der Kachel geben – auch dann, wenn sich dieser Inhalt erst ändert, nachdem der Benutzer die App gestartet hat und mit ihr interagiert. Lokale Benachrichtigungen sind auch eine gute Möglichkeit, die App-Kachel aktuell zu halten, selbst wenn Sie auch einen der anderen Benachrichtigungsmechanismen verwenden. Auf der Kachel einer Foto-App könnten z.B. Fotos aus einem kürzlich hinzugefügten Album angezeigt werden.

Es empfiehlt sich, die Kachel beim ersten Start der App lokal zu aktualisieren (oder spätestens dann, wenn der Benutzer eine Änderung vornimmt, die üblicherweise auf der Kachel zu sehen ist). Indem Sie diese Änderung schon während der Verwendung der App vornehmen, stellen Sie sicher, dass die Kachel bereits auf dem neuesten Stand ist, wenn der Benutzer die App beendet.

Auch wenn die API-Aufrufe lokal erfolgen, können die Benachrichtigungen auf Webbilder verweisen. Wenn das Webbild nicht zum Download verfügbar oder beschädigt ist oder nicht den Bildspezifikationen entspricht, reagieren Kacheln und Popups unterschiedlich:

-   Kacheln: Die Aktualisierung wird nicht angezeigt.
-   Popup: Die Benachrichtigung wird angezeigt, allerdings mit einem Platzhalterbild.

Obwohl lokale Benachrichtigungen nicht ablaufen, sollten Sie trotzdem eine explizite Ablaufzeit festlegen.

Weitere Informationen finden Sie unter folgenden Themen:

-   [Senden einer lokalen Kachelbenachrichtigung](tiles-and-notifications-sending-a-local-tile-notification.md)
-   [Codebeispiele für UWP (universelle Windows-Plattform)-Benachrichtigungen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Scheduled_notifications"></span><span id="scheduled_notifications"></span><span id="SCHEDULED_NOTIFICATIONS"></span>Geplante Benachrichtigungen


Geplante Benachrichtigungen sind lokale Benachrichtigungen, die den genauen Zeitpunkt angeben können, zu dem eine Kachel aktualisiert oder eine Popupbenachrichtigung angezeigt werden soll. Geplante Benachrichtigungen eignen sich ideal für Situationen, in denen der zu aktualisierende Inhalt im Voraus bekannt ist (z.B. eine Besprechungseinladung). Wenn Sie den Benachrichtigungsinhalt vorher nicht kennen, müssen Sie eine Pushbenachrichtigung oder eine lokale Benachrichtigung verwenden.

Geplante Benachrichtigungen laufen standardmäßig drei Tage nach ihrer Zustellung ab. Diese Standardeinstellung kann bei Bedarf mit einer anderen Ablaufzeit explizit überschrieben werden.

Weitere Informationen finden Sie unter folgenden Themen:

-   [Richtlinien für geplante Benachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh761464)
-   [Codebeispiele für UWP (universelle Windows-Plattform)-Benachrichtigungen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Periodic_notifications"></span><span id="periodic_notifications"></span><span id="PERIODIC_NOTIFICATIONS"></span>Periodische Benachrichtigungen


Periodische Benachrichtigungen bieten Ihnen Livekachelaktualisierungen mit minimaler Investition in Clouddienst und Client. Sie stellen auch eine exzellente Methode zum Verteilen desselben Inhalts an eine große Zielgruppe dar. Ihr Clientcode gibt die URL eines Cloudspeicherorts an, von dem Windows Kachel- und Signalaktualisierungen abruft, und die Häufigkeit, mit der der Speicherort abgerufen wird. In jedem Abrufintervall wird die URL von Windows aufgerufen, um den angegebene XML-Inhalt herunterzuladen und auf der Kachel anzuzeigen.

Für periodische Benachrichtigungen muss die App einen Clouddienst hosten, und dieser Dienst wird im angegebenen Intervall von allen Benutzern abgerufen, die die App installiert haben. Beachten Sie, dass periodische Aktualisierungen nicht für Popupbenachrichtigungen verwendet werden können. Popupbenachrichtigungen werden am besten mithilfe von geplanten Benachrichtigungen oder Pushbenachrichtigungen bereitgestellt.

Regelmäßige Benachrichtigungen laufen standardmäßig drei Tage nach der Abfrage ab. Diese Standardeinstellung kann bei Bedarf mit einer anderen Ablaufzeit explizit überschrieben werden.

Weitere Informationen finden Sie unter folgenden Themen:

-   [Übersicht über regelmäßige Benachrichtigungen](tiles-and-notifications-periodic-notification-overview.md)
-   [Codebeispiele für UWP (universelle Windows-Plattform)-Benachrichtigungen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Push_notifications"></span><span id="push_notifications"></span><span id="PUSH_NOTIFICATIONS"></span>Pushbenachrichtigungen


Pushbenachrichtigungen eignen sich ideal zum Kommunizieren von Echtzeitdaten oder für den Benutzer personalisierten Daten. Pushbenachrichtigungen werden auch für Inhalte verwendet, die zu unvorhersehbaren Zeiten generiert werden (z.B. Eilmeldungen oder Chatnachrichten). Pushbenachrichtigungen sind auch in Situationen hilfreich, in denen Daten so zeitempfindlich sind, dass periodische Benachrichtigungen nicht geeignet wären (z.B. Spielstände während eines Spiels).

Für Pushbenachrichtigungen ist ein Cloud-Dienst erforderlich, der Pushbenachrichtigungskanäle verwaltet und festlegt, wann und an wen Benachrichtigungen gesendet werden.

Pushbenachrichtigungen laufen standardmäßig drei Tage nach dem Empfang durch die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS) ab. Diese Standardeinstellung kann bei Bedarf mit einer anderen Ablaufzeit explizit überschrieben werden.

Weitere Informationen finden Sie unter:

-   [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
-   [Richtlinien für Pushbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh761462)
-   [Codebeispiele für UWP (Universelle Windows-Plattform)-Benachrichtigungen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)


## <span id="related_topics"></span>Verwandte Themen


* [Senden einer lokalen Kachelbenachrichtigung](tiles-and-notifications-sending-a-local-tile-notification.md)
* [Richtlinien für Pushbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Richtlinien für geplante Benachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh761464)
* [Richtlinien für Popupbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh465391)
* [Übersicht über regelmäßige Benachrichtigungen](tiles-and-notifications-periodic-notification-overview.md)
* [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
* [Codebeispiele für UWP (universelle Windows-Plattform)-Benachrichtigungen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)
 

 







<!--HONumber=Jun16_HO4-->


