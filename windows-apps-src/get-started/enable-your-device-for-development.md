---
author: GrantMeStrength
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: "Aktivieren Ihres Geräts für die Entwicklung"
description: "Konfigurieren Sie Ihr Windows10-Gerät für die Entwicklung und das Debugging."
keywords: "Gerät aktivieren"
translationtype: Human Translation
ms.sourcegitcommit: 6e8849b2ed067206ab14c4339f74c5219dcca16b
ms.openlocfilehash: 66413b43e5b9fd285324fd139fe14527dbee940a

---
# Aktivieren Ihres Geräts für die Entwicklung

Bevor Sie Apps schreiben können, müssen Sie auf Ihrem Entwicklungs-PC sowie auf allen anderen Geräten, auf denen Sie Ihren Code testen werden, den Entwicklermodus aktivieren.

## Verwenden von Entwicklerfeatures

### Entwickeln Ihrer App mit Microsoft Visual Studio

Sie müssen den Entwicklermodus auf Ihrem PC aktivieren, bevor Sie ein UWP-App-Projekt in Visual Studio öffnen können. Wenn Sie ein UWP-Projekt öffnen und der Entwicklermodus nicht aktiviert ist, wird automatisch die Einstellungsseite **Für Entwickler** geöffnet. Führen Sie die Schritte zum Aktivieren des Entwicklermodus im nächsten Abschnitt aus.

Wenn Sie ein UWP-App-Projekt in Visual Studio unter Windows10, Version 1511 oder älter öffnen, wird in Visual Studio dieses Dialogfeld angezeigt. 

![Dialogfeld zum Aktivieren des Entwicklermodus in Visual Studio](images/latestenabledialog.png)

Wenn dieses Dialogfeld angezeigt wird, klicken Sie auf **Einstellungen für Entwickler**, um die Einstellungsseite **Für Entwickler** zu öffnen, und aktivieren Sie den Entwicklermodus.

> Sie können jederzeit zur Seite **Für Entwickler** wechseln, um den Entwicklermodus zu aktivieren oder zu deaktivieren: Geben Sie einfach in der Taskleiste im Cortana-Suchfeld „Entwicklereinstellungen“ ein.

### Aktivieren Ihrer Windows10-Geräte

Sie können ein Gerät für die Entwicklung oder nur für das Querladen aktivieren.

-   *Querladen* dient zum Installieren und Ausführen oder Testen einer App, die nicht vom Windows Store zertifiziert wurde. Hierzu zählen beispielsweise interne Unternehmens-Apps.
-   Im *Entwicklermodus* können Sie Apps querladen und Apps aus Visual Studio im Debugmodus ausführen. 

    Wenn Sie den Entwicklermodus aktivieren, wird ein Paket mit Optionen installiert, das Folgendes enthält:
    - Installiert Windows Device Portal. Nur wenn die Option **Geräteportal aktivieren** aktiviert ist, ist das Geräteportal aktiviert und Firewallregeln werden für es konfiguriert.
    - Installiert, aktiviert und konfiguriert die Firewallregeln für SSH-Dienste, die eine Remote-Installation von Apps ermöglichen.
    - (Nur Desktop) Ermöglicht das Aktivieren des Windows-Subsystems für Linux. Weitere Informationen finden Sie unter [Über Bash on Ubuntu unter Windows](https://msdn.microsoft.com/commandline/wsl/about).

Nähere Informationen zu den Optionen finden Sie unter [Welche Einstellungen soll ich auswählen: Querladen von Apps oder Entwicklermodus?](https://msdn.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#which-settings-should-i-choose-sideload-apps-or-developer-mode)

**So verwenden Sie Entwicklerfeatures**

1.  Navigieren Sie auf dem zu aktivierenden Gerät zu **Einstellungen**. Wählen Sie **Update und Sicherheit** und dann **Für Entwickler** aus.
2.  Wählen Sie die geeignete Zugriffsebene aus: **Entwicklermodus** zum Entwickeln von UWP-Apps. 
3.  Lesen Sie den Haftungsausschluss für die ausgewählte Einstellung, und klicken Sie dann auf **Ja** um die Änderung zu übernehmen.

> [!NOTE]
> Wenn Ihr Gerät einem Unternehmen gehört, können von Ihrer Organisation einige Optionen deaktiviert werden, wie hier dargestellt.

Hier sehen Sie die Einstellungsseite auf Desktopgeräten.

![Navigieren Sie zu „Einstellungen > Update und Sicherheit“, und wählen Sie „Für Entwickler“ aus, um Ihre Optionen anzuzeigen.](images/devmode-pc-options.png)

Hier sehen Sie die Einstellungsseite auf Mobilgeräten.

![Navigieren Sie auf Ihrem Smartphone unter „Einstellungen“ zu „Update und Sicherheit“.](images/devmode-mob.png)

## Features im Entwicklermodus

Für jede Gerätefamilie können zusätzliche Entwicklerfeatures verfügbar sein. Diese Features sind nur verfügbar, wenn der Entwicklermodus auf dem Gerät aktiviert ist, und können sich abhängig von der verwendeten Betriebssystemversion unterscheiden.

Diese Abbildung zeigt Entwicklerfeatures für Mobilgeräte mit Windows 10, Version 1511.

![Entwicklermodusoptionen für Mobilgeräte](images/devmode-mob-options.png) 

### <span id="device-discovery-and-pairing"></span>Geräteportal

Weitere Informationen zur Gerätesuche und zum Device Portal finden Sie in der [Übersicht über das Windows Device Portal](../debug-test-perf/device-portal.md).

Gerätespezifische Anweisungen zum Einrichten finden Sie in folgenden Artikeln:
- [Geräteportal für Desktop](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [Geräteportal für HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Geräteportal für IoT](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [Geräteportal für Mobilgeräte](../debug-test-perf/device-portal-mobile.md)
- [Geräteportal für Xbox](../debug-test-perf/device-portal-xbox.md)

Wenn Probleme beim Aktivieren des Entwicklermodus oder des Geräteportals auftreten, finden Sie im Forum [Bekannte Probleme](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) Problemumgehungen. 

###SSH

Wenn Sie den Entwicklermodus auf Ihrem Gerät aktivieren, sind SSH-Dienste aktiviert.  Diese werden verwendet, wenn Ihr Gerät ein Bereitstellungsziel für UWP-Anwendungen ist.   Die Namen der Dienste sind „SSH Server Broker“ und „SSH Server Proxy“.

> [!NOTE]
> Dies ist nicht die OpenSSH-Implementierung von Microsoft, die Sie auf [GitHub](https://github.com/PowerShell/Win32-OpenSSH) finden.

Wenn Sie die SSH-Dienste nutzen möchten, können Sie die Gerätesuche aktivieren, um eine Pin-Kopplung zu ermöglichen. Wenn ein anderer SSH-Dienst ausgeführt werden soll, können Sie diesen auf einem anderen Anschluss einrichten oder die SSH-Dienste im Entwicklermodus deaktivieren. Um die SSH-Dienste zu deaktivieren, deaktivieren Sie einfach den Entwicklermodus.  

### Gerätesuche

Wenn Sie die Gerätesuche aktivieren, ist Ihr Gerät über mDNS für andere Geräte im Netzwerk sichtbar.  Dieses Feature ermöglicht auch das Abrufen der SSH-PIN zur Kopplung mit diesem Gerät.  

![PIN-Kopplung](images/devmode-pc-pinpair.PNG)

Die Gerätesuche sollte nur aktiviert werden, wenn das Gerät ein Bereitstellungsziel sein soll. Wenn beispielsweise eine App über das Geräteportal zum Testen auf einem Smartphone bereitgestellt wird, müssen Sie die Gerätesuche auf dem Smartphone, jedoch nicht auf Ihrem Entwicklungscomputer aktivieren.

### Fehlerberichterstattung (nur Mobile)

Legen Sie diesen Wert fest, um anzugeben, wie viele Absturzabbilder auf dem Smartphone gespeichert werden.

Indem Sie Absturzabbilder auf Ihrem Smartphone speichern, haben Sie direkt nach einem Absturz sofort Zugriff auf wichtige Informationen. Abbilder werden nur für von Entwicklern signierte Apps erfasst. Sie finden die Abbilder im Speicher Ihres Smartphones im Ordner „Documents\\Debug“. Weitere Informationen zu Abbilddateien finden Sie im Artikel zum [Verwenden von Dumpdateien](https://msdn.microsoft.com/library/d5zhxt22.aspx).

### Optimierungen für Windows-Explorer, Remotedesktop und PowerShell (nur Desktop)

 Auf der Desktopgerätefamilie finden Sie auf der Einstellungsseite **Für Entwickler** Verknüpfungen zu den Einstellungen, die Sie zum Optimieren Ihres PCs für Entwicklungsaufgaben verwenden können. Für jede Einstellung können Sie das Kontrollkästchen aktivieren und auf **Übernehmen** klicken, oder Sie können auf den Link **Einstellungen anzeigen** klicken, um die Einstellungsseite für diese Option zu öffnen. 

## Welche Einstellungen soll ich auswählen: Querladen von Apps oder Entwicklermodus?

Standardmäßig können Sie nur UWP-Apps (Universelle Windows-Plattform) aus dem Windows Store installieren. Wenn Sie diese Einstellungen ändern, um die Entwicklerfeatures zu verwenden, kann hierdurch die Sicherheitsstufe Ihres Geräts geändert werden. Sie sollten keine Apps aus nicht überprüften Quellen installieren.

### Querladen von Apps

Die Einstellung für das Querladen von Apps wird normalerweise von Unternehmen oder Bildungseinrichtungen verwendet, die benutzerdefinierte Apps auf verwalteten Geräten installieren müssen, ohne den Windows Store zu nutzen. In diesem Fall wird im Unternehmen häufig eine Richtlinie erzwungen, mit der die Einstellung *Windows Store-Apps* deaktiviert wird, wie oben in der Abbildung der Einstellungsseite dargestellt. Die Organisation stellt außerdem das erforderliche Zertifikat und den Installationsspeicherort zum Querladen von Apps bereit. Weitere Informationen finden Sie in den TechNet-Artikeln [Querladen von Branchen-Apps in Windows 10](https://technet.microsoft.com/library/mt269549.aspx) und [Erste Schritte mit der Bereitstellung von Apps in Microsoft Intune](https://technet.microsoft.com/library/dn646955.aspx).

Spezifische Informationen für Gerätefamilien

-   Desktopgeräte: Sie können ein App-Paket (APPX-Datei) und jedes Zertifikat, das zur Ausführung der App benötigt wird, mit dem Windows PowerShell-Skript installieren, das mit dem Paket erstellt wird („Add-AppDevPackage.ps1“). Weitere Informationen finden Sie unter [Verpacken von UWP-Apps](../packaging/packaging-uwp-apps.md).

-   Mobilgeräte: Wenn das erforderliche Zertifikat bereits installiert ist, tippen Sie auf die Datei, um beliebige APPX-Dateien zu installieren, die Sie über eine E-Mail oder eine SD-Karte erhalten haben.

Das **Querladen von Apps** ist eine sicherere Option als der Entwicklermodus, da Sie Apps ohne vertrauenswürdiges Zertifikat nicht auf dem Gerät installieren können.

> [!NOTE]
> Achten Sie beim Querladen von Apps darauf, dass diese von einer vertrauenswürdigen Quelle stammen. Wenn Sie eine quergeladene, nicht vom Windows Store zertifizierte App installieren, bestätigen Sie, dass Sie über alle erforderlichen Rechte zum Querladen dieser App verfügen und die alleinige Verantwortung für jegliche Schäden tragen, die durch das Installieren und Ausführen dieser App entstehen können. Weitere Informationen finden Sie in diesen [Datenschutzbestimmungen](http://go.microsoft.com/fwlink/?LinkId=521839) im Abschnitt zu Windows &gt; „Windows Store“.

### Entwicklermodus

Der Entwicklermodus ersetzt die Windows 8.1-Anforderungen durch eine Entwicklerlizenz.  Neben dem Querladen bietet der Entwicklermodus Debug- und zusätzliche Bereitstellungsoptionen. Hierzu gehört das Starten eines SSH-Diensts, der Bereitstellungen auf diesem Gerät ermöglicht. Um den Dienst zu beenden, müssen Sie den Entwicklermodus deaktivieren.

Spezifische Informationen für Gerätefamilien

-   Desktopgeräte:

    Aktivieren Sie den Entwicklermodus, um Apps in Visual Studio zu entwickeln und zu debuggen. Wie bereits beschrieben, wird in Visual Studio eine Meldung angezeigt, wenn der Entwicklermodus nicht aktiviert ist.

    Ermöglicht das Aktivieren des Windows-Subsystems für Linux. Weitere Informationen finden Sie unter [Über Bash on Ubuntu unter Windows](https://msdn.microsoft.com/commandline/wsl/about).

-   Mobilgeräte:

    Aktivieren Sie den Entwicklermodus, um Apps aus Visual Studio auf dem Gerät bereitzustellen und zu debuggen.

    Sie können auf die Datei tippen, um beliebige APPX-Dateien zu installieren, die Sie per E-Mail oder auf einer SD-Karte erhalten haben. Installieren Sie keine Apps aus nicht überprüften Quellen.

**Tipp**  
Es gibt verschiedene Tools, mit denen Sie eine App von einem Windows10-PC auf einem mobilen Gerät bereitstellen können. Beide Geräte müssen über eine kabelgebundene oder drahtlose Verbindung mit dem gleichen Subnetz des Netzwerks verbunden sein oder über USB verbunden werden. Mit beiden aufgeführten Methoden wird nur das App-Paket (APPX) installiert, nicht die Zertifikate.

-   Verwenden Sie das Tool für die Windows10-Anwendungsbereitstellung (WinAppDeployCmd). Weitere Informationen zum [Tool WinAppDeployCmd](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   Ab Windows10, Version 1511, ist im [Device Portal](#device_portal) die Bereitstellung aus Ihrem Browser auf einem mobilen Gerät unter Windows10, Version 1511 oder höher, möglich. Im Geräteportal können Sie auf der Seite **[Apps](../debug-test-perf/device-portal.md#apps)** ein App-Paket (APPX) hochladen und auf dem Gerät installieren.

## Verwenden von Gruppenrichtlinien oder Registrierungsschlüsseln zum Aktivieren von Geräten

Entwickler sollten meist die Einstellungs-Apps verwenden, um Geräte für das Debuggen zu aktivieren. In bestimmten Szenarien wie z.B. bei automatisierten Tests stehen weitere Möglichkeiten zum Aktivieren Ihres Windows10-Desktopgeräts für die Entwicklung zur Verfügung.

Sie können mithilfe von „gpedit.msc“ die Gruppenrichtlinien für die Geräteaktivierung festlegen (es sei denn, Sie verwenden Windows10 Home). In Windows10 Home müssen die Registrierungsschlüssel für die Geräteaktivierung direkt mithilfe von regedit oder von PowerShell-Befehlen festgelegt werden.

**Aktivieren des Geräts mithilfe von „gpedit“**

1.  Führen Sie **Gpedit.msc** aus.
2.  Navigieren Sie zu „Richtlinie für "Lokaler Computer" &gt; Computerkonfiguration &gt; Administrative Vorlagen &gt; Windows-Komponenten &gt; App-Paketbereitstellung.
3.  Bearbeiten Sie zum Aktivieren des Querladens die folgenden Richtlinien:

    -   **Zulassen, dass alle vertrauenswürdigen Apps installiert werden**

    - ODER

    Bearbeiten Sie zum Aktivieren des Entwicklermodus die Richtlinien, um beide zu aktivieren:

    -   **Zulassen, dass alle vertrauenswürdigen Apps installiert werden**
    -   **Hiermit wird die Entwicklung von Windows Store-Apps und Ihre Installation aus einer integrierten Entwicklungsumgebung (IDE) zugelassen.**

4.  Starten Sie Ihren Computer neu.

**Aktivieren des Geräts mithilfe von „regedit“**

1.  Führen Sie **regedit** aus.
2.  Legen Sie diesen DWORD-Wert auf 1 fest, um das Querladen zu aktivieren.

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - ODER

    Um den Entwicklermodus zu aktivieren, legen Sie diese DWORD-Werte auf 1 fest:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**Aktivieren des Geräts mithilfe von PowerShell**

1.  Führen Sie PowerShell mit Administratorrechten aus.
2.  Führen Sie zum Aktivieren des Querladens diesen Befehl aus:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - ODER

    Führen Sie zum Aktivieren des Entwicklermodus diesen Befehl aus:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## Aktualisieren Ihres Geräts von Windows 8.1 auf Windows 10

Wenn Sie Apps auf Ihrem Windows8.1-Gerät erstellen oder querladen, müssen Sie eine Entwicklerlizenz installieren. Beim Upgrade Ihres Geräts von Windows8.1 auf Windows10 bleiben diese Informationen erhalten. Führen Sie den folgenden Befehl aus, um diese Informationen von Ihrem aktualisierten Windows10-Gerät zu entfernen. Dieser Schritt ist nicht erforderlich, wenn Sie ein Upgrade von Windows8.1 direkt auf Windows10, Version1511 oder höher, ausführen.

**So heben Sie die Registrierung einer Entwicklerlizenz auf**

1.  Führen Sie PowerShell mit Administratorrechten aus.
2.  Führen Sie folgenden Befehl aus: **unregister-windowsdeveloperlicense**.

Danach müssen Sie Ihr Gerät wie in diesem Thema beschrieben für die Entwicklung aktivieren, damit Sie weiterhin auf diesem Gerät entwickeln können. Andernfalls erhalten Sie möglicherweise eine Fehlermeldung, wenn Sie Ihre App debuggen oder versuchen, ein Paket dafür zu erstellen. Hier ist ein Beispiel für diesen Fehler:

Fehler : DEP0700 : Registrierung der App fehlgeschlagen.



<!--HONumber=Aug16_HO5-->


