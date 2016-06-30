---
author: mcleanbyron
ms.assetid: fcebd659-438b-4d03-bc73-6b662ed6f1f3
description: "Hier wird die Entwicklung und Veröffentlichung einer App mit Anzeigen vollständig erläutert."
title: "Workflows für das Erstellen von Apps mit Anzeigen"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 69dd1a17290b7ffbc14dbc58404868119403f7c0


---

# Workflows für das Erstellen von Apps mit Anzeigen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Damit Anzeigen in Ihren Apps dargestellt werden können, muss Ihre App in der Lage sein, Anzeigen von einem Anzeigennetzwerk zu empfangen. Microsoft bietet einen Webdienst, der Entwicklern von Windows-Apps das Empfangen von Anzeigen ermöglicht. Wenn Benutzer in Ihrer App auf die Anzeige klicken, erhalten Sie (der *Veröffentlicher* der Anzeige) eine Vergütung vom Ersteller der Anzeige (dem *Werbekunden*). Zahlungen von Werbekunden werden über Ihr Konto abgewickelt.

Im Folgenden werden die wesentlichen Schritte zum Entwickeln und Veröffentlichen einer App mit Anzeigen beschrieben.

1.  Entwicklungsphase:

    * Einrichten des Windows Dev Center-Kontos
    * Entwickeln der App unter Verwendung von Werten des Testmodus

2.  Bereit für die Veröffentlichung:

    * Konfigurieren der App für den Empfang von Liveanzeigen
    * Übermitteln der App an das Windows Dev Center und Überprüfen der Leistungsdaten

Weitere Informationen zu den einzelnen Schritten finden Sie in den entsprechenden Abschnitten unten.

## Einrichten des Windows Dev Center-Kontos

Sie benötigen ein Windows Dev Center-Konto, um Ihre App zu veröffentlichen und Anzeigen zu empfangen. Dies gilt unabhängig davon, ob Sie die Anzeigenvermittlung verwenden. Die Verwaltung von Apps im Zusammenhang mit Werbung erfolgt ebenfalls im Windows Dev Center. Wenn Sie Werbung in Ihren Apps mithilfe von Microsoft pubCenter verwaltet haben, wurde diese Funktionalität durch Features im Windows Dev Center ersetzt.

Informationen zum Einrichten des Kontos bei Windows Dev Center finden Sie auf der [Startseite](https://dev.windows.com/windows-apps). Weitere Informationen finden Sie auf den [Hilfeseiten](https://dev.windows.com/develop) zu Windows Dev Center.

## Entwickeln der App unter Verwendung von Werten des Testmodus

Verwenden Sie die Anweisungen in den folgenden exemplarischen Vorgehensweisen, um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) oder [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) hinzuzufügen und Anzeigen in Ihrer App darzustellen:

-   [Interstitialanzeigen](interstitial-ads.md)
-   [AdControl in XAML und .NET](adcontrol-in-xaml-and--net.md)
-   [AdControl in HTML 5 und Javascript](adcontrol-in-html-5-and-javascript.md)
-   [AdControl in Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md)

Wenn Sie **AdControl** oder **InterstitialAd** zum Anzeigen von Werbung in Ihrer App verwenden, müssen Sie eine Anwendungs-ID und eine Anzeigeneinheits-ID im Code angeben, um die App mit Ihrem Windows Dev Center-Konto zu verknüpfen und um Anzeigen zu schalten. Während Sie Ihre App entwickeln, verwenden Sie die Testwerte für die Anwendungs-ID und Anzeigeneinheits-ID, um zu beobachten, wie die App Anzeigen während der Testphase rendert. Dadurch können Sie während der Testphase feststellen, wie die App Anzeigen empfängt und rendert. Weitere Informationen finden Sie unter [Testmoduswerte](test-mode-values.md).

Vollständige Beispielprojekte, die veranschaulichen, wie Sie JavaScript-/HTML- und XAML-Apps unter Verwendung von C# und C++ Werbebanner und Video-Interstitialanzeigen hinzufügen, finden Sie in den [Anzeigenbeispielen auf GitHub](http://aka.ms/githubads).

## Konfigurieren der App für den Empfang von Liveanzeigen

Nachdem Sie Ihre App getestet haben und bereit sind, sie an das Windows Dev Center zu übermitteln, müssen Sie Ihren App-Code so ändern, dass er die Anwendungs-ID und die Anzeigeneinheits-ID aus dem [Windows Dev Center-Dashboard](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx) verwendet. Wenn Sie Testwerte in Ihrer Live App verwenden, empfängt die App keine Liveanzeigen. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md).

## Übermitteln der App

Nachdem die Entwicklung abgeschlossen ist, können Sie die App mithilfe des Windows Dev Center-Dashboards im Windows Store veröffentlichen. Neben den Anforderungen, die alle Apps im Windows Store erfüllen müssen, gelten für Apps mit Anzeigen zusätzliche Anforderungen. Weitere Informationen finden Sie unter [Übermitteln einer App mit Werbung an den Windows Store](submit-an-app-with-ads-to-the-windows-store.md).

Nachdem die App veröffentlicht wurde und im Windows Store verfügbar ist, können Sie die [Berichte zur Anzeigen-Performance](../publish/advertising-performance-report.md) im Dev Center-Dashboard überprüfen.

 

 



<!--HONumber=Jun16_HO4-->


