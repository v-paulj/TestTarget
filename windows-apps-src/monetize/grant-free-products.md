---
author: mcleanbyron
ms.assetid: FA55C65C-584A-4B9B-8451-E9C659882EDE
description: "Verwenden Sie diese Methode in der Windows Store-Einkaufs-API, um einem bestimmten Benutzer eine kostenlose App oder ein kostenloses In-App-Produkt (IAP) zu gewähren."
title: "Gewähren kostenloser Produkte"
translationtype: Human Translation
ms.sourcegitcommit: f7e67a4ff6cb900fb90c5d5643e2ddc46cbe4dd2
ms.openlocfilehash: 64c600460c1cbcbd6bb486649e2bc98298ca9dbe

---

# Gewähren kostenloser Produkte

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Verwenden Sie diese Methode in der Windows Store-Einkaufs-API, um einem bestimmten Benutzer eine kostenlose App oder ein kostenloses In-App-Produkt (IAP) zu gewähren.

Derzeit können Sie nur kostenlose Produkte gewähren. Wenn Ihr Dienst versucht, diese Methode zum Gewähren eines nicht kostenlosen Produkts zu verwenden, gibt diese Methode einen Fehler zurück.

## Voraussetzungen

Zur Verwendung dieser Methode benötigen Sie:

-   Ein AzureAD-Zugriffstoken, das mit dem Zielgruppen-URI `https://onestore.microsoft.com` erstellt wurde.
-   Einen Windows Store-ID-Schlüssel, der durch Aufrufen der [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675)-Methode im clientseitigen Code der App generiert wurde.

Weitere Informationen finden Sie unter [Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md).

## Anforderung


### Anforderungssyntax

| Methode | Anforderungs-URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v6.0/purchases/grant``` |

<span/> 

### Anforderungsheader

| Header         | Typ   | Beschreibung                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorisierung  | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer**&lt;*token*&gt;.                           |
| Host           | string | Muss auf den Wert **collections.mp.microsoft.com** festgelegt werden.                                            |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Inhaltstyp   | string | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |

<span/>

### Anforderungstext

| Parameter      | Typ   | Beschreibung                                                                                                                                                                                                                                                                                                            | Erforderlich |
|----------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| availabilityId | string | Die Verfügbarkeits-ID des Produkts, das aus dem Windows Store-Katalog erworben werden soll.                                                                                                                                                                                                                                     | Ja      |
| b2bKey         | string | Der Windows Store-ID-Schlüssel, der die Identität des Kunden darstellt.                                                                                                                                                                                                                                                        | Ja      |
| devOfferId     | string | Eine vom Entwickler angegebene Angebotskennung, die nach dem Kauf im Auflistungselement angezeigt wird.                                                                                                                                                                                                                                 | Nein       |
| language       | string | Die Sprache des Benutzers.                                                                                                                                                                                                                                                                                              | Ja      |
| market         | string | Der Markt des Benutzers.                                                                                                                                                                                                                                                                                                | Ja      |
| orderId        | guid   | Eine für den Auftrag generierte GUID. Dieser Wert muss für den Benutzer eindeutig sein, aber nicht auftragsübergreifend.                                                                                                                                                                                              | Ja      |
| Produkt-ID      | string | Die Store-ID aus dem Windows Store-Katalog. Die Store-ID ist auf der [Seite mit der App-Identität](../publish/view-app-identity-details.md) des DevCenter-Dashboards verfügbar. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. | Ja      |
| quantity       | int    | Die Kaufmenge. Derzeit wird als einziger Wert 1 unterstützt. Ohne Angabe wird standardmäßig der Wert1 verwendet.                                                                                                                                                                                                                | Nein       |
| skuId          | string | Die SKU-ID aus dem Windows Store-Katalog. Beispiel für eine SKU-ID: 0010.                                                                                                                                                                                                                                                | Ja      |

<span/>

### Anforderungsbeispiel

```syntax
POST https://purchase.mp.microsoft.com/v6.0/purchases/grant HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJK……
Content-Length: 1863
Content-Type: application/json

{
    "b2bKey" : "eyJ0eXAiOiJK……",
    "availabilityId" : "9RT7C09D5J3W",
    "productId" : "9NBLGGH5WVP6",
    "skuId" : "0010",
    "language" : "en-us",
    "market" : "us",
    "orderId" : "3eea1529-611e-4aee-915c-345494e4ee76",
}
```

## Antwort


### Antworttext

| Parameter                 | Typ                        | Beschreibung                                                                                                                                              | Erforderlich |
|---------------------------|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| clientContext             | ClientContextV6             | Informationen zum Clientkontext für diesen Auftrag. Sie werden dem *clientID*-Wert aus dem Azure AD-Token zugewiesen.                                     | Ja      |
| createdtime               | datetimeoffset              | Der Zeitpunkt der Auftragserstellung.                                                                                                                          | Ja      |
| currencyCode              | string                      | Der Währungscode für *totalAmount* und *totalTaxAmount*. Für kostenlose Artikel nicht relevant.                                                                                | Ja      |
| friendlyName              | string                      | Der Anzeigename für den Auftrag. Für Aufträge über die Windows Store-Einkaufs-API nicht relevant.                                                               | Ja      |
| isPIRequired              | boolean                     | Gibt an, ob für die Bestellung ein Zahlungsmittel (Payment Instrument, PI) erforderlich ist.                                                                   | Ja      |
| language                  | string                      | Die Sprach-ID für den Auftrag (z. B. „de“).                                                                                                       | Ja      |
| market                    | string                      | Die Markt-ID für den Auftrag (z. B. „DEU“).                                                                                                         | Ja      |
| orderId                   | string                      | Eine ID, die den Auftrag für einen bestimmten Benutzer identifiziert.                                                                                                   | Ja      |
| orderLineItems            | list&lt;OrderLineItemV6&gt; | Die Liste mit den Auftragspositionen. In der Regel gibt es 1 Position pro Auftrag.                                                                          | Ja      |
| orderState                | string                      | Der Zustand des Auftrags. Gültige Zustände sind **Editing**, **CheckingOut**, **Pending**, **Purchased**, **Refunded**, **ChargedBack** und **Cancelled**. | Ja      |
| orderValidityEndTime      | string                      | Der Zeitpunkt, bis zu dem der Preis des Auftrags vor der Übermittlung gültig ist. Für kostenlose Apps nicht relevant.                                                                      | Ja      |
| orderValidityStartTime    | string                      | Der Zeitpunkt, ab dem der Preis des Auftrags vor der Übermittlung gültig ist. Für kostenlose Apps nicht relevant.                                                                     | Ja      |
| purchaser                 | IdentityV6                  | Ein Objekt, das die Identität des Käufers beschreibt.                                                                                                  | Ja      |
| totalAmount               | decimal                     | Der Gesamtkaufbetrag aller im Auftrag enthaltenen Artikel (inklusive Steuern).                                                                                       | Ja      |
| totalAmountBeforeTax      | decimal                     | Der Gesamtkaufbetrag aller im Auftrag enthaltenen Artikel (vor Steuern).                                                                                              | Ja      |
| totalChargedToCsvTopOffPI | decimal                     | Bei Verwendung eines separaten Zahlungsmittels und eines gespeicherten Werts (CSV) der für CSV in Rechnung gestellte Betrag.                                                                | Ja      |
| totalTaxAmount            | decimal                     | Die Gesamtsteuerbetrag für alle Positionen.                                                                                                              | Ja      |

<span/>

Das ClientContext-Objekt enthält die folgenden Parameter.

| Parameter | Typ   | Beschreibung                           | Erforderlich |
|-----------|--------|---------------------------------------|----------|
| client    | string | Die Client-ID, unter der der Auftrag erstellt wurde. | Nein       |

<span/>

Das OrderLineItemV6-Objekt enthält die folgenden Parameter.

| Parameter               | Typ           | Beschreibung                                                                                                  | Erforderlich |
|-------------------------|----------------|--------------------------------------------------------------------------------------------------------------|----------|
| agent                   | IdentityV6     | Der Agent, der die Position zuletzt bearbeitet hat. Weitere Informationen zu diesem Objekt finden Sie in der folgenden Tabelle.       | Nein       |
| availabilityId          | string         | Die Verfügbarkeits-ID des Produkts, das aus dem Windows Store-Katalog erworben werden soll.                           | Ja      |
| beneficiary             | IdentityV6     | Die Identität des Begünstigten für den Auftrag.                                                                | Nein       |
| billingState            | string         | Der Abrechnungszustand des Auftrags. Dieser wird bei Beendigung auf **Charged** festgelegt.                                   | Nein       |
| campaignId              | string         | Die Kampagnen-ID für diesen Auftrag.                                                                              | Nein       |
| currencyCode            | string         | Der verwendete Währungscode für die Preisdetails.                                                                    | Ja      |
| Beschreibung             | string         | Eine lokalisierte Beschreibung der Position.                                                                    | Ja      |
| devofferId              | string         | Die Auftrags-ID für diesen Auftrag, sofern vorhanden.                                                           | Nein       |
| fulfillmentDate         | datetimeoffset | Das Erfüllungsdatum.                                                                           | Nein       |
| fulfillmentState        | string         | Der Erfüllungszustand dieses Artikels. Dieser wird bei Beendigung auf **Fulfilled** festgelegt.                      | Nein       |
| isPIRequired            | boolean        | Gibt an, ob für diese Position ein Zahlungsmittel erforderlich ist.                                       | Ja      |
| isTaxIncluded           | boolean        | Gibt an, ob in den Preisdetails des Artikels Steuern enthalten sind.                                        | Ja      |
| legacyBillingOrderId    | string         | Die alte Abrechnungs-ID.                                                                                       | Nein       |
| lineItemId              | string         | Die Positions-ID für den Artikel in diesem Auftrag.                                                                 | Ja      |
| listPrice               | decimal        | Der Listenpreis des Artikels in diesem Auftrag.                                                                    | Ja      |
| Produkt-ID               | string         | Die Windows Store-Produkt-ID der Position.                                                               | Ja      |
| Produkttyp             | string         | Die Art des Produkts. Die unterstützten Werte sind **Durable**, **Application** und **UnmanagedConsumable**. | Ja      |
| Quantity                | int            | Die Menge des bestellten Artikels.                                                                            | Ja      |
| retailPrice             | decimal        | Der Einzelhandelspreis des bestellten Artikels.                                                                        | Ja      |
| revenueRecognitionState | string         | Der Zustand der Umsatzrealisierung.                                                                               | Ja      |
| skuId                   | string         | Die Windows Store-SKU-ID der Position.                                                                   | Ja      |
| taxAmount               | decimal        | Der Steuerbetrag für die Position.                                                                            | Ja      |
| taxType                 | string         | Der Steuertyp für die anfallenden Steuern.                                                                       | Ja      |
| Title                   | string         | Der lokalisierte Titel der Position.                                                                        | Ja      |
| totalAmount             | decimal        | Der Gesamtkaufbetrag der Position (inklusive Steuern).                                                    | Ja      |

<span/>

Das IdentityV6-Objekt enthält die folgenden Parameter.

| Parameter     | Typ   | Beschreibung                                                                        | Erforderlich |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | string | Enthält den Wert **"pub"**.                                                      | Ja      |
| identityValue | string | Der Zeichenfolgenwert von *publisherUserId* aus dem angegebenen Windows Store-ID-Schlüssel. | Ja      |

<span/> 

### Antwortbeispiel

```syntax
Content-Length: 1203
Content-Type: application/json
MS-CorrelationId: fb2e69bc-f26a-4aab-a823-7586c19f5762
MS-RequestId: c1bc832c-f742-47e4-a76c-cf061402f698
MS-CV: XjfuNWLQlEuxj6Mt.8
MS-ServerId: 030032362
Date: Tue, 13 Oct 2015 21:21:51 GMT

{
    "clientContext": {
        "client": "86b78998-d05a-487b-b380-6c738f6553ea"
    },
    "createdTime": "2015-10-13T21:21:51.1863494+00:00",
    "currencyCode": "USD",
    "isPIRequired": false,
    "language": "en-us",
    "market": "us",
    "orderId": "3eea1529-611e-4aee-915c-345494e4ee76",
    "orderLineItems": [{
        "availabilityId": "9RT7C09D5J3W",
        "beneficiary": {
            "identityType": "pub",
            "identityValue": "user1"
        },
        "billingState": "Charged",
        "currencyCode": "USD",
        "description": "Jewels, Jewels, Jewels - Consumable 2",
        "fulfillmentDate": "2015-10-13T21:21:51.639478+00:00",
        "fulfillmentState": "Fulfilled",
        "isPIRequired": false,
        "isTaxIncluded": true,
        "lineItemId": "2814d758-3ee3-46b3-9671-4fb3bdae9ffe",
        "listPrice": 0.0,
        "payments": [],
        "productId": "9NBLGGH5WVP6",
        "productType": "UnmanagedConsumable",
        "quantity": 1,
        "retailPrice": 0.0,
        "revenueRecognitionState": "None",
        "skuId": "0010",
        "taxAmount": 0.0,
        "taxType": "NoApplicableTaxes",
        "title": "Jewels, Jewels, Jewels - Consumable 2",
        "totalAmount": 0.0
    }],
    "orderState": "Purchased",
    "orderValidityEndTime": "2015-10-14T21:21:51.1863494+00:00",
    "orderValidityStartTime": "2015-10-13T21:21:51.1863494+00:00",
    "purchaser": {
        "identityType": "pub",
        "identityValue": "user1"
    },
    "testScenarios": "None",
    "totalAmount": 0.0,
    "totalTaxAmount": 0.0
}
```

## Fehlercodes


| Code | Fehler        | Interner Fehlercode           | Beschreibung                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Nicht autorisiert | AuthenticationTokenInvalid | Das Azure AD-Zugriffstoken ist ungültig. In einigen Fällen enthalten die Details zu ServiceError weitere Informationen, z. B. wenn das Token abgelaufen ist oder der *appid*-Anspruch fehlt. |
| 401  | Nicht autorisiert | PartnerAadTicketRequired   | An den Dienst wurde im Autorisierungsheader kein Azure AD-Zugriffstoken übergeben.                                                                                                   |
| 401  | Nicht autorisiert | InconsistentClientId       | Der *clientId*-Anspruch im Windows Store-ID-Schlüssel im Anforderungstext und der *appid*-Anspruch im Azure AD-Zugriffstoken im Autorisierungsheader stimmen nicht überein.                     |
| 400  | BadRequest   | InvalidParameter           | Die Details enthalten Informationen zum Anforderungstext und zu Feldern mit ungültigem Wert.                                                                                    |

<span/> 

## Verwandte Themen


* [Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md)
* [Produktabfrage](query-for-products.md)
* [Melden von Verbrauchsprodukten als erfüllt](report-consumable-products-as-fulfilled.md)
* [Verlängern eines Windows Store-ID-Schlüssels](renew-a-windows-store-id-key.md)
 

 



<!--HONumber=Jul16_HO1-->


