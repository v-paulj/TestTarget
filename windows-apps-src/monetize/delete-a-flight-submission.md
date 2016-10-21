---
author: mcleanbyron
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: "Verwenden Sie diese Methode der Windows Store-Übermittlungs-API zum Löschen einer vorhandenen Flight-Paketübermittlung."
title: "Löschen einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 9712f0ef68cc3614c9961183dba8b67a71c0ec39

---

# Löschen einer Flight-Paketübermittlung mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode der Windows Store-Übermittlungs-API zum Löschen einer vorhandenen Flight-Paketübermittlung.

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | String | Erforderlich. Die Store-ID der App, die die zu löschende Flight-Paketübermittlung enthält. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | String | Erforderlich. Die ID des Flight-Pakets, das die zu löschende Übermittlung enthält. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen eines Flight-Pakets](create-a-flight.md) und zum [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md) enthalten.  |
| submissionId | String | Erforderlich. Die ID der zu löschenden Übermittlung. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen einer Flight-Paketübermittlung](create-a-flight-submission.md) enthalten.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

<span/>

### Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie Sie eine Flight-Paketübermittlung löschen.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Wenn dies erfolgreich war, gibt die Methode einen leeren Antworttext zurück.

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 400  | Die Anforderungsparameter sind ungültig. |
| 404  | Die angegebene Übermittlung konnte nicht gefunden werden. |
| 409  | Die angegebene Übermittlung wurde gefunden, konnte jedoch nicht in ihrem aktuellen Zustand gelöscht werden. Oder die App verwendet ein Dev Center-Dashboard-Feature, das [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt wird](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md)
* [Abrufen einer Flight-Paketübermittlung](get-a-flight-submission.md)
* [Erstellen einer Flight-Paketübermittlung](create-a-flight-submission.md)
* [Ausführen eines Commit für eine Flight-Paketübermittlung](commit-a-flight-submission.md)
* [Aktualisieren einer Flight-Paketübermittlung](update-a-flight-submission.md)
* [Abrufen des Status einer Flight-Paketübermittlung](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


