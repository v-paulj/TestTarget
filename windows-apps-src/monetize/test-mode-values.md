---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "Verwenden Sie die für Tests vorgesehene Anwendungs-ID und die Anzeigeneinheits-ID aus diesem Artikel, um zu prüfen, wie Ihre App die Werbung während der Testphase rendert."
title: Testmoduswerte
translationtype: Human Translation
ms.sourcegitcommit: c6e0cf98c6eb2cdc656d5b4555d794ff6a94d2bc
ms.openlocfilehash: e1462ae48e8aae8f5ed0e5a7e46a6660bf33e786

---

# Testmoduswerte




Bei Verwendung von [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) oder [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) zum Anzeigen von Werbung in Ihrer App müssen Sie eine Anwendungs-ID und eine Anzeigeneinheits-ID angeben. Während Sie Ihre App entwickeln, verwenden Sie die entsprechende Testanwendungs-ID und Anzeigenblock-ID, um zu beobachten, wie die Werbung während der Testphase von Ihrer App gerendert wird.


Wenn Sie Testwerte in einer veröffentlichten App verwenden, wird die App keine Livewerbung empfangen. Um Werbung in Ihrer veröffentlichten App zu empfangen, müssen Sie in Ihrem Code die Anwendungs-ID und die Anzeigeneinheits-ID verwenden, die im Windows Dev Center-Dashboard bereitgestellt wird. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md).
 

Verwenden Sie die folgende Testwerte für interstitielles Video und Banneranzeigen.

* Für interstitielle Videoanzeigen:

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* Für Banneranzeigen:

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>


> **Wichtig**   Die Größe einer Liveanzeige wird durch die Eigenschaften **Width** und **Height** von **AdControl** bestimmt. Um optimale Ergebnisse zu erzielen, stellen Sie sicher, dass die Eigenschaften **Width** und **Height** zu den [unterstützten Eigenschaften für Banneranzeigen](supported-ad-sizes-for-banner-ads.md) gehören. Die Eigenschaften **Width** und **Height** ändern sich nicht entsprechend der Größe einer Liveanzeige.



 

 



<!--HONumber=Aug16_HO3-->


