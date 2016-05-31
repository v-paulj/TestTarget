---
author: awkoren
Description: Bereiten Sie die Windows-Desktopanwendung (z. B. Win32, WPF und Windows Forms) für die Konvertierung in eine UWP-App (Universelle Windows-Plattform) mithilfe der Desktop-Konvertierungserweiterungen vor.
Search.Product: eADQiWindows 10XVcnh
title: Konvertieren der Desktopanwendung in eine UWP-App (Universelle Windows-Plattform)
---

# Konvertieren der Desktopanwendung in eine UWP-App (Universelle Windows-Plattform)

\[Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

Bereiten Sie die Windows-Desktopanwendung (z. B. Win32, WPF und Windows Forms) für die Konvertierung in eine UWP-App (Universelle Windows-Plattform) mithilfe der Desktop-Konvertierungserweiterungen vor.

## Vorteile der Konvertierung der Anwendung in eine UWP-App

Die Universelle Windows-Plattform (UWP) stellt mithilfe der Desktop-Konvertierungserweiterungen eine Brücke dar, mit der Sie eine klassische Desktopanwendung (z. B. Win32, Windows Forms und WPF) bzw. ein Spiel in eine UWP-App bzw. ein UWP-Spiel konvertieren können. Weitere Informationen finden Sie unter [Anleitung für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx). Nach der Konvertierung wird die klassische Desktop-App gepackt, gewartet und als UWP-App-Paket (eine Appx- oder Appxbundle-Datei) für Windows 10 Desktop bereitgestellt.

Es gibt zwei Bestandteile der Technologie, die das Konvertieren von Desktop-Apps in UWP-Pakete ermöglichen. Der erste Bestandteil ist der Desktop-App-Konverter, mit dem Ihre vorhandenen Binärdateien neu als UWP-Paket gepackt werden. Der Code wird beibehalten, er wird nur anders gepackt. Der zweite Bestandteil umfasst Laufzeittechnologien im Windows Anniversary-Update, mit denen die im UWP-Paket enthaltenen ausführbaren Dateien, statt in einem App-Container, als vertrauenswürdige Dateien ausgeführt werden. Diese Technologie fügt außerdem einer konvertierten App eine Paketidentität hinzu, die zur Verwendung einiger UWP-APIs erforderlich ist.

Im Folgenden werden einige Vorteile der Konvertierung klassischer Desktop-Apps aufgeführt.
* Eine gleichmäßige Installationsumgebung der App für Ihre Kunden. Sie können über das Querladen für Computer bereitgestellt werden (siehe [Querladen von Branchen-Apps in Windows 10](https://technet.microsoft.com/library/mt269549.aspx)) und hinterlassen nach der Deinstallation keine Spuren. Langfristig können Sie Ihre App auch im Windows Store veröffentlichen.
* Da die konvertierte App Paketidentität aufweist, können Sie mehr UWP-APIs aufrufen als zuvor, auch diejenigen aus der vertrauenswürdigen Partition.
* Sie können in Ihrem eigenen Tempo Ihrem App-Paket UWP-Features hinzufügen, beispielsweise XAML-Benutzeroberfläche, Livekachelaktualisierungen, UWP-Hintergrundaufgaben, App-Dienste und vieles mehr. Die gesamte Funktionalität, die für alle anderen UWP-Apps verfügbar ist, steht auch für Ihre App zur Verfügung.
* Wenn Sie die gesamte Funktionalität der App aus der vertrauenswürdigen Partition in die App-Containerpartition verschieben, kann Ihre App auf allen Windows 10-Geräten ausgeführt werden.
* Ihre App kann als UWP-App die gleichen Aufgaben ausführen wie als klassische Desktop-App. Sie interagiert mit einer virtualisierten Ansicht der Registrierung und des Dateisystems, die von der tatsächlichen Registrierung und dem Dateisystem nicht zu unterscheiden ist.
* Ihre App kann an der im Windows Store integrierten Lizenzierung und automatischen Updatemöglichkeiten teilnehmen. Automatische Updates stellen einen sehr zuverlässigen und effizienten Mechanismus dar, da nur die geänderten Teile von Dateien heruntergeladen werden.

## Vorbereiten von Desktop-Apps für die Konvertierung in UWP-Apps
Sie müssen möglicherweise nicht viel tun, um Ihre App für die Konvertierung vorzubereiten. Denken Sie daran, dass Lizenzierung und automatische Updates im Rahmen des Windows Stores erfolgen, sodass Sie diese Features aus Ihrer Codebasis entfernen können. Wenn eine dieser Situationen auf Ihre Anwendung zutrifft, müssen Sie dieses Problems vor der Konvertierung beheben.

+ __Ihre App wird immer mit erhöhten Sicherheitsberechtigungen ausgeführt__. Ihre App muss als interaktiver Benutzer ausgeführt werden können. Benutzer, die Ihre App über den Windows Store installieren, sind möglicherweise keine Systemadministratoren. Wenn für die Ausführung Ihrer App erhöhte Rechte erforderlich sind, wird die App für Standardbenutzer nicht korrekt ausgeführt.
+ __Ihre App erfordert einen Kernelmodustreiber oder einen Windows-Dienst__. Die Brücke eignet sich für eine App, ein Kernelmodustreiber oder ein Windows-Dienst, die unter einem Systemkonto ausgeführt werden müssen, werden jedoch nicht unterstützt. Verwenden Sie anstelle eines Windows-Diensts eine [Hintergrundaufgabe](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).
+ __Ihre App-Module werden im Prozess für Prozesse geladen, die nicht im AppX-Paket enthalten sind__. Dies ist nicht zulässig. Prozessinterne Erweiterungen, beispielsweise [Shellerweiterungen](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), werden nicht unterstützt. Wenn zwei Apps in einem Paket enthalten sind, können Sie jedoch die prozessübergreifenden Kommunikation zwischen diesen verwenden.
+ __Ihre App verwendet eine benutzerdefinierte Anwendungsbenutzermodell-ID (Application User Model ID, AUMID)__. Wenn der Prozess [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) zum Festlegen einer eigenen Anwendungsbenutzermodell-ID aufruft, kann er nur die von der App-Modellumgebung/vom AppX-Paket generierte Anwendungsbenutzermodell-ID verwenden. Sie können keine benutzerdefinierten Anwendungsbenutzermodell-IDs definieren.
+ __Ihre App ändert die HKLM-Registrierungsstruktur (HKEY_LOCAL_MACHINE)__. Bei jedem Versuch, einen HKLM-Schlüssel zu erstellen oder einen zum Ändern zu öffnen, wird der Zugriff verweigert. Denken Sie daran, dass Ihre App über eine eigene private virtualisierte Ansicht der Registrierung verfügt. Damit kann das Konzept einer benutzer- und computerweiten Registrierungsstruktur (Zweck von HKLM) nicht angewendet werden. Sie müssen stattdessen eine andere Möglichkeit finden, um das zu erreichen, wozu Sie HKLM verwendet haben, beispielsweise Schreiben in HKEY_CURRENT_USER (HKCU).
+ __Ihre App verwendet einen DDEEXEC-Registrierungsunterschlüssel zum Starten einer anderen App__. Verwenden Sie stattdessen einen der DelegateExecute-Verbhandler genau wie von den verschiedenen Activatable*-Erweiterungen in dem [App-Paketmanifest](https://msdn.microsoft.com/library/windows/apps/br211474.aspx) konfiguriert.
+ __Ihre App schreibt in den AppData-Ordner, um Daten für eine andere App freizugeben__. Nach der Konvertierung wird AppData an den lokalen App-Datenspeicher weitergeleitet, der ein privater Speicher für jede UWP-App ist. Verwenden Sie verschiedene Methoden für die prozessübergreifende Datenfreigabe. Weitere Informationen finden Sie unter [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).
+ __Ihre App schreibt in das Installationsverzeichnis für die App__. Ihre App schreibt z. B. in eine Protokolldatei, die sich in demselben Verzeichnis wie die EXE-Datei befindet. Dies wird nicht unterstützt. Sie müssen daher einen anderen Speicherort wählen, z. B. den lokalen App-Datenspeicher.
+ __Ihre App-Installation erfordert eine Benutzerinteraktion__. Der App-Installer muss automatisch ausgeführt werden können und muss alle erforderlichen Komponenten installieren, die nicht standardmäßig auf einem sauberen Betriebssystemimage aktiviert sind.
+ __Ihre App verwendet das aktuelle Arbeitsverzeichnis__. Zur Laufzeit nutzt die konvertierte App nicht das gleiche Arbeitsverzeichnis, das Sie zuvor in Ihrer Desktop-LNK-Verknüpfung angegeben haben. Sie müssen das aktuelle Arbeitsverzeichnis zur Laufzeit ändern, wenn das richtige Verzeichnis notwendig ist, damit Ihre App ordnungsgemäß funktioniert.
+ __Ihre App benötigt UIAccess__. Wenn Ihre Anwendung `UIAccess=true` für das `requestedExecutionLevel`-Element im UAC-Manifest angibt, wird die Konvertierung in eine UWP-App derzeit nicht unterstützt. Weitere Informationen finden Sie unter [Sicherheit bei der Benutzeroberflächenautomatisierung – Übersicht](https://msdn.microsoft.com/library/ms742884.aspx).

## Inhalt dieses Abschnitts

| Thema | Beschreibung |
|-------|-------------|
| [Vorschau für den Desktop-App-Konverter (Project Centennial)](desktop-to-uwp-run-desktop-app-converter.md) | Beinhaltet Informationen zum Ausführen des Desktop-App-Konverters. Sie müssen wahrscheinlich nicht viel tun, wenn überhaupt, um Ihre App für die Konvertierung vorzubereiten. |
| [Bereitstellen und Debuggen der konvertierten UWP-App](desktop-to-uwp-deploy-and-debug.md) | Dieses Thema enthält Informationen, die Sie beim erfolgreichen Bereitstellen und Debuggen der App nach der Konvertierung unterstützen. In diesem Thema finden Sie auch Informationen zu einigen internen Aspekten der Desktop-Konvertierungserweiterungen. |


<!--HONumber=May16_HO2-->


