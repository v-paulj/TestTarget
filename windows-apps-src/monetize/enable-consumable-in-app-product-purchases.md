---
author: mcleanbyron
Description: "Sie können konsumierbare In-App-Produkte – Artikel, die gekauft, verwendet und wieder gekauft werden können – über die Store-Handelsplattform anbieten, um Ihren Kunden eine stabile und zuverlässige Kaufumgebung bereitzustellen."
title: "Käufe von konsumierbaren In-App-Produkten aktivieren"
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: In-App-Angebot, Codebeispiel
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 15092f726283f36c8dc5970157d3cd54dea9b837

---

# Käufe von konsumierbaren In-App-Produkten aktivieren




>**Hinweis**&nbsp;&nbsp;In diesem Artikel wird die Verwendung von Mitgliedern des [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) Namespace erläutert. Wenn Ihre App für Windows10, Version 1607 oder höher, vorgesehen ist, empfehlen wir die Verwendung von Mitgliedern des [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace zum Verwalten von Add-Ons (auch als In-App-Produkte oder IAPs bezeichnet) anstelle des **Windows.ApplicationModel.Store**-Namespace. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md).

Sie können In-App-Käufe von konsumierbaren Produkten – Artikel, die gekauft, verwendet und wieder gekauft werden können – über die Store-Handelsplattform anbieten, um den Kunden beim Kauf Stabilität und Zuverlässigkeit zu bieten. Dies ist besonders nützlich für Dinge wie spielinterne Währungen (Gold, Münzen usw.), die gekauft und dann zum Erwerben bestimmter Power-Ups verwendet werden können.

## Voraussetzungen

-   In diesem Thema erfahren Sie, wie In-App-Produktkäufe von Consumables erfüllt und gemeldet werden. Wenn Sie mit In-App-Produkten noch nicht vertraut sind, lesen Sie [Aktivieren von In-App-Produktkäufen](enable-in-app-product-purchases.md). Dort finden Sie Lizenzinformationen und eine Anleitung zur richtigen Eintragung Ihrer In-App-Produkte im Store.
-   Wenn Sie Code für neue In-App-Produkte erstmalig schreiben und testen, müssen Sie anstelle des [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)-Objekts das [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)-Objekt verwenden. Auf diese Weise können Sie überprüfen, ob die Lizenzlogik simulierte Aufrufe an den Lizenzserver und nicht an den Liveserver verwendet. Dazu müssen Sie die Datei mit dem Namen „WindowsStoreProxy.xml“ in %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData anpassen. Diese Datei wird vom Simulator in Microsoft Visual Studio erstellt, wenn Sie Ihre App erstmalig ausführen. Sie können jedoch auch eine benutzerdefinierte Version dieser Datei zur Laufzeit laden. Weitere Informationen finden Sie unter **CurrentAppSimulator**.
-   In diesem Thema wird auch auf Codebeispiele verwiesen, die im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store) zu finden sind. Dieses Beispiel bietet eine hervorragende Möglichkeit, die verschiedenen Monetarisierungsoptionen zu testen, die für Universelle Windows-Plattform (UWP)-Apps verfügbar sind.

## Schritt 1: Durchführen der Kaufanforderung

Die erste Kaufanforderung wird wie bei jedem Kauf über den Store mit [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381) durchgeführt. Der Unterschied bei konsumierbaren In-App-Produkten besteht darin, dass ein Kunde nach einem erfolgreichen Kauf das gleiche Produkt erst dann erneut kaufen kann, wenn die App den Store benachrichtigt hat, dass der vorige Kauf des Produkts erfolgreich erfüllt wurde. Ihre App ist dafür verantwortlich, gekaufte Consumables zu erfüllen und den Store über die Erfüllung in Kenntnis zu setzen.

Im nächsten Beispiel wird eine In-App-Kaufanforderung für ein Consumable gezeigt. Sie sehen Codekommentare, die angeben, wann Ihre App den Kauf des konsumierbaren In-App-Produkts lokal erfüllen sollte, und zwar für zwei verschiedene Szenarien – eine erfolgreiche Kaufanforderung und eine nicht erfolgreiche Kaufanforderung aufgrund eines nicht erfüllten Kaufs desselben Produkts.

```CSharp
PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1");
switch (purchaseResults.Status)
{
    case ProductPurchaseStatus.Succeeded:
        product1TempTransactionId = purchaseResults.TransactionId;

        // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;

    case ProductPurchaseStatus.NotFulfilled:
        product1TempTransactionId = purchaseResults.TransactionId;

        // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
        // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;
}
```

## Schritt 2: Verfolgen der lokalen Kauferfüllung des Consumable

Wenn Sie dem Kunden Zugriff auf das konsumierbare In-App-Produkt gewähren, müssen Sie dokumentieren, für welches Produkt der Kauf erfüllt wird (*productId*) und mit welcher Transaktion diese Erfüllung verbunden ist (*transactionId*).

**Wichtig**  Ihre App ist verantwortlich dafür, dass der Store ordnungsgemäß von der Erfüllung in Kenntnis gesetzt wird. Dieser Schritt ist die Grundlage dafür, dass der Kauf von den Kunden als fair und zuverlässig wahrgenommen wird.

Im folgenden Beispiel wird die Verwendung von [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392)-Eigenschaften aus dem [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381)-Aufruf im vorangehenden Schritt zum Identifizieren des Produkts für die Kauferfüllung gezeigt. Die Produktinformationen werden mithilfe eines Arrays an einem Ort gespeichert, der später referenziert werden kann, um zu bestätigen, dass die lokale Erfüllung erfolgreich war.

```CSharp
private void GrantFeatureLocally(string productId, Guid transactionId)
{
    if (!grantedConsumableTransactionIds.ContainsKey(productId))
    {
        grantedConsumableTransactionIds.Add(productId, new List<Guid>());
    }
    grantedConsumableTransactionIds[productId].Add(transactionId);

    // Grant the user their content. You will likely increase some kind of gold/coins/some other asset count.
}
```

Im folgenden Beispiel wird die Verwendung des Arrays aus dem vorhergehenden Beispiel gezeigt, um auf Produkt-ID/Transaktions-ID-Paare zuzugreifen, die später beim Melden der Erfüllung an den Store verwendet werden.

**Wichtig**  Welche Methode Ihre App auch immer verwendet, um die Erfüllung nachzuverfolgen und zu bestätigen – die App muss die erforderliche Sorgfalt aufweisen, sodass Ihren Kunden keine Produkte in Rechnung gestellt werden, die sie nicht erhalten haben.

```CSharp
private Boolean IsLocallyFulfilled(string productId, Guid transactionId)
{
    return grantedConsumableTransactionIds.ContainsKey(productId) && grantedConsumableTransactionIds[productId].Contains(transactionId);
}
```

## Schritt 3: Melden der Produkterfüllung an den Store

Nachdem die lokale Erfüllung abgeschlossen wurde, muss Ihre App einen [**ReportConsumableFulfillmentAsync**](https://msdn.microsoft.com/library/windows/apps/dn263380)-Aufruf ausführen, der die *productId* und die Transaktion des Produktkaufs enthält.

**Wichtig**  Wenn erfüllte Käufe konsumierbarer In-App-Produkte nicht an den Store gemeldet werden, kann der Benutzer das Produkt erst dann erneut kaufen, wenn die Erfüllung des vorangehenden Kaufs gemeldet wird.

```CSharp
FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync("product2", product2TempTransactionId);
```

## Schritt 4: Identifizieren nicht erfüllter Einkäufe

Ihre App kann jederzeit mithilfe der [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379)-Methode eine Überprüfung auf nicht erfüllte Käufe konsumierbarer In-App-Produkte durchführen. Diese Methode sollte regelmäßig aufgerufen werden, um nicht erfüllte Käufe konsumierbarer Produkte zu ermitteln, die aufgrund unerwarteter Ereignisse in der App aufgetreten sind, beispielsweise Unterbrechungen der Netzwerkverbindung oder Beenden der App.

Im folgenden Beispiel wird gezeigt, auf welche Weise [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) verwendet werden kann, um nicht erfüllte Käufe konsumierbarer Produkte aufzuzählen, und wie Ihre App diese Liste iterieren kann, um die lokale Erfüllung abzuschließen.

```CSharp
private async void GetUnfulfilledConsumables()
{
    products = await CurrentApp.GetUnfulfilledConsumablesAsync();

    foreach (UnfulfilledConsumable product in products)
    {
        logMessage += "\nProduct Id: " + product.ProductId + " Transaction Id: " + product.TransactionId;
        // This is where you would pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
    // To indicate local fulfillment to the Windows Store.
    }
}
```

## Verwandte Themen

* [Unterstützen von Käufen von In-App-Produkten](enable-in-app-product-purchases.md)
* [Store-Beispiel (zeigt Testversionen und In-App-Käufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 



<!--HONumber=Aug16_HO5-->


