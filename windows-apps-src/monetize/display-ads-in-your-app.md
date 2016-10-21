---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Mit dem Microsoft Store Services SDK haben Sie mehrere Möglichkeiten zur Monetarisierung Ihrer App mit Anzeigen."
title: Anzeigen von Werbung in Ihrer App
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 35dfe2864958a15cf01133d6017b7dd03f382e4a

---

# Anzeigen von Werbung in Ihrer App


Die universelle Windows-Plattform (UWP) und Windows Store bieten verschiedene Möglichkeiten zur Monetarisierung Ihrer App mit Anzeigen.

## Anzeigen von Banner- und Video-Interstitialanzeigen über die Microsoft Advertising-Bibliotheken

Steigern Sie dem mit UWP-Apps sowie Apps für Windows8.1 und Windows Phone8.x erzielten Umsatz, indem Sie Banner- und Video-Interstitialanzeigen einbinden. Die Anzeigen werden in Windows-Apps für PCs, Tablets und Smartphones angezeigt. Sie können die Performance der Werbung mithilfe des [Berichts zur Anzeigen-Performance](../publish/advertising-performance-report.md) im Windows Dev Center-Dashboard in Echtzeit verfolgen.

Um diese Art von Anzeigen in Ihre Apps einzubinden, verwenden Sie die Steuerelemente **AdControl** und **InterstitialAd** in den Advertising-Bibliotheken, die mit [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) (für UWP-Apps) und [Microsoft Advertising SDK für Windows und Windows Phone8.x](http://aka.ms/store-8-sdk) (für Windows8.1- und Windows Phone8.x-Apps) verteilt werden.


Die folgenden Themen enthalten Informationen zu allgemeinen Aufgaben im Zusammenhang mit den Windows-Advertising-Bibliotheken.

|  Aufgabe    | Thema |               
|----------|-------|
| Installieren und erste Schritte mit den Microsoft Advertising-Bibliotheken.     | Siehe [Erste Schritte mit Microsoft Advertising-Bibliotheken](get-started-with-microsoft-advertising-libraries.md).        |
| Anzeigen von Werbebannern in Ihrer XAML-/C#-App.     | Siehe [AdControl in XAML und .NET](adcontrol-in-xaml-and--net.md).        |
| Anzeigen von Werbebannern in Ihrer HTML-/JavaScript-App.     | Siehe [AdControl in HTML 5 und Javascript](adcontrol-in-html-5-and-javascript.md).        |
| Anzeigen von Werbebannern in Ihrer Windows Phone Silverlight8.x-App.     | Siehe [„AdControl“ in Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).        |
| Anzeigen einer Video-Interstitialanzeige in Ihrer App.     | Siehe [Interstitialanzeigen](interstitial-ads.md).       |
| Fügen Sie Videoinhalten in einer UWP-App (Universelle Windows-Plattform) Werbung hinzu, die mithilfe von JavaScript in HTML geschrieben wurde.   |  Siehe [Hinzufügen von Werbung zu Videoinhalten in HTML5 und JavaScript](add-advertisements-to-video-content.md).  |
| Herunterladen von Beispielprojekten, die veranschaulichen, wie Sie Banner- und Interstitialwerbung zu Apps hinzufügen.     |Siehe [Anzeigenbeispiele bei GitHub](http://aka.ms/githubads).       |
| Behandeln von [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Fehlern in Ihrer App.     | Siehe [Fehlerbehandlung](error-handling-with-advertising-libraries.md) und die exemplarischen Vorgehensweisen unter [AdControl-Fehlerbehandlung](adcontrol-error-handling.md).       |
| Melden eines Fehlers in den Microsoft Advertising-Bibliotheken.     | Besuchen der [Supportseite](https://go.microsoft.com/fwlink/p/?LinkId=331508).        |
| Erhalten von Community-Support.     | Besuchen des [Forums](http://go.microsoft.com/fwlink/p/?LinkId=401266)       |

                            

## Verwenden der Anzeigenvermittlung für Werbebanner (Windows8.1 und Windows Phone8.x)

Für Windows8.1- und Windows Phone8.x-Apps können Sie die **AdMediatorControl**-Klasse in Ihren XAML-Apps verwenden, um Ihren Umsatz aus Werbung zu optimieren, indem Sie Werbebanner mehrerer Anzeigennetzwerke anzeigen. Nachdem Sie Ihrer App dieses Steuerelement hinzugefügt haben, konfigurieren Sie Ihre Anzeigenvermittlung im Windows Dev Center-Dashboard, und wir kümmern uns um die Vermittlung von Banneranzeigenanforderungen mehrerer Anzeigennetzwerke, die Sie auswählen. Weitere Informationen finden Sie unter [Maximieren des Anzeigenumsatzes mit einer Anzeigenvermittlung](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

>**Hinweis**&nbsp;&nbsp;Die Anzeigenvermittlung mithilfe der **AdMediatorControl**-Klasse wird derzeit für UWP-Apps unter Windows10 nicht unterstützt. Die serverseitige Vermittlung steht demnächst für UWP-Apps zur Verfügung. Dabei werden für Banneranzeigen (**AdControl**) und Video-Interstitialanzeigen (**InterstitialAd**) dieselben APIs verwendet. Eine Anleitung zum Migrieren Ihrer UWP-Apps von **AdMediatorControl** zu **AdControl** finden Sie unter [Migrieren Ihrer UWP-Apps von „AdMediatorControl“ zu „AdControl“](migrate-from-admediatorcontrol-to-adcontrol.md).

<span id="silverlight_support"/>
## Anzeigenunterstützung für Windows Phone8.x Silverlight-Projekte

Einige Entwicklerszenarien werden für Windows Phone8.x Silverlight-Projekte nicht mehr unterstützt. Weitere Informationen finden Sie in der folgenden Tabelle.

|  Plattformversion  |  Vorhandene Projekte    |   Neue Projekte  |
|-----------------|----------------|--------------|
| Windows Phone8.0 Silverlight     |  Wenn Sie über ein vorhandenes Windows Phone8.0 Silverlight-Projekt verfügen, das bereits eine **AdControl** bzw. **AdMediatorControl** einer früheren Version des Universal Ad Client SDK oder Microsoft Advertising SDK verwendet, und die App bereits im Windows Store veröffentlicht wurde, können Sie das Projekt bearbeiten und neu erstellen. Zudem können Sie Ihre Änderungen auf einem Gerät debuggen oder testen. Das Debuggen und Testen des Projekts im Emulator wird nicht unterstützt.  |  Nicht unterstützt  |
| Windows Phone8.1 Silverlight    |  Wenn Sie über ein vorhandenes Windows Phone8.1 Silverlight-Projekt verfügen, das ein **AdControl** oder **AdMediatorControl** einer früheren SDK verwendet, können Sie es ändern und neu erstellen. Zum Debuggen oder Testen der App müssen Sie die App jedoch im Emulator ausführen und für die Anwendungs- und Anzeigeneinheits-ID die [Testmoduswerte](test-mode-values.md) verwenden. Das Debuggen oder Testen der App auf einem Gerät wird nicht unterstützt.  |   Sie können ein **AdControl** oder **AdMediatorControl** zu einem neuen Windows Phone8.1 Silverlight-Projekt hinzufügen. Zum Debuggen oder Testen der App müssen Sie die App jedoch im Emulator ausführen und für die Anwendungs- und Anzeigeneinheits-ID die [Testmoduswerte](test-mode-values.md) verwenden. Das Debuggen oder Testen der App auf einem Gerät wird nicht unterstützt. |

## Verwandte Themen

* [Microsoft Store Services SDK](microsoft-store-services-sdk.md)
* [Monetisierung Ihrer App mit Werbeanzeigen](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [Bericht zur Anzeigen-Performance](../publish/advertising-performance-report.md)



<!--HONumber=Sep16_HO2-->


