---
author: mcleanbyron
ms.assetid: 9165f709-71d7-42cf-9b30-3190fe029fb4
description: Hier werden die Unterschiede zwischen der AdControl-Klasse in den Microsoft Advertising-Bibliotheken und der AdMediatorControl-Klasse in den Bibliotheken der Anzeigenvermittlung erläutert.
title: Unterschiede zwischen AdMediatorControl und AdControl
---

# Unterschiede zwischen AdMediatorControl und AdControl


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Verwenden Sie in Ihrer App die Microsoft Advertising-Bibliotheken für XAML und JavaScript, um Werbebanner oder Video-Interstitialanzeigen von Microsoft einzublenden. Diese Bibliotheken unterscheiden sich von den Bibliotheken der Anzeigenvermittlung, die Sie zum Anzeigen von Werbung aus mehreren Anzeigennetzwerken verwenden. Verwenden Sie diese Dokumentation zu den Microsoft Advertising-Bibliotheken (XAML und JavaScript) in folgenden Fällen:

* Sie verwenden [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) oder [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) in einer XAML- oder JavaScript-App und nicht **AdMediatorControl**.
* Sie benötigen Referenzinformationen zur zugrunde liegenden **AdControl**-API, die von der Anzeigenvermittlung verwendet wird.

Die Microsoft Advertising-Bibliotheken und die Bibliotheken der Anzeigenvermittlung sind beide im Microsoft Store Engagement and Monetization SDK enthalten. Weitere Informationen zum Installieren dieses SDKs und zu den unterschiedlichen im SDK enthaltenen Microsoft Advertising-Bibliotheken finden Sie unter [Installieren der Microsoft Advertising-Bibliotheken](install-the-microsoft-advertising-libraries.md).

>**Hinweis**  Verwenden Sie das **InterstitialAd**-Steuerelement zum Anzeigen von Interstitialanzeigen. **AdControl** und **AdMediatorControl** sind nicht in der Lage, Interstitialanzeigen darzustellen. Weitere Informationen finden Sie unter [Interstitialanzeigen](interstitial-ads.md).

 

## Anzeigenvermittlung


Zum Anzeigen von Werbebannern (keinen Interstitialanzeigen) von Microsoft in Ihrer App wird **AdMediatorControl** empfohlen. **AdMediatorControl** zeigt Werbebanner von mehreren Anzeigennetzwerken an.

Wenn Sie **AdMediatorControl** in einem Projekt verwenden, müssen Sie mit dem Feature **Verbundene Dienste** in Visual Studio angeben, welche Anzeigennetzwerke verwendet werden sollen. Visual Studio versucht, die erforderlichen Assemblys für einige Anzeigennetzwerke programmgesteuert abzurufen. Assemblys, die nicht automatisch abgerufen werden können, müssen für die einzelnen Anzeigennetzwerke installiert werden. Weitere Informationen zur Anzeigenvermittlung finden Sie unter [Maximieren des Umsatzes mit einer Anzeigenvermittlung](use-ad-mediation-to-maximize-revenue.md).

**AdMediatorControl** abstrahiert die Verwendung von Anzeigeneinheits-IDs und Anwendungs-IDs. Diese IDs werden zusammen mit den Anzeigenvermittlungseinstellungen im Windows Dev Center-Dashboard durch **AdMediatorControl** verwaltet. Weitere Informationen finden Sie unter [Übermitteln der App und Konfigurieren der Anzeigenvermittlung](submit-your-app-and-configure-ad-mediation.md).

**AdMediatorControl** unterstützt die APIs für jeden von ihm vermittelten Werbedienste unter Verwendung der eigenen Attribute und Syntax. Weitere Informationen finden Sie unter [Hinzufügen und Verwenden des Ad Mediator-Steuerelements](add-and-use-the-ad-mediator-control.md).

## AdControl


Wenn Sie nur Werbebanner von Microsoft (und aus keinen anderen Anzeigennetzwerken) anzeigen möchten, können Sie **AdControl** direkt in XAML- und JavaScript-Apps verwenden. Verwenden Sie beim Testen einer App, die **AdControl** verwendet, die [Testmoduswerte](test-mode-values.md) für die Anwendungs-ID und Anzeigeneinheits-ID. Nachdem Sie Ihre App vollständig getestet haben und bereit sind, sie an das Windows Dev Center zu übermitteln, rufen Sie die Anzeigeneinheits-ID und Anwendungs-ID Ihrer Liveanzeige über das Windows Dev Center-Dashboard ab und ändern den Code, sodass er diese Werte verwendet. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md).

 

 


<!--HONumber=May16_HO2-->


