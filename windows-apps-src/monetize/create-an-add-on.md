---
author: mcleanbyron
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API zum Erstellen eines Add-Ons für eine App, die für Ihr Windows Dev Center-Konto registriert ist."
title: "Erstellen eines Add-Ons mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 11cf25fbeacbe3145c9cc3f4a80bdcce3028bf55

---

# Erstellen eines Add-Ons mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um ein Add-On (auch als In-App-Produkt oder IAP bezeichnet) für eine App zu erstellen, die für Ihr Windows Dev Center-Konto registriert ist.

>**Hinweis**&nbsp;&nbsp;Durch diese Methode wird ein Add-On ohne Übermittlungen erstellt. Verwenden Sie zum Erstellen einer Übermittlung für ein Add-On die Methoden unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungstext

Der Anforderungstext hat folgende Parameter.
 
|  Parameter  |  Typ  |  Beschreibung  |  Erforderlich  |
|------|------|------|------|
|  applicationIds  |  array  |  Ein Array, das die Store-ID der App enthält, der dieses Add-On zugeordnet ist. In diesem Array wird nur ein Element unterstützt.   |  Ja  |
|  productId  |  string  |  Die Produkt-ID des Add-Ons. Dies ist eine ID, die im Code verwendet werden kann, um auf das Add-On zu verweisen. Weitere Informationen finden Sie unter [Festlegen von Produkttyp und Produkt-ID für das IAP](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Ja  |
|  productType  |  string  |  Der Produkttyp des Add-Ons. Die folgenden Werte werden unterstützt: **Gebrauchsgut** und **Verbrauchsartikel**.  |  Ja  |

<span/>

### Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie ein neues Endverbraucher-Add-On für eine App erstellen.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Add-On-Übermittlungsressource](manage-add-ons.md#add-on-object).

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung                                                                                                                                                                           |
|--------|------------------|
| 400  | Die Anforderung ist ungültig. |
| 409  | Das Add-On konnte im aktuellen Zustand nicht erstellt werden, oder im Add-On wird ein Dev Center-Dashboard-Feature verwendet, das [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported) wird. |   

<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md)
* [Abrufen aller Add-Ons](get-all-add-ons.md)
* [Abrufen eines Add-Ons](get-an-add-on.md)
* [Löschen eines Add-Ons](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


