---
author: mcleanbyron
Description: "Das Microsoft Store Engagement and Monetization SDK bietet Bibliotheken und Tools zum Hinzufügen von Features zu Ihren Apps, mit denen Sie mehr Geld verdienen und Kunden gewinnen können."
title: Microsoft Store Engagement and Monetization SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: de85956c7c1d2a0ba509d61ee8928b412f057f8a
ms.openlocfilehash: 481cf2aab806a1f9ce368256a9df8930cbc756c1

---

# Microsoft Store Engagement and Monetization SDK

Das Microsoft Store Engagement and Monetization SDK bietet Bibliotheken und Tools, mit denen Sie mehr Geld verdienen und Kunden gewinnen können. Mit diesen können Sie z. B. Anzeigen in Ihren Apps einblenden und Experimente mit A/B-Tests durchführen. Dieses SDK ersetzt das Microsoft Universal Ad Client SDK und wird ständig weiterentwickelt, um neue Funktionen für die Kundengewinnung und Monetarisierung bereitzustellen.


## In dem SDK verfügbare Features

Das Microsoft Store Engagement and Monetization SDK enthält Bibliotheken und Tools, die die folgenden Features unterstützen.

### Durchführen von Experimenten mit A/B-Tests für UWP-Apps

Führen Sie A/B-Tests in Ihren UWP-Apps durch, um die Effektivität der Features für einige Kunden zu messen, bevor Sie die Features für alle Benutzer freigeben. Verwenden Sie nach Definition eines Experiments im Dev Center-Dashboard die [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx)-Klasse zum Abrufen von Varianten für Ihr Experiment in der App, verwenden Sie diese Daten zum Ändern des Verhaltens des Features, das Sie testen, und verwenden Sie dann die [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx)-Methode zum Senden des Anzeigeereignisses und der Umwandlungsereignisse an Dev Center. Verwenden Sie dann das Dashboard zum Anzeigen der Ergebnisse und Verwalten des Experiments.

Weitere Informationen finden Sie unter [Durchführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md).

### App-Feedback für UWP-Apps

Verwenden Sie die [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx)-Klasse in Ihren UWP-Apps, um Ihre Windows 10-Kunden auf den Feedback-Hub zu verweisen. Dort können Kunden ihre Probleme und Vorschläge übermitteln und Feedback anderer Benutzer lesen und bewerten. Verwalten Sie anschließend dieses Feedback im [Feedbackbericht](../publish/feedback-report.md) im Dev Center-Dashboard.

Weitere Informationen finden Sie unter [Feedback-Hub über Ihre App starten](launch-feedback-hub-from-your-app.md).

>**Hinweis** Der **Feedback**-Bericht ist zurzeit nur für Entwicklerkonten verfügbar, die Mitglied des [Dev Center-Insider-Programms](../publish/dev-center-insider-program.md) sind.

### Anzeigen von Werbung in Ihren Apps

Erhöhen Sie Ihren Umsatz durch Anzeigen von Werbebannern und Video-Interstitialanzeigen von Microsoft in UWP-Apps, Windows 8.1- und Windows Phone 8.x-Apps. Sie können auch Ihre Anzeigenfüllraten erhöhen, indem Sie die Anzeigenermittlung zum Anzeigen von Anzeigen von mehreren Anzeigennetzwerkanbietern verwenden.

Weitere Informationen finden Sie unter [Anzeigen von Werbung in Ihrer App](display-ads-in-your-app.md).

>**Hinweis** Die Werbefeatures aus früheren Versionen des Universal Ad Client SDK, der Ad Mediator-Erweiterung und des Microsoft Advertising SDK sind jetzt im Microsoft Store Monetization and Engagement SDK enthalten.

### API-Referenz

Referenzdokumentation zu den APIs im SDK finden Sie unter [Referenz zum Microsoft Store Engagement and Monetization SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

## Installieren des SDK

So installieren Sie das Microsoft Store Engagement and Monetization SDK

1.  Schließen Sie alle Instanzen von Visual Studio 2013 oder Visual Studio 2015, und deinstallieren Sie alle früheren Versionen vom Universal Ad Client SDK, der Ad Mediator-Erweiterung oder vom Microsoft Advertising SDK.
2.  Laden Sie das [SDK](http://aka.ms/store-em-sdk) herunter, und installieren Sie es. Die Installation kann ein paar Minuten dauern. Warten Sie unbedingt, bis der Vorgang abgeschlossen ist.
3.  Starten Sie Visual Studio neu.

Microsoft veröffentlicht in regelmäßigen Abständen neue Versionen des Microsoft Store Monetization and Engagement SDK, die Leistungsverbesserungen und neue Features umfassen. Wenn Sie über vorhandene Projekte verfügen, die das Microsoft Store Monetization and Engagement SDK verwenden, und Sie die neueste Version verwenden möchten, laden Sie einfach die neueste Version des SDK herunter, und installieren Sie diese.

Die Werbefeatures aus früheren Versionen des Universal Ad Client SDK, der Ad Mediator-Erweiterung und des Microsoft Advertising SDK sind jetzt im Microsoft Store Monetization and Engagement SDK enthalten. Wenn Sie über vorhandene Visual Studio 2015- oder Visual Studio 2013-Projekte verfügen, die die Webefeatures aus den früheren Versionen verwenden, können Sie nach der Installation des Microsoft Store Monetization and Engagement SDK wie gewohnt mit Ihren Projekten weiterarbeiten.

>**Hinweis** Damit Sie das Microsoft Store Engagement and Monetization SDK mit Visual Studio 2015 installieren können, muss Version 1.1 oder höher der Visual Studio-Tools für universelle Windows-Apps installiert sein. Weitere Informationen zu diesem Update der Visual Studio-Tools für universelle Windows-Apps finden Sie in den [Versionshinweisen](http://go.microsoft.com/fwlink/?LinkID=624516).

## Frameworkpakete im SDK

Die folgende Bibliothek im Microsoft Store Monetization and Engagement SDK wird als *Frameworkpaket* konfiguriert:

* Microsoft.Advertising.dll (nur für UWP-App-Projekte). Diese Bibliothek enthält die Werbe-APIs in den **Microsoft.Advertising**- und **Microsoft.Advertising.WinRT.UI**-Namespaces.

Dies bedeutet, dass diese Bibliothek nach der Installation des SDK auf Ihrem Entwicklungscomputer automatisch über Windows Update aktualisiert wird, sobald neue Versionen der Bibliotheken mit Fixes und Leistungsverbesserungen veröffentlicht werden. Dadurch wird sichergestellt, dass immer die neueste Version der Bibliothek auf Ihrem Entwicklungscomputer installiert ist.

Darüber hinaus wird die Bibliothek, nachdem ein Benutzer eine Version Ihrer App installiert, die diese Bibliothek verwendet, automatisch auch auf seinem Gerät aktualisiert, wenn neue Versionen der Bibliothek mit Fixes und Leistungsverbesserungen veröffentlicht werden. Dies bedeutet, dass Benutzer immer über die aktuellste Version der Bibliothek verfügen, ohne dass Sie aktualisierte Versionen Ihrer App im Store veröffentlichen müssen.

Wenn wir eine neue Version des SDK veröffentlichen, in der neue APIs oder Features in dieser Bibliothek eingeführt werden, müssen Sie die neueste Version des SDK zur Verwendung dieser Features installieren. In diesem Szenario müssen Sie auch die aktualisierte App im Store veröffentlichen.

Andere Bibliotheken im SDK, einschließlich Microsoft.Advertising.dll für andere Zielplattformen und die Bibliotheken für die Anzeigenvermittlung, werden derzeit nicht als Frameworkbibliotheken konfiguriert.

## Verwandte Themen

* [Durchführen von Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)
* [Referenz zu APIs im Microsoft Store Engagement and Monetization SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Feedback-Hub über Ihre App starten](launch-feedback-hub-from-your-app.md)



<!--HONumber=Jun16_HO4-->


