---
author: JordanEllis6809
title: "Portieren von Unity-Spielen für Xbox auf die UWP"
description: "UWP-Entwicklung für Unity-Spiele auf Xbox."
translationtype: Human Translation
ms.sourcegitcommit: ea3bea2e5d6de0e55615de701a69e90d81f0f553
ms.openlocfilehash: 73f701a2608c6ce8d10cab817683ada4e9eecc08

---

# Portieren von Unity-Spielen für Xbox auf die UWP


In diesem ausführlichen Lernprogramm wird davon ausgegangen, dass Sie bereits über ein Spiel in Unity verfügen, das zum Erstellen und Bereitstellen bereit ist.

Beachten Sie auch die [Videoversion des Lernprogramms](https://www.youtube.com/watch?v=f0Ptvw7k-CE).

Möchten Sie eine Versionsverwaltung für Ihr Unity-UWP-Projekt verwenden? Siehe [Versionskontrolle für Ihr UWP-Projekt](development-lanes-unity-versioning.md).

## Schritt0: Sicherstellen, dass Unity richtig installiert wurde

Beim Installieren von Unity müssen folgende Komponenten ausgewählt sein:

![Installationskomponenten von Unity](images/unity-install-components.png)

## Schritt 1: Erstellen der UWP-Lösung

Öffnen Sie in Ihrem Unity-Spielprojekt das Fenster mit den **Build-Einstellungen** unter **Datei -> Build-Einstellungen**, und wechseln Sie zum Menü mit den Windows Store-Optionen.

![Fenster mit Build-Einstellungen](images/build-settings.png)

Stellen Sie sicher, dass die **SDK**-Einstellung auf **Universal10** gesetzt ist, und klicken Sie dann auf die Schaltfläche **Build**. Ein Datei-Explorer-Fenster wird geöffnet, in dem Sie nach einem Zielordner gefragt werden. Erstellen Sie neben dem Verzeichnis **Assets** Ihres Projekts den Ordner **UWP**, und wählen Sie diesen Ordner als Zielordner des Builds aus.

![Build-Zielordner](images/build-destination.png)

Unity hat eine neue Visual Studio-Lösung erstellt, die wir zum Bereitstellen Ihres UWP-Spiels verwenden.

![UWP-VS-Lösung](images/uwp-vs-solution.png)

## Schritt 2: Bereitstellen des Spiels

Öffnen Sie die neu erstellte Lösung im Ordner **UWP**, und ändern Sie die Zielplattform zu **x64**.

![x64-Build-Plattform](images/x64-build-platform.png)

Nachdem Sie nun über eine Visual Studio-UWP-Lösung für Ihr Spiel verfügen, [befolgen Sie diese Schritte](getting-started.md), um Ihr Spiel erfolgreich auf Ihrer Xbox One-Konsole für den Einzelhandel bereitzustellen!

## Schritt3: Ändern und Neuerstellen

Wenn etwas anderes als ein Skript geändert wird, muss das Projekt im Editor neu erstellt werden (siehe __Schritt1__), damit die Änderungen in den UWP-Build Ihres Spiel übernommen werden.

## Versionsverwaltung für Ihr UWP-Projekt

In bestimmten Situationen müssen Teile dieses neu generierten UWP-Verzeichnisses der Versionskontrolle hinzugefügt werden. Dies ist beispielsweise der Fall, wenn Sie dem UWP-Projekt eine neue Abhängigkeit (z.B. das Xbox Live SDK) hinzufügen.  Wir betrachten dieses Beispiel unter [Versionskontrolle für Ihr UWP-Projekt](development-lanes-unity-versioning.md) ausführlich.

## Weitere Informationen
- [Portieren vorhandener Spiele zu Xbox](development-lanes-landing.md)
- [UWP auf XboxOne](index.md)



<!--HONumber=Aug16_HO4-->


