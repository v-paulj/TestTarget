---
description: "Hier finden Sie einen Designleitfaden und Codierungsanweisungen für das Hinzufügen von Steuerelementen und Mustern zu Ihrer UWP-App."
title: "Steuerelemente und Muster – Entwicklung von Windows-Apps"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 9f75c39d26bd0c8858f404ab4fcd3d23562ea033
ms.openlocfilehash: 0562e3df0c2abbb0808df5f75ff4fe0b96eb6d7e

---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


# Steuerelemente und Muster für UWP-Apps

In der UWP-App-Entwicklung ist ein <i>Steuerelement</i> ein UI-Element, das Inhalte anzeigt oder Interaktionen ermöglicht. Steuerelemente sind die Bausteine der Benutzeroberfläche. Wir bieten mehr als 45 Steuerelemente, angefangen bei einfachen Schaltflächen bis hin zu leistungsstarken Datensteuerelementen wie der Rasteransicht. Ein <i>Muster</i> ist eine Anleitung zum Kombinieren verschiedener Steuerelemente, um etwas Neues zu erstellen.

Die Artikel in diesem Abschnitt enthalten Designrichtlinien und Codierungsanweisungen für das Hinzufügen von Steuerelementen und Mustern zu Ihrer UWP-App. 

## Einführung

Allgemeine Anweisungen und Codebeispiele für das Hinzufügen und Formatieren von Steuerelementen in XAML und C#.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen](controls-and-events-intro.md)</b> <br/>
Es gibt 3 wichtige Schritte, die Sie ausführen müssen, um Ihrer App Steuerelemente hinzuzufügen: das Hinzufügen eines Steuerelements zu Ihrer App-UI, das Festlegen der Eigenschaften für das Steuerelement und das Hinzufügen von Code zu den Ereignishandlern des Steuerelements, sodass dieses eine Aktion ausführt.</li>
</ul> 
</p>
  </div>
  <div class="side-by-side-content-right">
   <p><b>[Formatieren von Steuerelementen](styling-controls.md)</b> <br/>
Das XAML-Framework bietet zahlreiche Anpassungsmöglichkeiten für die App-Darstellung. Sie können mit Formaten die Steuerelementeigenschaften festlegen und diese Einstellungen dann für andere Steuerelemente wiederverwenden, um ein einheitliches Erscheinungsbild zu erzielen.</p>
  </div>
</div>
</div>

## Alphabetischer Index 

Detaillierte Informationen zu bestimmten Steuerelementen und Mustern.

(Eine nach Funktionen sortierte Liste finden Sie unter [Index der Steuerelemente nach Funktion](controls-by-function.md).)

<div class="uwpd-list-of-links">
<ul>

<li>[Aktiver Zeichenbereich](active-canvas.md)</li>

<li>[Feld mit automatischen Vorschlägen](auto-suggest-box.md)</li>

<li>[Balken](app-bars.md)</li>

<li>[Schaltflächen](buttons.md)</li>

<li>[Kontrollkästchen ](checkbox.md)</li>

<li>[Steuerelemente für Datum und Uhrzeit](date-and-time.md)
<ul>

<li>[Kalenderdatumsauswahl](calendar-date-picker.md)</li>

<li>[Kalenderansicht](calendar-view.md)</li>

<li>[Datumsauswahl](date-picker.md)</li>

<li>[Uhrzeitauswahl](time-picker.md)</li>
</ul>
</li>


<li>[Dialogfelder, Popups und Menüs](dialogs-popups-menus.md)</li>

<li>[Flip-Ansicht](flipview.md)</li>

<li>[Hub](hub.md)</li>

<li>[Hyperlinks](hyperlinks.md)</li>

<li>[Bilder und Bildpinsel](images-imagebrushes.md)</li>

<li>[Listen](lists.md)</li>

<li>[Master/Details](master-details.md)</li>

<li>[Medienwiedergabe](media-playback.md)</li>

<li>[Benutzerdefinierte Transportsteuerelemente](custom-transport-controls.md)</li>

<li>[Navigationsbereich](nav-pane.md)</li>

<li>[Statussteuerelemente](progress-controls.md)</li>

<li>[Optionsfeld](radio-button.md)</li>

<li>[Steuerelemente für Bildlauf und Schwenken](scroll-controls.md)</li>

<li>[Suche](search.md)</li>

<li>[Semantischer Zoom](semantic-zoom.md)</li>

<li>[Schieberegler](slider.md)</li>

<li>[Geteilte Ansicht](split-view.md)</li>

<li>[Registerkarten und Pivotbereiche](tabs-pivot.md)</li>

<li>[Textsteuerelemente](text-controls.md)
<ul>

<li>[Schriftarten](fonts.md)</li>
<li>[Beschriftungen](labels.md)</li>

<li>[Kennwortfelder](password-box.md)</li>

<li>[Rich-Edit-Felder](rich-edit-box.md)</li>

<li>[Rich-Text-Blöcke](rich-text-block.md)</li>

<li>[Segoe MDL2-Symbole](segoe-ui-symbol-font.md)</li>

<li>[Rechtschreibprüfung und Vorhersage](spell-checking-and-prediction.md)</li>

<li>[Textblock](text-block.md)</li>

<li>[Textfeld](text-box.md)</li>
</ul>
</li>



<li>[Kacheln, Signale und Benachrichtigungen](tiles-badges-notifications.md)
<ul>

<li>[Kacheln](tiles-and-notifications-creating-tiles.md)</li>

<li>[Adaptive Kacheln](tiles-and-notifications-create-adaptive-tiles.md)</li>

<li>[Adaptives Kachelschema](tiles-and-notifications-adaptive-tiles-schema.md)</li>

<li>[Objektrichtlinien](tiles-and-notifications-app-assets.md)</li>

<li>[Spezielle Kachelvorlagen](tiles-and-notifications-special-tile-templates-catalog.md)</li>

<li>[Adaptive und interaktive Popupbenachrichtigungen](tiles-and-notifications-adaptive-interactive-toasts.md)</li>

<li>[Notifications Visualizer](tiles-and-notifications-notifications-visualizer.md)</li>

<li>[Methoden für die Benachrichtigungsübermittlung](tiles-and-notifications-choosing-a-notification-delivery-method.md)</li>

<li>[Lokale Kachelbenachrichtigungen](tiles-and-notifications-sending-a-local-tile-notification.md)</li>

<li>[Periodische Benachrichtigungen](tiles-and-notifications-periodic-notification-overview.md)</li>

<li>[WNS](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</li>

<li>[Unformatierte Benachrichtigungen](tiles-and-notifications-raw-notification-overview.md)</li>
</ul>
</li>


<li>[Ein-/Ausschalten](toggles.md)</li>
<li>[QuickInfos](tooltips.md)</li>

<li>[Webansicht](web-view.md)</li>
</ul>
</div>



<!--HONumber=Jun16_HO4-->


