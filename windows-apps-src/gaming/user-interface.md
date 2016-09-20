---
author: mtoepke
title: DirectX-Spielprojektvorlagen
description: "Hier erhalten Sie Informationen zu den Vorlagen zum Erstellen eines DirectX-Spiels für die Universelle Windows-Plattform (UWP)."
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: b7d01fda0bc8dbafebb7485ec01acb5c7431a815

---

# DirectX-Spielprojektvorlagen


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Mit Vorlagen für DirectX und die universelle Windows-Plattform (UWP) können Sie schnell ein Projekt als Ausgangspunkt für Ihr Spiel erstellen.

## Voraussetzungen


Gehen Sie wie folgt vor, um das Projekt zu erstellen:

-   
            [Laden Sie Microsoft Visual Studio2015 herunter](https://www.visualstudio.com/vs-2015-product-editions). Visual Studio2015 enthält Tools für die Grafikprogrammierung (beispielsweise Debugtools). Eine Übersicht über DirectX-Grafik- und -Spielefeatures/-tools finden Sie unter [VisualStudio-Tools für die Entwicklung von DirectX-Spielen](set-up-visual-studio-for-game-development.md).

## Auswählen einer Vorlage


Visual Studio2015 umfasst drei Vorlagen für DirectX und UWP:

-   DirectX 11-App (Universelle Windows-App): Mit der Vorlage "DirectX 11-App (Universelle Windows-App)" wird ein UWP-Projekt erstellt, das direkt in einem App-Fenster mit DirectX11 gerendert wird.
-   DirectX 12-App (Universelle Windows-App): Mit der Vorlage "DirectX 12-App (Universelle Windows-App)" wird ein UWP-Projekt erstellt, das direkt in einem App-Fenster mit DirectX12 gerendert wird.
-   DirectX11- und XAML-App (Universelle Windows-App): Mit der Vorlage „DirectX11- und XAML-App (Universelle Windows-App)“ wird ein UWP-Projekt erstellt, das direkt in einem XAML-Steuerelement mit DirectX11 gerendert wird. Diese Vorlage verwendet ein [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)-Element und ermöglicht so die Verwendung von XAML-UI-Steuerelementen. Dies kann zwar das Hinzufügen von Benutzeroberflächenelementen vereinfachen, die Verwendung der XAML-Vorlage beeinträchtigt aber unter Umständen die Leistung.

Die Entscheidung für eine Vorlage hängt von der Leistung und von den gewünschten Technologien ab.

## Vorlagenstruktur


Die DirectX-UWP-Vorlagen enthalten die folgenden Dateien:

-   "pch.h" und "pch.cpp": Unterstützung für vorkompilierte Headerdateien.
-   Package.appxmanifest: Die Eigenschaften des Bereitstellungspakets für die App.
-   \*.pfx: Zertifikate für die Anwendung.
-   Externe Abhängigkeiten: Links zu externen Dateien, die vom Projekt verwendet werden.
-   „\*Main.h“ und „\*Main.cpp“: Methoden zum Verwalten von Anwendungsressourcen, zum Aktualisieren des Anwendungszustands und zum Rendern des Frames.
-   "App.h" und "App.cpp": Haupteinstiegspunkt für die Anwendung. Verbindet die App mit der Windows-Shell und behandelt App-Lebenszyklusereignisse. Diese Dateien werden nur in den Vorlagen „DirectX11-App (Universelle Windows-App)“ und „DirectX12-App (Universelle Windows-App)“ angezeigt.
-   "App.Xaml", "App.Xaml.cpp" und "App.Xaml.h": Haupteinstiegspunkt für die Anwendung. Verbindet die App mit der Windows-Shell und behandelt App-Lebenszyklusereignisse. Diese Dateien werden nur in der Vorlage "DirectX 11- und XAML-App (Universelle Windows-App)" angezeigt.
-   "DirectXPage.xaml", "DirectXPage.xaml.cpp" und "DirectXPage.xaml.h": Eine Seite, die ein DirectX-SwapChainPanel hostet. Diese Dateien werden nur in der Vorlage "DirectX 11- und XAML-App (Universelle Windows-App)" angezeigt.
-   Inhalt
    -   Sample3DSceneRenderer.h und Sample3DSceneRenderer.cpp – Ein Beispielrenderer, der eine grundlegende Renderingpipeline instanziiert.
    -   SampleFpsTextRenderer.h und SampleFpsTextRenderer.cpp – Rendert den aktuellen FPS-Wert mit Direct2D und DirectWrite rechts unten auf dem Bildschirm. Diese Dateien werden nur in den Vorlagen „DirectX11-App (Universelle Windows-App)“ und „DirectX11- und XAML-App (Universelle Windows-App)“ angezeigt.
    -   SamplePixelShader.hlsl – Ein einfacher Beispiel-Pixelshader.
    -   SampleVertexShader.hlsl – Ein einfacher Beispiel-Vertex-Shader.
    -   ShaderStructures.h – Zum Senden von Daten an den Beispiel-Vertex-Shader verwendete Strukturen.
-   Allgemein
    -   StepTimer.h – Eine Hilfsklasse für das Animations- und Simulationstiming.
    -   DirectXHelper.h – Verschiedene Hilfsfunktionen.
    -   DeviceResources.h und Device Resources.cpp – Stellt eine Schnittstelle für eine Anwendung bereit, die Eigentümer von DeviceResources ist, um benachrichtigt zu werden, wenn das Gerät verloren geht oder erstellt wird.
    -   d3dx12.h – Enthält die D3DX12-Hilfsprogrammbibliothek. Diese Datei ist nur in der DirectX 12-App (Universelle Windows-App) vorhanden.
-   Ressourcen: Logo- und Splashscreen-Bilder, die von der Anwendung verwendet werden.

## Nächste Schritte


Erweitern Sie basierend auf diesen Grundkenntnissen Ihr Wissen im Bereich der Spielentwicklung und speziell die Fähigkeiten für die Entwicklung von WindowsStore-Spielen.

Wenn Sie ein vorhandenes Spiel portieren, sind die folgenden Themen hilfreich:

-   [Portieren von OpenGL ES 2.0 zu Direct3D 11.1](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [Portieren von DirectX9 zur universellen Windows-Plattform](porting-your-directx-9-game-to-windows-store.md)

Wenn Sie ein neues DirectX-Spiel erstellen, sind die folgenden Themen hilfreich.

-   [Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-metro-style-directx-game.md)
-   [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

> **Hinweis**  
Dieser Artikel ist für Windows10-Entwickler gedacht, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows8.x oder Windows Phone8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 







<!--HONumber=Jun16_HO4-->


