---
author: mijacobs
title: "Bildschirmgrößen und Haltepunkte für reaktionsfähiges Design"
description: .
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 5977b6253e2ee10fa0153d79053f89b705c2e1a3

---

#  Bildschirmgrößen und Haltepunkte für reaktionsfähiges Design

Die Anzahl der Geräteziele und Bildschirmgrößen bei Windows 10 ist zu groß, um alle beim Optimieren der Benutzeroberfläche zu bedenken. Stattdessen wird empfohlen, für ein paar wichtige Breiten (auch als „Haltepunkte“ bezeichnet) zu entwickeln: 360, 640, 1024 und 1366 Epx.

**Tipp** Beim Entwerfen für bestimmte Haltepunkte sollten Sie den für Ihre App verfügbaren Bildschirmbereich (App-Fenster) berücksichtigen. Wenn die App im Vollbildmodus ausgeführt wird, hat das App-Fenster die gleiche Bildschirmgröße zur Verfügung, in anderen Fällen ist es jedoch kleiner.
 

In dieser Tabelle werden die verschiedenen Größenklassen beschrieben und allgemeine Empfehlungen für die Anpassung für diese Größenklassen aufgeführt.

![Reaktionsfähige Designhaltepunkte](images/rsp-design/rspd-breakpoints.png)

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Größenklasse</th>
<th align="left">klein</th>
<th align="left">mittel</th>
<th align="left">groß</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Normale Bildschirmgröße (diagonal)</td>
<td align="left">4&quot; bis 6&quot;</td>
<td align="left">7&quot; bis 12&quot;, oder TV-Geräte</td>
<td align="left">13&quot; und größer</td>
</tr>
<tr class="even">
<td align="left">Typische Geräte</td>
<td align="left">Smartphones</td>
<td align="left">Phablets, Tablets, TV-Geräte</td>
<td align="left">PCs, Laptops, Surface Hubs</td>
</tr>
<tr class="odd">
<td align="left">Übliche Fenstergrößen in effektiven Pixeln</td>
<td align="left">320 x 569, 360 x 640, 480 x 854</td>
<td align="left">960 x 540, 1024 x 640</td>
<td align="left">1366 x 768, 1920 x 1080</td>
</tr>
<tr class="even">
<td align="left">Fensterbreite-Haltepunkte in effektiven Pixeln</td>
<td align="left">640 Pixel oder weniger</td>
<td align="left">641 Pixel bis 1007 Pixel</td>
<td align="left">1008 Pixel oder größer</td>
</tr>
<tr class="odd">
<td align="left" valign="top">Allgemeine Empfehlungen</td>
<td align="left" valign="top"><ul>
<li>Zentrieren Sie Registerkartenelemente.</li>
<li>Legen Sie den linken und den rechten Fensterrand auf 12px fest, um eine visuelle Trennung zwischen dem linken und dem rechten Rand des App-Fensters zu erzielen.</li>
<li>Docken Sie [App-Leisten](../controls-and-patterns/app-bars.md) für bessere Erreichbarkeit am unteren Fensterrand an.</li>
<li>Verwenden Sie jeweils eine Spalte/Region.</li>
<li>Verwenden Sie ein Symbol zum Darstellen der Suche (kein Suchfeld anzeigen).</li>
<li>Verwenden Sie den [Navigationsbereich](../controls-and-patterns/nav-pane.md) im Überlagerungsmodus, um Platz auf dem Bildschirm zu sparen.</li>
<li>Verwenden Sie für das [Master/Details-Modell](../controls-and-patterns/master-details.md) den gestapelten Darstellungsmodus, um Platz auf dem Bildschirm zu sparen.</li>
</ul></td>
<td align="left" valign="top"><ul>
<li>Richten Sie Registerkartenelemente linksbündig aus.</li>
<li>Legen Sie den linken und den rechten Fensterrand auf 24px fest, um eine visuelle Trennung zwischen dem linken und dem rechten Rand des App-Fensters zu erzielen.</li>
<li>Positionieren Sie Elemente wie [App-Leisten](../controls-and-patterns/app-bars.md) am oberen Rand des App-Fensters.</li>
<li>Bis zu zwei Spalten/Regionen</li>
<li>Zeigen Sie das Suchfeld an.</li>
<li>Legen Sie für [Navigationsleiste](../controls-and-patterns/nav-pane.md) den Streifenmodus fest, sodass immer ein schmaler Streifen mit Symbolen angezeigt wird.</li>
<li>Ziehen Sie weitere Anpassungen für [TV-Umgebungen](http://go.microsoft.com/fwlink/?LinkId=760736) in Erwägung.</li>
</ul></td>
<td align="left" valign="top"><ul>
<li>Richten Sie Registerkartenelemente linksbündig aus.</li>
<li>Legen Sie den linken und den rechten Fensterrand auf 24px fest, um eine visuelle Trennung zwischen dem linken und dem rechten Rand des App-Fensters zu erzielen.</li>
<li>Positionieren Sie Elemente wie [App-Leisten](../controls-and-patterns/app-bars.md) am oberen Rand des App-Fensters.</li>
<li>Bis zu drei Spalten/Regionen</li>
<li>Zeigen Sie das Suchfeld an.</li>
<li>Platzieren Sie den [Navigationsbereich](../controls-and-patterns/nav-pane.md) im angedockten Modus so, dass er immer angezeigt wird.</li>
</ul></td>
</tr>
</tbody>
</table>

Mit [**Continuum for Phones**](http://go.microsoft.com/fwlink/p/?LinkID=699431), einer neuen Funktion für kompatible Windows 10-Mobilgeräte, können Benutzer ihre Smartphones mit einem Bildschirm, einer Maus und einer Tastatur verbinden und damit ihr Gerät wie einen Laptop nutzen. Berücksichtigen Sie diese neue Funktion beim Entwerfen für bestimmte Haltepunkte – ein Mobiltelefon bleibt nicht immer in einer Klasse mit geringer Größe.
 



<!--HONumber=Aug16_HO3-->


