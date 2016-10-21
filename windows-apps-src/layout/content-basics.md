---
author: mijacobs
Description: "Der Hauptzweck einer App besteht darin, den Zugriff auf Inhalte zu gewähren. In einer Fotobearbeitungs-App sind Fotos der Content, in einer Reise-App Karten und Informationen über die Reiseziele usw."
title: "Grundlagen des Inhaltsdesigns für UWP-Apps (Universelle Windows-Plattform)"
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 4ac800f9d2dd51ac074ec50cd1046e5a78c80710

---

#  Grundlagen des Inhaltsdesigns für UWP-Apps (Universelle Windows-Plattform)

Der Hauptzweck jeder App ist das Bereitstellen von Zugriff auf Content: In einer Fotobearbeitungs-App sind Fotos der Content, in einer Reise-App Karten und Informationen über die Reiseziele usw. Navigationselemente bieten Zugriff auf Content; Befehlselemente ermöglichen dem Benutzer die Interaktion mit Content; Content-Elemente zeigen den tatsächlichen Content an.

Dieser Artikel bietet Empfehlungen zum Content-Design für drei Content-Szenarien.

## <span id="Design_for_the_right_content_scenario"></span><span id="design_for_the_right_content_scenario"></span><span id="DESIGN_FOR_THE_RIGHT_CONTENT_SCENARIO"></span>Design für das richtige Content-Szenario


Dies sind die drei wichtigsten Contentszenarien:

-   **Konsum**: Eine hauptsächlich einseitige Erfahrung, bei der der Content konsumiert wird. Beinhaltet Aufgaben wie Lesen, Musik hören, Videos, Fotos und Bilder ansehen.
-   **Erstellen**: Eine hauptsächlich einseitige Erfahrung, bei der der Schwerpunkt auf dem Erstellen von neuem Content liegt. Kann aufgeschlüsselt werden in das Erstellen von Dingen von Grund auf, wie etwa Aufnehmen eines Fotos oder Videos, das Erstellen eines neuen Bilds in einer Mal-App oder das Öffnen eines leeren Dokuments.
-   **Interaktiv**: Eine bilaterale Content-Erfahrung, die den Konsum, die Erstellung und Überprüfung von Content umfasst.

## <span id="Consumption-focused_apps"></span><span id="consumption-focused_apps"></span><span id="CONSUMPTION-FOCUSED_APPS"></span>Konsumorientierte Apps


Content-Elemente erhalten die höchste Priorität in einer konsumorientierten App, gefolgt von den [Navigationselementen](navigation-basics.md), die erforderlich sind, damit die Benutzer den gewünschten Content finden. Beispiele für konsumorientierte Apps sind Movie-Player, Lese-Apps, Musik-Apps und Foto-Viewer.

![Eine Nachrichten-App](images/news-reader/v2/newsreader-v2-tablet-phone.png)

Allgemeine Empfehlungen für konsumorientierte Apps:

-   Erstellen Sie dedizierte [Navigationsseiten](navigation-basics.md) und Content-Seiten, sodass die Benutzer den gewünschten Content finden und auf einer dedizierten Seite ohne Ablenkung ansehen können.
-   Erstellen Sie eine Vollbild-Anzeigeoption, die den Content auf den gesamten Bildschirm erweitert und alle anderen UI-Elemente ausblendet.

## <span id="Creation-focused_apps"></span><span id="creation-focused_apps"></span><span id="CREATION-FOCUSED_APPS"></span>Erstellungsorientierte Apps


Content- und [Befehlselemente](commanding-basics.md) sind die wichtigsten UI-Elemente in einer erstellungsorientierten App. Befehlselemente ermöglichen dem Benutzer das Erstellen von neuem Content. Zu den Beispielen gehören Mal-Apps, Fotobearbeitungs-Apps, Videobearbeitungs-Apps und Textverarbeitungs-Apps.

Als Beispiel finden Sie hier ein Design einer Foto-App, die Befehlszeilen zum Bereitstellen des Zugriffs auf Tools und Fotobearbeitungsoptionen verwendet. Da alle Befehle in der Befehlsleiste enthalten sind, kann die App den Großteil des Platzes auf dem Bildschirm dem Content widmen, also dem Foto, das bearbeitet wird.

![Beispiel für den Entwurf einer Fotobearbeitungs-App, die eine aktive Canvas verwendet](images/photo-editor/uap-photo-tabletphone-sbs.png)

Allgemeine Empfehlungen für erstellungsorientierte Apps:

-   Minimieren Sie die Verwendung von [Navigationselementen](navigation-basics.md).
-   [Befehlselemente](commanding-basics.md) sind insbesondere bei erstellungsorientierten Apps wichtig. Da Benutzer zahlreiche Befehle ausführen, empfehlen wir die Bereitstellung eines Befehlsverlaufs/einer Rückgängig-Funktion.

## <span id="Apps_with_interactive_content"></span><span id="apps_with_interactive_content"></span><span id="APPS_WITH_INTERACTIVE_CONTENT"></span>Apps mit interaktivem Content


In einer App mit interaktivem Content erstellen die Benutzer Content, zeigen ihn an und bearbeiten ihn. Viele Apps passen in diese Kategorie. Zu den Beispielen dieser Typen von Apps gehören Branchen-Apps, Bestandsverwaltungs-Apps, Koch-Apps, die dem Benutzer des Erstellen oder Bearbeiten von Rezepten ermöglichen.

![Ein Design für ein Zusammenarbeitstool, eine App mit interaktivem Content](images/collaboration-tool/uap-collaboration-tabphone-700.png)

Diese Art Apps muss ein Gleichgewicht aller drei UI-Elemente enthalten:

-   [Navigationselemente](navigation-basics.md) helfen Benutzern beim Suchen und Anzeigen von Content. Falls Sie Content anzeigen und suchen, ist das wichtigste Szenario, Navigationselemente zu priorisieren, zu filtern, zu sortieren und zu suchen.
-   [Befehlselemente](commanding-basics.md) lassen den Benutzer Content erstellen und bearbeiten.

Allgemeine Empfehlungen für Apps mit interaktivem Content:

-   Es kann schwierig sein, ein Gleichgewicht aus Navigations-, Content- und Befehlselementen zu finden, wenn alle drei wichtig sind. Stellen Sie möglichst separate Bildschirme zum Durchsuchen, Erstellen und Bearbeiten von Content oder zum Wechseln des Modus bereit.

## <span id="Commonly_used_content_elements"></span><span id="commonly_used_content_elements"></span><span id="COMMONLY_USED_CONTENT_ELEMENTS"></span>Häufig verwendete Content-Elemente


Häufig zum Anzeigen von Content verwendete UI-Elemente. (Eine vollständige Liste der UI-Elemente finden Sie unter [Steuerelemente und Muster](https://msdn.microsoft.com/library/windows/apps/dn611856).)

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Kategorie</th>
<th align="left">Elemente</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Audio und Video</td>
<td align="left">[Steuerelemente für Medienwiedergabe und -transport](../controls-and-patterns/media-playback.md)</td>
<td align="left">Gibt Audio- und Videoinhalte wieder.</td>
</tr>
<tr class="even">
<td align="left">Bild-Viewer</td>
<td align="left">[Flip-Ansicht](../controls-and-patterns/flipview.md), [Bild](../controls-and-patterns/images-imagebrushes.md)</td>
<td align="left">Zeigt Bilder an. Die „Flip-Ansicht“ zeigt Bilder nacheinander in einer Sammlung an, wie etwa Fotos in einem Album oder Elemente auf einer Produktdetailseite.</td>
</tr>
<tr class="odd">
<td align="left">Listen</td>
<td align="left">[Dropdownliste, Listenfeld, Listen- und Rasteransicht](../controls-and-patterns/lists.md)</td>
<td align="left">Stellt Elemente in einer interaktiven Liste oder einem Raster dar. Verwenden Sie diese Elemente, um Benutzern die Auswahl eines Films aus einer Liste mit Neuerscheinungen oder die Verwaltung von Inventar zu ermöglichen.</td>
</tr>
<tr class="even">
<td align="left">Text und Texteingabe</td>
<td align="left"><p>[Textblock](../controls-and-patterns/text-block.md), [Textfeld](../controls-and-patterns/text-box.md), [Rich-Edit-Feld](../controls-and-patterns/rich-edit-box.md)</p>
</td>
<td align="left">Zeigt Text an. Einige Elemente ermöglichen dem Benutzer das Bearbeiten von Text. Weitere Informationen finden Sie unter [Textsteuerelemente](../controls-and-patterns/text-controls.md)</td>
</tr>
</tbody>
</table>



 

 







<!--HONumber=Aug16_HO3-->


