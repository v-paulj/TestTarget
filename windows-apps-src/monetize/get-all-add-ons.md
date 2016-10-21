---
author: mcleanbyron
ms.assetid: 7B6A99C6-AC86-41A1-85D0-3EB39A7211B6
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API, um alle Add-On-Daten für Apps abzurufen, die für Ihr Windows Dev Center-Konto registriert wurden."
title: "Abrufen aller Add-Ons, die die Windows Store-Übermittlungs-API verwenden"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 64d40badaa4664e4f516900e82d290f60a499149

---

# Abrufen aller Add-Ons, die die Windows Store-Übermittlungs-API verwenden




Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API zum Abrufen von Daten für alle Add-Ons (auch als In-App-Produkte oder IAPs bezeichnet) für alle Apps, die für Ihr Windows Dev Center-Konto registriert wurden.

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

Alle Anforderungsparameter sind optional für diese Methode. Wenn Sie diese Methode ohne Parameter aufrufen, enthält die Antwort Daten für alle Add-Ons für alle Apps, die für Ihr Konto registriert sind.
 
|  Parameter  |  Typ  |  Beschreibung  |  Erforderlich  |
|------|------|------|------|
|  top  |  int  |  Die Anzahl von Elementen, die in der Anforderung zurückgegeben werden sollen (d.h. die Anzahl der zurückzugebenden-Add-Ons). Wenn Ihr Konto über mehr Add-Ons als der Wert verfügt, den Sie in der Abfrage festlegen, enthält der Antworttext einen relativen URI-Pfad, den Sie an den URI der Methode anfügen können, um die nächste Seite mit Daten anzufordern.  |  Nein  |
|  skip  |  int  |  Die Anzahl der Elemente, die in der Abfrage umgangen werden sollen, bevor die verbleibenden Elemente zurückgegeben werden. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Zum Beispiel werden bei top=10 und skip=0 die Elemente 1 bis 10 abgerufen, bei top=10 und skip=10 die Elemente 11 bis 20 und so weiter.  |  Nein  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie alle Add-On-Daten für alle Apps abgerufen werden können, die für Ihr Konto registriert sind.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

Im folgenden Beispiel wird veranschaulicht, wie nur die ersten 10 Add-Ons abgerufen werden.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Im folgenden Beispiel wird der JSON-Antworttext veranschaulicht, der von einer erfolgreichen Anforderung für die ersten 5 Add-Ons zurückgegeben wird, die für ein Entwicklerkonto mit insgesamt 1072 Add-Ons registriert wurden. Aus Platzgründen sind in diesem Beispiel nur die Daten für die ersten beiden Add-Ons dargestellt, die von der Anforderung zurückgegeben werden. Weitere Informationen zu den Werten im Antworttext finden Sie im folgenden Abschnitt.

```json
{
  "@nextLink": "inappproducts/?skip=5&top=5",
  "value": [
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMP",
      "productId": "a8b8310b-fa8d-4da0-aca0-577ef6dce4dd",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243619",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243705",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
      }
    },
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMN",
      "productId": "6a3c9788-a350-448a-bd32-16160a13018a",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243538",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243538"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243106",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243106"
      }
    },

  // Other add-ons omitted for brevity...
  ],
  "totalCount": 1072
}
```

### Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | string | Wenn weitere Datenseiten vorhanden sind, enthält diese Zeichenfolge einen relativen Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` zum Anfordern der nächsten Datenseite anfügen können. Wenn beispielsweise der Parameter *top* des anfänglichen Anforderungstexts auf 10 festgelegt ist, für Ihr Konto jedoch 100 Add-Ons registriert wurden, enthält der Antworttext den @nextLink-Wert ```inappproducts?skip=10&top=10```, der angibt, dass Sie ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?skip=10&top=10``` aufrufen können, um die nächsten 10 Add-Ons anzufordern. |
| value            | Array  |  Ein Array, das die Objekte enthält, die Informationen über jedes Add-On bereitstellen. Weitere Informationen finden Sie unter [Add-On-Ressource](manage-add-ons.md#add-on-object).   |
| totalCount   | int  | Die Anzahl der App-Objekte im *value*-Array des Antworttexts.                                                                                                                                                 |



## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Es wurden keine Add-Ons gefunden. |
| 409  | Die Apps oder Add-Ons verwenden Dev Center-Dashboard-Funktionen, die [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt werden](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md)
* [Abrufen eines Add-Ons](get-an-add-on.md)
* [Erstellen eines Add-Ons](create-an-add-on.md)
* [Löschen eines Add-Ons](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


