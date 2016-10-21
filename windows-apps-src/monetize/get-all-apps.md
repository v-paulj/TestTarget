---
author: mcleanbyron
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um Informationen über alle Apps abzurufen, die für Ihr Windows Dev Center-Konto registriert wurden."
title: "Abrufen aller Apps, die die Windows Store-Übermittlungs-API verwenden"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 6180cc4ef94df3e28af4843685e16f2d1fdfa7ac

---

# Abrufen aller Apps, die die Windows Store-Übermittlungs-API verwenden




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um Daten für Apps abzurufen, die für Ihr Windows Dev Center-Konto registriert wurden.

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

Alle Anforderungsparameter sind optional für diese Methode. Wenn Sie diese Methode ohne Parameter aufrufen, enthält die Antwort Daten für alle Apps, die für Ihr Konto registriert wurden.
 
|  Parameter  |  Typ  |  Beschreibung  |  Erforderlich  |
|------|------|------|------|
|  top  |  int  |  Die Anzahl von Elementen, die in der Anforderung zurückgegeben werden sollen (d.h. die Anzahl der zurückzugebenden Apps). Wenn Ihr Konto über mehr Apps als der Wert verfügt, den Sie in der Abfrage festlegen, enthält der Antworttext einen relativen URI-Pfad, den Sie an den URI der Methode anfügen können, um die nächste Seite mit Daten anzufordern.  |  Nein  |
|  skip  |  int  |  Die Anzahl der Elemente, die in der Abfrage umgangen werden sollen, bevor die verbleibenden Elemente zurückgegeben werden. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Zum Beispiel werden bei top=10 und skip=0 die Elemente 1 bis 10 abgerufen, bei top=10 und skip=10 die Elemente 11 bis 20 und so weiter.  |  Nein  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie Informationen über alle Apps abgerufen werden können, die für Ihr Konto registriert wurden.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

Im folgenden Beispiel wird veranschaulicht, wie die ersten 10 Apps abgerufen werden können, die für Ihr Konto registriert sind.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Im folgenden Beispiel wird der JSON-Antworttext veranschaulicht, der von einer erfolgreichen Anforderung für die ersten 10 Apps zurückgegeben wird, die für ein Entwicklerkonto mit insgesamt 21 Apps registriert wurden. Aus Platzgründen sind in diesem Beispiel nur die Daten für die ersten beiden Apps dargestellt, die von der Anforderung zurückgegeben werden. Weitere Informationen zu den Werten im Antworttext finden Sie im folgenden Abschnitt.

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | Array  | Ein Array von Objekten, die Informationen zu jeder App enthalten, die für Ihr Konto registriert wurde. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unter [Anwendungsressource](get-app-data.md#application_object).                                                                                                                           |
| @nextLink  | string | Wenn weitere Datenseiten vorhanden sind, enthält diese Zeichenfolge einen relativen Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` zum Anfordern der nächsten Datenseite anfügen können. Wenn beispielsweise der Parameter *top* des anfänglichen Anforderungstexts auf 10 festgelegt ist, für Ihr Konto jedoch 20 Apps registriert wurden, enthält der Antworttext den @nextLink-Wert ```applications?skip=10&top=10```, der angibt, dass Sie ```https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10``` aufrufen können, um die nächsten 10 Apps anzufordern. |
| totalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage (d.h. die Gesamtanzahl von Apps, die für Ihr Konto registriert wurden).                                                                                                                                                                                                                             |

<span/>

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Es wurden keine Apps gefunden. |
| 409  | Die Apps verwenden Dev Center-Dashboard-Funktionen, die [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt werden](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen einer App](get-an-app.md)
* [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md)
* [Abrufen von Add-Ons für eine App](get-add-ons-for-an-app.md)



<!--HONumber=Aug16_HO5-->


