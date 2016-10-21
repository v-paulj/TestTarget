---
author: mcleanbyron
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: "Verwenden Sie diese Methode in der Windows Store-Übermittlungs-API zum Löschen eines Add-Ons für eine App, die für Ihr Windows Dev Center-Konto registriert ist."
title: "Löschen eines Add-Ons mit der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 8c28205ca004bd63b3f6a3fea2971cf927f7e2e4

---

# Löschen eines Add-Ons mit der Windows Store-Übermittlungs-API




Verwenden Sie diese Methode der Windows Store-Übermittlungs-API, um ein Add-On (auch als In-App-Produkt oder IAP bezeichnet) für eine App zu löschen, die in Ihrem Windows Dev Center-Konto registriert wurde.

## Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60Minuten Zeit, das Token zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

>**Hinweis**&nbsp;&nbsp;Diese Methode kann nur für Windows Dev Center-Konten verwendet werden, die eine Berechtigung zur Verwendung der Windows Store-Übermittlungs-API erhalten haben. Diese Berechtigung ist nicht für alle Konten aktiviert.

## Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |

<span/>
 

### Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

<span/>

### Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | String | Erforderlich. Die Store-ID des zu löschenden Add-Ons. Die Store-ID ist im Dev Center-Dashboard verfügbar.  |

<span/>

### Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

<span/>

### Anforderungsbeispiel

Im folgenden Beispiel wird das Löschen eines Add-Ons veranschaulicht.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## Antwort

Wenn dies erfolgreich war, gibt die Methode einen leeren Antworttext zurück.

## Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung                                                                                                                                                                           |
|--------|------------------|
| 400  | Die Anforderung ist ungültig. |
| 404  | Das angegebene Add-On konnte nicht gefunden werden.  |
| 409  | Das angegebene Add-On wurde gefunden, konnte jedoch nicht im aktuellen Zustand gelöscht werden. Oder das Add-On verwendet ein Dev Center-Dashboard-Feature, das [derzeit nicht von der Windows Store-Übermittlungs-API unterstützt wird](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen aller Add-Ons](get-all-add-ons.md)
* [Abrufen eines Add-Ons](get-an-add-on.md)
* [Erstellen eines Add-Ons](create-an-add-on.md)



<!--HONumber=Aug16_HO5-->


