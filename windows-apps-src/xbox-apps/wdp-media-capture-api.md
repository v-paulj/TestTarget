---
author: WilliamsJason
title: Referenz zur Medienerfassungs-API
description: Erfahren Sie, wie Sie programmgesteuert auf die Medienerfassungs-API zugreifen.
ms.sourcegitcommit: 4356bd2cfc7697905ed91d60b5829c06d850e109
ms.openlocfilehash: 2e32c4d516a35b5f5bd8d57bce75ff8bdce6b461

---

# Referenz zur Medienerfassungs-API #

**Anforderung**

Mithilfe des folgenden Anforderungsformats können Sie eine PNG-Darstellung des aktuellen Bildschirms erfassen.

| Methode        | Anforderungs-URI     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:


| URI-Parameter      | Beschreibung     | 
| ------------------ |-----------------|
| download (optional)| Ein boolescher Wert, der angibt, ob HTTP-Antwortheader festgelegt werden sollen, die angeben, dass der Hostbrowser den Screenshot als Anhang herunterladen soll, anstatt ihn im Browser zu rendern.  |
<br>

**Anforderungsheader**

* Keine

**Anforderungstext**

* Keiner

###Antwort###

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode   | Beschreibung     | 
| ------------------ |-----------------|
| 200                | Die Screenshotanforderung war erfolgreich, und der erfasste Inhalt wurde zurückgegeben. |
| 5XX                | Fehlercodes für unerwartete Fehler |
<br>

**Verfügbare Gerätefamilien**

* Windows Xbox




<!--HONumber=Jun16_HO4-->

