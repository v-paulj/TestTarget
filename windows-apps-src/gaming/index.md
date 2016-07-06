---
author: mtoepke
title: Spiele und DirectX
description: "Die Universelle Windows-Plattform (UWP) bietet neue Möglichkeiten zum Erstellen, Verteilen und Monetisieren von Spielen. Hier erhalten Sie Informationen zum Starten eines neuen Spiels oder Portieren eines vorhandenen Spiels."
ms.assetid: 4073b835-c900-4ff2-9fc5-da52f9432a1f
ms.sourcegitcommit: 41ee0d2a45408b5b1a0dbc0b102f1b59843814b2
ms.openlocfilehash: e5447f6238ece768513d160579e1c7e89b04e509

---

# Spiele und DirectX


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Die Universelle Windows-Plattform (UWP) bietet neue Möglichkeiten zum Erstellen, Verteilen und Monetisieren von Spielen. Hier erhalten Sie Informationen zum Starten eines neuen Spiels oder Portieren eines vorhandenen Spiels.

| Thema | Beschreibung |
|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Handbuch zur Entwicklung von Spielen unter Windows 10](e2e.md) | Ein umfassender Leitfaden für Ressourcen und Informationen zur Entwicklung von UWP-Spielen. |
| [Spieletechnologien für Apps für die universelle Windows-Plattform](game-development-platform-guide.md) | Dieses Handbuch enthält Informationen über verfügbare Technologien für die Entwicklung von UWP-Spielen. |
| [Projektvorlagen und Tools für Spiele](prepare-your-dev-environment-for-windows-store-directx-game-development.md) | Hier erfahren Sie, was erforderlich ist, um mit dem Programmieren von DirectX-Spielen für UWP beginnen zu können. |
| [Das App-Objekt und DirectX](about-the-metro-style-user-interface-and-directx.md) | Für die UWP mit DirectX-Spielen werden nur wenige der UI-Elemente und -objekte der Windows-Benutzeroberfläche genutzt. Da sie auf einer niedrigeren Ebene des Windows-Runtime-Stapels ausgeführt werden, müssen sie stattdessen auf eine grundlegendere Weise mit dem Benutzeroberflächenframework interagieren, und zwar indem sie direkt auf das App-Objekt zugreifen und mit diesem interagieren. Im Folgenden erfahren Sie, zu welchem Zeitpunkt und auf welche Weise eine solche Interaktion erfolgt und wie Sie dieses Modell als DirectX-Entwickler beim Entwickeln von UWP-Apps effizient nutzen können. |
| [Starten und Reaktivieren von Apps](launching-and-resuming-apps-directx-and-cpp.md) | Erfahren Sie, wie Sie Ihre UWP-DirectX-App starten, anhalten und fortsetzen können. |
| [2D-Grafiken für DirectX-Spiele](working-with-2d-graphics-in-your-directx-game.md) | Hier finden Sie Informationen zum Einsatz von 2D-Bitmapgrafiken und -effekten im Allgemeinen und in Ihrem Spiel. |
| [Grundlegendes zu 3D-Grafiken für DirectX-Spiele](an-introduction-to-3d-graphics-with-directx.md) | Im Folgenden zeigen wir Ihnen, wie Sie grundlegende Konzepte von 3D-Grafiken durch die Programmierung mit DirectX umsetzen können. |
| [Laden von Ressourcen im DirectX-Spiel](load-a-game-asset.md) | In den meisten Spielen werden Ressourcen und Objekte (wie Shader, Texturen, vordefinierte Gitter oder andere Grafikdaten) an bestimmten Stellen aus dem lokalen Speicher oder über einen anderen Datenstrom geladen. Dieser Abschnitt enthält eine allgemeine Übersicht über die Aspekte, die beim Laden dieser Dateien zur Verwendung in Ihrem UWP-Spiel zu berücksichtigen sind. |
| [Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-metro-style-directx-game.md) | In diesen Tutorials lernen Sie, wie Sie ein einfaches UWP-Spiel mit DirectX und C++ erstellen. Wir befassen uns mit allen wichtigen Teilen eines Spiels. Hierzu zählen die Prozesse zum Laden von Ressourcen wie Grafiken und Gittern, zum Erstellen einer Hauptschleife für das Spiel, zum Implementieren einer einfachen Rendering-Pipeline sowie zum Hinzufügen von Soundeffekten und Steuerelementen. |
| [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md) | In diesem Abschnitt der Dokumentation wird das Erstellen von UWP-3D-Spielen mit DirectX und Visual C++ beschrieben. Die vorliegende Dokumentation veranschaulicht, wie Sie das 3D-Spiel Marble Maze erstellen, das auf neuen Formfaktoren wie Tablet PCs genau wie auf herkömmlichen Desktopcomputern und Laptops gespielt werden kann. |
| [Unterstützen der Bildschirmausrichtung](supporting-screen-rotation-directx-and-cpp.md) | An dieser Stelle erläutern wir die bewährten Methoden zum Umgang mit der Bildschirmausrichtung in Ihrer UWP-DirectX-App, sodass die Grafikhardware der Windows 10-Geräte effizient und effektiv verwendet wird. |
| [Audio für Spiele](working-with-audio-in-your-directx-game.md) | Hier erfahren Sie, wie Sie Musik und Sound entwickeln und in Ihr DirectX-Spiel integrieren. Außerdem erfahren Sie, wie Sie Audiosignale verarbeiten, um dynamischen und positionsbezogenen Sound zu erzeugen. |
| [Toucheingabesteuerelemente für Spiele](tutorial--adding-touch-controls-to-your-directx-game.md) | Hier erfahren Sie, wie Sie Ihrem UWP-Spiel mit DirectX und C++ einfache touchbasierte Steuerelemente hinzufügen. Wir zeigen Ihnen, wie Sie touchbasierte Steuerelemente hinzufügen, um eine Kamera mit fester Ebene in einer Direct3D-Umgebung zu bewegen, indem die Kameraperspektive durch Bewegen des Fingers oder Eingabestifts geändert wird. |
| [Bewegungs-/Blicksteuerungen für Spiele](tutorial--adding-move-look-controls-to-your-directx-game.md) | Hier erfahren Sie, wie Sie Ihrem DirectX-Spiel herkömmliche Bewegungs-/Blicksteuerungen für Maus und Tastatur (auch als Maussteuerungen bezeichnet) hinzufügen. |
| [Optimieren von Eingabe und Renderschleife](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) | Die Eingabelatenz kann das Spielerlebnis erheblich beeinträchtigen, und Spiele wirken professioneller, wenn in diesem Bereich eine Optimierung vorgenommen wird. Außerdem kann eine richtige Optimierung der Eingabeereignisse zu einer besseren Akkulaufzeit führen. Hier erfahren Sie, wie Sie die richtigen Verarbeitungsoptionen für [CoreDispatcher](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md)-Eingabeereignisse auswählen, um sicherzustellen, dass die Eingaben im Spiel so reibungslos wie möglich verarbeitet werden. |
| [Swapchainskalierung und Überlagerungen](multisampling--scaling--and-overlay-swap-chains.md) | Hier erfahren Sie, wie Sie skalierte Swapchains zum schnelleren Rendern auf mobilen Geräten erstellen und Überlagerungsswapchains (falls verfügbar) verwenden, um die visuelle Qualität zu steigern. |
| [Reduzieren der Latenz mit DXGI 1.3-Swapchains](reduce-latency-with-dxgi-1-3-swap-chains.md) | Verwenden Sie DXGI 1.3 zum Reduzieren der geltenden Framelatenz, indem Sie warten, bis die Swapchain den geeigneten Zeitpunkt zum Beginnen mit dem Rendern eines neuen Frames signalisiert. |
| [Multisampling in UWP-Apps](multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md) | Hier erfahren Sie, wie Sie das Multisampling in UWP-Apps verwenden, die mit Direct3D erstellt wurden. |
| [Behandeln von Szenarien mit entfernten Geräten in Direct3D 11](handling-device-lost-scenarios.md) | In diesem Thema wird erläutert, wie Sie die Geräteschnittstellenkette für Direct3D und DXGI neu erstellen, wenn die Grafikkarte entfernt oder neu initialisiert wird. |
| [Asynchrone Programmierung für Spiele](asynchronous-programming-directx-and-cpp.md) | Dieses Thema behandelt verschiedene Aspekte, die Sie beim Verwenden der asynchronen Programmierung und des Threadings mit DirectX berücksichtigen sollten. |
| [Netzwerkfunktionen für Spiele](work-with-networking-in-your-directx-game.md) | Erfahren Sie, wie Sie Netzwerkfeatures entwickeln und in ein DirectX-Spiel integrieren. |
| [Barrierefreiheit von Spielen](accessibility-for-games.md) | Erfahren Sie, wie Sie die Barrierefreiheit von Spielen verbessern. |
| [Cloud für Spiele](cloud-for-games.md) | Erfahren Sie, wie Sie Cloudtechnologien für die Entwicklung von Spielen nutzen können. |
| [Interoperabilität von DirectX und XAML](directx-and-xaml-interop.md) | In Ihrem UWP-Spiel können Sie eine Kombination aus Extensible Application Markup Language (XAML) und Microsoft DirectX verwenden. |
| [Packen Ihres Spiels](package-your-windows-store-directx-game.md) | Umfangreichere UWP-Spiele können leicht relativ groß werden. Dies gilt besonders für Spiele, bei denen mehrere Sprachen mit regionsspezifischen Ressourcen unterstützt werden oder die über optionale HD-Ressourcen verfügen. In diesem Thema erfahren Sie, wie Sie App-Pakete und App-Bündel zum Anpassen der App verwenden, damit Kunden nur die wirklich benötigten Ressourcen erhalten. |
| [Handbücher zum Portieren von Spielen](porting-guides.md) | Hier finden Sie Anleitungen zum Portieren von vorhandenen Spielen zu Direct3D 11, UWP und Windows 10. |
| [Ressourcen für die Spieleprogrammierung](additional-directx-game-programming-resources.md) | Weitere Informationen zur Spieleprogrammierung unter Windows finden Sie in den folgenden Ressourcen. |

 

> **Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, hilft Ihnen die [archivierte Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132) weiter.

 

Damit Sie die Übersichten und Lernprogramme zur Spieleentwicklung optimal nutzen können, sollten Sie mit den folgenden Themen vertraut sein:

-   Microsoft C++ mit Komponentenerweiterungen (C++/CX). Dies ist ein Update für Microsoft C++, das die automatische Verweiszählung einführt. Außerdem handelt es sich hierbei um die Sprache für die Entwicklung eines UWP-Spiels mit DirectX 11.1 oder einer höheren Version.
-   Grundlegende Terminologie für die Grafikprogrammierung.
-   Grundlegende Konzepte der Windows-Programmierung.
-   Grundlegende Kenntnisse der APIs von Direct3D 9 oder 11.

 

 







<!--HONumber=Jun16_HO4-->


