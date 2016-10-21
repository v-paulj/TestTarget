---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "Verwenden Sie diese Methoden in der Windows Store-Übermittlungs-API, um Flight-Pakete für Apps zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden."
title: "Verwalten von Flight-Paketen mithilfe der Windows Store-Übermittlungs-API"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: de1c23d721cee67d813520e3a23eb553cd90b7e9

---

# Verwalten von Flight-Paketen mithilfe der Windows Store-Übermittlungs-API




Verwenden Sie die folgenden Methoden in der Windows Store-Übermittlungs-API, um Flight-Pakete für Apps zu verwalten, die in Ihrem Windows Dev Center-Konto registriert wurden. Eine Einführung in die Windows Store-Übermittlungs-API einschließlich der Voraussetzungen für die Verwendung der API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit WindowsStore-Diensten](create-and-manage-submissions-using-windows-store-services.md).

>**Hinweis**&nbsp;&nbsp;Diese Methoden können nur für Windows Dev Center-Konten verwendet werden, die zur Verwendung der Windows Store-Übermittlungs-API berechtigt sind. Diese Berechtigung ist nicht für alle Konten aktiviert. Diese Methoden können nur verwendet werden, um Flight-Pakete abzurufen, zu erstellen oder zu löschen. Verwenden Sie zum Erstellen von Übermittlungen für Flight-Pakete die Methoden unter [Verwalten von Flight-Paket-Übermittlungen](manage-flight-submissions.md).

| Methode        | URI    | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | Ruft Daten für ein Flight-Paket für eine App ab, die in Ihrem Windows Dev Center-Konto registriert ist. Weitere Informationen finden Sie unter [Abrufen eines Flight-Pakets](get-a-flight.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` | Erstellt ein neues Flight-Paket. Weitere Informationen finden Sie unter [Erstellen eines Flight-Pakets](create-a-flight.md).|
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | Löscht ein Flight-Paket. Weitere Informationen finden Sie unter [Löschen eines Flight-Pakets](delete-a-flight.md). |


## Voraussetzungen

Falls noch nicht geschehen, erfüllen Sie vor der Verwendung dieser Methoden alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Windows Store-Übermittlungs-API.

## Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit Windows Store-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paket-Übermittlungen](manage-flight-submissions.md)



<!--HONumber=Aug16_HO5-->


