---
author: mcleanbyron
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API zum Erstellen einer neuen Add-On-Übermittlung für eine App, die für Ihr Windows Dev Center-Konto registriert ist."
title: "Erstellen einer Add-On-Übermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: b7de4b00fb4d48b9f4c542437c38e0282e155a29

---

# Erstellen einer Add-On-Übermittlung mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um eine neue Übermittlung für ein Add-On (auch als In-App-Produkt oder IAP bezeichnet) für eine App zu erstellen, die für Ihr Windows Dev Center-Konto registriert ist. Nachdem Sie erfolgreich eine neue Übermittlung mit dieser Methode erstellt haben, [aktualisieren Sie die Übermittlung](update-an-add-on-submission.md), um erforderliche Änderungen an den Übermittlungsdaten vorzunehmen, und führen Sie ein [Commit für die Übermittlung](commit-an-add-on-submission.md) zur Aufnahme und Veröffentlichung durch.

Weitere Informationen dazu, wie diese Methode zum Erstellen einer Add-On-Übermittlung mithilfe der Windows Store-Übermittlungs-API passt, finden Sie unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

>**Hinweis**&nbsp;&nbsp;Durch diese Methode wird eine Übermittlung für ein vorhandenes Add-On erstellt. Um ein Add-On zu erstellen, verwenden Sie die Methode zum [Erstellen eines Add-Ons](create-an-add-on.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie ein Add-On für eine App im Dev Center-Konto. Verwenden Sie hierzu das Dev Center-Dashboard oder die Methode zum [Erstellen eines Add-Ons](create-an-add-on.md).

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Erforderlich. Die Store-ID des Add-Ons, für das Sie eine Übermittlung erstellen möchten. Die Store-ID ist im Windows Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen eines Add-Ons](create-an-add-on.md) und zum [Abrufen von Add-On-Details](get-all-add-ons.md) enthalten.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie eine neue Übermittlung für ein Add-On erstellen.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen zur neuen Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Add-On-Übermittlungsressource](manage-add-on-submissions.md#add-on-submission-object).

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 400  | Die Übermittlung konnte nicht erstellt werden, da die Anforderung ungültig ist. |
| 409  | Die Übermittlung konnte im aktuellen Zustand der App nicht erstellt werden, oder in der App wird ein Dev Center-Dashboard-Feature verwendet, das [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported) wird. |   

<span/>


## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md)
* [Abrufen einer Add-On-Übermittlung](get-an-add-on-submission.md)
* [Ausführen eines Commit für eine Add-On-Übermittlung](commit-an-add-on-submission.md)
* [Aktualisieren einer Add-On-Übermittlung](update-an-add-on-submission.md)
* [Löschen einer Add-On-Übermittlung](delete-an-add-on-submission.md)
* [Abrufen des Status einer Add-On-Übermittlung](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->


