---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: "Erfahren Sie mehr über die Installation der Microsoft Advertising-Bibliotheken."
title: Installieren der Microsoft Advertising-Bibliotheken
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 0951818ceaf3d96543f9f97ec6993d08fdaab2b8


---

# Installieren der Microsoft Advertising-Bibliotheken


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Die Microsoft Advertising-Bibliotheken für Windows-Apps sind im [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) enthalten. Dieses SDK ist eine Erweiterung von Visual Studio2015 und Visual Studio2013.

Installationsanweisungen finden Sie unter [Monetisierung Ihrer App und Kundengewinnung mit dem Microsoft Store Engagement and Monetization SDK](https://msdn.microsoft.com/windows/uwp/monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk).

> 
            **Hinweis** Wenn Sie Windows 10 Anniversary SDK Preview Build14295 oder höher mit Visual Studio2015 installiert haben, müssen Sie auch die WinJS-Bibliothek installieren, wenn Sie einer JavaScript/HTML-App Anzeigen hinzufügen möchten. Diese Bibliothek war in den früheren Versionen von Windows SDK für Windows 10 enthalten. Ab Windows10 Anniversary SDK Preview Build 14295 muss diese Bibliothek jedoch separat installiert werden. Informationen zur Installation von WinJS finden Sie unter [WinJS herunterladen](http://try.buildwinjs.com/download/GetWinJS/).

## Bibliotheksnamen für Werbung und Anzeigenvermittlung


Das Microsoft Store Engagement and Monetization SDK enthält zwei Sätze von Anzeigenbibliotheken: die Microsoft Advertising-Bibliotheken (die die Klassen [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) und [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) für XAML- und JavaScript/HTML-Apps bereitstellen) und die Bibliotheken für die Anzeigenvermittlung (die die Klasse **AdMediatorControl** bereitstellen).

In dieser Dokumentation wird die Verwendung der Klassen **AdControl** und **InterstitialAd** in den Microsoft Advertising-Bibliotheken zur Anzeige von Banner- oder Videointerstitialanzeigen von Microsoft beschrieben. Weitere Informationen zur Verwendung der Anzeigenvermittlung finden Sie unter [Maximieren des Umsatzes mit einer Anzeigenvermittlung](https://msdn.microsoft.com/windows/uwp/monetize/use-ad-mediation-to-maximize-revenue).


Bevor Sie eines der Anzeigensteuerelemente in Ihrem App-Code verwenden können, müssen Sie auf die entsprechende Bibliothek in Ihrem Projekt verweisen. In den folgenden Tabellen werden die Namen der einzelnen Bibliotheken im Microsoft Store Engagement und Monetisierung SDK aufgelistet, wie sie im Dialogfeld **Verweis-Manager** in Visual Studio angezeigt werden.


<table>
    <thead>
        <tr><th>Name des Steuerelements</th><th>Projekttyp</th><th>Name der Bibliothek im Verweis-Manager</th><th>Versionsnummer</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">
            **AdControl** und **InterstitialAd** (XAML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising SDK für XAML</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>Ad Mediator SDK für Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>WindowsPhone8.1</td>
            <td>Ad Mediator SDK für Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">
            **AdControl** und **InterstitialAd** (JavaScript/HTML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising SDK für JavaScript</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>Microsoft Advertising SDK für Windows8.1 Native (JS)</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>WindowsPhone8.1</td>
            <td>Microsoft Advertising SDK für Windows Phone8.1 Native (JS)</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">
            **AdMediatorControl** (nur XAML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising Universal SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>Ad Mediator SDK für Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>Ad Mediator SDK für Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 



<!--HONumber=Jun16_HO4-->


