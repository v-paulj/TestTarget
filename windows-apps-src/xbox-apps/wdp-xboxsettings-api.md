---
author: payzer
title: "Device Portal - Referenz zur API für Xbox-Entwicklereinstellungen"
description: Erfahren Sie, wie Sie auf Xbox-Entwicklereinstellungen zugreifen.
translationtype: Human Translation
ms.sourcegitcommit: a9a2b6e58dfa0d1e77164a59f204deabf8f5c3e0
ms.openlocfilehash: e3637f5a8481c0800af42c011fb811b908b946b1

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
GET | /ext/settings/<setting name>
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

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
PUT | /ext/settings/<setting name>
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

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




<!--HONumber=Jul16_HO2-->


