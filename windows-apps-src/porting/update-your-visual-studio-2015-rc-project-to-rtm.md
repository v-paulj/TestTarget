---
author: mcleblanc
description: "Wenn Sie über ein mit Microsoft Visual Studio2015RC erstelltes Windows10-Projekt verfügen, können Sie die Projektdateien auf zwei Arten auf das für Visual Studio2015RTM geeignete Format aktualisieren."
title: Aktualisieren Ihres UWP-Projekts von der RC-Version auf die RTM-Version von Microsoft Visual Studio2015
ms.assetid: 104E36CE-36DE-4E9C-A944-711C200B44EF
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 10c65c359f3a0791ba03288a745bc732b94251b7

---

# Aktualisieren Ihres UWP-Projekts von der RC-Version auf die RTM-Version von Microsoft Visual Studio2015

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Wenn Sie über ein mit Microsoft Visual Studio2015RC erstelltes Windows10-Projekt verfügen, können Sie die Projektdateien auf zwei Arten auf das für Visual Studio2015RTM geeignete Format aktualisieren. Die empfohlene Methode ist das Erstellen eines neuen Windows10-Projekts in Visual Studio2015RTM und das Kopieren Ihrer Dateien in dieses Projekt. Alternativ können Sie zum Bearbeiten Ihrer vorhandenen Projektdateien die erweiterte Dokumentation befolgen und sie in das neue Format konvertieren.

## Anzeige beim Öffnen eines in Visual Studio2015RC erstellten Windows10-Projekts in Visual Studio2015RTM

Wenn Sie ein in Visual Studio2015RC erstelltes Windows10-Projekt in Visual Studio2015RTM öffnen, werden Sie im Projektmappen-Explorer**** durch eine Meldung darauf hingewiesen, dass ein Update erforderlich ist.

![Update erforderlich](images/vsrc-to-rtm/solution-explorer.png)

Wenn Sie im Projektmappen-Explorer**** das Kontextmenü für das Projekt aufrufen und **Projekt erneut laden** auswählen, erscheint das folgende Dialogfeld:

![VisualStudio-Updateerforderlich](images/vsrc-to-rtm/reload-project.png)

## Erstellen eines neuen Projekts und Kopieren der Dateien in das Projekt

1.  Starten Sie Microsoft Visual Studio2015RTM, und erstellen Sie ein neues (leeres) Anwendungsprojekt vom Typ „Windows Universal“. Denken Sie daran, dass das neue Projekt standardmäßig ein App-Paket (eine APPX-Datei) erstellt, das für die universelle Gerätefamilie geeignet ist. Ändern Sie es, wenn ein Projekt auf eine oder mehrere bestimmte Gerätefamilien abzielt.
2.  Geben Sie in Ihrem Visual Studio2015RC-Projekt alle Quellcodedateien und visuellen Ressourcendateien an, die Sie kopieren möchten. Kopieren Sie mithilfe des Datei-Explorers die Datenmodelle, Ansichtsmodelle, visuellen Ressourcen, Ressourcenverzeichnisse sowie die Ordnerstruktur und alles andere, das Sie in dem neuen Projekt wiederverwenden möchten. Kopieren oder erstellen Sie gegebenenfalls Unterordner auf dem Datenträger.
3.  Kopieren Sie auch Ansichten (z.B. „MainPage.xaml“ und „MainPage.xaml.cs“) in das neue Projekt. Erstellen Sie bei Bedarf auch hier neue Unterordner, und entfernen Sie die vorhandenen Ansichten aus dem Projekt. Erstellen Sie vor dem Überschreiben oder Entfernen einer von Visual Studio generierten Ansicht jedoch eine Kopie, um später darauf zurückgreifen zu können.
4.  Vergewissern Sie sich im Projektmappen-Explorer****, dass **Alle Dateien anzeigen** aktiviert ist. Wählen Sie die kopierten Dateien aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie dann auf **Zu Projekt hinzufügen**. Dadurch werden die zugehörigen Ordner automatisch hinzugefügt. Sie können **Alle Dateien anzeigen** jetzt ggf. deaktivieren. Eine alternative Vorgehensweise ist die Verwendung des Befehls **Vorhandenes Element hinzufügen**, wenn Sie alle erforderlichen Unterordner im **Projektmappen-Explorer** von Visual Studio erstellt haben. Stellen Sie sicher, dass für Ihre visuellen Ressourcen **Buildvorgang** auf **Inhalt** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** festgelegt ist.
5.  Fügen Sie Verweise zu allen Erweiterungs-SDKs hinzu, auf die Sie in Ihrem RC-Projekt verwiesen haben, und kopieren Sie alle Änderungen aus Ihrem vorherigen Package.appxmanifest (z. B. alle Funktionen, die Sie deklariert haben) in das des neuen RTM-Projekts.

## Erweitert: Bearbeiten der vorhandenen Projektdateien

Ein erheblicher Unterschied zwischen dem Windows10-Projektformat von Visual Studio2015RC und Visual Studio2015RTM ist die Verwendung der [NuGet](http://docs.nuget.org/)-Version3 im RTM-Format. Beachten Sie diesen Unterschied, wenn Sie beabsichtigen, Ihr Projekt manuell zu aktualisieren.

Wenn Sie Ihr Projekt manuell aktualisieren oder wissen möchten, worin die Unterschiede zwischen den Projektformaten von Visual Studio2015RC und Visual Studio2015RTM liegen, finden Sie unter [Migrieren von Apps auf die universelle Windows-Plattform (UWP)](http://msdn.microsoft.com/library/mt148501.aspx) weitere Informationen.




<!--HONumber=Aug16_HO3-->


