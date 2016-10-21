---
author: mcleanbyron
ms.assetid: 8C63D33B-557D-436E-9DDA-11F7A5BFA2D7
description: "Verwenden Sie diese Methode aus der Windows Store-Übermittlungs-Api zur Aktualisierung einer vorhandenen Add-On-Übermittlung."
title: "Aktualisieren einer Add-On-Übermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: b7a8e1d39d5ee0a0858382b84ab00cc2c481da5d

---

# Aktualisieren einer Add-On-Übermittlung mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode der Windows Store-Übermittlungs-API zur Aktualisierung einer vorhandenen Add-On-Übermittlung (Add-Ons werden auch als In-App-Produkt bzw. IAP bezeichnet). Nachdem Sie mit dieser Methode eine Übermittlung erfolgreich aktualisiert haben, müssen Sie ein [Commit für die Übermittlung](commit-an-add-on-submission.md) für Aufnahme und Veröffentlichung durchführen.

Weitere Informationen dazu, wie diese Methode zum Erstellen einer Add-On-Übermittlung mithilfe der Windows Store-Übermittlungs-API passt, finden Sie unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine Add-On-Übermittlung für eine App im Dev Center-Konto. Sie können dies im Dev Center-Dashboard oder durch Verwenden der Methode [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md) erreichen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Erforderlich. Die Store-ID des Add-Ons, für das Sie eine Übermittlung aktualisieren möchten. Die Store-ID ist im Windows Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen eines Add-Ons](create-an-add-on.md) und zum [Abrufen von Add-On-Details](get-all-add-ons.md) enthalten.  |
| submissionId | string | Erforderlich. Die ID der zu aktualisierenden Übermittlung. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md) enthalten.  |

<span/>

### Anforderungstext

Der Anforderungstext hat folgende Parameter.

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| contentType           | string  |  Der [Inhaltstyp](../publish/enter-iap-properties.md#content-type), der im Add-On bereitgestellt wird. Folgende Werte sind möglich: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | Ein Array von Zeichenfolgen, das bis zu 10 [Schlüsselwörter](../publish/enter-iap-properties.md#keywords) für das Add-On enthalten kann. Die App kann mit diesen Schlüsselwörter Add-Ons abfragen.   |
| lifetime           | string  |  Die Lebensdauer des Add-Ons. Folgende Werte sind möglich: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | object  | Ein Objekt, das Eintragsinfos für das Add-On enthält. Weitere Informationen finden Sie unter [Eintragsressource](manage-add-on-submissions.md#listing-object).  |
| pricing           | object  | Ein Objekt, das Eintragsinfos für das Add-On enthält. Weitere Informationen finden Sie unter [Preisressource](manage-add-on-submissions.md#pricing-object).  |
| targetPublishMode           | string  | Der Publish-Modus für die Übermittlung. Folgende Werte sind möglich: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |
| tag           | string  |  Das [Tag](../publish/enter-iap-properties.md#tag) für das Add-On.   |
| visibility  | string  |  Die Sichtbarkeit des Add-Ons. Folgende Werte sind möglich: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |

<span/>

### Anforderungsbeispiel

Im folgenden Beispiel wird die Aktualisierung einer Add-On-Übermittlung veranschaulicht.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
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
}
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen zur aktualisierten Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Add-On-Übermittlungsressource](manage-add-on-submissions.md#add-on-submission-object).

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
| 400  | Die Übermittlung konnte nicht aktualisiert werden, da die Anforderung ungültig ist. |
| 409  | Die Übermittlung konnte im aktuellen Zustand des Add-Ons nicht aktualisiert werden, oder im Add-On wird ein Dev Center-Dashboard-Feature verwendet, das [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported) wird. |   

<span/>


## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md)
* [Abrufen einer Add-On-Übermittlung](get-an-add-on-submission.md)
* [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md)
* [Ausführen eines Commit für eine Add-On-Übermittlung](commit-an-add-on-submission.md)
* [Löschen einer Add-On-Übermittlung](delete-an-add-on-submission.md)
* [Abrufen des Status einer Add-On-Übermittlung](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->


