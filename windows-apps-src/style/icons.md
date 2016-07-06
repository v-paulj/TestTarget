---
author: mijacobs
Description: "Gute Symbole harmonieren mit der Typografie und der übrigen Designsprache. Sie mischen keine Metaphern und geben so einfach und schnell wie möglich lediglich die erforderlichen Informationen weiter."
title: Symbole
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: e5e601bf3ff9d0b1518c86130a5b0fefbb86a773

---

# Symbole für UWP-Apps

Gute Symbole harmonieren mit der Typografie und der übrigen Designsprache. Sie mischen keine Metaphern und geben so einfach und schnell wie möglich lediglich die erforderlichen Informationen weiter. 

## Größenabstufungen für lineare Skalierungen 

<table>
    <tr> 
        <td>16 x 16 px</td>
        <td>24 x 24 px</td>
        <td>32 x 32 px</td>
        <td>48 x 48 px</td>
    </tr>
    <tr> 
        <td>![Icons at 16x16 effective pixels](images/icons-16x16.png)</td>
        <td>![Icons at 24x24 effective pixels](images/icons-24x24.png)</td>
        <td>![Icons at 32x32 effective pixels](images/icons-32x32.png)</td>
        <td>![Icons at 48x48 effective pixels](images/icons-48x48.png)</td>
    </tr>
</table>

## Häufige Formen

Grundsätzlich sollten Symbole den vorhandenen Platz mit geringem Abstand möglichst vollständig ausnutzen. Diese Formen sind Ausgangspunkte für die Bestimmung der Größe einfacher Formen. 

![Raster der Größe 32 x 32 px](images/icons-common-shapes.png)

Verwenden Sie die Form, die der Ausrichtung des Symbols entspricht, und berücksichtigen Sie dabei diese grundlegenden Parameter. Symbole müssen die Form nicht unbedingt ausfüllen oder vollständig hinein passen. Sie können nach Bedarf angepasst werden, um ausgewogen zu wirken. 

<table>
    <tr>
        <td>Kreis<td>
        <td>Quadrat</td>
        <td>Dreieck</td>
    </tr>
    <tr>
        <td>![A circle](images/icons-common-shapes-examples-1.png)<td>
        <td>![A square](images/icons-common-shapes-examples-2.png)</td>
        <td>![A triangle ](images/icons-common-shapes-examples-3.png)</td>
    </tr>
        <tr>
        <td>Horizontales Rechteck<td>
        <td colspan="2">Vertikales Rechteck</td>        
        </tr>
    <tr>
        <td>![A horizontal rectangle](images/icons-common-shapes-examples-4.png)<td>
        <td colspan="2">![A vertical rectangle](images/icons-common-shapes-examples-5.png)</td>
         
    </tr>

</table>

## Winkel

Neben der Verwendung des gleichen Rasters und der gleichen Linienbreite werden Symbole mit einheitlichen Elemente erstellt. 

Wenn Sie beim Erstellen von Formen nur diese Winkel verwenden, sehen all Ihre Symbole einheitlich aus. Außerdem werden die Symbole dann richtig gerendert. 

Diese Linien können beim Erstellen von Symbolen kombiniert, verknüpft, gedreht und gespiegelt werden. 

<table>
    <tr>
        <td>**1:1**<br/>45°</td>
        <td>**1:2**<br />26,57° (vertikal)<br/>63,43° (horizontal)</td>
        <td>**1:3**<br/>18,43° (vertikal)<br/>71,57° (horizontal)</td>
        <td>**1:4**<br/>14,04° (vertikal)<br/>75,96° (horizontal)</td>
    </tr>
    <tr>
        
        <td>![1:1](images/icons-grid-1-1.png)</td>
        <td>![1:2](images/icons-grid-1-2.png)</td>
        <td>![1:3](images/icons-grid-1-3.png)</td>
        <td>![1:4](images/icons-grid-1-4.png)</td>
    </tr>  
</table>

<p>Einige Beispiele:</p>

<table>
    <tr>
        <td>![A 1:1 angle example](images/icons-angles-examples-1.png)</td>
        <td>![A 1:2 angle example](images/icons-angles-examples-2.png)</td>
        <td>![A 1:3 angle example](images/icons-angles-examples-3.png)</td>
        <td>![A 1:4 angle example](images/icons-angles-examples-4.png)</td>
    </tr>
</table>

## Kurven

Kurvenlinien entstehen aus Abschnitten eines ganzen Kreises und sollten nur dann verzerrt werden, wenn dies für die Verankerung am Pixelraster erforderlich ist. 

<table>
    <tr>
        <td>1/4-Kreis</td>
        <td>1/8-Kreis</td>
    </tr>
    <tr>
        <td>![1/4 circle](images/icons-curves-14circle.png)</td>
        <td>![1/8 circle](images/icons-curves-18circle.png)</td>
    </tr>
    <tr>
        <td>![1/4 cirlce example](images/icons-curves-examples-1.png)</td>
        <td>![1/8 circle example](images/icons-curves-examples-2.png)</td>
    </tr>    
</table>

## Geometrische Konstruktion

Wir empfehlen, beim Erstellen von Symbolen ausschließlich rein geometrische Formen zu verwenden.

![Gitarrensymbol mit geometrischer Überlagerung ](images/icons-geometric-construction.png)

## Gefüllte Formen 

Bei Bedarf können Symbole gefüllte Formen enthalten, aber sie sollten bei einer Rastergröße von 32 × 32 px maximal 4 px groß sein. Gefüllte Kreise dürfen maximal 6 × 6 px groß sein. 

![Füllung mit 5 px x 8 px ](images/icons-filled-shapes.png)

## Signale

Ein „Signal“ ist ein allgemeiner Begriff zur Beschreibung eines Elements, das einem Symbol hinzugefügt wird, das nicht in das Ausgangselement des Symbols integriert werden soll. Diese enthalten in der Regel andere Informationen über das Symbol wie Status oder Aktion. Andere gängige Begriffe sind beispielsweise Überlagerung, Anmerkung oder Modifizierer. 

![Statussignal ](images/icons-badge-status.png)

![Aktionssignal ](images/icons-badge-action.png)

Statussignale nutzen ein ausgefülltes, farbiges Objekt, das sich auf dem Symbol befindet, während Aktionssignale im gleichen monochromen Stil und mit der gleichen Linienstärke in das Symbol integriert sind.

<table>
<tr>
    <td>Gängige Statussignale</td>
    <td>Gängige Aktionssignale</td>
</tr>
<tr>
    <td>![Status badge ](images/icons-badge-common-states-1.png)</td>
    <td>![Action badge ](images/icons-badge-common-states-2.png)</td>
</tr>
</table>
<p></p>

### Signalfarbe 

Farbsignale sollten nur zur Übermittlung des Status eines Symbols verwendet werden. Die Farben für Statussignale übermitteln dem Benutzer bestimmte emotionale Botschaften. 

<table>
<tr><td>Grün: #128B44</td><td>Blau: #2C71B9</td><td>Gelb: #FDC214</td></tr>
<tr><td>Positiv: fertig, abgeschlossen </td><td>Neutral: Hilfe, Benachrichtigung </td><td>Warnend: Hinweis, Warnung </td></tr>
<tr><td>![Green status](images/icons-color-inbadging-1.png)</td><td>![Blue status](images/icons-color-inbadging-2.png)</td>
<td>![Yellow status](images/icons-color-inbadging-3.png)</td></tr>
</table>
<p></p>

### Signalposition

Die Standardposition für jeden Status bzw. jede Aktion ist unten rechts. Verwenden Sie die anderen Positionen nur dann, wenn das Design die Standardposition nicht zulässt. 

### Signalgröße

Signale sollten bei einem Raster der Größe 32 × 32 px 10 bis 18 px umfassen. 

## Verwandte Artikel

* [Richtlinien für die Ressourcen für Kacheln und Symbole](../controls-and-patterns/tiles-and-notifications-app-assets.md)



<!--HONumber=Jun16_HO4-->


