---
author: Jwmsft
Description: "Ein Flyout ist ein kleines Popupmenü, das vorübergehend UI zu aktuellen Benutzeraktionen anzeigt."
title: "Kontextmenüs und Dialogfelder"
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, dialogs, and popups
template: detail.hbs
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: e268a5facebbdb80d7cc5cdd52c1a6f944ef7d00

---
# Menüs, Dialogfelder, Flyouts und Popups

Menüs, Dialogfelder, Flyouts und Popups zeigen vorübergehende UI-Elemente an, die angezeigt werden, wenn der Benutzer sie anfordert oder eine Aktion erfolgt, die eine Benachrichtigung oder Genehmigung erfordert.

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [MenuFlyout-Klasse](https://msdn.microsoft.com/library/windows/apps/dn299030)
-   [Flyout-Klasse](https://msdn.microsoft.com/library/windows/apps/dn279496)
-   [ContentDialog-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)

Ein Kontextmenü stellt sofortige Aktionen für den Benutzer bereit. Es kann mit Textbefehlen gefüllt werden. Kontextmenüs können einfach ausgeblendet werden, indem Benutzer auf eine Stelle außerhalb des Menüs tippen oder klicken.

Dialogfelder sind modale Benutzeroberflächenüberlagerungen, die kontextbezogene App-Informationen enthalten. Dialogfelder blockieren Interaktionen mit dem App-Fenster, bis sie explizit geschlossen werden. Sie verlangen häufig eine Aktion vom Benutzer.

Ein Flyout ist ein einfaches kontextbezogenes Popupmenü zum Anzeigen der Benutzeroberfläche, die im Zusammenhang mit den Aktionen des Benutzers steht. Es umfasst die Platzierung und Größe und kann ein ausgeblendetes Steuerelement oder weitere Details zu einem Element anzeigen oder den Benutzer zum Bestätigen einer Aktion auffordern. Flyouts können einfach ausgeblendet werden, indem Benutzer auf eine Stelle außerhalb des Popups tippen oder klicken.


## Ist dies das richtige Steuerelement?

Kontextmenüs können in folgenden Situationen verwendet werden:

-   Kontextbezogene Aktionen.
-   Befehle für ein Objekt, das Gegenstand einer Aktion sein muss, aber nicht ausgewählt werden kann.

Dialogfelder können in folgenden Situationen verwendet werden:

- Für wichtige Informationen, die der Benutzer vor dem Fortsetzen lesen und bestätigen muss.
- Zum Anfordern einer eindeutigen Aktion des Benutzers oder zum Kommunizieren einer wichtigen Nachrichten, die der Benutzer bestätigen muss. Beispiele:
  - Die Sicherheit des Benutzers ist möglicherweise gefährdet.
  - Der Benutzer möchte eine wertvolle Ressource endgültig ändern.
  - Der Benutzer möchte eine wertvolle Ressource löschen.
  - Bestätigen eines In-App-Einkaufs
- Fehlermeldungen, die sich auf den Gesamtkontext der App beziehen, z. B. Verbindungsfehler.
- Fragen, wenn dem Benutzer eine blockierende Frage gestellt werden muss, z. B. wenn die App nicht im Auftrag des Benutzers eine Auswahl treffen kann. Eine blockierende Frage kann nicht ignoriert oder verschoben werden und sollte dem Benutzer klar definierte Auswahlmöglichkeiten bieten.

Flyouts können in folgenden Situationen verwendet werden:

-   Kontextbezogene, vorübergehende Benutzeroberfläche.
-   Warnungen und Bestätigungen, z. B. im Zusammenhang mit möglicherweise schädlichen Aktionen.
-   Anzeigen weiterer Informationen, z. B. von Details oder ausführlicheren Beschreibungen eines Elements auf der Seite.


## Beispiele

Hier ist ein typisches Kontextmenü mit einem einzigen Bereich mit einer kurzen Liste einfacher Befehle. Sie können bei Bedarf einen Bildlauf durchführen. Verwenden Sie Trennlinien zum Gruppieren von ähnlichen Befehlen.

![Beispiel für ein typisches Kontextmenü](images/controls_contextmenu_singlepane.png)

Ein überlappendes Kontextmenü kann für eine umfassendere Sammlung von Befehlen verwendet werden. Es verfügt über mehrere Flyoutebenen und Bildlauf. Verwenden Sie Trennlinien zum Gruppieren von ähnlichen Befehlen.

![Beispiel für ein überlappendes Kontextmenü](images/controls_contextmenu_cascading.png)

Dies ist ein Beispiel für ein Bestätigungsdialogfeld im Vollbildmodus mit einer Schaltfläche. Bei dieser Art von Dialogfeld wird dem Benutzer ein Großteil der Informationen angezeigt, die gelesen werden sollen, bevor sie auf die Schalfläche zum Fortfahren klicken.

![Beispiel für ein ganzseitiges Dialogfeld mit einer Schaltfläche](images/controls_dialog_singlebutton.png)

Hier ist ein Beispiel eines Dialogfelds mit zwei Schaltflächen, bei dem der Benutzer eine Wahl zwischen A und B hat. Im Allgemeinen ist die Menge an Informationen in diesem Dialogfeld gering.

![Beispiel für ein Dialogfeld mit einer Schaltfläche](images/controls_dialog_twobutton.png)

## Modales Ausblenden im Vergleich zu einfachem Ausblenden

Dialogfelder sind modal, was bedeutet, dass sie alle Interaktionen mit der App blockieren, bis der Benutzer eine Dialogfeldschaltfläche auswählt. Um das modale Verhalten von Dialogfeldern visuell zu unterstreichen, zeichnen sie eine Overlay-Ebene, welche die vorübergehend nicht erreichbare App-UI teilweise verdeckt.


            **Hinweis** Ist „Abbrechen“ eine der verfügbaren Dialogfeldoptionen, können Apps Benutzer das Dialogfeld durch Drücken der ESC-Taste schließen lassen. Dieses Verhalten ist nicht in das Steuerelement integriert, stellt aber einen häufig implementierten Shortcut dar.

Flyouts und Kontextmenüs sind einfach auszublendende Steuerelemente, was bedeutet, dass Benutzer eine Reihe von Aktionen auswählen können, um vorübergehend angezeigte Benutzeroberflächen schnell zu schließen. Diese Interaktionen sollen leicht sein und den Benutzer nicht blockieren. Aktionen zum einfachen Ausblenden sind folgende
- Klicken oder Tippen außerhalb der vorübergehend angezeigten Benutzeroberfläche
- Drücken der ESC-Taste
- Drücken der Zurück-Taste
- Skalieren des App-Fensters
- Ändern der Geräteausrichtung


## Nutzungsrichtlinien zur Verwendung von Dialogfeldern

-   Sie sollten das Problem oder das Ziel direkt in der ersten Zeile des Dialogfeldtexts deutlich benennen.
-   Der Dialogfeldtitel ist die Hauptanweisung und optional.
    -   Verwenden Sie einen kurzen Titel, um dem Benutzer die erforderlichen Aktionen im Dialogfeld zu erklären. Lange Titel werden nicht umgebrochen und daher abgeschnitten.
    -   Wenn Sie mit dem Dialogfeld eine einfache Meldung, einen Fehler oder eine Frage anzeigen möchten, können Sie den Titel optional auslassen. Vermitteln Sie dann im Inhalt die wichtigsten Informationen.
    -   Stellen Sie sicher, dass sich der Titel direkt auf die Auswahlmöglichkeiten der Schaltflächen bezieht.
-   Der Inhalt des Dialogfelds enthält den beschreibenden Text und ist erforderlich.
    -   Stellen Sie die Meldung, den Fehler oder die blockierende Frage so einfach wie möglich dar.
    -   Stellen Sie bei Verwendung eines Dialogfeldtitels mithilfe des Inhaltsbereichs weitere Details bereit, oder definieren Sie Terminologie. Wiederholen Sie nicht den Titel mit anderen Worten.
-   Mindestens eine Dialogfeldschaltfläche muss angezeigt werden.
    -   Schaltflächen sind die einzigen Mechanismen, mit denen Benutzer das Dialogfeld schließen können.
    -   Verwenden Sie Schaltflächen mit Text, die eine konkrete Reaktion auf die Hauptanweisung oder den Inhalt repräsentieren. Beispiel: "Möchten Sie AppName den Zugriff auf Ihren Standort erlauben?", gefolgt von den Schaltflächen "Zulassen" und "Blockieren". Klare Antworten erleichtern das Verständnis und damit die schnelle Entscheidungsfindung.
-   Bei Fehlerdialogfeldern wird die Fehlermeldung im Dialogfeld zusammen mit allen relevanten Informationen angezeigt. Die einzige in einem Fehlerdialogfeld verwendete Schaltfläche sollte „Schließen“ oder eine ähnliche Aktion sein.
-   Kontextbezogene Fehler, die sich auf eine bestimmte Stelle auf der Seite beziehen, beispielsweise Validierungsfehler (wie in Kennwortfeldern), verwenden die Canvas der App selbst zum Anzeigen von Inlinefehlern.

## Kontextmenüs und Flyouts

Kontextmenüs und Flyouts sind eng verwandte Steuerelemente mit gemeinsamem Interaktionsverhalten. Der Hauptunterschied zwischen diesen Steuerelementen ist der Typ des Inhalts, den sie akzeptieren.

### MenuFlyout
Ein Kontextmenü, das mit der MenuFlyout-Klasse implementiert wurde, kann [**MenuFlyoutItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyoutitem.aspx), [**ToggleMenuFlyoutItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.togglemenuflyoutitem.aspx), [**MenuFlyoutSubItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyoutsubitem.aspx) und [**MenuFlyoutSeparator**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyoutseparator.aspx) enthalten. Verwenden Sie zum Anzeigen von anderen UI-Typen Flyout.

- **Nutzungsrichtlinien**
  - Verwenden Sie für folgende Aktionen Trennlinien zwischen Gruppen von Befehlen in einem Kontextmenü:
    - Trennen von Gruppen verwandter Befehle
    - Gruppieren von Befehlen
    - Trennen von spezifischen Befehlen für eine App oder Ansicht von Standardbefehlen, z.B. für die Zwischenablage, (Ausschneiden/Kopieren/Einfügen)
  -   Auf Laptops und Desktops sind Kontextmenüs und QuickInfos nicht auf das Anwendungsfenster begrenzt und können teilweise außerhalb davon gezeichnet werden. Wenn die App versucht, ein Kontextmenü vollständig außerhalb des Fensters darzustellen, wird eine Ausnahme ausgelöst.

- **Empfohlene und nicht empfohlene Vorgehensweisen**
  -   Achten Sie darauf, dass Kontextmenübefehle kurz sind. Längere Befehle werden abgeschnitten.
  -   Verwenden Sie die Großschreibung am Anfang für jeden Befehlsnamen.
  -   Zeigen Sie in jedem Kontextmenü die Obergrenze für die Anzahl möglicher Befehle.
  -   Wenn die direkte Manipulation eines Benutzeroberflächenelements möglich ist, vermeiden Sie es, diesen Befehl in einem Kontextmenü zu platzieren. Ein Kontextmenü sollte für Kontextbefehle reserviert werden, die andernfalls auf dem Bildschirm nicht sichtbar sind.

### Flyout

Ein Flyout ist ein offener Container, der beliebige UI als Inhalt anzeigen kann.  Flyouts weisen selbst keine visuellen Teile auf. Sie sind einfach ein Inhaltssteuerelement. Flyouts verfügen über einen Rand und optionale Bildlaufleisten, die sie zu ihren Inhalten hinzufügen. Um ein Flyout zu formatieren, ändern Sie den `FlyoutPresenterStyle`.

Der folgende Code zeigt einen Absatz mit Textumbruch und macht den Textblock für ein Bildschirmleseprogramm zugänglich.

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode" Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

### Aufrufen und Platzierung

Flyouts und Kontextmenüs sind bestimmten Steuerelementen angefügt. Wenn sie sichtbar sind, sollten sie am aufrufenden Objekt verankert sein und ihre bevorzugte relative Position zum Objekt angeben: oben, links, unten oder rechts. Flyout verfügt außerdem über einen vollständigen Platzierungsmodus, der versucht, das Flyout zu strecken und innerhalb des App-Fensters zu zentrieren.

Die [Schaltflächen-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) enthält eine `Flyout`-Eigenschaft, mit der Sie die vorübergehend angezeigte Benutzeroberfläche angeben können, die geöffnet wird, wenn der Benutzer auf die Schaltfläche klickt oder tippt.

````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="Yay!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

Um ein Kontextmenü zu öffnen, können Benutzer eine der folgenden Aktionen ausführen:
- Rechtsklicken mit der Maus
- Drücken und Halten mit dem Finger
- Eingeben von UMSCHALT + F10
- Drücken der Menü-Taste auf der Tastatur
- Drücken der Gamepad-Menütaste

Um ein Kontextmenü oder Flyout als Reaktion auf einen der oben genannten Schritte leicht zu öffnen, können Apps die neue [`ContextFlyout`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)-Eigenschaft auf [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx), die Basisklasse für die meisten Steuerelemente, verwenden.

````xaml
<Rectangle Height="100" Width="100" Fill="Red">
  <Rectangle.ContextFlyout>
     <MenuFlyout>
        <MenuFlyoutItem Text="Close"/>
     </MenuFlyout>
  </Rectangle.Flyout>
</Rectangle>
````

## Verwandte Artikel

**Für Entwickler**
- [**MenuFlyout-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [**Flyout-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**ContentDialog-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)



<!--HONumber=Jun16_HO4-->


