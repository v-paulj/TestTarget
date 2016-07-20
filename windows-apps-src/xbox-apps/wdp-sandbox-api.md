---
author: payzer
title: "Device Portal - Referenz zur API für den Xbox Live-Sandkasten"
description: Erfahren Sie, wie Sie programmgesteuert auf den Xbox Live-Sandkasten zugreifen.
ms.sourcegitcommit: a857ba338a971e651653193ff2149f08b1665a36
ms.openlocfilehash: c1671db55dcb05ab7a126fad271a5e49146fe9ac

---

# Referenz zur API für den Xbox Live-Sandkasten   
Mit dieser REST-API können Sie den Xbox Live-Sandkasten abrufen und festlegen.

## Abrufen des Xbox Live-Sandkastens

**Anforderung**

Mit der folgenden Anforderung können Sie den aktuellen Wert für den Xbox Live-Sandkasten des Geräts lesen:

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/xboxlive/sandbox
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**   
Sandbox (Zeichenfolge): Der aktuelle Sandkasten, in dem sich das Gerät befindet.   

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung für den Zugriff auf die Anmeldeinformationen für die Dateifreigabe wurde gewährt.
4XX | Fehlercodes
5XX | Fehlercodes

## Festlegen des Xbox Live-Sandkastens
Mit der folgenden Anforderung können Sie den Xbox Live-Sandkasten des Geräts ändern. Bei der Xbox One muss das Gerät neu gestartet werden, damit die Einstellung wirksam wird.

**Anforderung**

Mit der folgenden Anforderung können Sie den aktuellen Wert für den Xbox Live-Sandkasten des Geräts festlegen:

Methode      | Anforderungs-URI
:------     | :-----
PUT | /ext/xboxlive/sandbox
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   
Der Anforderungstext ist ein JSON-Objekt mit dem folgenden Feld:   
Sandbox (Zeichenfolge): Der neue Wert, auf den der Sandkasten des Geräts festgelegt werden soll.

**Antwort**   
Sandbox (Zeichenfolge): Der aktuelle Sandkasten, in dem sich das Gerät befindet.   

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung für den Zugriff auf die Anmeldeinformationen für die Dateifreigabe wurde gewährt.
4XX | Fehlercodes
5XX | Fehlercodes

<br />
**Verfügbare Gerätefamilien**

* Windows Xbox




<!--HONumber=Jun16_HO4-->


