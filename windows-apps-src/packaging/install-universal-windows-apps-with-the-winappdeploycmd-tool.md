---
author: msatranjr
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: "Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“"
description: "Die Windows-Anwendungsbereitstellung (WinAppDeployCmd.exe) ist ein Befehlszeilentool, mit dem Sie eine App für die Universelle Windows-Plattform (UWP) App von einem Windows10-Computer auf jedem Windows10-Gerät bereitstellen können."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 5f6bfb13e2e80f21902ec923e32f68046f313a13

---
# Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Die Windows-Anwendungsbereitstellung (WinAppDeployCmd.exe) ist ein Befehlszeilentool, mit dem Sie eine App für die Universelle Windows-Plattform (UWP) App von einem Windows10-Computer auf jedem Windows10-Gerät bereitstellen können. Dieses Tool ermöglicht Ihnen die Bereitstellung eines APPX-Pakets, wenn das Windows10-Gerät über USB verbunden ist oder im gleichen Subnetz verfügbar ist, ohne dass Microsoft Visual Studio oder die Projektmappe für diese App erforderlich sind. Sie können die App auch bereitstellen, ohne sie zuerst zu einem Remote-PC oder zu Xbox One zu verpacken. Dieser Artikel beschreibt, wie UWP-Apps mit diesem Tool installiert werden.

Um das Tool WinAppDeployCmd über eine Eingabeaufforderung oder Skriptdatei auszuführen, muss lediglich das Windows 10 SDK installiert sein. Wenn Sie eine App mit „WinAppDeployCmd.exe“ installieren, werden die APPX-Datei oder AppxManifest (für lose Dateien) verwendet, um Ihre App auf ein Windows10-Gerät querzuladen. Mit diesem Befehl wird nicht das für Ihre App erforderliche Zertifikat installiert. Zum Ausführen der App muss sich das Windows10-Gerät im Entwicklermodus befinden oder bereits über das Zertifikat verfügen.

Um eine Bereitstellung auf mobilen Geräten auszuführen, müssen Sie zunächst ein Paket erstellen. Weitere Informationen finden Sie [hier](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

Das Tool **WinAppDeployCmd.exe** befindet sich unter folgendem Pfad auf Ihrem Windows10-Computer: **C:\\Programme (x86)\\Windows Kits\\10\\bin\\x86\\WinAppDeployCmd.exe** (abhängig vom Installationspfad für das SDK). Verbinden Sie zunächst das Windows10-Gerät mit dem gleichen Subnetz oder direkt mit Ihrem Windows 10-Computer über eine USB-Verbindung. Verwenden Sie anschließend die folgende Syntax und die Beispiele zu diesem Befehl weiter unten in diesem Artikel, um die UWP-App bereitzustellen:

## Syntax und Optionen für WinAppDeployCmd

Dies ist die mögliche Syntax, die Sie für **WinAppDeployCmd.exe** verwenden können:

``` syntax
WinAppDeployCmd command -option <argument> ...
    WinAppDeployCmd devices
    WinAppDeployCmd devices <x>
    WinAppDeployCmd install -file <path> -ip <address>
    WinAppDeployCmd install -file <path> -guid <address> -pin <p>
    WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> ...
    WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b> ...
    WinAppDeployCmd uninstall -file <path>
    WinAppDeployCmd uninstall -package <name>
    WinAppDeployCmd update -file <path>
    WinAppDeployCmd list -ip <address>
    WinAppDeployCmd list -guid <address>
    WinAppDeployCmd deployfiles -file <path> -remotedeploydir <remoterelativepath> -ip <address>
    WinAppDeployCmd registerfiles -remotedeploydir <remoterelativepath> -ip <address>
    WinAppDeployCmd addcreds -credserver <server> -credusername <username> -credpassword <password> -ip <address>
    WinAppDeployCmd getcreds -credserver <server> -ip <address>
    WinAppDeployCmd deletecreds -credserver <server> -ip <address>

```

Sie können eine App auf dem Zielgerät installieren oder deinstallieren oder eine bereits installierte App aktualisieren. Um die Daten oder Einstellungen beizubehalten, die von einer bereits installierten App gespeichert wurden, verwenden Sie die **update**-Optionen anstelle der **install**-Optionen.

Die folgende Tabelle enthält die Befehle für **WinAppDeployCmd.exe**.


| **Befehl**  | **Beschreibung**                                                     |
|--------------|---------------------------------------------------------------------|
| Geräte      | Zeigt die Liste verfügbarer Netzwerkgeräte an.                         |
| install      | Installiert ein UWP-App-Paket auf dem Zielgerät.                     |
| update       | Aktualisiert eine UWP-App, die bereits auf dem Zielgerät installiert ist.    |
| list         | Zeigt die Liste der auf dem angegebenen Zielgerät installierten UWP-Apps an. |
| uninstall    | Deinstalliert das angegebene App-Paket vom Zielgerät.         |
| deployfiles  | Kopiert die App mit loser Datei am Zielpfad zum relativen Remotepfad auf dem Gerät.|
| registerfiles| Registriert die App mit loser Datei am Remotebereitstellungsverzeichnis.         |
| addcreds     | Fügt einer Xbox Anmeldeinformationen hinzu, um auf einen Netzwerkspeicherort für die App-Registrierung zuzugreifen.|
| getcreds     | Ruft Netzwerkanmeldeinformationen ab, die das Ziel beim Ausführen einer Anwendung von einer Netzwerkfreigabe verwendet.|
| deletecreds  | Löscht Netzwerkanmeldeinformationen, die das Ziel beim Ausführen einer Anwendung von einer Netzwerkfreigabe verwendet.|

 
Die folgende Tabelle enthält die Optionen für **WinAppDeployCmd.exe**.

| **Befehl**  | **Beschreibung**                                                     |
|--------------|---------------------------------------------------------------------|
| -h (-help)       | Zeigt die Befehle, Optionen und Argumente an.|
| -ip              | Die IP-Adresse des Zielgeräts.|
| -g (-guid)       | Der eindeutige Bezeichner des Zielgeräts.|
| -d (-dependency) | (Optional) Gibt den Abhängigkeitspfad für jede Paketabhängigkeit an. <br />Wenn kein Pfad angegeben ist, sucht das Tool Abhängigkeiten im Stammverzeichnis des App-Pakets und in den SDK-Verzeichnissen.|
| -f (-file)       | Der Dateipfad für das App-Paket, das installiert, aktualisiert oder deinstalliert werden soll.|
| -p (-package)    | Der vollständige Paketname für das zu deinstallierende App-Paket. <br />(Mit dem list-Befehl können Sie den vollständigen Namen von Paketen suchen, die bereits auf dem Gerät installiert sind.)|
| -pin             | Eine PIN, falls zum Herstellen einer Verbindung mit dem Zielgerät erforderlich. <br />(Wenn eine Authentifizierung erforderlich ist, werden Sie aufgefordert, den Versuch mit der Option -pin zu wiederholen.)|
| -credserver      | Der Servername der Netzwerkanmeldeinformationen für die Verwendung durch das Ziel.|
| -credusername    | Der Benutzername der Netzwerkanmeldeinformationen für die Verwendung durch das Ziel.|
| -credpassword    | Das Kennwort der Netzwerkanmeldeinformationen für die Verwendung durch das Ziel.|
| -connecttimeout  | Die Zeitüberschreiung in Sekunden, die bei der Herstellung der Verbindung mit dem Gerät verwendet wird.|
| -remotedeploydir | Der relative Pfad/Name des Verzeichnisses, in das Dateien auf dem Remotegerät kopiert werden sollen. <br />Dies ist ein bekannter, automatisch festgelegter Remotebereitstellungsordner.|
| -deleteextrafile | Wechselt, um anzugeben, ob vorhandene Dateien im Remoteverzeichnis gelöscht werden sollen, um mit dem Quellverzeichnis übereinzustimmen.|
 

Die folgende Tabelle enthält die Optionen für **WinAppDeployCmd.exe**.

| **Argument**           | **Beschreibung**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | Zeitüberschreitung in Sekunden. (Der Standardwert ist 10.)                                          |
| &lt;address&gt;        | Die IP-Adresse oder der eindeutige Bezeichner des Zielgeräts.                        |
| &lt;a&gt;&lt;b&gt; ... | Der Abhängigkeitspfad für die einzelnen App-Paket-Abhängigkeiten.                    |
| &lt;p&gt;              | Eine alphanumerische PIN, die in den Geräteeinstellungen angezeigt und zum Herstellen einer Verbindung verwendet wird. |
| &lt;path&gt;           | Dateisystempfad.                                                            |
| &lt;name&gt;           | Vollständiger Paketname für das zu deinstallierende App-Paket.                          |
| &lt;server&gt;         | Server im Dateinetzwerk.                                                  |
| &lt;username&gt;       | Benutzer für die Anmeldeinformationen mit Zugriff auf den Server im Dateinetzwerk.      |
| &lt;password&gt;       | Kennwort für die Anmeldeinformationen mit Zugriff auf den Server im Dateinetzwerk. |
| &lt;remotedeploydir&gt;| Verzeichnis auf dem Gerät relativ zum Bereitstellungsspeicherort.                      |

 
## Beispiele für „WinAppDeployCmd.exe“

Im Folgenden finden Sie einige Beispiele dazu, wie Sie mithilfe der Syntax für **WinAppDeployCmd.exe** eine Bereitstellung über die Befehlszeile ausführen.

Zeigt die für die Bereitstellung verfügbaren Geräte an. Der Befehl verursacht in drei Sekunden ein Timeout.

``` syntax
WinAppDeployCmd devices 3
```

Installiert die App aus dem Paket „MyApp.appx“, das sich im Downloadverzeichnis Ihres PCs befindet, auf ein Windows10-Gerät mit der IP-Adresse 192.168.0.1 und der PIN A1B2C3, um eine Verbindung mit dem Gerät herzustellen.

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Deinstalliert das angegebene Paket (unter Verwendung des vollständigen Namens) von einem Windows10-Gerät mit der IP-Adresse 192.168.0.1. Mit dem list-Befehl können Sie die vollständigen Namen aller Pakete anzeigen, die auf einem Gerät installiert sind.

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Aktualisiert die bereits auf dem Windows10-Gerät mit der IP-Adresse 192.168.0.1 installierte App unter Verwendung des angegebenen APPX-Pakets.

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

Stellt die Dateien einer App auf einem PC oder auf Xbox mit der IP-Adresse 192.168.0.1 im gleichen Ordner wie das AppxManifest im Verzeichnis app1_F5 am Bereitstellungspfad des Geräts bereit.

``` syntax
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

Registriert die App im Verzeichnis app1_F5 am Bereitstellungspfad des PCs oder der Xbox mit 192.168.0.1.

``` syntax
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```



<!--HONumber=Jul16_HO2-->


