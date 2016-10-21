---
author: mcleblanc
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: "Dieser Artikel führt Sie Schritt für Schritt durch die Ausrichtung Ihrer Apps auf verschiedene Bereitstellungs- und Debugziele."
title: Bereitstellen und Debuggen von UWP (Universelle Windows-Plattform)-Apps
translationtype: Human Translation
ms.sourcegitcommit: 3fe300a88c9b4fbb5a8ee21485269ea150111ed6
ms.openlocfilehash: 00693e6debfba6511a75164ef6e1964088ae3516

---

# Bereitstellen und Debuggen von UWP (Universelle Windows-Plattform)-Apps

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieser Artikel führt Sie Schritt für Schritt durch die Ausrichtung Ihrer Apps auf verschiedene Bereitstellungs- und Debugziele.

Microsoft Visual Studio ermöglicht es Ihnen, Ihre UWP-Apps (Universelle Windows-Plattform) auf einer Vielzahl von Windows10-Geräten bereitzustellen und zu debuggen. Das Erstellen und Registrieren der App auf dem Zielgerät wird von Visual Studio abgewickelt.

## Auswählen eines Bereitstellungsziels
 
Zur Auswahl eines Ziels navigieren Sie zur Dropdownliste mit Debugzielen neben der Schaltfläche **Debugging starten**, und wählen Sie das Ziel aus, auf dem Sie Ihre App bereitstellen möchten. Nachdem Sie das Ziel ausgewählt haben, wählen Sie entweder **Debugging starten (F5)** aus, um das Bereitstellen und Debuggen in einem Schritt auszuführen, oder drücken Sie **STRG+F5**, um nur die Bereitstellung auf dem Ziel auszuführen.

![Liste mit Zielgeräten für das Debuggen](images/debug-device-target-list.png)

-   Mit **Simulator** wird die App in einer simulierten Umgebung auf dem aktuellen Entwicklungscomputer bereitgestellt. Diese Option ist nur verfügbar, wenn die **Mindestversion der Zielplattform** Ihrer App niedriger oder gleich der Betriebssystemversion auf Ihrem Entwicklungscomputer ist.
-   Mit **Lokaler Computer** wird die App auf dem aktuellen Entwicklungscomputer bereitgestellt. Diese Option ist nur verfügbar, wenn die **Mindestversion der Zielplattform** Ihrer App niedriger oder gleich der Betriebssystemversion auf Ihrem Entwicklungscomputer ist.
-   Über **Remotecomputer** können Sie ein Remoteziel angeben, auf dem die App bereitgestellt werden soll. Weitere Informationen zur Bereitstellung auf einem Remotecomputer finden Sie unter [Angeben eines Remotegeräts](#specifying-a-remote-device).
-   Mit **Gerät** wird die App auf einem über USB verbundenen Gerät bereitgestellt. Das Gerät muss für Entwickler entsperrt sein und über einen entsperrten Bildschirm verfügen.
-   Bei einem **Emulator**-Ziel wird die App in einem Emulator gestartet und bereitgestellt, wobei die Konfiguration im Namen angegeben ist. Emulatoren sind nur auf Hyper-V-fähigen Computern verfügbar, auf denen Windows 8.1 oder eine höhere Version ausgeführt wird.


## Debuggen von bereitgestellten Apps
Visual Studio kann über den Menübefehl **Debuggen**, **An Prozess anhängen** an alle ausgeführten Prozesse in einer UWP-App angehängt werden. Für das Anhängen an einen laufenden Prozess ist das ursprüngliche Visual Studio-Projekt nicht erforderlich. Das Laden der [Symbole](#symbols) des Prozesses hilft beim Debuggen eines Prozesses ohne den ursprünglichen Code erheblich.  
  
Darüber hinaus kann über den Menübefehl **Debuggen**, **Andere**, **Installierte App-Pakete debuggen** jedes installierten App-Paket angehängt und debuggt werden.   
 
![Dialogfeld „Installiertes App-Paket debuggen"](images/gs-debug-uwp-apps-002.png)   

Bei der Auswahl von **Eigenen Code zunächst nicht starten, sondern debuggen** wird der Visual Studio-Debugger an Ihre UWP-App angehängt, sobald Sie ihn starten. Dies bietet eine effektive Möglichkeit zum Debuggen von Steuerelementpfaden über [verschiedene Startmethoden](../xbox-apps/automate-launching-uwp-apps.md) (z.B. die Protokollaktivierung mit benutzerdefinierten Parametern).  

UWP-Apps können unter Windows8.1 oder höher entwickelt und kompiliert werden. Sie müssen jedoch unter Windows10 ausgeführt werden. Wenn Sie eine UWP-App auf einem PC unter Windows8.1 entwickeln, können Sie eine auf einem anderen Windows10-Gerät ausgeführte UWP-App remote debuggen. Der Host und der Zielcomputer müssen sich im selben LAN befinden. Laden Sie hierzu auf beiden Computern die [Remotetools für Visual Studio](http://aka.ms/remotedebugger) herunter, und installieren Sie sie. Die installierte Version muss der installierten Version von Visual Studio entsprechen. Die ausgewählte Architektur der Auswahl (x86, x64) muss mit der Architektur der Ziel-App übereinstimmen.   
  
## Paketlayout
In Visual Studio2015 Update3 wurde für Entwickler die Option hinzugefügt, den Layoutpfad für ihre UWP-Apps anzugeben. Hiermit wird festgelegt, an welchen Ort auf dem Datenträger das Paketlayout beim Erstellen der App kopiert wird. In der Standardeinstellung wird diese Eigenschaft relativ zum Stammverzeichnis des Projekts festgelegt. Wenn Sie diese Eigenschaft nicht ändern, entspricht das Verhalten dem der früheren Versionen von Visual Studio.

Diese Eigenschaft kann in den **Debug**-Eigenschaften des Projekts geändert werden. 

Wenn Sie beim Erstellen eines Pakets für die App alle Layoutdateien in dieses einschließen möchten, müssen Sie die Projekteigenschaft `<IncludeLayoutFilesInPackage>true</IncludeLayoutFilesInPackage>` hinzufügen. 

So fügen Sie diese Eigenschaft hinzu

1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Projekt entladen** aus. 
2. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **[Projektname].xxproj bearbeiten** aus (XXPROJ lautet je nach Projektsprache unterschiedlich). 
3. Fügen Sie die Eigenschaft hinzu, und laden Sie das Projekt erneut. 

## Festlegen eines Remotegeräts

### C# und Microsoft Visual Basic

Wenn Sie einen Remotecomputer für C#- oder Microsoft Visual Basic-Apps angeben möchten, wählen Sie in der Dropdownliste mit Debugzielen **Remotecomputer** aus. Das Dialogfeld **Remoteverbindungen** wird angezeigt. Hier können Sie eine IP-Adresse angeben oder ein erkanntes Gerät auswählen. Standardmäßig ist der Authentifizierungsmodus **Universell** ausgewählt. Lesen Sie den Abschnitt [Authentifizierungsmodi](#authentication-modes), um den geeigneten Authentifizierungsmodus zu ermitteln.

![Dialogfeld „Remoteverbindungen“](images/debug-remote-connections.png)

Um zu diesem Dialogfeld zurückzukehren, können Sie die Projekteigenschaften öffnen und zur Registerkarte **Debuggen** navigieren. Wählen Sie dort neben **Remotecomputer** die Option **Suchen** aus.

![Registerkarte „Debuggen“](images/debug-remote-machine-config.png)

Wenn Sie eine App auf einem Remote-PC bereitstellen, müssen Sie auch die Visual Studio-Remotetools auf den Ziel-PC herunterladen und dort installieren. Eine vollständige Anleitung finden Sie unter [Anweisungen zu Remote-PCs](#remote-pc-instructions). 

### C++ und JavaScript

So geben Sie ein Remotecomputer-Ziel für eine C++- oder JavaScript-UWP-App an:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Eigenschaften**. 
2. Navigieren Sie zu den Einstellungen für **Debuggen**, und wählen Sie unter **Zu startender Debugger** die Option **Remotecomputer** aus. 
3. Geben Sie den **Computernamen** ein (oder klicken Sie auf **Suchen**, um einen Namen zu suchen), und legen Sie die Eigenschaft **Authentifizierungstyp** fest.

![Seiten mit den Eigenschaften für das Debuggen](images/debug-property-pages.png)

Nachdem der Computer angegeben wurde, können Sie in der Dropdownliste mit Debugzielen **Remotecomputer** auswählen, um zum angegebenen Computer zurückzukehren. Es kann jeweils nur ein Remotecomputer ausgewählt werden.

### Anweisungen für Remote-PCs

Für die Bereitstellung auf einem Remote-PC müssen auf dem Ziel-PC die Visual Studio-Remotetools installiert sein. Außerdem muss auf dem Remote-PC eine Version von Windows ausgeführt werden, die höher oder gleich der Version ist, die in der Eigenschaft **Mindestversion der Zielplattform** Ihrer App festgelegt wurde. Nachdem Sie die Remotetools installiert haben, müssen Sie den Remotedebugger auf dem Ziel-PC starten. 

Suchen Sie dazu im Menü **Start** nach **Remotedebugger**, öffnen Sie den Debugger, und erlauben Sie ihm die Konfiguration Ihrer Firewalleinstellungen, wenn Sie dazu aufgefordert werden. Standardmäßig wird der Debugger mit Windows-Authentifizierung gestartet. Somit werden die Benutzeranmeldeinformationen benötigt, falls der angemeldete Benutzer nicht auf beiden PCs identisch ist.

Um die Einstellung in **Keine Authentifizierung** zu ändern, wechseln Sie unter **Remotedebugger** zu **Extras** -&gt; **Optionen**, und legen Sie **Keine Authentifizierung** fest. Nach dem Einrichten des Remotedebuggers müssen Sie zudem sicherstellen, dass das Hostgerät auf [Entwicklermodus](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) gesetzt wurde. Anschließend können Sie die Bereitstellung über Ihren Entwicklungscomputer ausführen.

Weitere Informationen finden Sie auf der Downloadseite für [Remotetools für Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=717039).

## Authentifizierungsmodi

Für die Bereitstellung auf Remotecomputern gibt es drei Authentifizierungsmodi:

- **Universell (unverschlüsseltes Protokoll)**: Verwenden Sie diesen Authentifizierungsmodus bei der Bereitstellung auf einem Remotegerät, sofern es sich dabei nicht um einen Windows-PC (Desktop oder Laptop) handelt. Derzeit können dies IoT-, Xbox- oder HoloLens Geräte sein. „Universell (unverschlüsseltes Protokoll)“ sollte nur in vertrauenswürdigen Netzwerken verwendet werden. Die Debuggingverbindung ist anfällig für böswillige Benutzer, die zwischen dem Entwicklungs- und dem Remotecomputer übertragene Daten abfangen und manipulieren könnten.
- **Windows**: Dieser Authentifizierungsmodus sollte nur für die Bereitstellung auf Remote-PCs (Desktop oder Laptop) verwendet werden. Verwenden Sie diesen Authentifizierungsmodus, wenn Sie Zugriff auf die Anmeldeinformationen des beim Zielcomputer angemeldeten Benutzers haben. Dies ist der sicherste Kanal für die Remotebereitstellung.
- **Keine**: Dieser Authentifizierungsmodus sollte nur für die Bereitstellung auf Remote-PCs (Desktop oder Laptop) verwendet werden. Verwenden Sie diese Authentifizierungsmoduseinstellung, wenn Sie einen Testcomputer in einer Umgebung eingerichtet haben, bei der die Anmeldung über ein Testkonto erfolgte und keine Anmeldeinformationen eingegeben werden können. Die Remotedebuggereinstellungen müssen so festgelegt sein, dass sie den Modus „Keine Authentifizierung“ akzeptieren.

## Erweiterte Remotebereitstellungsoptionen
Mit der Veröffentlichung von Visual Studio2015 Update3 und Windows10 Anniversary Update sind neue erweiterte Remotebereitstellungsoptionen für bestimmte Windows 10-Geräte verfügbar. Die erweiterten Optionen für Remotebereitstellung finden Sie im Menü **Debuggen** für die Projekteigenschaften.

Zu den neuen Eigenschaften zählen:
* Bereitstellungstyp
* Paketregistrierungspfad
* Beibehalten aller Dateien auf dem Gerät – auch wenn diese nicht mehr Teil des Layouts sind

### Anforderungen
Um die erweiterten Remotebereitstellungsoptionen verwenden zu können, müssen folgende Anforderungen erfüllt sein:
* Visual Studio2015 Update3 muss mit Windows10 Tools1.4.1 (einschließlich des Windows10 Anniversary Update-SDKs) installiert sein.
* Ziel muss ein Windows10 Anniversary Update-Xbox-Remotegerät sein
* Verwenden des universellen Authentifizierungsmodus 

### Eigenschaftenseiten
Bei einer C#- oder Visual Basic-UWP-App wird die Eigenschaftenseite wie folgt aussehen.

![CS- oder VB-Eigenschaften](images/advanced-remote-deploy-cs.png)

Für eine C++-UWP-App wird die Eigenschaftenseite wie folgt aussehen.

![Cpp-Eigenschaften](images/advanced-remote-deploy-cpp.png)

### Dateien auf Gerät kopieren
Mit **Dateien auf Gerät kopieren** werden die Dateien über das Netzwerk physisch auf das Remotegerät übertragen. Das Paketlayout für das **Layout Ordnerpfad** wird kopiert und registriert. Die auf das Gerät kopierten Dateien werden in Visual Studio weiter mit denen des Visual Studio-Projekts synchronisiert. Es gibt jedoch auch die Option, **alle Dateien auf dem Gerät beizubehalten – auch wenn diese nicht mehr Teil des Layouts sind**. Diese Option bedeutet, dass alle Dateien, die zuvor auf das Remotegerät kopiert wurden und nicht mehr Teil Ihres Projekts sind, auf dem Remotegerät verbleiben.

Der beim **Kopieren von Dateien auf das Gerät** angegebene **Paketregistrierungspfad** ist der physische Speicherort auf dem Remotegerät, auf das die Dateien kopiert werden. Dieser Pfad kann als beliebiger relativer Pfad angegeben werden. Der Speicherort, an dem die Dateien bereitgestellt werden, ist relativ zum Stammverzeichnis der Entwicklungsdateien und daher je nach Zielgerät unterschiedlich. Es ist sinnvoll, diesen Pfad anzugeben, wenn mehrere Entwickler ein Gerät verwenden und an Paketen mit identischer Build-Abweichung arbeiten.

> [!NOTE]
> **Dateien auf Gerät kopieren** wird derzeit für Xbox unter Windows10 Anniversary Update unterstützt.

Auf dem Remotegerät wird das Layout je nach Gerätefamilie an folgenden Standardspeicherort kopiert:
  `Xbox: \\MY-DEVKIT\DevelopmentFiles\PACKAGE-REGISTRATION-PATH`

### Registrieren des Layouts über das Netzwerk
Wenn Sie das Layout über das Netzwerk registrieren möchten, können Sie ein Paketlayout für eine Netzwerkfreigabe erstellen, um anschließend das Layout auf dem Remotegerät direkt über das Netzwerk zu registrieren. Hierzu müssen Sie einen Ordnerpfad für das Layout (eine Netzwerkfreigabe) angeben, auf den vom Remotegerät aus zugegriffen werden kann. Die Eigenschaft **Layout Ordnerpfad** ist der relative Pfad zum Visual Studio-PC, während es sich bei der Eigenschaft **Paketregistrierungspfad** um denselben Pfad handelt, der jedoch relativ zum Remotegerät angegeben wurde. 

Um das Layout erfolgreich über das Netzwerk zu registrieren, müssen Sie zunächst für **Layout Ordnerpfad** einen freigegebenen Netzwerkordner angeben. Klicken Sie hierzu im Datei-Explorer mit der rechten Maustaste auf den Ordner, und wählen Sie **Freigeben für > Bestimmte Personen** sowie anschließend die Benutzer aus, für die der Ordner freigegeben werden soll. Wenn Sie versuchen, das Layout über das Netzwerk zu registrieren, werden Sie aufgefordert, Anmeldeinformationen anzugeben. Damit wird sichergestellt, dass Sie die Registrierung als Benutzer mit Zugriff auf die Freigabe durchführen.

Hilfe hierzu bieten die folgenden Beispiele:

- Beispiel1 (lokaler Layoutordner, auf den als Netzwerkfreigabe zugegriffen werden kann):
  * **Ordnerpfad des Layouts** = `D:\Layouts\App1` 
  * **Paketregistrierungspfad** = `\\NETWORK-SHARE\Layouts\App1`

- Beispiel2 (Netzwerk-Layoutordner):
  * **Ordnerpfad des Layouts** = `\\NETWORK-SHARE\Layouts\App1`
  * **Paketregistrierungspfad** = `\\NETWORK-SHARE\Layouts\App1` 

Wenn Sie das Layout zunächst über das Netzwerk registrieren, werden Ihre Anmeldeinformationen auf dem Zielgerät zwischengespeichert, sodass Sie sich nicht immer wieder anmelden müssen. Um zwischengespeicherte Anmeldeinformationen zu entfernen, verwenden Sie [WinAppDeployCmd.exe Tool](https://msdn.microsoft.com/windows/uwp/packaging/install-universal-windows-apps-with-the-winappdeploycmd-tool) (Teil des Windows10-SDKs) mit dem Befehl **Deletecreds**. 

Beim Registrieren des Geräts über das Netzwerk können Sie **Alle Dateien auf dem Gerät beibehalten** nicht auswählen, da keine Dateien physisch auf das Remotegerät kopiert werden. 

> [!NOTE]
> **Layout über das Netzwerk registrieren** wird derzeit für Xbox unter Windows10 Anniversary Update unterstützt.

Auf dem Remotegerät wird das Layout je nach Gerätefamilie an folgendem Standardspeicherort registriert:
  `Xbox: \\MY-DEVKIT\DevelopmentFiles\XrfsFiles`


## Debugoptionen

Unter Windows 10 können Sie die Startleistung von UWP-Apps verbessern, indem Sie Apps proaktiv starten und dann mithilfe eines als [Vorabstart](https://msdn.microsoft.com/library/windows/apps/Mt593297) bezeichneten Verfahrens anhalten. Viele Apps sind in diesem Modus sofort funktionsfähig, das Verhalten einiger Apps muss jedoch möglicherweise angepasst werden. Um das Debuggen von Problemen in diesen Codepfaden zu erleichtern, können Sie das Debuggen der App von Visual Studio im Vorabstartmodus starten. 

Das Debuggen wird sowohl von einem Visual Studio-Projekt (**Debuggen** -&gt; **Andere Debugziele** -&gt; **Vorabstart der universellen Windows-App debuggen**) als auch für bereits auf dem Computer installierte Apps (**Debuggen** -&gt; **Andere Debugziele** -&gt; **Installiertes App-Paket debuggen** mit aktiviertem Kontrollkästchen **App mit Vorabstart aktivieren**) unterstützt. Weitere Informationen finden Sie unter [Debuggen des UWP-Vorabstarts](http://go.microsoft.com/fwlink/p/?LinkId=717245).

Sie können die folgenden Bereitstellungsoptionen auf der Eigenschaftenseite **Debuggen** des Startprojekts festlegen:

- **Lokales Netzwerkloopback zulassen**

  Aus Sicherheitsgründen darf eine UWP-App, die mit der Standardmethode installiert wurde, keine Netzwerkaufrufe an das Gerät senden, auf dem sie installiert ist. Für die bereitgestellte App erstellt die Visual Studio-Bereitstellung standardmäßig eine Ausnahme von dieser Regel. Diese Ausnahme macht es möglich, Kommunikationsverfahren auf einem einzelnen Computer zu testen. Vor dem Übermitteln Ihrer App an den Windows Store sollten Sie Ihre App ohne die Ausnahme testen.
  
  So entfernen Sie die Netzwerkloopback-Ausnahme aus der App
  
  -   Deaktivieren Sie auf der Eigenschaftenseite **Debuggen** für C# und Visual Basic das Kontrollkästchen **Lokales Netzwerkloopback zulassen**.
  -   Legen Sie den Wert **Lokales Netzwerkloopback zulassen** auf der Eigenschaftenseite **Debuggen** für JavaScript und C++ auf **Nein** fest.

- **Eigenen Code zunächst nicht starten, sondern debuggen/Anwendung starten**

  So konfigurieren Sie die Bereitstellung für das automatische Starten einer Debugsitzung beim Starten der App
  
  -   Aktivieren Sie auf der Eigenschaftenseite **Debuggen** für C# und Visual Basic das Kontrollkästchen **Eigenen Code zunächst nicht starten, sondern debuggen**.
  -   Legen Sie den Wert **Anwendung starten** auf der Eigenschaftenseite **Debuggen** für JavaSCript und C++ auf **Ja** fest.

## Symbole

Symboldateien enthalten eine Vielzahl von Daten, die sehr hilfreich beim Debuggen von Code sind (z.B. Variablen, Funktionsnamen und Adressen von Einsprungspunkten). Mit diesen Daten können Sie die Ausnahmen und Ausführungsreihenfolge von Aufruflisten besser überblicken. Über den [Microsoft-Symbolserver](http://msdl.microsoft.com/download/symbols) stehen Symbole für die meisten Windows-Varianten zur Verfügung. Für schnellere Offline-Lookups können Sie jedoch auch unter [Herunterladen von Windows-Symbolpaketen](http://aka.ms/winsymbols) heruntergeladen werden.

Wählen Sie zum Festlegen von Symboloptionen für Visual Studio **Extras > Optionen** aus, und navigieren Sie im Dialogfeld zu **Debuggen > Symbole**.

![Dialogfeld „Optionen“](images/gs-debug-uwp-apps-004.png)

Legen Sie die **Sympath**-Variable auf dem Speicherort des Symbolpakets fest, um die Symbole in einer Debugsitzung mit [WinDbg](#windbg) zu laden. Der folgende Befehl lädt beispielsweise Symbole vom Microsoft-Symbolserver und speichert sie dann im Verzeichnis C:\Symbols zwischen:

```
.sympath SRV*C:\Symbols*http://msdl.microsoft.com/download/symbols
.reload
```

Mit `‘;’` als Trennzeichen oder dem Befehl `.sympath+` können Sie mehrere Pfade hinzufügen. Mehr zu erweiterten Symbolvorgängen mit WinDbg finden Sie unter [Öffentliche und private Symbole](https://msdn.microsoft.com/library/windows/hardware/ff553493).

## WinDbg

WinDbg ist ein leistungsstarker Debugger, der als Teil der Debugtools für Windows bereitgestellt wird (Letzteres ist des [Windows SDK](http://go.microsoft.com/fwlink/p/?LinkID=271979)). Die Windows-SDK-Installation ermöglicht die Installation der Debugtools für Windows als eigenständiges Produkt. WinDbg ist beim Debuggen von systemeigenem Code sehr hilfreich. Es wird jedoch nicht für Apps empfohlen, die in verwaltetem Code oder in HTML5 geschrieben wurden. 

Um WinDbg mit UWP-Apps zu verwenden, müssen Sie zunächst die Prozesslebensdauer-Verwaltung (Process Lifetime Management, PLM) für Ihr App-Paket mit PLMDebug deaktivieren (siehe [Test- und Debugtools für die Prozesslebensdauer-Verwaltung (PLM)](testing-debugging-plm.md). 

```
plmdebug /enableDebug [PackageFullName] "\"C:\Program Files\Debugging Tools for Windows (x64)\WinDbg.exe\" -server npipe:pipe=test"
```

Im Gegensatz zu Visual Studio arbeitet der Großteil der Kernfunktionen von WinDbg mit Befehlen im Befehlsfenster. Mit den Befehlen können Sie den Ausführungszustand anzeigen, Benutzermodus-Speicherabbilder prüfen und in verschiedensten Modi debuggen. 

Einer der am häufigsten in WinDbg verwendeten Befehle ist `!analyze -v`. Er wird verwendet, um ausführliche Informationen zur aktuellen Ausnahme abzurufen. Diese Informationen umfassen unter anderem:

- FAULTING_IP: Anweisungszeiger zum Zeitpunkt des Fehlers
- EXCEPTION_RECORD: Adresse, Code und Flags der aktuellen Ausnahme
- STACK_TEXT: Stapelüberwachung vor der Ausnahme

Eine vollständige Liste aller WinDbg-Befehle finden Sie unter [Debuggerbefehle](https://msdn.microsoft.com/library/ff540507).

## Verwandte Themen
- [Testen und Debuggen von Tools für die Prozesslebensdauer-Verwaltung (Process Lifetime Management, PLM)](testing-debugging-plm.md)
- [Debugging, Tests und Leistung](index.md)



<!--HONumber=Aug16_HO3-->


