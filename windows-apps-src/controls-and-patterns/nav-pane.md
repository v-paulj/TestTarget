---
author: Jwmsft
Description: Stellt die Navigation auf oberster Ebene bereit und spart gleichzeitig Platz auf dem Bildschirm.
title: "Richtlinien für Navigationsbereiche"
ms.assetid: 8FB52F5E-8E72-4604-9222-0B0EC6A97541
label: Nav pane
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 4adf3cc8425bba467a8a379a9680360c4e0e3037

---
# Navigationsbereiche

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Ein Navigationsbereich ist ein Muster, das trotz Einsparung des Bildschirmbereichs viele Navigationselemente auf oberster Ebene ermöglicht. Der Navigationsbereich wird häufig für mobile Apps verwendet, eignet sich aber auch gut für größere Bildschirme. Wenn der Bereich als eine Überlagerung verwendet wird, bleibt er reduziert und damit im Hintergrund, bis der Benutzer die Schaltfläche drückt; dies eignet sich gut für kleinere Bildschirme. Wenn der Bereich im angedockten Modus verwendet wird, bleibt er geöffnet. Dadurch kann er bei ausreichend vorhandenem Platz auf dem Bildschirm besser genutzt werden.

![Beispiel für einen Navigationsbereich](images/navHero.png)

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn864360"><strong>SplitView-Klasse</strong></a></li>
</ul>

</div>
</div>






## Ist dies das richtige Muster?

Der Navigationsbereich eignet sich gut für:

-   Apps mit vielen Navigationselementen auf oberster Ebene, die einen ähnlichen Typ haben. Dies kann beispielsweise eine Sport-App mit Kategorien wie Football, Baseball, Basketball, Fußball usw. sein.
-   Bereitstellen einer konsistenten Navigationsumgebungen über verschiedene Apps hinweg. Der Navigationsbereich sollte nur Navigationselemente, keine Aktionen enthalten.
-   Eine mittlere bis hohe Zahl (5-10+) an Navigationskategorien der obersten Ebene.
-   Sparen von Platz auf dem Bildschirm (als Overlay).
-   Navigationselemente, auf die selten zugegriffen wird. (als Overlay).

## Erstellen eines Navigationsbereichs

Das Navigationsbereichsmuster besteht aus einem Bereich für die Navigationskategorien, einem Inhaltsbereich und einer optionalen Schaltfläche zum Öffnen oder schließen des Bereichs. Am einfachsten erstellen Sie einen Navigationsleistenbereich mit einem [Steuerelement für die geteilte Ansicht](split-view.md). Diese verfügt über einen leeren Bereich und einen Inhaltsbereich, der stets angezeigt wird.

Um Code zu testen, der dieses Muster implementiert, laden Sie die [XAML-Navigationslösung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlNavigation) von GitHub herunter.


### Bereich

Überschriften für Navigationskategorien gehören in den Bereich. Einstiegspunkte für App-Einstellungen und Kontoverwaltung werden ebenfalls im Bereich platziert, wenn zutreffend. Die Navigationsüberschriften bilden in der Regel eine Liste von Elementen, aus denen der Benutzer auswählen kann.

![Beispiel für den Bereich des Navigationsbereichs](images/nav_pane_expanded.png)

### Inhaltsbereich

Im Inhaltsbereich werden Informationen für den ausgewählten Navigationsspeicherort angezeigt. Er kann einzelne Elemente oder andere untergeordnete Navigationselemente enthalten.

### Schaltfläche

Wenn vorhanden, ermöglicht die Schaltfläche den Benutzern, den Bereich zu öffnen und zu schließen. Die Schaltfläche wird in einer festen Position angezeigt und wird nicht mit dem Bereich verschoben. Es wird empfohlen, die Schaltfläche in der oberen linken Ecke der App zu platzieren. Die Schaltfläche des Navigationsbereichs wird standardmäßig in drei horizontalen Linien angeordnet und häufig als „Hamburger“-Schaltfläche bezeichnet.

![Beispiel für die Schaltfläche des Navigationsbereichs](images/nav_button.png)

Die Schaltfläche wird in der Regel einer Textzeichenfolge zugeordnet. Auf der obersten Ebene der App kann der App-Titel neben der Schaltfläche angezeigt werden. Auf niedrigeren Ebenen der App kann die Textzeichenfolge der Titel oder die Seite sein, die dem Benutzer gerade angezeigt wird.

## Navigationsbereichsvarianten

Der Navigationsbereich verfügt über drei Modi: überlagert, kompakt und inline. Eine Überlagerung wird je nach Bedarf reduziert und erweitert. Im kompakten Modus wird der Bereich stets als schmaler Streifen angezeigt, der erweitert werden kann. Ein Bereich im Inlinemodus bleibt standardmäßig geöffnet.

### Überlagerung

-   Eine Überlagerung kann für jede Bildschirmgröße und im Hoch- oder Querformat verwendet werden. Im Standardzustand (minimiert) beansprucht die Überlagerung keinen Platz auf dem Bildschirm, da nur die Schaltfläche angezeigt wird.
-   Bietet ein On-Demand-Navigationssystem, das Platz auf dem Bildschirm spart. Ideal für Apps auf Smartphones und Tablets
-   Der Bereich ist standardmäßig ausgeblendet, nur die Schaltfläche ist sichtbar.
-   Durch Drücken der Schaltfläche des Navigationsbereichs wird der Bereich geöffnet und geschlossen.
-   Der erweiterte Zustand ist flüchtig und wird beendet, wenn eine Auswahl getroffen wird, wenn die Zurück-Schaltfläche verwendet wird, oder wenn der Benutzer außerhalb des Bereichs tippt.
-   Die Überlagerung wird im Vordergrund des Inhalts gezeichnet und formatiert den Inhalt nicht neu.

### Kompakt

-   Der kompakte Modus kann als `CompactOverlay` angegeben werden und überlagert dann Inhalte, wenn er geöffnet ist, oder als `CompactInline`, der dann Inhalte verschiebt. Wir empfehlen die Verwendung von CompactOverlay.
-   Kompakte Bereiche zeigen die ausgewählte Position an, während sie nur eine kleine Menge des Bildschirminventars belegen.
-   Dieser Modus ist besser für mittelgroße Bildschirme wie Tablets geeignet.
-   Standardmäßig wird der Bereich geschlossen, sodass nur Navigationssymbole und die Schaltfläche angezeigt werden.
-   Durch Klicken auf die Schaltfläche des Navigationsbereichs wird der Bereich geöffnet und geschlossen. Das Verhalten entspricht dem Überlagerungs- oder Inlinemodus, abhängig vom angegebenen Anzeigemodus.
-   Die Auswahl sollte für die Listensymbole angezeigt werden, um hervorzuheben, wo sich der Benutzer in der Navigationsstruktur befindet.

### Inline

-   Der Navigationsbereich bleibt geöffnet. Dieser Modus ist besser für größere Bildschirme geeignet.
-   Er unterstützt Drag & Drop-Szenarien in und aus dem Bereich.
-   Die Schaltfläche des Navigationsbereichs ist für diesen Zustand nicht erforderlich. Wenn die Schaltfläche verwendet wird, wird der Inhaltsbereich nach außen verschoben, und der Inhalt in diesem Bereich wird neu angeordnet.
-   Die Auswahl sollte für die Listenelemente angezeigt werden, um hervorzuheben, wo sich der Benutzer in der Navigationsstruktur befindet.

## Anpassungsfähigkeit

Um die Anwendbarkeit einer Vielzahl von Geräten zu maximieren, wird die Nutzung von [Haltepunkten](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) und die Anpassung des Navigationsbereichmodus an die Breite des App-Fensters empfohlen.
-   Kleines Fenster
   -   Kleiner als oder gleich 640 Pixeln.
   -   Der Navigationsbereich sollte den Überlagerungsmodus verwenden, standardmäßig geschlossen.
-   Mittelgroßes Fenster
   -   Größer als 640 Pixel mit einer Breite, die kleiner als oder gleich 1007Pixeln ist.
   -   Der Navigationsbereich sollte den Streifenmodus verwenden, standardmäßig geschlossen.
-   Großes Fenster
   -   Breite größer als 1007Pixel.
   -   Der Navigationsbereich sollte den Andockmodus verwenden, standardmäßig geöffnet.

## Anpassung

Um die [10-Fuß-Erfahrung](http://go.microsoft.com/fwlink/?LinkId=760736) Ihrer App zu optimieren, sollten Sie die Anpassung des Navigationsbereichs durch Ändern der visuellen Darstellung der Navigationselemente in Betracht ziehen. Je nach Interaktionskontext ist es möglicherweise wichtiger, die Aufmerksamkeit des Benutzers auf das ausgewählte Navigationselement oder das fokussierte Navigationselement zu ziehen. Im Fall von 10-Fuß-Umgebungen, in denen Gamepads die häufigsten Eingabegeräte sind, ist es besonders wichtig, sicherzustellen, dass der Benutzer die Position des zurzeit fokussierten Element auf dem Bildschirm verfolgen kann.

![Beispiel für angepasste Navigationselemente](images/nav_item_states.png)

## Verwandte Themen

* [Steuerelement für geteilte Ansicht](split-view.md)
* [Master/Details](master-details.md)
* [Navigationsgrundlagen](https://msdn.microsoft.com/library/windows/apps/dn958438)
 

 



<!--HONumber=Aug16_HO3-->


