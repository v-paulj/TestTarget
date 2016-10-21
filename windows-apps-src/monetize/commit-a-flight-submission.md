---
author: mcleanbyron
ms.assetid: F94AF8F6-0742-4A3F-938E-177472F96C00
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um eine neue oder aktualisierte Flight-Paketübermittlung in Windows Dev Center zu übernehmen."
title: "Übernehmen einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: a9ea8de7b92b254c7bb8d63a5a3ea41afdd2d966

---

# Übernehmen einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um eine neue oder aktualisierte Flight-Paketübermittlung in Windows Dev Center zu übernehmen. Durch die Übernahmeaktion wird Dev Center darüber benachrichtigt, dass die Übermittlungsdaten (darunter alle zugehörigen Pakete) hochgeladen wurden. Als Reaktion übernimmt Dev Center die Änderungen der Übermittlungsdaten zur Aufnahme und Veröffentlichung. Nachdem der Übernahmevorgang erfolgreich ausgeführt wurde, werden die Änderungen der Übermittlung im Dev Center-Dashboard angezeigt.

Weitere Informationen dazu, wie der Übernahmevorgang in den Prozess zum Erstellen einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API passt, finden Sie unter [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* [Erstellen Sie eine Flight-Paketübermittlung](create-a-flight-submission.md), und [aktualisieren Sie die Übermittlung](update-a-flight-submission.md) mit allen erforderlichen Änderungen der Übermittlungsdaten.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Erforderlich. Die Store-ID der App, die die Flight-Paketübermittlung enthält, die Sie übernehmen möchten. Die Store-ID für die App ist im Dev Center-Dashboard verfügbar.  |
| flightId | string | Erforderlich. Die ID des Flight-Pakets, das die zu übernehmende Übermittlung enthält. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen eines Flight-Pakets](create-a-flight.md) und zum [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md) enthalten.  |
| submissionId | string | Erforderlich. Die ID der zu übernehmenden Übermittlung. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen einer Flight-Paketübermittlung](create-a-flight-submission.md) enthalten.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie eine Flight-Paketübermittlung übernommen wird.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie in den folgenden Abschnitten.

```json
{
  "status": "CommitStarted"
}
```

### Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | Der Status der Übermittlung. Folgende Werte sind möglich: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |

<span/>

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 400  | Die Anforderungsparameter sind ungültig. |
| 404  | Die angegebene Übermittlung konnte nicht gefunden werden. |
| 409  | Die angegebene Übermittlung wurde gefunden, konnte jedoch nicht in ihrem aktuellen Zustand übernommen werden. Oder die App verwendet ein Dev Center-Dashboard-Feature, das [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt wird](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>


## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md)
* [Abrufen einer Flight-Paketübermittlung](get-a-flight-submission.md)
* [Erstellen einer Flight-Paketübermittlung](create-a-flight-submission.md)
* [Aktualisieren einer Flight-Paketübermittlung](update-a-flight-submission.md)
* [Löschen einer Flight-Paketübermittlung](delete-a-flight-submission.md)
* [Abrufen des Status einer Flight-Paketübermittlung](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


