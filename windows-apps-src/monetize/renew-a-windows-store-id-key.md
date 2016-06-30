---
author: mcleanbyron
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: "Verwenden Sie diese Methode zum Verlängern eines Windows Store-Schlüssels."
title: "Verlängern eines Windows Store-ID-Schlüssels"
ms.sourcegitcommit: 2f4351d6f9bdc0b9a131ad5ead10ffba7e76c437
ms.openlocfilehash: 6255346c568ed24e17c795834ab182f73707c4de

---

# Verlängern eines Windows Store-ID-Schlüssels


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Verwenden Sie diese Methode zum Verlängern eines Windows Store-Schlüssels. Wenn Sie einen Windows Store-ID-Schlüssel durch Aufrufen der [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674)-Methode oder [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675)-Methode generieren, ist der Schlüssel 90 Tage lang gültig. Nach Ablauf des Schlüssels können Sie anhand des abgelaufenen Schlüssels einen neuen Schlüssel aushandeln, indem Sie diese Methode verwenden.

## Voraussetzungen


Zur Verwendung dieser Methode benötigen Sie:

-   Ein Azure AD-Zugriffstoken, das mit dem Zielgruppen-URI `https://onestore.microsoft.com` erstellt wurde.
-   Einen abgelaufenen Windows Store-ID-Schlüssel, der durch Aufrufen der [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674)-Methode oder [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675)-Methode im clientseitigen Code der App generiert wurde.

Weitere Informationen finden Sie unter [Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md).

## Anforderung


### Anforderungssyntax

| Schlüsseltyp    | Methode | Anforderungs-URI                                              |
|-------------|--------|----------------------------------------------------------|
| Sammlungen | POST   | `https://collections.mp.microsoft.com/v6.0/b2b/keys/renew` |
| Einkauf    | POST   | `https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew`    |

<br/> 

### Anforderungsheader

| Header         | Typ   | Beschreibung                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | string | Muss auf den Wert **collections.mp.microsoft.com** oder **purchase.mp.microsoft.com** festgelegt werden.           |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | string | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |

<br/> 

### Anforderungstext

| Parameter     | Typ   | Beschreibung                       | Erforderlich |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | string | Das Azure AD-Zugriffstoken.        | Ja      |
| key           | string | Der abgelaufene Windows Store-ID-Schlüssel. | Nein       |

<br/> 

### Anforderungsbeispiel

```syntax
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## Antwort


### Antworttext

| Parameter | Typ   | Beschreibung                                                                                                            | Erforderlich |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| key       | string | Der aktualisierte Windows Store-Schlüssel kann dann für zukünftige Aufrufe der Sammlungs- oder Einkaufs-API von Windows Store verwendet werden. | Nein       |

<br/> 

### Antwortbeispiel

```syntax
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## Fehlercodes


| Code | Fehler        | Interner Fehlercode           | Beschreibung                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Nicht autorisiert | AuthenticationTokenInvalid | Das Azure AD-Zugriffstoken ist ungültig. In einigen Fällen enthalten die Details zu ServiceError weitere Informationen, z. B. wenn das Token abgelaufen ist oder der *appid*-Anspruch fehlt. |
| 401  | Nicht autorisiert | InconsistentClientId       | Der *clientId*-Anspruch im Windows Store-ID-Schlüssel und der *appid*-Anspruch im Azure AD-Zugriffstoken stimmen nicht überein.                                                                     |

<br/> 

## Verwandte Themen


* [Anzeigen von Produkten und Gewähren von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md)
* [Produktabfrage](query-for-products.md)
* [Melden von Verbrauchsprodukten als erfüllt](report-consumable-products-as-fulfilled.md)
* [Gewähren kostenloser Produkte](grant-free-products.md)



<!--HONumber=Jun16_HO4-->


