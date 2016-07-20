---
author: mtoepke
title: "Erstellen eines einfachen Spiels für die universelle Windows-Plattform (UWP) mit DirectX"
description: "In diesen Tutorials lernen Sie, wie Sie ein einfaches Spiel für die universelle Windows-Plattform (UWP) mit DirectX und C++ erstellen."
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords:
- DirectX game sample
- game sample, Universal Windows Platform (UWP)
- Direct3D 11 game
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 7891642d33e7391e05ae0639e36ff90e6697def6

---

# Erstellen eines einfachen Spiels für die universelle Windows-Plattform (UWP) mit DirectX


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In diesen Tutorials lernen Sie, wie Sie ein einfaches Spiel für die universelle Windows-Plattform (UWP) mit DirectX und C++ erstellen. Wir befassen uns mit allen wichtigen Teilen eines Spiels. Hierzu zählen die Prozesse zum Laden von Ressourcen wie Grafiken und Gittern, das Erstellen einer Hauptschleife für das Spiel, das Implementieren einer einfachen Rendering-Pipeline sowie das Hinzufügen von Soundeffekten und Steuerelementen.

Wir machen Sie mit den Techniken und Überlegungen für die Entwicklung von UWP-Spielen vertraut. Sie bekommen von uns allerdings kein vollständiges Spiel geliefert. Stattdessen konzentrieren wir uns auf wichtige Konzepte für die Entwicklung von UWP-DirectX-Spielen und weisen auf Windows-Runtime-spezifische Aspekte im Zusammenhang mit diesen Konzepten hin.

## Ziel


-   Hier lernen Sie, wie Sie die grundlegenden Konzepte und Komponenten eines UWP-DirectX-Spiels einsetzen. Danach wird Ihnen auch die Entwicklung von UWP-Spielen mit DirectX leichter fallen.

## Wissenswertes


Für dieses Tutorial müssen Sie mit den folgenden Themen vertraut sein:

-   Microsoft C++ mit Komponentenerweiterungen (C++/CX). Dies ist ein Update für MicrosoftC++, das die automatische Verweiszählung einführt. Außerdem handelt es sich hierbei um die Sprache für die Entwicklung eines UWP-Spiels mit DirectX11.1 oder einer höheren Version.
-   Grundlegende Konzepte der linearen Algebra und der newtonschen Physik.
-   Grundlegende Terminologie für die Grafikprogrammierung.
-   Grundlegende Konzepte der Windows-Programmierung.
-   Grundlegende Kenntnisse der APIs von [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) und [Direct3D11](https://msdn.microsoft.com/library/windows/desktop/hh404569).

##  Beispiel für einen WindowsStore-Direct3D-Shooter


Dieses Beispiel implementiert einen einfachen Schießstand, an dem der Spieler aus der Ich-Perspektive mit Bällen auf bewegliche Ziele schießt. Wird ein Ziel getroffen, erhält der Spieler eine bestimmte Anzahl von Punkten. Der Spieler kann nacheinander sechs Level mit jeweils steigendem Schwierigkeitsgrad absolvieren. Am Ende der Level werden die Punkte zusammengezählt, und der Spieler erhält einen endgültigen Punktestand.

Das Beispiel veranschaulicht folgende Spielkonzepte:

-   Zusammenarbeit zwischen DirectX11.1 und der Windows-Runtime
-   Dreidimensionale Ich-Perspektive und entsprechende Kamera
-   Stereoskopische 3D-Effekte
-   Kollisionserkennung zwischen Objekten in 3D
-   Behandlung von Spielereingaben per Maus, Toucheingabe und Xbox360-Controller
-   Audiomixing und -wiedergabe
-   Einfacher Spielzustandsautomat

![Das Spielbeispiel in Aktion](images/simple3dgame-display.png)


| Thema | Beschreibung |
|---------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Einrichten des Spieleprojekts](tutorial--setting-up-the-games-infrastructure.md) | Im ersten Schritt für die Erstellung Ihres Spiels richten Sie ein Projekt in Microsoft Visual Studio so ein, dass Sie möglichst wenig Aufwand mit der Bearbeitung der Codeinfrastruktur haben. Sie können sich eine Menge Zeit und Arbeit ersparen, wenn Sie die richtige Vorlage verwenden und das Projekt speziell für die Spieleentwicklung konfigurieren. Im Anschluss finden Sie die erforderlichen Einrichtungs- und Konfigurationsschritte für ein einfaches Spieleprojekt. |
| [Definieren des UWP-App-Frameworks für das Spiel](tutorial--building-the-games-metro-style-app-framework.md) | Wenn Sie Code für ein UWP-Spiel mit DirectX erstellen, müssen Sie zunächst das Framework erstellen, das die Interaktion der Spielobjekte mit Windows ermöglicht. Hierzu gehören Windows-Runtime-Eigenschaften wie die Behandlung von Anhalte-/Fortsetzungsereignissen, Fensterfokus und Andocken sowie die Ereignisse, Interaktionen und Übergänge für die Benutzeroberfläche. Wir erläutern, wie das Beispielspiel strukturiert ist und wie der übergeordnete Zustandsautomat für die Interaktion zwischen Spieler und System definiert wird. |
| [Definieren des Hauptobjekts für das Spiel](tutorial--defining-the-main-game-loop.md) | In diesem Abschnitt widmen wir uns den Details des Hauptobjekts des Beispielspiels. Außerdem erfahren Sie, wie die implementierten Regeln in Interaktionen mit der Spielwelt übersetzt werden. |
| [Zusammensetzen des Renderingframeworks](tutorial--assembling-the-rendering-pipeline.md) | Jetzt ist es Zeit, einen Blick darauf zu werfen, wie diese Struktur und der Zustand im Beispielspiel zum Anzeigen der Grafiken verwendet werden. Hier beschäftigen wir uns mit der Implementierung eines Renderingframeworks – von der Initialisierung des Grafikgeräts bis zur Darstellung der anzuzeigenden Grafikobjekte. |
| [Hinzufügen einer Benutzeroberfläche](tutorial--adding-a-user-interface.md) | Sie haben gesehen, wie das Beispiel das Hauptspielobjekt implementiert, und das grundlegende Renderingframework kennengelernt. Jetzt wollen wir uns ansehen, wie das Spiel dem Spieler Feedback zum Spielzustand gibt. Hier erfahren Sie, wie Sie einfache Menüoptionen und Head-Up-Anzeigekomponenten zur 3D-Grafikpipelineausgabe hinzufügen. |
| [Hinzufügen von Steuerelementen](tutorial--adding-controls.md) | In diesem Thema befassen wir uns damit, wie das Beispielspiel die Bewegungs-/Blicksteuerung in einem 3D-Spiel implementiert und wie einfache Steuerungen für Toucheingabe, Maus und Gamecontroller entwickelt werden. |
| [Hinzufügen von Sound](tutorial--adding-sound.md) | In diesem Schritt untersuchen wir, wie das Beispielshooterspiel mit den [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813)-APIs ein Objekt für die Soundwiedergabe erstellt. |
| [Erweitern des Spielbeispiels](tutorial-resources.md) | Herzlichen Glückwunsch! Sie sind nun mit den Schlüsselkomponenten eines einfachen dreidimensionalen UWP-DirectX-Spiels vertraut. Sie können das Framework für ein Spiel (einschließlich Ansichtsanbieter und Rendering-Pipeline) einrichten und eine einfache Spielschleife implementieren. Zudem können Sie ein einfaches Benutzeroberflächenoverlay erstellen und Soundeffekte und Steuerelemente einbauen. Damit sind Sie der Erstellung eines eigenen Spiels einen großen Schritt näher gekommen. Hier finden Sie einige Ressourcen, um Ihre Kenntnisse im Bereich der DirectX-Spieleentwicklung weiter zu vertiefen. |
 

 

 







<!--HONumber=Jul16_HO2-->


