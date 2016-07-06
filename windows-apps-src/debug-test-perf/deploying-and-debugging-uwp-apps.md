---
author: mcleblanc
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: "Dieser Artikel führt Sie Schritt für Schritt durch die Ausrichtung Ihrer Apps auf verschiedene Bereitstellungs- und Debugziele."
title: Bereitstellen und Debuggen von UWP (Universelle Windows-Plattform)-Apps
translationtype: Human Translation
ms.sourcegitcommit: 14f6684541716034735fbff7896348073fa55f85
ms.openlocfilehash: e2209e90080c7346bb363304b1a28f6446300332

---

# Bereitstellen und Debuggen von UWP (Universelle Windows-Plattform)-Apps

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieser Artikel führt Sie Schritt für Schritt durch die Ausrichtung Ihrer Apps auf verschiedene Bereitstellungs- und Debugziele.

Microsoft Visual Studio ermöglicht es Ihnen, Ihre UWP-Apps (Universelle Windows-Plattform) auf einer Vielzahl von Windows 10-Geräten bereitzustellen und zu debuggen. Die Erstellung und Registrierung der Anwendung auf dem Zielgerät wird von Visual Studio abgewickelt.

## Auswählen eines Bereitstellungsziels

Zur Auswahl eines Ziels navigieren Sie zur Dropdownliste mit Debugzielen neben der Schaltfläche **Debugging starten** und wählen das Ziel aus, auf dem Sie Ihre Anwendung bereitstellen möchten. Nachdem Sie das Ziel ausgewählt haben, wählen Sie entweder **Debugging starten (F5)** aus, um die Bereitstellung und das Debuggen in einem Schritt auszuführen, oder drücken Sie **STRG+F5**, um nur die Bereitstellung auf dem Ziel auszuführen.

![](images/debug-device-target-list.png)

-   Mit **Lokaler Computer** wird die Anwendung auf dem aktuellen Entwicklungscomputer bereitgestellt. Diese Option ist nur verfügbar, wenn die **Mindestversion der Zielplattform** Ihrer Anwendung niedriger oder gleich der Betriebssystemversion auf Ihrem Entwicklungscomputer ist.
-   Mit **Simulator** wird die Anwendung in einer simulierten Umgebung auf dem aktuellen Entwicklungscomputer bereitgestellt. Diese Option ist nur verfügbar, wenn die **Mindestversion der Zielplattform** Ihrer Anwendung niedriger oder gleich der Betriebssystemversion auf Ihrem Entwicklungscomputer ist.
-   Mit **Gerät** wird die Anwendung auf einem über USB verbundenen Gerät bereitgestellt. Das Gerät muss für Entwickler entsperrt sein und über einen entsperrten Bildschirm verfügen.
-   Bei einem **Emulator**-Ziel wird die Anwendung in einem Emulator gestartet und bereitgestellt, wobei die Konfiguration im Namen angegeben ist. Emulatoren sind nur auf Hyper-V-fähigen Computern verfügbar, auf denen Windows 8.1 oder eine höhere Version ausgeführt wird.
-   Über **Remotecomputer** können Sie ein Remoteziel angeben, auf dem die Anwendung bereitgestellt werden soll. Weitere Informationen zur Bereitstellung auf einem Remotecomputer finden Sie unter [Angeben eines Remotegeräts](#specifying-a-remote-device).

## Debuggen von bereitgestellten Apps
Visual Studio kann über den Menübefehl **Debuggen**, **An Prozess anhängen** an alle ausgeführten Prozesse in einer UWP-App angehängt werden. Für das Anhängen an einen laufenden Prozess ist das ursprüngliche Visual Studio-Projekt nicht erforderlich. Das Laden der [Symbole](#symbols) des Prozesses hilft beim Debuggen eines Prozesses ohne den ursprünglichen Code erheblich.  
  
Darüber hinaus kann über den Menübefehl **Debuggen**, **Andere**, **Installierte App-Pakete debuggen** jedes installierten App-Paket angehängt und debuggt werden.   
 
![Dialogfeld „Installiertes App-Paket debuggen"](images/gs-debug-uwp-apps-002.png)  

Bei der Auswahl von **Eigenen Code zunächst nicht starten, sondern debuggen** wird der Visual Studio-Debugger an Ihre UWP-App angehängt, sobald Sie ihn starten. Dies bietet eine effektive Möglichkeit zum Debuggen von Steuerelementpfaden über [verschiedene Startmethoden](../xbox-apps/automate-launching-uwp-apps.md) (z. B. die Protokollaktivierung mit benutzerdefinierten Parametern).  

UWP-Apps können unter Windows 8.1 oder höher entwickelt und kompiliert werden. Sie müssen jedoch unter Windows 10 ausgeführt werden. Wenn Sie eine UWP-App auf einem PC unter Windows 8.1 entwickeln, können Sie eine auf einem anderen Windows 10-Gerät ausgeführte UWP-App remote debuggen. Der Host und der Zielcomputer müssen sich im selben LAN befinden. Laden Sie hierzu auf beiden Computern die [Remotetools für Visual Studio](http://aka.ms/remotedebugger) herunter, und installieren Sie sie. Die installierte Version muss der installierten Version von Visual Studio entsprechen. Die ausgewählte Architektur der Auswahl (x86, x64) muss mit der Architektur der Zielanwendung übereinstimmen.   
  

## Festlegen eines Remotegeräts

### C# und Microsoft Visual Basic

Wenn Sie einen Remotecomputer für C#- oder Microsoft Visual Basic-Apps angeben möchten, wählen Sie **Remotecomputer** in der Dropdownliste mit Debugzielen aus. Das Dialogfeld **Remoteverbindungen** wird angezeigt, in dem Sie einen IP-Adresse angeben oder ein erkanntes Gerät auswählen können. Standardmäßig ist der Authentifizierungsmodus **Universell** ausgewählt. Lesen Sie den Abschnitt [Authentifizierungsmodi](#authentication-modes), um den geeigneten Authentifizierungsmodus zu ermitteln.

![](images/debug-remote-connections.png)

Um zu diesem Dialogfeld zurückzukehren, können Sie die Projekteigenschaften öffnen und zur Registerkarte **Debuggen** navigieren. Wählen Sie dort neben **Remotecomputer** die Option **Suchen**

![](images/debug-remote-machine-config.png)

Wenn Sie eine Anwendung auf einem Remote-PC bereitstellen, müssen Sie auch die Visual Studio-Remotetools auf den Ziel-PC herunterladen und dort installieren. Eine vollständige Anleitung finden Sie unter [Anweisungen zu Remote-PCs](#remote-pc-instructions).

### C++ und JavaScript

Wenn Sie ein Remotecomputerziel für eine C++- oder JavaScript-UWP-App angeben möchten, wechseln Sie zu den Projekteigenschaften, indem Sie mit der rechten Maustaste auf das Projekt im **Projektmappen-Explorer** klicken und dann auf **Eigenschaften** klicken. Navigieren Sie zu den Einstellungen für **Debuggen**, und ändern Sie **Zu startender Debugger** in **Remotecomputer**. Füllen Sie dann das Feld **Computername** aus (oder klicken Sie auf **Suchen** , um einen Namen zu suchen), und legen Sie die Eigenschaft **Authentifizierungstyp** fest.

![](images/debug-property-pages.png)
Nachdem der Computer angegeben wurde, können Sie **Remotecomputer** in der Dropdownliste mit Debugzielen auswählen, um zum angegebenen Computer zurückzukehren. Es kann jeweils nur ein Remotecomputer ausgewählt werden.

### Anweisungen für Remote-PCs

Für die Bereitstellung auf einem Remote-PC müssen auf dem Ziel-PC die Visual Studio-Remotetools installiert sein. Außerdem muss auf dem Remote-PC eine Version von Windows ausgeführt werden, die höher oder gleich der Version ist, die in der Eigenschaft **Mindestversion der Zielplattform** Ihrer App festgelegt wurde. Nachdem Sie die Remotetools installiert haben, müssen Sie den Remotedebugger auf dem Ziel-PC starten. Suchen Sie dazu nach **Remotedebugger** im Menü **Start**, starten Sie den Debugger, und erlauben Sie ihm die Konfiguration Ihrer Firewalleinstellungen, wenn Sie dazu aufgefordert werden. Standardmäßig wird der Debugger mit Windows-Authentifizierung gestartet. Somit werden die Benutzeranmeldeinformationen benötigt, falls der angemeldete Benutzer nicht auf beiden PCs identisch ist. Um die Einstellung in **Keine Authentifizierung** zu ändern, wechseln Sie im **Remotedebugger** zu **Extras** - -&gt; **Optionen**, und legen Sie **Keine Authentifizierung** fest. Sobald der Remotedebugger eingerichtet ist, können Sie mit der Bereitstellung von Ihrem Entwicklungscomputer beginnen.

Weitere Informationen finden Sie auf der Downloadseite für [Remotetools für Visual Studio]( http://go.microsoft.com/fwlink/?LinkId=717039)

## Authentifizierungsmodi

Für die Bereitstellung auf Remotecomputern gibt es drei Authentifizierungsmodi:

- **Universell (unverschlüsseltes Protokoll)**: Verwenden Sie diesen Authentifizierungsmodus bei der Bereitstellung auf einem Remotegerät, das kein Windows-PC (Desktop oder Laptop) ist. Derzeit trifft dies nur auf IoT-Geräte zu. „Universell (unverschlüsseltes Protokoll)“ sollte nur in vertrauenswürdigen Netzwerken verwendet werden. Die Debuggingverbindung ist anfällig für böswillige Benutzer, die zwischen dem Entwicklungs- und dem Remotecomputer übertragene Daten abfangen und manipulieren könnten.
- **Windows**: Dieser Authentifizierungsmodus sollte nur für die Bereitstellung auf Remote-PCs (Desktop oder Laptop) verwendet werden. Verwenden Sie diesen Authentifizierungsmodus, wenn Sie Zugriff auf die Anmeldeinformationen des beim Zielcomputer angemeldeten Benutzers haben. Dies ist der sicherste Kanal für die Remotebereitstellung.
- **Keine**: Dieser Authentifizierungsmodus sollte nur für die Bereitstellung auf Remote-PCs (Desktop oder Laptop) verwendet werden. Verwenden Sie diese Authentifizierungsmoduseinstellung, wenn Sie einen Testcomputer in einer Umgebung eingerichtet haben, bei der die Anmeldung über ein Testkonto erfolgte und keine Anmeldeinformationen eingegeben werden können. Die Remotedebuggereinstellungen müssen so festgelegt sein, dass sie den Modus „Keine Authentifizierung“ akzeptieren.

## Debugoptionen

Unter Windows 10 können Sie die Startleistung von UWP-Apps verbessern, indem Sie Apps proaktiv starten und dann mithilfe eines als [Vorabstart](https://msdn.microsoft.com/library/windows/apps/Mt593297) bezeichneten Verfahrens anhalten. Viele Anwendungen sind auf Anhieb in diesem Modus funktionsfähig, das Verhalten einiger Anwendungen muss jedoch u. U. angepasst werden. Um das Debuggen von Problemen in diesen Codepfaden zu erleichtern, können Sie das Debuggen der App von Visual Studio im Vorabstartmodus starten. Das Debuggen wird sowohl von einem Visual Studio-Projekt (**Debuggen** - -&gt;**Andere Debugziele** - -&gt;**Vorabstart der universellen Windows-App debuggen**) als auch für bereits auf dem Computer installierte Apps (**Debuggen** - -&gt;**Andere Debugziele** - -&gt;**Installiertes App-Paket debuggen** mit aktiviertem Kontrollkästchen **App mit Vorabstart aktivieren**) unterstützt. Weitere Informationen finden Sie unter [Debuggen des UWP-Vorabstarts]( http://go.microsoft.com/fwlink/?LinkId=717245).

Sie können die folgenden Bereitstellungsoptionen auf der Eigenschaftenseite **Debuggen** des Startprojekts festlegen.

**Lokales Netzwerkloopback zulassen**

Aus Sicherheitsgründen darf eine UWP-App, die mit der Standardmethode installiert wurde, keine Netzwerkaufrufe an das Gerät senden, auf dem sie installiert ist. Für die bereitgestellte App erstellt die Visual Studio-Bereitstellung standardmäßig eine Ausnahme von dieser Regel. Diese Ausnahme macht es möglich, Kommunikationsverfahren auf einem einzelnen Computer zu testen. Vor dem Übermitteln Ihrer App an den Windows Store sollten Sie Ihre App ohne die Ausnahme testen.

So entfernen Sie die Netzwerkloopback-Ausnahme aus der App

-   Deaktivieren Sie auf der Eigenschaftenseite **Debuggen** für C# und Visual Basic das Kontrollkästchen **Lokales Netzwerkloopback zulassen**.
-   Legen Sie den Wert **Lokales Netzwerkloopback zulassen** auf der Eigenschaftenseite **Debuggen** für JavaScript und C++ auf **Nein** fest.

**Eigenen Code zunächst nicht starten, sondern debuggen (C#- und Visual Basic)/App starten (JavaScript und C++)**

So konfigurieren Sie die Bereitstellung für das automatische Starten einer Debugsitzung beim Starten der App

-   Aktivieren Sie auf der Eigenschaftenseite **Debuggen** für C# und Visual Basic das Kontrollkästchen **Eigenen Code zunächst nicht starten, sondern debuggen**.
-   Legen Sie den Wert **Anwendung starten** auf der Eigenschaftenseite **Debuggen** für JavaSCript und C++ auf **Ja** fest.

## Symbole

Symboldateien enthalten eine Vielzahl von Daten, die sehr hilfreich beim Debuggen von Code sind (z. B. Variablen, Funktionsnamen und Adressen von Einsprungspunkten). Mit diesen Daten können Sie die Ausnahmen und Ausführungsreihenfolge von Aufruflisten besser überblicken. Über den [Microsoft-Symbolserver](http://msdl.microsoft.com/download/symbols) stehen Symbole für die meisten Windows-Varianten zur Verfügung. Für schnellere Offline-Lookups können Sie jedoch auch unter [Herunterladen von Windows-Symbolpaketen](http://aka.ms/winsymbols) heruntergeladen werden.

Wählen Sie zum Festlegen von Symboloptionen für Visual Studio **Extras > Optionen** aus, und navigieren Sie im Dialogfeld zu **Debuggen > Symbole**.

**Abbildung 4. Dialogfeld „Optionen“. **
            
![Dialogfeld „Optionen“](images/gs-debug-uwp-apps-004.png)

Legen Sie die **Sympath**-Variable auf dem Speicherort des Symbolpakets fest, um die Symbole in einer Debugsitzung mit [WinDbg](#windbg) zu laden. Der folgende Befehl lädt beispielsweise Symbole vom Microsoft-Symbolserver und speichert sie dann im Verzeichnis C:\Symbols zwischen:

```
.sympath SRV*C:\Symbols*http://msdl.microsoft.com/download/symbols
.reload
```

Mit dem „;“ als Trennzeichen oder dem `.sympath+`-Befehl können Sie mehrere Pfade hinzufügen. Mehr zu erweiterten Symbolvorgängen mit WinDbg finden Sie unter [Öffentliche und private Symbole](https://msdn.microsoft.com/library/windows/hardware/ff553493).

## WinDbg

WinDbg ist ein leistungsstarker Debugger, der als Teil der Debugtools für Windows bereitgestellt wird (Letzteres ist des [Windows SDK](http://go.microsoft.com/fwlink/p?LinkID=271979)). Die Windows-SDK-Installation ermöglicht die Installation der Debugtools für Windows als eigenständiges Produkt. WinDbg ist beim Debuggen von systemeigenem Code sehr hilfreich. Es wird jedoch nicht für Apps empfohlen, die in verwaltetem Code oder in HTML5 geschrieben wurden. 

Um WinDbg mit UWP-Apps zu verwenden, müssen Sie zuerst, wie im vorherigen Abschnitt beschrieben, über PLMDebug PLM für Ihr App-Paket deaktivieren. 

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



<!--HONumber=Jun16_HO4-->


