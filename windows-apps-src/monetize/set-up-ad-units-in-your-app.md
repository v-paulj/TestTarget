---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "Erfahren Sie, wie Sie Ihrer App die Anwendungs-ID und die Anzeigeneinheits-ID aus dem Windows Dev Center-Dashboard hinzufügen, bevor Sie die App an den Store übermitteln."
title: Einrichten von Anzeigeneinheiten in der App
translationtype: Human Translation
ms.sourcegitcommit: c6e0cf98c6eb2cdc656d5b4555d794ff6a94d2bc
ms.openlocfilehash: 705955faf7ddd67f80098f8c3ac7b2844553de95


---

# Einrichten von Anzeigeneinheiten in der App




Bei Verwendung von [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) oder [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) zum Anzeigen von Werbung in Ihrer App müssen Sie eine Anwendungs-ID und eine Anzeigeneinheits-ID angeben. Während Sie Ihre App entwickeln, verwenden Sie die entsprechende [Testanwendungs-ID und Anzeigeneinheits-ID](test-mode-values.md), um zu beobachten, wie die Werbung während der Testphase von Ihrer App gerendert wird.

Nachdem Sie Ihre App getestet haben und bereit sind, sie an das Windows Dev Center zu übermitteln, müssen Sie Ihren App-Code so ändern, dass er die Anwendungs-ID und die Anzeigeneinheits-ID aus dem [Windows Dev Center-Dashboard](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx) enthält. Wenn Sie Testwerte in einer veröffentlichten App verwenden, wird die App keine Livewerbung empfangen.

So richten Sie Anwendungs-ID und Anzeigeblöcke für die endgültige App ein:

1.  Wählen Sie Ihre App im Windows Dev Center-Dashboard und klicken Sie dann auf **Monetisierung > Gewinnbringende Nutzung mit Anzeigen**.
2.  Im Abschnitt **Microsoft Advertising-Anzeigeneinheiten** auf dieser Seite erstellen Sie eine Anzeigeneinheit. Wählen Sie als Anzeigentyp **Banner** bei Verwendung von **AdControl** oder **Interstitialvideo** bei Verwendung von **InterstitialAd**. Weitere Informationen zu dieser Seite finden Sie unter [Gewinnbringende Nutzung mit Anzeigen](../publish/monetize-with-ads.md).

3.  Für jede generierte Anzeigeneinheit werden auf der Seite eine **Anwendungs-ID** und eine **Anzeigeneinheits-ID** angezeigt. Zum Einblenden von Anzeigen in Ihrer App müssen Sie diese Werte in Ihrem Code verwenden:

    * Wenn Ihre App Werbebanner anzeigt, weisen Sie diese Werte den Eigenschaften **ApplicationId** und **AdUnitId** Ihres **AdControl**-Objekts zu.

    * Wenn Ihre App Video-Interstitialanzeigen anzeigt, übergeben Sie diese Werte an die **RequestAd**-Methode Ihres **InterstitialAd**-Objekts.

 

## Verwandte Themen

[Testmoduswerte](test-mode-values.md)


 

 



<!--HONumber=Aug16_HO3-->


