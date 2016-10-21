---
author: mcleanbyron
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API zum Erstellen einer neuen Flight-Paketübermittlung für eine App, die für Ihr Windows Dev Center-Konto registriert ist."
title: "Erstellen einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 33f008c7b2bd32aacaf5a31d0b3201a00d145276

---

# Erstellen einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API zum Erstellen einer neuen Übermittlung eines Flight-Pakets für eine App. Nachdem Sie erfolgreich eine neue Übermittlung mit dieser Methode erstellt haben, [aktualisieren Sie die Übermittlung](update-a-flight-submission.md), um erforderliche Änderungen an den Übermittlungsdaten vorzunehmen, und führen Sie ein [Commit für die Übermittlung](commit-a-flight-submission.md) zur Aufnahme und Veröffentlichung durch.

Weitere Informationen dazu, wie diese Methode in das Erstellen einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API passt, finden Sie unter [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md).

>**Hinweis**&nbsp;&nbsp;Durch diese Methode wird eine Übermittlung für ein vorhandenes Flight-Paket erstellt. Verwenden Sie zum Erstellen eines Flight-Pakets die Methode zum [Erstellen eines Flight-Pakets](create-a-flight.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie ein Flight-Paket für eine App im Dev Center-Konto. Verwenden Sie hierzu das Dev Center-Dashboard oder die Methode zum [Erstellen eines Flight-Pakets](create-a-flight.md).

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Erforderlich. Die Store-ID der App, für die Sie eine Flight-Paketübermittlung erstellen möchten. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Erforderlich. Die ID des Flight-Pakets, für das Sie die Übermittlung hinzufügen möchten. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen eines Flight-Pakets](create-a-flight.md) und zum [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md) enthalten.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie eine neue Flight-Paketübermittlung für eine App mit der Store-ID 9WZDNCRD91MD erstellen.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen zur neuen Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Flight-Paketressource](manage-flight-submissions.md#flight-submission-object).

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 400  | Die Flight-Paketübermittlung konnte nicht erstellt werden, da die Anforderung ungültig ist. |
| 409  | Die Flight-Paketübermittlung konnte im aktuellen Zustand der App nicht erstellt werden, oder in der App wird ein Dev Center-Dashboard-Feature verwendet, das [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported) wird. |   

<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md)
* [Abrufen einer Flight-Paketübermittlung](get-a-flight-submission.md)
* [Ausführen eines Commit für eine Flight-Paketübermittlung](commit-a-flight-submission.md)
* [Aktualisieren einer Flight-Paketübermittlung](update-a-flight-submission.md)
* [Löschen einer Flight-Paketübermittlung](delete-a-flight-submission.md)
* [Abrufen des Status einer Flight-Paketübermittlung](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


