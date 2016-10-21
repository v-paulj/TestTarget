---
author: mcleanbyron
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: "Verwenden Sie diese Methode aus der Windows Store-Übermittlung API zur Aktualisierung einer vorhandenen Flight-Paketübermittlung."
title: "Aktualisieren Sie eine Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 9dfad9c0cc6b6e03a2196946ac578ca3170fccc5

---

# Aktualisieren Sie eine Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API


Verwenden Sie diese Methode aus der Windows Store-Übermittlung API zur Aktualisierung einer vorhandenen Flight-Paketübermittlung. Nachdem Sie mit dieser Methode eine Übermittlung erfolgreich aktualisiert haben, müssen Sie ein [Commit für die Übermittlung](commit-a-flight-submission.md) für Aufnahme und Veröffentlichung durchführen.

Weitere Informationen dazu, wie diese Methode in das Erstellen einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API passt, finden Sie unter [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine Flight-Paketübermittlung für eine App im Dev Center-Konto. Sie können dies im Dev Center-Dashboard oder unter Verwendung der Methode [Erstellen einer Flight-Paketübermittlung](create-a-flight-submission.md) erreichen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions{submissionId}``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Erforderlich. Die Store-ID der App, für die Sie eine Flight Paketübermittlung aktualisieren möchten. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Erforderlich. Die ID des Flight-Pakets, für das Sie eine Übermittlung aktualisieren möchten. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen eines Flight-Pakets](create-a-flight.md) und zum [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md) enthalten.  |
| submissionId | string | Erforderlich. Die ID der zu aktualisierenden Übermittlung. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen einer Flight-Paketübermittlung](create-a-flight-submission.md) enthalten.  |

<span/>

### Anforderungstext

Der Anforderungstext hat folgende Parameter.

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | array  | Enthält Objekte, die Details über die einzelnen Pakete der Übermittlung bereitstellen. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Flight-Paketressource](manage-flight-submissions.md#flight-package-object). Beim Aufruf dieser Methode zur Aktualisierung einer App-Übermittlung sind im Antworttext nur die *fileName*-, *fileStatus*-, *minimumDirectXVersion*- und *minimumSystemRam*-Werte dieser Objekte erforderlich. Die übrigen Werte werden von Dev Center aufgefüllt. |
| targetPublishMode           | string  | Der Publish-Modus für die Übermittlung. Folgende Werte sind möglich: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |
| notesForCertification           | string  |  Enthält zusätzliche Informationen für Zertifizierungstester wie Anmeldeinformationen für Testkonten und Schritte zum Zugriff auf und zur Überprüfung von Features. Weitere Informationen finden Sie unter [Hinweise zur Zertifizierung](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span/>

### Anforderungsbeispiel

Im folgenden Beispiel wird die Aktualisierung einer Flight-Paketübermittlung für eine App veranschaulicht.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen zur aktualisierten Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Flight-Paketressource](manage-flight-submissions.md#flight-submission-object).

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
| 400  | Die Flight-Paketübermittlung konnte nicht aktualisiert werden, da die Anforderung ungültig ist. |
| 409  | Die Flight-Paketübermittlung konnte im aktuellen Zustand der App nicht aktualisiert werden, oder in der App wird ein Dev Center-Dashboard-Feature verwendet, das [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported) wird. |   

<span/>


## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md)
* [Abrufen einer Flight-Paketübermittlung](get-a-flight-submission.md)
* [Erstellen einer Flight-Paketübermittlung](create-a-flight-submission.md)
* [Ausführen eines Commit für eine Flight-Paketübermittlung](commit-a-flight-submission.md)
* [Löschen einer Flight-Paketübermittlung](delete-a-flight-submission.md)
* [Abrufen des Status einer Flight-Paketübermittlung](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


