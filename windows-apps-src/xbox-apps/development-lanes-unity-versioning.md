---
author: JordanEllis6809
title: "Unity – Versionskontrolle für Ihr UWP-Projekt"
description: "Versionsverwaltung für Ihr Unity-UWP."
translationtype: Human Translation
ms.sourcegitcommit: a1b759b00e35092323b8c4634907dd5c0fffa68c
ms.openlocfilehash: 3b796c31e6b284cea628ba68a34799cf9317ee2e

---

# Unity: Versionskontrolle für Ihr UWP-Projekt

Sie haben Ihr Unity-Spiel für Xbox noch immer nicht in die universelle Windows-Plattform (UWP) portiert?  Lesen Sie zunächst [Portieren von Unity-Spielen für Xbox auf die UWP](development-lanes-unity.md).

Es gibt verschiedene Gründe dafür, Teile Ihres generierten UWP-Verzeichnisses der Versionskontrolle hinzuzufügen. Hierzu zählt unter anderem das Hinzufügen von Abhängigkeiten (z.B. Xbox Live SDK).  Dieses Szenario wird in diesem Lernprogramm als Beispiel herangezogen und hilft Ihnen hoffentlich dabei, die individuellen Anforderungen Ihres Projekts zu erfüllen.

***Hinweis: Wir verwenden Git als Versionskontrolllösung.  Die Konzepte sind jedoch üblicherweise auf andere Lösungen übertragbar.***

Zur Erinnerung: Das Verzeichnis für unser Spiel ***ScrapyardPhoenix*** sieht aktuell wie folgt aus:

![Build-Zielordner](images/build-destination.png)

Und unser UWP-Verzeichnis sieht wie folgt aus:

![UWP-VS-Lösung](images/uwp-vs-solution.png)

In diesem Verzeichnis interessiert uns nur der Ordner ***ScrapyardPhoenix*** (Name Ihres Spiels).  Alles andere ist für unsere Versionskontrolle nicht relevant.

***Noch keine Erfahrung mit GITIGNORE-Dateien?  Siehe [Gitignore](https://git-scm.com/docs/gitignore).***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

Wir möchten einige unterschiedliche Dateien und Ordner aus dem Ordner **UWP/ScrapyardPhoenix** auswählen und unserer Versionskontrolle hinzufügen.  Sehen wir uns das Ganze erst einmal im Detail an:

![UWP-Buildverzeichnis](images/uwp-build-directory.png)  

## Ordner  

`Assets` | ***Einbeziehen*** | Enthält WindowsStore-Bilder.  
`Data`   | ***Ignorieren*** | Der Ort, an dem Unity Ihr Projekt kompiliert (Szenen, Shader, Skripts, vorgefertigte Elemente usw.).  
`Dependencies` | ***Einbeziehen*** | In diesem von mir erstellten Ordner werden alle UWP-Abhängigkeiten (z.B. XboxLiveSDK.dll) gespeichert.  
`Properties` | ***Einbeziehen*** | Enthält erweiterte Einstellungen, die vom Entwickler geändert werden können.  
`Unprocessed` | ***Ignorieren*** | Enthält Unity-Dateien vom Typ `.dll` und `.pdb`  

## Dateien  

`App.cs` | ***Einbeziehen*** | Der Einstiegspunkt für Ihre UWP-Anwendung. Dieser kann geändert und mit anderen Quelldateien erweitert werden.  
`Package.appxmanifest` | ***Einbeziehen*** | Das Paketmanifest für Ihr AppX-Paket.  
`project.json` | ***Einbeziehen*** | Beschreibt die NuGet-Pakete, von denen Ihre Datei vom Typ `*.csproj` abhängig ist.  
`ScrapyardPhoenix.csproj` | ***Einbeziehen*** | Beschreibt Ihr UWP-Buildziel. Wenn Sie Ihrem UWP-Projekt zusätzliche Abhängigkeiten hinzufügen, sind die entsprechenden Informationen in dieser Datei vom Typ `*.csproj` enthalten.  
`ScrapyardPhoenix.csproj.user` | ***Ignorieren*** | Diese Datei enthält lokale Benutzerinformationen.

## Resultierende GITIGNORE-Datei

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

Geschafft: Ihre Teammitglieder sind nun mit dem von Ihnen generierten UWP-Projekt synchronisiert. Nun können Sie Ihrem UWP-Projekt problemlos zusätzliche Ressourcen, Quellen und Abhängigkeiten hinzufügen.

Einige weitere Versionsverwaltungsbeispiele für den UWP-Ordner finden Sie [hier](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

## Hinzufügen von Abhängigkeiten zu Ihrer UWP-App

Fügen Sie Abhängigkeiten zu DLL- und WINMD-Dateien hinzu, indem Sie diese im Unterordner **Unity Assets** des Ordners **Plug-Ins** ablegen und auswählen. Legen Sie anschließend im Paketprüfungstool die Plattform-Zieleinstellungen fest.

![UWP-Lösung](images/uwp-solution.PNG)

***ScrapyardPhoenix (Universal Windows)*** ist das Projekt, dem Sie einen Verweis (etwa auf das Xbox Live SDK) hinzufügen würden.

## Weitere Informationen
- [Portieren vorhandener Spiele zu Xbox](development-lanes-landing.md)
- [UWP auf XboxOne](index.md)



<!--HONumber=Aug16_HO4-->


