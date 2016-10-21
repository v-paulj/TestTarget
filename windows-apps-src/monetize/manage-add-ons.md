---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "Verwenden Sie diese Methoden in der Windows Store-Übermittlungs-API, um Add-Ons für Apps zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden."
title: "Verwalten von Add-Ons mithilfe der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 75548ee4689fd31d734c570f8e3eca5d33a6181f

---

# Verwalten von Add-Ons mithilfe der Windows Store-Übermittlungs-API




Verwenden Sie die folgenden Methoden in der Windows Store-Übermittlungs-API, um Add-Ons (auch bekannt als In-App-Produkte oder IAPs) zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden. Eine Einführung in die Windows Store-Übermittlungs-API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit Windows Store-Diensten](create-and-manage-submissions-using-windows-store-services.md).

>**Hinweis**&nbsp;&nbsp;Diese Methoden können nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert. Diese Methoden können nur verwendet werden, um Add-Ons abzurufen, zu erstellen oder zu löschen. Um Übermittlungen für Add-Ons zu erstellen, verwenden Sie die Methoden in [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

| Methode        | URI    | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | Ruft Daten für alle Add-Ons für alle Apps ab, die in Ihrem Windows Dev Center-Konto registriert sind. Weitere Informationen finden Sie unter [Abrufen aller Add-Ons](get-all-add-ons.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId} ``` | Ruft Daten für ein bestimmtes Add-On ab. Weitere Informationen finden Sie unter [Abrufen eines Add-Ons](get-an-add-on.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | Erstellt ein neues Add-On. Weitere Informationen finden Sie unter [Erstellen eines Add-Ons](create-an-add-on.md).  |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` | Löscht ein Add-On. Weitere Informationen finden Sie unter [Löschen eines Add-Ons](delete-an-add-on.md). |

## Voraussetzungen

Falls noch nicht geschehen, erfüllen Sie vor der Verwendung dieser Methoden alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.

## Ressourcen

Diese Methoden verwenden die folgenden Ressourcen zum Formatieren von Daten.

<span id="add-on-object" />
### Add-On

Diese Ressource stellt ein Add-On dar. Das folgende Beispiel veranschaulicht das Format der Ressource.

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
  "productId": "TestAddOn",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

Diese Ressource hat die folgenden Werte.

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applications      | array  | Ein Array mit einem Objekt, das die App darstellt, der dieses Add-On zugeordnet ist. Weitere Informationen zu den Daten in diesem Objekt finden Sie unten im Abschnitt [Application-Objekt](#application-object). In diesem Array wird nur ein Element unterstützt.  |
| id | string  | Die Store-ID des Add-Ons. Dieser Wert wird vom Store bereitgestellt. Beispiel für eine Store-ID: 9NBLGGH4TNMP.  |
| productId | string  | Die Produkt-ID des Add-Ons. Dies ist die ID, die vom Entwickler während der Erstellung des Add-Ons angegeben wurde. Weitere Informationen finden Sie unter [Festlegen von Produkttyp und Produkt-ID für das IAP](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id). |
| productType | string  | Der Produkttyp des Add-Ons. Die folgenden Werte werden unterstützt: **Durable** und **Consumable**.  |
| lastPublishedInAppProductSubmission       | object | Ein Objekt mit Informationen über die letzte veröffentlichte Übermittlung für das Add-On. Weitere Informationen finden Sie unten im Abschnitt [Übermittlung](#submission-object).                                                                                                                                                          |
| pendingInAppProductSubmission        | object  |  Ein Objekt mit Informationen über die aktuell ausstehende Übermittlung für das Add-On. Weitere Informationen finden Sie unten im Abschnitt [Übermittlung](#submission-object).  |   |

<span id="application-object" />
### Anwendung

Diese Ressource stellt eine App dar, der ein Add-On zugeordnet ist. Das folgende Beispiel veranschaulicht das Format der Ressource.

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
}
```

Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|-----------|
| value            | object  |  Ein Objekt, das die folgenden Werte enthält: <br/><br/> <ul><li>*id*. Die Store-ID der App. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).</li><li>*resourceLocation*. Ein relativer Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` anfügen können, um die vollständigen Daten für die App abzurufen.</li></ul>   |
| totalCount   | int  | Die Anzahl der App-Objekte im *applications*-Array des Antworttexts.                                                                                                                                                 |

<span id="submission-object" />
### Übermittlung

Diese Ressource enthält Informationen über eine Übermittlung für ein Add-On. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Die ID der Übermittlung.    |
| resourceLocation   | string  | Ein relativer Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` anfügen können, um die vollständigen Daten für die Übermittlung abzurufen.                                                                                                                                               |
 
<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit Windows Store-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Add-On-Übermittlungen mithilfe der Windows Store-Übermittlungs-API](manage-add-on-submissions.md)
* [Abrufen aller Add-Ons](get-all-add-ons.md)
* [Abrufen eines Add-Ons](get-an-add-on.md)
* [Erstellen eines Add-Ons](create-an-add-on.md)
* [Löschen eines Add-Ons](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


