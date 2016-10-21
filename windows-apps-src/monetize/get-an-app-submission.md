---
author: mcleanbyron
ms.assetid: BF296C25-A2E6-48E4-9D08-0CCDB5FAE0C8
description: "Verwenden Sie diese Methode aus der Windows Store-Übermittlungs-API zum Abrufen von Daten für eine vorhandene App-Übermittlung."
title: "Abrufen einer App-Übermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 178b70db1583790c174d65e060c8bce6e4f69243
ms.openlocfilehash: c845b59919a3a487949bc8926f7261992dac60ae

---

# Abrufen einer App-Übermittlung mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode aus der Windows Store-Übermittlungs-API zum Abrufen von Daten für eine vorhandene App-Übermittlung. Weitere Informationen über den Erstellungsprozess einer App-Übermittlung mithilfe der Windows Store-Übermittlungs-API finden Sie unter [Verwalten von App-Übermittlungen](manage-app-submissions.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie Übermittlung für eine App im Dev Center-Konto. Sie können dies im Dev Center-Dashboard oder unter Verwendung der Methode [Erstellen einer App-Übermittlung](create-an-app-submission.md) ausführen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId} ``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Erforderlich. Die Store-ID der App, für die Sie eine Übermittlung aktualisieren möchten. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | string | Erforderlich. Die ID der abzurufenden Übermittlung. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen einer App-Übermittlung](create-an-app-submission.md) enthalten.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiel

Im folgenden Beispiel wird das Abrufen einer App-Übermittlung veranschaulicht.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680 HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen über die angegebene Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie unter [App-Übermittlungsressource](manage-app-submissions.md#app-submission-object).

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2"
}
```

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Die Übermittlung konnte nicht gefunden werden. |
| 409  | Die Übermittlung gehört nicht zur angegebenen App, oder die App verwendet eine Dev Center-Dashboard-Funktion, die [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt wird](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Erstellen einer App-Übermittlung](create-an-app-submission.md)
* [Ausführen eines Commit für eine App-Übermittlung](commit-an-app-submission.md)
* [Aktualisieren einer App-Übermittlung](update-an-app-submission.md)
* [Löschen einer App-Übermittlung](delete-an-app-submission.md)
* [Abrufen des Status einer App-Übermittlung](get-status-for-an-app-submission.md)



<!--HONumber=Aug16_HO5-->


