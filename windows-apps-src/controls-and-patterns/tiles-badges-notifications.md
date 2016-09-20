---
author: mijacobs
Description: "Erfahren Sie, wie Sie mithilfe von Kacheln, Signalen, Popups und Benachrichtigungen Einstiegspunkte in Ihre App bereitstellen und Benutzer auf dem neuesten Stand halten können."
title: Kacheln, Signale und Benachrichtigungen
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 32b1c3ec674a84ca4ed08d98119fe21f77e15554

---

# Kacheln, Signale und Benachrichtigungen für UWP-Apps




Erfahren Sie, wie Sie mithilfe von Kacheln, Signalen, Popups und Benachrichtigungen Einstiegspunkte in Ihre App bereitstellen und Benutzer auf dem neuesten Stand halten können.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><img src="images/tile-and-live-tile.png" alt="Breakdown of tile elements" /></td>
<td align="left"><p>Jede App verfügt über eine Kachel. Eine <em>Kachel</em> ist die Darstellung einer App im Menü „Start“. Sie können verschiedene Kachelgrößen festlegen (klein, mittel, breit und groß). Sie können eine <em>Kachelbenachrichtigung</em> verwenden, um die Kachel zu aktualisieren und neue Informationen an den Benutzer zu übermitteln, z.B. neue Schlagzeilen oder den Betreff der letzten ungelesenen Nachricht. Sie können ein <em>Signal</em> oder <em>Benachrichtigungssignal</em> verwenden, um Statusinfos oder zusammenfassende Informationen in Form einer vom System bereitgestellten Glyphe oder einer Zahl von 1 bis 99 bereitzustellen.</p>
<p>Eine <em>Popupbenachrichtigung</em> ist eine Benachrichtigung, die Ihre App über ein Popup-UI-Element namens <em>Popup</em> (oder <em>Banner</em>) an den Benutzer sendet. Die Benachrichtigung kann unabhängig davon gelesen werden, ob sich der Benutzer in Ihrer App befindet oder nicht.</p>
<p>Eine <em>Pushbenachrichtigung</em> oder <em>unformatierte Benachrichtigung</em> ist eine Benachrichtigung, die über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Services, WNS) oder eine Hintergrundaufgabe an Ihre App gesendet wird. Ihre App kann auf diese Benachrichtigungen entweder durch Benachrichtigen des Benutzers reagieren, das etwas von Interesse geschehen ist (über Signal-, Kachel- oder Popupaktualisierungen), oder sie können die gewünschte Reaktion selbst festlegen.</p></td>
</tr>
</tbody>
</table>

 
## Kacheln 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Erstellen von Kacheln](tiles-and-notifications-creating-tiles.md)</p></td>
<td align="left"><p>Passen Sie die Standardkachel für Ihre App an, und stellen Sie Ressourcen für unterschiedliche Bildschirmgrößen bereit.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md)</p></td>
<td align="left"><p>Vorlagen für adaptive Kacheln sind ein neues Feature in Windows 10 und ermöglichen den Entwurf eigener Inhalte für Kachelbenachrichtigungen mithilfe einer einfachen, flexiblen Markupsprache, die sich an unterschiedliche Bildschirmdichten anpasst. Dieser Artikel beschreibt, wie Sie adaptive Livekacheln für Ihre App für die Universelle Windows-Plattform (UWP) erstellen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Adaptives Kachelschema](tiles-and-notifications-adaptive-tiles-schema.md)</p></td>
<td align="left"><p>Im Folgenden werden Elemente und Attribute aufgeführt, mit denen Sie adaptive Kacheln erstellen können.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Spezielle Kachelvorlagen](tiles-and-notifications-special-tile-templates-catalog.md)</p></td>
<td align="left"><p>Spezielle Kachelvorlagen sind individuelle Vorlagen, die animiert sind oder mit denen Sie Vorgänge durchführen können, die mit adaptiven Kacheln nicht möglich sind.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Ressourcen für App-Symbol](tiles-and-notifications-app-assets.md)</p></td>
<td align="left"><p>Ressourcen für App-Symbole, die in einer Vielzahl von Formen innerhalb des Windows 10-Betriebssystems vorkommen, sind die Aushängeschilder für Ihre App für die Universelle Windows-Plattform (UWP). In diesen Richtlinien wird beschrieben, wo Ressourcen für App-Symbole im System angezeigt werden, und Sie erhalten ausführliche Designtipps zum Erstellen ansprechender Symbole.</p></td>
</tr>
</tbody>
</table>

## Benachrichtigungen


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Adaptive und interaktive Popupbenachrichtigungen](tiles-and-notifications-adaptive-interactive-toasts.md)</p></td>
<td align="left"><p>Mit adaptiven und interaktiven Popupbenachrichtigungen können Sie flexible Popupbenachrichtigungen mit mehr Inhalt, optionalen Inlinebildern und optionaler Benutzerinteraktion erstellen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Notifications Visualizer](tiles-and-notifications-notifications-visualizer.md)</p></td>
<td align="left"><p>Notifications Visualizer ist eine neue App für die Universelle Windows-Plattform (UWP) im [Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1), die Entwickler dabei unterstützt, adaptive Live-Kacheln für Windows 10 zu entwerfen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Auswählen einer Methode für die Übermittlung von Benachrichtigungen](tiles-and-notifications-choosing-a-notification-delivery-method.md)</p></td>
<td align="left"><p>In diesem Artikel werden die vier Benachrichtigungsoptionen – lokal, geplant, periodisch und Push – behandelt, die Kachel- und Signalaktualisierungen sowie Popupbenachrichtigungsinhalte bereitstellen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Senden einer lokalen Kachelbenachrichtigung](tiles-and-notifications-sending-a-local-tile-notification.md)</p></td>
<td align="left"><p>In diesem Artikel wird beschrieben, wie Sie mit adaptiven Kachelvorlagen eine lokale Benachrichtigung an eine primäre Kachel und an eine sekundäre Kachel senden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Übersicht über regelmäßige Benachrichtigungen](tiles-and-notifications-periodic-notification-overview.md)</p></td>
<td align="left"><p>Regelmäßige Benachrichtigungen – auch als abgerufene Benachrichtigungen bezeichnet – aktualisieren Kacheln und Signale in festgelegten Intervallen, indem sie Inhalte aus einem Clouddienst herunterladen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</p></td>
<td align="left"><p>Mithilfe des Windows-Pushbenachrichtigungsdiensts (WNS) können Drittanbieterentwickler Popup-, Kachel-, Signalupdates und unformatierte Updates von ihren eigenen Clouddiensten aus senden. Dadurch steht ein Mechanismus zur Verfügung, mit dem Sie Ihren Benutzern auf energieeffiziente und verlässliche Weise neue Updates bereitstellen können.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Vom Assistenten für Pushbenachrichtigungen generierter Code](tiles-and-notifications-the-code-generated-by-the-push-notification-wizard.md)</p></td>
<td align="left"><p>Mithilfe eines Assistenten in Visual Studio können Sie Pushbenachrichtigungen über einen mobilen Dienst generieren, der unter Verwendung von Azure Mobile Services erstellt wurde. Mit dem Visual Studio-Assistenten wird Code als Starthilfe generiert. In diesem Thema wird erläutert, wie der Assistent Ihr Projekt modifiziert, welche Schritte mit dem generierten Code ausgeführt werden, wie der Code verwendet wird und was Sie als Nächstes tun können, um Pushbenachrichtigungen optimal einzusetzen. Weitere Informationen finden Sie unter [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Übersicht über unformatierte Benachrichtigungen](tiles-and-notifications-raw-notification-overview.md)</p></td>
<td align="left"><p>Unformatierte Benachrichtigungen sind kurze, allgemeine Pushbenachrichtigungen. Sie dienen ausschließlich zu Anweisungszwecken und enthalten keine UI-Komponente. Wie bei anderen Pushbenachrichtigungen übermittelt das WNS-Feature unformatierte Benachrichtigungen von Ihrem Clouddienst an Ihre App.</p></td>
</tr>
</tbody>
</table>

 

 

 







<!--HONumber=Jun16_HO4-->


