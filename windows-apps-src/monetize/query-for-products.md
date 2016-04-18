---
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: Verwenden Sie diese Methode in der Windows Store-Sammlungs-API, um alle Produkte, die sich im Besitz eines Kunden befinden, für Apps abzurufen, die Ihrer Azure AD-Client-ID zugeordnet sind. Sie können die Abfrage auf ein bestimmtes Produkt beschränken oder weitere Filter verwenden.
title: Produktabfrage
---

# Produktabfrage


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Verwenden Sie diese Methode in der Windows Store-Sammlungs-API, um alle Produkte, die sich im Besitz eines Kunden befinden, für Apps abzurufen, die Ihrer Azure AD-Client-ID zugeordnet sind. Sie können die Abfrage auf ein bestimmtes Produkt beschränken oder weitere Filter verwenden.

Diese Methode ist so konzipiert, dass sie von Ihrem Dienst als Reaktion auf eine Nachricht von Ihrer App aufgerufen wird. Der Dienst sollte nicht nach einem regelmäßigen Zeitplan alle Benutzer abfragen.

## Voraussetzungen


Zur Verwendung dieser Methode benötigen Sie:

-   Ein Azure AD-Zugriffstoken, das mit dem Zielgruppen-URI **https://onestore.microsoft.com** erstellt wurde.
-   Einen Windows Store-ID-Schlüssel, der durch Aufrufen der [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674)-Methode im clientseitigen Code der App generiert wurde.

Weitere Informationen finden Sie unter [Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md).

## Anforderung

### Anforderungssyntax

| Methode | Anforderungs-URI                                                 |
|--------|-------------------------------------------------------------|
| POST   | https://collections.mp.microsoft.com/v6.0/collections/query |

 
### Anforderungsheader

| Header         | Typ   | Beschreibung                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;.                           |
| Host           | string | Muss auf den Wert **collections.mp.microsoft.com** festgelegt werden.                                            |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | string | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |

 

### Anforderungstext

| Parameter         | Typ         | Beschreibung                                                                                                                                                                                                                                                          | Erforderlich |
|-------------------|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| beneficiaries     | UserIdentity | Ein UserIdentity-Objekt, das den Benutzer darstellt, von dem Produkte abgefragt werden.                                                                                                                                                                                           | Ja      |
| continuationToken | string       | Bei mehreren Produktgruppen gibt der Antworttext ein Fortsetzungstoken zurück, sobald das Seitenlimit erreicht ist. Geben Sie das Fortsetzungstoken hier bei nachfolgenden Aufrufen an, um die verbleibenden Produkte abzurufen.                                                      | Nein       |
| maxPageSize       | number       | Die maximale Anzahl von Produkten, die in einer Antwort zurückgegeben werden sollen. Der standardmäßige Höchstwert ist 100.                                                                                                                                                                      | Nein       |
| modifiedAfter     | datetime     | Falls angegeben, gibt der Dienst nur Produkte zurück, die nach diesem Datum geändert wurden.                                                                                                                                                                             | Nein       |
| parentProductId   | string       | Falls angegeben, gibt der Dienst nur IAPs zurück, die der angegebenen App entsprechen.                                                                                                                                                                                    | Nein       |
| productSkuIds     | ProductSkuId | Falls angegeben, gibt der Dienst nur Produkte für die angegebenen Produkt-SKU-Paare zurück.                                                                                                                                                                        | Nein       |
| productTypes      | string       | Falls angegeben, gibt der Dienst nur Produkte zurück, die den angegebenen Produkttypen entsprechen. Unterstützte Produkttypen sind **Application**, **Durable** und **UnmanagedConsumable**.                                                                                       | Nein       |
| validityType      | string       | Bei Festlegung auf **Alle** werden alle Produkte für einen Benutzer, einschließlich abgelaufener Elemente, zurückgegeben. Bei Festlegung auf **Valid** werden nur Produkte zurückgegeben, die zum aktuellen Zeitpunkt gültig sind (aktiver Status, Startdatum &lt; Istdatum und Enddatum &gt; Istdatum). | Nein       |

 

Das UserIdentity-Objekt enthält die folgenden Parameter.

| Parameter            | Typ   | Beschreibung                                                                                                                                                                                                                  | Erforderlich |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| identityType         | string | Gibt den Zeichenfolgenwert **b2b** an.                                                                                                                                                                                            | Ja      |
| identityValue        | string | Zeichenfolgenwert des Windows Store-ID-Schlüssels.                                                                                                                                                                                    | Ja      |
| localTicketReference | string | Angeforderter Bezeichner für die zurückgegebenen Produkte. Die zurückgegebenen Elemente im Antworttext verfügen über ein übereinstimmendes *localTicketReference*-Element. Es wird empfohlen, denselben Wert als *userId*-Anspruch im Windows Store-ID-Schlüssel zu verwenden. | Ja      |

 

Das ProductSkuId-Objekt enthält die folgenden Parameter.

| Parameter | Typ   | Beschreibung                                                                                                                                                                                                                                                                                                            | Erforderlich |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| productId | string | Die Produkt-ID aus dem Windows Store-Katalog. Wenn Sie Ihre Produkt-ID ermitteln möchten, navigieren Sie im Windows Dev Center-Dashboard zu Ihrer App, wechseln zur Seite **App-Verwaltung** &gt; **App-Identität** und rufen das Suffix der im Feld **URL für Windows 10** dargestellten Zeichenfolge ab. Beispiel für eine Produkt-ID: 9WZDNCRFJ3Q8. | Ja      |
| skuID     | string | Die SKU-ID aus dem Windows Store-Katalog. Beispiel für eine SKU-ID: 0010.                                                                                                                                                                                                                                                | Ja      |

 

### Anforderungsbeispiel

```
POST https://collections.mp.microsoft.com/v6.0/collections/query HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q…….
Host: collections.mp.microsoft.com
Content-Length: 2531
Content-Type: application/json

{
    "maxPageSize": 100,
    "beneficiaries": [
            {
                "localTicketReference": "1055521810674918",
                "identityValue": "eyJ0eXAiOiJ……",
                "identityType": "b2b"
            }
        ],
    "modifiedAfter": "\/Date(-62135568000000)\/",
    "productSkuIds": [
            {
                "productId": "9NBLGGH5WVP6",
                "skuId": "0010"
            }
        ],
    "productTypes": [
            "UnmanagedConsumable"
        ],
    "validityType": "All"
}
```

## Antwort


### Antworttext

| Parameter         | Typ                     | Beschreibung                                                                                                                                                                                | Erforderlich |
|-------------------|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| continuationToken | string                   | Bei mehreren Produktgruppen wird dieses Token zurückgegeben, sobald das Seitenlimit erreicht ist. Sie können dieses Fortsetzungstoken in nachfolgenden Aufrufen angeben, um verbleibende Produkte abzurufen. | Nein       |
| Items             | CollectionItemContractV6 | Ein Array von Produkten für den angegebenen Benutzer.                                                                                                                                               | Nein       |

 

Das CollectionItemContractV6-Objekt enthält die folgenden Parameter.

| Parameter            | Typ               | Beschreibung                                                                                                                                        | Erforderlich |
|----------------------|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| acquiredDate         | datetime           | Das Datum, an dem der Benutzer den Artikel erworben hat.                                                                                                      | Ja      |
| campaignId           | string             | Die Kampagnen-ID, die beim Kauf dieses Artikels angegeben wurde.                                                                                  | Nein       |
| devOfferId           | string             | Die Angebotskennung aus einem In-App-Kauf.                                                                                                              | Nein       |
| endDate              | datetime           | Das Enddatum des Artikels.                                                                                                                          | Ja      |
| fulfillmentData      | string             | N/V                                                                                                                                                | Nein       |
| inAppOfferToken      | string             | Die vom Entwickler angegebene Produkt-ID-Zeichenfolge, die dem Element im Windows Dev Center-Dashboard zugewiesen wurde. Beispiel für eine Produkt-ID: product123. | Nein       |
| itemId               | string             | Eine ID zur Unterscheidung dieses Sammlungselements von anderen Artikeln des Benutzers. Diese ID ist pro Produkt eindeutig.                                          | Ja      |
| localTicketReference | string             | Die ID des zuvor bereitgestellten `localTicketReference`-Elements im Anforderungstext.                                                                      | Ja      |
| modifiedDate         | datetime           | Das Datum, an dem dieser Artikel zuletzt geändert wurde.                                                                                                              | Ja      |
| orderId              | string             | Die Auftrags-ID, mit der dieser Artikel erworben wurde (sofern angegeben).                                                                                          | Nein       |
| orderLineItemId      | string             | Die Position des Auftrags, für den dieser Artikel erworben wurde (sofern vorhanden).                                                                | Nein       |
| ownershipType        | string             | Die Zeichenfolge „OwnedByBeneficiary“.                                                                                                                   | Ja      |
| productId            | string             | Die Produkt-ID aus dem Windows Store-Katalog. Beispiel für eine Produkt-ID: 9WZDNCRFJ3Q8.                                                            | Ja      |
| productType          | string             | Einer der folgenden Produkttypen: **Application**, **Durable** und **UnmanagedConsumable**.                                                     | Ja      |
| purchasedCountry     | string             | N/V                                                                                                                                               | Nein       |
| purchaser            | IdentityContractV6 | Die Identität des Artikelkäufers (sofern vorhanden). Details zu diesem Objekt finden Sie weiter unten.                                      | Nein       |
| Quantity             | number             | Die Artikelmenge. Derzeit beträgt die Menge immer 1.                                                                                        | Nein       |
| skuId                | string             | Die SKU-ID aus dem Windows Store-Katalog. Beispiel für eine SKU-ID: 0010.                                                                            | Ja      |
| skuType              | string             | Der SKU-Typ. Mögliche Werte sind **Trial**, **Full** und **Rental**.                                                                      | Ja      |
| startDate            | datetime           | Das Datum, ab dem der Artikel gültig ist.                                                                                                         | Ja      |
| Status               | string             | Der Status des Elements. Mögliche Werte sind **Active**, **Expired**, **Revoked** und **Banned**.                                              | Ja      |
| Tags                 | string             | N/V                                                                                                                                                | Ja      |
| transactionId        | guid               | Die aus dem Artikelkauf resultierende Transaktions-ID. Kann verwendet werden, um zu melden, dass der Artikelkauf abgewickelt wurde.                                       | Ja      |

 

Das IdentityContractV6-Objekt enthält die folgenden Parameter.

| Parameter     | Typ   | Beschreibung                                                                        | Erforderlich |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | string | Enthält den Wert **"pub"**.                                                      | Ja      |
| identityValue | string | Der Zeichenfolgenwert von *publisherUserId* aus dem angegebenen Windows Store-ID-Schlüssel. | Ja      |

 

### Antwortbeispiel

```
HTTP/1.1 200 OK
Content-Length: 7241
Content-Type: application/json
MS-CorrelationId: 699681ce-662c-4841-920a-f2269b2b4e6c
MS-RequestId: a9988cf9-652b-4791-beba-b0e732121a12
MS-CV: xu2HW6SrSkyfHyFh.0.1
MS-ServerId: 020022359
Date: Tue, 22 Sep 2015 20:28:18 GMT

{
    "items" : [
        {
            "acquiredDate" : "2015-09-22T19:22:51.2068724+00:00",
            "devOfferId" : "f9587c53-540a-498b-a281-8a349491ed47",
            "endDate" : "9999-12-31T23:59:59.9999999+00:00",
            "fulfillmentData" : [],
            "inAppOfferToken" : "consumable2",
            "itemId" : "4b8fbb13127a41f299270ea668681c1d",
            "localTicketReference" : "1055521810674918",
            "modifiedDate" : "2015-09-22T19:22:51.2513155+00:00",
            "orderId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31",
            "ownershipType" : "OwnedByBeneficiary",
            "productId" : "9NBLGGH5WVP6",
            "productType" : "UnmanagedConsumable",
            "purchaser" : {
                "identityType" : "pub",
                "identityValue" : "user123"
            },
            "skuId" : "0010",
            "skuType" : "Full",
            "startDate" : "2015-09-22T19:22:51.2068724+00:00",
            "status" : "Active",
            "tags" : [],
            "transactionId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31"
        }
    ]
}
```

## Verwandte Themen

* [Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md)
* [Melden von Verbrauchsprodukten als erfüllt](report-consumable-products-as-fulfilled.md)
* [Gewähren kostenloser Produkte](grant-free-products.md)
* [Verlängern eines Windows Store-ID-Schlüssels](renew-a-windows-store-id-key.md)


<!--HONumber=Mar16_HO1-->


