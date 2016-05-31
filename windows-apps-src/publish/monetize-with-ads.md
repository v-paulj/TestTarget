---
author: jnHs
Description: Wenn Ihre App die Anzeigenvermittlung verwendet oder Banner- bzw. Videointerstitialanzeigen aus Microsoft Advertising anzeigt, verwalten Sie die Verwendung Ihrer Anzeigen auf der Seite Monetisierung &gt; Gewinnbringende Nutzung mit Anzeigen.
title: Monetisierung durch Werbeanzeigen
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
---

# Monetisierung durch Werbeanzeigen


Wenn Ihre App ein **AdMediatorControl**-, **AdControl**- oder **InterstitialAd**-Steuerelement zum Anzeigen von Banner- oder Videointerstitialanzeigen verwendet, verwalten Sie die Verwendung Ihrer Anzeigen auf der Seite **Monetisierung** &gt; **Gewinnbringende Nutzung mit Anzeigen**.

## Windows-Anzeigenvermittlung


Wenn Ihre App die Anzeigenvermittlung verwendet, verwenden Sie diesen Abschnitt zum Konfigurieren Ihrer Vermittlungseinstellungen und zum Hinzufügen erforderlicher Parameter für alle von Ihrer App genutzten Anzeigennetzwerke. Weitere Informationen zu den Optionen in diesem Abschnitt finden Sie unter [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](https://msdn.microsoft.com/library/windows/apps/mt219689).

Mithilfe der Anzeigenvermittlung können Sie den Umsatz mit In-App-Werbung optimieren, indem Sie Banneranzeigenanforderungen von mehreren Anzeigennetzwerken vermitteln. Weitere Informationen zur Anzeigenvermittlung finden Sie unter [Maximieren des Umsatzes mit einer Anzeigenvermittlung](https://msdn.microsoft.com/library/windows/apps/mt219691).

## COPPA-Compliance

Um die Einhaltung der Richtlinien des Children's Online Privacy Protection Act („COPPA“) sicherzustellen, müssen Sie Microsoft benachrichtigen, wenn Ihre App an Kinder unter 13 Jahren gerichtet ist. Wenn unter Verwendung von Dev Center Microsoft mitteilen, dass Ihre App an Kinder unter 13 Jahren gerichtet ist, deaktiviert Microsoft die verhaltensorientierten Werbedienste bei der Übermittlung von Werbung an Ihre App. Wenn Ihre App an Kinder unter 13 Jahren gerichtet ist, ergeben sich aus COPPA bestimmte Verpflichtungen für Sie.

Weitere Informationen über Ihre Verpflichtungen im Rahmen von COPPA finden Sie [auf dieser Seite](http://go.microsoft.com/fwlink/p/?linkid=536558).

## Microsoft-Partneranzeigen

Aktivieren Sie das Kontrollkästchen in diesem Abschnitt, wenn Sie Microsoft-Partneranzeigen in Ihrer App anzeigen möchten. Wenn Sie dieses Kontrollkästchen aktivieren, werden für Ihre App Anzeigen für Produkte im Store bereitgestellt, u. a. Musik, Spiele, Filme, Apps, Hardware und Software, wenn keine Anzeigen von anderen Anzeigennetzwerken verfügbar sind. Wenn Benutzer auf die Anzeigen klicken und innerhalb eines bestimmten Attributionsfensters Produkte im Store kaufen, verdienen Sie bei jedem Verkauf eine Provision.

Wenn Sie diese Auswahl ändern, müssen Sie Ihre App nicht neu veröffentlichen, damit die Änderungen wirksam werden. Weitere Informationen zu Microsoft-Partneranzeigen finden Sie unter [Informationen zu Partneranzeigen](about-affiliate-ads.md).

> **Hinweis**  Wenn Ihre App die Anzeigenvermittlung verwendet (d. h. ein **AdMediatorControl**-Steuerelement zum Anzeigen von Werbeanzeigen verwendet), kann Ihre App nur Partneranzeigen anzeigen, wenn die Einstellungen für die Anzeigenvermittlung zum Anzeigen von Werbeanzeigen von Microsoft konfiguriert sind.

## Community-Anzeigen

Aktivieren Sie das Kontrollkästchen in diesem Abschnitt, wenn Sie Ihre App mit Apps anderer Entwickler bewerben möchten. Wenn Sie dieses Kontrollkästchen aktivieren und dann [eine Community-Anzeigenkampagne erstellen](create-an-ad-campaign-for-your-app.md), werden in Ihrer App Werbeanzeigen für Apps anderer Entwickler angezeigt, die auch Community-Anzeigenkampagnen erstellen, und Werbeanzeigen für deren Apps in Ihrer App angezeigt. Community-Anzeigen sind kostenlos und werden nur angezeigt, wenn keine Werbeanzeigen von anderen Anzeigennetzwerken verfügbar sind.

Wenn Sie diese Auswahl ändern, müssen Sie Ihre App nicht neu veröffentlichen, damit die Änderungen wirksam werden. Weitere Informationen zu Community-Anzeigen finden Sie unter [Über Community-Anzeigen](about-community-ads.md).

## Microsoft Advertising-Anzeigeneinheiten

In diesem Abschnitt erstellen Sie eine Microsoft Advertising-Anzeigeneinheit. Anzeigeeinheiten müssen Sie nur in den folgenden Szenarien erstellen:

-   Ihre App zeigt Werbebanner von Microsoft Advertising mithilfe eines [AdControl](https://msdn.microsoft.com/library/mt313154.aspx)-Objekts an.
-   Ihre App zeigt Video-Interstitialanzeigen aus Microsoft Advertising mithilfe eines [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx)-Objekts an.

So erstellen Sie eine Anzeigeneinheit für diese Szenarien:

1.  Der Name der Anzeigeneinheit.
2.  Wählen Sie den Anzeigeneinheitstyp (**Banner** oder **Video-Interstitialanzeige**).
3.  Wählen Sie den Gerätetyp (**Mobilgerät** oder **PC/Tablet**).
4.  Klicken Sie auf **Anzeigeneinheit erstellen**.

Ihre Anzeigeneinheiten werden in einer Tabelle am Ende dieses Abschnitts angezeigt. Für jede Anzeigeneinheit werden eine **Anwendungs-ID** und eine **Anzeigeneinheits-ID** angezeigt. Zum Einblenden von Anzeigen in Ihrer App müssen Sie diese Werte in Ihrem Code verwenden:

-   Wenn Ihre App Werbebanner anzeigt, weisen Sie diese Werte den Eigenschaften [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) und [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) Ihres [AdControl](https://msdn.microsoft.com/library/mt313154.aspx)-Objekts hinzu.
-   Wenn Ihre App Video-Interstitialanzeigen anzeigt, übergeben Sie diese Werte an die [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx)-Methode Ihres [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx)-Objekts.

> **Hinweis**  Wenn Ihre App die Anzeigenvermittlung zum Anzeigen von Werbebannern aus Microsoft Advertising verwendet (d. h. ein **AdMediatorControl**-Objekt verwendet), müssen Sie keine Anzeigeneinheiten anfordern. In diesem Szenario werden Microsoft Advertising-Anzeigeeinheiten automatisch generiert.

 

 

 


<!--HONumber=May16_HO2-->


