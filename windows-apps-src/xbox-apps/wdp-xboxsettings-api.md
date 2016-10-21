---
author: payzer
title: "Device Portal - Referenz zur API für Xbox-Entwicklereinstellungen"
description: Erfahren Sie, wie Sie auf Xbox-Entwicklereinstellungen zugreifen.
translationtype: Human Translation
ms.sourcegitcommit: c51eff41e63d815f6298b4fc46a9b11314bc8bc9
ms.openlocfilehash: 5a983714cda9b5a5f45e555e2cb6f980f082a003

---

# Referenz zur API für Entwicklereinstellungen   
Mit dieser API können Sie auf Xbox One-Einstellungen zugreifen, die für die Entwicklung nützlich sind.

## Gleichzeitiges Abrufen aller Entwicklereinstellungen

**Anforderung**

Mithilfe der folgenden Anforderung können Sie alle Entwicklereinstellungen in einer einzigen Anforderung abrufen.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/settings
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**   
Die Antwort ist ein JSON-Einstellungsarray mit allen Einstellungen. Jedes Einstellungsobjekt enthält die folgenden Felder:   

Name (Zeichenfolge): Der Name der Einstellung.   
Value (Zeichenfolge): Der Wert der Einstellung.   
RequiresReboot („Yes“|„No“): Dieses Feld gibt an, ob ein Neustart erforderlich ist, damit die Einstellung wirksam wird.
Kategorie (Zeichenfolge): Die Kategorie der Einstellung.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

## Abrufen einzelner Einstellungen
Einstellungen können auch einzeln abgerufen werden.

**Anforderung**

Mit der folgenden Anforderung können Sie Informationen zu einer einzelnen Einstellung abrufen.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/settings/\<setting name\>
<br />
**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**   
Die Antwort ist ein JSON-Objekt mit folgenden Feldern:   

Name (Zeichenfolge): Der Name der Einstellung.   
Value (Zeichenfolge): Der Wert der Einstellung.   
RequiresReboot („Yes“|„No“): Dieses Feld gibt an, ob ein Neustart erforderlich ist, damit die Einstellung wirksam wird.
Kategorie (Zeichenfolge): Die Kategorie der Einstellung.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

## Festlegen des Werts einer Einstellung
Sie können den Wert einer Einstellung festlegen.

**Anforderung**

Mit der folgenden Anforderung können Sie den Wert für eine Einstellung festlegen.

Methode      | Anforderungs-URI
:------     | :-----
PUT | /ext/settings/\<setting name\>
<br />
**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**   
Der Anforderungstext ist ein JSON-Objekt mit dem folgenden Feld:   
Value (Zeichenfolge): Der neue Wert für die Einstellung.

**Antwort**   

- Keine

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

<br />
**Verfügbare Gerätefamilien**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


