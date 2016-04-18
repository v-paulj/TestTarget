---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“
description: Die Windows-Anwendungsbereitstellung (WinAppDeployCmd.exe) ist ein Befehlszeilentool, mit dem Sie eine UWP (Universelle Windows-Plattform)-App von einem Windows 10-Computer auf beliebigen Windows 10 Mobile-Geräten bereitstellen können.
---
# Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Die Windows-Anwendungsbereitstellung (WinAppDeployCmd.exe) ist ein Befehlszeilentool, mit dem Sie eine UWP (Universelle Windows-Plattform)-App von einem Windows 10-Computer auf beliebigen Windows 10 Mobile-Geräten bereitstellen können. Dieses Tool ermöglicht die Bereitstellung eines APPX-Pakets, wenn das Windows 10 Mobile-Gerät über USB verbunden ist oder sich im selben Subnetz befindet, ohne dass Microsoft Visual Studio oder die Projektmappe für diese App erforderlich ist. Dieser Artikel beschreibt, wie UWP-Apps mit diesem Tool installiert werden.

Um das Tool WinAppDeployCmd über eine Eingabeaufforderung oder Skriptdatei auszuführen, muss lediglich das Windows 10 SDK installiert sein. Wenn Sie eine App mit „WinAppDeployCmd.exe“ installieren, verwendet es die APPX-Datei, um Ihre App auf ein Windows 10 Mobile-Gerät querzuladen. Mit diesem Befehl wird nicht das für Ihre App erforderliche Zertifikat installiert. Zum Ausführen der App muss sich das Windows 10 Mobile-Gerät im Entwicklermodus befinden oder bereits über das Zertifikat verfügen.

Bevor Sie die App für Windows 10 Mobile-Geräte bereitstellen können, müssen Sie zunächst ein Paket erstellen. Weitere Informationen finden Sie unter \[parent link here\].

Das Tool **WinAppDeployCmd.exe** befindet sich unter dem folgenden Pfad auf Ihrem Computer unter Windows 10: **C:\\Programme (x86)\\Windows Kits\\10\\bin\\x86\\WinAppDeployCmd.exe** (basierend auf dem Installationspfad für das SDK). Verbinden Sie zunächst das Windows 10 Mobile-Gerät mit demselben Subnetz, oder verbinden Sie es über einen USB-Port direkt mit Ihrem Windows 10-Computer. Verwenden Sie dann die folgende Syntax, und orientieren Sie sich an den Beispielen zu diesem Befehl weiter unten in diesem Artikel, um das APPX-Paket bereitzustellen:

## Syntax und Optionen zu WinAppDeployCmd

Hier die mögliche Syntax, die Sie für **WinAppDeployCmd.exe** verwenden können:

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
```

Sie können eine App auf dem Zielgerät installieren oder deinstallieren oder eine bereits installierte App aktualisieren. Um die Daten oder Einstellungen beizubehalten, die von einer bereits installierten App gespeichert wurden, verwenden Sie die **update**-Optionen anstelle der **install**-Optionen.

Die folgende Tabelle enthält die Befehle für **WinAppDeployCmd.exe**.

|             |                                                                     |
|-------------|---------------------------------------------------------------------|
| **Befehl** | **Beschreibung**                                                     |
| Geräte     | Zeigt die Liste verfügbarer Netzwerkgeräte an.                         |
| install     | Installiert ein UWP-App-Paket auf dem Zielgerät.                     |
| update      | Aktualisiert eine UWP-App, die bereits auf dem Zielgerät installiert ist.    |
| list        | Zeigt die Liste der auf dem angegebenen Zielgerät installierten UWP-Apps an. |
| uninstall   | Deinstalliert das angegebene App-Paket vom Zielgerät.         |

 

Die folgende Tabelle enthält die Optionen für **WinAppDeployCmd.exe**.

|                  |                                                                                                                                                                                                               |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Befehl**      | **Beschreibung**                                                                                                                                                                                               |
| -h (-help)       | Zeigt die Befehle, Optionen und Argumente an.                                                                                                                                                                     |
| -ip              | Die IP-Adresse des Zielgeräts.                                                                                                                                                                              |
| -g (-guid)       | Der eindeutige Bezeichner des Zielgeräts.                                                                                                                                                                       |
| -d (-dependency) | (Optional) Gibt den Abhängigkeitspfad für jede Paketabhängigkeit an. Wenn kein Pfad angegeben ist, sucht das Tool Abhängigkeiten im Stammverzeichnis des App-Pakets und in den SDK-Verzeichnissen. |
| -f (-file)       | Der Dateipfad für das App-Paket, das installiert, aktualisiert oder deinstalliert werden soll.                                                                                                                                                |
| -p (-package)    | Der vollständige Paketname für das zu deinstallierende App-Paket. (Mit dem list-Befehl können Sie den vollständigen Namen von Paketen suchen, die bereits auf dem Gerät installiert sind.)                                                   |
| -pin             | Eine PIN, falls zum Herstellen einer Verbindung mit dem Zielgerät erforderlich. (Wenn eine Authentifizierung erforderlich ist, werden Sie aufgefordert, den Versuch mit der -pin-Option zu wiederholen.)                                                 |

 

Die folgende Tabelle enthält die Optionen für **WinAppDeployCmd.exe**.

|                        |                                                                              |
|------------------------|------------------------------------------------------------------------------|
| **Argument**           | **Beschreibung**                                                              |
| &lt;x&gt;              | Das Timeout in Sekunden. (Der Standardwert ist 10.)                                          |
| &lt;Adresse&gt;        | Die IP-Adresse oder der eindeutige Bezeichner des Zielgeräts.                        |
| &lt;a&gt;&lt;b&gt; ... | Der Abhängigkeitspfad für die einzelnen App-Paketabhängigkeiten.                    |
| &lt;p&gt;              | Eine alphanumerische PIN, die in den Geräteeinstellungen angezeigt und zum Herstellen einer Verbindung verwendet wird. |
| &lt;Pfad&gt;           | Der Dateisystempfad.                                                            |
| &lt;Name&gt;           | Der vollständige Paketname für das zu deinstallierende App-Paket.                          |

 
## Beispiele zu „WinAppDeployCmd.exe“

Im Folgenden einige Beispiele dazu, wie Sie mithilfe der Syntax für **WinAppDeployCmd.exe** eine Bereitstellung über die Befehlszeile vornehmen.

Zeigt die für die Bereitstellung verfügbaren Geräte an. Der Befehl verursacht in drei Sekunden ein Timeout.

``` syntax
WinAppDeployCmd devices 3
```

Installiert die App aus dem Paket „MyApp.appx“, das sich im Downloadverzeichnis Ihres PCs befindet, auf einem Windows 10 Mobile-Gerät. Die IP-Adresse und die PIN zum Herstellen einer Verbindung mit dem Gerät lautet 192.168.0.1 bzw. A1B2C3.

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Deinstalliert das angegebene Paket (unter Verwendung des vollständigen Namens) von einem Windows 10 Mobile-Gerät mit der IP-Adresse 192.168.0.1. Mit dem list-Befehl können Sie den vollständigen Namen von Paketen anzeigen, die auf einem Gerät installiert sind.

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Aktualisiert die bereits auf dem Windows 10 Mobile-Gerät mit der IP-Adresse 192.168.0.1 installierte App mit dem angegebenen APPX-Paket.

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```



<!--HONumber=Mar16_HO1-->


