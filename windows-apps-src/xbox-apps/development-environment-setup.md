---
author: Mtoepke
title: Einrichten der Umgebung für die UWP-Entwicklung auf Xbox
description: Schritte zum Einrichten und Testen der Umgebung für die UWP-Entwicklung auf Xbox
area: Xbox
---

# Einrichten der Umgebung für die UWP-Entwicklung auf Xbox

Die Umgebung für die UWP-Entwicklung (Universelle Windows-Plattform) auf Xbox besteht aus einem Entwicklungscomputer, der über ein lokales Netzwerk mit einer Xbox One-Konsole verbunden ist.
Der Entwicklungscomputer erfordert Windows 10, Visual Studio 2015 Update 2, Windows 10 SDK Preview Build 14295 und eine Reihe von Unterstützungstools.


In diesem Artikel werden die Schritte zum Einrichten und Testen der Entwicklungsumgebung beschrieben.

## Einrichten von Visual Studio

1. Installieren Sie Visual Studio 2015 Update 2 oder höher. Weitere Informationen und Downloads für die Installation finden Sie unter [Downloads und Tools für Windows 10](https://dev.windows.com/downloads).

1. Wenn Sie Visual Studio 2015 Update 2 installieren, stellen Sie sicher, dass das Kontrollkästchen **Entwicklungstools für universelle Windows-Apps** aktiviert ist.

  ![Installieren von Visual Studio 2015 Update 2](images/vs_install_tools.png)

## Einrichten des Windows 10 SDK

Installieren Sie Windows 10 Anniversary SDK Preview Build 14295. Informationen zur Installation finden Sie unter [Insider Preview-Updates für Entwickler herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=780552).

  > **Wichtig**
            &nbsp;&nbsp;Sie müssen das aktuelle SDK installieren, Sie müssen jedoch _nicht_ die aktuelle Windows Insider Preview-Version des Betriebssystems installieren.

## Erstellen Ihrer ersten Anwendung

1. Stellen Sie sicher, dass sich der Entwicklungscomputer in demselben lokalen Netzwerk wie die Ziel-Xbox One-Konsole befindet. Dies bedeutet normalerweise, dass beide denselben Router verwenden und sich im gleichen Subnetz befinden. Es wird eine drahtgebundene Netzwerkverbindung empfohlen.

1. Stellen Sie sicher, dass sich die Xbox One-Konsole im Entwicklermodus befindet.  Weitere Informationen finden Sie unter [Aktivieren des Entwicklermodus auf Xbox One](devkit-activation.md).

1. Legen Sie die Programmiersprache fest, die Sie für Ihre UWP-App verwenden möchten.

1. Wählen Sie auf dem Entwicklungscomputer **Neues Projekt** und dann **Windows / Universell / Leere App** aus.

### Starten eines C#-Projekts

  ![Dialogfeld „Neues Projekt“](images/vs_universal_blank.jpg)

1. Wählen Sie im Dialogfeld **Neues universelle Windows-Projekt** die Standardoptionen aus. Wenn das Dialogfeld **Entwicklermodus** angezeigt wird, klicken Sie auf **OK**. Es wird eine neue leere App erstellt.

1. Konfigurieren Sie die Entwicklungsumgebung für das Remotedebuggen:

  1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Eigenschaften** aus.
  1. Ändern Sie auf der Registerkarte **Debuggen** das **Zielgerät** in **Remotecomputer**.
  1. Geben Sie in **Remotecomputer** die System-IP-Adresse oder den Hostnamen der Xbox One-Konsole ein. Informationen zum Ermitteln der IP-Adresse oder des Hostnamens finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).
  1. Wählen Sie in der Dropdownliste **Authentifizierungsmodus** den Eintrag **Universell (unverschlüsseltes Protokoll)** aus.

    ![C#-Eigenschaftenseiten für „Leere App“](images/vs_remote.jpg)

### Starten eines C++-Projekts

  ![C++-Projekt](images/vs_universal_cpp_blank.jpg)

1. Wählen Sie im Dialogfeld **Neues universelle Windows-Projekt** die Standardoptionen aus. Wenn das Dialogfeld **Entwicklermodus** angezeigt wird, klicken Sie auf **OK**. Es wird eine neue leere App erstellt.

1. Konfigurieren Sie die Entwicklungsumgebung für das Remotedebuggen:

   1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Eigenschaften** aus.
   1. Ändern Sie **Zu startender Debugger** auf der Registerkarte **Debuggen** in **Remotecomputer**.
   1. Geben Sie in **Computername** die System-IP-Adresse oder den Hostnamen der Xbox One-Konsole ein. Informationen zum Ermitteln der IP-Adresse oder des Hostnamens finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).
   1. Wählen Sie in der Dropdownliste **Authentifizierungstyp** den Eintrag **Universell (unverschlüsseltes Protokoll)** aus.

    ![C++-Eigenschaftenseiten für „Leere App“](images/vs_remote_cpp.jpg)

### Koppeln des Geräts per PIN mit Visual Studio

1. Speichern Sie die Einstellungen, und stellen Sie sicher, dass sich die Xbox One-Konsole im Entwicklermodus befindet.

1. Drücken Sie F5.

1. Wenn dies Ihre erste Bereitstellung ist, werden Sie in einem Dialogfeld in Visual Studio aufgefordert, Ihr Gerät per PIN zu koppeln.

  1. Um eine PIN abzurufen, öffnen Sie auf der Startseite der Xbox One-Konsole **Dev Home**.
  1. Wählen Sie **Mit Visual Studio koppeln** aus.

    ![Dialogfeld „Mit Visual Studio koppeln“](images/devhome_visualstudio.png)

  1. Geben Sie im Dialogfeld **Mit Visual Studio koppeln** die PIN ein. Die folgende PIN ist nur ein Beispiel und stimmt nicht mit Ihrer PIN überein.

    ![Dialogfeld „Mit Visual Studio per PIN koppeln“](images/devhome_pin.png)

  1. Gegebenenfalls auftretende Bereitstellungsfehler werden im Fenster **Ausgabe** angezeigt.

Herzlichen Glückwunsch! Sie haben Ihre erste UWP-App auf Xbox erfolgreich erstellt und bereitgestellt!



## Siehe auch
- [Aktivieren des Entwicklermodus auf Xbox One](devkit-activation.md)  
- [Downloads und Tools für Windows 10](https://dev.windows.com/downloads)  
- [Insider Preview-Updates für Entwickler herunterladen](http://go.microsoft.com/fwlink/?LinkId=780552)  
- [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md) 
- [UWP auf Xbox One](index.md)

----


<!--HONumber=May16_HO2-->


