---
author: payzer
title: "Geräteportal – Xbox-Entwickler-DevKit – Update Richtlinien-API-Referenz"
description: "Erfahren Sie, wie Sie die Update-Richtlinie für die Konsole programmgesteuert festlegen können."
translationtype: Human Translation
ms.sourcegitcommit: 8f02e0c2f6fa30a3ac56945347c5bec253189bd8
ms.openlocfilehash: f9313d3c8b93ba13074c547f1f63c9f3204f0f58

---

HINWEIS: Diese API ist in der nächsten Entwicklervorschau erhältlich.

# Systemupdate-Richtlinien-API-Referenz   
Sie können mit dieser API ermitteln, welche Update-Richtlinie auf Ihre Konsole angewendet wird, und eine neue Richtlinie festlegen.

WICHTIG: Die meisten Konsolen erhalten beim Versuch, diese API aufzurufen, die Antwort „Zugriff verweigert“. Dies liegt daran, dass nicht alle Entwicklungskonsolen in der Lage sind, ihre Update-Richtlinie zu ändern.

Diese API wirkt sich nur auf die Update-Richtlinie für Konsolen im Entwicklermodus und nicht auf handelsübliche Konsolen aus.

## Die Update-Richtlinie für die Konsole abrufen

**Anforderung**

Sie können die Update-Richtlinie für Ihre Konsole mit der folgenden Anforderung abrufen.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/Update/Policy
<br />
**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**   
Die Antwort ist ein JSON-Array, das die Systemupdate-Gruppenmitgliedschaften der Konsole enthält. Jedes Objekt hat folgende Felder:   

ID - (Zeichenfolge) die ID der Update-Gruppe.   
Anzeigename - (Zeichenfolge) der Anzeigename der Update-Gruppe.   
Beschreibung - ("Zeichenfolge") die Beschreibung der Update-Gruppe.
IsDevKitGroup - (true | false) gibt an, ob sich die Update-Gruppe auf Entwickler-Builds bezieht.
ResourceSetID - (Zeichenfolge) Ignorieren - wird von der Systemupdate-Infrastruktur verwendet.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

## Festlegen der Systemupdate-Richtlinien für eine Konsole
Sie können mit dieser API die Systemupdate-Gruppenmitgliedschaft für die Konsole ändern.

Hinweis: Konsolen können nur jeweils einer Systemupdate-Gruppe angehören.

**Anforderung**

Sie können die Systemupdate- Gruppenmitgliedschaft einer Konsole mit der folgenden Anforderung festlegen.

Methode      | Anforderungs-URI
:------     | :-----
POST | /ext/Update/Policy
<br />
**URI-Parameter**

- Kein

**Anforderungsheader**

- Kein

**Anforderungstext**   
Der Anforderungstext ist ein JSON-Objekt mit den folgenden Feldern:   
GroupIdToJoin - (Zeichenfolge) die ID der Systemupdate-Gruppe, der die Konsole betreten soll.  
GroupIdToLeave - (Zeichenfolge) die ID Gruppe der Systemupdate-Gruppe, die die Konsole verlassen soll.

Alle Felder sind erforderlich.

Die möglichen Werte für „GroupIDs“ sind folgende:   
* Kein Update - "b2dbed33-2845-44cc-a7a1-4a9afb29d8d9"   
* Neueste Produktionswiederherstellung - "7432ae99-8c09-48dd-99f9-a2697499e240"   
* Neueste Vorschauwiederherstellung - "a8153054-1a1b-47cc-acc9-9aed90c1f8db"    

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


