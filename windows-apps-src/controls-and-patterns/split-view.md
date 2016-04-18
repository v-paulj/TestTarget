---
title: Geteilte Ansicht
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: Ein Steuerelement für die geteilte Ansicht verfügt über einen erweiterbaren/reduzierbaren Bereich und einen Inhaltsbereich.
label: Geteilte Ansicht
template: detail.hbs
---

# Richtlinien für das Steuerelement für die geteilte Ansicht


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**SplitView-Klasse (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**SplitView-Objekt (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

Ein Steuerelement für die geteilte Ansicht verfügt über einen erweiterbaren/reduzierbaren Bereich und einen Inhaltsbereich. Der Inhaltsbereich ist immer sichtbar. Der Bereich kann erweitert und reduziert werden oder geöffnet bleiben und kann vom linken oder rechten Rand eines App-Fensters eingeblendet werden. Der Bereich verfügt über drei Modi:

-   **Überlagerung**

    Der Bereich ist ausgeblendet, bis er geöffnet wird. Ist der Bereich geöffnet, überlagert er den Inhaltsbereich.

-   **Inline**

    Der Bereich ist immer sichtbar und überlagert den Inhaltsbereich nicht. Der Bereich und der Inhaltsbereich teilen sich die verfügbare Bildschirmfläche.

-   **Kompakt**

    Der Bereich ist in diesem Modus immer sichtbar. Diese Ansicht ist gerade breit genug für die Anzeige von Symbolen (in der Regel 48 epx breit). Der Bereich und der Inhaltsbereich teilen sich die verfügbare Bildschirmfläche. Obwohl der standardmäßige kompakte Modus den Inhaltsbereich nicht überlagert kann er in einen breiteren Bereich umgewandelt werden, um mehr Inhalt anzuzeigen. Dadurch wird der Inhaltsbereich überlagert.

## <span id="Is_this_the_right_control_"></span><span id="is_this_the_right_control_"></span><span id="IS_THIS_THE_RIGHT_CONTROL_"></span>Ist dies das richtige Steuerelement?


Das Steuerelement für die geteilte Ansicht kann zum Erstellen eines [Navigationsbereichsmusters](nav-pane.md) verwendet werden. Zum Erstellen dieses Musters fügen Sie dem Steuerelement für die geteilte Ansicht eine Schaltfläche zum Erweitern/Reduzieren (die „Hamburger“-Schaltfläche) und eine Listenansicht hinzu.

## <span id="Examples"></span><span id="examples"></span><span id="EXAMPLES"></span>Beispiele


Das Steuerelement für die geteilte Ansicht in seiner standardmäßigen Form ist ein grundlegender Container. Werden eine Schaltfläche und eine Listenansicht hinzugefügt, ist das Steuerelement für die geteilte Ansicht bereit zur Verwendung als Navigationsmenü. Hier sehen Sie Beispiele des Steuerelements für die geteilte Ansicht als Navigationsmenü im erweiterten und kompakten Modus.

![Beispiel für das Menü einer geteilten Ansicht im Überlagerungsmodus und im kompakten Modus](images/controls-splitview-menu01.png)
## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Empfehlungen


-   Bei Verwendung der geteilten Ansicht für ein Navigationsmenü wird empfohlen, Navigationssteuerelemente, die Zugriff auf andere Bereiche der App bieten, im Bereich zu platzieren. Das Verwenden des Bereichs für die Navigation ermöglicht eine einheitliche Benutzererfahrung. Diese Menüimplementierung hilft Benutzern darüber hinaus, einen Überblick über die Bestandteile der App zu gewinnen, sie kann einen Schnellzugriff auf die Startseite der Apps bieten und Benutzer motivieren, weitere Bereiche der App genauer zu erkunden.

\[Dieser Artikel enthält spezielle Informationen zu Apps für die universelle Windows-Plattform (UWP) und Windows 10. Laden Sie für Windows 8.1 die [PDF-Datei mit Windows 8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]

## <span id="related_topics"></span>Verwandte Themen


* [Navigationsbereichsmuster](nav-pane.md)
* [Listenansicht](lists.md)
 

 






<!--HONumber=Mar16_HO1-->


