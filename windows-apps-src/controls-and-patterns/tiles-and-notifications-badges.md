---
author: mijacobs
Description: "Erfahren Sie, wie Sie mithilfe von Kacheln, Signalen, Popups und Benachrichtigungen Einstiegspunkte in Ihre App bereitstellen und Benutzer auf dem neuesten Stand halten können."
title: Kacheln, Signale und Benachrichtigungen
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: a02793e45f190b9401f18e845af3dc73d235c3fc

---
# Signalbenachrichtigungen für UWP-Apps

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>Eine Kachel, die mit dem numerischen Signal „63“<br/> auf 63 ungelesene E-Mails hinweist.</div>

Ein Benachrichtigungssignal enthält eine Zusammenfassung oder Statusinformationen für Ihre App. Diese Informationen können numerisch (1–99) oder eine Gruppe der vom System bereitgestellten Glyphen sein. Beispiele für Informationen, die am besten über ein Signal vermittelt werden, sind der Netzwerkverbindungsstatus in einem Onlinespiel, der Benutzerstatus in einer Nachrichten-App, die Anzahl ungelesener Nachrichten in einer E-Mail-App und die Anzahl neuer Beiträge in einer Social-Media-App. 

Benachrichtigungssignale werden unabhängig davon, ob die App gerade ausgeführt wird, auf dem Taskleisten-Symbol Ihrer App und in der unteren rechten Ecke der zugehörigen Kachel angezeigt. Signale können auf allen Kachelgrößen angezeigt werden.  

**Hinweis**&nbsp;&nbsp;Es ist nicht möglich, ein eigenes Signalbild bereitzustellen. Sie können nur die vom System bereitgestellten Signalbilder verwenden.

## Numerische Signale

<table>
    <tr>
        <th>Wert</th>
        <th>Signal</th>
        <th>XML</th>
    </tr>
    <tr>
        <td>Eine Zahl zwischen 1 und 99 Ein Nullwert entspricht dem Glyphenwert "none" und führt dazu, dass das Signal gelöscht wird.</td>
        <td>![Ein numerisches Signal unter 100](images/badges/badge-numeric.png)</td>
        <td>`<badge value="1"/>`</td>
    </tr>
    <tr>
        <td>Eine beliebige Zahl über 99</td>
        <td>![Ein numerisches Signal über 99](images/badges/badge-numeric-greater.png)</td></td>
        <td>`<badge value="100"/>`</td>
    </tr>    
</table>

## Glyphensignale
Anstelle einer Zahl kann in einem Signal eine der nicht erweiterbaren Statusglyphen angezeigt werden. 

<table>
<tr>
    <th>Status</th>
    <th>Glyphe</th>
    <th>XML</th>
</tr>
<tr>
    <td>keine</td>
    <td>(Es wird kein Signal angezeigt.)</td>
    <td>`<badge value="none"/>`</td>
</tr>
<tr>
    <td>Aktivität</td>
    <td>![Glyphe](images/badges/badge-activity.png)</td>
    <td>`<badge value="activity"/>`</td>
</tr>
<tr>
    <td>Alarm</td>
    <td>![Glyphe](images/badges/badge-alarm.png)</td>
    <td>`<badge value="alarm"/>`</td>
</tr>
<tr>
    <td>Benachrichtigung</td>
    <td>![Glyphe](images/badges/badge-alert.png)</td>
    <td>`<badge value="alert"/>`</td>
</tr>
<tr>
    <td>Achtung</td>
    <td>![Glyphe](images/badges/badge-attention.png)</td>
    <td>`<badge value="attention"/>`</td>
</tr>
<tr>
    <td>verfügbar</td>
    <td>![Glyphe](images/badges/badge-available.png)</td>
    <td>`<badge value="available"/>`</td>
</tr>
<tr>
    <td>abwesend</td>
    <td>![Glyphe](images/badges/badge-away.png)</td>
    <td>`<badge value="away"/>`</td>
</tr>
<tr>
    <td>beschäftigt</td>
    <td>![Glyphe](images/badges/badge-busy.png)</td>
    <td>`<badge value="busy"/>`</td>
</tr>
<tr>
    <td>Fehler</td>
    <td>![Glyphe](images/badges/badge-error.png)</td>
    <td>`<badge value="error"/>`</td>
</tr>
<tr>
    <td>newMessage</td>
    <td>![Glyphe](images/badges/badge-newMessage.png)</td>
    <td>`<badge value="newMessage"/>`</td>
</tr>
<tr>
    <td>angehalten</td>
    <td>![Glyphe](images/badges/badge-paused.png)</td>
    <td>`<badge value="paused"/>`</td>
</tr>
<tr>
    <td>Wiedergabe</td>
    <td>![Glyphe](images/badges/badge-playing.png)</td>
    <td>`<badge value="playing"/>`</td>
</tr>
<tr>
    <td>nicht verfügbar</td>
    <td>![Glyphe](images/badges/badge-unavailable.png)</td>
    <td>`<badge value="unavailable"/>`</td>
</tr>
</table>

## Erstellen eines Signals

Diese Beispiele zeigen, wie eine Signalaktualisierung erstellt wird.

### Erstellen eines numerischen Signals

````csharp
private void setBadgeNumber(int num)
{

    // Get the blank badge XML payload for a badge number
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeNumber);

    // Set the value of the badge in the XML to our number
    XmlElement badgeElement = badgeXml.SelectSingleNode("/badge") as XmlElement;
    badgeElement.SetAttribute("value", num.ToString());

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### Erstellen eines Glyphensignals
````csharp
private void updateBadgeGlyph()
{
    string badgeGlyphValue = "alert";

    // Get the blank badge XML payload for a badge glyph
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeGlyph);

    // Set the value of the badge in the XML to our glyph value
    Windows.Data.Xml.Dom.XmlElement badgeElement = 
        badgeXml.SelectSingleNode("/badge") as Windows.Data.Xml.Dom.XmlElement;
    badgeElement.SetAttribute("value", badgeGlyphValue);

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### Löschen eines Signals

````csharp
private void clearBadge()
{
    BadgeUpdateManager.CreateBadgeUpdaterForApplication().Clear();
}
````

## Beispiele herunterladen

* [Benachrichtigungsbeispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/Notifications)<br/> Zeigt, wie Sie Live-Kacheln erstellen, Signalupdates senden und Popupbenachrichtigungen anzeigen können. 

## Verwandte Artikel

* [Adaptive und interaktive Popupbenachrichtigungen](tiles-and-notifications-adaptive-interactive-toasts.md)
* [Erstellen von Kacheln](tiles-and-notifications-creating-tiles.md)
* [Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md)


<!--HONumber=Aug16_HO3-->


