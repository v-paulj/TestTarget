---  
author: WilliamsJason
title: Xbox Live-Test - Referenz zu den Benutzerverwaltungs-APIs
description: Erfahren Sie, wie Sie programmgesteuert auf die Benutzerverwaltungs-APIs zugreifen.
ms.sourcegitcommit: 67f158b1d3d5ece14c36483a2513a2db2f478660
ms.openlocfilehash: ad01d4daf089c61fc50c7927cfbf123d7d7ee4df

---  

#Xbox Live-Benutzerverwaltung#

**Anforderung**

Sie können die Liste der Benutzer auf der Konsole abrufen oder aktualisieren – Benutzer hinzufügen, entfernen, anmelden, abmelden oder vorhandene Benutzer ändern.

| Methode        | Anforderungs-URI     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |
<br>

**URI-Parameter**

* Keine

**Anforderungsheader**

* Keine

**Anforderungstext**

PUT-Aufrufe müssen ein JSON-Array mit folgender Struktur enthalten:

* Benutzer
  * AutoSignIn (optional): Ein boolescher Wert, der die automatische Anmeldung für das durch EmailAddress oder UserId angegebene Konto deaktiviert oder aktiviert.
  * EmailAddress (optional – muss angegeben werden, wenn UserId nicht angegeben ist, es sei denn, ein gesponserter Benutzer wird angemeldet): Die E-Mail-Adresse, die den Benutzer angibt, der geändert/hinzugefügt/gelöscht werden soll.
  * Password (optional – muss angegeben werden, wenn der Benutzer derzeit nicht auf der Konsole enthalten ist): Das Kennwort, das zum Hinzufügen eines neuen Benutzers zur Konsole verwendet wird.
  * SignedIn (optional): Ein boolescher Wert, der angibt, ob das angegebene Konto an- oder abgemeldet werden soll.
  * UserId (optional – muss angegeben werden, wenn EmailAddress nicht angegeben ist, es sei denn, ein gesponserter Benutzer wird angemeldet): Die Benutzer-ID, die den Benutzer angibt, der geändert/hinzugefügt/gelöscht werden soll.
  * SponsoredUser (optional): Ein boolescher Wert, der angibt, ob ein gesponserter Benutzer hinzugefügt werden soll.
  * Delete (optional): Ein boolescher Wert, der angibt, ob dieser Benutzer von der Konsole gelöscht werden soll.

###Antwort###

**Antworttext**

GET-Aufrufe geben ein JSON-Array mit folgenden Eigenschaften zurück:

* Benutzer
  * AutoSignIn (optional)
  * EmailAddress (optional)
  * Gamertag
  * SignedIn
  * UserId
  * XboxUserId
  * SponsoredUser (optional)
  
**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode   | Beschreibung     | 
| ------------------ |-----------------|
| 200                | Der GET-Aufruf war erfolgreich, und im Antworttext wurde ein JSON-Array mit Benutzern zurückgegeben. |
| 204                | Der PUT-Aufruf war erfolgreich, und die Benutzer auf der Konsole wurden aktualisiert. |
| 4XX                | Verschiedene Fehler für ungültige Anforderungsdaten oder ein ungültiges Anforderungsformat |
| 5XX                | Fehlercodes für unerwartete Fehler |
<br>





<!--HONumber=Jun16_HO4-->


