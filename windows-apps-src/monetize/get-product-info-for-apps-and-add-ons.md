---
author: mcleanbyron
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: "Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um Store-bezogene Produktinformationen für die aktuelle App oder eines ihrer Add-ons abzurufen."
title: Abrufen von Produktinformationen zu Apps und Add-Ons
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: c453dc74730fc451bbe9babdffb2ce4d72712082

---

# Abrufen von Produktinformationen zu Apps und Add-Ons

Apps für Windows 10, Version 1607 oder höher, können Methoden der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse im [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace verwenden, um auf Store-bezogene Informationen für die aktuelle App oder eines ihrer Add-Ons (auch als In-App-Produkte oder IAPs bezeichnet) zuzugreifen. Die folgenden Beispiele in diesem Artikel zeigen, wie dies für verschiedene Szenarien durchgeführt wird. Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

>**Hinweis**&nbsp;&nbsp;Dieser Artikel bezieht sich auf Apps für Windows 10, Version 1607 oder höher. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie unter [In-App-Einkäufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## Voraussetzungen

Für diese Beispiele gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine UWP (Universelle Windows-Plattform)-App, die für Windows 10, Version 1607 oder höher, geeignet ist.
* Sie haben eine App im Windows Dev Center-Dashboard erstellt. Diese App wird veröffentlicht und ist im Store verfügbar. Dies kann eine App sein, die Sie für Kunden freigeben möchten, oder es kann eine einfache App sein, die den Mindestanforderungen gemäß dem [Zertifizierungskit für Windows-Apps](https://developer.microsoft.com/windows/develop/app-certification-kit) entspricht und nur zu Testzwecken verwendet wird. Weitere Informationen finden Sie unter [Hinweise für Tests](in-app-purchases-and-trials.md#testing).

Der Code in diesen Beispielen geht von Folgendem aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx), die einen [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) mit dem Namen ```workingProgressRing``` und einen [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den **Windows.Services.Store**-Namespace.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Einkäufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## Abrufen von Informationen für die aktuelle App

Verwenden Sie zum Abrufen von Store-Produktinformationen zur aktuellen App die [GetStoreProductForCurrentAppAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getstoreproductforcurrentappasync.aspx)-Methode. Dies ist eine asynchrone Methode, die ein [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekt zurück gibt, das Sie verwenden können, um Informationen wie z. B. den Preis abzurufen.

```csharp
private StoreContext context = null;

public async void GetAppInfo()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get app store product details. Because this might take several moments,   
    // display a ProgressRing during the operation.
    workingProgressRing.IsActive = true;
    StoreProductResult queryResult = await context.GetStoreProductForCurrentAppAsync();
    workingProgressRing.IsActive = false;

    if (queryResult.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {queryResult.ExtendedError.Message}";
        return;
    }

    if (queryResult.Product == null)
    {
        // The Store catalog returned an unexpected result.
        textBlock.Text = "Something went wrong, the product was not returned.";
        return;
    }

    // Display the price of the app.
    textBlock.Text = $"The price of this app is: {queryResult.Product.Price.FormattedBasePrice}";
}
```

## Abrufen von Informationen zu Produkten mit bekannten Store-IDs

Verwenden Sie zum Abrufen von Store-Produktinformationen für Apps oder Add-ons, deren [Store-IDs](in-app-purchases-and-trials.md#store_ids) Sie bereits kennen, die [GetStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706579.aspx)-Methode. Hierbei handelt es sich um eine asynchrone Methode, die eine Sammlung von [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekten zurück gibt, die alle Apps oder Add-ons darstellen. Zusätzlich zu den Store-IDs müssen Sie eine Liste mit Zeichenfolgen an diese Methode übergeben, welche die Typen der Add-ons identifizieren. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie in der [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx)-Eigenschaft.

Im folgenden Beispiel werden Informationen für dauerhafte Add-ons mit den angegebenen Store-IDs abgerufen.

```csharp
private StoreContext context = null;

public async void GetProductInfo()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Specify the kinds of add-ons to retrieve.
    string[] productKinds = { "Durable" };
    List<String> filterList = new List<string>(productKinds);

    // Specify the Store IDs of the products to retrieve.
    string[] storeIds = new string[] { "9NBLGGH4TNMP", "9NBLGGH4TNMN" };

    workingProgressRing.IsActive = true;
    StoreProductQueryResult queryResult =
        await context.GetStoreProductsAsync(filterList, storeIds);
    workingProgressRing.IsActive = false;

    if (queryResult.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {queryResult.ExtendedError.Message}";
        return;
    }

    foreach (KeyValuePair<string, StoreProduct> item in queryResult.Products)
    {
        // Access the Store info for the product.
        StoreProduct product = item.Value;

        // Use members of the product object to access info for the product...
    }
}
```

## Abrufen von Informationen für Add-ons, die für die aktuelle App verfügbar sind

Verwenden Sie zum Abrufen von Store-Produktinformationen für die Add-ons, die für die aktuelle App verfügbar sind, die [GetAssociatedStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706571.aspx)-Methode. Hierbei handelt es sich um eine asynchrone Methode, die eine Sammlung von [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekten zurück gibt, die alle verfügbaren Add-ons darstellen. Sie müssen eine Liste mit Zeichenfolgen an diese Methode übergeben, welche die Typen von Add-ons identifizieren, die Sie abrufen möchten. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie in der [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx)-Eigenschaft.

Im folgenden Beispiel werden Informationen für alle dauerhaften Add-ons, vom Store verwalteten Endverbraucher-Add-ons und von Entwicklern verwalteten Endverbraucher-Add-ons abgerufen.

```csharp
private StoreContext context = null;

public async void GetAddOnInfo()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Specify the kinds of add-ons to retrieve.
    string[] productKinds = { "Durable", "Consumable", "UnmanagedConsumable" };
    List<String> filterList = new List<string>(productKinds);

    workingProgressRing.IsActive = true;
    StoreProductQueryResult queryResult = await context.GetAssociatedStoreProductsAsync(filterList);
    workingProgressRing.IsActive = false;

    if (queryResult.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {queryResult.ExtendedError.Message}";
        return;
    }

    foreach (KeyValuePair<string, StoreProduct> item in queryResult.Products)
    {
        // Access the Store product info for the add-on.
        StoreProduct product = item.Value;

        // Use members of the product object to access listing info for the add-on...
    }
}
```

>**Hinweis**&nbsp;&nbsp;Wenn die App über viele Add-ons verfügt, können Sie alternativ die [GetAssociatedStoreProductsWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706572.aspx)-Methode verwenden, um für die Rückgabe der Add-on-Ergebnisse Paging zu verwenden.


## Abrufen von Informationen zu Add-ons für die aktuelle App, zu deren Verwendung der Benutzer berechtigt ist

Verwenden Sie zum Abrufen von Store-Produktinformationen für Add-ons, zu deren Verwendung der Benutzer berechtigt ist, die [GetUserCollectionAsync](https://msdn.microsoft.com/library/windows/apps/mt706580.aspx)-Methode. Hierbei handelt es sich um eine asynchrone Methode, die eine Sammlung von [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekten zurück gibt, die alle Add-ons darstellen. Sie müssen eine Liste mit Zeichenfolgen an diese Methode übergeben, welche die Typen von Add-ons identifizieren, die Sie abrufen möchten. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie in der [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx)-Eigenschaft.

Im folgenden Beispiel werden Informationen für dauerhafte Add-ons mit den angegebenen Store-IDs abgerufen.

```csharp
private StoreContext context = null;

public async void GetUserCollection()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Specify the kinds of add-ons to retrieve.
    string[] productKinds = { "Durable" };
    List<String> filterList = new List<string>(productKinds);

    workingProgressRing.IsActive = true;
    StoreProductQueryResult queryResult = await context.GetUserCollectionAsync(filterList);
    workingProgressRing.IsActive = false;

    if (queryResult.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {queryResult.ExtendedError.Message}";
        return;
    }

    foreach (KeyValuePair<string, StoreProduct> item in queryResult.Products)
    {
        StoreProduct product = item.Value;

        // Use members of the product object to access info for the product...
    }
}
```

>**Hinweis**&nbsp;&nbsp;Wenn die App über viele Add-ons verfügt, können Sie alternativ die [GetUserCollectionWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706581.aspx)-Methode verwenden, um für die Rückgabe der Add-on-Ergebnisse Paging zu verwenden.

## Verwandte Themen

* [In-App-Einkäufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Einkäufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->


