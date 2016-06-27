---
author: Karl-Bridge-Microsoft
Description: "Mit einer Verschiebung oder einem Bildlauf können Benutzer innerhalb einer einzelnen Ansicht navigieren, um den Inhalt der Ansicht anzuzeigen, der nicht in den Anzeigebereich passt. Beispiele für Ansichten sind die Ordnerstruktur eines Computers, eine Dokumentbibliothek oder ein Fotoalbum."
title: Verschieben
ms.assetid: b419f538-c7fb-4e7c-9547-5fb2494c0b71
label: Panning
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 7aafb0bbef2a33f926f76e41c26dd0f6920de274

---

# Richtlinien für das Verschieben

Mit einer Verschiebung oder einem Bildlauf können Benutzer innerhalb einer einzelnen Ansicht navigieren, um den Inhalt der Ansicht anzuzeigen, der nicht in den Anzeigebereich passt. Beispiele für Ansichten sind die Ordnerstruktur eines Computers, eine Dokumentbibliothek oder ein Fotoalbum.

**Wichtige APIs**

-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)



## <span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Empfohlene und nicht empfohlene Vorgehensweisen


**Verschiebungsindikatoren und Bildlaufleisten**

-   Stellen Sie sicher, dass eine Verschiebung bzw. ein Bildlauf möglich ist, bevor Sie Inhalt in die App laden.

-   Zeigen Sie Verschiebungsanzeigen und Bildlaufleisten als Hinweise auf die Position und die Größe an. Blenden Sie sie aus, wenn Sie eine benutzerdefinierte Navigationsfunktion bereitstellen.

    **Hinweis**  Anders als standardmäßige Bildlaufleisten haben Verschiebungsindikatoren rein informativen Charakter. Sie werden nicht für Eingabegeräte verfügbar gemacht und können in keiner Weise geändert werden.

     

**Verschiebung entlang einer Achse (eindimensionaler Überlauf)**

-   Verwenden Sie die Verschiebung entlang einer Achse für Inhaltsbereiche, die über eine Viewportgrenze (vertikal oder horizontal) hinausgehen.

    -   Vertikale Verschiebung für eine eindimensionale Liste von Elementen.
    -   Horizontale Verschiebung für ein Raster von Elementen.
-   Verwenden Sie bei der Verschiebung entlang einer Achse keine erforderlichen Andockpunkte, wenn Benutzer in der Lage sein müssen, den Inhalt zu verschieben und zwischen Andockpunkten anzuhalten. Erforderliche Andockpunkte gewährleisten, dass der Benutzer an einem Andockpunkt anhält. Verwenden Sie stattdessen Näherungsandockpunkte.

**Formfreie Verschiebung (zweidimensionaler Überlauf)**

-   Verwenden Sie die Verschiebung entlang zweier Achsen für Inhaltsbereiche, die über beide Viewportgrenzen (vertikal und horizontal) hinausgehen.

    -   Setzen Sie das Standardverhalten der Führungsschienen außer Kraft, und verwenden Sie die formfreie Verschiebung für unstrukturierten Inhalt, den der Benutzer wahrscheinlich in mehrere Richtungen verschieben möchte.
-   Die formfreie Verschiebung eignet sich in der Regel für die Navigation innerhalb von Bildern oder Karten.

**Seitenansicht**

-   Verwenden Sie erforderliche Andockpunkte, wenn sich der Inhalt aus separaten Elementen zusammensetzt oder Sie ein Element vollständig anzeigen möchten. Dies können z. B. Seiten eines Buchs oder einer Zeitschrift, eine Spalte mit Elementen oder einzelne Bilder sein.

    -   An jeder logischen Grenze sollte ein Andockpunkt platziert werden.
    -   Jedes Element sollte in seiner Größe an die Ansicht angepasst oder skaliert werden.

**Logische und Schlüsselpunkte**

-   Verwenden Sie Näherungsandockpunkte, wenn der Inhalt Schlüsselpunkte oder logische Orte aufweist, an denen Benutzer wahrscheinlich anhalten. Ein Beispiel hierfür wäre eine Abschnittsüberschrift.

-   Wenn Beschränkungen oder Grenzen der maximalen und minimalen Größe definiert sind, sollte ein visuelles Feedback erfolgen, wenn der Benutzer die Grenzen erreicht oder überschreitet.

**Verketten von eingebettetem oder geschachteltem Inhalt**

-   Verwenden Sie die Verschiebung entlang einer Achse (normalerweise horizontal) und Spaltenlayouts für text- und rasterbasierte Inhalte. In diesen Fällen wird der Inhalt meist umgebrochen und fließt natürlich von Spalte zu Spalte, sodass die Benutzeroberfläche bei allen Windows Store-Apps konsistent und erkennbar bleibt.

-   Verwenden Sie keine eingebetteten verschiebbaren Bereiche zum Anzeigen von Text oder Elementlisten. Da die Verschiebungsanzeigen und Bildlaufleisten nur angezeigt werden, wenn der Eingabekontakt im Bereich erkannt wird, wäre die Benutzeroberfläche andernfalls weder intuitiv noch erkennbar.

-   Verketten Sie keine verschiebbaren Bereiche miteinander bzw. platzieren Sie nicht einen verschiebbaren Bereich in einem anderen, wenn beide in derselben Richtung verschoben werden, wie hier gezeigt. Dies kann zu einer unbeabsichtigten Verschiebung im übergeordneten Bereich führen, wenn eine Grenze des untergeordneten Bereichs erreicht wird. Es ist sinnvoll, die Achsen für die Verschiebung im rechten Winkel zueinander anzuordnen.

    ![Abbildung eines eingebetteten verschiebbaren Bereichs, in dem der Bildlauf in derselben Richtung erfolgt wie im Container](images/scrolling-embedded3.png)

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Weitere Hinweise zur Verwendung


Das Verschieben per Toucheingabe mittels Streif- oder Ziehbewegung mit einem oder mehreren Fingern funktioniert wie der Bildlauf mit der Maus. Die Verschiebungsinteraktion gleicht eher dem Drehen des Mausrads oder Verschieben des Bildlauffelds als dem Klicken auf die Bildlaufleiste. Sofern keine Unterscheidung in einer API gemacht wird oder für eine gerätespezifische Windows-UI erforderlich ist, bezeichnen wir beide Interaktionen einfach als Verschiebung.

Je nach Eingabegerät verwendet der Benutzer eine der folgenden Methoden, um die Anzeige in einem verschiebbaren Bereich zu verschieben:

-   Eine Maus, ein Touchpad oder ein aktiver Zeichen-/Eingabestift zum Klicken auf die Bildlaufpfeile, Ziehen des Bildlauffelds oder Klicken in die Bildlaufleiste
-   Die Radtaste der Maus, um das Ziehen des Bildlauffelds zu emulieren
-   Die erweiterten Schaltflächen (XBUTTON1 und XBUTTON2) bei Unterstützung durch die Maus
-   Die Pfeiltasten auf der Tastatur, um das Ziehen des Bildlauffelds zu emulieren, oder die BILD-AB- oder BILD-AUF-TASTE, um das Klicken in die Bildlaufleiste zu emulieren
-   Toucheingabe, Touchpad oder ein passiver Zeichen-/Eingabestift zum Ziehen oder Streifen der Finger in die gewünschte Richtung

Beim Ziehen werden die Finger nur in der Verschiebungsrichtung bewegt. Diese Bewegung führt zu einem 1:1-Verhältnis, d. h. der Inhalt wird genauso schnell und weit verschoben wie die Finger bewegt werden. Beim Streifen, dem schnellen Ziehen und Anheben der Finger, werden die folgenden physischen Aspekte auf die Verschiebungsanimation angewendet:

-   Verlangsamung (Trägheit): Wenn die Finger angehoben werden, wird die Verschiebung langsamer. Dies ist mit allmählichem Anhalten auf glattem Untergrund vergleichbar.
-   Absorption: Die Dynamik der Verschiebung bewirkt bei der Verlangsamung ein leichtes Zurückspringen, wenn entweder ein Andockpunkt oder eine Grenze des Inhaltsbereichs erreicht wird.

**Arten der Verschiebung**

Windows 8 unterstützt drei Arten der Verschiebung:

-   Eine Achse – die Verschiebung wird nur in eine Richtung unterstützt (horizontal oder vertikal).
-   Führungsschienen – die Verschiebung wird in alle Richtungen unterstützt. Sobald jedoch der Benutzer in einer bestimmten Richtung eine Distanzschwelle überschreitet, wird die Verschiebung auf die betreffende Achse beschränkt.
-   Formfrei – die Verschiebung wird in alle Richtungen unterstützt.

**Verschiebungs-UI**

Die Interaktion für die Verschiebung ist von Eingabegerät zu Eingabegerät unterschiedlich, bietet aber trotzdem eine ähnliche Funktion.

**Verschiebbare Bereiche** Das Verhalten verschiebbarer Bereiche wird für Entwickler von Windows Store-Apps mit JavaScript zur Entwurfszeit über Cascading Stylesheets (CSS) verfügbar gemacht.

Abhängig vom erkannten Eingabegerät sind zwei Anzeigemodi für die Verschiebung verfügbar:

-   Verschiebungsanzeigen für Fingereingabe.
-   Bildlaufleisten für andere Eingabegeräte wie Maus, Touchpad, Tastatur und Eingabestift.

**Hinweis**  Verschiebungsanzeigen sind nur sichtbar, wenn der Berührungskontakt innerhalb des verschiebbaren Bereichs erfolgt. Ebenso ist die Bildlaufleiste nur sichtbar, wenn sich der Mauszeiger, Eingabe-/Zeichenstiftcursor oder Tastaturfokus im bildlauffähigen Bereich befindet.

 

**Verschiebungsindikatoren** Verschiebungsindikatoren sind mit dem Bildlauffeld auf einer Bildlaufleiste vergleichbar. Sie zeigen das Verhältnis zwischen dem angezeigten Inhalt und dem gesamten verschiebbaren Bereich und die relative Position des angezeigten Inhalts im verschiebbaren Bereich an.

Das folgende Diagramm zeigt zwei verschiebbare Bereiche unterschiedlicher Länge und die zugehörigen Verschiebungsindikatoren.

![Abbildung, die zwei verschiebbare Bereiche unterschiedlicher Länge und die zugehörigen Verschiebungsindikatoren zeigt](images/scrolling-indicators.png)

**Verschiebungsverhalten**
            
          
            **Andockpunkte** Beim Verschieben mit der Streifbewegung weist die Interaktion ein Trägheitsverhalten auf, wenn der Berührungskontakt gelöst wird. Ohne direkte Eingabe des Benutzers wird der Inhalt aufgrund der Trägheit weiter verschoben, bis eine Distanzschwelle erreicht wird. Verwenden Sie Andockpunkte, um dieses Trägheitsverhalten zu ändern.

Mit Andockpunkten werden logische Stopps im App-Inhalt festgelegt. Von Benutzern werden Andockpunkte als Paginierungsmechanismus wahrgenommen. Sie minimieren die Ermüdung durch ständiges Ziehen oder Streifen in großen verschiebbaren Bereichen. Mit Andockpunkten können Sie ungenaue Benutzereingaben behandeln und sicherstellen, dass eine bestimmte Teilmenge des Inhalts oder wichtige Informationen im Viewport angezeigt werden.

Es gibt zwei Arten von Andockpunkten:

-   Näherung - Nachdem der Kontakt aufhoben wurde, wird ein Andockpunkt ausgewählt, wenn die Trägheitsbewegung innerhalb einer Distanzschwelle zum Andockpunkt anhält. Die Verschiebung kann trotzdem zwischen Näherungsandockpunkten angehalten werden.
-   Erforderlich – Der ausgewählte Andockpunkt ist der Punkt direkt vor oder nach dem Andockpunkt, der vor dem Aufheben des Kontakts zuletzt überschritten wurde (abhängig von der Richtung und Geschwindigkeit der Bewegung). Die Verschiebung muss an einem erforderlichen Andockpunkt enden.

Andockpunkte für die Verschiebung sind nützlich für Anwendungen wie Webbrowser und Fotoalben, die in Seiten aufgeteilten Inhalt emulieren oder logische Gruppen von Elementen aufweisen, die dynamisch neu gruppiert werden können, damit sie in einen Viewport oder eine Anzeige passen.

Die folgenden Diagramme zeigen, wie der Inhalt automatisch zu einem logischen Ort verschoben wird, wenn der Benutzer die Verschiebung bis zu einem bestimmten Punkt ausführt und dann loslässt.

|                                                                |                                                                                         |                                                                                                                 |
|----------------------------------------------------------------|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| ![Abbildung eines verschiebbaren Bereichs](images/ux-panning-snap1.png) | ![Abbildung eines verschiebbaren Bereichs, der nach links verschoben wird](images/ux-panning-snap2.png) | ![Abbildung eines verschiebbaren Bereichs, in dem die Verschiebung an einem logischen Andockpunkt beendet wurde](images/ux-panning-snap3.png) |
| Streifen zum Verschieben.                                                  | Aufheben des Berührungskontakts.                                                                     | Der verschiebbare Bereich stoppt am Andockpunkt, nicht an der Stelle, an der der Berührungskontakt aufgehoben wurde.                                |

 

**Führungsschienen** Inhalt kann breiter und höher als die Abmessungen und die Auflösung eines Anzeigegeräts sein. Aus diesem Grund ist oft eine zweidimensionale Verschiebung (horizontal und vertikal) erforderlich. Führungsschienen verbessern in diesen Fällen die Benutzerfreundlichkeit, da sie die Verschiebung entlang der Bewegungsachse (vertikal oder horizontal) hervorheben.

Das folgende Diagramm verdeutlicht das Konzept der Führungsschienen.

![Diagramm eines Bildschirms mit Führungsschienen, die die Verschiebung einschränken](images/ux-panning-rails.png)

**Verketten von eingebettetem oder geschachteltem Inhalt**

Wenn ein Benutzer in einem Element, das in ein anderes zoomfähiges oder bildlauffähiges Element geschachtelt ist, ein Zoom- oder Bildlauflimit erreicht, können Sie angeben, ob das übergeordnete Element den im untergeordneten Element begonnenen Zoom- oder Bildlaufvorgang fortsetzen soll. Dies wird als Verketten von Zoom- oder Bildlaufvorgängen bezeichnet.

Die Verkettung wird für die Verschiebung in einem Inhaltsbereich mit einer Achse, der mindestens einen Bereich für die Verschiebung entlang einer Achse oder die formfreie Verschiebung enthält (sofern die Berührung in einem dieser untergeordneten Bereiche erfolgt). Wenn die Grenze des untergeordneten Bereichs für die Verschiebung in einer bestimmten Richtung erreicht wird, wird die Verschiebung in derselben Richtung für den übergeordneten Bereich aktiviert.

Wenn ein verschiebbarer Bereich in einen anderen verschiebbaren Bereich geschachtelt ist, ist es wichtig, ausreichend Platz zwischen dem Container und dem eingebetteten Inhalt zu lassen. In den folgenden Diagrammen befindet sich ein verschiebbarer Bereich in einem anderen verschiebbaren Bereich. Die Bereiche können jeweils im rechten Winkel zueinander verschoben werden. In jedem Bereich ist ausreichend Platz für die Verschiebung vorhanden.

![Abbildung eines eingebetteten verschiebbaren Bereichs](images/scrolling-embedded.png)

Wenn wie im folgenden Diagramm nicht genügend Platz vorhanden ist, kann der eingebettete verschiebbare Bereich die Verschiebung im Container stören, was dazu führen kann, dass in einem oder mehreren der verschiebbaren Bereiche eine unbeabsichtigte Verschiebung erfolgt.

![Abbildung eines eingebetteten verschiebbaren Bereichs mit unzureichendem Abstand](images/ux-panning-embedded-wrong.png)

Dieser Leitfaden ist auch für Apps wie Fotoalben oder Karten-Apps hilfreich, die uneingeschränkte Verschiebung in einzelnen Bildern oder Karten und gleichzeitig die Verschiebung entlang einer Achse im Album (zum vorherigen oder nächsten Bild) bzw. im Detailbereich unterstützen. In Apps, die über einen Detail- oder Optionsbereich für das formfreie Verschieben eines Bilds oder einer Karte verfügen, empfehlen wir, das Seitenlayout mit den Detail- und Optionsbereichen zu beginnen, da der Bereich zum ungehinderten Verschieben eines Bilds oder einer Karte das Verschieben in den Detailbereich stören kann.

## <span id="related_topics"></span>Verwandte Artikel


* [Benutzerdefinierte Benutzerinteraktionen](https://msdn.microsoft.com/library/windows/apps/mt185599)
* [Optimieren von ListView und GridView](https://msdn.microsoft.com/library/windows/apps/mt204776)
* [Barrierefreiheit der Tastaturnavigation](https://msdn.microsoft.com/library/windows/apps/mt244347)

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
* [Eingabe: Beispiel für Windows 8-Bewegungen](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Eingabe: Beispiel für Manipulationen und Gesten (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Beispiel für die DirectX-Fingereingabe](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 







<!--HONumber=Jun16_HO3-->


