---
Description: Ein Flyout ist ein kleines Popupmenü, das vorübergehend UI zu aktuellen Benutzeraktionen anzeigt.
title: Kontextmenüs und Dialogfelder
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, dialogs, and popups
template: detail.hbs
---
# Menüs, Dialogfelder und Popups

Menüs, Dialogfelder und Popups zeigen vorübergehende UI-Elemente an, die angezeigt werden, wenn der Benutzer sie anfordert oder eine Aktion erfolgt, die eine Benachrichtigung oder Genehmigung erfordert. 

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [MenuFlyout-Klasse](https://msdn.microsoft.com/library/windows/apps/dn299030)
-   [Flyout-Klasse](https://msdn.microsoft.com/library/windows/apps/dn279496)

Ein Kontextmenü stellt sofortige Aktionen für den Benutzer bereit. Es kann mit benutzerdefinierten Befehlen gefüllt werden. Kontextmenüs können geschlossen werden, indem Benutzer auf eine Stelle außerhalb des Menüs tippen oder klicken.

Dialogfelder sind modale Benutzeroberflächenüberlagerungen, die kontextbezogene App-Informationen enthalten. Die meisten Dialogfelder blockieren die Interaktion mit dem App-Fenster, bis sie explizit geschlossen werden, und erfordern häufig eine Aktion des Benutzers.

Ein Popup (auch als Flyout bezeichnet) ist ein einfaches kontextbezogenes Popupmenü zum Anzeigen der Benutzeroberfläche, die im Zusammenhang mit den Aktionen des Benutzers steht. Es umfasst die Platzierung und Größe und kann ein Menü, ein ausgeblendetes Steuerelement oder weitere Details zu einem Element anzeigen oder den Benutzer zum Bestätigen einer Aktion auffordern. Popups können geschlossen werden, indem Benutzer auf eine Stelle außerhalb des Popups tippen oder klicken.


## Ist dies das richtige Steuerelement?

Kontextmenüs können in folgenden Situationen verwendet werden:

-   Kontextbezogene Aktionen beim Auswählen von Text, z. B. Kopieren, Ausschneiden, Einfügen, Rechtschreibprüfung usw.
-   Befehle für die Zwischenablage
-   Benutzerdefinierte Befehle
-   Befehle für ein Objekt, das Gegenstand einer Aktion sein muss, aber weder ausgewählt noch anderweitig angegeben werden kann.

Dialogfelder können in folgenden Situationen verwendet werden:

- Für wichtige Informationen, die der Benutzer vor dem Fortsetzen lesen und bestätigen muss.
- Zum Anfordern einer eindeutigen Aktion des Benutzers oder zum Kommunizieren einer wichtigen Nachrichten, die der Benutzer bestätigen muss. Beispiele:
  - Die Sicherheit des Benutzers ist möglicherweise gefährdet.
  - Der Benutzer möchte eine wertvolle Ressource endgültig ändern.
  - Der Benutzer möchte eine wertvolle Ressource löschen.
  - Bestätigen eines In-App-Einkaufs
- Fehlermeldungen, die sich auf den Gesamtkontext der App beziehen, z. B. Verbindungsfehler.
- Fragen, wenn dem Benutzer eine blockierende Frage gestellt werden muss, z. B. wenn die App nicht im Auftrag des Benutzers eine Auswahl treffen kann. Eine blockierende Frage kann nicht ignoriert oder verschoben werden und sollte dem Benutzer klar definierte Auswahlmöglichkeiten bieten.

Popups (Flyouts) können in folgenden Situationen verwendet werden:

-   Kontextbezogene, vorübergehende Benutzeroberfläche.
-   Warnungen und Bestätigungen, z. B. im Zusammenhang mit möglicherweise schädlichen Aktionen.
-   Anzeigen von weiteren Informationen, z. B. Details zu einem Element auf dem Bildschirm.
-   Menüs der zweiten Ebene.
-   Benutzerdefinierte Befehle


## Beispiele

Im Folgenden wird ein typisches Kontextmenü mit einem Bereich dargestellt. Dieses wird für eine kürzere Liste von einfachen Befehlen empfohlen. Verwenden Sie Trennlinien zum Gruppieren von ähnlichen Befehlen.

![Beispiel für ein typisches Kontextmenü](images/controls_contextmenu_singlepane.png)

Ein überlappendes Kontextmenü wird für eine umfassendere Sammlung von Befehlen empfohlen. Es verfügt über eine Flyoutebene und Bildlauf. Verwenden Sie Trennlinien zum Gruppieren von ähnlichen Befehlen.

![Beispiel für ein überlappendes Kontextmenü](images/controls_contextmenu_cascading.png)

Dies ist ein Beispiel für ein Bestätigungsdialogfeld im Vollbildmodus mit einer Schaltfläche. Bei dieser Art von Dialogfeld wird dem Benutzer ein Großteil der Informationen angezeigt, die gelesen werden sollen, bevor sie auf die Schalfläche zum Fortfahren klicken.

![Beispiel für ein ganzseitiges Dialogfeld mit einer Schaltfläche](images/controls_dialog_singlebutton.png)

Hier ist ein Beispiel eines Dialogfelds mit zwei Schaltflächen, bei dem der Benutzer eine Wahl zwischen A und B hat. Im Allgemeinen ist die Menge an Informationen in diesem Dialogfeld gering.

![Beispiel für ein Dialogfeld mit einer Schaltfläche](images/controls_dialog_twobutton.png)


## Informationen zur Verwendung

**Kontextmenüs**
- Verwenden Sie für folgende Aktionen Trennlinien zwischen Gruppen von Befehlen in einem Kontextmenü:
  - Trennen von Gruppen verwandter Befehle
  - Gruppieren von Befehlen
  - Trennen von spezifischen Befehlen für eine App oder Ansicht von Standardbefehlen, z. B. für die Zwischenablage, (Ausschneiden/Kopieren/Einfügen)
-   Auf Laptops und Desktops sind Kontextmenüs und QuickInfos nicht auf das Anwendungsfenster begrenzt und können außerhalb davon gezeichnet werden. Wenn die App versucht, ein Kontextmenü vollständig außerhalb des Fensters darzustellen, wird eine Ausnahme ausgelöst.

**Dialogfelder**
-   Sie sollten das Problem oder das Ziel direkt in der ersten Zeile des Dialogfeldtexts deutlich benennen.
-   Der Dialogfeldtitel ist die Hauptanweisung und optional.
    -   Verwenden Sie einen kurzen Titel, um dem Benutzer die erforderlichen Aktionen im Dialogfeld zu erklären. Lange Titel werden nicht umgebrochen und daher abgeschnitten.
    -   Wenn Sie mit dem Dialogfeld eine einfache Meldung, einen Fehler oder eine Frage anzeigen möchten, können Sie den Titel optional auslassen. Vermitteln Sie dann im Inhalt die wichtigsten Informationen.
    -   Stellen Sie sicher, dass sich der Titel direkt auf die Auswahlmöglichkeiten der Schaltflächen bezieht.
-   Der Inhalt des Dialogfelds enthält den beschreibenden Text und ist erforderlich.
    -   Stellen Sie die Meldung, den Fehler oder die blockierende Frage so einfach wie möglich dar.
    -   Stellen Sie bei Verwendung eines Dialogfeldtitels mithilfe des Inhaltsbereichs weitere Details bereit, oder definieren Sie Terminologie. Wiederholen Sie nicht den Titel mit anderen Worten.
-   Mindestens eine Dialogfeldschaltfläche muss angezeigt werden.
    -   Verwenden Sie Schaltflächen mit Text, die eine konkrete Reaktion auf die Hauptanweisung oder den Inhalt repräsentieren. Beispiel: "Möchten Sie AppName den Zugriff auf Ihren Standort erlauben?", gefolgt von den Schaltflächen "Zulassen" und "Blockieren". Klare Antworten erleichtern das Verständnis und damit die schnelle Entscheidungsfindung.
-   Bei Fehlerdialogfeldern wird die Fehlermeldung im Dialogfeld zusammen mit allen relevanten Informationen angezeigt. Die einzige in einem Fehlerdialogfeld verwendete Schaltfläche sollte „Schließen“ oder eine ähnliche Aktion sein.
-   Kontextbezogene Fehler, die sich auf eine bestimmte Stelle auf der Seite beziehen, beispielsweise Validierungsfehler (wie in Kennwortfeldern), verwenden die Canvas der App selbst zum Anzeigen von Inlinefehlern.

**Popups**

-   Platzieren Sie ein Popup, wie andere kontextbezogene UI-Elemente, neben dem Punkt, von dem es aufgerufen wird.
    -   Legen Sie das Objekt fest, mit dem das Popup verknüpft werden soll, und die Seite des Objekts, auf dem das Popup angezeigt wird.
    -   Versuchen Sie, das Popup so zu positionieren, dass es keine wichtigen UI-Elemente blockiert.
-   Das Popup sollte geschlossen werden, sobald etwas darin ausgewählt ist.
-   Popupmenüs funktionieren am besten mit einer Ebene. Popupmenüs mit mehreren Ebenen erschweren die Navigation und beeinträchtigen die Benutzerfreundlichkeit.

## Empfohlene und nicht empfohlene Vorgehensweisen

-   Achten Sie darauf, dass Kontextmenübefehle kurz sind. Längere Befehle werden abgeschnitten.
-   Verwenden Sie die Großschreibung am Anfang für jeden Befehlsnamen.
-   Zeigen Sie in jedem Kontextmenü die Obergrenze für die Anzahl möglicher Befehle.
-   Wenn die direkte Manipulation eines Benutzeroberflächenelements möglich ist, vermeiden Sie es, diesen Befehl in einem Kontextmenü zu platzieren. Ein Kontextmenü sollte für Kontextbefehle reserviert werden, die andernfalls auf dem Bildschirm nicht sichtbar sind.



## Verwandte Artikel

**Für Entwickler**
- [**MenuFlyout-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [**Flyout-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn279496)


<!--HONumber=Mar16_HO4-->


