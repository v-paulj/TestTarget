---
author: Jwmsft
title: Geteilte Darstellung
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "Ein Steuerelement für die geteilte Darstellung verfügt über einen erweiterbaren/reduzierbaren Bereich und einen Inhaltsbereich."
label: Split view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 7fae1477b997508ade92a5bbb977c1d6530a181f

---
# Steuerelement für geteilte Ansicht

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Ein Steuerelement für die geteilte Darstellung verfügt über einen erweiterbaren/reduzierbaren Bereich und einen Inhaltsbereich.

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn864360"><strong>SplitView-Klasse (XAML)</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn919970"><strong>SplitView-Objekt (HTML)</strong></a></li>
</ul>

</div>
</div>




 Der Inhaltsbereich einer geteilten Ansicht ist immer sichtbar. Der Bereich kann erweitert und reduziert werden oder geöffnet bleiben und kann vom linken oder rechten Rand eines App-Fensters eingeblendet werden. Der Bereich verfügt über vier Modi:

-   **Überlagerung**

    Der Bereich ist ausgeblendet, bis er geöffnet wird. Ist der Bereich geöffnet, überlagert er den Inhaltsbereich.

-   **Inline**

    Der Bereich ist immer sichtbar und überlagert den Inhaltsbereich nicht. Der Bereich und der Inhaltsbereich teilen sich die verfügbare Bildschirmfläche.

-   **CompactOverlay**

    Ein kleiner Teil des Bereich – gerade breit genug für die Anzeige von Symbolen – ist in diesem Modus immer sichtbar. Die Standardbreite für den geschlossen Bereich ist 48px und kann mit `CompactPaneLength` geändert werden. Wenn das Fenster geöffnet ist, wird den Inhaltsbereich überlagert werden.

-   **CompactInline**

    Ein kleiner Teil des Bereich – gerade breit genug für die Anzeige von Symbolen – ist in diesem Modus immer sichtbar. Die Standardbreite für den geschlossen Bereich ist 48px und kann mit `CompactPaneLength` geändert werden. Wenn das Fenster geöffnet ist, reduziert es den Platz für Inhalte, die weggeschoben werden.

## Ist dies das richtige Steuerelement?

Das Steuerelement für die geteilte Darstellung kann zum Erstellen eines [Navigationsbereichs](nav-pane.md) verwendet werden. Zum Erstellen dieses Musters fügen Sie eine Schaltfläche zum Erweitern/Reduzieren (die „Hamburger“-Schaltfläche) sowie eine Listenansicht mit den Navigationselementen hinzu.

Das Steuerelement für die geteilte Darstellung kann auch für „Schubladen“-Funktionalität (Benutzer können den zusätzlichen Bereich öffnen und schließen) verwendet werden.

## Beispiele

Das Steuerelement für die geteilte Darstellung ist in seiner standardmäßigen Form ein Basiscontainer. Hier ein Beispiel der Microsoft Edge-App, in dem SplitView verwendet wird, um den Hub anzuzeigen.

![Beispiel für die geteilte Darstellung mit Microsoft Edge](images/split_view_Edge.png)



## Verwandte Themen


* [Navigationsbereichsmuster](nav-pane.md)
* [Listenansicht](lists.md)
 

 



<!--HONumber=Aug16_HO3-->


