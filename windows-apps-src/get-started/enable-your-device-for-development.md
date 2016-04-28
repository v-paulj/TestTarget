---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Aktivieren Ihres Geräts für die Entwicklung
description: Bei Windows 10-Geräten wird ein anderer Entwicklungsansatz verfolgt.
keywords: Erste Schritte
keywords: Entwicklerlizenz
keywords: Visual Studio, Entwicklerlizenz
keywords: Gerät aktivieren
---
# Aktivieren Ihres Geräts für die Entwicklung

Bei Windows 10-Geräten wird ein anderer Entwicklungsansatz verfolgt. Sie benötigen nun nicht mehr für jedes Gerät, das Sie zum Entwickeln, Installieren oder Testen Ihrer App verwenden möchten, eine Entwicklerlizenz. Stattdessen muss ein Gerät nur noch einmalig in den Geräteeinstellungen für diese Aufgaben aktiviert werden. Das war's. Sie müssen also nicht mehr alle 30 oder 90 Tage Ihre Entwicklerlizenzen verlängern!

Wenn Sie zum Entwickeln oder Testen Ihrer Apps mit Microsoft Visual Studio 2013 oder Microsoft Visual Studio 2015 noch ein Windows 8.1-Gerät verwenden, müssen Sie weiterhin [eine Entwicklerlizenz anfordern](https://msdn.microsoft.com/library/windows/apps/Hh974578) oder Ihr [Windows Phone registrieren](https://msdn.microsoft.com/library/windows/apps/Dn614128).

## Verwenden von Entwicklerfeatures

### Entwickeln Ihrer App mit Microsoft Visual Studio

Wenn Sie Microsoft Visual Studio auf einem Windows 10-Gerät verwenden und eine Projektmappe für eine Windows 8.1- oder Windows 10-App öffnen, werden Sie im folgenden Dialogfeld zum Aktivieren Ihres Geräts aufgefordert. Das Gerät muss für die Verwendung der Designer sowie zum Debuggen Ihrer App aktiviert sein.

![Dialogfeld zum Aktivieren des Entwicklermodus in Visual Studio](images/latestenabledialog.png)

Klicken Sie in diesem Dialogfeld auf **Einstellungen für Entwickler**, um direkt die Seite **Update und Sicherheit** aufzurufen, wie unten dargestellt. Sie könne auch auf **OK** klicken und anschließend die folgenden Schritte ausführen, um Ihr Windows 10-Gerät für die Entwicklung zu aktivieren.

### Aktivieren Ihrer Windows 10-Geräte

Unter Windows 10 können Sie wählen, welche Entwicklerfeatures Sie auf dem Gerät aktivieren möchten. Dies gilt für sämtliche Windows 10-Geräte – also sowohl für Desktops als auch für Tablets und Smartphones. Sie können ein Gerät für die Entwicklung oder nur für das Querladen aktivieren.

-   *Querladen* dient zum Installieren und anschließendem Ausführen oder Testen einer App, die nicht vom Windows Store zertifiziert wurde. Hierzu zählen beispielsweise interne Unternehmens-Apps.
-   Im *Entwicklermodus* können Sie Apps querladen und Apps aus Visual Studio im Debugmodus ausführen.

**Hinweis**  Achten Sie beim Querladen von Apps darauf, dass diese aus einer vertrauenswürdigen Quelle stammen. Wenn Sie eine quergeladene, nicht vom Windows Store zertifizierte App installieren, bestätigen Sie, dass Sie über alle erforderlichen Rechte zum Querladen dieser App verfügen und die alleinige Verantwortung für jegliche Schäden tragen, die durch das Installieren und Ausführen dieser App entstehen können. Weitere Informationen finden Sie in diesen [Datenschutzbestimmungen](http://go.microsoft.com/fwlink/?LinkId=521839) im Abschnitt zu Windows unter „Windows Store“.

**So verwenden Sie Entwicklerfeatures**

1.  Navigieren Sie auf dem zu aktivierenden Gerät zu **Einstellungen**. Wählen Sie **Update und Sicherheit** und dann **Für Entwickler** aus.
2.  Wählen Sie die erforderliche Zugriffsstufe aus. Nähere Informationen zu den Optionen finden Sie unter [Welche Einstellungen soll ich auswählen: Querladen von Apps oder Entwicklermodus?](#WhichSettings)
3.  Lesen Sie den Haftungsausschluss für die ausgewählte Einstellung, und klicken Sie dann auf **Ja** um die Änderung zu übernehmen.

Hier sehen Sie die Einstellungsseite auf Desktopgeräten.

![Navigieren Sie zu „Einstellungen“ > „Update and security“ (Update und Sicherheit), und wählen Sie „For developers“ (Für Entwickler), um Ihre Optionen anzuzeigen.](images/devmode-pc-options.png)

Hier sehen Sie die Einstellungsseite auf Mobilgeräten.

![Navigieren Sie auf Ihrem Smartphone unter „Einstellungen“ zu „Update and security“ (Update und Sicherheit).](images/devmode-mob.png)

### Welche Einstellungen soll ich auswählen: Querladen von Apps oder Entwicklermodus?

Standardmäßig können Sie nur UWP-Apps (Universelle Windows-Plattform) aus dem Windows Store installieren. Wenn Sie diese Einstellungen ändern, um die Entwicklerfeatures zu verwenden, kann hierdurch die Sicherheitsstufe Ihres Geräts geändert werden. Sie sollten keine Apps aus nicht überprüften Quellen installieren.

**Querladen von Apps**

Die Einstellung für das Querladen von Apps wird normalerweise von Unternehmen oder Bildungseinrichtungen verwendet, die benutzerdefinierte Apps auf verwalteten Geräten installieren müssen, ohne den Windows Store zu nutzen. In diesem Fall wird in der Organisation häufig eine Richtlinie erzwungen, mit der die Einstellung *Windows Store-Apps* deaktiviert wird, wie oben in der Abbildung der Einstellungsseite für Smartphones dargestellt. Die Organisation stellt außerdem das erforderliche Zertifikat und den Installationsspeicherort zum Querladen von Apps bereit. Weitere Informationen finden Sie in den TechNet-Artikeln [Querladen von Branchen-Apps in Windows 10](https://technet.microsoft.com/library/mt269549.aspx) und [Erste Schritte mit der Bereitstellung von Apps in Microsoft Intune](https://technet.microsoft.com/library/dn646955.aspx).

Spezifische Informationen für Gerätefamilien

-   Desktopgeräte: Sie können ein App-Paket (APPX-Datei) und jedes Zertifikat, das zur Ausführung der App benötigt wird, mit dem Windows PowerShell-Skript installieren, das mit dem Paket erstellt wird („Add-AppDevPackage.ps1“).

-   Mobilgeräte: Wenn das erforderliche Zertifikat bereits installiert ist, tippen Sie auf die Datei, um beliebige APPX-Dateien zu installieren, die Sie über eine E-Mail oder eine SD-Karte erhalten haben.

Das **Querladen von Apps** ist eine sicherere Option als der Entwicklermodus, da Sie Apps ohne vertrauenswürdiges Zertifikat nicht auf dem Gerät installieren können.

**Entwicklermodus**

Neben dem Querladen bietet die Einstellung für den Entwicklermodus Debuggen und zusätzliche Bereitstellungsoptionen. Er ersetzt die Windows 8.1-Anforderung für eine Entwicklerlizenz.

Spezifische Informationen für Gerätefamilien

-   Desktopgeräte:

    Aktivieren Sie den Entwicklermodus, um Apps in Visual Studio zu entwickeln und zu debuggen. Wie bereits beschrieben, wird in Visual Studio eine Meldung angezeigt, wenn der Entwicklermodus nicht aktiviert ist.

-   Mobilgeräte:

    Aktivieren Sie den Entwicklermodus, um Apps aus Visual Studio auf dem Gerät bereitzustellen und zu debuggen.

    Sie können auf die Datei tippen, um beliebige APPX-Dateien zu installieren, die Sie per E-Mail oder auf einer SD-Karte erhalten haben. Installieren Sie keine Apps aus nicht überprüften Quellen.

**Tipp**  
Es gibt verschiedene Tools, mit denen Sie eine App von einem Windows 10-PC auf einem mobilen Gerät bereitstellen können. Beide Geräte müssen über eine kabelgebundene oder drahtlose Verbindung mit dem gleichen Subnetz des Netzwerks verbunden sein oder über USB verbunden werden. Mit beiden aufgeführten Methoden wird nur das App-Paket (APPX) installiert, nicht die Zertifikate.

-   Verwenden Sie das Tool für die Windows 10-Anwendungsbereitstellung (WinAppDeployCmd). Weitere Informationen zum [Tool WinAppDeployCmd](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   Ab Windows 10, Version 1511, ist im [Geräteportal](#device_portal) die Bereitstellung aus Ihrem Browser auf einem mobilen Gerät unter Windows 10, Version 1511 oder höher, möglich. Auf der Seite **Apps** im Geräteportal (&lt;IP&gt;/appmanager.md) können Sie ein App-Paket (APPX) hochladen und auf dem Gerät installieren.

 

### Festlegen von Gruppenrichtlinien oder Registrierungsschlüsseln

Alternativ können Sie auch Gruppenrichtlinien oder Registrierungsschlüssel zum Aktivieren Ihres Windows 10-Desktopgeräts für die Entwicklung verwenden.

**Desktopgeräte**

Legen Sie mithilfe von „gpedit.msc“ die Gruppenrichtlinien für die Geräteaktivierung fest (es sei denn, Sie verwenden Windows 10 Home). In Windows 10 Home müssen die Registrierungsschlüssel für die Geräteaktivierung direkt mithilfe von regedit oder von PowerShell-Befehlen festgelegt werden.

**Aktivieren des Geräts mithilfe von „gpedit“**

1.  Führen Sie **Gpedit.msc** aus.
2.  Navigieren Sie zu Richtlinie für "Lokaler Computer" > Computerkonfiguration > Administrative Vorlagen > Windows-Komponenten > App-Paketbereitstellung.
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

## Features des Entwicklermodus

Für jede Gerätefamilie können zusätzliche Entwicklerfeatures verfügbar sein. Diese Features sind nur verfügbar, wenn der **Entwicklermodus** auf dem Gerät aktiviert ist, und können sich abhängig von der verwendeten Betriebssystemversion unterscheiden.

Diese Abbildung zeigt Entwicklerfeatures für Mobilgeräte unter Windows 10, Version 1511.

![Entwicklermodusoptionen für Mobilgeräte](images/devmode-mob-options.png)

### <span id="device-discovery-and-pairing"></span>Geräteerkennung und Geräteportal

Weitere Informationen zur Geräteerkennung und zum Geräteportal finden Sie in der [Übersicht über das Windows-Geräteportal](../debug-test-perf/device-portal.md).

Gerätespezifische Anweisungen zum Einrichten finden Sie in folgenden Artikeln:
- [Geräteportal für HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Geräteportal für IoT](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [Geräteportal für mobile Geräte](../debug-test-perf/device-portal-mobile.md)
- [Geräteportal für Xbox](../debug-test-perf/device-portal-xbox.md)

### Fehlerberichterstattung

Legen Sie diesen Wert fest, um anzugeben, wie viele Absturzabbilder auf dem Smartphone gespeichert werden.

Indem Sie Absturzabbilder auf Ihrem Smartphone speichern, haben Sie direkt nach einem Absturz sofort Zugriff auf wichtige Informationen. Abbilder werden nur für von Entwicklern signierte Apps erfasst. Sie finden die Abbilder im Speicher Ihres Smartphones im Ordner Documents\\Debug. Weitere Informationen zu Abbilddateien finden Sie im Artikel zum [Verwenden von Dumpdateien](https://msdn.microsoft.com/library/d5zhxt22.aspx).

## Aktualisieren Ihres Geräts von Windows 8.1 auf Windows 10

Wenn Sie Apps auf Ihrem Windows 8.1-Gerät erstellen oder querladen, müssen Sie eine Entwicklerlizenz installieren. Beim Upgrade Ihres Geräts von Windows 8.1 auf Windows 10 bleiben diese Informationen erhalten. Führen Sie den folgenden Befehl aus, um diese Informationen von Ihrem aktualisierten Windows 10-Gerät zu entfernen. Dieser Schritt ist nicht erforderlich, wenn Sie ein Upgrade von Windows 8.1 direkt auf Windows 10, Version 1511 oder höher, ausführen.

**So heben Sie die Registrierung einer Entwicklerlizenz auf**

1.  Führen Sie PowerShell mit Administratorrechten aus.
2.  Führen Sie folgenden Befehl aus: **unregister-windowsdeveloperlicense**.

Danach müssen Sie Ihr Gerät wie in diesem Thema beschrieben für die Entwicklung aktivieren, damit Sie weiterhin auf diesem Gerät entwickeln können. Andernfalls erhalten Sie möglicherweise eine Fehlermeldung, wenn Sie Ihre App debuggen oder versuchen, ein Paket dafür zu erstellen. Hier ist ein Beispiel für diesen Fehler:

Fehler : DEP0700 : Registrierung der App fehlgeschlagen.




<!--HONumber=Mar16_HO5-->


