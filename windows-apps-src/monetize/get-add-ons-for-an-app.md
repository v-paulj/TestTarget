---
author: mcleanbyron
ms.assetid: E59FB6FE-5318-46DF-B050-73F599C3972A
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API zum Abrufen von Informationen über In-App-Einkäufe für eine App, die für Ihr Windows Dev Center-Konto registriert wurde."
title: "Abrufen von Add-Ons für eine App mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 1edf52b45578078f7abb7e499723b072832d6628

---

# Abrufen von Add-Ons für eine App mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, die Add-Ons (auch als In-App-Produkte oder IAPs bezeichnet) für eine App auflisten, die für Ihr Windows Dev Center-Konto registriert wurde.

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` |

<span/>
 
### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung  |  Erforderlich  |    
|---------------|--------|----------------------------------|
| applicationId | string | Erforderlich. Die Store-ID der App, für die Sie Add-Ons abrufen möchten. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Ja  |
|  top  |  int  |  Die Anzahl von Elementen, die in der Anforderung zurückgegeben werden sollen (d.h. die Anzahl der zurückzugebenden-Add-Ons). Wenn die App mehr Add-Ons als der Wert verfügt, den Sie in der Abfrage festlegen, enthält der Antworttext einen relativen URI-Pfad, den Sie an den URI der Methode anfügen können, um die nächste Seite mit Daten anzufordern.  |  Nein  |
|  skip  |  int  |  Die Anzahl der Elemente, die in der Abfrage umgangen werden sollen, bevor die verbleibenden Elemente zurückgegeben werden. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Zum Beispiel werden bei top=10 und skip=0 die Elemente 1 bis 10 abgerufen, bei top=10 und skip=10 die Elemente 11 bis 20 und so weiter.  |  Nein  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie alle Add-Ons für eine App aufgelistet werden können.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

Im folgenden Beispiel wird veranschaulicht, wie die ersten 10 Add-Ons für eine App aufgelistet werden können.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext, der von einer erfolgreichen Anforderung für die ersten 10 Add-Ons für eine App mit insgesamt 53 Add-Ons zurückgegeben wird. Aus Platzgründen sind in diesem Beispiel nur die Daten für die ersten drei Add-Ons dargestellt, die von der Anforderung zurückgegeben werden. Weitere Informationen zu den Werten im Antworttext finden Sie im folgenden Abschnitt.

```json
{
  "@nextLink": "applications/9NBLGGH4R315/listinappproducts/?skip=10&top=10",
  "value": [
    {
      "inAppProductId": "9NBLGGH4TNMP"
    },
    {
      "inAppProductId": "9NBLGGH4TNMN"
    },
    {
      "inAppProductId": "9NBLGGH4TNNR"
    },
    // Next 7 add-ons  are omitted for brevity ...
  ],
  "totalCount": 53
}
```

### Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | string | Wenn weitere Datenseiten vorhanden sind, enthält diese Zeichenfolge einen relativen Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` zum Anfordern der nächsten Datenseite anfügen können. Wenn beispielsweise der Parameter *top* des anfänglichen Anforderungstexts auf 10 festgelegt ist, für die App jedoch 50 Add-Ons vorhanden sind, enthält der Antworttext den @nextLink-Wert ```applications/{applicationid}/listinappproducts/?skip=10&top=10```, der angibt, dass Sie ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listinappproducts/?skip=10&top=10``` aufrufen können, um die nächsten 10 Add-Ons anzufordern. |
| value      | Array  | Ein Array von Objekten, die die Store-ID für jedes Add-On für die angegebene App auflisten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unter [-Add-On-Ressource](get-app-data.md#add-on-object).                                                                                                                           |
| totalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage (d.h. die Gesamtanzahl der Add-Ons für die angegebene App).                                                                                                                                                                                                                             |

<span/>

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Es wurden keine Add-Ons gefunden. |
| 409  | Die Add-Ons verwenden Dev Center-Dashboard-Funktionen, die [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt werden](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen aller Apps](get-all-apps.md)
* [Abrufen einer App](get-an-app.md)
* [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md)



<!--HONumber=Aug16_HO5-->


