---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "Überprüfen Sie die Versionshinweise für die Microsoft Advertising-Bibliotheken im „Microsoft Store Engagement and Monetization“-SDK."
title: "Versionshinweise für die Microsoft Advertising-Bibliotheken"
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 8e2114e969b27d579f62195f026cfcfd9672a94a

---

# Versionshinweise für die Microsoft Advertising-Bibliotheken


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieser Abschnitt enthält Versionshinweise für die aktuelle Version der Microsoft Advertising-Bibliotheken im „Microsoft Store Engagement and Monetization“-SDK. Diese Bibliotheken unterstützt XAML- und JavaScript/HTML-Apps für Windows 10, Windows 8.1, Windows Phone 8.1 und Windows Phone 8.

## Installation


Die Microsoft Advertising-Bibliotheken stehen als Teil des [Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk)-SDK zur Verfügung. Für alle Projekttypen außer Windows Phone 8.x Silverlight sind die Microsoft Advertising-Assemblys, die mit den früher eigenständigen SDKs „Microsoft Universal Ad Client“ und „Microsoft Advertising“-SDK verteilt wurden, nun im „Microsoft Store Engagement and Monetization“-SDK installiert. Weitere Informationen zum Installieren des SDK und der darin enthaltenen Bibliotheken finden Sie unter [installieren der Microsoft Advertising-Bibliotheken](install-the-microsoft-advertising-libraries.md).

Um die Microsoft Advertising-Assemblys für Windows Phone 8.x Silverlight-Projekte zu erhalten, installieren Sie das [Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk)-SDK, öffnen Ihr Projekt in Visual Studio und wechseln dann zu **Projekt** > **Verbundenen Dienst hinzufügen** > **Ad Mediator**. Die Assemblys werden anschließend automatisch geladen. Im Anschluss daran können Sie die Ad Mediator-Referenzen aus Ihrem Projekt entfernen, wenn Sie Ad Mediator nicht verwenden möchten. Weitere Informationen finden Sie unter [AdControl in Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).

## Unterschied zwischen den Microsoft Advertising-Bibliotheken und den Ad Mediation-Bibliotheken

Obwohl die Microsoft Advertising-Bibliotheken und die Ad Mediation-Bibliotheken beide im „Microsoft Store Engagement and Monetization“-SDK bereitgestellt werden, sind diese Bibliotheken für unterschiedliche Zwecke konzipiert. Verwenden Sie die Klassen [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) und [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) aus den Microsoft Advertising-Bibliotheken, wenn Sie Banner oder Werbevideos von Microsoft in einer XAML- oder JavaScript-App anzeigen möchten. Verwenden Sie die **AdMediatorControl**-Klasse aus den Ad Mediation-Bibliotheken, wenn Sie Werbebanner aus unterschiedlichen Anzeigennetzwerken in einer XAML-App einblenden möchten (Ad Mediation wird von JavaScript/HTML-Apps nicht unterstützt). Weitere Informationen finden Sie unter [Was ist der Unterschied zwischen AdMediatorControl und AdControl?](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

## Deinstallieren früherer Versionen

Wir empfehlen dringend, alle Instanzen des „Microsoft Universal Ad Client“-SDK und des „Microsoft Advertising“-SDK zu deinstallieren, bevor Sie das „Microsoft Store Engagement and Monetization“-SDK installieren.

## Zielarchitekturspezifische Buildausgabe

Wenn Sie die Microsoft Advertising-Bibliotheken verwenden, können Sie in Ihrem Projekt nicht das Ziel **Any CPU** angeben. Sollte in Ihrem Projekt die Zielplattform **Any CPU** definiert sein, wird in Ihrem Projekt eine Warnung ausgegeben, sobald Sie einen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Um diese Warnung zu entfernen, müssen Sie eine architekturspezifische Buildausgabe verwenden (beispielsweise **x86**) und das Projekt entsprechend aktualisieren. Weitere Informationen finden Sie unter [Bekannte Probleme](known-issues-for-the-advertising-libraries.md).

## C++-Unterstützung

Die Microsoft Advertising-Bibliotheken (in den die Klassen **AdControl** und **InterstitialAd** enthalten sind) unterstützen in C++ geschriebene Apps sowie DirectX mit Windows-Runtime-Interoperabilität (*interop*). Weitere Informationen und Beispiele zum Programmieren mit XAML und C++ finden Sie unter [Typsystem](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx).

## Kein Toolbox-Steuerelement

In der aktuellen Version der Microsoft Advertising-Bibliotheken im „Microsoft Store Engagement and Monetization“-SDK gibt es kein Toolbox-Steuerelement zum Ziehen eines **AdControl** oder **InterstitialAd** in die Entwurfsoberfläche Ihrer App. Informationen zum Hinzufügen dieser Steuerelemente in Ihrem Markup und Code finden Sie in der [Exemplarische Vorgehensweisen für Entwickler](developer-walkthroughs.md).

## Eigenschaften Latitude und Longitude nicht mehr verfügbar

Die **AdControl**-Klasse verfügt nicht mehr über die Eigenschaften **Latitude** und **Longitude** für UWP-Apps. Stattdessen erkennt der Code im Anzeigensteuerelement diese Werte und sendet sie für die App an die Anzeigenserver.

## Wichtiger Hinweis

Lesen Sie die Endbenutzer-Lizenzbedingungen vollständig durch. Weitere Informationen finden Sie im Thema [Wichtiger Hinweis zu den Endbenutzer-Lizenzbedingungen](important-notice-eula.md).

 

 



<!--HONumber=Jun16_HO4-->


