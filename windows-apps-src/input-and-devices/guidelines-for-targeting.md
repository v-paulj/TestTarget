---
author: Karl-Bridge-Microsoft
Description: "Dieses Thema beschreibt die Verwendung von Kontaktgeometrie zur Bestimmung von Touchzielen sowie bewährte Methoden für Ziele in Windows-Runtime-Apps."
title: Zielbestimmung
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 5707d36533e1b6566efb23ca9b6bf2bbf659cc63

---

# Richtlinien für die Zielbestimmung

Die Touchzielbestimmung in Windows verwendet den vollständigen Kontaktbereich jedes Fingers, der durch ein Touchdigitalisierungsgerät erkannt wird. Die größere und komplexere Menge an Eingabedaten, die vom Digitalisierungsgerät gemeldet wird, wird verwendet, um die Präzision bei der Ermittlung des durch den Benutzer (höchstwahrscheinlich) beabsichtigten Ziels zu erhöhen.

**Wichtige APIs**

-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)



Dieses Thema beschreibt die Verwendung von Kontaktgeometrie zur Bestimmung von Touchzielen und enthält bewährte Methoden für Ziele in UWP-Apps.

## Maße und Skalierung


Um die Konsistenz bei unterschiedlichen Bildschirmgrößen und Pixeldichten zu wahren, sind alle Zielgrößen in physischen Einheiten (Millimeter) angegeben. Physische Einheiten können mit der folgenden Gleichung in Pixel konvertiert werden:

Pixel = Pixeldichte × Maße

Die folgenden Beispiele verwenden diese Formel, um eine Zielgröße von 9mm auf einem Display mit 135PPI (Pixel Per Inch, Pixel pro Zoll) und einem einfachen Skalierungsplateau in die Pixelgröße umrechnen:

Pixel = 135 PPI × 9 mm

Pixel = 135PPI×(0,03937Zoll pro mm×9mm)

Pixel = 135PPI×0,35433Zoll

Pixel = 48 Pixel

Dieses Ergebnis muss an jedes der im System definierten Skalierungsplateaus angepasst werden.

## Schwellenwerte


Entfernungs- und Zeitschwellenwerte können verwendet werden, um das Ergebnis einer Interaktion zu ermitteln.

Wenn zum Beispiel ein Aufsetzen erkannt wird, wird ein Tippen registriert, wenn das Objekt in weniger als 2,7 mm Entfernung vom Aufsetzpunkt gezogen wird und die Berührung innerhalb von 0,1 Sekunden oder kürzer vom Aufsetzen aufgehoben wird. Wenn der Finger über den Schwellenwert von 2,7 mm hinaus bewegt wird, wird das Objekt gezogen und entweder ausgewählt oder verschoben (weitere Informationen finden Sie unter [Richtlinien für Querziehen](guidelines-for-cross-slide.md)). Wenn Sie abhängig von Ihrer App den Finger länger als 0,1 Sekunde gedrückt halten, kann dadurch die Ausführung einer Interaktion mit automatischem Einblenden durch das System ausgelöst werden (weitere Informationen finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md#selfreveal)).

## Zielgrößen


Verwenden Sie für Ihr Touchziel grundsätzlich mindestens eine Fläche von 9x9mm (48x48-Pixel auf einem 135PPI-Display bei einem Skalierungsplateau von1,0). Verwenden Sie keine Touchziele mit weniger als 7x7mm.

Im folgenden Diagramm wird gezeigt, dass die Größe eines Ziels in der Regel eine Kombination aus visuellem Ziel, tatsächlicher Zielgröße und dem Abstand zwischen dem tatsächlichen Ziel und den anderen potenziellen Zielen ist.

![Diagramm mit den empfohlenen Größen für visuelles Ziel, tatsächliches Ziel und Abstand](images/targeting-size.png)

Die folgende Tabelle enthält die Mindestgrößen und die empfohlenen Größen für die Komponenten eines Touchziels:

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Zielkomponente</th>
<th align="left">Mindestgröße</th>
<th align="left">Empfohlene Größe</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Abstand</td>
<td align="left">2mm</td>
<td align="left">Nicht zutreffend</td>
</tr>
<tr class="even">
<td align="left">Visuelle Zielgröße</td>
<td align="left">&lt; 60% der tatsächlichen Größe</td>
<td align="left">90 – 100% der tatsächlichen Größe
<p>Visuelle Ziele mit weniger als 4,2x 4,2mm (60% der empfohlenen Mindestgröße von 7x7mm) werden von den meisten Benutzern nicht als toucheingabefähiges Element erkannt.</p></td>
</tr>
<tr class="odd">
<td align="left">Tatsächliche Zielgröße</td>
<td align="left">7x7mm</td>
<td align="left">Mindestens 9x0mm (48x48Pixel @ 1x)</td>
</tr>
<tr class="even">
<td align="left">Gesamtzielgröße</td>
<td align="left">11 x 11 mm (ca. 60px: drei 20-px-Rastereinheiten @1x)</td>
<td align="left">13,5 x 13,5 mm (72 x 72 px @ 1x)
<p>Dies impliziert, dass die Größe des tatsächlichen Ziels und des Abstands zusammen größer sein muss als die jeweiligen Mindestgrößen.</p></td>
</tr>
</tbody>
</table>

 

Sie können diese Empfehlungen für die Zielgröße an die Anforderungen des jeweiligen Szenarios anpassen. In die Empfehlungen sind unter anderem die folgenden Überlegungen eingeflossen:

-   Häufigkeit der Fingereingaben: Erwägen Sie, Ziele, die wiederholt bzw. häufig berührt werden, größer als die Mindestgröße auszulegen.
-   Folgen von Fehlern: Ziele, bei denen die versehentliche Berührung schwerwiegende Folgen hat, sollten einen größeren Abstand haben und weiter vom Rand des Inhaltsbereichs entfernt platziert werden. Dies gilt insbesondere für Ziele, die häufig berührt werden.
-   Position im Inhaltsbereich
-   Formfaktor und Bildschirmgröße
-   Fingerhaltung
-   Fingereingabevisualisierungen
-   Hardware und Fingereingabe-Digitalisierungsgeräte

## Unterstützung bei der Zielbestimmung


Windows unterstützt die Zielbestimmung für Szenarien, in denen die hier genannten Empfehlungen für Mindestgröße und Abstand nicht anwendbar sind. Dies gilt beispielsweise für Hyperlinks auf einer Webseite, Kalendersteuerelemente, Dropdownlisten und Kombinationsfelder oder für eine Textauswahl.

Diese Verbesserungen der Zielbestimmungsplattform und des Verhaltens der Benutzeroberfläche bewirken in Kombination mit visuellem Feedback (Mehrdeutigkeitsvermeidungs-UI) mehr Genauigkeit und Sicherheit für die Benutzer. Weitere Informationen finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).

Wenn ein berührbares Element kleiner als die empfohlene Mindestzielgröße sein muss, können Sie mit den folgenden Techniken die dadurch entstehenden Probleme bei der Zielbestimmung minimieren.

## Anzeigen einer Verbindungslinie


Durch Anzeigen einer Verbindungslinie (zwischen einem Kontaktpunkt und dem Begrenzungsrechteck eines Objekts) können Sie Benutzer visuell darauf hinweisen, dass sie mit einem Objekt verbunden sind und mit diesem interagieren, obwohl kein direkter Kontakt zwischen Eingabekontakt und Objekt besteht. Dies ist in folgenden Situationen möglich:

-   Ein Berührungskontakt wurde zuerst innerhalb eines Näherungsschwellenwerts für ein Objekt erkannt, und das Objekt wurde als wahrscheinlichstes Ziel des Kontakts identifiziert.
-   Ein Berührungskontakt wurde von einem Objekt wegbewegt, aber der Kontakt befindet sich noch innerhalb eines Näherungsschwellenwerts.

Dieses Feature wird für Entwickler von Windows Store-Apps, die JavaScript verwenden, nicht verfügbar gemacht.

## Scrubbing


Beim Scrubbing wird eine beliebige Stelle in einem Feld mit Zielen berührt, und das gewünschte Ziel wird durch Ziehen ausgewählt. Der Finger wird dabei erst gehoben, wenn er sich auf dem gewünschten Ziel befindet. Dies wird auch als "Loslassaktivierung" bezeichnet, bei der das Objekt aktiviert wird, das zuletzt berührt wurde, als der Finger vom Bildschirm gehoben wurde.

Halten Sie sich an die folgenden Richtlinien, wenn Sie Scrubbinginteraktionen entwerfen:

-   Scrubbing wird in Verbindung mit der Mehrdeutigkeitsvermeidungs-UI verwendet. Weitere Informationen finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).
-   Die empfohlene Mindestgröße für ein Scrubbing-Fingereingabeziel beträgt 20px (3,75mm @1xGröße).
-   Scrubbing hat Vorrang, wenn es auf einer Oberfläche ausgeführt wird, die Verschieben unterstützt (beispielsweise eine Webseite).
-   Scrubbingziele sollten nah beieinander liegen.
-   Eine Aktion wird abgebrochen, wenn Benutzer einen Finger vom Scrubbingziel wegziehen.
-   Das Anzeigen einer Verbindungslinie zu einem Scrubbingziel wird angegeben, wenn die vom Ziel ausgeführten Aktionen nicht destruktiv sind (beispielsweise das Wechseln zwischen Daten in einem Kalender).
-   Das Anzeigen von Verbindungslinien wird in einer einzigen Richtung angegeben: horizontal oder vertikal.

## Verwandte Artikel


**Beispiele**
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: Beispiel XAML-Benutzereingabeereignisse](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für Fingereingabe-Treffertests](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoomen](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: vereinfachtes Freihandbeispiel](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Eingabe: Beispiel für Windows8-Bewegungen](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Eingabe: Beispiel für Manipulationen und Gesten (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Beispiel für die DirectX-Fingereingabe](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 







<!--HONumber=Aug16_HO3-->


