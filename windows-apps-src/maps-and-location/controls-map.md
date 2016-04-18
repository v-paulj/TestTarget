---
Description: Mithilfe des Kartensteuerelements können Straßenkarten und Luftansichten, Wegbeschreibungen, Suchergebnisse und Verkehr angezeigt werden.
title: Richtlinien für Karten
ms.assetid: 7B5B6BC9-D1EC-4978-8876-20B78EF44797
---

# Richtlinien für Karten


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Mithilfe des Kartensteuerelements können Straßenkarten, Luftbilder, 3D-Ansichten, Wegbeschreibungen, Suchergebnisse und Verkehr angezeigt werden. In einer Karte können Sie die Position des Benutzers, Wegbeschreibungen und interessante Orte anzeigen. Zudem kann eine Karte 3D-Luftbilder, Streetside-Ansichten, den Verkehr, öffentliche Verkehrsmittel und lokale Unternehmen enthalten.

![Beispiel für eine Karte, Basisansicht](./images/win10fa/controls-maps-basic.jpg)

## Ist dies das richtige Steuerelement?


Verwenden Sie ein Kartensteuerelement, wenn Sie in Ihrer App eine Karte benötigen, auf der Benutzer App-spezifische oder allgemeine geografische Informationen anzeigen können. Wenn die App ein Kartensteuerelement enthält, müssen Benutzer die App für diese Informationen nicht verlassen.

**Hinweis**  Wenn es Ihnen nichts ausmacht, dass Benutzer die App für diese Informationen verlassen, können Sie diese Informationen auch mit der Windows-Karten-App bereitstellen. Ihre App kann die Windows-Karten-App starten, um bestimmte Karten, Wegbeschreibungen und Suchergebnisse anzuzeigen. Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](https://msdn.microsoft.com/library/windows/apps/mt228341).

## Beispiele


Dieses Beispiel zeigt eine Karte mit einer Streetside-Ansicht:

![Beispiel für die Streetside-Ansicht des Kartensteuerelements](./images/win10fa/controls-maps-streetside.jpg)

 

Dieses Beispiel zeigt eine Karte mit einem 3D-Luftbild:

![Beispiel für die 3D-Ansicht des Kartensteuerelements](./images/win10fa/controls-maps-3dview.jpg)

 

Dieses Beispiel zeigt eine App mit einem 3D-Luftbild und einer Streetside-Ansicht:

![Beispiel einer 3D-Kartenansicht mit Streetside-Ansicht](./images/win10fa/controls-maps-3dstreetview.png)


## Empfehlungen


-   Verwenden Sie für die Anzeige der Karte ausreichend Platz auf dem Bildschirm (oder den gesamten Bildschirm), sodass Benutzer beim Anzeigen geografischer Informationen keine übermäßigen Verschiebungen oder Zoomvorgänge ausführen müssen.

-   Wenn die Karte nur zum Anzeigen einer statischen, informativen Ansicht dient, ist möglicherweise eine kleinere Karte ausreichend. Wenn Sie eine kleinere, statische Karte verwenden, orientieren Sie sich für die Größe an der Benutzerfreundlichkeit. Die Karte sollte klein genug sein, um genügend Platz auf dem Bildschirm zu lassen, aber groß genug, um lesbar zu bleiben.

-   Betten Sie die interessanten Orte mithilfe von [**map elements**](https://msdn.microsoft.com/library/windows/apps/dn637034) in die Kartenszene ein. Zusätzliche Informationen können als vorübergehende, die Kartenszene überlagernde Benutzeroberfläche angezeigt werden.

## Verwandte Themen


* [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](https://msdn.microsoft.com/library/windows/apps/mt219695)
* [Anzeigen von interessanten Orten (POI) auf einer Karte](https://msdn.microsoft.com/library/windows/apps/mt219696)
* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Beispiel zu UWP-Karten](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [//Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Starten der Windows-Karten-App](https://msdn.microsoft.com/library/windows/apps/mt228341)
 

 






<!--HONumber=Mar16_HO1-->


