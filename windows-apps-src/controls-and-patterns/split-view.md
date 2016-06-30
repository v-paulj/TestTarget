---
author: Jwmsft
title: Geteilte Darstellung
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "Ein Steuerelement für die geteilte Darstellung verfügt über einen erweiterbaren/reduzierbaren Bereich und einen Inhaltsbereich."
label: Split view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 391bfdbbf09474ad707dbbf306d4997825fa8386

---

# Richtlinien für das SplitView -Steuerelement



**Wichtige APIs**

-   [**SplitView-Klasse (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**SplitView-Objekt (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

Ein Steuerelement für die geteilte Ansicht verfügt über einen erweiterbaren/reduzierbaren Bereich und einen Inhaltsbereich. Der Inhaltsbereich ist immer sichtbar. Der Bereich kann erweitert und reduziert werden oder geöffnet bleiben und kann vom linken oder rechten Rand eines App-Fensters eingeblendet werden. Der Bereich verfügt über vier Modi:

-   **Überlagerung**

    Der Bereich ist ausgeblendet, bis er geöffnet wird. Ist der Bereich geöffnet, überlagert er den Inhaltsbereich.

-   **Inline**

    Der Bereich ist immer sichtbar und überlagert den Inhaltsbereich nicht. Der Bereich und der Inhaltsbereich teilen sich die verfügbare Bildschirmfläche.

-   **CompactOverlay**

    Ein kleiner Teil des Bereich – gerade breit genug für die Anzeige von Symbolen – ist in diesem Modus immer sichtbar. Die Standardbreite für den geschlossen Bereich ist 48px und kann mit `CompactPaneLength` geändert werden. Wenn das Fenster geöffnet ist, wird den Inhaltsbereich überlagert werden.

-   **CompactInline**

    Ein kleiner Teil des Bereich – gerade breit genug für die Anzeige von Symbolen – ist in diesem Modus immer sichtbar. Die Standardbreite für den geschlossen Bereich ist 48px und kann mit `CompactPaneLength` geändert werden. Wenn das Fenster geöffnet ist, reduziert es den Platz für Inhalte, die weggeschoben werden.

## <span id="Is_this_the_right_control_"></span><span id="is_this_the_right_control_"></span><span id="IS_THIS_THE_RIGHT_CONTROL_"></span>Ist dies das richtige Steuerelement?

Das Steuerelement für die geteilte Darstellung kann zum Erstellen eines [Navigationsbereichs](nav-pane.md) verwendet werden. Zum Erstellen dieses Musters fügen Sie eine Schaltfläche zum Erweitern/Reduzieren (die „Hamburger“-Schaltfläche) sowie eine Listenansicht mit den Navigationselementen hinzu.

Das Steuerelement für die geteilte Darstellung kann auch für „Schubladen“-Funktionalität (Benutzer können den zusätzlichen Bereich öffnen und schließen) verwendet werden.

## <span id="Examples"></span><span id="examples"></span><span id="EXAMPLES"></span>Beispiele

Das Steuerelement für die geteilte Darstellung ist in seiner standardmäßigen Form ein Basiscontainer. Hier ein Beispiel der Microsoft Edge-App, in dem SplitView verwendet wird, um den Hub anzuzeigen.

![Beispiel für die geteilte Darstellung mit Microsoft Edge](images/split_view_Edge.png)



## <span id="related_topics"></span>Verwandte Themen


* [Navigationsbereichsmuster](nav-pane.md)
* [Listenansicht](lists.md)
 

 



<!--HONumber=Jun16_HO4-->


