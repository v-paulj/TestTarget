---
Description: Description: Gestalten Sie die Benutzerinteraktion mit Ihren Apps für die universelle Windows-Plattform (UWP) intuitiv und unverwechselbar, und optimieren Sie sie für Touchpads. Dabei sollte die Funktionalität jedoch für alle Eingabegeräte einheitlich sein.
title: title: Touchpad-Interaktionen
ms.assetid: CEDEA30A-FE94-4553-A7FB-6C1FA44F06AB
label: Touchpad interactions
template: detail.hbs
---

# ms.assetid: CEDEA30A-FE94-4553-A7FB-6C1FA44F06AB


label: Touchpad-Interaktionen template: detail.hbs

Touchpad-Designrichtlinien \[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

 

![Gestalten Sie Ihre App so, dass Benutzer über ein Touchpad mit ihr interagieren können.](images/input-patterns/input-touchpad.jpg)


Ein Touchpad vereint die indirekte Multitoucheingabe mit der Präzisionseingabe eines Zeigergeräts (beispielsweise eine Maus).

-   Dadurch ist das Touchpad sowohl für eine toucheingabeoptimierte Benutzeroberfläche als auch die kleineren Ziele von Produktivitäts-Apps geeignet.

    Touchpad Interaktionen per Touchpad erfordern drei Dinge:

-   Ein standardmäßiges Touchpad oder ein Windows-Präzisionstouchpad.
-   Präzisionstouchpads sind für UWP-Geräte (Universelle Windows-Plattform) optimiert.

Sie ermöglichen es dem System, bestimmte Aspekte der Touchpadverwendung (etwa die Fingerverfolgung und Handflächenerkennung) nativ zu behandeln, um geräteübergreifend eine konsistentere Umgebung zu bieten.

-   Der direkte Kontakt einzelner oder mehrerer Finger auf dem Touchpad. Bewegung der Berührungskontakte (oder das Fehlen der Bewegung basierend auf einem Zeitschwellenwert).
-   Mögliche Eingabedaten des Touchpadsensors:
-   Interpretation als physische Geste für die direkte Manipulation von einem oder mehreren UI-Elementen (z. B. Schwenken, Drehen, Vergrößern/Verkleinern oder Verschieben).

Die Interaktion mit einem Element über das zugehörige Eigenschaftenfenster oder ein anderes Dialogfeld gilt dagegen als indirekte Manipulation. Erkennung als alternative Eingabemethode, z. B. Maus oder Stift. Wird zum Ergänzen oder Ändern von Aspekten anderer Eingabemethoden verwendet, z. B. zum Verwischen eines mit einem Stift gezeichneten Freihandstrichs.

Ein Touchpad vereint die indirekte Multitoucheingabe mit der Präzisionseingabe eines Zeigergeräts (etwa eine Maus). Dadurch ist das Touchpad sowohl für die berührungsoptimierte Benutzeroberfläche als auch die kleineren Ziele der Produktivitäts-Apps und der Desktopumgebung geeignet.

Optimieren Sie das Design Ihrer Windows Store-Apps für die Toucheingabe, und profitieren Sie von der standardmäßigen Touchpad-Unterstützung.

## Aufgrund der Konvergenz der Interaktionsformen, die von Touchpads unterstützt werden, empfehlen wir die Verwendung des [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)-Ereignisses, um zusätzlich zur integrierten Unterstützung für die Toucheingabe Benutzeroberflächenbefehle für die Mauseingabe bereitzustellen.


Verwenden Sie beispielsweise Zurück- und Weiter-Schaltflächen, mit denen Benutzer sowohl Inhaltsseiten durchblättern als auch Inhalte verschieben können. Die in diesem Thema beschriebenen Gesten und Richtlinien können dabei helfen, die Unterstützung der Touchpadeingabe nahtlos und mit minimalem Programmieraufwand in Ihre App zu integrieren.

<span id="The_touchpad_language"></span><span id="the_touchpad_language"></span><span id="THE_TOUCHPAD_LANGUAGE"></span>Sprache für Eingabe per Touchpad Ein kompakter Satz von Touchpadinteraktionen wird durchgängig im ganzen System verwendet.

![Indem Sie Ihre App für die Touch- und Mauseingabe optimieren, sorgen Sie dafür, dass sich Benutzer sofort in Ihrer App zurechtfinden. So erleichtern Sie Benutzern den Einstieg und die Verwendung Ihrer App.](images/mouse-touchpad-settings-standard.png)

Benutzer können viel mehr Gesten und Interaktionsverhalten für Präzisionstouchpads festlegen als für ein standardmäßiges Touchpad.

![Diese beiden Bilder zeigen die verschiedenen Touchpad-Einstellungsseiten (unter „Einstellungen“ &gt; „Geräte“ &gt; „Maus und Touchpad“) für ein standardmäßiges Touchpad und ein Präzisionstouchpad.](images/mouse-touchpad-settings-ptp.png)

Einstellungen für ein standardmäßiges Touchpad

<sup>Standard\\ Touchpad\\ Einstellungen</sup>

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Einstellungen für ein Windows-Präzisionstouchpad</th>
<th align="left"><sup>Windows\\ Präzision\\ Touchpad\\ Einstellungen</sup></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Three-finger_tap"></span><span id="three-finger_tap"></span><span id="THREE-FINGER_TAP"></span>Im Anschluss folgen einige Beispiele für touchpadoptimierte Gesten zum Ausführen allgemeiner Aufgaben.</p></td>
<td align="left"><p>Begriff</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Three_finger_slide"></span><span id="three_finger_slide"></span><span id="THREE_FINGER_SLIDE"></span>Beschreibung</p></td>
<td align="left"><p>Tippen mit drei Fingern</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="Single_finger_tap_for_primary_action"></span><span id="single_finger_tap_for_primary_action"></span><span id="SINGLE_FINGER_TAP_FOR_PRIMARY_ACTION"></span>Benutzereinstellung für die Suche mit <strong>Cortana</strong> oder die Anzeige des Info-Centers<strong></strong>.</p></td>
<td align="left"><p>Ziehbewegung mit drei Fingern</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Two_finger_tap_to_right-click"></span><span id="two_finger_tap_to_right-click"></span><span id="TWO_FINGER_TAP_TO_RIGHT-CLICK"></span>Benutzereinstellung zum Öffnen der Aufgabenansicht des virtuellen Desktops, zum Anzeigen des Desktops oder zum Wechseln zwischen geöffneten Apps.</p></td>
<td align="left"><p>Tippen mit einem Finger: Aufrufen der primären Aktion</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="Two_finger_slide_to_pan"></span><span id="two_finger_slide_to_pan"></span><span id="TWO_FINGER_SLIDE_TO_PAN"></span>Durch Tippen mit einem Finger auf ein Element wird dessen primäre Aktion aufgerufen (z. B. das Starten einer App oder das Ausführen eines Befehls).</p></td>
<td align="left"><p>Tippen mit zwei Fingern: Ausführen eines Rechtsklicks</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Pinch_and_stretch_to_zoom"></span><span id="pinch_and_stretch_to_zoom"></span><span id="PINCH_AND_STRETCH_TO_ZOOM"></span>Tippen Sie mit zwei Fingern gleichzeitig auf ein Element, um es auszuwählen und Kontextbefehle anzuzeigen.</p></td>
<td align="left"><p>Ziehen mit zwei Fingern: Verschieben</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="Single_finger_press_and_slide_to_rearrange"></span><span id="single_finger_press_and_slide_to_rearrange"></span><span id="SINGLE_FINGER_PRESS_AND_SLIDE_TO_REARRANGE"></span>Das Ziehen wird in erster Linie für Verschiebungen verwendet, kann jedoch auch zum Schieben, Zeichnen oder Schreiben genutzt werden.</p></td>
<td align="left"><p>Zusammendrücken/Aufziehen: Zoom</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Single_finger_press_and_slide_to_select_text"></span><span id="single_finger_press_and_slide_to_select_text"></span><span id="SINGLE_FINGER_PRESS_AND_SLIDE_TO_SELECT_TEXT"></span>Die Zusammendrück- und Aufziehbewegungen werden üblicherweise für Größenänderungen und zum Ausführen des semantischen Zooms verwendet.</p></td>
<td align="left"><p>Drücken und Ziehen mit einem Finger: Neuanordnen Ziehen eines Elements.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="Left_and_right_click_zone"></span><span id="left_and_right_click_zone"></span><span id="LEFT_AND_RIGHT_CLICK_ZONE"></span>Drücken und Ziehen mit einem Finger: Textauswahl</p></td>
<td align="left"><p>Durch Drücken und Ziehen in auswählbarem Text wird Text ausgewählt.</p></td>
</tr>
</tbody>
</table>

 

## Durch Doppeltippen wird ein einzelnes Wort ausgewählt.


Zonen für Links- und Rechtsklick Mit diesen Zonen emulieren Sie die Links- und Rechtsklickfunktion einer Maus.

<span id="Hardware"></span><span id="hardware"></span><span id="HARDWARE"></span>Hardware

## Fragen Sie die Funktionen des Mausgeräts ([**MouseCapabilities**](https://msdn.microsoft.com/library/windows/apps/br225626)) ab, um zu ermitteln, auf welche Elemente der Benutzeroberfläche Ihrer App die Touchpad-Hardware direkt zugreifen kann.


-   Wir empfehlen die Bereitstellung einer Benutzeroberfläche, die sowohl Touch- als auch Mauseingabe ermöglicht. Weitere Informationen zum Abfragen von Gerätefunktionen finden Sie unter [Identifizieren von Eingabegeräten](identify-input-devices.md). <span id="Visual_feedback"></span><span id="visual_feedback"></span><span id="VISUAL_FEEDBACK"></span>Visuelles Feedback
-   Blenden Sie die für Touchpadinteraktionen spezifische Benutzeroberfläche ein, sobald ein Touchpad-Cursor erkannt wird (durch Bewegungs- oder Zeigeereignisse), um die Funktionalität des Elements verfügbar zu machen.
-   Wenn der Touchpad-Cursor für eine bestimmte Zeit nicht bewegt wird oder der Benutzer eine Toucheingabeinteraktion auslöst, blenden Sie die für Touchpad-Interaktionen spezifische Benutzeroberfläche schrittweise aus.
-   Somit bleibt die Benutzeroberfläche sauber und aufgeräumt. Verwenden Sie nicht den Cursor für Zeigefeedback, das Feedback des Elements reicht aus (siehe [Cursor](#Cursors) unten).
-   Lassen Sie kein visuelles Feedback anzeigen, wenn ein Element keine Interaktionen unterstützt (z. B. statischer Text).

Verwenden Sie keine Fokusrechtecke für Interaktionen per Touchpad.

## Diese sind ausschließlich für Tastaturinteraktionen vorgesehen.


Zeigen Sie für alle Elemente, die das gleiche Eingabeziel darstellen, das gleiche visuelle Feedback an. Allgemeine Informationen zum visuellen Feedback finden Sie unter [Richtlinien für visuelles Feedback](https://msdn.microsoft.com/library/windows/apps/hh465342).

<span id="Cursors"></span><span id="cursors"></span><span id="CURSORS"></span>Cursor In Windows Store-Apps sind einige Standardcursor verfügbar, die als Touchpad-Zeiger verwendet werden können. Diese Cursor werden verwendet, um die primäre Aktion eines Elements anzugeben.

Jedem Standardcursor ist ein entsprechendes Standardbild zugewiesen.

-   Benutzer einer App können das einem Standardcursor zugewiesene Standardbild jederzeit ändern.![In Windows Store-Apps werden Cursorbilder durch die [**PointerCursor**](https://msdn.microsoft.com/library/windows/apps/br208273)-Funktion angegeben.](images/cursor-arrow.png)Beachten Sie beim Anpassen des Mauszeigers Folgendes: Verwenden Sie immer den Pfeilcursor (![Pfeilcursor](images/cursor-pointinghand.png)) für klickbare Elemente. Verwenden Sie den Zeigefingercursor (
-   Zeigefingercursor![) nicht für Links oder andere interaktive Elemente.](images/cursor-text.png)Verwenden Sie stattdessen Zeigeeffekte (wie bereits beschrieben).
-   Verwenden Sie den Textcursor (![Textcursor](images/cursor-move.png)) für auswählbaren Text. Verwenden Sie den Bewegungscursor (
-   Bewegungscursor![), wenn die primäre Aktion eine Bewegung ist (etwa beim Ziehen oder Zuschneiden).](images/cursor-vertical.png)Verwenden Sie den Bewegungscursor nicht, wenn die primäre Aktion eine Navigationsaktion ist (etwa bei Start-Kacheln). ![Verwenden Sie die Cursor für horizontale, vertikale und diagonale Größenänderung (](images/cursor-horizontal.png)Cursor für vertikale Größenänderung ![,](images/cursor-diagonal2.png)Cursor für horizontale Größenänderung ![,](images/cursor-diagonal1.png)Cursor für diagonale Größenänderung (unten links, oben rechts)
-   ,![Cursor für diagonale Größenänderung (oben links, unten rechts)](images/cursor-pan1.png)), wenn die Größe eines Objekts geändert werden kann. ![Verwenden Sie den Handcursor (](images/cursor-pan2.png)Handcursor (offen)

## ,


* [Handcursor (geschlossen)](handle-pointer-input.md)
* [) beim Verschieben von Inhalt innerhalb einer Canvas (etwa bei einer Karte).](identify-input-devices.md)
**<span id="related_topics"></span>Verwandte Artikel**
* [Behandeln von Zeigereingaben](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [[Identifizieren von Eingabegeräten](identify-input-devices.md)](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [**Beispiele**](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**Beispiel für Eingabe mit niedriger Latenz**
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [[Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [**Archivbeispiele**](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 





<!--HONumber=Apr16_HO3-->


