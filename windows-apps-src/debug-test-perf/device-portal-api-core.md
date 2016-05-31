---
author: dbirtolo
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Referenz zu Kern-APIs des Device Portal
description: Hier erhalten Sie Informationen zu den Kern-REST-APIs für das Windows Device Portal, die Sie für den Zugriff auf die Daten und die programmatische Steuerung des Geräts verwenden können.
---

# Referenz zu Kern-APIs des Geräteportals

Alle Komponenten im Windows Device Portal basieren auf REST-APIs, die Sie für den Zugriff auf die Daten und die programmatische Steuerung des Geräts verwenden können.

## App-Bereitstellung

---
### Installieren einer App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine App installieren.

Methode      | Anforderungs-URI
:------     | :-----
POST | /api/app/packagemanager/package
<br />
**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
package   | (**erforderlich**) Der Dateiname des zu installierenden Pakets.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Bereitstellungsanforderung akzeptiert und wird verarbeitet
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Abrufen des App-Installationsstatus

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Status einer derzeit ausgeführten App-Installation abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/app/packagemanager/state
<br />
**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Das Ergebnis der letzten Bereitstellung
204 | Die Installation läuft
404 | Keine Installationsaktion wurde gefunden
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Deinstallieren einer App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine App deinstallieren.
 
Methode      | Anforderungs-URI
:------     | :-----
DELETE | /api/app/packagemanager/package
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
### Abrufen installierter Apps

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Liste der auf dem System installierten Apps abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/app/packagemanager/packages
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält eine Liste der installierten Pakete mit zugehörigen Details. Die Vorlage für diese Antwort lautet wie folgt.
```
{"InstalledPackages": [
    {
        "Name": string,
        "PackageFamilyName": string,
        "PackageFullName": string,
        "PackageOrigin": int, (https://msdn.microsoft.com/en-us/library/windows/desktop/dn313167(v=vs.85).aspx)
        "PackageRelativeId": string,
        "Publisher": string,
        "Version": {
            "Build": int,
            "Major": int,
            "Minor": int,
            "Revision": int
     },
     "RegisteredUsers": [
     {
        "UserDisplayName": string,
        "UserSID": string
     },...
     ]
    },...
]}
```
**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
## Geräte-Manager
---
### Abrufen der auf dem Computer installierten Geräte

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Liste der auf dem Computer installierten Geräte abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/devicemanager/devices
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält ein JSON-Array von Geräten, die mit dem Gerät verbunden sind.
``` 
{"DeviceList": [
    {
        "Class": string,
        "Description": string,
        "ID": string,
        "Manufacturer": string,
        "ParentID": string,
        "ProblemCode": int,
        "StatusCode": int
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* IoT

---
## Absturzabbildsammlung
---
### Abrufen der Liste alle Absturzabbilder für Apps

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Liste aller verfügbaren Absturzabbilder für jede quergeladene App abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/debug/dump/usermode/dumps
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält eine Liste der Absturzabbilder für jede quergeladene Anwendung.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Abrufen der Absturzabbildsammlungs-Einstellungen für eine App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Absturzabbildsammlungs-Einstellungen für eine quergeladene App abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/debug/dump/usermode/crashcontrol
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort weist das folgende Format auf.
```
{"CrashDumpEnabled": bool}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Löschen eines Absturzabbilds für eine quergeladene App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das Absturzabbild einer quergeladenen App löschen.
 
Methode      | Anforderungs-URI
:------     | :-----
DELETE | /api/debug/dump/usermode/crashdump
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.
fileName   | (**erforderlich**) Der Name der Absturzabbilddatei, die gelöscht werden soll.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Deaktivieren der Absturzabbilder für eine quergeladene App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie Absturzabbilder für eine quergeladene App deaktivieren.
 
Methode      | Anforderungs-URI
:------     | :-----
DELETE | /api/debug/dump/usermode/crashcontrol

<br />
**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Herunterladen des Absturzabbilds für eine quergeladene App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das Absturzabbild einer quergeladenen App herunterladen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/debug/dump/usermode/crashdump
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.
fileName   | (**erforderlich**) Der Name der Absturzabbilddatei, die Sie herunterladen möchten.
<br />
**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält eine Absturzabbilddatei. Sie können die Absturzabbilddatei mit WinDbg oder Visual Studio untersuchen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Aktivieren der Absturzabbilder für eine quergeladene App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie Absturzabbilder für eine quergeladene App aktivieren.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/debug/dump/usermode/crashcontrol
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Abrufen der Liste der Fehlerüberprüfungsdateien

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Liste der Fehlerüberprüfungs-Minidumpdateien abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/debug/dump/kernel/dumplist
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält eine Liste der Minidumpdateinamen und die Größen dieser Dateien. Diese Liste wird das folgende Format aufweisen. Der zweite *FileName*-Parameter ist die Größe der Datei. Dies ist ein bekanntes Problem.
```
{"DumpFiles": [
    {
        "FileName": string,
        "FileName": string
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Herunterladen einer Fehlerüberprüfungs-Speicherabbilddatei

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Fehlerüberprüfungs-Speicherabbilddatei herunterladen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/debug/dump/kernel/dump
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
filename   | (**erforderlich**) Der Dateiname der Speicherabbilddatei. Sie finden diesen mithilfe der API zum Abrufen der Speicherabbildliste.
<br />
**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält die Speicherabbilddatei. Sie können diese Datei mithilfe von WinDbg untersuchen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Abrufen der CrashControl-Fehlerüberprüfungseinstellungen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die CrashControl-Fehlerüberprüfungseinstellungen abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/debug/dump/kernel/crashcontrol

<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält die CrashControl-Einstellungen. Weitere Informationen zu CrashControl finden Sie im Artikel [](https://technet.microsoft.com/library/cc951703.aspx). Die Vorlage für die Antwort lautet wie folgt.
```
{
    "autoreboot": int,
    "dumptype": int,
    "maxdumpcount": int,
    "overwrite": int
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Abrufen eines Live-Kernelspeicherabbilds

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie ein Live-Kernelspeicherabbild abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/debug/dump/livekernel
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält das vollständige Kernelmodus-Speicherabbild. Sie können diese Datei mithilfe von WinDbg untersuchen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Abrufen eines Speicherabbilds von einem Livebenutzerprozess

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das Speicherabbild von einem Livebenutzerprozess abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/debug/dump/usermode/live
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
pid   | (**erforderlich**) Die eindeutige Prozess-ID für den betreffenden Prozess.
<br />
**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält die Prozesssicherung. Sie können diese Datei mit WinDbg oder Visual Studio untersuchen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Festlegen der CrashControl-Fehlerüberprüfungseinstellungen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Einstellungen zum Sammeln von Fehlerüberprüfungsdaten festlegen.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/debug/dump/kernel/crashcontrol
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
autoreboot   | (**optional**) True oder False. Dieser Parameter gibt an, ob das System nach einem Fehler oder Einfrieren automatisch neu gestartet wird.
dumptype   | (**optional**) Der Speicherabbildtyp. Die unterstützten Werte finden Sie unter [CrashDumpType-Enumeration](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).
maxdumpcount   | (**optional**) Die maximale Anzahl der zu speichernden Speicherabbilder.
overwrite   | (**optional**) True oder False. Dieser Parameter gibt an, ob alte Speicherabbilder überschrieben werden, wenn der durch *maxdumpcount* angegebene Höchstwert für die Anzahl von Speicherabbildern erreicht wurde.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
## ETW
---
### Erstellen einer Echtzeit-ETW-Sitzung über ein Websocket

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Echtzeit-ETW-Sitzung erstellen. Dies erfolgt über ein Websocket.  ETW-Ereignisse werden auf dem Server zusammengefasst und einmal pro Sekunde an den Client gesendet. 
 
Methode      | Anforderungs-URI
:------     | :-----
GET/WebSocket | /api/etw/session/realtime
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält die ETW-Ereignisse von den aktivierten Anbietern.  ETW-WebSocket-Befehle finden Sie unten. 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

### ETW-WebSocket-Befehle
Diese Befehle werden vom Client an den Server gesendet.

Befehl | Beschreibung
:----- | :-----
Anbieter *{guid}* aktivieren *{level}* | Den durch *{guid}* (ohne Klammern) markierten Anbieter auf der angegebenen Ebene aktivieren. *{level}* ist ein **int** von 1 (am wenigsten detailliert) bis 5 (ausführlich).
Anbieter *{guid}* deaktivieren | Den durch *{guid}* (ohne Klammern) markierten Anbieter deaktivieren.

Diese Antworten werden vom Server an den Client gesendet. Diese werden als Text gesendet, und erhalten Sie das folgende Format durch eine JSON-Analyse.
```
{
    "Events":[
        {
            "Timestamp": int,
            "Provider": string,
            "ID": int, 
            "TaskName": string,
            "Keyword": int,
            "Level": int,
            payload objects...
        },...
    ],
    "Frequency": int
}
```

Nutzlast-Objekte sind zusätzliche Schlüssel-Wert-Paare (Zeichenkette:Zeichenkette), die im ursprünglichen ETW-Ereignis bereitgestellt werden.

Beispiel:
```
{
    "ID" : 42, 
    "Keyword" : 9223372036854775824, 
    "Level" : 4, 
    "Message" : "UDPv4: 412 bytes transmitted from 10.81.128.148:510 to 132.215.243.34:510. ",
    "PID" : "1218", 
    "ProviderName" : "Microsoft-Windows-Kernel-Network", 
    "TaskName" : "KERNEL_NETWORK_TASK_UDPIP", 
    "Timestamp" : 131039401761757686, 
    "connid" : "0", 
    "daddr" : "132.245.243.34", 
    "dport" : "500", 
    "saddr" : "10.82.128.118", 
    "seqnum" : "0", 
    "size" : "412", 
    "sport" : "500"
}
```

---
### Auflisten der registrierten ETW-Anbieter

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die registrierten Anbieter auflisten.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/etw/providers
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält die Liste der ETW-Anbieter. Diese Liste enthält den Anzeigenamen und die GUID für jeden Anbieter im folgenden Format.
```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Auflisten der benutzerdefinierten ETW-Anbieter, die von der Plattform verfügbar gemacht werden.

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die registrierten Anbieter auflisten.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/etw/customproviders
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

200 OK. Die Antwort enthält die Liste der ETW-Anbieter. Diese Liste enthält den Anzeigenamen und die GUID für jeden Anbieter.

```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Statuscode**

- Standardstatuscodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
## Betriebssysteminformationen
---
### Abrufen des Computernamens

**Anforderung**

Sie können den Namen eines Computers durch Verwendung des folgenden Anforderungsformats abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/os/machinename
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält den Namen des Computers im folgenden Format. 

```
{"ComputerName": string}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
### Abrufen der Betriebssysteminformationen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Betriebssysteminformationen für einen Computer abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/os/info
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält die Betriebssysteminformationen im folgenden Format.

```
{
    "ComputerName": string,
    "OsEdition": string,
    "OsEditionId": int,
    "OsVersion": string,
    "Platform": string
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
### Festlegen des Computernamens

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Namen eines Computers festlegen.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/os/machinename
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
name | (**erforderlich**) Der neue Name für den Computer.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
## Leistungsdaten
---
### Abrufen der Liste der ausgeführten Prozesse

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Liste der derzeit ausgeführten Prozesse abrufen.  Dies kann auch zu einer WebSocket-Verbindung aktualisiert werden, wobei einmal pro Sekunde die gleichen JSON-Daten per Push an den Client gesendet werden. 
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/resourcemanager/processes
GET/WebSocket | /api/resourcemanager/processes
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält eine Liste der Prozesse mit Details für jeden Prozess. Die Informationen sind im JSON-Format und haben die folgende Vorlage.
```
{"Processes": [
    {
        "CPUUsage": int,
        "ImageName": string,
        "PageFileUsage": int,
        "PrivateWorkingSet": int,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": int,
        "WorkingSetSize": int
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Abrufen der Leistungsstatistik des Systems

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Leistungsstatistik des Systems abrufen. Diese Informationen umfassen z. B. Lese- und Schreibzyklen sowie die Menge des genutzten Arbeitsspeichers.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/resourcemanager/systemperf
GET/WebSocket | /api/resourcemanager/systemperf
<br />
Dies kann auch auf eine WebSocket-Verbindung aktualisiert werden.  Sie stellt unten einmal pro Sekunde die gleichen JSON-Daten bereit. 

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält die Leistungsstatistik für das System, z. B. CPU- und GPU-Nutzung, Speicherzugriff und Netzwerkzugriff. Diese Informationen sind im JSON-Format und haben die folgende Vorlage.
```
{
    "AvailablePages": int,
    "CommitLimit": int,
    "CommittedPages": int,
    "CpuLoad": int,
    "IOOtherSpeed": int,
    "IOReadSpeed": int,
    "IOWriteSpeed": int,
    "NonPagedPoolPages": int,
    "PageSize": int,
    "PagedPoolPages": int,
    "TotalInstalledInKb": int,
    "TotalPages": int,
    "GPUData": 
    {
        "AvailableAdapters": [{ (One per detected adapter)
            "DedicatedMemory": int,
            "DedicatedMemoryUsed": int,
            "Description": string,
            "SystemMemory": int,
            "SystemMemoryUsed": int,
            "EnginesUtilization": [ float,... (One per detected engine)]
        },...
    ]},
    "NetworkingData": {
        "NetworkInBytes": int,
        "NetworkOutBytes": int
    }
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
## Stromversorgung
---
### Abrufen des aktuellen Akkustatus

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den aktuellen Akkustatus abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/battery
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Informationen zum aktuellen Akkustatus werden im folgenden Format zurückgegeben.
```
{
    "AcOnline": int (0 | 1),
    "BatteryPresent": int (0 | 1),
    "Charging": int (0 | 1),
    "DefaultAlert1": int,
    "DefaultAlert2": int,
    "EstimatedTime": int,
    "MaximumCapacity": int,
    "RemainingCapacity": int
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT
* Mobilgerät

---
### Abrufen des aktiven Energieschemas

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das aktive Energieschema abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/activecfg
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Das aktive Energieschema hat das folgende Format.
```
{"ActivePowerScheme": string (guid of scheme)}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Abrufen des Unterwerts für ein Energieschema

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Unterwert für ein Energieschema abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/cfg/*<power scheme path>*
<br />
Optionen:
- SCHEME_CURRENT

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

Eine vollständige Liste der verfügbaren Energiezustände ist auf einzelne Anwendungen bezogen und enthält die Einstellungen zur Kennzeichnung verschiedener Energiezustände wie niedriger und kritischer Ladezustand. 

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Abrufen des Energiestatus des Systems

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Energiestatus des Systems überprüfen. So können Sie überprüfen, ob es sich im Energiesparmodus befindet.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/state
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Informationen zum Energiezustand haben die folgende Vorlage.
```
{"LowPowerStateAvailable": bool}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Festlegen des aktiven Energieschemas

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das aktive Energieschema festlegen.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/power/activecfg
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
scheme | (**erforderlich**) Die GUID des Schemas, das Sie als das aktive Energieschema für das System festlegen möchten.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Festlegen des Unterwerts für ein Energieschema

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Unterwert für ein Energieschema festlegen.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/power/cfg/*<power scheme path>*
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
valueAC | (**erforderlich**) Der für den Netzbetrieb zu verwendende Wert.
valueDC | (**erforderlich**) Der für den Akkubetrieb zu verwendende Wert.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Abrufen eines Berichts zur Ruhezustandsuntersuchung

**Anforderung**

Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/sleepstudy/report
<br />
Mit dem folgenden Anforderungsformat können Sie einen Bericht zur Ruhezustandsuntersuchung abrufen.

**URI-Parameter**
URI-Parameter | Beschreibung
:---          | :---
FileName | (**erforderlich**) Der vollständige Name für die Datei, die Sie herunterladen möchten. Dieser Wert sollte hex64-codiert sein.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort ist eine Datei mit der Ruhezustandsuntersuchung. 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Auflisten der verfügbaren Berichte zu Ruhezustandsuntersuchungen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die verfügbaren Berichte zu Ruhezustandsuntersuchungen auflisten
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/sleepstudy/reports
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Liste der verfügbaren Berichte hat die folgende Vorlage.

```
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Abrufen der Transformation der Ruhezustandsuntersuchung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Transformation der Ruhezustandsuntersuchung abrufen. Diese Transformation ist eine XSLT-Datei, die den Bericht zur Ruhezustandsuntersuchung in ein lesbares XML-Format konvertiert.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/sleepstudy/transform
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält die Transformation der Ruhezustandsuntersuchung.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
## Fernbedienung
---
### Neustarten des Zielcomputers

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Zielcomputer neu starten.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/control/restart
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
### Herunterfahren des Zielcomputers

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Zielcomputer herunterfahren.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/control/shutdown
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
## Task-Manager
---
### Starten einer Modern App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Modern App starten.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/taskmanager/app
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
appid   | (**erforderlich**) Die PRAID für die App, die gestartet werden soll. Dieser Wert sollte hex64-codiert sein.
package   | (**erforderlich**) Der vollständige Name für das App-Paket, das Sie starten möchten. Dieser Wert sollte hex64-codiert sein.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
### Beenden einer Modern App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Modern App beenden.
 
Methode      | Anforderungs-URI
:------     | :-----
DELETE | /api/taskmanager/app
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
package   | (**erforderlich**) Der vollständige Name des App-Pakets, das Sie beenden möchten. Dieser Wert sollte hex64-codiert sein.
forcestop   | (**optional**) Der Wert **yes** gibt an, dass das Beenden sämtlicher Prozesse erzwungen werden soll.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
## Netzwerk
---
### Abrufen der aktuellen IP-Konfiguration

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die aktuelle IP-Konfiguration abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/networking/ipconfig
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält die IP-Konfiguration in der folgenden Vorlage.

```
{"Adapters": [
    {
        "Description": string,
        "HardwareAddress": string,
        "Index": int,
        "Name": string,
        "Type": string,
        "DHCP": {
            "LeaseExpires": int, (timestamp)
            "LeaseObtained": int, (timestamp)
            "Address": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "WINS": {(WINS is optional)
            "Primary": {
                "IpAddress": string,
                "Mask": string
            },
            "Secondary": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "Gateways": [{ (always 1+)
            "IpAddress": "10.82.128.1",
            "Mask": "255.255.255.255"
            },...
        ],
        "IpAddresses": [{ (always 1+)
            "IpAddress": "10.82.128.148",
            "Mask": "255.255.255.0"
            },...
        ]
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

--
### Auflisten der Drahtlos-Netzwerkschnittstellen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Drahtlos-Netzwerkschnittstellen auflisten.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/wifi/interfaces
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Eine Liste der verfügbaren Drahtlosschnittstellen mit Details im folgenden Format.

``` 
{"Interfaces": [{
    "Description": string,
    "GUID": string (guid with curly brackets),
    "Index": int,
    "ProfilesList": [
        {
            "GroupPolicyProfile": bool,
            "Name": string, (Network currently connected to)
            "PerUserProfile": bool
        },...
    ]
    }
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
### Auflisten von Drahtlosnetzwerken

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Liste der Drahtlosnetzwerke an der angegebenen Schnittstelle auflisten.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/wifi/networks
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
interface   | (**erforderlich**) Die GUID für die Netzwerkschnittstelle, die zum Suchen nach Drahtlosnetzwerken verwendet werden soll, ohne Klammern. 
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die Liste der an der angegebenen *interface* gefundenen Drahtlosnetzwerke. Diese enthält Details für die Netzwerke im folgenden Format.

```
{"AvailableNetworks": [
    {
        "AlreadyConnected": bool,
        "AuthenticationAlgorithm": string, (WPA2, etc)
        "Channel": int,
        "CipherAlgorithm": string, (e.g. AES)
        "Connectable": int, (0 | 1)
        "InfrastructureType": string,
        "ProfileAvailable": bool,
        "ProfileName": string,
        "SSID": string,
        "SecurityEnabled": int, (0 | 1)
        "SignalQuality": int,
        "BSSID": [int,...],
        "PhysicalTypes": [string,...]
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
### Herstellen der Verbindung mit einem WLAN-Netzwerk und Trennen der Verbindung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Verbindung mit einem WLAN-Netzwerk herstellen oder die Verbindung trennen.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/wifi/network
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
interface   | (**erforderlich**) Die GUID für die Netzwerkschnittstelle, die zum Herstellen der Verbindung mit dem Netzwerk verwendet werden soll.
op   | (**erforderlich**) Gibt die durchzuführende Aktion an. Mögliche Werte sind „connect“ und „disconnect“.
ssid   | (**erforderlich, wenn *op* == connect**) Die SSID des Netzwerks, mit dem die Verbindung hergestellt werden soll.
Schlüssel   | (**erforderlich, wenn *op* == connect und Netzwerk erfordert Authentifizierung**) Die gemeinsam verwendete Schlüssel.
createprofile | (**erforderlich**) Erstellen Sie ein Profil für das Netzwerk auf dem Gerät.  Dadurch stellt das Gerät künftig automatisch eine Verbindung zum Netzwerk her. Dies kann **ja** oder **nein** sein. 

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
### Löschen eines WLAN-Profils

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie ein Profil löschen, das einem Netzwerk an einer bestimmten Schnittstelle zugeordnet ist.
 
Methode      | Anforderungs-URI
:------     | :-----
DELETE | /api/wifi/network
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
interface   | (**erforderlich**) Die GUID der Netzwerkschnittstelle, die dem zu löschenden Profil zugeordnet ist.
profile   | (**erforderlich**) Der Name des zu löschenden Profils.
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
## Windows-Fehlerberichterstattung (WER)
---
### Herunterladen einer WER-Datei (Windows Error Reporting)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WER-Datei herunterladen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/wer/report/file
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
user   | (**erforderlich**) Der dem Bericht zugeordnete Benutzername.
type   | (**erforderlich**) Der Typ des Berichts. Dieser kann **queried** oder **archived** lauten.
name   | (**erforderlich**) Der Name des Berichts. Dieser sollte base64-codiert sein. 
file   | (**erforderlich**) Der Name der Datei des Berichts, die heruntergeladen werden soll. Dieser sollte base64-codiert sein. 
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

- Antwort enthält die angeforderte Datei. 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Auflisten von Dateien in einem Bericht zur Windows-Fehlerberichterstattung (WER)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Dateien in einem WER-Bericht auflisten.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/wer/report/files
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
user   | (**erforderlich**) Der dem Bericht zugeordnete Benutzer.
type   | (**erforderlich**) Der Typ des Berichts. Dieser kann **queried** oder **archived** lauten.
name   | (**erforderlich**) Der Name des Berichts. Dieser sollte base64-codiert sein. 
<br />
**Anforderungsheader**

- Keiner

**Anforderungstext**

```
{"Files": [
    {
        "Name": string, (Filename, not base64 encoded)
        "Size": int (bytes)
    },...
]}
```

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Auflisten der Berichte zur Windows-Fehlerberichterstattung (WER)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die WER-Berichte abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/wer/reports
<br />

**URI-Parameter**

- Keiner

**Anforderungsheader**

- Keiner

**Anforderungstext**

- Keiner

**Antwort**

Die WER-Berichte in folgendem Format.

```
{"WerReports": [
    {
        "User": string,
        "Reports": [
            {
                "CreationTime": int,
                "Name": string, (not base64 encoded)
                "Type": string ("Queue" or "Archive")
            },
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
## Windows Performance Recorder (WPR) 
---
### Starten der Ablaufverfolgung mit einem benutzerdefinierten Profil

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie ein WPR-Profil hochladen und die Ablaufverfolgung mit diesem Profil starten.  Es kann immer nur eine Spur ausgeführt werden. Das Profil bleibt nicht auf dem Gerät. 
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/wpr/customtrace
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Ein multipart-konformer HTTP-Text, der das benutzerdefinierte WPR-Profil enthält.

**Antwort**

Der Status der WPR-Sitzung im folgenden Format.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Starten einer Startleistungs-Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WPR-Start-Ablaufverfolgungssitzung starten. Diese wird auch als Leistungs-Ablaufverfolgungssitzung bezeichnet.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/wpr/boottrace
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
profile   | (**erforderlich**) Dieser Parameter ist beim Starten erforderlich. Der Name des Profils, das eine Leistungs-Ablaufverfolgungssitzung starten soll. Die möglichen Profile werden in „perfprofiles/profiles.json“ gespeichert.
<br />
**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Beim Start gibt diese API den Status der WPR-Sitzung im folgenden Format zurück.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Beenden einer Startleistungs-Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WPR-Start-Ablaufverfolgungssitzung beenden. Diese wird auch als Leistungs-Ablaufverfolgungssitzung bezeichnet.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/wpr/boottrace
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Gibt die ETL-Ablaufverfolgungsdatei zurück.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Starten einer Leistungs-Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WPR-Ablaufverfolgungssitzung starten. Diese wird auch als Leistungs-Ablaufverfolgungssitzung bezeichnet.  Es kann immer nur eine Spur ausgeführt werden. 
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/wpr/trace
<br />

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
profile   | (**erforderlich**) Der Name des Profils, das eine Leistungs-Ablaufverfolgungssitzung starten soll. Die möglichen Profile werden in „perfprofiles/profiles.json“ gespeichert.
<br />
**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Beim Start gibt diese API den Status der WPR-Sitzung im folgenden Format zurück.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Beenden einer Leistungs-Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WPR-Ablaufverfolgungssitzung beenden. Diese wird auch als Leistungs-Ablaufverfolgungssitzung bezeichnet.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/wpr/trace
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Gibt die ETL-Ablaufverfolgungsdatei zurück.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Abrufen des Status einer Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Status der aktuellen WPR-Sitzung abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/wpr/status
<br />

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Der Status der WPR-Ablaufverfolgungssitzung im folgenden Format.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | OK
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT


<!--HONumber=May16_HO2-->


