---
author: mcleanbyron
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: "Wenn Ihre App einen großen In-App-Produktkatalog enthält, können Sie optional das in diesem Thema beschriebene Verfahren zum Verwalten des Katalogs ausführen."
title: "Verwalten eines großen Katalogs mit In-App-Produkten"
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 0927df3cd696e5a6fbd3a235d2b87074f1d63929

---

# Verwalten eines großen Katalogs mit In-App-Produkten


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Wenn Ihre App einen großen In-App-Produktkatalog enthält, können Sie optional das in diesem Thema beschriebene Verfahren zum Verwalten des Katalogs ausführen. Sie erstellen einige wenige Produkteinträge für bestimmte Preisniveaus, mit denen im Katalog jeweils Hunderte von Produkten dargestellt werden können.

Zur Aktivierung dieser Funktion verwenden Sie die [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)-Methodenüberladung, die ein von der App definiertes Angebots angibt, das zu einem In-App-Produkt im Store gehört. Neben der Angabe einer Zuordnung eines Angebots zu einem Produkt während des Aufrufs sollte Ihre App auch ein [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384)-Objekt übergeben, das die Angebotsdetails des großen Katalogs enthält. Wenn diese Details nicht angegeben sind, werden stattdessen die Details für das gelistete Produkt verwendet.

Im Store wird nur die *offerId* aus der Kaufanforderung des entsprechenden [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) verwendet. Mit diesem Verfahren werden die Informationen, die ursprünglich bei der [Eintragung des In-App-Produkts im Store](https://msdn.microsoft.com/library/windows/apps/mt148551) bereitgestellt wurden, nicht direkt geändert.


            **Hinweis**  Ab Windows10 ist die Anzahl von Produkteinträgen pro Entwicklerkonto im Store nicht länger eingeschränkt. In früheren Versionen galt eine Store-Beschränkung von 200 Produkteinträgen pro Entwicklerkonto; mit dem in diesem Thema beschriebenen Verfahren kann diese Beschränkung umgangen werden.

## Voraussetzungen

-   In diesem Thema wird die Store-Unterstützung für die Darstellung mehrerer In-App-Angebote mithilfe eines einzelnen im Store aufgeführten In-App-Produkts erläutert. Wenn Sie mit In-App-Käufen noch nicht vertraut sind, lesen Sie [Aktivieren von In-App-Produktkäufen](enable-in-app-product-purchases.md). Dort finden Sie Lizenzinformationen und eine Anleitung zur richtigen Eintragung Ihres In-App-Kaufs im Store.
-   Wenn Sie Code für neue In-App-Angebote erstmalig schreiben und testen, müssen Sie anstelle des [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)-Objekts das [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)-Objekt verwenden. Auf diese Weise können Sie überprüfen, ob die Lizenzlogik simulierte Aufrufe an den Lizenzserver und nicht an den Liveserver verwendet. Dazu müssen Sie die Datei mit dem Namen „WindowsStoreProxy.xml“ in %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData anpassen. Diese Datei wird vom Simulator in Microsoft Visual Studio erstellt, wenn Sie Ihre App erstmalig ausführen. Sie können jedoch auch eine benutzerdefinierte Version dieser Datei zur Laufzeit laden. Weitere Informationen finden Sie unter **CurrentAppSimulator**.
-   In diesem Thema wird auch auf Codebeispiele verwiesen, die im [Store-Beispiel](http://go.microsoft.com/fwlink/p/?LinkID=627610) zu finden sind. Dieses Beispiel bietet eine hervorragende Möglichkeit, die verschiedenen Monetarisierungsoptionen zu testen, die für universelle Windows-Plattform (UWP)-Apps verfügbar sind.

## Durchführen der Kaufanforderung für das In-App-Produkt

Die Kaufanforderung für ein bestimmtes Produkt in einem umfangreichen Katalog wird ähnlich gehandhabt wie andere In-App-Kaufanforderungen. Wenn Ihre App die neue [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)-Methodenüberladung aufruft, stellt die App sowohl ein *OfferId* - als auch ein [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263390)-Objekt mit dem Namen des In-App-Produkts bereit.

```CSharp
string offerId = "1234";
string displayPropertiesName = "MusicOffer1";
var displayProperties = new ProductPurchaseDisplayProperties(displayPropertiesName);

try
{
    PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1", offerId, displayProperties);
    switch (purchaseResults.Status)
    {
        case ProductPurchaseStatus.Succeeded:
            // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotFulfilled:
            // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
            // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotPurchased:
            // Notify user that the purchase was not completed due to cancellation or error.
            break;
    }
}
catch (Exception)
{
    //Notify the user of the purchase error.
}
```

## Melden der Erfüllung des In-App-Angebots

Die App muss die Produkterfüllung an den Store melden, sobald die lokale Erfüllung für das Angebot abgeschlossen ist. Wenn die App die Erfüllung des Angebots bei Verwendung eines großen Katalogs nicht meldet, kann der Benutzer keine In-App-Angebote erwerben, für die der gleiche Store-Produkteintrag genutzt wird.

Wie bereits erwähnt verwendet der Store nur bereitgestellte Angebotsinformationen zum Auffüllen des [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392)-Elements und erstellt keine beständige Zuordnung zwischen einem Angebot aus einem umfangreichen Katalog und einem Produkteintrag im Store. Daher müssen Sie die Benutzerberechtigungen für Produkte aus umfangreichen Katalogen nachverfolgen und dem Benutzer produktspezifischen Kontext (wie z.B. den Namen des angebotenen Artikels oder Details dazu) außerhalb des [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)-Vorgangs zur Verfügung stellen.

Der folgende Code veranschaulicht den Erfüllungsaufruf sowie ein Muster für Meldungen auf der Benutzeroberfläche, in das die angebotsspezifischen Informationen eingefügt werden. Da keine produktspezifischen Informationen vorhanden sind, werden im Beispiel Informationen aus dem [**ListingInformation**](https://msdn.microsoft.com/library/windows/apps/br225163)-Element des Produkts verwendet.

```CSharp
string offerId = "1234";
product1ListingName = product1.Name;
string displayPropertiesName = "MusicOffer1";

if (String.IsNullOrEmpty(displayPropertiesName))
{
    displayPropertiesName = product1ListingName;
}
var offerIdMsg = " with offer id " + offerId;
if (String.IsNullOrEmpty(offerId))
{
    offerIdMsg = " with no offer id";
}

FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync(productId, transactionId);
switch (result)
{
    case FulfillmentResult.Succeeded:
        Log("You bought and fulfilled " + displayPropertiesName + offerIdMsg, NotifyType.StatusMessage);
        break;
    case FulfillmentResult.NothingToFulfill:
        Log("There is no purchased product 1 to fulfill.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchasePending:
        Log("You bought product 1. The purchase is pending so we cannot fulfill the product.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchaseReverted:
        Log("You bought product 1. But your purchase has been reverted.", NotifyType.StatusMessage);
        // Since the user' s purchase was revoked, they got their money back.
        // You may want to revoke the user' s access to the consumable content that was granted.
        break;
    case FulfillmentResult.ServerError:
        Log("You bought product 1. There was an error when fulfilling.", NotifyType.StatusMessage);
        break;
}
```

## Verwandte Themen

* [Unterstützen von In-App-Produktkäufen](enable-in-app-product-purchases.md)
* [Unterstützen von Käufen konsumierbarer In-App-Produkte](enable-consumable-in-app-product-purchases.md)
* [Store-Beispiel (zeigt Testversionen und In-App-Einkäufe)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384)



<!--HONumber=Jun16_HO4-->


