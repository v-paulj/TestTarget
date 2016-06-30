---
title: Bringen Sie Ihr Unity-Spiel auf Xbox One
author: JordanEllis6809
ms.sourcegitcommit: 008ff2566b17a05b52dee0a8cd6c070d841b1f62
ms.openlocfilehash: cc854bc707a9c08687d3c6d92a704f5099d52d5b

---

# Bringen Sie Ihr Unity-Spiel auf Xbox One

In dieser ausführlichen Einführungsanleitung wird davon ausgegangen, dass Sie bereits ein Spiel in Unity haben, das zur Erstellung und Bereitstellung bereit ist.

[Videoversion dieses Lernprogramms.](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

## Schritt 0: Stellen Sie sicher, dass Unity korrekt installiert ist

Bei der Installation von Unity müssen diese Komponenten ausgewählt sein:

![Installationskomponenten von Unity](images/unity-install-components.png)

## Schritt 1: Erstellen der UWP-Lösung

Öffnen Sie in Ihrem Unity-Spielprojekt das Fenster mit den Build-Einstellungen unter `File -> Build Settings...`, und rufen Sie das unten dargestellte Menü mit den Windows Store-Optionen auf.

![Fenster mit Build-Einstellungen](images/build-settings.png)

Stellen Sie sicher, dass die `SDK`-Einstellung auf `Universal 10` festgelegt ist. Klicken Sie als Nächstes am unteren Rand des Menüs auf die Build-Schaltfläche. Dadurch wird ein Explorer-Fenster geöffnet, in dem Sie nach einem Zielordner gefragt werden. Erstellen Sie einen Ordner mit dem Namen `UWP` im Verzeichnis `Assets` Ihres Projekts, und wählen Sie diesen Ordner als Zielordner des Builds.

![Build-Zielordner](images/build-destination.png)

Unity hat jetzt eine neue Visual Studio-Lösung erstellt, die wir zum Bereitstellen Ihres UWP-Spiels im nächsten Schritt verwenden.

![UWP-VS-Lösung](images/uwp-vs-solution.png)

## Schritt 2: Bereitstellen des Spiels

Öffnen Sie die neu erstellte Lösung im Ordner `Assets/UWP`.  Ändern Sie die Zielplattform anschließend in X64.

![x64-Build-Plattform](images/x64-build-platform.png)

Nachdem Sie nun über eine Visual Studio-UWP-Lösung für Ihr Spiel verfügen, [befolgen Sie diese Schritte](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started), um Ihr Spiel erfolgreich auf Ihrer Xbox One Einzelhandels-Konsole bereitzustellen!

## Hinweise für Entwickler

- Es wird empfohlen, den UWP-Ordner in Ihrer Versionskontrolle zu ignorieren. Wenn Sie weitere XAML-Elemente zu Ihrem Projekt hinzufügen möchten und einige Ressourcen innerhalb des UWP-Ordners versionieren müssen, finden Sie hier [Beispiele, die genau das tun](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

- Wenn Sie in Unity Änderungen an Inhalten (mit Ausnahme von Skripts) vornehmen, die im Build Ihres Spiels enthalten sind, müssen Sie Ihre UWP-Lösung neu erstellen, damit diese Änderungen bei der nächsten Bereitstellung angezeigt werden. Das liegt daran, dass während des Build-Schritts von Unity alle Ressourcen Ihres Projekts in einer Ressourcendatei kompiliert werden. Die UWP-Lösung verweist auf diese generierte Ressourcendatei, wenn sie das Spiel bereitstellt.




<!--HONumber=Jun16_HO4-->


