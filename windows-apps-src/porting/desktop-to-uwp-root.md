---
author: awkoren
Description: "Bereiten Sie die Windows-Desktopanwendung (z. B. Win32, WPF und Windows Forms) für die Konvertierung in eine UWP-App (Universelle Windows-Plattform) mithilfe der Desktop-Konvertierungserweiterungen vor."
Search.Product: eADQiWindows 10XVcnh
title: Konvertieren der Desktopanwendung in eine UWP-App (Universelle Windows-Plattform)
translationtype: Human Translation
ms.sourcegitcommit: ff8cd3ab5e38cfc3a2b5fabaad15f78a5f2620f2
ms.openlocfilehash: 6cf6367ed0f6acea0f87ac36a050e874425423b1

---

# Konvertieren der Desktopanwendung in eine UWP-App (Universelle Windows-Plattform)

\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

Bereiten Sie die Windows-Desktopanwendung (z. B. Win32, WPF und Windows Forms) für die Konvertierung in eine UWP-App (Universelle Windows-Plattform) mithilfe der Desktop-Konvertierungserweiterungen vor.

## Vorteile der Konvertierung der Anwendung in eine UWP-App

Die Universelle Windows-Plattform (UWP) stellt mithilfe der Desktop-Konvertierungserweiterungen eine Brücke dar, mit der Sie eine Windows-Desktopanwendung (z.B. Win32, Windows Forms und WPF) bzw. ein Spiel in eine UWP-App bzw. ein UWP-Spiel konvertieren können. Weitere Informationen finden Sie unter [Anleitung für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx). Nach der Konvertierung wird die Windows-Desktopanwendung gepackt, gewartet und als UWP-App-Paket (eine APPX- oder APPXBUNDLE-Datei) für Windows10 Desktop bereitgestellt.

Es gibt zwei Bestandteile der Technologie, die das Konvertieren von Desktop-Apps in UWP-Pakete ermöglichen. Der erste Bestandteil ist der Desktop-App-Konverter, mit dem Ihre vorhandenen Binärdateien neu als UWP-Paket gepackt werden. Der Code wird beibehalten, er wird nur anders gepackt. Der zweite Bestandteil umfasst Laufzeittechnologien im Windows Anniversary-Update, mit denen die im UWP-Paket enthaltenen ausführbaren Dateien, statt in einem App-Container, als vertrauenswürdige Dateien ausgeführt werden. Diese Technologie fügt außerdem einer konvertierten App eine Paketidentität hinzu, die zur Verwendung einiger UWP-APIs erforderlich ist.

Im Folgenden werden einige Vorteile der Konvertierung von Windows-Desktopanwendungen aufgeführt.

* Eine gleichmäßige Installationsumgebung der App für Ihre Kunden. Sie können über das Querladen für Computer bereitgestellt werden (siehe [Querladen von Branchen-Apps in Windows 10](https://technet.microsoft.com/library/mt269549.aspx)) und hinterlassen nach der Deinstallation keine Spuren. Langfristig können Sie Ihre App auch im Windows Store veröffentlichen.

* Da die konvertierte App Paketidentität aufweist, können Sie mehr UWP-APIs aufrufen als zuvor, auch diejenigen aus der vertrauenswürdigen Partition. Eine vollständige Liste finden Sie unter [Unterstützte UWP-APIs für konvertierte Desktop-Apps](desktop-to-uwp-supported-api.md). 

* Sie können in Ihrem eigenen Tempo Ihrem App-Paket UWP-Features hinzufügen, beispielsweise XAML-Benutzeroberfläche, Livekachelaktualisierungen, UWP-Hintergrundaufgaben, App-Dienste und vieles mehr. Die gesamte Funktionalität, die für alle anderen UWP-Apps verfügbar ist, steht auch für Ihre App zur Verfügung.

* Wenn Sie die gesamte Funktionalität der App aus der vertrauenswürdigen Partition in die App-Containerpartition verschieben, kann Ihre App auf allen Windows 10-Geräten ausgeführt werden.

* Ihre App kann als UWP-App die gleichen Aufgaben ausführen wie als Windows-Desktopanwendung. Sie interagiert mit einer virtualisierten Ansicht der Registrierung und des Dateisystems, die von der tatsächlichen Registrierung und dem Dateisystem nicht zu unterscheiden ist.

* Ihre App kann an der im Windows Store integrierten Lizenzierung und automatischen Updatemöglichkeiten teilnehmen. Automatische Updates stellen einen sehr zuverlässigen und effizienten Mechanismus dar, da nur die geänderten Teile von Dateien heruntergeladen werden.

## Vorbereiten von Desktop-Apps für die Konvertierung in UWP-Apps

Sie müssen möglicherweise nicht viel tun, um Ihre App für die Konvertierung vorzubereiten. Denken Sie daran, dass Lizenzierung und automatische Updates im Rahmen des Windows Stores erfolgen, sodass Sie diese Features aus Ihrer Codebasis entfernen können. Wenn eine dieser Situationen auf Ihre Anwendung zutrifft, müssen Sie dieses Problems vor der Konvertierung beheben.

+ __Ihre App verwendet eine frühere .NET-Version als 4.6.1__. Es wird nur .NET 4.6.1 unterstützt. Sie müssen Ihre App vor dem Konvertieren für .NET 4.6.1 neu zuweisen. 

+ __Ihre App wird immer mit erhöhten Sicherheitsberechtigungen ausgeführt__. Ihre App muss als interaktiver Benutzer ausgeführt werden können. Benutzer, die Ihre App über den Windows Store installieren, sind möglicherweise keine Systemadministratoren. Wenn für die Ausführung Ihrer App erhöhte Rechte erforderlich sind, wird die App für Standardbenutzer nicht korrekt ausgeführt.

+ __Ihre App erfordert einen Kernelmodustreiber oder einen Windows-Dienst__. Die Brücke eignet sich für eine App, ein Kernelmodustreiber oder ein Windows-Dienst, die unter einem Systemkonto ausgeführt werden müssen, werden jedoch nicht unterstützt. Verwenden Sie anstelle eines Windows-Diensts eine [Hintergrundaufgabe](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Ihre App-Module werden im Prozess für Prozesse geladen, die nicht im APPX-Paket enthalten sind__. Dies ist nicht zulässig. Prozessinterne Erweiterungen, beispielsweise [Shellerweiterungen](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), werden nicht unterstützt. Wenn zwei Apps in einem Paket enthalten sind, können Sie jedoch die prozessübergreifende Kommunikation zwischen diesen verwenden.

+ __Ihre App ruft [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) oder [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__ auf. Diese Funktionen werden derzeit für konvertierte Apps nicht unterstützt. Die Unterstützung ist in einer zukünftigen Version enthalten. Um dieses Problem zu umgehen, können Sie all jene DLLs in das Paketstammverzeichnis kopieren, die mit diesen Funktionen gefunden wurden. 

+ __Ihre App verwendet eine benutzerdefinierte Anwendungsbenutzermodell-ID (Application User Model ID, AUMID)__. Wenn der Prozess [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) zum Festlegen einer eigenen Anwendungsbenutzermodell-ID aufruft, kann er nur die von der App-Modellumgebung/vom AppX-Paket generierte Anwendungsbenutzermodell-ID verwenden. Sie können keine benutzerdefinierten Anwendungsbenutzermodell-IDs definieren.

+ __Ihre App ändert die HKLM-Registrierungsstruktur (HKEY_LOCAL_MACHINE)__. Bei jedem Versuch, einen HKLM-Schlüssel zu erstellen oder einen zum Ändern zu öffnen, wird der Zugriff verweigert. Denken Sie daran, dass Ihre App über eine eigene private virtualisierte Ansicht der Registrierung verfügt. Damit kann das Konzept einer benutzer- und computerweiten Registrierungsstruktur (Zweck von HKLM) nicht angewendet werden. Sie müssen stattdessen eine andere Möglichkeit finden, um das zu erreichen, wozu Sie HKLM verwendet haben, beispielsweise Schreiben in HKEY_CURRENT_USER (HKCU).

+ __Ihre App verwendet einen DDEEXEC-Registrierungsunterschlüssel zum Starten einer anderen App__. Verwenden Sie stattdessen einen der DelegateExecute-Verbhandler genau wie von den verschiedenen Activatable*-Erweiterungen in dem [App-Paketmanifest](https://msdn.microsoft.com/library/windows/apps/br211474.aspx) konfiguriert.

+ __Ihre App schreibt in den AppData-Ordner, um Daten für eine andere App freizugeben__. Nach der Konvertierung wird AppData an den lokalen App-Datenspeicher weitergeleitet, der ein privater Speicher für jede UWP-App ist. Verwenden Sie verschiedene Methoden für die prozessübergreifende Datenfreigabe. Weitere Informationen finden Sie unter [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Ihre App schreibt in das Installationsverzeichnis für die App__. Ihre App schreibt z. B. in eine Protokolldatei, die sich in demselben Verzeichnis wie die EXE-Datei befindet. Dies wird nicht unterstützt. Sie müssen daher einen anderen Speicherort wählen, z. B. den lokalen App-Datenspeicher.

+ __Ihre App-Installation erfordert eine Benutzerinteraktion__. Der App-Installer muss automatisch ausgeführt werden können und muss alle erforderlichen Komponenten installieren, die nicht standardmäßig auf einem sauberen Betriebssystemimage aktiviert sind.

+ __Ihre App verwendet das aktuelle Arbeitsverzeichnis__. Zur Laufzeit nutzt die konvertierte App nicht das gleiche Arbeitsverzeichnis, das Sie zuvor in Ihrer Desktop-LNK-Verknüpfung angegeben haben. Sie müssen das aktuelle Arbeitsverzeichnis zur Laufzeit ändern, wenn das richtige Verzeichnis notwendig ist, damit Ihre App ordnungsgemäß funktioniert.

+ __Ihre App benötigt UIAccess__. Wenn Ihre Anwendung `UIAccess=true` für das `requestedExecutionLevel`-Element im UAC-Manifest angibt, wird die Konvertierung in eine UWP-App derzeit nicht unterstützt. Weitere Informationen finden Sie unter [Sicherheit bei der Benutzeroberflächenautomatisierung – Übersicht](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Ihre App ist bereits eine vollständige UWP-App und versucht, im App-Paket einen vollständig vertrauenswürdigen Prozess aufzurufen__. Das „umgekehrte“ Verwenden der Brücke wird nicht unterstützt, und UWP-Apps, die versuchen, vertrauenswürdige Prozesse aufzurufen, erhalten keine Store-Zertifizierung. Um dieses Szenario in einer quergeladenen App zu aktivieren, schließen Sie zunächst die ```<desktop:Extension>```-Deklaration in das App-Manifest ein (einschließlich des *EntryPoint*-Attributs mit dem Wert *Windows.FullTrustApplication*). Setzen Sie anschließend ```<TargetDeviceFamily>``` für die App auf *Windows.Desktop*. Dies bedeutet, dass die App nur auf die Desktop-Gerätefamilie, nicht jedoch auf alle UWP-Geräte abzielt. Wenn Ihre App mit diesen Einstellungen den Fehler *APPX0501: Überprüfungsfehler* zurückgibt, überprüfen Sie Ihre Zielgerätefamilie. Ihre App zielt möglicherweise auf die Familie *Windows.Universal* und nicht auf *Windows.Desktop* ab. 

+ __Ihre App macht COM-Objekte oder GAC-Assemblys zur Verwendung durch andere Prozesse verfügbar__. In der aktuellen Version kann Ihre App COM-Objekte oder GAC-Assemblys nicht zur Verwendung durch Prozesse verfügbar machen, die von ausführbaren Dateien stammen, die sich außerhalb Ihres AppX-Pakets befinden. Prozesse aus dem Paket können COM-Objekte und GAC-Assemblys wie gewohnt registrieren und verwenden, aber sie sind nicht extern sichtbar. Dies bedeutet, dass Interoperabilitätsszenarien wie OLE nicht funktionieren, wenn sie von externen Prozessen aufgerufen werden. 

+ __Ihre App verknüpft C-Laufzeitbibliotheken auf eine nicht unterstützte Weise__. Die Microsoft C/C++-Laufzeitbibliothek enthält Routinen für die Programmierung für das Microsoft Windows-Betriebssystem. Diese Routinen automatisieren viele allgemeine Programmieraufgaben, die von den Programmiersprachen C# und C++ nicht bereitgestellt werden. Wenn Ihre App eine C/C++-Laufzeitbibliothek verwendet, müssen Sie sicherstellen, dass sie auf eine unterstützte Weise verknüpft ist. 
    
    Visual Studio 2015 unterstützt die dynamische Verknüpfung, damit Ihr Code gängige DLL-Dateien verwenden kann, und statische Verknüpfung, um die Bibliothek direkt in Ihren Code, mit der aktuellen Version der CRT, zu verknüpfen. Wir empfehlen, dass Ihre App möglichst die dynamische Verknüpfung mit VS 2015 verwendet. 

    Unterstützung in früheren Versionen von Visual Studio variiert. Weitere Informationen finden Sie in der folgenden Tabelle: 

    <table>
    <th>Version von Visual Studio</td><th>Dynamische Verknüpfung</th><th>Statische Verknüpfung</th></th>
    <tr><td>2005 (VC 8)</td><td>Nicht unterstützt</td><td>Unterstützt</td>
    <tr><td>2008 (VC 9)</td><td>Nicht unterstützt</td><td>Unterstützt</td>
    <tr><td>2010 (VC 10)</td><td>Unterstützt</td><td>Unterstützt</td>
    <tr><td>2012 (VC 11)</td><td>Unterstützt</td><td>Nicht unterstützt</td>
    <tr><td>2013 (VC 12)</td><td>Unterstützt</td><td>Nicht unterstützt</td>
    <tr><td>2015 (VC 14)</td><td>Unterstützt</td><td>Unterstützt</td>
    </table>
    
    Hinweis: In allen Fällen müssen Sie eine Verknüpfung zur neuesten öffentlich verfügbaren CRT herstellen.

+ __Ihre App installiert und lädt Assemblys aus dem Windows-Seite-an-Seite-Ordner__. Beispielsweise verwendet Ihre App C-Laufzeitbibliotheken VC8 oder VC9 und verknüpft diese dynamisch aus dem Windows-Seite-an-Seite-Ordner, was bedeutet, dass Ihr Code die gemeinsamen DLL-Dateien aus einem freigegebenen Ordner verwendet. Dies wird nicht unterstützt. Sie müssen diese statisch verknüpfen, indem sie eine Verknüpfung mit den verteilbaren Bibliotheksdateien direkt in den Code erstellen.

## Starten des Konvertierungsprozesses

Sie verfügen über eine Reihe von Optionen für das Konvertieren der App.

* **Desktop App Converter (DAC)**. DAC ist ein Tool, das Ihre App automatisch konvertiert und signiert. DAC ist praktisch und automatisch, und es ist hilfreich, wenn die App zahlreiche Systemänderungen vornimmt, oder wenn nicht vollständig klar ist, was das Installationsprogramm durchführt. Die ersten Schrittemit DAC finden Sie unter [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md). 

* **Manuelles Konvertieren**. Wenn Ihre App mit XCOPY installiert wurde oder Ihnen die Änderungen bekannt sind, die das Installationsprogramm am System vornimmt, kann das manuelle Konvertieren die unkomplizierteste Wahl sein. Diese Option umfasst das Erstellen einer Manifestdatei, das Ausführen des Tools MakeAppx.exe sowie das Signieren des App-Pakets. Weitere Informationen zum manuellen Konvertieren finden Sie unter [Manuelles Konvertieren Ihrer Windows-Desktopanwendung in eine UWP-App (Universelle Windows-Plattform)](desktop-to-uwp-manual-conversion.md). 

* **Installationsprogramme von Drittanbietern**. Drei beliebte Installationsprogramme von Drittanbietern ([InstallShield von Flexera](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer), [WiX von FireGiant](https://www.firegiant.com/r/appx) und [Advanced Installer von Caphyon](http://www.advancedinstaller.com/uwp-app-package)) unterstützen jetzt die Desktop-Brücke und können mit wenigen Klicks sowohl ein MSI-Installationsprogramm als auch ein konvertiertes App-Paket generieren. Weitere Informationen finden Sie auf den Websites der einzelnen Installationsprogramme. 

## Support erhalten und Feedback geben

Wenn beim Konvertieren Ihrer App Probleme auftreten, finden Sie Hilfe in den [Foren](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Wenn Sie Feedback oder Verbesserungsvorschläge haben, sehen Sie sich [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial) an. 

## Inhalt dieses Abschnitts

| Thema | Beschreibung |
|-------|-------------|
| [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | Beinhaltet Informationen zum Ausführen von Desktop App Converter. Sie müssen wahrscheinlich nicht viel tun, um Ihre App für die Konvertierung vorzubereiten. |
| [Manuelles Konvertieren von Desktop-Apps](desktop-to-uwp-manual-conversion.md) | Enthält Informationen zum manuellen Erstellen eines App-Pakets und -Manifests. |
| [Erweiterungen für konvertierte Desktop-Apps](desktop-to-uwp-extensions.md) | Optimieren Sie die konvertierte App mit Erweiterungen, um Features wie z.B. Startaufgaben, Datei-Explorer-Integration und vieles mehr zu ermöglichen. |
| [Unterstützte UWP-APIs für konvertierte Desktop-Apps](desktop-to-uwp-supported-api.md) | Erfahren Sie, welche UWP-APIs für Ihre konvertierte Desktop-App verfügbar sind. |
| [Signieren der konvertierten Desktop-App](desktop-to-uwp-signing.md) | Erfahren Sie, wie Sie Ihr konvertiertes APPX-Paket mit einem Zertifikat signieren können. |
| [Bereitstellen und Debuggen der konvertierten UWP-App](desktop-to-uwp-deploy-and-debug.md) | Hier erhalten Sie Hilfe zum Bereitstellen und Debuggen Ihrer App nach dem Konvertieren. In diesem Thema finden Sie auch Informationen zu einigen internen Aspekten der Desktop-Konvertierungserweiterungen. |
| [Desktop-App-Bridge zu UWP-Codebeispielen](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Codebeispiele auf GitHub veranschaulichen Funktionen konvertierter Apps. |


<!--HONumber=Sep16_HO3-->


