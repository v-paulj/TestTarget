---
Description: Description: Reagieren Sie in Ihren Apps auf Mauseingaben, indem Sie die gleichen einfachen Zeigerereignisse behandeln, die Sie für Touch- und Stifteingaben verwenden.
title: title: Mausinteraktionen
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
---

# ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC


label: Maus template: detail.hbs

Mausinteraktionen

 

![\[ Aktualisiert für UWP-Apps unter Windows 10.](images/input-patterns/input-mouse.jpg)



Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \] Optimieren Sie das Design Ihrer UWP-Apps für die Toucheingabe, und freuen Sie sich über die standardmäßige allgemeine Unterstützung von Mausgeräten.

Maus Die Mauseingabe eignet sich am besten für Benutzerinteraktionen, die exaktes Zeigen und Klicken erfordern.

Die Benutzeroberfläche von Windows unterstützt selbstverständlich diese Genauigkeit, ist aber zugleich für die ungenauere Toucheingabe optimiert.

## Die Maus- und Toucheingabe unterscheiden sich dahingehend, dass bei der Toucheingabe die direkte Manipulation von UI-Elementen auf dem Bildschirm durch physische Gesten für diese Objekte (z. B. Streifen, Ziehen, Drehen usw.) emuliert werden kann.


Manipulationen mit der Maus erfordern in der Regel einigen UI-Aufwand, wie z. B. die Verwendung von Handles für das Anpassen der Größe oder Drehen eines Objekts.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">In diesem Thema werden Designüberlegungen für Mausinteraktionen behandelt.</th>
<th align="left"><span id="The_UWP_app_mouse_language"></span><span id="the_uwp_app_mouse_language"></span><span id="THE_UWP_APP_MOUSE_LANGUAGE"></span>Die UWP-App-Sprache für Mauseingaben</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Hover_to_learn"></span><span id="hover_to_learn"></span><span id="HOVER_TO_LEARN"></span>Ein kompakter Satz von Mausinteraktionen wird durchgängig im ganzen System verwendet.</p></td>
<td align="left"><p>Benennung</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Left-click_for_primary_action"></span><span id="left-click_for_primary_action"></span><span id="LEFT-CLICK_FOR_PRIMARY_ACTION"></span>Beschreibung</p></td>
<td align="left"><p>Lernen durch Zeigen</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="Scroll_to_change_view"></span><span id="scroll_to_change_view"></span><span id="SCROLL_TO_CHANGE_VIEW"></span>Zeigen Sie auf ein Element, um weitere Informationen oder visuelle Hinweise (wie etwa QuickInfos) aufzurufen, ohne eine Aktion auszuführen.</p></td>
<td align="left"><p>Linksklick, um primäre Aktion auszuführen Klicken Sie mit der linken Maustaste auf ein Element, um dessen primäre Aktion aufzurufen (z. B. das Starten einer App oder das Ausführen eines Befehls). Bildlauf, um Ansicht zu ändern</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Right-click_to_select_and_command"></span><span id="right-click_to_select_and_command"></span><span id="RIGHT-CLICK_TO_SELECT_AND_COMMAND"></span>Zeigen Sie Bildlaufleisten an, damit Benutzer in einem Inhaltsbereich nach oben, unten, links und rechts navigieren können.</p></td>
<td align="left"><p>Benutzer können durch Klicken auf Bildlaufleisten oder Drehen des Mausrads einen Bildlauf durchführen. Auf Bildlaufleisten kann die Position der aktuellen Ansicht innerhalb des Inhaltsbereichs angezeigt werden (durch das Schwenken bei der Fingereingabe wird eine ähnliche Benutzeroberfläche angezeigt).</p>
<div class="alert">Rechtsklick, um Auswahl zu treffen und Befehl auszuwählen Klicken Sie mit der rechten Maustaste, um die Navigationsleiste (sofern verfügbar) und die App-Leiste mit globalen Befehlen anzuzeigen.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="UI_commands_to_zoom"></span><span id="ui_commands_to_zoom"></span><span id="UI_COMMANDS_TO_ZOOM"></span>Klicken Sie mit der rechten Maustaste auf ein Element, um es auszuwählen und die App-Leiste mit Kontextbefehlen für das ausgewählte Element anzuzeigen.</p></td>
<td align="left"><p>
<strong>Hinweis</strong> Klicken Sie mit der rechten Maustaste, um ein Kontextmenü anzuzeigen, wenn die in der Auswahl oder der App-Leiste verfügbaren Befehle nicht das gewünschte Benutzeroberflächenverhalten ermöglichen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="UI_commands_to_rotate"></span><span id="ui_commands_to_rotate"></span><span id="UI_COMMANDS_TO_ROTATE"></span>Wir empfehlen jedoch ausdrücklich, die App-Leiste für alle Befehlsverhalten zu verwenden.</p></td>
<td align="left"><p>Benutzeroberflächenbefehle zum Zoomen Zeigen Sie Benutzeroberflächenbefehle auf der App-Leiste an (z. B. "+" und "-"), oder drücken Sie STRG und drehen Sie das Mausrad, um Zusammendrück- und Aufziehbewegungen zum Zoomen zu emulieren.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="Left-click_and_drag_to_rearrange"></span><span id="left-click_and_drag_to_rearrange"></span><span id="LEFT-CLICK_AND_DRAG_TO_REARRANGE"></span>Benutzeroberflächenbefehle zum Drehen</p></td>
<td align="left"><p>Zeigen Sie Benutzeroberflächenbefehle auf der App-Leiste an, oder drücken Sie STRG+UMSCHALTTASTE, und drehen Sie das Mausrad, um die Drehbewegung zum Drehen zu emulieren.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Left-click_and_drag_to_select_text"></span><span id="left-click_and_drag_to_select_text"></span><span id="LEFT-CLICK_AND_DRAG_TO_SELECT_TEXT"></span>Drehen Sie das Gerät selbst, um den ganzen Bildschirm zu drehen.</p></td>
<td align="left"><p>Linksklick und ziehen, um neu anzuordnen Klicken Sie mit der linken Maustaste auf ein Element, und ziehen Sie, um es zu bewegen.</p></td>
</tr>
</tbody>
</table>

## Linksklick und ziehen, um Text auszuwählen

Klicken Sie mit der linken Maustaste auf auswählbaren Text, und ziehen Sie, um Text auszuwählen.

Doppelklicken Sie, um ein Wort auszuwählen. Mausereignisse

Reagieren Sie in Ihren Apps auf Mauseingaben, indem Sie die gleichen einfachen Zeigerereignisse behandeln, die Sie für Touch- und Stifteingaben verwenden.


- [Verwenden Sie [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)-Ereignisse, um grundlegende Eingabefunktionen zu implementieren, ohne separaten Code für jedes Zeigereingabegerät schreiben zu müssen.](http://go.microsoft.com/fwlink/p/?linkid=231530)

- [Sie können aber trotzdem die speziellen Funktionen jedes Geräts nutzen (z. B. die Ereignisse des Mausrads), indem Sie die Zeiger-, Gestik- und Manipulationsereignisse dieses Objekts verwenden.](http://go.microsoft.com/fwlink/p/?linkid=226855)

- [**Beispiele: **Sehen Sie sich unter [Beispiele für Apps](http://go.microsoft.com/fwlink/p/?LinkID=264996) diese Funktionalität in Aktion an.](http://go.microsoft.com/fwlink/p/?LinkID=231605)

## Eingabe: Beispiel für Gerätefunktionen


-   Eingabebeispiel Eingabe: Gesten und Manipulationen mit GestureRecognizer <span id="Guidelines_for_visual_feedback"></span><span id="guidelines_for_visual_feedback"></span><span id="GUIDELINES_FOR_VISUAL_FEEDBACK"></span>Richtlinien für visuelles Feedback
-   Wenn eine Maus erkannt wird (durch Bewegungs- oder Daraufzeigen-Ereignisse), zeigen Sie eine für Mausinteraktionen spezifische Benutzeroberfläche an, um auf vom Element verfügbar gemachte Funktionen hinzuweisen.
-   Wenn die Maus für eine bestimmte Zeit nicht bewegt wird oder der Benutzer eine Fingereingabeinteraktion auslöst, blenden Sie die für Mausinteraktionen spezifische Benutzeroberfläche schrittweise aus.
-   Somit bleibt die Benutzeroberfläche sauber und aufgeräumt. Verwenden Sie nicht den Cursor für Zeigefeedback, das Feedback des Elements reicht aus (siehe Cursor unten).
-   Zeigen Sie kein visuelles Feedback an, wenn ein Element keine Interaktionen unterstützt (z. B. statischer Text).
-   Verwenden Sie keine Fokusrechtecke für Mausinteraktionen.

Diese sind ausschließlich für Tastaturinteraktionen vorgesehen.


## Zeigen Sie für alle Elemente, die das gleiche Eingabeziel darstellen, das gleiche visuelle Feedback an.


Stellen Sie Schaltflächen (z. B. „+“ und „-“) zur Verfügung, um fingereingabebasierte Manipulationen wie etwa Schwenken, Drehen, Zoomen usw. zu emulieren. Allgemeine Informationen zum visuellen Feedback finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).

<span id="Cursors"></span><span id="cursors"></span><span id="CURSORS"></span>Cursor Für einen Mauszeiger ist eine Reihe von Standardcursor verfügbar. Diese geben die primäre Aktion eines Elements an.

Jedem Standardcursor ist ein entsprechendes Standardbild zugewiesen.

-   Benutzer einer App können das Standardbild, das einem Standardcursor zugewiesen ist, jederzeit ändern.![Geben Sie über die [**PointerCursor**](https://msdn.microsoft.com/library/windows/apps/br208273)-Funktion die Abbildung eines Cursors an.](images/cursor-arrow.png)Wenn Sie den Mauszeiger anpassen müssen: Verwenden Sie immer den Pfeilcursor (![Pfeilcursor](images/cursor-pointinghand.png)) für klickbare Elemente. Verwenden Sie den Zeigefingercursor (
-   Zeigefingercursor![) nicht für Links oder andere interaktive Elemente.](images/cursor-text.png)Verwenden Sie stattdessen Zeigeeffekte (wie bereits beschrieben).
-   Verwenden Sie den Textcursor (![Textcursor](images/cursor-move.png)) für auswählbaren Text. Verwenden Sie den Bewegungscursor (
-   Bewegungscursor![), wenn die primäre Aktion eine Bewegung ist (wie etwa beim Ziehen oder Zuschneiden).](images/cursor-vertical.png)Verwenden Sie den Bewegungscursor nicht, wenn die primäre Aktion eine Navigation ist (wie etwa bei Start-Kacheln). ![Verwenden Sie die Cursor für horizontale, vertikale und diagonale Größenänderung (](images/cursor-horizontal.png)Cursor für vertikale Größenänderung ![,](images/cursor-diagonal2.png)Cursor für horizontale Größenänderung ![,](images/cursor-diagonal1.png)Cursor für diagonale Größenänderung (unten links, oben rechts)
-   ,![Cursor für diagonale Größenänderung (oben links, unten rechts)](images/cursor-pan1.png)), wenn die Größe eines Objekts geändert werden kann. ![Verwenden Sie das Handwerkzeug (](images/cursor-pan2.png)Handwerkzeug (offen)

## ,

* [Handwerkzeug (geschlossen)](handle-pointer-input.md)
* [) beim Verschieben von Inhalt innerhalb einer Canvas (etwa einer Karte).](identify-input-devices.md)

**<span id="related_topics"></span>Verwandte Artikel**
* [Behandeln von Zeigereingaben](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Identifizieren von Eingabegeräten](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiele](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Beispiel für Eingabe mit niedriger Latenz**
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Archivbeispiele](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 
 

 






<!--HONumber=Apr16_HO3-->


