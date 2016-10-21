---
author: mcleanbyron
ms.assetid: C78176D6-47BB-4C63-92F8-426719A70F04
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um den Status einer Flight-Paketübermittlung abzurufen."
title: "Abrufen des Status einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 9c087a01d8a499f8dd31c428f0c1abdbe9b19fac

---

# Abrufen des Status einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um den Status einer Flight-Paketübermittlung abzurufen. Weitere Informationen über den Erstellungsprozess einer Flight-Paketübermittlung mithilfe der Windows Store-Übermittlungs-API finden Sie unter [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine Flight-Paketübermittlung für eine App im Dev Center-Konto. Dies können Sie im Dev Center-Dashboard oder unter Verwendung der Methode [Erstellen einer Flight-Paketübermittlung](create-a-flight-submission.md) ausführen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions{submissionId}/status``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Erforderlich. Die Store-ID der App mit der Flight-Paketübermittlung, für die der Status abgerufen werden soll. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Erforderlich. Die ID des Flight-Pakets mit der Übermittlung, für die der Status abgerufen werden soll. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen eines Flight-Pakets](create-a-flight.md) und zum [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md) enthalten.  |
| submissionId | string | Erforderlich. Die ID der Übermittlung, für die der Status abgerufen werden sollen. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen einer Flight-Paketübermittlung](create-a-flight-submission.md) enthalten.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie der Status einer Flight-Paketübermittlung abgerufen werden kann.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/status HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen über die angegebene Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie im folgenden Abschnitt.

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | Der Status der Übermittlung. Folgende Werte sind möglich: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | Objekt  |  Enthält zusätzliche Details über den Status der Übermittlung, einschließlich Informationen zu Fehlern. Weitere Informationen finden Sie unter [Statusdetails-Ressource](manage-flight-submissions.md#status-details-object). |


<span/>

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Die Übermittlung konnte nicht gefunden werden. |
| 409  | Die App verwendet eine Dev Center-Dashboard-Funktion, die [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt wird](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>


## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md)
* [Abrufen einer App-Übermittlung](get-an-app-submission.md)
* [Erstellen einer App-Übermittlung](create-an-app-submission.md)
* [Ausführen eines Commit für eine App-Übermittlung](commit-an-app-submission.md)
* [Aktualisieren einer App-Übermittlung](update-an-app-submission.md)
* [Löschen einer App-Übermittlung](delete-an-app-submission.md)



<!--HONumber=Aug16_HO5-->


