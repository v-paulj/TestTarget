---
author: mcleanbyron
Description: "Der Microsoft Store Services SDK bietet Bibliotheken und Tools zum Hinzufügen von Features zu Ihren Apps, mit denen Sie mehr Geld verdienen und Kunden gewinnen können."
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 98675e9cb06b05e55d49ca625818626aea5a5346

---

# Microsoft Store Services SDK

Der Microsoft Store Services SDK bietet Bibliotheken und Tools, mit denen Sie in Ihren Apps für die universelle Windows-Plattform (UWP) mehr Geld verdienen und Kunden gewinnen können. Mit diesen können Sie z.B. Anzeigen in Ihren Apps einblenden und Experimente mit A/B-Tests durchführen. Dieser SDK wird ständig weiterentwickelt, um neue Funktionen für die Vernetzung und Monetarisierung bereitzustellen.


## Im SDK verfügbare Features

Der Microsoft Store Services SDK enthält Bibliotheken und Tools, die die folgenden Features unterstützen.

### Durchführen von Experimenten mit A/B-Tests für UWP-Apps

Führen Sie A/B-Tests in Ihren UWP-Apps durch, um die Effektivität der Features für einige Kunden zu messen, bevor Sie die Features für alle Benutzer freigeben. Verwenden Sie nach der Definition eines Experiments im Dev Center-Dashboard die [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx)-Klasse zum Abrufen von Varianten für Ihr Experiment in der App, verwenden Sie diese Daten zum Ändern des Verhaltens des Features, das Sie testen, und verwenden Sie dann die [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx)-Methode zum Senden des Anzeigeereignisses und der Umwandlungsereignisse an Dev Center. Verwenden Sie dann das Dashboard zum Anzeigen der Ergebnisse und Verwalten des Experiments.

Weitere Informationen finden Sie unter [Durchführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md).

### App-Feedback für UWP-Apps

Verwenden Sie die [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx)-Klasse in Ihren UWP-Apps, um Ihre Windows10-Kunden auf den Feedback-Hub zu verweisen. Dort können Kunden ihre Probleme und Vorschläge übermitteln und das Feedback anderer Benutzer lesen und bewerten. Verwalten Sie anschließend dieses Feedback im [Feedbackbericht](../publish/feedback-report.md) im Dev Center-Dashboard.

Weitere Informationen finden Sie unter [Feedback-Hub über Ihre App starten](launch-feedback-hub-from-your-app.md).

### Anzeigen von Werbung in Ihren Apps

Steigern Sie Ihren Umsatz, indem Sie Banneranzeigen oder Videointerstitialanzeigen von Microsoft in UWP-Apps anzeigen. Sie können auch Ihre Anzeigenfüllraten erhöhen, indem Sie die Anzeigenermittlung zum Anzeigen von Anzeigen von mehreren Anzeigennetzwerkanbietern verwenden.

Weitere Informationen finden Sie unter [Anzeigen von Werbung in Ihrer App](display-ads-in-your-app.md).

>**Hinweis**&nbsp;&nbsp;Microsoft Store Services SDK unterstützt nur UWP-Apps. Um Anzeigen in Windows8.1 und Windows Phone8.x-Apps anzuzeigen, verwenden Sie den [Microsoft Advertising SDK for Windows and Windows Phone 8.x](http://aka.ms/store-8-sdk).

### API-Referenz

Die Referenzdokumentation zu den APIs im Microsoft Store Services SDK finden Sie unter [Microsoft Store Services SDK-API-Referenz](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

## Installieren des SDK

So installieren Sie den Microsoft Store Services SDK

1.  Schließen Sie alle Instanzen von Visual Studio2013 oder Visual Studio2015, und deinstallieren Sie alle früheren Versionen von Microsoft Store Engagement and Monetization SDK, Universal Ad Client SDK, Ad Mediator-Erweiterung oder Microsoft Advertising SDK.
2.  Laden Sie das [SDK](http://aka.ms/store-em-sdk) herunter, und installieren Sie es. Die Installation kann ein paar Minuten dauern. Warten Sie unbedingt, bis der Vorgang abgeschlossen ist.
3.  Starten Sie Visual Studio neu.

Microsoft veröffentlicht in regelmäßigen Abständen neue Versionen des Microsoft Store Services SDK, die Leistungsverbesserungen und neue Features umfassen. Wenn Sie über vorhandene Projekte verfügen, die den Microsoft Store Services SDK verwenden, und Sie die neueste Version verwenden möchten, laden Sie einfach die neueste Version des SDK herunter, und installieren Sie diese.

Die Werbefeatures für UWP-Apps aus früheren Versionen des Microsoft Store Engagement and Monetization SDK, des Universal Ad Client SDK, der Ad Mediator-Erweiterung und des Microsoft Advertising SDK sind nun im Microsoft Store Services SDK enthalten. Wenn Sie vorhandene UWP-Projekte haben, die Werbungsfeatures aus der vorherigen Versionen verwenden, können Sie nach der Installation des Microsoft Store Services SDK weiterhin mit Ihren Projekten arbeiten, ohne dass Änderungen erforderlich sind.

>**Hinweis**  Um den Microsoft Store Services SDK mit Visual Studio2015 zu installieren, muss Version1.1 oder höher der Visual Studio-Tools für universelle Windows-Apps installiert sein. Weitere Informationen zu diesem Update der Visual Studio-Tools für universelle Windows-Apps finden Sie in den [Versionshinweisen](http://go.microsoft.com/fwlink/?LinkID=624516).

## Frameworkpakete im SDK

Die folgenden Bibliotheken im Microsoft Store Services SDK werden als *Frameworkpakete* konfiguriert:

* Microsoft.Advertising.dll. Diese Bibliothek enthält die Werbe-APIs in den [Microsoft.Advertising](https://msdn.microsoft.com/en-us/library/windows/apps/mt313187.aspx)- und [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.aspx)-Namespaces.
* Microsoft.Services.Store.Engagement.dll. Diese Bibliothek enthält die APIs im [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.aspx)-Namespace.

Dies bedeutet, dass diese Bibliotheken nach der Installation des SDK auf Ihrem Entwicklungscomputer automatisch über Windows Update aktualisiert werden, sobald neue Versionen der Bibliotheken mit Fixes und Leistungsverbesserungen veröffentlicht werden. Dadurch wird sichergestellt, dass immer die neueste Version der Bibliotheken auf Ihrem Entwicklungscomputer installiert ist.

Darüber hinaus werden die Bibliotheken, nachdem ein Benutzer eine Version Ihrer App installiert hat, die diese Bibliotheken verwendet, automatisch auch auf dessen Gerät aktualisiert, wenn neue Versionen der Bibliotheken mit Fixes und Leistungsverbesserungen veröffentlicht werden. Dies bedeutet, dass Benutzer stets über die jeweils aktuelle Version der Bibliotheken verfügen, ohne dass Sie aktualisierte Versionen Ihrer App im Store veröffentlichen müssen.

Wenn wir eine neue Version des SDK veröffentlichen, in der neue APIs oder Features in diese Bibliotheken eingeführt werden, müssen Sie die neueste Version des SDK zur Verwendung dieser Features installieren. In diesem Szenario müssen Sie auch die aktualisierte App im Store veröffentlichen.

## Verwandte Themen

* [Microsoft Store Services SDK-API-Referenz](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Ausführen von Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)
* [Starten des Feedback-Hubs über Ihre App](launch-feedback-hub-from-your-app.md)
* [Anzeigen von Werbung in Ihrer App](display-ads-in-your-app.md)



<!--HONumber=Sep16_HO2-->


