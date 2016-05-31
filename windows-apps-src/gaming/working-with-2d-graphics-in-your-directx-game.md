---
author: mtoepke
title: 2D-Grafiken für DirectX-Spiele
description: Hier finden Sie Informationen zum Einsatz von 2D-Bitmapgrafiken und -effekten im Allgemeinen und in Ihrem Spiel.
ms.assetid: ad69e680-d709-83d7-4a4c-7bbfe0766bc7
---

# 2D-Grafiken für DirectX-Spiele


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Hier finden Sie Informationen zum Einsatz von 2D-Bitmapgrafiken und -effekten im Allgemeinen und in Ihrem Spiel.

2D-Grafiken sind eine Untergruppe von 3D-Grafiken mit 2D-Grundtypen oder -Bitmaps. Ganz allgemein wird hier keine z-Koordinate wie in einem 3D-Spiel verwendet, da das Gameplay üblicherweise auf die x-y-Ebene beschränkt ist. Spiele dieser Art nutzen gelegentlich 3D-Grafiktechniken zur Erstellung visueller Komponenten und sind im Allgemeinen einfacher zu entwickeln. Falls Sie noch keine Erfahrungen im Bereich der Spieleentwicklung gesammelt haben, ist ein 2D-Spiel eine gute Einstiegsmöglichkeit. Bei der Entwicklung von 2D-Grafiken können Sie sich zudem sehr gut in die Verwendung von DirectX einarbeiten.

2D-Spielgrafiken können in DirectX mit Direct2D, Direct3D oder einer Kombination aus beidem entwickelt werden. Viele der hilfreichen Klassen für die Entwicklung von 2D-Spielen befinden sich in Direct3D (so zum Beispiel die [**Sprite**](https://msdn.microsoft.com/library/windows/desktop/bb205601)-Klasse). Bei Direct2D handelt es sich um eine Gruppe von APIs, die in erster Linie für Benutzeroberflächen und Apps gedacht sind, die das Zeichnen von Grundtypen (wie Kreise, Linien und flache Polygonformen) unterstützen müssen. Darüber hinaus steht Ihnen aber auch eine leistungsfähige Sammlung von Klassen und Methoden für die Entwicklung von Spielgrafiken (insbesondere für Overlays, Oberflächen und Head-up-Displays (HUDs) für das Spiel) oder für die Entwicklung verschiedenster 2D-Spiele zur Verfügung – von sehr einfachen Spielen bis hin zu Spielen mit recht hohem Detailgrad. Da sich 2D-Grafiken allerdings am effektivsten mit Elementen aus beiden Bibliotheken entwickeln lassen, werden wir diesen Ansatz auch in diesem Thema verfolgen.

## Konzepte auf einen Blick


Vor dem Siegeszug moderner 3D-Grafiken und der entsprechenden Hardware waren Spiele in erster Linie zweidimensional, und bei der Grafiktechnologie ging es häufig um die Verschiebung von Speicherblöcken. Hierbei handelte es sich üblicherweise um Arrays mit Farbdaten, die 1:1 in Pixel auf dem Monitor übersetzt bzw. umgewandelt wurden.

In DirectX sind 2D-Grafiken Teil der 3D-Pipeline. Die Menge der verfügbaren Bildschirmauflösungen und Grafikhardware hat deutlich zugenommen, und Ihre 2D-Grafikengine muss diese ohne spürbare Unterschiede bei der Wiedergabetreue unterstützen.

Im Anschluss finden Sie einige der grundlegenden Konzepte, mit denen Sie vertraut sein sollten, bevor Sie mit der Entwicklung von 2D-Grafiken beginnen:

-   Pixel und Rasterkoordinaten. Ein Pixel ist ein einzelner Punkt einer Rasteranzeige und besitzt lediglich ein Koordinatenpaar (bestehend aus x- und y-Koordinate), das seine Anzeigeposition angibt. (Der Begriff „Pixel“ wird häufig synonym für die physischen Pixel der Anzeige und für die adressierbaren Speicherelemente verwendet, die die Farb- und Alphawerte der Pixel enthalten, bevor sie an die Anzeige gesendet werden.) Das Raster wird von APIs als rechteckiges Raster mit Pixelelementen behandelt. Dieses entspricht häufig 1:1 dem Raster der physischen Pixel einer Anzeige. Rasterkoordinatensysteme beginnen links oben mit dem Pixel an der Position (0, 0). Dieses Pixel befindet sich somit im Raster ganz links oben.
-   Bitmapgrafiken werden gelegentlich auch als Rastergrafiken bezeichnet und sind Grafikelemente, die als rechteckiges Raster mit Pixelwerten dargestellt werden. Sprites (berechnete Pixelarrays, die unabhängig vom Raster verwaltet werden) sind eine Art von Bitmapgrafik, die in einem Spiel häufig für die aktiven Charaktere oder für animierte Objekte verwendet werden, die vom Hintergrund unabhängig sind. Die verschiedenen Animationsframes für ein Sprite werden als Bitmapsammlungen dargestellt und als Blätter oder Stapel bezeichnet. Bei Hintergründen handelt es sich um größere Bitmapobjekte, die mindestens die gleiche Auflösung besitzen wie das Bildschirmraster und häufig als eine Art Kulisse für das Spielfeld des Spiels fungieren.
-   Vektorgrafiken nutzen geometrische Grundtypen wie Punkte, Linien, Kreise und Polgygone, um zweidimensionale Objekte zu definieren. Sie werden nicht als Pixelarrays dargestellt, sondern als mathematische Gleichungen, die sie im zweidimensionalen Raum definieren. Sie entsprechen nicht unbedingt 1:1 dem Pixelraster der Anzeige und müssen aus dem Koordinatensystem, in dem sie gerendert wurden, in das Rasterkoordinatensystem der Anzeige transformiert werden.
-   Bei der Translation wird die neue Position eines Punkts oder Vertexes im gleichen Koordinatensystem berechnet.
-   Beim Skalieren wird ein Objekt um einen bestimmten Skalierungsfaktor vergrößert oder verkleinert. Bei einem Vektorbild werden die Komponentenscheitelpunkte verkleinert bzw. vergrößert. Bei einer Bitmap werden die Pixelelemente vergrößert oder verkleinert. Beim Verkleinern von Bitmapbildern gehen Pixeldaten verloren, beim Vergrößern des Bilds werden dagegen die einzelnen Pixel vergrößert. Im zweiten Fall können Sie mithilfe von Pixel-Farbinterpolationen (beispielsweise bilineare Filterung) die harten Übergänge zwischen den vergrößerten Pixeln glätten.
-   Drehung bezeichnet das Drehen eines Objekts um mindestens eine feste Achse. Bei einem Vektorbild werden die Vertices der Geometrie anhand einer Rotationsmatrix multipliziert, um den gedrehten Vertex zu erhalten. Bei einem Bitmapbild können verschiedene Algorithmen verwendet werden, die jeweils zu unterschiedlich exakten Ergebnissen führen. Genau wie bei der Skalierung und Translation gibt es auch für die Drehung spezielle APIs.
-   Bei der Transformation wird für einen (Scheitel-)Punkt in einem Koordinatensystem ein entsprechender (Scheitel-)Punkt in einem anderen Koordinatensystem berechnet. Dies beinhaltet Translation, Skalierung und Drehung sowie andere Koordinatenberechnungen.
-   Beim Clipping werden Teile der Bitmaps oder Geometrie entfernt, die sich nicht im sichtbaren Bereich der Anzeige befinden oder von Objekten mit höherer Anzeigepriorität verdeckt werden.
-   Der Framepuffer ist ein Speicherbereich, der sich häufig in der Grafikhardware befindet und die finale Rasterstruktur enthält, die auf den Bildschirm gezeichnet wird. Die Swapchain ist eine Sammlung von Puffern. Hier erfolgt der Zeichnungsvorgang in einem Hintergrundpuffer, und das Bild wird nach Fertigstellung des Vorgangs in den Vordergrund gebracht, um es anzuzeigen.

## Überlegungen bei der Entwicklung


Mit der Entwicklung von 2D-Grafiken können Sie sich sehr gut in die Direct3D-Entwicklung einarbeiten und haben mehr Zeit für andere wichtige Aspekte der Spieleentwicklung wie Audio, Steuerung und Spielmechanik.

Verwenden Sie für den Zeichnungsvorgang immer einen Hintergrundpuffer. Wenn der Zeichnungsvorgang direkt im Framepuffer erfolgt, wird das Bild angezeigt, sobald das Signal zum Anzeigen empfangen wird (üblicherweise jede sechzigstel Sekunde), auch wenn der Zeichnungsvorgang noch gar nicht abgeschlossen sein sollte.

Achten Sie bei Ihrer Grafikengine darauf, dass diese eine Reihe von Auflösungen unterstützt – von 1024 x 600 bis mindestens 1920 x 1080. Ihre Zielgruppe wird es zu schätzen wissen, wenn Ihr Spiel die native Auflösung ihres LCD-Monitors unterstützt (besonders bei 2D-Grafiken).

In Sachen Optik sind großartige Grafiken Ihre wichtigste Ressource. Ihre Bitmapgrafiken sind zwar vielleicht nicht ganz so beeindruckend wie fotorealistische 3D-Grafik mit den neuesten Shadermodell-Features, mit einer großartigen Grafik in hoher Auflösung können Sie aber mindestens genauso viel Stil und Persönlichkeit erzeugen – und das bei deutlich geringerem Leistungsbedarf.

## Referenz


-   [Übersicht über Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987)
-   [Schnellstartanleitung für Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd535473)
-   [Übersicht über die Interoperabilität von Direct2D und Direct3D](https://msdn.microsoft.com/library/windows/desktop/dd370966)

> **Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 






<!--HONumber=May16_HO2-->


