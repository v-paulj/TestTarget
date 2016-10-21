---
author: mcleanbyron
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: "Verwenden Sie diese Methoden in der Windows Store-Übermittlungs-API, um Daten für Apps abzurufen, die in Ihrem Windows Dev Center-Konto registriert wurden."
title: "Abrufen von App-Daten mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99d609decc8c38952961deac5bb8ec6926d91c88

---

# Abrufen von App-Daten mit der Windows Store-Übermittlungs-API

Verwenden Sie die folgenden Methoden in der Windows Store-Übermittlungs-API, um Daten für Apps abzurufen, die in Ihrem Windows Dev Center-Konto registriert wurden. Eine Einführung in die Windows Store-Übermittlungs-API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit Windows Store-Diensten](create-and-manage-submissions-using-windows-store-services.md).

>**Hinweis**&nbsp;&nbsp;Diese Methoden können nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert. Diese Methoden können nur verwendet werden, um Daten für Apps abzurufen. Informationen zum Erstellen oder Verwalten von Übermittlungen für Apps finden Sie unter den Methoden in [Verwalten von App-Übermittlungen](manage-app-submissions.md).

| Methode        | URI    | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` | Ruft Daten für alle Apps ab, die in Ihrem Windows Dev Center-Konto registriert sind. Weitere Informationen finden Sie unter [Abrufen aller Apps](get-all-apps.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}``` | Ruft Daten zu einer bestimmten App ab, die in Ihrem Windows Dev Center-Konto registriert ist. Weitere Informationen finden Sie unter [Abrufen einer App](get-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` | Führt die Add-Ons (auch bekannt als In-App-Produkte oder IAPs) für eine App auf, die in Ihrem Windows Dev Center-Konto registriert ist. Weitere Informationen finden Sie unter [Abrufen von Add-Ons für eine App](get-add-ons-for-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` | Führt die Flight-Pakete für eine App auf, die in Ihrem Windows Dev Center-Konto registriert ist. Weitere Informationen finden Sie unter [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md). |

<span/>

## Voraussetzungen

Falls noch nicht geschehen, erfüllen Sie vor der Verwendung dieser Methoden alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.

## Ressourcen

Diese Methoden verwenden die folgenden Ressourcen zum Formatieren von Daten.

<span id="application_object" />
### Anwendung

Diese Ressource steht für eine App, die in Ihrem Konto registriert ist. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  }
}
```

Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ID            | string  | Die Store-ID der App. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | string  | Der Primärname der App.                                                                                                                                                   |
| packageFamilyName | string  | Der Paketfamilienname der App.                                                                                                                                                                                                         |
| packageIdentityName          | string  | Die Paketidentität der App.                                                                                                                                                              |
| publisherName       | string  | Die Windows-Herausgeber-ID, die mit der App verknüpft ist. Diese entspricht dem **Package/Identity/Publisher**-Wert, der auf der Seite [App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) für die App im WindowsDevCenter-Dashboard angezeigt wird.                                                                                             |
| firstPublishedDate      | string  | Das Datum, an dem die App erstmals im Format ISO 8601 veröffentlicht wurde.                                                                                         |
| lastPublishedApplicationSubmission       | Objekt | Ein Objekt mit Informationen über die letzte veröffentlichte Übermittlung für die App. Weitere Informationen finden Sie unten im Abschnitt [Übermittlung](#submission_object).                                                                                                                                                          |
| pendingApplicationSubmission        | Objekt  |  Ein Objekt mit Informationen über die aktuell ausstehende Übermittlung für die App. Weitere Informationen finden Sie unten im Abschnitt [Übermittlung](#submission_object).  |   |


<span id="add-on-object" />
### Add-On

Diese Ressource enthält Informationen zu einem Add-On. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inAppProductId            | string  | Die Store-ID des Add-Ons. Dieser Wert wird im Store bereitgestellt. Beispiel für eine Store-ID: 9NBLGGH4TNMP.   |


<span id="flight-object" />
### Test-Flight

Diese Ressource enthält Informationen zu einem Flight-Paket für eine App. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
    "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
    "friendlyName": "myflight",
    "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
    },
    "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
    },
    "groupIds": [
        "1152921504606962205"
    ],
    "rankHigherThan": "Non-flighted submission"
}
```

Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | string  | Die ID für das Flight-Paket. Dieser Wert wird von Dev Center bereitgestellt.  |
| friendlyName           | string  | Der Name des Flight-Pakets nach Vorgabe des Entwicklers.   |
| lastPublishedFlightSubmission       | Objekt | Ein Objekt, das Informationen über die letzte veröffentlichte Übermittlung für das Flight-Paket enthält. Weitere Informationen finden Sie unten im Abschnitt [Übermittlung](#submission_object).  |
| pendingFlightSubmission        | Objekt  |  Ein Objekt, das Informationen über die aktuell ausstehende Übermittlung für das Flight-Paket enthält. Weitere Informationen finden Sie unten im Abschnitt [Übermittlung](#submission_object).  |    
| groupIds           | Array  | Ein Array von Zeichenfolgen, die die IDs der Test-Flight-Gruppen enthalten, die dem Flight-Paket zugeordnet sind. Weitere Informationen zu Test-Flight-Gruppen finden Sie unter [Flight-Pakete](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | string  | Der Anzeigename des Flight-Pakets, das den unmittelbar niedrigeren Rang als das aktuelle Flight-Paket erhält. Weitere Informationen zur Bewertung von Test-Flight-Gruppen finden Sie unter [Flight-Pakete](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


<span id="submission_object" />
### Übermittlung

Diese Ressource enthält Informationen zu einer Übermittlung. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
}
```

Diese Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Die ID der Übermittlung.    |
| resourceLocation   | string  | Ein relativer Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` anfügen können, um die vollständigen Daten für die Übermittlung abzurufen.                                                                                                                                               |
 
<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von App-Übermittlungen mithilfe der Windows Store-Übermittlungs-API](manage-app-submissions.md)
* [Abrufen aller Apps](get-all-apps.md)
* [Abrufen einer App](get-an-app.md)
* [Abrufen von Add-Ons für eine App](get-add-ons-for-an-app.md)
* [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md)



<!--HONumber=Aug16_HO5-->


