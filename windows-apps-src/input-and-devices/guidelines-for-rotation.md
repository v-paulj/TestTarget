---
Description: Description: In diesem Thema wird die neue Windows-Benutzeroberfläche für Drehungen beschrieben. Außerdem enthält das Thema Richtlinien für die Benutzeroberfläche, die Sie berücksichtigen sollten, wenn Sie diesen neuen Interaktionsmechanismus in einer Windows Store-App verwenden.
title: title: Drehung
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
---

# ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca


label: Drehung template: detail.hbs

Drehung

**\[ Aktualisiert für UWP-Apps unter Windows 10.**

-   [**Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]**](https://msdn.microsoft.com/library/windows/apps/br242084)
-   [**In diesem Artikel wird die neue Windows-Benutzeroberfläche für Drehungen beschrieben. Außerdem enthält das Thema Richtlinien für die Benutzeroberfläche, die Sie berücksichtigen sollten, wenn Sie diesen neuen Interaktionsmechanismus in einer UWP-App verwenden.**](https://msdn.microsoft.com/library/windows/apps/br227994)


## Wichtige APIs


-   Windows.UI.Input

## Windows.UI.Xaml.Input


**<span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Empfohlene und nicht empfohlene Vorgehensweisen**

Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können.

<span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Weitere Hinweise zur Verwendung

-   Übersicht über Drehung
-   Die Drehung ist eine für Touchscreens optimierte Technik, die von UWP-Apps verwendet wird und mit der Benutzer ein Objekt kreisförmig drehen können (im Uhrzeigersinn oder gegen den Uhrzeigersinn).

**Abhängig vom Eingabegerät wird für die Drehungsinteraktion Folgendes verwendet:**

Eine Maus oder ein aktiver Zeichenstift/Eingabestift zum Verschieben des Drehungsziehelements eines ausgewählten Objekts. Berührung oder passiver Zeichen-/Eingabestift zum Drehen des Objekts in die gewünschte Richtung mit der Drehbewegung.

![Gründe für die Verwendung von Drehung](images/ux-rotate-positions.png)

**Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können.**  
In den folgenden Diagrammen sehen Sie einige der unterstützten Fingerpositionen für die Drehungsinteraktion. Diagramm zur Veranschaulichung verschiedener Fingerhaltungen, die für Drehungen unterstützt werden

**Hinweis**

![Intuitiv ist der Drehungspunkt in den meisten Fällen einer der zwei Berührungspunkte, es sei denn, der Benutzer kann einen Drehungspunkt angeben, der in keinem Bezug zu den Kontaktpunkten steht (beispielsweise in einer Zeichen- oder Layoutanwendung).](images/ux-rotate-points1.png)
In den folgenden Abbildungen wird gezeigt, wie die Benutzeroberfläche beeinträchtigt werden kann, wenn der Drehungspunkt nicht auf diese Weise eingeschränkt ist. In der ersten Abbildung sehen Sie den ersten (Daumen) und den zweiten Berührungspunkt (Zeigefinger): Der Zeigefinger berührt einen Baum, und der Daumen berührt einen Holzblock.

![Abbildung mit den zwei anfänglichen Berührungspunkten für die Drehbewegung](images/ux-rotate-points2.png)
In dieser zweiten Abbildung wird die Drehung um den anfänglichen Berührungspunkt (Daumen) ausgeführt. Nach der Drehung berührt immer noch der Zeigefinger den Baumstamm und der Daumen den Holzblock (Drehungspunkt).

![Abbildung mit einem gedrehten Bild, dessen Drehungspunkt durch einen der zwei anfänglichen Berührungspunkte eingeschränkt ist](images/ux-rotate-points3.png)
In dieser dritten Abbildung wurde der Mittelpunkt der Drehung durch die Anwendung als Mittelpunkt des Bilds definiert (oder vom Benutzer entsprechend festgelegt). Da das Bild nicht um einen der Finger gedreht wurde, hat der Benutzer nach der Drehung nicht das Gefühl, direkt etwas geändert zu haben (es sei denn, er hat diese Einstellung ausgewählt).

![Abbildung mit einem gedrehten Bild, dessen Drehungspunkt durch den Mittelpunkt des Bilds anstatt durch die zwei anfänglichen Berührungspunkte eingeschränkt ist](images/ux-rotate-points4.png)

 

In dieser letzten Abbildung wurde der Mittelpunkt der Drehung durch die Anwendung als Mittelpunkt des Bilds definiert (oder vom Benutzer entsprechend festgelegt).

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Auch in diesem Fall hat der Benutzer nicht das Gefühl, direkt etwas geändert zu haben (es sei denn, er hat diese Einstellung ausgewählt).</th>
<th align="left">Abbildung mit einem gedrehten Bild, dessen Drehungspunkt durch den am weitesten links angeordneten Mittelpunkt des Bilds anstatt durch die zwei anfänglichen Berührungspunkte eingeschränkt ist</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Windows 8 unterstützt drei Drehungsarten: frei, eingeschränkt und kombiniert.</td>
<td align="left"><p>Typ Beschreibung Freie Drehung</p></td>
</tr>
<tr class="even">
<td align="left">Bei der freien Drehung können Benutzer Inhalte frei um bis zu 360 Grad drehen.</td>
<td align="left"><p>Wenn das Objekt losgelassen wird, bleibt es in der ausgewählten Position. Die freie Drehung ist hilfreich in Zeichen- und Layoutanwendungen wie beispielsweise Microsoft PowerPoint, Word, Visio und Paint sowie Adobe Photoshop, Illustrator und Flash.</p>
<p>Eingeschränkte Drehung Die eingeschränkte Drehung unterstützt freie Drehung während der Änderung, erzwingt aber beim Loslassen Andockpunkte in 90-Grad-Schritten (0, 90, 180 und 270). Wenn Benutzer das Objekt loslassen, wird es automatisch zum nächsten Andockpunkt gedreht.</p></td>
</tr>
<tr class="odd">
<td align="left">Die eingeschränkte Drehung ist die gängigste Drehungsmethode und funktioniert ähnlich wie ein Bildlauf in Inhalten.</td>
<td align="left"><p>Dank der Andockpunkte müssen Benutzer nicht präzise arbeiten und können trotzdem ihr Ziel erreichen. Die eingeschränkte Drehung ist hilfreich für Anwendungen wie beispielsweise Webbrowser und Fotoalben.</p>
<div class="alert">Kombinierte Drehung
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## Die kombinierte Drehung unterstützt freie Drehung mit Zonen (ähnlich wie Führungsschienen in [Guidelines for panning](guidelines-for-panning.md)) an jedem der 90-Grad-Andockpunkte, die durch die eingeschränkte Drehung erzwungen werden.


**Wenn das Objekt außerhalb einer der 90-Grad-Zonen losgelassen wird, bleibt das Objekt in dieser Position. Anderenfalls wird das Objekt automatisch zu einem Andockpunkt gedreht.**
* [
<strong>Hinweis</strong> Eine Benutzeroberflächen-Führungsschiene ist ein Feature, bei dem ein Bereich um ein Ziel die Bewegung zu einem bestimmten Wert oder einer bestimmten Stelle hin einschränkt, um die Auswahl zu beeinflussen.](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [<span id="related_topics"></span>Verwandte Themen](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiele](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**Eingabebeispiel mit geringer Latenz**
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [[Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [**Archivbeispiele**](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Eingabe: Beispiel XAML-Benutzereingabeereignisse](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Eingabe: Beispiel für Fingereingabe-Treffertests](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoomen](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Eingabe: vereinfachtes Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 






<!--HONumber=Apr16_HO3-->


