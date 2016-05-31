---
author: mijacobs
Description: Bei den Befehlselementen in einer Universellen Windows-Plattform (UWP)-App handelt es sich um die interaktiven Benutzeroberflächenelemente, mit denen der Benutzer Aktionen durchführen kann, um beispielsweise eine E-Mail zu senden, ein Element zu löschen oder ein Formular zu übermitteln.
title: Befehlsdesigngrundlagen für Apps der universellen Windows-Plattform (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
---

#  Befehlsdesigngrundlagen für UWP-Apps

Bei den *Befehlselementen* in einer Universellen Windows-Plattform (UWP)-App handelt es sich um die interaktiven Benutzeroberflächenelemente, mit denen der Benutzer Aktionen durchführen kann, um beispielsweise eine E-Mail zu senden, ein Element zu löschen oder ein Formular zu übermitteln. Dieser Artikel beschreibt Befehlselemente wie Schaltflächen und Kontrollkästchen, die unterstützten Interaktionen sowie die Befehlsoberflächen (wie etwa Befehlsleisten und Kontextmenüs), auf denen sie sich befinden können.

## <span id="Provide_the_right_type_of_interactions"></span><span id="provide_the_right_type_of_interactions"></span><span id="PROVIDE_THE_RIGHT_TYPE_OF_INTERACTIONS"></span>Bereitstellen geeigneter Interaktionen


Die wichtigste Entscheidung bei der Gestaltung einer Befehlsschnittstelle ist die Wahl der möglichen Benutzeraktionen. Wenn Sie beispielsweise eine Foto-App entwickeln, benötigen die Benutzer Werkzeuge für die Fotobearbeitung. Beim Erstellen einer App für soziale Medien, mit der auch Fotos angezeigt werden können, stellt das Bearbeiten von Bildern möglicherweise keine Priorität dar. Daher können Bearbeitungstools weggelassen werden, um Platz zu sparen. Überlegen Sie, welche Aktionen der Benutzer ausführen soll, und stellen Sie die entsprechenden Tools bereit.

Empfehlungen zur Planung geeigneter Interaktionen für Ihre App finden Sie unter [Planen Ihrer App](https://msdn.microsoft.com/library/windows/apps/hh465427.aspx).

## <span id="Use_the_right_command_element_for_the_interaction"></span><span id="use_the_right_command_element_for_the_interaction"></span><span id="USE_THE_RIGHT_COMMAND_ELEMENT_FOR_THE_INTERACTION"></span>Verwenden Sie das richtige Befehlelement für die Interaktion


Die Verwendung passender Elemente für die richtigen Interaktionen kann den Unterschied zwischen einer intuitiv bedienbaren App und einer komplizierten oder verwirrenden App ausmachen. Die Universal Windows Platform (UWP) bietet eine Vielzahl von Befehlselementen in Form von Steuerelementen, die Sie in Ihrer App verwenden können. Die folgende Liste enthält einige der gängigsten Steuerelemente sowie eine Zusammenfassung der jeweils möglichen Interaktionen:

| Kategorie              | Elemente                                                                                                                                                                                                            | Interaktion                                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Schaltflächen               | [Schaltfläche](https://msdn.microsoft.com/library/windows/apps/hh465470)                                                                                                                                                     | Löst eine sofortige Aktion aus, um beispielsweise eine E-Mail zu senden, eine Aktion in einem Dialogfeld zu bestätigen oder Formulardaten zu übermitteln.                                    |
| Datums- und Zeitauswahl | [Kalenderdatumsauswahl, Kalenderansicht, Datumsauswahl, Zeitauswahl](https://msdn.microsoft.com/library/windows/apps/hh465466)                                                                                                                 | Ermöglicht dem Benutzer das Anzeigen und Ändern von Datums- und Uhrzeitinformationen (beispielsweise beim Eingeben des Ablaufdatums einer Kreditkarte oder beim Festlegen einer Weckzeit).                   |
| Listen                 | [Dropdownliste, Listenfeld, Listen- und Rasteransicht](https://msdn.microsoft.com/library/windows/apps/mt186889)                                                                                                                                              | Stellt Elemente in einer interaktiven Liste oder in einem Raster dar. Verwenden Sie diese Elemente, um Benutzern die Auswahl eines Films aus einer Liste mit Neuerscheinungen oder die Verwaltung von Inventar zu ermöglichen. |
| Textvorhersage | [Feld mit automatischen Vorschlägen](https://msdn.microsoft.com/library/windows/apps/dn997762)                                                                                                                                                                    | Liefert Vorschläge während der Eingabe durch den Benutzer und ermöglicht somit schnellere Dateneingaben und Abfragen.                                                   |
| Auswahlsteuerelemente    | [Kontrollkästchen](https://msdn.microsoft.com/library/windows/apps/hh700393), [Optionsfeld](https://msdn.microsoft.com/library/windows/apps/hh700395), [Umschalter](https://msdn.microsoft.com/library/windows/apps/hh465475) | Gibt dem Benutzer die Wahl zwischen verschiedenen Optionen (beispielsweise bei einer Umfrage oder beim Konfigurieren von App-Einstellungen).                                      |

 

Eine vollständige Liste finden Sie unter [Steuerelemente und UI-Elemente](https://dev.windows.com/design/controls-patterns).

## <span id="_________Place_commands_on_the_right_surface_______"></span><span id="_________place_commands_on_the_right_surface_______"></span><span id="_________PLACE_COMMANDS_ON_THE_RIGHT_SURFACE_______"></span> Platzieren von Befehlen auf der passenden Oberfläche


Befehlselemente können in Ihrer App auf verschiedenen Oberflächen platziert werden. Hierzu zählen etwa die App-Canvas (Inhaltsbereich der App) sowie spezielle Befehlselemente (z. B. Befehlsleisten, Menüs, Dialogfelder und Flyouts), die als Befehlscontainer fungieren können. Im Anschluss finden Sie einige allgemeine Empfehlungen für die Platzierung von Befehlen:

-   Ermöglichen Sie es den Benutzern nach Möglichkeit, Inhalte direkt auf der App-Canvas manipulieren, anstatt Befehle für die Interaktion mit dem Inhalt hinzuzufügen. Gestalten Sie also beispielsweise die Reise-App so, dass die Benutzer ihre Reiseroute verändern können, indem sie Aktivitäten in einer Liste auf der Canvas per Drag & Drop verschieben, anstatt die Aktivität auswählen und sie mithilfe entsprechender Befehlsschaltflächen nach oben/unten verschieben zu müssen.
-   Wenn Benutzer Inhalte nicht direkt bearbeiten können, platzieren Sie die Befehle auf einer der folgenden Benutzeroberflächen:

    -   Auf der [Befehlsleiste](https://msdn.microsoft.com/library/windows/apps/hh465302): Platzieren Sie den Großteil der Befehle auf der Befehlsleiste. Dies kommt der Übersichtlichkeit zugute und erleichtert den Zugriff.
    -   Auf der App-Canvas: Befindet sich der Benutzer auf einer Seite oder in einer Ansicht mit nur einem Zweck, können Sie Befehle für diesen Zweck direkt auf der Canvas bereitstellen. Es sollte nur sehr wenige dieser Befehle geben.
    -   In einem [Kontextmenü](https://msdn.microsoft.com/library/windows/apps/hh465308): Sie können Kontextmenüs für Zwischenablageaktionen (z. B. Ausschneiden, Kopieren und Einfügen) oder für Befehle verwenden, die auf Inhalte angewendet werden, die nicht ausgewählt werden können (z. B. Hinzufügen einer Stecknadel zu einem Standort auf einer Karte).

Die folgende Liste enthält die unter Windows verfügbaren Befehlsoberflächen sowie Empfehlungen für deren Verwendung:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Oberfläche</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">App-Canvas (Inhaltsbereich)
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>
<td align="left"><p>Wichtige oder häufig benötigte Befehle für die Hauptszenarien der App sollten auf der Canvas (dem Inhaltsbereich Ihrer App) platziert werden. Da Sie Befehle in der Nähe von (oder auf) Objekten platzieren können, die durch den Befehl beeinflusst werden, sind auf der Canvas platzierte Befehle besonders komfortabel und intuitiv.</p>
<p>Wählen Sie die Befehle für die Canvas aber mit Bedacht. Wenn Sie zu viele Befehle auf der App-Canvas positionieren, geht wertvoller Platz und die Übersichtlichkeit für den Benutzer verloren. Selten verwendete Befehle sind unter Umständen auf einer anderen Befehlsoberfläche besser aufgehoben – beispielsweise in einem Menü oder auf der Befehlsleiste im Bereich „Mehr“.</p></td>
</tr>
<tr class="even">
<td align="left">[Befehlsleiste](https://msdn.microsoft.com/library/windows/apps/hh465302)
<p><img src="images/controls-appbar-icons-200.png" alt="Example of a command bar with icons" /></p></td>
<td align="left"><p>Über eine Befehlsleiste können Benutzer komfortabel auf Aktionen zugreifen. Auf einer App-Leiste können Sie Befehle oder Optionen für den jeweiligen Benutzerkontext anzeigen (etwa Fotoauswahl oder Zeichnungsmodus).</p>
<p>Befehlsleisten können am oberen Bildschirmrand, am unteren Bildschirmrand oder sowohl am oberen als auch am unteren Bildschirmrand platziert werden. Das folgende Design einer Fotobearbeitungs-App zeigt den Inhaltsbereich und die Befehlsleiste:</p>
<p><img src="images/commands-appcanvas-example.png" alt="A photo app" /></p>
<p>Weitere Informationen zu Befehlsleisten finden Sie unter [Richtlinien für die Befehlsleiste](https://msdn.microsoft.com/library/windows/apps/hh465302).</p></td>
</tr>
<tr class="odd">
<td align="left">[Menüs und Kontextmenüs](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left"><p>Mitunter ist es effektiver, mehrere Befehle in einem Befehlsmenü zu gruppieren. In Menüs können Sie platzsparend mehrere Optionen unterbringen. Menüs können interaktive Steuerelemente enthalten.</p>
<p>Kontextmenüs eignen sich für Verknüpfungen mit häufig verwendeten Aktionen und ermöglichen den Zugriff auf sekundäre Befehle, die nur in bestimmten Kontexten relevant sind.</p>
<p>Kontextmenüs sind für die folgenden Arten von Befehlen und Befehlsszenarien gedacht:</p>
<ul>
<li>Kontextbezogene Aktionen beim Auswählen von Text, z. B. Kopieren, Ausschneiden, Einfügen, Rechtschreibprüfung usw.</li>
<li>Befehle für ein Objekt, das Gegenstand einer Aktion sein soll, aber weder ausgewählt noch anderweitig angegeben werden kann.</li>
<li>Anzeigen von Befehlen in der Zwischenablage</li>
<li>Benutzerdefinierte Befehle</li>
</ul>
<p>Dieses Beispiel zeigt das Design für eine U-Bahn-App mit einem Kontextmenü, über das der Benutzer die Route ändern, ein Lesezeichen für die Route hinzufügen oder einen anderen Zug auswählen kann.</p>
<p><img src="images/subway/uap-subway-ak-8in-dashboard-200.png" alt="A context menu in an subway app" /></p>
<p>Weitere Informationen zu Kontextmenüs finden Sie unter [Richtlinien für Kontextmenüs](https://msdn.microsoft.com/library/windows/apps/hh465308).</p></td>
</tr>
<tr class="even">
<td align="left">[Dialogfeld-Steuerelemente](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left"><p>Dialogfelder sind modale Benutzeroberflächenüberlagerungen, die kontextbezogene App-Informationen enthalten. Die meisten Dialogfelder blockieren die Interaktion mit dem App-Fenster, bis sie explizit geschlossen werden, und erfordern häufig eine Aktion des Benutzers.</p>
<p>Dialogfelder können als störend empfunden werden und sollten daher nur in bestimmten Situationen zum Einsatz kommen. Weitere Informationen finden Sie unter [Bestätigen oder Rückgängigmachen von Aktionen](#whentoconfirm)</p></td>
</tr>
<tr class="odd">
<td align="left">[Flyout](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left"><p>Ein einfaches kontextbezogenes Popupmenü zum Anzeigen von UI-Elementen im Zusammenhang mit den Aktionen des Benutzers. Flyouts eignen sich für Folgendes:</p>
<p></p>
<ul>
<li>Anzeigen eines Menüs</li>
<li>Anzeigen weiterer Details zu einem Element</li>
<li>Anfordern einer Aktionsbestätigung, ohne die Interaktion mit der App zu blockieren</li>
</ul>
<p>Flyouts können geschlossen werden, indem Benutzer auf eine Stelle außerhalb des Flyouts tippen oder klicken. Weitere Informationen zu den Flyout-Steuerelementen finden Sie im Artikel [Dialogfelder, Menüs und Popups](../controls-and-patterns/dialogs-popups-menus.md).</p></td>
</tr>
</tbody>
</table>

 

## <span id="whentoconfirm"></span><span id="WHENTOCONFIRM"></span>Bestätigen oder Rückgängigmachen von Aktionen


Ganz gleich, wie gut die Benutzeroberfläche gestaltet ist und wie vorsichtig der Benutzer vorgeht: Irgendwann wird jeder Benutzer eine Aktion ausführen, die er lieber nicht ausgeführt hätte. In einer solchen Situation kann Ihre App dem Benutzer helfen, indem sie eine Aktionsbestätigung anfordert oder eine Möglichkeit zum Rückgängigmachen der kürzlich durchgeführten Aktionen anbietet.

-   Für Aktionen, die nicht rückgängig gemacht werden können und schwerwiegende Auswirkungen haben, empfehlen wir die Verwendung eines Bestätigungsdialogfelds. Beispiele für solche Aktionen:
    -   Überschreiben einer Datei
    -   Schließen einer Datei ohne Speichern
    -   Bestätigen der endgültigen Löschung einer Datei oder von Daten
    -   Einkaufen (es sei denn, der Benutzer lehnt eine Bestätigungsanforderung ab)
    -   Senden eines Formulars, z. B. bei Registrierungen
-   Für Aktionen, die rückgängig gemacht werden können, ist ein einfacher „Rückgängig“-Befehl in der Regel ausreichend. Beispiele für solche Aktionen:
    -   Löschen einer Datei
    -   Löschen einer E-Mail (nicht endgültig)
    -   Ändern des Inhalts oder Bearbeiten von Text
    -   Umbenennen einer Datei

**Tipp**  Verwenden Sie nicht zu viele Bestätigungsdialogfelder. Diese können zwar sehr hilfreich sein, wenn dem Benutzer ein Fehler unterläuft, bei bewusst durchgeführten Aktionen sind sie jedoch eher hinderlich.

 

## <span id="_________Optimize_for_specific_input_types_______"></span><span id="_________optimize_for_specific_input_types_______"></span><span id="_________OPTIMIZE_FOR_SPECIFIC_INPUT_TYPES_______"></span> Optimieren für bestimmte Eingabearten


Ausführliche Informationen zum Optimieren der Benutzerfreundlichkeit bei einem bestimmten Eingabetyp oder -gerät finden Sie unter [Einführung in die Interaktion](../input-and-devices/input-primer.md).




 

 






<!--HONumber=May16_HO2-->


