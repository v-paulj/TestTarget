---
author: Jwmsft
Description: "Listen zeigen und ermöglichen die Interaktion mit sammlungsbasierten Inhalten."
title: Listen
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Lists
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 96fd7c2af74ec609a6cfbb41a14b6f4086747813

---
# Listen

Listen zeigen und ermöglichen die Interaktion mit sammlungsbasierten Inhalten. Zu den in diesem Artikel behandelten vier Listenmustern gehören:

-   Listenansichten, die in erster Linie zum Anzeigen von textlastigen Inhaltssammlungen verwendet werden
-   Rasteransichten, die in erster Linie zum Anzeigen von bildlastigen Inhaltssammlungen verwendet werden
-   Dropdownlisten, aus denen Benutzer ein Element aus einer erweiterten Liste auswählen können
-   Listenfelder, in denen Benutzer ein einzelnes Element oder mehrere Elemente aus einem Feld auswählen können, in dem gescrollt werden kann

Für jedes Listenmuster sind Entwurfsrichtlinien, Features und Beispiele aufgeführt. Am Ende des Artikels befinden sich Links zu verwandten Themen und APIs.

## Wichtige APIs

-   [**ListView-Klasse **](https://msdn.microsoft.com/library/windows/apps/br242878)
-   [**GridView-Klasse **](https://msdn.microsoft.com/library/windows/apps/br242705)
-   [**ComboBox-Klasse **](https://msdn.microsoft.com/library/windows/apps/br209348)


## Listenansichten

Mit Listenansichten können Sie Elemente kategorisieren und Gruppenüberschriften zuweisen, Elemente per Drag & Drop verschieben, Inhalt überprüfen und Elemente neu anordnen.

### Ist dies das richtige Steuerelement?

Mit einer Listenansicht können Sie:

-   Inhaltssammlungen anzeigen, die in erster Linie aus Text bestehen.
-   Durch eine einzelne oder kategorisierte Inhaltssammlung navigieren.
-   Den Masterbereich mit dem [Master-/Detailmuster](master-details.md) erstellen. Ein Master-/Detailmuster wird häufig in E-Mail-Apps verwendet, in denen ein Bereich (der Masterbereich) eine Liste auswählbarer Elemente enthält, während im anderen eine detaillierte Ansicht des ausgewählten Elements enthalten ist.

### Beispiele

Bei der Verwendung eines [Master-/Detailmusters](master-details.md) können Sie den Masterbereich mit einer Listenansicht organisieren. Der Masterbereich zeigt eine Liste mit auswählbaren Elementen an. Wenn der Benutzer ein Element im Masterbereich auswählt, werden zusätzliche Informationen zum gewählten Element im Detailbereich angezeigt. Der Detailbereich enthält häufig eine Rasteransicht.

![Beispiel für das Master/Details-Muster](images/Stock_Tracker/uap_finance_desktop700.png)

Sie können mehrere Listen miteinander verketten, um komplexe Master-/Detailhierarchien zu erstellen. Weitere Informationen finden Sie unter [Master-/Detailmuster](master-details.md).

Das Beispiel für ein Listenlayout enthält Gruppenheader und wird als einzelne Spalte angezeigt:

![Beispiel für eine Listenansicht mit vier Haupteinheitentypen](images/controls_listview_4types.png)

### Empfehlungen

-   Elemente in einer Liste sollten das gleiche Verhalten aufweisen.
-   Wenn Ihre Liste in Gruppen unterteilt ist, verwenden Sie den [semantischen Zoom](semantic-zoom.md), mit dem Benutzern die Navigation in gruppierten Inhalten erleichtert wird.

## Rasteransichten

Rasteransichten eignen sich zum Anordnen und Durchsuchen bildbasierter Inhaltssammlungen. Ein Rasteransichtslayout wird vertikal gescrollt und horizontal bewegt. Elemente werden von links nach rechts und anschließend von oben nach unten in Leserichtung angeordnet.

### Ist dies das richtige Steuerelement?

Mit einer Listenansicht können Sie:

-   Eine Inhaltssammlung anzeigen, die in erster Linie aus Bildern besteht.
-   Inhaltsbibliotheken anzeigen.
-   Die zwei Inhaltsansichten formatieren, die dem [semantischen Zoom](semantic-zoom.md) zugeordnet sind.

### Beispiele

Dieses Beispiel zeigt ein typisches Rasteransichtslayout, in diesem Fall zum Durchsuchen von Apps. Metadaten für Rasteransichtselemente sind in der Regel auf wenige Textzeilen und eine Bewertung des Elements beschränkt.

![Beispiel eines Rasteransichtslayouts](images/controls_gridview_example02.png)

Eine Rasteransicht eignet sich ideal für eine Inhaltsbibliothek, die häufig verwendet wird, um Medien wie Bilder und Videos darzustellen. In einer Inhaltsbibliothek erwarten Benutzer, dass sie auf ein Element tippen können, um eine Aktion aufzurufen.

![Beispiel einer Inhaltsbibliothek](images/controls_list_contentlibrary.png)

### Empfehlungen

-   Elemente in einer Liste sollten das gleiche Verhalten aufweisen.
-   Wenn Ihre Liste in Gruppen unterteilt ist, verwenden Sie den [semantischen Zoom](semantic-zoom.md), mit dem Benutzern die Navigation in gruppierten Inhalten erleichtert wird.

## Dropdownlisten

Dropdownlisten, auch als Kombinationsfelder bezeichnet, werden in einem kompakten Zustand gestartet und erweitert, um eine Liste mit auswählbaren Elementen anzuzeigen. Eine Dropdownliste unterstützt die Einzelauswahl oder die Mehrfachauswahl. Das ausgewählte Element ist immer sichtbar, und nicht sichtbare Elemente können eingeblendet werden, wenn der Benutzer auf das ausgewählte Element tippt.

### Ist dies das richtige Steuerelement?

-   Mit einer Dropdownliste können Benutzer einen einzelnen Wert aus einer Reihe von Elementen auswählen, die mit einzelnen Textzeilen angemessen dargestellt werden können.
-   Verwenden Sie eine Liste oder eine Rasteransicht anstelle einer Dropdownliste, um Elemente anzuzeigen, die mehrere Textzeilen oder Bilder enthalten.
-   Wenn weniger als fünf Elemente vorhanden sind, können Sie stattdessen die Verwendung von [Optionsfeldern](radio-button.md) (wenn nur ein Element ausgewählt werden kann) oder [Kontrollkästchen](checkbox.md) (wenn mehrere Elemente ausgewählt werden können) in Betracht ziehen.
-   Verwenden Sie eine Dropdownliste, wenn die Auswahlelemente für den Fluss der App von sekundärer Bedeutung sind. Wenn die Standardoption für die meisten Benutzer in den meisten Situationen empfohlen wird, kann die Anzeige aller Elemente in einem Listenfeld mehr Aufmerksamkeit auf die Optionen ziehen als nötig. Sie können Platz sparen und Ablenkungen reduzieren, indem Sie eine Dropdownliste verwenden.

### Beispiele

Eine Dropdownliste in ihrem kompakten Zustand kann einen Header anzeigen.

![Beispiel für eine Dropdownliste in ihrem kompakten Zustand](images/combo_box_collapsed.png)

Obwohl Dropdownlisten erweitert werden, um längere Zeichenfolgen zu unterstützen, sollten Sie extrem lange Zeichenfolgen vermeiden, die nur schwer lesbar sind.

![Beispiel einer Dropdownliste mit einer langen Textzeichenfolge](images/combo_box_listitemstate.png)

Wenn die Sammlung in einer Dropdownliste lang genug ist, wird dafür eine Scrollleiste angezeigt. Gruppieren Sie die Elemente in der Liste logisch.

![Beispiel einer Scrollleiste in einer Dropdownliste](images/combo_box_scroll.png)

### Empfehlungen

-   Beschränken Sie den Text von Dropdownlistenelementen auf eine einzelne Zeile.
-   Sortieren Sie die Elemente in einer Dropdownliste in der logischsten Reihenfolge. Gruppieren Sie verwandte Optionen, platzieren Sie die am häufigsten verwendeten Optionen oben, und sortieren Sie Elemente alphabetisch. Sortieren Sie Namen in alphabetischer Reihenfolge, Nummern in numerischer Reihenfolge und Datumsangaben in chronologischer Reihenfolge.

## Listenfelder

Mit einem Listenfeld kann der Benutzer ein einzelnes Element oder mehrere Elemente aus einer Auflistung auswählen. Listenfelder ähneln Dropdownlisten, abgesehen davon, dass Listenfelder immer geöffnet sind; für ein Listenfeld gibt es keinen kompakten Zustand (nicht erweitert). Elemente in einem Listenfeld können gescrollt werden, wenn der Platz nicht ausreicht, um alle Elemente anzuzeigen.

### Ist dies das richtige Steuerelement?

-   Ein Listenfeld kann nützlich sein, wenn Elemente in der Liste so relevant sind, dass sie auffälliger dargestellt werden sollten, und genügend Platz auf dem Bildschirm zum Anzeigen der vollständigen Liste vorhanden ist.
-   Ein Listenfeld sollte den Benutzer bei einer wichtigen Entscheidung bzw. Auswahl auf alle Alternativen aufmerksam machen. Im Gegensatz dazu lenkt eine Dropdownliste die Aufmerksamkeit des Benutzers zunächst auf das ausgewählte Element.
-   Vermeiden Sie die Verwendung eines Listenfelds in folgenden Fällen:
    -   Es gibt eine sehr kleine Anzahl von Elementen für die Liste. Ein Einzelauswahl-Listenfeld, das immer zwei identische Optionen enthält, sollte besser in Form von [Optionsfeldern](radio-button.md) dargestellt werden. Ziehen Sie die Verwendung von Optionsfeldern ebenfalls in Erwägung, wenn die Liste drei oder vier statische Elemente enthält.
    -   Das Listenfeld ist eine Einzelauswahl und enthält immer die gleichen zwei Optionen, wobei eine als „nicht die andere Option“ impliziert werden kann (beispielsweise „Ein“ und „Aus“). Verwenden Sie ein einzelnes Kontrollkästchen oder einen Umschalter.
    -   Die Anzahl von Elementen ist sehr hoch. Bessere Optionen für lange Listen sind Rasteransicht und Listenansicht. Für sehr lange Listen mit gruppierten Daten wir der semantische Zoom bevorzugt.
    -   Bei den Elementen handelt es sich um zusammenhängende numerische Werte. Wenn dies der Fall ist, sollten Sie einen [Schieberegler](slider.md) in Erwägung ziehen.
    -   Die Auswahlelemente sind im Fluss Ihrer App von sekundärer Bedeutung, oder für die meisten Benutzer wird in den meisten Situationen die Standardoption empfohlen. Verwenden Sie stattdessen eine Dropdownliste.

### Empfehlungen

-   Der ideale Bereich von Elementen in einem Listenfeld beträgt 3 bis 9.
-   Ein Listenfeld funktioniert gut, wenn die Elemente darin dynamisch variieren können.
-   Legen Sie die Größe eines Listenfelds möglichst so fest, dass für die Liste der zugehörigen Elemente keine Verschiebung und kein Scrollen erforderlich sind.
-   Stellen Sie sicher, dass der Zweck des Listenfelds und die momentan ausgewählten Elemente klar sind.
-   Reservieren Sie visuelle Effekte und Animationen für das Feedback für die Fingereingabe und für den ausgewählten Zustand von Elementen.
-   Beschränken Sie den Text von Listenfeldelementen auf eine einzelne Zeile. Wenn es sich bei den Elementen um visuelle Objekte handelt, können Sie die Größe anpassen. Wenn ein Element mehrere Textzeilen oder Bilder enthält, verwenden Sie stattdessen eine Raster- oder Listenansicht.
-   Verwenden Sie die standardmäßige Schriftart, sofern Sie gemäß Ihren Markenrichtlinien keine andere verwenden müssen.
-   Verwenden Sie ein Listenfeld nicht zum Ausführen von Befehlen oder zum dynamischen Anzeigen oder Ausblenden anderer Steuerelemente.

## Auswahlmodus

Mit dem Auswahlmodus können Benutzer ein einzelnes oder mehrere Elemente auswählen und dafür Aktionen vornehmen. Er kann über ein Kontextmenü aufgerufen werden, indem Sie bei gedrückter STRG- oder UMSCHALTTASTE auf ein Element klicken oder in einer Fotogalerieansicht bei einem Element auf ein Ziel zeigen. Wenn der Auswahlmodus aktiviert ist, werden Kontrollkästchen neben jedem Listenelement angezeigt, und Aktionen können am oberen oder unteren Bildschirmrand angezeigt werden.

Es gibt drei verschiedene Auswahlmodi:

-   Einzeln: Dabei kann der Benutzer jeweils nur ein Element auswählen.
-   Mehrfach: Der Benutzer kann mehrere Elemente ohne Modifizierer auswählen.
-   Erweitert: Dabei kann der Benutzer mit Zusatztasten mehrere Elemente auswählen, z. B. durch Gedrückthalten der UMSCHALTTASTE.

Durch Tippen auf ein Element wird es ausgewählt. Das Tippen auf die Aktion auf der Befehlsleiste wirkt sich auf alle ausgewählten Elemente aus. Wenn kein Element ausgewählt ist, sind die Aktionen auf der Befehlsleiste mit Ausnahme von „Alle auswählen“ in der Regel inaktiv.

Im Auswahlmodus funktioniert das einfache Ausblenden nicht. Wenn Sie außerhalb des Frames tippen, in dem der Auswahlmodus aktiv ist, wird der Modus nicht beendet. Dadurch wird die unbeabsichtigte Deaktivierung des Modus verhindert. Per Klick auf die Zurück-Schaltfläche wird der Mehrfachauswahlmodus beendet.

Wenn eine Aktion ausgewählt wurde, wird eine visuelle Bestätigung angezeigt. Ziehen Sie die Anzeige eines Bestätigungsdialogfelds für bestimmte Aktionen in Erwägung, insbesondere bei destruktiven Aktionen wie Löschen.

Der Auswahlmodus ist auf die Seite beschränkt, auf der er aktiv ist. Er wirkt sich nicht auf Elemente außerhalb dieser Seite aus.

Der Einstiegspunkt für den Auswahlmodus sollte neben dem Inhalt platziert werden, auf den er sich auswirkt.

Empfehlungen für die Befehlsleiste finden Sie unter [Richtlinien für Befehlsleisten](app-bars.md).

## Prüfliste für Globalisierung und Lokalisierung

<table>
<tr>
<th>Umbruch</th><td>Zwei Zeilen für die Listenbeschriftung zulassen.</td>
</tr>
<tr>
<th>Horizontale Erweiterung</th><td>Stellen Sie sicher, dass die Felder sich an Texte unterschiedlicher Längen anpassen können und stets ein Bildlauf möglich ist.</td>
</tr>
<tr>
<th>Vertikaler Abstand</th><td>Verwenden Sie nicht lateinische Zeichen für vertikalen Abstand, um sicherzustellen, dass nicht lateinische Schriften richtig angezeigt werden.</td>
</tr>
</table>


## Verwandte Artikel

- [Hub](hub.md)
- [Master/Details](master-details.md)
- [Navigationsbereich](nav-pane.md)
- [Semantischer Zoom](semantic-zoom.md)

**Für Entwickler**
- [**ListView-Klasse**](https://msdn.microsoft.com/library/windows/apps/br242878)
- [**GridView-Klasse**](https://msdn.microsoft.com/library/windows/apps/br242705)
- [**ComboBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209348)
- [**ListBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br242868)
- [Hinzufügen von Kombinationsfeldern und Listenfeldern](https://msdn.microsoft.com/library/windows/apps/xaml/hh780616)




<!--HONumber=Jun16_HO3-->


