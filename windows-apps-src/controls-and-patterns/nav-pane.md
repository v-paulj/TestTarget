---
Description: Stellt die Navigation auf oberster Ebene bereit und spart gleichzeitig Platz auf dem Bildschirm.
title: Richtlinien für Navigationsbereiche
ms.assetid: 8FB52F5E-8E72-4604-9222-0B0EC6A97541
label: Nav pane
template: detail.hbs
---

Navigationsbereiche
=============================================================================================
Ein Navigationsbereich ist ein Muster, das trotz Einsparung des Bildschirmbereichs viele Navigationselemente auf oberster Ebene ermöglicht. Der Navigationsbereich wird häufig für mobile Apps verwendet, eignet sich aber auch gut für größere Bildschirme. Wenn der Bereich als eine Überlagerung verwendet wird, bleibt er reduziert und damit im Hintergrund, bis der Benutzer die Schaltfläche drückt; dies eignet sich gut für kleinere Bildschirme. Wenn der Bereich im angedockten Modus verwendet wird, bleibt er geöffnet. Dadurch kann er bei ausreichend vorhandenem Platz auf dem Bildschirm besser genutzt werden.

![Beispiel für einen Navigationsbereich](images/NAV_PANE_EXAMPLE.png)

<span class="sidebar_heading" style="font-weight: bold;">Wichtige APIs</span>

-   [**SplitView-Klasse (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**SplitView-Objekt (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

<<<<<<< HEAD

=======

>>>>>>> Ursprung

<span id="Is_this_the_right_pattern_"></span><span id="is_this_the_right_pattern_"></span><span id="IS_THIS_THE_RIGHT_PATTERN_"></span>Ist dies das richtige Muster?
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

Der Navigationsbereich eignet sich gut für:

-   Apps mit vielen Navigationselementen auf oberster Ebene in derselben Klasse, z. B. eine Sport-App mit Kategorien wie Fußball, Handball, Basketball, Eishockey usw.
-   Zum Bereitstellen einer einheitlichen Navigationsumgebung auf allen Apps, vorausgesetzt, dass nur Navigationselemente im Bereich platziert werden.
-   Eine mittlere bis hohe Zahl (5-10+) an Navigations-Kategorien der obersten Ebene.
-   Sparen von Platz auf dem Bildschirm (als Overlay).
-   Navigationselemente, auf die selten zugegriffen wird (als Overlay).
-   Drag & Drop-Szenarien (wenn angedockt).

<span id="Building_a_nav_pane"></span><span id="building_a_nav_pane"></span><span id="BUILDING_A_NAV_PANE"></span>Erstellen eines Navigationsbereichs
-------------------------------------------------------------------------------------------------------------------------------------

Das Navigationsleistenmuster besteht aus einer Schaltfläche, einem Fenster für Navigationskategorien und einem Inhaltsbereich. Am einfachsten erstellen Sie einen Navigationsleistenbereich mit einem [Steuerelement für die geteilte Ansicht](split-view.md). Diese verfügt über einen leeren Bereich und einen Inhaltsbereich, der immer sichtbar ist. Der Bereich kann entweder sichtbar oder ausgeblendet sein und kann vom linken oder rechten Rand eines App-Fensters eingeblendet werden.

Wenn Sie einen Navigationsbereich ohne das Steuerelement für die geteilte Ansicht erstellen möchten, benötigen Sie drei Hauptkomponenten: eine Schaltfläche, einen Bereich und einen Inhaltsbereich. Mit der Schaltfläche können Benutzer den Bereich öffnen und schließen. Der Bereich ist ein Container für Navigationselemente. Der Inhaltsbereich zeigt Informationen aus dem ausgewählten Navigationselement an. Der Navigationsbereich kann auch in einem angedockten Modus vorhanden sein, in dem der Bereich immer angezeigt wird. Eine Schaltfläche wäre in diesem Fall nicht nötig.

### <span id="Button"></span><span id="button"></span><span id="BUTTON"></span>Schaltfläche

Die Schaltfläche des Navigationsbereichs wird standardmäßig in drei horizontalen Linien gestapelt und wird häufig als „Hamburger“-Schaltfläche bezeichnet. Mit der Schaltfläche können Benutzer den Bereich bei Bedarf öffnen und schließen, und sie wird nicht mit dem Bereich bewegt. Es wird empfohlen, die Schaltfläche in der oberen linken Ecke der App zu platzieren. Die Schaltfläche wird nicht mit dem Bereich verschoben.

![Beispiel für die Schaltfläche des Navigationsbereichs](images/NAVPANE_BUTTONONLY.png)

Die Schaltfläche wird in der Regel einer Textzeichenfolge zugeordnet. Auf der obersten Ebene der App kann der App-Titel neben der Schaltfläche angezeigt werden. Auf niedrigeren Ebenen der App kann die Textzeichenfolge der Seite zugeordnet werden, die dem Benutzer gerade angezeigt wird.

### <span id="Pane"></span><span id="pane"></span><span id="PANE"></span>Bereich

Überschriften für Navigationskategorien gehören in den Bereich. Einstiegspunkte für App-Einstellungen und Kontoverwaltung werden ggf. auch im Bereich platziert. Navigationsheader können entweder auf der obersten Ebene platziert oder auf der obersten/zweiten Ebene geschachtelt werden.

![Beispiel für den Bereich des Navigationsbereichs](images/NAVPANE_PANE.png)

### <span id="Content_area"></span><span id="content_area"></span><span id="CONTENT_AREA"></span>Inhaltsbereich

Im Inhaltsbereich werden Informationen für den ausgewählten Navigationsspeicherort angezeigt. Er kann einzelne Elemente oder andere untergeordnete Navigationselemente enthalten.

<span id="Nav_pane_variations"></span><span id="nav_pane_variations"></span><span id="NAV_PANE_VARIATIONS"></span>Navigationsbereichsvarianten
-------------------------------------------------------------------------------------------------------------------------------------

Der Navigationsbereich verfügt über zwei Hauptvarianten: Überlagerungsmodus und angedockter Modus. Eine Überlagerung wird je nach Bedarf reduziert und erweitert. Ein angedocktes Fenster bleibt standardmäßig geöffnet.

### <span id="Overlay"></span><span id="overlay"></span><span id="OVERLAY"></span>Überlagerung

-   Eine Überlagerung kann auf jeder Bildschirmgröße und entweder im Hoch- oder Querformat verwendet werden. Im Standardzustand (minimiert) beansprucht die Überlagerung keinen Platz auf dem Bildschirm, da nur die Schaltfläche angezeigt wird.
-   Bietet ein On-Demand-Navigationssystem, das Platz auf dem Bildschirm spart. Ideal für Apps auf Smartphones und Tablets
-   Der Bereich ist standardmäßig ausgeblendet, nur die Schaltfläche ist sichtbar.
-   Durch Drücken der Schaltfläche des Navigationsbereichs wird der Bereich geöffnet und geschlossen.
-   Der erweiterte Zustand ist flüchtig und wird beendet, wenn eine Auswahl getroffen wird, wenn die Zurück-Schaltfläche verwendet wird, oder wenn der Benutzer außerhalb des Bereichs tippt.
-   Die Überlagerung wird im Vordergrund des Inhalts gezeichnet und formatiert Inhalt nicht um.

### <span id="Docked"></span><span id="docked"></span><span id="DOCKED"></span>Angedockt

-   Der Navigationsbereich bleibt geöffnet. Dieser Modus eignet sich besser für größere Bildschirme, allgemein für Tablets und größere Geräte.
-   Im Querformat beträgt die minimale Bildschirmbreite, in der der angedockte Zustand genutzt werden kann, 720 Epx. Der angedockte Zustand erfordert bei dieser Größe u. U. eine besonders sorgfältige Inhaltsskalierung.
-   Unterstützt Drag & Drop-Szenarien in und aus dem Bereich.
-   Die Schaltfläche des Navigationsbereichs ist für diesen Zustand nicht erforderlich. Wenn die Schaltfläche verwendet wird, wird der Inhaltsbereich nach außen verschoben, und der Inhalt in diesem Bereich wird neu angeordnet.
-   Die Auswahl sollte für die Listenelemente angezeigt werden, um hervorzuheben, wo sich der Benutzer in der Navigationsstruktur befindet.
-   Wenn das Gerät im Hochformat zu schmal für die Anzeige des angedockten Bereichs ist, wird beim Drehen des Geräts das folgende Verhalten empfohlen:
    -   Querformat zu Hochformat. Der Bereich wird entweder in den Überlagerungszustand oder den minimierten Zustand reduziert.
    -   Hochformat zu Querformat. Der Bereich wird erneut angezeigt.

<span id="related_topics"></span>Verwandte Themen
-----------------------------------------------

* [Steuerelement für geteilte Ansicht](split-view.md)
* [Master/Details](master-details.md)
* [Navigationsgrundlagen](https://msdn.microsoft.com/library/windows/apps/dn958438)
 

 


<!--HONumber=Mar16_HO4-->


