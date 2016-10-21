---
author: mcleanbyron
ms.assetid: 55315F38-6EC5-4889-A14E-7D8EC282FE98
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um den Status einer Add-On-Übermittlung abzurufen."
title: "Abrufen des Status einer Add-On-Übermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: e5be4ee466c204255a074adae4d150860286ba0a

---

# Abrufen des Status einer Add-On-Übermittlung mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode der Windows Store-Übermittlungs-API zum Abrufen des Status für eine Add-On-Übermittlung (auch als In-App-Produkt oder IAP bezeichnet). Weitere Informationen über den Erstellungsprozess einer Add-On-Übermittlung mithilfe der Windows Store-Übermittlungs-API finden Sie unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine Add-On-Übermittlung für eine App im Dev Center-Konto. Sie können dies im Dev Center-Dashboard oder unter Verwendung der Methode [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md) ausführen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/status``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Erforderlich. Die Store-ID des Add-Ons mit der Übermittlung, für die der Status abgerufen werden soll. Die Store-ID ist im Dev Center-Dashboard verfügbar.  |
| submissionId | string | Erforderlich. Die ID der Übermittlung, für die der Status abgerufen werden sollen. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md) enthalten.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie der Status einer Add-On-Übermittlung abgerufen werden kann.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680/status HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen über die angegebene Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie in den folgenden Abschnitten.

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
| statusDetails           | Objekt  |  Enthält zusätzliche Details über den Status der Übermittlung, einschließlich Informationen zu Fehlern. Weitere Informationen finden Sie unter [Statusdetails-Ressource](manage-add-on-submissions.md#status-details-object). |

<span/>

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Die Übermittlung konnte nicht gefunden werden. |
| 409  | Das Add-On verwendet eine Dev Center-Dashboard-Funktion, die [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt wird](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>


## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen einer Add-On-Übermittlung](get-an-add-on-submission.md)
* [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md)
* [Ausführen eines Commit für eine Add-On-Übermittlung](commit-an-add-on-submission.md)
* [Aktualisieren einer Add-On-Übermittlung](update-an-add-on-submission.md)
* [Löschen einer Add-On-Übermittlung](delete-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->


