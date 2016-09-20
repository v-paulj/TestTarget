---
author: mcleanbyron
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um mit Endverbraucher-Add-Ons zu arbeiten.
title: "Unterstützen von Endverbraucher-Add-On-Käufen"
keywords: In-App-Angebot, Codebeispiel
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 1e9ecad5abb9addbe41b38d0b56b84404716f2a8

---

# Unterstützen von Endverbraucher-Add-On-Käufen

Apps für Windows 10, Version 1607 oder höher, können Methoden der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse im [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace verwenden, um die Erfüllung des Benutzers von Endverbraucher-Add-Ons in Ihren UWP-Apps zu verwalten (Add-Ons werden auch als In-App-Produkte oder IAPs bezeichnet). Verwenden Sie Endverbraucher-Add-ons für Artikel, die gekauft, verwendet und erneut gekauft werden können. Dies ist besonders nützlich für Dinge wie spielinterne Währungen (Gold, Münzen usw.), die gekauft und dann zum Erwerben bestimmter Power-Ups verwendet werden können.

>**Hinweis**&nbsp;&nbsp;Dieser Artikel bezieht sich auf Apps für Windows 10, Version 1607 oder höher. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie unter [In-App-Einkäufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## Übersicht über Endverbraucher-Add-ons

Apps für Windows 10, Version 1607 oder höher, können zwei Arten von Endverbraucher-Add-Ons anbieten, die sich auf die Art und Weise unterscheiden, wie Erfüllungen verwaltet werden:

* **Von Entwicklern verwaltetes Endverbraucher-Add-On**. Bei dieser Art von Verbrauchsartikel sind Sie dafür verantwortlich, das Guthaben des Benutzers an Elementen zu verfolgen, die das Add-On darstellt, und den Kauf des Add-Ons dem Store als erfüllt zu melden, nachdem der Benutzer alle Elemente genutzt hat. Der Benutzer kann das Add-On erst dann erneut kaufen, nachdem Ihre App den vorherigen Kauf des Add-Ons als erfüllt gemeldet hat.

  Wenn beispielsweise das Add-On 100 Münzen in einem Spiel darstellt und der Benutzer 10 Münzen nutzt, muss die App oder der Dienst den neuen Restbetrag von 90 Münzen für den Benutzer verwalten. Nachdem der Benutzer alle 100 Münzen genutzt hat, muss die App das Add-On als erfüllt melden, und danach kann der Benutzer das Add-On für 100 Münzen erneut kaufen.

* **Vom Store verwalteter Verbrauchsartikel**. Bei dieser Art von Verbrauchsartikel verfolgt der Store das Guthaben des Benutzers an Elementen, die das Add-On darstellt. Wenn der Benutzer Elemente nutzt, sind Sie verantwortlich dafür, diese Elemente dem Store als erfüllt zu melden, und der Store aktualisiert das Guthaben des Benutzers. Ihre App kann das aktuelle Guthaben für den Benutzer jederzeit abfragen. Nachdem der Benutzer alle Elemente genutzt hat, kann er das Add-On erneut kaufen.

  Wenn das Add-On beispielsweise eine anfängliche Menge von 100 Münzen in einem Spiel darstellt und der Benutzer 10 Münzen nutzt, meldet die App dem Store, dass 10 Einheiten des Add-Ons erfüllt wurden, und der Store aktualisiert den Restbetrag. Nachdem der Benutzer alle 100 Münzen genutzt hat, kann er das Add-On für 100 Münzen erneut kaufen.

  >**Hinweis**&nbsp;&nbsp;Vom Store verwaltete Verbrauchsartikel sind ab Windows 10, Version 1607, verfügbar. Die Möglichkeit zum Erstellen eines vom Store verwalteten Verbrauchsartikels im Windows Dev Center-Dashboard ist in Kürze verfügbar.

Um einem Benutzer ein Endverbraucher-Add-on anzubieten, befolgen Sie dieses allgemeine Verfahren:

1. Ermöglichen Sie Benutzern den [Erwerb des Add-ons](enable-in-app-purchases-of-apps-and-add-ons.md) in Ihrer App.
3. Wenn der Benutzer das Add-on nutzt (z. B. indem er Münzen in einem Spiel ausgibt), [melden Sie das Add-on als erfüllt](enable-consumable-add-on-purchases.md#report_fulfilled).

Auch das [Abrufen des Restbetrags](enable-consumable-add-on-purchases.md#get_balance) für einen vom Store verwalteten Verbrauchsartikel ist jederzeit möglich.

## Voraussetzungen

Für diese Beispiele gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine UWP (Universelle Windows-Plattform)-App, die für Windows 10, Version 1607 oder höher, geeignet ist.
* Sie haben im Windows Dev Center-Dashboard eine App mit Endverbraucher-Add-ons (auch als App-Produkte oder IAPs bezeichnet) erstellt, und diese Anwendung wird veröffentlicht und ist im Store verfügbar. Dies kann eine App sein, die Sie für Kunden freigeben möchten, oder es kann eine einfache App sein, die den Mindestanforderungen gemäß dem [Zertifizierungskit für Windows-Apps](https://developer.microsoft.com/windows/develop/app-certification-kit) entspricht und nur zu Testzwecken verwendet wird. Weitere Informationen finden Sie unter [Hinweise für Tests](in-app-purchases-and-trials.md#testing).

Der Code in diesen Beispielen geht von Folgendem aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx), die einen [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) mit dem Namen ```workingProgressRing``` und einen [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den **Windows.Services.Store**-Namespace.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Einkäufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="report_fulfilled" />
## Melden eines Endverbraucher-Add-Ons als erfüllt

Nachdem der Benutzer den [Erwerb des Add-ons](enable-in-app-purchases-of-apps-and-add-ons.md) über Ihre App tätigt und das Add-On nutzt, muss Ihre App durch Aufrufen der [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx)-Methode der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse das Add-on als erfüllt melden. Sie müssen die folgenden Informationen an diese Methode übergeben:

* Die [Store ID](in-app-purchases-and-trials.md#store_ids) des Add-Ons, das Sie als erfüllt melden möchten.
* Die Einheiten des Add-Ons, das Sie als erfüllt melden möchten.
  * Geben Sie für einen von Entwicklern verwalteten Verbrauchsartikel als Parameter für die *Menge* 1 an. Dadurch wird der Store benachrichtigt, dass der Verbrauchsartikel erfüllt wurde, und der Kunde kann dann den Verbrauchsartikel erneut kaufen. Der Benutzer kann den Verbrauchsartikel erst dann erneut kaufen, wenn Ihre App den Store benachrichtigt hat, dass er erfüllt wurde.
  * Geben Sie für einen vom Store verwalteten Verbrauchsartikel die tatsächliche Anzahl der Einheiten an, die verbraucht wurden. Der Store aktualisiert den Restbetrag für den Verbrauchsartikel.
* Die Tracking-ID für die Erfüllung. Dies ist eine vom Entwickler angegebene GUID, welche die spezielle Transaktion, mit der der Erfüllungsvorgang verknüpft ist, für Nachverfolgungszwecke identifiziert. Weitere Informationen finden Sie in den Hinweisen in [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx).

In diesem Beispiel wird gezeigt, wie ein vom Store verwalteter Verbrauchsartikel als erfüllt gemeldet wird.

```csharp
private StoreContext context = null;

public async void ConsumeAddOn(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // This is an example for a Store-managed consumable, where you specify the actual number
    // of units that you want to report as consumed so the Store can update the remaining
    // balance. For a developer-managed consumable where you maintain the balance, specify 1
    // to just report the add-on as fulfilled to the Store.
    uint quantity = 10;
    string addOnStoreId = "9NBLGGH4TNNR";
    Guid trackingId = Guid.NewGuid();

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.ReportConsumableFulfillmentAsync(
        addOnStoreId, quantity, trackingId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "The fulfillment was successful. Remaining balance: " +
                result.BalanceRemaining;
            break;

        case StoreConsumableStatus.InsufficentQuantity:
            textBlock.Text = "The fulfillment was unsuccessful because the user " +
            "doesn't have enough remaining balance." + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "The fulfillment was unsuccessful due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "The fulfillment was unsuccessful due to a server error.";
            break;

        default:
            textBlock.Text = "The fulfillment was unsuccessful due to an unknown error.";
            break;
    }
}
```

<span id="get_balance" />
## Abrufen des Restbetrags für einen vom Store verwalteten Verbrauchsartikel

Dieses Beispiel zeigt, wie die [GetConsumableBalanceRemainingAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getconsumablebalanceremainingasync.aspx)-Methode der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse verwendet wird, um den Restbetrag für ein vom Store verwaltetes Endverbraucher-Add-on abzurufen.

```csharp
private StoreContext context = null;

public async void GetRemainingBalance(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    string addOnStoreId = "9NBLGGH4TNNR";

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.GetConsumableBalanceRemainingAsync(addOnStoreId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "Remaining balance: " + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "Could not retrieve balance due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "Could not retrieve balance due to a server error.";
            break;

        default:
            textBlock.Text = "Could not retrieve balance due to an unknown error.";
            break;
    }
}
```

## Verwandte Themen

* [In-App-Einkäufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Unterstützen von In-App-Einkäufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->


