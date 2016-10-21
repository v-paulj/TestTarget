---
author: mcleanbyron
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um Daten für ein Flight-Paket für eine App abzurufen, die für Ihr Windows Dev Center-Konto registriert ist."
title: "Abrufen eines Flight-Pakets mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: fb8328981a45e353987a62d7794158c2e1179087

---

# Abrufen eines Flight-Pakets mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um Daten für ein Flight-Paket für eine App abzurufen, die für Ihr Windows Dev Center-Konto registriert ist.

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter


| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Erforderlich. Die Store-ID der App, die die Flight-Paketübermittlung enthält, die Sie abrufen möchten. Die Store-ID für die App ist im Dev Center-Dashboard verfügbar.  |
| flightId | string | Erforderlich. Die ID des abzurufenden Flight-Pakets. Diese ID ist im Dev Center-Dashboard verfügbar und in den Antwortdaten für Anforderungen zum [Erstellen eines Flight-Pakets](create-a-flight.md) und zum [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md) enthalten.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

<span/>

### Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie Informationen über ein Flight-Paket mit der ID 43e448df-97c9-4a43-a0bc-2a445e736bcd für eine App mit dem Store-ID-Wert 9WZDNCRD91MD abgerufen werden.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie in den folgenden Abschnitten.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | string  | Die ID für das Flight-Paket. Dieser Wert wird von Dev Center bereitgestellt.  |
| friendlyName           | string  | Der Name des Flight-Pakets nach Vorgabe des Entwicklers.   |  
| lastPublishedFlightSubmission       | Objekt | Ein Objekt, das Informationen über die letzte veröffentlichte Übermittlung für das Flight-Paket enthält. Weitere Informationen finden Sie unten im Abschnitt [Übermittlungsobjekt](#submission_object).  |
| pendingFlightSubmission        | Objekt  |  Ein Objekt, das Informationen über die aktuell ausstehende Übermittlung für das Flight-Paket enthält. Weitere Informationen finden Sie unten im Abschnitt [Übermittlungsobjekt](#submission_object).  |   
| groupIds           | Array  | Ein Array von Zeichenfolgen, die die IDs der Test-Flight-Gruppen enthalten, die dem Flight-Paket zugeordnet sind. Weitere Informationen zu Test-Flight-Gruppen finden Sie unter [Flight-Pakete](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | string  | Der Anzeigename des Flight-Pakets, das den unmittelbar niedrigeren Rang als das aktuelle Flight-Paket erhält. Weitere Informationen zur Bewertung von Test-Flight-Gruppen finden Sie unter [Flight-Pakete](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |

<span id="submission_object" />
### Übermittlungsobjekt

Die Werte *LastPublishedFlightSubmission* und *PendingFlightSubmission* im Antworttext enthalten Objekte mit Ressourceninformationen über eine Übermittlung für das Flight-Paket. Diese Objekte enthalten folgende Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Die ID der Übermittlung.    |
| resourceLocation   | string  | Ein relativer Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` anfügen können, um die vollständigen Daten für die Übermittlung abzurufen.                                                                                                                                               |
 
<span/>

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung     |
|--------|---------------------  |
| 400  | Die Anforderung ist ungültig. |
| 404  | Das angegebene Flight-Paket konnte nicht gefunden werden.   |   
| 409  | Die App verwendet eine Dev Center-Dashboard-Funktion, die [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt wird](create-and-manage-submissions-using-windows-store-services.md#not_supported). |                                                                                                 

<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Erstellen eines Flight-Pakets](create-a-flight.md)
* [Löschen eines Flight-Pakets](delete-a-flight.md)



<!--HONumber=Aug16_HO5-->


