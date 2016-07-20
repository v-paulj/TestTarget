---
author: awkoren
Description: "Führen Sie die Desktop-Konverter-App zum Konvertieren einer Windows-Desktopanwendung (z. B. Win32, WPF und Windows Forms) in eine UWP-App (Universelle Windows-Plattform)."
Search.Product: eADQiWindows 10XVcnh
title: "Vorschau für den Desktop-App-Konverter (Project Centennial)"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 8a22285467005722ad6ee5bf4f129a7dfdea944c

---

# Vorschau für den Desktop-App-Konverter (Project Centennial)

\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

[Holen Sie sich den Desktop-App-Konverter.](http://go.microsoft.com/fwlink/?LinkId=785437)

Desktop-App-Konverter ist die Vorabversion eines Tools, mit dem Sie vorhandene für.NET 4.6.1 oder Win32 geschriebene Desktop-Apps für die Universelle Windows-Plattform (UWP) konvertieren können. Sie können Ihre Desktop-Installer über den Konverter im unbeaufsichtigten Modus (automatisch) ausführen und ein AppX-Paket abrufen, das Sie mithilfe des Add-AppxPackage-PowerShell-Cmdlets auf Ihrem Entwicklungscomputer installieren können.

Der Konverter führt den Desktop-Installer in einer isolierten Windows-Umgebung mit einem sauberen Basisimage als Teil des Konverterdownloads aus. Es werden alle Registrierungs- und Dateisystem-E/A des Desktop-Installers erfasst und als Teil der Ausgabe gepackt. Der Konverter gibt ein AppX-Paket mit Paketidentität und der Möglichkeit aus, eine breite Palette von WinRT-APIs aufzurufen.

## Das ist neu:

In diesem Abschnitt werden Änderungen zwischen Versionen des Desktop-App-Konverters behandelt. 

### 07.07.2016 (v0.1.22)

* Unterstützung für die automatische Erkennung von Shellerweiterungen in Ihrer Desktopanwendung und für die Deklaration der Erweiterungen in der APPXMANIFEST-Datei Ihres UWP-Pakets wurde hinzugefügt. Weitere Informationen zu den Desktoperweiterungen finden Sie unter [**Erweiterungen für konvertierte Desktop-Apps**](desktop-to-uwp-extensions.md). 
* Die AppExecutable-Erkennung bei einer großen Menge von Apps wurde verbessert. 

### 16.06.2016 (v0.1.20)

* Probleme beim Konvertieren für die neuesten Windows10 Insider Preview-Builds wurden behoben. 
* ```–CreateX86Package``` wurde durch ```–PackageArch``` ersetzt. Dies ermöglicht das Angeben der Architektur für das generierte Paket. 

### 08.06.2016

* Unterstützung für die Generierung von x86-appx-Paketen auf AMD64-Hostrechnern hinzugefügt, auf denen der Konverter ausgeführt wird.
* Speicherplatzverwendung durch Entfernen aller zuvor erweiterter Basisimages reduziert.
* Unterstützung für das Bereinigen temporärer Dateien und aller unnötigen Basisimages hinzugefügt.
* Unterstützung für das Erkennen von Dateityp- und Protokollzuordnungen verbessert.
* Logik zum Ermitteln der AppExecutable-Eigenschaft für eine große Anzahl von Apps verbessert.
* Unterstützung für die Bereitstellung zusätzlicher –InstallerArguments für MSI-basierte Installationsprogramme hinzugefügt.
* Fehlerkorrekturen für alle PathTooLongException-Fehler während der Konvertierung.

### 12.05.2016

- Unterstützung für Pro-Edition von Windows wiederhergestellt. 
- Konverter-```-Setup```-Markierung aktiviert nun Windows-Container-Feature und verarbeitet Basisimage-Erweiterung. Führen Sie Folgendes an einer PowerShell-Eingabeaufforderung mit erhöhten Rechten für die einmalige Einrichtung aus: ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- Automatische Erkennung des App-Installationspfads hinzugefügt und Anwendungsstamm außerhalb von VFS verschoben, um alle nicht benötigten Dateisystemumleitungen zur Laufzeit zu verringern.
- Automatische Erkennung des erweiterten Basisimage als Teil des Konvertierungsprozesses hinzugefügt.
- Automatische Erkennung für Dateitypzuordnungen und Protokolle hinzugefügt.
- Logik zum Erkennen der Startmenüverknüpfung verbessert.
- Dateisystemfilter zum Beibehalten der von der App installierten MUI-Dateien verbessert.
- Die unterstützte Desktop-Mindestversion (10.0.14342.0) für Project Centennial im Manifest aktualisiert.

## Systemanforderungen

### Unterstütztes Betriebssystem
+ Windows 10 Anniversary Update Enterprise Edition Preview (Build 10.0.14342.0 und höher)

### Erforderliche Hardwarekonfiguration

Der Computer muss die folgenden Mindestfunktionen aufweisen:
+ 64-Bit (x64)-Prozessor
+ Hardwareunterstützte Virtualisierung
+ Adressübersetzung der zweiten Ebene (Second Level Address Translation, SLAT)

### Empfohlene Ressourcen
+ [Windows Software Development Kit (SDK) für Windows10](http://go.microsoft.com/fwlink/?LinkId=615097)

## Einrichten des Desktop-App-Konverters   
Der Desktop-App-Konverter basiert auf Windows 10-Features mit Test-Flight als Bestandteil des Windows-Insider Preview-Builds. Stellen Sie sicher, dass Sie den aktuellen Build für die Verwendung des Konverters nutzen.

1. Stellen Sie sicher, dass Sie das neueste Windows 10 Insider Preview-Betriebssystem – Enterprise- oder Pro-Edition (http://insider.windows.com) – haben. 
2. Laden Sie die DesktopAppConverter.zip und die WIM-Basisimage-Datei herunter, die Ihrem Insider Preview-Build (http://aka.ms/converter) entspricht. 
3. Extrahieren Sie die Datei „DesktopAppConverter.zip“ in einen lokalen Ordner.
4. Führen Sie in einem PowerShell-Administratorfenster den folgenden Befehl aus:  
```CMD
PS C:\> Set-ExecutionPolicy bypass
```
5. Führen Sie den folgenden Befehl in einem PowerShell- Administratorfenster zum Einrichten des Konverters aus:
```CMD
PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose
```
6. Wenn Sie beim Ausführen des vorherigen Befehls zum Neustarten aufgefordert werden, starten Sie den Computer neu, und führen Sie den Befehl erneut aus.

## Ausführen des Desktop-App-Konverters
Der Desktop-App-Konverter verfügt über zwei Einstiegspunkte: PowerShell und Befehlsshell. Sie können diese Einstiegspunkte zum Starten des Konvertierungsprozesses verwenden.

### Verwendung
```CMD
DesktopAppConverter.ps1
-Installer <String> [-InstallerArguments <String>] [-InstallerValidExitCodes <Int32>]
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]  
```

### Beispiel
Das folgende Beispiel veranschaulicht, wie eine Desktop-App mit dem Namen *MyApp* von *&lt;Herausgebername&gt;* in ein UWP-Paket (AppX) konvertiert wird.

+ Führen Sie in einem PowerShell-Administratorfenster den folgenden Befehl aus:
```CMD
PS C:\>.\DesktopAppConverter.ps1 -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## Bereitstellen des konvertierten AppX-Pakets
Verwenden Sie das [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx)-Cmdlet in PowerShell zum Bereitstellen eines signierten App-Pakets (.appx) für ein Benutzerkonto. Informationen zum Signieren Ihrer Appx-Pakets finden Sie im Abschnitt „Signieren des Appx-Pakets“. Sie können auch den *Register*-Parameter des Cmdlets hinzufügen, um eine Installation von einem Ordner mit nicht gepackten Dateien während des Entwicklungsprozesses vorzunehmen. Weitere Informationen finden Sie unter [Bereitstellen und Debuggen der konvertierten UWP-App](desktop-to-uwp-deploy-and-debug.md).

## Signieren des Appx-Pakets

Das Add-AppxPackage-Cmdlet erfordert, dass das bereitgestellte Anwendungspaket (.appx) signiert ist. Verwenden Sie zum Signieren des Appx-Pakets SignTool.exe, die im Lieferumfang des Microsoft Windows 10 SDK enthalten ist.

### Beispiel
```CMD
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

              **Hinweis:** Wenn Sie beim Ausführen von „MakeCert.exe“ zum Eingeben eines Kennworts aufgefordert werden, wählen Sie **Kein**.

Weitere Informationen zu Zertifikaten und zum Signieren finden Sie unter:

+ [Gewusst wie: Erstellen temporärer Zertifikate für die Verwendung während der Entwicklung](https://msdn.microsoft.com/library/ms733813.aspx)
+ [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
+ [SignTool.exe (Signaturtool)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)

### Einschränkungen
1. Der Windows 10-Build auf dem Hostcomputer muss mit dem Basisimage übereinstimmen, das Sie im Rahmen des Desktop-App-Konverter-Downloads erhalten haben.  
2. Stellen Sie sicher, dass sich der Desktop-Installer in einem unabhängigen Verzeichnis befindet, da der Konverter alle Verzeichnisinhalte in eine isolierte Windows-Umgebung kopiert.  
3. Der Desktop-App-Konverter unterstützt den Konvertierungsprozess derzeit nur unter 64-Bit-Versionen des Betriebssystems. Sie können die konvertierten Appx-Pakete nur für 64-Bit-Versionen (x64) des Betriebssystems bereitstellen.  
4. Für den Desktop-App-Konverter muss der Desktop-Installer im unbeaufsichtigten Modus ausgeführt werden. Stellen Sie sicher, dass Sie das automatische Kennzeichen an den Installer für den Konverter mithilfe des *-InstallerArguments*-Parameters übergeben.
5. Die Veröffentlichung von öffentlichen SxS Fusion-Assemblys ist nicht möglich. Während der Installation kann eine Anwendung öffentliche parallele Fusion-Assemblys veröffentlichen, auf die von jedem anderen Prozess zugegriffen werden kann. Während der Erstellung des Prozessaktivierungskontexts werden diese Assemblys von einem Systemprozess mit dem Namen CSRSS.exe abgerufen. Wenn dies für einen Centennial-Prozess abgeschlossen ist, tritt beim Erstellen des Aktivierungskontexts und Laden des Moduls dieser Assemblys ein Fehler auf. Integrierte Assemblys, z. B. ComCtl, sind im Lieferumfang des Betriebssystems enthalten, sodass von diesen abhängige Centennial-Prozesse sicher sind. Die SxS Fusion-Assemblys werden an folgenden Orten registriert:
  + Registrierung: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Dateisystem: %windir%\\SideBySide

## Bekannte Probleme

+ Wenn Sie einen Windows-Insider-Flight auf einem Entwicklercomputer erhalten, auf dem zuvor die Desktop App Konverter Preview installiert war, erhalten Sie möglicherweise den Fehler `New-ContainerNetwork: The object already exists`, wenn Sie das neue Basisimage einrichten. Um dieses Problem zu umgehen, führen Sie den Befehl `Netsh int ipv4 reset` an einer Eingabeaufforderung mit erhöhten Rechten aus, und starten Sie den Computer dann neu. 
+ Die Installation einer mit der Buildoption „AnyCPU“ kompilierten .NET-App schlägt fehl, wenn die ausführbare Hauptdatei oder eine Abhängigkeit unter „Programme“ oder „Windows\System32“ abgelegt wurden. Um dieses Problem zu umgehen, verwenden Sie das Desktop-Installationsprogramm für Ihre Architektur (32-Bit oder 64-Bit), um ein AppX-Paket erfolgreich zu generieren.

## Telemetriedaten aus Desktop-App-Konverter  
Desktop-App-Konverter erfasst ggf. Informationen über Sie und die Verwendung der Software, und sendet diese Informationen an Microsoft. Weitere Informationen zur Sammlung von Daten und deren Verwendung von Microsoft finden Sie in der Produktdokumentation und in den [Datenschutzbestimmungen von Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Sie stimmen der Einhaltung aller geltenden Vorschriften der Datenschutzbestimmungen von Microsoft zu.

Standardmäßig ist Telemetrie für den Desktop-App-Konverter aktiviert. Fügen Sie den folgenden Registrierungsschlüssel hinzu, um Telemetrie entsprechend zu konfigurieren:  
```CMD
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Fügen Sie den *DisableTelemetry*-Wert hinzu, oder bearbeiten Sie diesen, indem Sie DWORD auf 1 festlegen.
+ Entfernen Sie zum Aktivieren von Telemetrie den Schlüssel, oder legen Sie den Wert auf 0 fest.

## Nutzung des Desktop-App-Konverters
Im Folgenden finden Sie eine Liste der Parameter für den Desktop-App-Konverter. Sie können diese Liste auch im Windows Powershell-Fenster anzeigen, indem Sie den folgenden Befehl ausführen:  
```CMD
get-help .\DesktopAppConverter.ps1 -detailed
```

### Setupparameter  
|Parameter|Beschreibung|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | Führt DesktopAppConverter im Setupmodus aus. Setupmodus unterstützt die Erweiterung eines bereitgestellten Basisimages.|
|```-BaseImage <String>``` | Vollständiger Pfad zu einem nicht erweiterten Basisimage. Dieser Parameter ist erforderlich, wenn -Setup angegeben wurde.|
|```-LogFile <String>``` [optional] | Gibt eine Protokolldatei an. Wird dieser nicht angegeben, wird ein temporärer Speicherort für eine Protokolldatei erstellt.|
|```-NatSubnetPrefix <String>``` [optional] | Präfixwert, der für die Nat-Instanz verwendet wird. In der Regel möchten Sie diesen nur ändern, wenn der Hostcomputer an den gleichen Subnetzbereich wie NetNat des Konverters angefügt ist. Sie können die aktuelle NetNat-Konfiguration des Konverters mithilfe des **Get-NetNat**-Cmdlets abrufen. |
|```-NoRestart [<SwitchParameter>]``` | Fordern Sie nicht zum Neustart auf, wenn Sie das Setupprogramm ausführen (Neustart ist erforderlich, um das Container-Feature zu aktivieren). |

### Konvertierungsparameter  
|Parameter|Beschreibung|
|---------|-----------|
|```-Installer <String>``` | Der Pfad zum Installer für Ihre Anwendung. Muss im unbeaufsichtigten Modus bzw. automatisch ausgeführt werden können.|
|```-InstallerArguments <String>``` [optional] | Eine durch Trennzeichen getrennte Liste oder Zeichenfolge mit Argumenten, die das unbeaufsichtigte bzw. automatische Ausführen des Installers erzwingen. Dieser Parameter ist optional, wenn der Installer eine MSI-Datei ist. Geben Sie zum Abrufen eines Protokolls vom Installer hier das Argument für die Protokollierung an, und verwenden Sie den Pfad ```<log_folder>```, ein Token, das vom Konverter mit dem entsprechenden Pfad ersetzt wird. <br><br>**Hinweis: Unbeaufsichtigte/automatische Flags und Protokollargumente können bei den verschiedenen Installationstechnologien variieren.** <br><br>Ein Verwendungsbeispiel für diesen Parameter: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Ein weiteres Beispiel, in dem keine Protokolldatei generiert wird, kann wie folgt aussehen: ```-InstallerArguments "/quiet", "/norestart"``` Alle Protokolle müssen auf den Tokenpfad ```<log_folder>``` verweisen, wenn der Konverter diese erfassen und im endgültigen Protokollordner speichern soll.|
|```-InstallerValidExitCodes <Int32>``` [optional] | Eine durch Trennzeichen getrennte Liste mit Exitcodes, die angeben, das der Installer erfolgreich ausgeführt wurde (z. B.: 0, 1234, 5678).  Standardmäßig ist dieser 0 für Nicht-MSI-Dateien und 0, 1641, 3010 für MSI-Dateien.|
|```-Destination <String>``` | Das gewünschte Ziel für die appx-Ausgabe des Konverters. DesktopAppConverter kann diesen Speicherort erstellen, wenn er nicht bereits vorhanden ist.|

### Appx-Identitätsparameter  
|Parameter|Beschreibung|
|---------|-----------|
|```-PackageName <String>``` | Der Name des UWP-Pakets
|```-Publisher <String>``` | Der Herausgeber des UWP-Pakets
|```-Version <Version>``` | Die Versionsnummer für das UWP-Paket

### Optionale Appx-Manifestparameter  
|Parameter|Beschreibung|
|---------|-----------|
|```-AppExecutable <String>``` [optional] | Der vollständige Pfad zur ausführbaren Hauptdatei der Anwendung, wenn diese installiert werden soll (falls nicht erforderlich), z.B. „C:\Programme (x86)\MyApp\MyApp.exe“.|
|```-AppFileTypes <String>``` [optional] | Eine durch Trennzeichen getrennte Liste von Dateitypen, denen die Anwendung zugeordnet wird (z.B. „txt, .doc“, ohne die Anführungszeichen).|
|```-AppId <String>``` [optional] | Gibt einen Wert fest, auf den die Anwendungs-ID im appx-Manifest festgelegt wird. Wird kein Wert angegeben, wird diese auf den für *PackageName* übergebenen Wert festgelegt.|
|```-AppDisplayName <String>``` [optional] | Gibt einen Wert fest, auf den der Anzeigename der Anwendung im appx-Manifest festgelegt wird. Wird kein Wert angegeben, wird dieser auf den für *PackageName* übergebenen Wert festgelegt. |
|```-AppDescription <String>``` [optional] | Gibt einen Wert fest, auf den die Anwendungsbeschreibung im appx-Manifest festgelegt wird. Wird kein Wert angegeben, wird diese auf den für *PackageName* übergebenen Wert festgelegt.|
|```-PackageDisplayName <String>``` [optional] | Gibt einen Wert fest, auf den der Anzeigename des Pakets im appx-Manifest festgelegt wird. Wird kein Wert angegeben, wird dieser auf den für *PackageName* übergebenen Wert festgelegt. |
|```-PackagePublisherDisplayName <String>``` [optional] | Gibt einen Wert fest, auf den der Anzeigename des Paketherausgebers im appx-Manifest festgelegt wird. Wird kein Wert angegeben, wird dieser auf den für *Publisher* übergebenen Wert festgelegt. |

### Weitere Konvertierungsparameter  
|Parameter|Beschreibung|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [optional] | Vollständige Pfad zu einem bereits erweiterten Basisimage.|
|```-MakeAppx [<SwitchParameter>]``` [optional] | Ein Switch, mit dem, falls vorhanden, dieses Skript zum Aufrufen von MakeAppx für die Ausgabe angewiesen wird. |
|```-LogFile <String>``` [optional] | Gibt eine Protokolldatei an. Wird dieser nicht angegeben, wird ein temporärer Speicherort für eine Protokolldatei erstellt. |
|```<Common parameters>``` | Dieses Cmdlet unterstützt die folgenden allgemeinen Parameter: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* und *OutVariable*. Weitere Informationen finden Sie unter [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |

### BereinigungsParameter
|Parameter|Beschreibung|
|---------|-----------|
|```Cleanup [<Option>]``` | Führt Bereinigung für die DesktopAppConverter-Artefakte aus. Es gibt 3 gültige Optionen für den Bereinigungsmodus. |
|```Cleanup All``` | Löscht alle erweiterten Basisimages, entfernt alle temporären Konverterdateien, entfernt das Containernetzwerk und deaktiviert die optionale Windows-Funktion. |
|```Cleanup WorkDirectory``` | Entfernt alle temporären Konverterdateien. |
|```Cleanup ExpandedImages``` | Löscht alle erweiterten Basisimages, die auf dem Hostcomputer installiert sind. |

### Paketarchitektur
Die Vorschau für den Desktop-App-Konverter unterstützt jetzt die Erstellung von x86- und x64-App-Paketen, die Sie auf x86- und amd64-Computern ausführen können. Beachten Sie, dass der Desktop-App-Konverter weiterhin auf einem AMD64-Computer ausgeführt werden muss, um eine erfolgreiche Konvertierung durchzuführen.

|Parameter|Beschreibung|
|---------|-----------|
|```-PackageArch <String>``` | Generiert ein Paket mit der angegebenen Architektur. Gültige Optionen sind „x86“ und „x64“ (Beispiel: -PackageArch x86). Dieser Parameter ist optional. Falls nicht angegeben, versucht der DesktopAppConverter die Paketarchitektur automatisch zu erkennen. Wenn die automatische Erkennung fehlschlägt, wird standardmäßig ein x64-Paket erstellt. |

## Siehe auch
+ [Holen Sie sich den Desktop-App-Konverter](http://go.microsoft.com/fwlink/?LinkId=785437)
+ [Migrieren Ihrer Desktop-App zur Universellen Windows-Plattform](https://developer.microsoft.com/windows/bridges/desktop)
+ [Migrieren von Desktop-Apps zur Universellen Windows-Plattform mit dem Desktop-App-Konverter](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial: Migrieren vorhandener Desktopanwendungen zur Universellen Windows-Plattform](https://channel9.msdn.com/events/Build/2016/B829)  
+ [UserVoice für Desktop-Brücke (Project Centennial)](http://aka.ms/UserVoiceDesktopToUwp)
+ [Desktop-App-Bridge zu UWP-Codebeispielen auf GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)



<!--HONumber=Jul16_HO2-->


