---
author: mcleblanc
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: "Geräteportal für HoloLens"
description: "Hier erfahren Sie, wie Sie mit dem Windows Device Portal für HoloLens Ihr HoloLens-Gerät per Fernzugriff konfigurieren und verwalten können."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: bd1ae8ccdd81f319fc36ca04b8b839cb313b2854

---
# Geräteportal für HoloLens


## Einrichten des Geräteportals für HoloLens

### Aktivieren des Geräteportals

**So aktivieren Sie das Geräteportal**

1. Schalten Sie die HoloLens ein, und setzen Sie sie auf.
2. Führen Sie die [Blütengeste](https://dev.windows.com/holographic/Gestures.html#Bloom) aus, um das Hauptmenü zu starten.
3. Betrachten Sie die Kachel **Einstellungen**, und führen Sie die [Zeigefingergeste](https://dev.windows.com/holographic/Gestures.html#Press_and_release) aus. Führen Sie die Zeigefingergeste ein zweites Mal aus, um die App in Ihrer Umgebung zu platzieren. Die Einstellungs-App wird gestartet, nachdem Sie sie platziert haben.
4. Wählen Sie das Menüelement **Aktualisieren** aus.
5. Wählen Sie das Menüelement **Für Entwickler** aus.
6. Aktivieren Sie den **Entwicklermodus**.
7. [Führen Sie einen Bildlauf nach unten durch](https://dev.windows.com/holographic/Gestures.html#Navigation), und aktivieren Sie Device Portal.


### Koppeln des Geräts

### Herstellen einer WLAN-Verbindung 

1. Verbinden Sie die HoloLens mit dem WLAN.
2. Suchen Sie die IP-Adresse des Geräts. – Suchen Sie die IP-Adresse auf dem Gerät unter „Einstellungen“ > „Netzwerk und Internet“ > „WLAN“ > „Erweiterte Optionen“.
    Sie können auch fragen „Hey Cortana, wie lautet meine IP-Adresse?“

3. Rufen Sie in einem Webbrowser auf dem PC „“ auf. `https://<YOUR_HOLOLENS_IP_ADDRESS>`
    - Im Browser wird die folgende Meldung angezeigt: „Es besteht ein Problem mit dem Sicherheitszertifikat der Website.“ Der Grund dafür ist, dass das für das Geräteportal ausgestellte Zertifikat ein Testzertifikat ist. Sie können diesen Zertifikatfehler vorerst ignorieren und fortfahren.

### Herstellen einer Verbindung über USB 

1. Installieren Sie die Tools, um sicherzustellen, dass auf dem PC Visual Studio Update 1 mit den Windows 10-Entwicklertools installiert ist. Hierdurch wird USB-Konnektivität aktiviert.
2. Schließen Sie die HoloLens mit einem Micro-USB-Kabel am PC an.
3. Rufen Sie in einem Webbrowser auf dem PC „`http://127.0.0.1:10080`“ auf.

### Herstellen einer Verbindung mit einem Emulator 

Sie können das Geräteportal auch mit dem Emulator verwenden. Verwenden Sie die Symbolleiste, um die Verbindung mit dem Geräteportal herzustellen. Klicken Sie auf dieses Symbol:
- Öffnen Sie das Geräteportal: Öffnen Sie das Windows Device Portal für das HoloLens-Betriebssystem im Emulator.

### Erstellen eines Benutzernamens und Kennworts 

Wenn Sie das erste Mal eine Verbindung der HoloLens mit dem Geräteportal herstellen, müssen Sie einen Benutzernamen und ein Kennwort erstellen.
1. Geben Sie in einem Webbrowser auf dem PC die IP-Adresse der HoloLens ein. Die Seite „Set up access“ wird geöffnet.
2. Klicken oder tippen Sie auf „Request pin“, und betrachten Sie die HoloLens-Anzeige, um die generierte PIN abzurufen.
3. Geben Sie im Textfeld „PIN displayed on your device“ die PIN ein.
4. Geben Sie den Benutzernamen ein, den Sie zum Herstellen der Verbindung mit dem Geräteportal verwenden. Dabei muss es sich nicht um den Namen eines Microsoft-Kontos (MSA) oder einen Domänennamen handeln.
5. Geben Sie ein Kennwort ein, und bestätigen Sie es. Das Kennwort muss mindestens sieben Zeichen lang sein. Es muss kein MSA- oder Domänenkennwort sein.
6. Klicken Sie auf „Koppeln“, um auf der HoloLens eine Verbindung mit dem Windows Device Portal herzustellen.

Wenn Sie den Benutzernamen oder das Kennwort ändern möchten, können Sie diesen Vorgang jederzeit wiederholen, indem Sie die Seite für Gerätesicherheit besuchen. Klicken Sie hierzu oben rechts auf den Link „Sicherheit“, oder navigieren Sie zu „`https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`“.

### Sicherheitszertifikat 

Wenn im Browser eine Meldung zu einem Zertifikatfehler angezeigt wird, können Sie diesen beheben, indem Sie eine Vertrauensstellung mit dem Gerät erstellen.

Jede HoloLens generiert ein eindeutiges selbstsigniertes Zertifikat für die SSL-Verbindung. Standardmäßig wird dieses Zertifikat vom Webbrowser des PC nicht als vertrauenswürdig angesehen, und Sie erhalten möglicherweise eine Meldung zu einem Zertifikatfehler. Sie können dieses Zertifikat von der HoloLens herunterladen (über USB oder ein vertrauenswürdiges WLAN-Netzwerk) und es auf dem PC als vertrauenswürdig einstufen, um eine sichere Verbindung mit dem Gerät herzustellen.
1. Vergewissern Sie sich, dass Sie sich in einem sicheren Netzwerk (USB-Verbindung oder vertrauenswürdiges WLAN-Netzwerk) befinden.
2. Laden Sie das Zertifikat des Geräts von der Seite „Sicherheit“ im Geräteportal herunter. Klicken Sie in der rechten oberen Liste der Symbole auf den Link „Sicherheit“, oder navigieren Sie zu „“. `https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`

3. Installieren Sie das Zertifikat im Speicher „Vertrauenswürdige Stammzertifizierungsstellen“ auf dem PC.- Geben Sie im Windows-Menü „Computerzertifikate verwalten“ ein, und starten Sie das Applet.
    - Erweitern Sie den Ordner „Vertrauenswürdige Stammzertifizierungsstellen“.
    - Klicken Sie auf den Ordner „Zertifikate“.
    - Wählen Sie im Menü „Aktion“ die Aktion „Alle Aufgaben“ > „Importieren“ aus.
    - Führen Sie den Zertifikatimport-Assistenten mit der Zertifikatdatei aus, die Sie vom Geräteportal heruntergeladen haben.

4. Starten Sie den Browser neu.


## Seiten des Geräteportals 

### Startseite 

Die Geräteportalsitzung beginnt auf der Startseite. Der Zugriff auf andere Seiten erfolgt über die Navigationsleiste links von der Startseite.

Die Symbolleiste am oberen Rand der Seite ermöglicht den Zugriff auf häufig verwendete Status und Features.
- **Online**: Gibt an, ob das Gerät mit dem WLAN verbunden ist.
- **Herunterfahren**: Schaltet das Gerät aus.
- **Neu starten**: Schaltet das Gerät aus und wieder ein.
- **Sicherheit**: Öffnet die Seite „Device Security“.
- **Cool**: Gibt die Temperatur des Geräts an.
- **A/C**: Gibt an, ob das Gerät angeschlossen ist und geladen wird.
- **Hilfe**: Öffnet die Seite mit der Dokumentation der REST-Schnittstelle.

Auf der Startseite werden die folgenden Informationen angezeigt:
- **Gerätestatus**: Überwacht die Integrität des Geräts und meldet schwerwiegende Fehler.
- **Windows-Informationen**: Zeigt den Namen der HoloLens und die derzeit installierte Version von Windows an.
- **Einstellungen**: Dieser Abschnitt enthält die folgenden Einstellungen:
    - **IPD**: Legt den Pupillenabstand (Interpupillary Distance, IPD) fest. Dies ist der Abstand in Millimeter zwischen dem Mittelpunkt der Pupillen des Benutzers, wenn dieser geradeaus schaut. Die Einstellung wird sofort wirksam. Der Standardwert wurde beim Einrichten des Geräts automatisch berechnet.
    - **Gerätename**: Weisen Sie der HoloLens einen Namen zu. Nach dem Ändern dieses Werts müssen Sie das Gerät neu starten, damit er wirksam wird. Nach dem Klicken auf „Speichern“ wird ein Dialogfeld mit der Frage angezeigt, ob Sie das Gerät sofort oder später neu starten möchten.
    - **Standbymoduseinstellungen**: Hier legen Sie die Wartezeit fest, bevor das Gerät in den Ruhezustand wechselt, wenn es angeschlossen ist und wenn es mit Akkustrom betrieben wird.

### 3D View 

Auf der Seite „3D View“ können Sie erkennen, wie die HoloLens Ihre Umgebung interpretiert. Navigieren Sie mit der Maus in der Ansicht:
- **Drehen**: Linksklick + Bewegen der Maus
- **Schwenken**: Rechtsklick + Bewegen der Maus
- **Zoomen**: Drehen des Mausrads
- **Tracking options**: Wenn Sie „Force visual tracking“ markieren, aktivieren Sie die fortlaufende visuelle Nachverfolgung. Mit „Anhalten“ wird die visuelle Nachverfolgung angehalten.
- **Ansichtsoptionen**: Legen Sie die Optionen für die 3D-Ansicht fest: „Tracking“ gibt an, ob die visuelle Nachverfolgung aktiviert ist.
- **Show floor**: Zeigt eine schachbrettartige Bodenfläche an.
- **Show frustum**: Zeigt das Ansichts-Frustum an.
- **Show stabilization plane**: Zeigt die Ebene an, die von der HoloLens für die Bewegungsstabilisierung verwendet wird.
- **Show mesh**: Zeigt das Surface-Mapping-Gitter an, das die Umgebung darstellt.
- **Details anzeigen**: Zeigt die Änderung von Handpositionen, der Kopfdrehungsquaternionen und des Geräteursprungsvektors in Echtzeit an.
- **Vollbildschaltfläche**: Mit dieser Schaltfläche wird die Seite „3D View“ im Vollbildmodus angezeigt. Drücken Sie die ESC-Taste, um die Vollbildansicht zu beenden.

- Oberflächenrekonstruktion: Klicken oder tippen Sie auf „Aktualisieren“, um das aktuelle Spatial-Mapping-Gitter vom Gerät anzuzeigen. Ein vollständiger Durchlauf kann bis zu einige Sekunden lang dauern. Das Gitter wird in der 3D-Ansicht nicht automatisch aktualisiert. Sie müssen auf „Update“ klicken, um das aktuelle Gitter vom Gerät abzurufen. Klicken Sie auf „Speichern“ um das aktuelle Spatial-Mapping-Gitter als OBJ-Datei auf dem PC zu speichern.

### Mixed Reality Capture 

Auf der Seite „Mixed Reality Capture“ können Sie Mediendatenströme von der HoloLens speichern.
- Einstellungen: Steuern Sie die erfassten Mediendatenströme durch Aktivieren der folgenden Einstellungen: „Holograms“ erfasst den holografischen Anteil des Videodatenstroms. Hologramme werden in Mono und nicht in Stereo gerendert.
- **PV camera**: Erfasst den Videodatenstrom der Foto-/Videokamera.
- **Mic Audio**: Erfasst Audioaufnahmen vom Mikrofonarray.
- **App Audio**: Erfasst Audioaufnahmen von der derzeit ausgeführten App.
- **Live preview quality**: Wählen Sie die Bildschirmauflösung, Bildfrequenz und Streamingrate für die Live-Vorschau aus.

- Klicken oder tippen Sie auf die Schaltfläche „Live preview“, um den Aufnahmedatenstrom anzuzeigen. Mit „Stop live preview“ wird der Aufnahmedatenstrom beendet.
- Klicken oder tippen Sie auf „Aufzeichnen“, um die Aufzeichnung des Mixed-Reality-Datenstroms mit den angegebenen Einstellungen zu starten. Mit „Aufnahme beenden“ wird die Aufzeichnung beendet und gespeichert.
- Klicken oder tippen Sie auf „Take photo“, um ein Standbild des Aufnahmedatenstroms zu erstellen.
- **Videos and photos**: Zeigt eine Liste der auf dem Gerät aufgenommenen Videos und Fotos an.

Beachten Sie, dass HoloLens-Apps kein MRC-Foto oder -Video aufnehmen können, während Sie über das Geräteportal eine Live-Vorschau aufzeichnen oder streamen.

### Systemleistung 

Das Tool „Systemleistung“ der HoloLens bietet drei zusätzliche Metriken, die aufgezeichnet werden können. 

Die folgenden Metriken sind verfügbar:
- **SoC Power**: Sofortige Nutzung des System-on-a-Chip-Stroms, gemittelt über eine Minute.
- **Systemstromversorgung**: Sofortige Nutzung des Systemstroms, gemittelt über eine Minute.
- **Framerate**: Bilder pro Sekunde, übersprungene VBlanks pro Sekunde und aufeinanderfolgende übersprungene VBlanks

### Seite „App Crash Dumps“ 

Auf dieser Seite können Sie Absturzabbilder für die quergeladenen Apps erfassen. Aktivieren Sie das Kontrollkästchen „Crash Dumps Enabled“ für jede App, für die Sie Absturzabbilder erfassen möchten. Kehren Sie zum Erfassen von Absturzabbildern zu dieser Seite zurück. Speicherabbilddateien können zum Debuggen in Visual Studio geöffnet werden.

### Kioskmodus 

Aktiviert den Kioskmodus, in dem die Möglichkeiten des Benutzers zum Starten neuer Apps oder Ändern der derzeit ausgeführten App beschränkt sind. Wenn der Kioskmodus aktiviert ist, sind die Blütengeste und Cortana deaktiviert, und platzierte Apps werden in der Umgebung des Benutzers nicht angezeigt.

Markieren Sie „Enable Kiosk Mode“, um die HoloLens in den Kioskmodus zu versetzen. Wählen Sie in der Dropdownliste „Startup app“ die App aus, die beim Starten ausgeführt werden soll. Klicken oder tippen Sie auf „Speichern“, um die Einstellungen zu übernehmen.

Beachten Sie, dass die App auch dann beim Starten ausgeführt wird, wenn der Kioskmodus nicht aktiviert ist. Wählen Sie „Keine“ aus, wenn beim Starten keine App ausgeführt werden soll.

### Simulation 

Ermöglicht Ihnen das Aufzeichnen und Wiedergeben von Eingabedaten für Testzwecke.
- **Capture room**: Wird verwendet, um eine Datei für einen simulierten Raum herunterzuladen, die das Spatial-Mapping-Gitter für die Umgebung des Benutzers enthält. Benennen Sie den Raum, und klicken Sie auf „Aufnahme“, um die Daten als XEF-Datei auf dem PC zu speichern. Diese Raumdatei kann in den HoloLens-Emulator geladen werden.
- **Aufzeichnung**: Markieren Sie die aufzuzeichnenden Datenströme, benennen Sie die Aufzeichnung, und klicken oder tippen Sie auf „Aufzeichnen“, um die Aufzeichnung zu starten. Führen Sie mit der HoloLens Aktionen aus, und klicken Sie dann auf „Beenden“, um die Daten als XEF-Datei auf dem PC zu speichern. Diese Datei kann im HoloLens-Emulator oder auf dem Gerät geladen werden.
- **Wiedergabe**: Klicken oder tippen Sie auf „Upload recording“, um auf dem PC eine XEF-Datei auszuwählen und die Daten an die HoloLens zu senden.
- **Steuerungsmodus**: Wählen Sie in der Dropdownliste „Standard“ oder „Simulation“ aus, und klicken oder tippen Sie auf die Schaltfläche „Festlegen“, um den Modus der HoloLens auszuwählen. Durch Auswahl von „Simulation“ werden die realen Sensoren auf der HoloLens deaktiviert und stattdessen hochgeladene simulierte Daten verwendet. Wenn Sie zu „Simulation“ wechseln, reagiert die HoloLens nicht auf den realen Benutzer, bis Sie zurück zu „Standard“ wechseln.


### Virtual Input 

Sendet die Tastatureingabe vom Remotecomputer an die HoloLens.

Klicken oder tippen Sie auf den Bereich unter „Virtual keyboard“, um das Senden von Tastatureingaben an die HoloLens zu aktivieren. Geben Sie im Textfeld „Eingabetext“ Text ein, und klicken oder tippen Sie auf „Senden“, um die Tastatureingaben an die aktive App zu senden.



<!--HONumber=Jun16_HO4-->


