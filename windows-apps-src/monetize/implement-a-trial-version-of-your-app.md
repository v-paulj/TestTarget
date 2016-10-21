---
author: mcleanbyron
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um eine Testversion Ihrer App zu implementieren.
title: Implementieren einer Testversion Ihrer App
keywords: Kostenloses Codebeispiel zu Testzwecken
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 22f355c23f4cc87932563e9885f390e9a5ac4130

---

# Implementieren einer Testversion Ihrer App

Wenn Sie Kunden ermöglichen, Ihre App während eines Testzeitraums kostenlos zu verwenden, können Sie durch die Deaktivierung oder Einschränkung einiger Features während des Testzeitraums Ihre Kunden dazu bringen, ein Upgrade auf die Vollversion Ihrer App auszuführen. Bestimmen Sie die einzuschränkenden Features, bevor Sie mit dem Codieren beginnen, und stellen Sie dann sicher, dass diese nur beim Erwerb einer Lizenz für die Vollversion der App verfügbar sind. Außerdem können Sie Features wie Banner oder Wasserzeichen aktivieren, die nur in der Testversion angezeigt werden, bevor ein Kunde Ihre App kauft.

Apps für Windows10, Version1607 oder höher, können Mitglieder der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse im [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace verwenden, um zu bestimmen, ob Benutzer eine Testversion Ihrer App ausführen, und benachrichtigt zu werden, wenn während der Ausführung der App der Status geändert wird.

>**Hinweis**&nbsp;&nbsp;Dieser Artikel bezieht sich auf Apps für Windows10, Version 1607 oder höher. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie unter [In-App-Einkäufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## Richtlinien für die Implementierung einer Testversion

Der aktuelle Lizenzstatus Ihrer App wird als Eigenschaften der [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx)-Klasse gespeichert. In der Regel nehmen Sie die vom Lizenzstatus abhängigen Funktionen in einen Bedingungsblock auf wie im nächsten Schritt beschrieben. Stellen Sie beim Auswählen dieser Features sicher, dass sie auf eine Weise implementiert werden können, dass sie in jedem Lizenzstatus funktionieren.

Überlegen Sie auch, wie Änderungen an der App-Lizenz während der Ausführung der App verarbeitet werden sollen. Ihre Test-App kann alle Features, jedoch zusätzlich In-App-Anzeigenbanner enthalten, die die kostenpflichtige Version nicht enthält. Oder in der Test-App sind bestimmte Features deaktiviert, oder es werden regelmäßig Aufforderungen zum Kauf angezeigt.

Überlegen Sie, welche Art App Sie erstellen und welche Test- oder Ablaufstrategie sich gut für sie eignet. Bei einer Testversion für ein Spiel hat es sich bewährt, den Umfang des Spielinhalts, den ein Anwender nutzen kann, zu beschränken. Bei der Testversion eines Hilfsprogramms möchten Sie vielleicht ein Ablaufdatum festlegen oder die Features einschränken, die ein potenzieller Kunde verwenden kann.

Bei den meisten Apps, die keine Spiele sind, ist das Festlegen eines Ablaufdatums eine gute Methode, da Benutzer ein gutes Verständnis für die vollständige App entwickeln. Im Folgenden sind häufige Ablaufszenarien und Ihre Optionen für ihre Handhabung aufgeführt.

-   **Ablauf der Testlizenz während der App-Ausführung**

    Falls die Testversion abläuft, während die App ausgeführt wird, kann Ihre App:

    -   keine Aktion ausführen.
    -   dem Kunden eine Meldung anzeigen.
    -   geschlossen werden.
    -   den Kunden auffordern, die App zu kaufen.

    Die beste Vorgehensweise besteht darin, eine Meldung mit der Aufforderung zum Kauf der App anzuzeigen und nach dem Kauf die App mit allen aktivierten Features fortzusetzen. Wenn der Kunde die App nicht kauft, wird die App geschlossen, oder der Kunde wird in regelmäßigen Abständen daran erinnert, die App zu kaufen.

-   **Ablauf der Testlizenz vor dem Starten der App**

    Falls die Testversion abläuft, bevor der Benutzer die App startet, wird Ihre App nicht gestartet. Stattdessen sehen Benutzer ein Dialogfeld, das ihnen die Möglichkeit bietet, die App im Windows Store zu kaufen.

-   **Kauf der App durch den Kunden während der App-Ausführung**

    Wenn der Kunde Ihre App während der Ausführung erwirbt, kann die App:

    -   keine Aktion ausführen und die Fortsetzung im Testmodus bis zum Neustart der App zulassen.
    -   dem Kunden für den Kauf danken oder eine andere Meldung anzeigen.
    -   die Features aktivieren, die mit einer Volllizenz verfügbar sind (oder die Meldungen, dass es sich um eine Testversion handelt, deaktivieren).

Beschreiben Sie, wie sich Ihre App während und nach dem kostenlosen Testzeitraum verhält, sodass Ihre Kunden vom Verhalten Ihrer App nicht überrascht werden. Weitere Informationen zum Beschreiben Ihrer App finden Sie unter [Erstellen von App-Beschreibungen](https://msdn.microsoft.com/library/windows/apps/mt148529).

## Voraussetzungen

Für dieses Beispiel gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine App für die universelle Windows-Plattform (UWP), die für Windows10, Version1607 oder höher, geeignet ist.
* Sie haben eine App im Windows Dev Center-Dashboard erstellt, die als [kostenlose Testversion](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) ohne zeitliche Begrenzung konfiguriert ist, im Store veröffentlicht ist und dort verfügbar ist. Dies kann eine App sein, die Sie für Kunden freigeben möchten, oder eine einfache App sein, die den Mindestanforderungen gemäß dem [Zertifizierungskit für Windows-Apps](https://developer.microsoft.com/windows/develop/app-certification-kit) entspricht und nur zu Testzwecken verwendet wird. Weitere Informationen finden Sie unter [Hinweise für Tests](in-app-purchases-and-trials.md#testing).

Der Code in diesem Beispiel geht von folgenden Voraussetzungen aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx), die einen [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) mit dem Namen ```workingProgressRing``` und einen [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den **Windows.Services.Store**-Namespace.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Einkäufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

## Codebeispiel

Ruft während des Initialisierens der App das [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx)-Objekt für Ihre App ab und behandelt das [OfflineLicensesChanged](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.offlinelicenseschanged.aspx)-Ereignis, um Benachrichtigungen zu empfangen, wenn die Lizenz während der Ausführung der App geändert wird. Die App-Lizenz kann zum Beispiel geändert werden, wenn der Testzeitraum abläuft oder der Kunde die App in einem Store kauft. Ruft die neue Lizenz ab, wenn die Lizenz geändert wird, und aktiviert oder deaktiviert dementsprechend ein Feature Ihrer App.

Wenn ein Benutzer die App gekauft hat, wird empfohlen, den Benutzer zu diesem Zeitpunkt über den geänderten Lizenzstatus zu informieren. Gegebenenfalls müssen Sie den Benutzer zum Neustarten der App auffordern, falls Ihre Programmierung dies erfordert. Versuchen Sie jedoch, diesen Übergang so nahtlos und unmerklich wie möglich zu machen.


```csharp
private StoreContext context = null;
private StoreAppLicense appLicense = null;

// Call this while your app is initializing.
private async void InitializeLicense()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    workingProgressRing.IsActive = true;
    appLicense = await context.GetAppLicenseAsync();
    workingProgressRing.IsActive = false;

    // Register for the licenced changed event.
    context.OfflineLicensesChanged += context_OfflineLicensesChanged;
}

private async void context_OfflineLicensesChanged(StoreContext sender, object args)
{
    // Reload the license.
    workingProgressRing.IsActive = true;
    appLicense = await context.GetAppLicenseAsync();
    workingProgressRing.IsActive = false;

    if (appLicense.IsActive)
    {
        if (appLicense.IsTrial)
        {
            textBlock.Text = $"This is the trial version. Expiration date: {appLicense.ExpirationDate}";

            // Show the features that are available during trial only.
        }
        else
        {
            // Show the features that are available only with a full license.
        }
    }
}
```

Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Unterstützen von In-App-Einkäufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Käufen konsumierbarer Add-Ons](enable-consumable-add-on-purchases.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->


