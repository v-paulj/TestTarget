---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Mit dem Microsoft Store Engagement and Monetization SDK haben Sie mehrere Möglichkeiten zur Monetarisierung Ihrer App mit Anzeigen.
title: Anzeigen von Werbung in Ihrer App
---

# Anzeigen von Werbung in Ihrer App


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Mit dem [Microsoft Store Engagement and Monetization SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) haben Sie mehrere Möglichkeiten zur Monetarisierung Ihrer App mit Anzeigen.

## Anzeigen von Bannern und Video-Interstitialanzeigen über die Microsoft Advertising-Bibliotheken

Steigern Sie Ihre Einnahmen mit Windows-Apps, indem Sie Banner und Video-Interstitialanzeigen einbinden. Die Anzeigen werden in Windows-Apps für PCs, Tablets und Smartphones angezeigt. Sie können die Performance der Werbung mithilfe des Windows Dev Center-Dashboards in Echtzeit verfolgen.

Um Werbung in Ihre Apps einzufügen, verwenden Sie die Steuerelemente **AdControl** und **InterstitialAd** in den Advertising-Bibliotheken, die im Microsoft Store Engagement and Monetization SDK verteilt werden. Anhand dieser Steuerelemente können Sie Banner und Video-Interstitialanzeigen von Microsoft in Ihren XAML- oder JavaScript-/HTML-Apps für Windows 10, Windows 8.1, Windows Phone 8.1 und Windows Phone 8 anzeigen.

Weitere Informationen finden Sie unter [Anzeigen von Werbung mithilfe der Microsoft Advertising-Bibliotheken](display-ads-using-the-microsoft-advertising-libraries.md). Verwenden Sie nach der Veröffentlichung einer App mit Werbung den [Bericht zur Anzeigen-Performance](../publish/advertising-performance-report.md), um die Leistung der Anzeigen nachzuverfolgen.                                           

## Verwenden der Anzeigenvermittlung für Werbebanner aus mehreren Anzeigennetzwerken

Sie können die **AdMediatorControl**-Klasse in Ihren XAML-Apps verwenden, um Ihren Umsatz aus Werbung zu optimieren, indem Sie Werbebanner aus mehreren Anzeigennetzwerken anzeigen. Nachdem Sie Ihrer App dieses Steuerelement hinzugefügt haben, konfigurieren Sie Ihre Anzeigenvermittlung im Windows Dev Center-Dashboard, und wir kümmern uns um die Vermittlung von Banneranzeigenanforderungen mehrerer Anzeigennetzwerke, die Sie auswählen. Weitere Informationen finden Sie unter [Maximieren des Anzeigenumsatzes mit einer Anzeigenvermittlung](use-ad-mediation-to-maximize-revenue.md).

## Unterschiede zwischen den Microsoft Advertising-Bibliotheken und Anzeigenvermittlung

Das Microsoft Store Engagement and Monetization SDK enthält die Bibliotheken für Microsoft Advertising und Anzeigenvermittlung. Diese Bibliotheken bieten allerdings verschiedene Klassen und dienen unterschiedlichen Zwecken.

* Verwenden Sie die Klassen **AdControl** und **InterstitialAd** aus den Microsoft Advertising-Bibliotheken, wenn Sie Banner oder Video-Interstitialanzeigen in einer XAML- oder JavaScript-App anzeigen möchten.
* Verwenden Sie die **AdMediatorControl**-Klasse in den Bibliotheken der Anzeigenvermittlung, wenn Werbebanner aus mehreren Anzeigennetzwerken in einer XAML-App angezeigt werden sollen.

Weitere Informationen finden Sie unter [Was ist der Unterschied zwischen AdMediatorControl und AdControl?](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

## Verwandte Themen

* [Microsoft Store Engagement and Monetization SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)
* [Monetisierung Ihrer App mit Werbeanzeigen]( http://go.microsoft.com/fwlink/p/?LinkId=699559)


<!--HONumber=May16_HO2-->


