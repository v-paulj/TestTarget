---
author: mcleanbyron
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um eine neue oder aktualisierte Add-On-Übermittlung in Windows Dev Center zu übernehmen."
title: "Übernehmen einer Add-On-Übermittlung mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: f9b9e5801f94101156850086c16311cf567b1e7d

---

# Übernehmen einer Add-On-Übermittlung mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um eine neue oder aktualisierte Add-On-Übermittlung (auch bekannt als In-App-Produkt- oder IAP-Übermittlung) in Windows Dev Center zu übernehmen. Durch die Übernahmeaktion wird Dev Center darüber benachrichtigt, dass die Übermittlungsdaten (darunter alle zugehörigen Symbole) hochgeladen wurden. Als Reaktion übernimmt Dev Center die Änderungen der Übermittlungsdaten zur Aufnahme und Veröffentlichung. Nachdem der Übernahmevorgang erfolgreich ausgeführt wurde, werden die Änderungen der Übermittlung im Dev Center-Dashboard angezeigt.

Weitere Informationen dazu, wie der Übernahmevorgang in den Prozess zur Übermittlung eines Add-Ons mit der Windows Store-Übermittlungs-API passt, finden Sie unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* [Erstellen Sie eine Add-On-Übermittlung](create-an-add-on-submission.md), und [aktualisieren Sie die Übermittlung](update-an-add-on-submission.md) mit allen erforderlichen Änderungen der Übermittlungsdaten.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Erforderlich. Die Store-ID des Add-Ons, die die Übermittlung enthält, die Sie übernehmen möchten. Die Store-ID ist im Windows Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Abrufen aller Add-Ons](get-all-add-ons.md) und [Erstellen eines Add-Ons](create-an-add-on.md) enthalten. |
| submissionId | string | Erforderlich. Die ID der Übermittlung, die Sie übernehmen möchten. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md) enthalten.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie eine Add-On-Übermittlung übernommen wird.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
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
| 409  | Die angegebene Übermittlung wurde gefunden, konnte jedoch nicht in ihrem aktuellen Zustand übernommen werden. Oder das Add-On verwendet ein Dev Center-Dashboard-Feature, das [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt wird](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>


## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen einer Add-On-Übermittlung](get-an-add-on-submission.md)
* [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md)
* [Aktualisieren einer Add-On-Übermittlung](update-an-add-on-submission.md)
* [Löschen einer Add-On-Übermittlung](delete-an-add-on-submission.md)
* [Abrufen des Status einer Add-On-Übermittlung](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->


