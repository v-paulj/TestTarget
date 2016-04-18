---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Referenz zu Kern-APIs des Geräteportals
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
POST | /api/appx/packagemanager/package

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
package   | (**erforderlich**) Der Dateiname des zu installierenden Pakets.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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
GET | /api/appx/packagemanager/state

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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
DELETE | /api/appx/packagemanager/package


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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
GET | /api/appx/packagemanager/packages


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

Die Antwort enthält eine Liste der installierten Pakete mit zugehörigen Details.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält eine JSON-Struktur, die eine hierarchische Struktur der Geräte enthält.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält eine Liste der Absturzabbilder für jede quergeladene Anwendung.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.
fileName   | (**erforderlich**) Der Name der Absturzabbilddatei, die gelöscht werden soll.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.
fileName   | (**erforderlich**) Der Name der Absturzabbilddatei, die Sie herunterladen möchten.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält eine Absturzabbilddatei. Sie können die Absturzabbilddatei mit WinDbg oder Visual Studio untersuchen.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält eine Liste der Minidumpdateinamen und die Größen dieser Dateien.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
filename   | (**erforderlich**) Der Dateiname der Speicherabbilddatei. Sie finden diesen mithilfe der API zum Abrufen der Speicherabbildliste.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält die Speicherabbilddatei. Sie können diese Datei mithilfe von WinDbg untersuchen.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält die CrashControl-Einstellungen. Weitere Informationen zu CrashControl finden Sie im Artikel [](https://technet.microsoft.com/library/cc951703.aspx).

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält das vollständige Kernelmodus-Speicherabbild. Sie können diese Datei mithilfe von WinDbg untersuchen.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
pid   | (**erforderlich**) Die eindeutige Prozess-ID für den betreffenden Prozess.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält die Prozesssicherung. Sie können diese Datei mit WinDbg oder Visual Studio untersuchen.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
autoreboot   | (**optional**) True oder False. Dieser Parameter gibt an, ob das System nach einem Fehler oder Einfrieren automatisch neu gestartet wird.
dumptype   | (**optional**) Der Speicherabbildtyp. Die unterstützten Werte finden Sie unter [CrashDumpType-Enumeration](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).
maxdumpcount   | (**optional**) Die maximale Anzahl der zu speichernden Speicherabbilder.
overwrite   | (**optional**) True oder False. Dieser Parameter gibt an, ob alte Speicherabbilder überschrieben werden, wenn der durch *maxdumpcount* angegebene Höchstwert für die Anzahl von Speicherabbildern erreicht wurde.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
## ETW
---
### Erstellen einer Echtzeit-ETW-Sitzung über ein Websocket

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Echtzeit-ETW-Sitzung erstellen. Dies erfolgt über ein Websocket.
 
Methode      | Anforderungs-URI
:------     | :-----
GET/WebSocket | /api/etw/session/realtime


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält die ETW-Ereignisse von den aktivierten Anbietern.

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Auflisten der registrierten ETW-Anbieter

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die registrierten Anbieter auflisten.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/etw/providers


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält die Liste der ETW-Anbieter. Diese Liste enthält den Anzeigenamen und die GUID für jeden Anbieter.

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
## Netzwerke
---
### Abrufen der aktuellen IP-Konfiguration

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die aktuelle IP-Konfiguration abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/networking/ipconfig


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält die IP-Konfiguration.

**Statuscode**

Die folgende Tabelle enthält mögliche zusätzliche Statuscodes, die durch diesen Vorgang zurückgegeben werden können.

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Der Vorgang wurde erfolgreich abgeschlossen.
500 | Es ist ein interner Serverfehler aufgetreten.

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
## Betriebssysteminformationen
---
### Abrufen des Computernamens

**Anforderung**

Sie können mit dem folgenden Anforderungsformat den Namen eines Computers abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/os/machinename


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
name | (**erforderlich**) Der neue Name für den Computer.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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

Mit dem folgenden Anforderungsformat können Sie die Liste der derzeit ausgeführten Prozesse abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/resourcemanager/processes


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält eine Liste der Prozesse mit Details für jeden Prozess. Die Informationen weisen das JSON-Format auf.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Antwort enthält die Leistungsstatistik für das System, z. B. CPU- und GPU-Nutzung, Speicherzugriff und Netzwerkzugriff. Diese Informationen weisen das JSON-Format auf.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Abrufen des aktiven Energieschemas

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das aktive Energieschema abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/activecfg


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
### Abrufen eines Berichts zur Ruhezustandsuntersuchung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie einen Bericht zur Ruhezustandsuntersuchung abrufen.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/sleepstudy/reports


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
FileName | (**erforderlich**) Der Dateiname des Berichts zur Ruhezustandsuntersuchung, den Sie herunterladen möchten.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Festlegen des aktiven Energieschemas

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das aktive Energieschema festlegen.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/power/activecfg


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
scheme | (**erforderlich**) Die GUID des Schemas, das Sie als das aktive Energieschema für das System festlegen möchten.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
valueAC | (**erforderlich**) Der für den Netzbetrieb zu verwendende Wert.
valueDC | (**erforderlich**) Der für den Akkubetrieb zu verwendende Wert.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows-Desktop
* IoT

---
### Abrufen der Transformation der Ruhezustandsuntersuchung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Transformation der Ruhezustandsuntersuchung abrufen. Diese Transformation ist eine XSLT-Datei, die den Bericht zur Ruhezustandsuntersuchung in ein lesbares XML-Format konvertiert.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/power/sleepstudy/reports


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
appid   | (**erforderlich**) Die PRAID für die App, die gestartet werden soll. Dieser Wert sollte hex64-codiert sein.
package   | (**erforderlich**) Der vollständige Name für das App-Paket, das Sie starten möchten. Dieser Wert sollte hex64-codiert sein.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
package   | (**erforderlich**) Der vollständige Name des App-Pakets, das Sie beenden möchten. Dieser Wert sollte hex64-codiert sein.
forcestop   | (**optional**) Der Wert **yes** gibt an, dass das Beenden sämtlicher Prozesse erzwungen werden soll.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* Xbox
* HoloLens
* IoT

---
## WLAN
---
### Auflisten der Drahtlos-Netzwerkschnittstellen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Drahtlos-Netzwerkschnittstellen auflisten.
 
Methode      | Anforderungs-URI
:------     | :-----
GET | /api/wifi/interfaces


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Eine Liste der verfügbaren Drahtlosschnittstellen mit Details. Die Details umfassen z. B. GUID, Beschreibung, Anzeigename usw.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
interface   | (**erforderlich**) Die GUID für die Netzwerkschnittstelle, die zum Suchen nach Drahtlosnetzwerken verwendet werden soll.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Die Liste der an der angegebenen *interface* gefundenen Drahtlosnetzwerke. Diese enthält Details zu den Netzwerken.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
interface   | (**erforderlich**) Die GUID für die Netzwerkschnittstelle, die zum Herstellen der Verbindung mit dem Netzwerk verwendet werden soll.
op   | (**erforderlich**) Gibt die durchzuführende Aktion an. Mögliche Werte sind „connect“ und „disconnect“.
ssid   | (**erforderlich, wenn *op* == connect**) Die SSID des Netzwerks, mit dem die Verbindung hergestellt werden soll.
key   | (**erforderlich, wenn *op* == connect**) Der gemeinsam verwendete Schlüssel.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
interface   | (**erforderlich**) Die GUID der Netzwerkschnittstelle, die dem zu löschenden Profil zugeordnet ist.
profile   | (**erforderlich**) Der Name des zu löschenden Profils.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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
GET | /api/wer/reports/file


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
user   | (**erforderlich**) Der dem Bericht zugeordnete Benutzername.
type   | (**erforderlich**) Der Typ des Berichts. Dieser kann **queried** oder **archived** lauten.
name   | (**erforderlich**) Der Name des Berichts.
file   | (**erforderlich**) Der Name der Datei des Berichts, die heruntergeladen werden soll.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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
GET | /api/wer/reports/files


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
user   | (**erforderlich**) Der dem Bericht zugeordnete Benutzer.
type   | (**erforderlich**) Der Typ des Berichts. Dieser kann **queried** oder **archived** lauten.
name   | (**erforderlich**) Der Name des Berichts.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows-Desktop
* HoloLens
* IoT

---
## Windows Performance Recorder (WPR) 
---
### Starten der Ablaufverfolgung mit einem benutzerdefinierten Profil

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie ein WPR-Profil hochladen und die Ablaufverfolgung mit diesem Profil starten.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/wpr/customtrace


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Ein multipart-konformer HTTP-Text, der das benutzerdefinierte WPR-Profil enthält.

**Antwort**

- Gibt den WPR-Sitzungsstatus zurück.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
profile   | (**erforderlich**) Dieser Parameter ist beim Starten erforderlich. Der Name des Profils, das eine Leistungs-Ablaufverfolgungssitzung starten soll. Die möglichen Profile werden in „perfprofiles/profiles.json“ gespeichert.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Diese API gibt beim Starten den WPR-Sitzungsstatus zurück.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Gibt die ETL-Ablaufverfolgungsdatei zurück.

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT

---
### Starten einer Leistungs-Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WPR-Ablaufverfolgungssitzung starten. Diese wird auch als Leistungs-Ablaufverfolgungssitzung bezeichnet.
 
Methode      | Anforderungs-URI
:------     | :-----
POST | /api/wpr/trace


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter | Beschreibung
:---          | :---
profile   | (**erforderlich**) Der Name des Profils, das eine Leistungs-Ablaufverfolgungssitzung starten soll. Die möglichen Profile werden in „perfprofiles/profiles.json“ gespeichert.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Diese API gibt beim Starten den WPR-Sitzungsstatus zurück.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Gibt die ETL-Ablaufverfolgungsdatei zurück.

**Statuscode**

- Standardstatuscodes

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


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner

**Antwort**

- Der Status der WPR-Ablaufverfolgungssitzung.

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows-Desktop
* HoloLens
* IoT


<!--HONumber=Mar16_HO5-->


