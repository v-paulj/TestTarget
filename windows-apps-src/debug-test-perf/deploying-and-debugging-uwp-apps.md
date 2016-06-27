---
author: mcleblanc
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: "Dieser Artikel führt Sie Schritt für Schritt durch die Ausrichtung Ihrer Apps auf verschiedene Bereitstellungs- und Debugziele."
title: Bereitstellen und Debuggen von UWP (Universelle Windows-Plattform)-Apps
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: eb639e78bf144572dfbfd2d65514bb4eff7c7be1

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

## Angeben eines Remotegeräts

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





<!--HONumber=Jun16_HO3-->


