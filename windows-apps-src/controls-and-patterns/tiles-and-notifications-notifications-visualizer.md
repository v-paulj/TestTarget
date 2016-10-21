---
author: mijacobs
Description: "Notifications Visualizer ist eine neue App für die Universelle Windows-Plattform (UWP) im Store, die Entwickler dabei unterstützt, adaptive Livekacheln für Windows 10 zu entwerfen."
title: Notifications Visualizer
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
label: TBD
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: a954404ccc2e986c1603402315c8497f802ad254

---
# Notifications Visualizer

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


Notifications Visualizer ist eine neue App für die Universelle Windows-Plattform (UWP) im [Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1), die Entwickler dabei unterstützt, adaptive Livekacheln für Windows 10 zu entwerfen.

## Übersicht


Die App „Notifications Visualizer” bietet beim Bearbeiten eine sofortige visuelle Vorschau Ihrer Kachel, vergleichbar mit dem XAML-Editor/der Designansicht von Visual Studio. Die App prüft auch auf Fehler. So wird sichergestellt, dass Sie eine gültige Kachelnutzlast erstellen.

Dieser Screenshot der App zeigt die XML-Nutzlast und die Art und Weise, wie Kachelgrößen auf einem bestimmten Gerät angezeigt werden:

![Screenshot des App-Editors von Notifications Visualizer mit Code und Kacheln](images/notif-visualizer-001.png)

 

Mit Notifications Visualizer können Sie adaptive Kachelnutzlasten erstellen und testen, ohne die App selbst bearbeiten und bereitstellen zu müssen. Nachdem Sie eine Nutzlast mit idealen visuellen Ergebnisse erstellt haben, können Sie sie in Ihre App integrieren. Weitere Informationen finden Sie unter [Senden einer lokalen Kachelbenachrichtigung](tiles-and-notifications-sending-a-local-tile-notification.md).

**Hinweis**   Die Simulation des Windows-Startmenüs mit Notifications Visualizer ist nicht immer hundertprozentig genau, und bestimmte Nutzlasteigenschaften wie [baseUri](https://msdn.microsoft.com/library/windows/apps/br208712) werden nicht unterstützt. Wenn Sie das gewünschte Kacheldesign fertig haben, testen Sie es, indem Sie die Kachel an das tatsächliche Startmenü anheften, um sicherzustellen, dass es wie gewünscht angezeigt wird.

 

## Features


Notifications Visualizer enthält eine Reihe von Beispielnutzlasten, um zu zeigen, was mit adaptiven Live-Kacheln möglich ist und um Sie bei den ersten Schritten zu unterstützen. Sie können mit den verschiedenen Textoptionen, Gruppen/Untergruppen, Hintergrundbildern experimentieren und sehen, wie sich die Kachel an verschiedene Geräte und Bildschirme anpasst. Wenn Sie Änderungen vornehmen, können Sie Ihre aktualisierte Nutzlast in einer Datei zur späteren Verwendung speichern.

Der Editor stellt Fehler und Warnungen in Echtzeit bereit. Wenn Ihre App-Nutzlast beispielsweise auf weniger als 5KB (eine Plattformbeschränkung) begrenzt ist, warnt Notifications Visualizer Sie, falls Ihre Nutzlast diese Grenze überschreitet. Sie erhalten Warnungen aufgrund falscher Attributnamen oder Werte, wodurch das Debuggen visueller Probleme erleichtert wird.

Sie können Kacheleigenschaften, wie Anzeigename, Farbe, Logos, ShowName, Signalwert, steuern. Anhand dieser Optionen verstehen Sie sofort, wie Ihre Kacheleigenschaften und Kachelbenachrichtigungsnutzlasten interagieren und welche Ergebnisse sie produzieren.

Dieser Screenshot der App zeigt den Kachel-Editor:

![Screenshot des Notifications Visualizer-Editors mit Kacheln](images/notif-visualizer-004.png)

 

## Verwandte Themen


* [Notifications Visualizer aus dem Store herunterladen](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md)
* [Vorlagen für adaptive Kacheln: Schema und Dokumentation](tiles-and-notifications-adaptive-tiles-schema.md)
* [Kacheln und Popups (MSDN-Blog)](http://blogs.msdn.com/b/tiles_and_toasts/)
* [NotificationsExtensions-Bibliothek (MSDN-Blog)](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/08/20/introducing-notificationsextensions-for-windows-10.aspx)
 

 







<!--HONumber=Aug16_HO3-->


