---
author: WilliamsJason
title: Device Portal - Referenz zu den APIs zum Hochladen von Ordnern
description: Erfahren Sie, wie Sie programmgesteuert auf die APIs zum Hochladen von Ordnern zugreifen.
ms.sourcegitcommit: fdc25fa4bd7bd5bfa598b993f23cd0ae9783dd0e
ms.openlocfilehash: 942ddc13b0deba382ad7758bc30bd9a5b0cceb11

---

# Hochladen eines Ordners in das Entwicklungsverzeichnis

**Anforderung**

Sie können einen vollständigen Ordner unter der ID für bekannte Ordner in den Ordner „DevelopmentFiles“ (oder in einen Unterordner innerhalb dieses Ordners) auf einmal hochladen.

Methode      | Anforderungs-URI
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter      | Beschreibung
:------     | :-----
destinationFolder (erforderlich) | Der Name des Zielordners für den Ordner, der hochgeladen werden soll. Dieser Ordner wird unter „d:\developmentfiles\LooseApps“ auf der Konsole gespeichert. Der Ordnername muss base64-codiert sein, da er Pfadtrennzeichen enthalten kann, wenn der Ordner ein Unterordner unter „LooseApps“ ist.
<br />

**Anforderungsheader**

- Keine

**Anforderungstext**

- Multipart-konformer HTTP-Text des Verzeichnisinhalts.

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Erfolg
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Xbox




<!--HONumber=Jun16_HO4-->


