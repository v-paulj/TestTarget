---
description: Das Hub-Steuerelement verwendet ein hierarchisches Navigationsmuster, um Apps zu unterstützen, die eine relationale Informationsarchitektur erfordern.
title: Hub-Steuerelemente
ms.assetid: F1319960-63C6-4A8B-8DA1-451D59A01AC2
label: Hub
template: detail.hbs
---
# Hub-Steuerelement/-Muster


Mit einem Hub-Steuerelement können Sie App-Inhalte in unterschiedlichen, aber zusammenhängenden Abschnitten oder Kategorien organisieren. Abschnitte in einem Hub sind für das Durchlaufen in einer bevorzugten Reihenfolge vorgesehen und können als Ausgangspunkt für den Zugriff auf detaillierte Inhalte dienen.

![Beispiel für einen Hub](images/hub_example_tablet.png)

Inhalte in einem Hub können in einer stabilen horizontal verschiebbaren Ansicht dargestellt werden, die Benutzern einen schnellen Überblick über Neuigkeiten sowie verfügbare und wichtige Inhalte bietet. Hubs verfügen in der Regel über eine Kopfzeile, während mehrere Inhaltsabschnitte jeweils eine Abschnittsüberschrift aufweisen.

Das Hub-Steuerelement verfügt über verschiedene Funktionen, durch die es gut für die Erstellung eines Inhaltsnavigationsmustern geeignet ist.

-   **Visuelle Navigation**

    Ein Hub ermöglicht das Anzeigen von Inhalten in einem vielseitigen, kurzen, übersichtlichen Array.

-   **Kategorisierung**

    Jeder Hub-Abschnitt ermöglicht die Anordnung seiner Inhalte in einer logischen Reihenfolge.

-   **Gemischte Inhaltstypen**

    Bei gemischten Inhaltstypen sind variable Ressourcengrößen und -verhältnisse üblich. Ein Hub ermöglicht das individuelle und feinsäuberliche Anordnen jedes Inhaltstyps in den einzelnen Hub-Abschnitten.

-   **Variable Breite für Seiten und Inhalte**

    Da es sich um ein Panoramamodell handelt, ermöglicht der Hub variable Abschnittsbreiten. Dadurch eignet sich das Modell ideal für Inhalte unterschiedlicher Tiefen und ermöglicht die konsistente Formatierung weniger, aber auch zahlreicher Elemente.

-   **Flexible Architektur**

    Wenn Sie Ihre flache App-Architektur beibehalten möchten, können Sie alle Kanalinhalte in eine Hub-Abschnittszusammenfassung einfügen.

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**Hub-Klasse (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn251843)
-   [**HubSection-Klasse (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn251845)
-   [**Hub-Objekt (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn255137)


## Ist dies das richtige Steuerelement?

Das Hub-Steuerelement eignet sich gut zum Anzeigen großer Mengen von Inhalten, die in einer Hierarchie angeordnet sind. Hubs wenden beim Durchsuchen und Entdecken neuer Inhalte eine Priorität an und sind daher nützlich zum Anzeigen von Elementen in einem Store oder einer Mediensammlung.

Ein Hub ist nur eines von mehreren Navigationselementen, die Ihnen zur Verfügung stehen. Weitere Informationen zu Navigationsmustern und anderen Navigationselementen finden Sie unter [Navigationsdesigngrundlagen für UWP-Apps (Universelle Windows-Plattform)](https://msdn.microsoft.com/library/windows/apps/dn958438).

## Hub-Architektur

Das Hub-Steuerelement verfügt über ein hierarchisches Navigationsmuster zur Unterstützung von Apps, die eine relationale Informationsarchitektur erfordern. Ein Hub besteht aus verschiedenen Inhaltskategorien, die den einzelnen Bereichsseiten der App zugeordnet sind. Bereichsseiten können in jeder beliebigen Form dargestellt werden, die dem jeweiligen Szenario und den Inhalten des Bereichs am besten entspricht.

![Drahtmodell der hierarchischen App "Essen mit Freunden"](images/navigation_diagram_food_with_friends_app_new.png)

## Layouts und Schwenken/Bildlauf

Es gibt eine Reihe von Möglichkeiten zum Erstellens eines Layouts und zum Navigieren von Inhalten in einem Hub, stellen Sie nur sicher, dass Inhaltslisten in einem Hub immer senkrecht zur Richtung schwenken, in die der Bildlauf des Hubs erfolgt.

**Horizontale Verschiebung**

![Beispiel für einen horizontal verschobenen Hub](images/controls_hub_horizontal_pan.png)
**Vertikale Verschiebung**

![Beispiel für einen vertikal verschobenen Hub](images/controls_hub_vertical_pan.png)
**Horizontale Verschiebung mit vertikalem Bildlauf der Liste/des Rasters**

![Beispiel für einen horizontal verschobenen Hub mit einem vertikalen Bildlauf der Liste](images/controls_hub_horizontal_vertical_scroll.png)
**Vertikale Verschiebung mit horizontalem Bildlauf der Liste/des Rasters**

![Beispiel für einen horizontal verschobenen Hub](images/controls_hub_vertical_horizontal_scroll.png)

## Beispiele

Der Hub bietet eine hohe Flexibilität beim Entwerfen. Dadurch können Sie Apps mit einer großen Auswahl an attraktiven und visuell komplexen UI-Elementen entwerfen. Sie können für die erste Gruppe ein Favoritenbild oder einen Inhaltsabschnitt verwenden. Ein großes Favoritenbild kann sowohl vertikal als auch horizontal zugeschnitten werden, ohne dass der Interessenschwerpunkt verloren geht. Hier sehen Sie ein Beispiel für ein einzelnes Favoritenbild und für seinen Zuschnitt in das Quer- und Hochformat und auf eine schmale Breite.

![zugeschnittenes Favoritenbild für unterschiedliche Fenstergrößen](images/hub_hero_cropped2.png)

Auf mobilen Geräten ist jeweils ein Hub-Abschnitt sichtbar.

![Beispiel für ein Hubmuster auf einem kleinen Bildschirm](images/phone_hub_example.png)

## Empfehlungen

-   Wir empfehlen, den Inhalt zu beschneiden, sodass ein bestimmter Teil davon verkürzt eingeblendet wird, um Benutzern mitzuteilen, dass weitere Inhalte in einem Hub-Abschnitt vorhanden sind.
-   Je nach den Anforderungen Ihrer App können Sie dem Hub-Steuerelement mehrere Hub-Abschnitte hinzufügen, wobei jedes Steuerelement eine bestimmte Funktion erfüllt. Ein Abschnitt kann beispielsweise eine Reihe von Links und Steuerelementen enthalten, während ein anderer Abschnitt als Repository für Miniaturansichten dient. Benutzer können zwischen diesen Abschnitten mit der Geste wechseln, die von dem Hub-Steuerelement unterstützt wird.
-   Das dynamische Umbrechen von Inhalt ist die beste Methode, um verschiedene Fenstergrößen einzuschließen.
-   Wenn Sie viele Hub-Abschnitte haben, sollten Sie das Hinzufügen des semantischen Zooms in Erwägung ziehen. Dadurch können die Abschnitte auch leichter gefunden werden, wenn die App-Größe in eine schmale Breite geändert wird.
-   Es wird empfohlen, dass ein Element in einem Hub-Abschnitt nicht zu einem anderen Hub führt. Stattdessen können Sie interaktive Überschriften für die Navigation zu einem anderen Hub-Abschnitt oder einer anderen Seite verwenden.
-   Der Hub dient als Ausgangspunkt und sollte an die Anforderungen Ihrer App angepasst werden. Sie können die folgenden Aspekte eines Hubs ändern:
    -   Anzahl Abschnitte
    -   Inhaltstyp in den einzelnen Abschnitten
    -   Positionierung und Reihenfolge von Abschnitten
    -   Größe von Abschnitten
    -   Abstand zwischen Abschnitten
    -   Abstand zwischen einem Abschnitt und dem oberen oder unteren Rand des Hubs
    -   Textstil und -größe in Überschriften und Inhalt
    -   Farbe des Hintergrunds, der Abschnitte, Abschnittsüberschriften und Abschnittsinhalte

\[Dieser Artikel enthält spezielle Informationen zu UWP-Apps und Windows 10. Laden Sie für Windows 8.1 die [PDF-Datei mit Windows 8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]

## Verwandte Artikel
-----------------------------------------------

**Für Designer**
- [Navigationsgrundlagen](https://msdn.microsoft.com/library/windows/apps/dn958438)

**Für Entwickler (XAML)**
- [Hierarchisches Navigationsmuster von A bis Z](https://msdn.microsoft.com/library/windows/apps/xaml/dn440585)
- [**Hub-Klasse „Windows.UI.Xaml.Controls“**](https://msdn.microsoft.com/library/windows/apps/dn251843)
- [Beispiel für ein XAML-Hub-Steuerelement](http://go.microsoft.com/fwlink/p/?LinkID=310072)
- [Verwenden von Hubs](https://msdn.microsoft.com/library/windows/apps/xaml/dn308518)


<!--HONumber=Mar16_HO1-->

