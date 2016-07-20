---
author: payzer
title: Device Portal - Referenz zu SMB-APIs
description: Erfahren Sie, wie Sie programmgesteuert auf die SMB-APIs zugreifen.
ms.sourcegitcommit: 3d76bf181baa9dfd973467d43241230fddf2daf7
ms.openlocfilehash: 5efe2af3524d97e6014c4d6be2a8f1aef22f2e66

---

# Referenz zur API für den Entwicklerordner   
Für den Zugriff auf Dateien auf Ihrer Xbox One, die sich auf die Entwicklung beziehen, können Sie einen standardmäßigen Datei-Explorer verwenden. Dadurch können Sie problemlos Dateien von Ihrem PC anzeigen und auf der Konsole ersetzen.

**Anforderung**

Sie können mithilfe der folgenden Anforderung auf den Entwicklerordner zugreifen. Die Anforderung gibt Folgendes zurück:    
* Den Speicherort der Dateifreigabe. Dieser Speicherort kann in die Adressleiste eines Datei-Explorers eingegeben werden.
* Den Benutzernamen für den Zugriff auf die Dateifreigabe.
* Das Kennwort für den Zugriff auf die Dateifreigabe.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/smb/developerfolder
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**   
Path: Der Pfad zur Dateifreigabe mit den Entwicklerdateien.   
Username: Der Benutzername für den Zugriff auf die Freigabe mit den Entwicklerdateien.   
Password: Das Kennwort für den Zugriff auf die Freigabe mit den Entwicklerdateien.   

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


