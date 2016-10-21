---
author: mcleanbyron
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: "Verwenden Sie diese Methode aus der Windows Store Collection-API, um den Kauf eines Verbrauchsprodukt für einen bestimmten Kunden als abgewickelt zu melden. Damit ein Benutzer ein Verbrauchsprodukt erneut erwerben kann, muss Ihre App oder Ihr Dienst das Verbrauchsprodukt für den betreffenden Benutzer als abgewickelt melden."
title: "Melden von Verbrauchsprodukten als erfüllt"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: dd3e687d49e538187c123b7123c184f9182905de

---

# Melden von Verbrauchsprodukten als erfüllt




Verwenden Sie diese Methode aus der Windows Store Collection-API, um den Kauf eines Verbrauchsprodukt für einen bestimmten Kunden als abgewickelt zu melden. Damit ein Benutzer ein Verbrauchsprodukt erneut erwerben kann, muss Ihre App oder Ihr Dienst das Verbrauchsprodukt für den betreffenden Benutzer als erfüllt melden.

Sie können diese Methode auf zwei Weisen verwenden, um ein Verbrauchsprodukt als erfüllt zu melden:

-   Geben Sie die (im **itemId**-Parameter einer [Produktabfrage](query-for-products.md)) zurückgegebene) Artikelkennung des Verbrauchsprodukts und eine von Ihnen bereitgestellte eindeutige Tracking-ID an. Wenn die gleiche Tracking-ID für mehrere Versuche verwendet wird, wird auch dann das gleiche Ergebnis zurückgegeben, wenn der Artikel bereits in Anspruch genommen wurde. Wenn Sie nicht sicher sind, ob eine Verbrauchsanforderung erfolgreich war, sollte Ihr Dienst Verbrauchsanforderungen mit derselben Tracking-ID erneut übermitteln. Die Tracking-ID ist immer mit der jeweiligen Verbrauchsanforderung verknüpft und kann beliebig oft erneut übermittelt werden.
-   Geben Sie die (im **productId**-Parameter einer [Produktabfrage](query-for-products.md) zurückgegebene) Produkt-ID und eine Transaktions-ID an, die aus einer der Quellen abgerufen wird, die in der Beschreibung für den **transactionId**-Parameter im Abschnitt „Anforderungstext“ unten aufgeführt sind.

## Voraussetzungen


Zur Verwendung dieser Methode benötigen Sie:

-   Ein AzureAD-Zugriffstoken, das mit dem Zielgruppen-URI `https://onestore.microsoft.com` erstellt wurde.
-   Einen WindowsStore-ID-Schlüssel, der durch Aufrufen der [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674)-Methode im clientseitigen Code der App generiert wurde.

Weitere Informationen finden Sie unter [Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md).

## Anforderung


### Anforderungssyntax

| Methode | Anforderungs-URI                                                   |
|--------|---------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |

<span/> 

### Anforderungsheader

| Header         | Typ   | Beschreibung                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorisierung  | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;.                           |
| Host           | string | Muss auf den Wert **collections.mp.microsoft.com** festgelegt werden.                                            |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Inhaltstyp   | string | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |

<span/>

### Anforderungstext

| Parameter     | Typ         | Beschreibung         | Erforderlich |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | Der Benutzer, für den dieser Artikel genutzt wird.                                                                                                                                                                                                                                                                 | Ja      |
| itemId        | String       | Der von einer [Produktabfrage](query-for-products.md) zurückgegebene „itemId“-Wert. Verwenden Sie diesen Parameter mit „trackingId“.                                                                                                                                                                                                  | Nein       |
| trackingId    | GUID         | Eine eindeutige, vom Entwickler angegebene Tracking-ID. Verwenden Sie diesen Parameter mit „itemId“.                                                                                                                                                                                                                                     | Nein       |
| Produkt-ID     | String       | Der von einer [Produktabfrage](query-for-products.md) zurückgegebene „productId“-Wert. Verwenden Sie diesen Parameter mit „transactionId“.                                                                                                                                                                                            | Nein       |
| transactionId | GUID         | Ein Transaktions-ID-Wert, der aus einer der folgenden Quellen abgerufen wird. Verwenden Sie diesen Parameter mit „productId“.  <br/><br/><ul><li>Die [TransactionID](https://msdn.microsoft.com/library/windows/apps/dn263396)-Eigenschaft der [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392)-Klasse.</li><li>Der von [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381), [RequestAppPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/hh967813) oder [GetAppReceiptAsync](https://msdn.microsoft.com/library/windows/apps/hh967811) zurückgegebene App- oder Produktbeleg.</li><li>Der von einer [Produktabfrage](query-for-products.md)zurückgegebene transactionId-Parameter.</li></ul>                                                                                                                                                                                                                                   | Nein       |

 
<span/>

Das UserIdentity-Objekt enthält die folgenden Parameter.

| Parameter            | Typ   | Beschreibung                                                                                                                                 | Erforderlich |
|----------------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------|----------|
| identityType         | string | Gibt den Zeichenfolgenwert **b2b** an.                                                                                                           | Ja      |
| identityValue        | string | Zeichenfolgenwert des Windows Store-ID-Schlüssels.                                                                                                   | Ja      |
| localTicketReference | string | Angeforderter Bezeichner für die zurückgegebene Antwort. Es wird empfohlen, denselben Wert als *userId*-Anspruch im Windows Store-ID-Schlüssel zu verwenden. | Ja      |

<span/> 

### Anforderungsbeispiele

Im folgenden Beispiel werden *itemId* und *trackingId* verwendet.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

Im folgenden Beispiel werden *productId* und *transactionId* verwendet.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```

## Antwort


Bei erfolgreicher Nutzung wird kein Inhalt zurückgegeben.

### Antwortbeispiel

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## Fehlercodes


| Code | Fehler        | Interner Fehlercode           | Beschreibung                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Nicht autorisiert | AuthenticationTokenInvalid | Das Azure AD-Zugriffstoken ist ungültig. In einigen Fällen enthalten die Details zu ServiceError weitere Informationen, z. B. wenn das Token abgelaufen ist oder der *appid*-Anspruch fehlt. |
| 401  | Nicht autorisiert | PartnerAadTicketRequired   | An den Dienst wurde im Autorisierungsheader kein Azure AD-Zugriffstoken übergeben.                                                                                                   |
| 401  | Nicht autorisiert | InconsistentClientId       | Der *clientId*-Anspruch im Windows Store-ID-Schlüssel im Anforderungstext und der *appid*-Anspruch im Azure AD-Zugriffstoken im Autorisierungsheader stimmen nicht überein.                     |

<span/> 

## Verwandte Themen

* [Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md)
* [Produktabfrage](query-for-products.md)
* [Gewähren kostenloser Produkte](grant-free-products.md)
* [Verlängern eines Windows Store-ID-Schlüssels](renew-a-windows-store-id-key.md)
 

 



<!--HONumber=Aug16_HO3-->


