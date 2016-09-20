---
author: mtoepke
title: Asynchrone Programmierung (DirectX und C++)
description: "In diesem Thema werden verschiedene Aspekte behandelt, die Sie beim Verwenden der asynchronen Programmierung und des Threadings mit DirectX berücksichtigen sollten."
ms.assetid: 17613cd3-1d9d-8d2f-1b8d-9f8d31faaa6b
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 67a75e9d1324e7ac50e0575bfd7bda870a87efb2

---

# Asynchrone Programmierung (DirectX und C++)


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieses Thema behandelt verschiedene Aspekte, die Sie beim Verwenden der asynchronen Programmierung und des Threadings mit DirectX berücksichtigen sollten.

## Asynchrone Programmierung und DirectX


Wenn Sie gerade erste Erfahrungen mit DirectX sammeln oder wenn Sie sogar bereits mit DirectX vertraut sind, sollten Sie Ihre gesamte Grafikverarbeitungspipeline in einen Thread einfügen. In jeder beliebigen Szene eines Spiels gibt es allgemeine Ressourcen wie Bitmaps, Shader und andere Objekte, die einen exklusiven Zugriff erfordern. Genau für diese Ressourcen müssen Sie den Zugriff über parallele Threads hinweg synchronisieren. Es ist schwierig, das Rendering über mehrere Threads zu parallelisieren.

Wenn Ihr Spiel ausreichend komplex ist oder Sie die Leistung verbessern möchten, können Sie mithilfe der asynchronen Programmierung einige der Komponenten parallelisieren, die nicht für Ihre Renderingpipeline spezifisch sind. Moderne Hardware ist mit Hyperthreading-CPUs mit mehreren Kernen ausgestattet, und Ihre App sollte sich diese Leistungsmerkmale zu Nutze machen! Sie können dies sicherstellen, indem Sie eine asynchrone Programmierung für einige der Komponenten Ihres Spiels verwenden, die keinen direkten Zugriff auf den Direct3D-Gerätekontext benötigen. Hierzu gehören:

-   Datei-E/A
-   Physische Effekte
-   Künstliche Intelligenz (KI)
-   Netzwerk
-   Audio
-   Steuerelemente
-   XAML-basierte UI-Komponenten

Ihre App kann diese Komponenten in mehreren gleichzeitigen Threads verwalten. Das asynchrone Laden wirkt sich positiv auf die Datei-E/A aus, insbesondere auf das Laden von Objekten, da Ihr Spiel oder Ihre App sich in einem interaktiven Zustand befinden kann, während mehrere (oder mehrere Hundert) Megabytes von Objekten geladen oder gestreamt werden. Es ist am einfachsten, diese Threads mit der [Parallel Patterns Library (PPL)](https://msdn.microsoft.com/library/dd492418.aspx) und dem **task**-Muster zu erstellen und zu verwalten, die in dem in der Datei „PPLTasks.h“ definierten **concurrency**-Namespace enthalten sind. Wenn Sie die [Parallel Patterns Library](https://msdn.microsoft.com/library/dd492418.aspx) verwenden, können Sie direkt von den Hyperthreading-CPUs mit mehreren Kernen profitieren und verschiedene Verbesserungen erzielen, z. B. durch verkürzte Ladezeiten oder weniger Störungen und Verzögerungen bei umfassenden CPU-Berechnungen oder Netzwerkverarbeitung.

> 
            **Hinweis**  In einer App für die universelle Windows-Plattform (UWP) wird die Benutzeroberfläche komplett in einem Singlethread-Apartment (STA) ausgeführt. Wenn Sie eine UI für Ihr DirectX-Spiel mit [XAML-Interoperabilität](directx-and-xaml-interop.md) erstellen, können Sie nur mithilfe von STA auf die Steuerelemente zugreifen.

 

## Multithreading mit Direct3D-Geräten


Multithreading für Gerätekontexte ist nur für Grafikgeräte verfügbar, die eine Direct3D-Featureebene von 11\_0 oder höher unterstützen. Sie können jedoch die Nutzung des leistungsfähigen Grafikprozessors (GPU) auf vielen Plattformen maximieren, z. B. auf speziellen Gaming-Plattformen. Im einfachsten Fall möchten Sie vielleicht das Rendering einer HUD-Überlagerung (Heads-up-Display) vom Rendern der 3D-Szene trennen, sodass beide Komponenten separate parallele Pipelines verwenden. Beide Threads müssen jedoch dasselbe Singlethread-[**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)-Element verwenden, um Ressourcenobjekte (Texturen, Gitter, Shader und andere Objekte) zu erstellen und zu verwalten. Dies erfordert, dass Sie für einen sicheren Zugriff eine Art von Synchronisierungsmechanismus implementieren (z. B. wichtige Abschnitte). Sie können zwar separate Befehlslisten für den Gerätekontext in verschiedenen Threads (für verzögertes Rendering) erstellen, diese Befehlslisten aber nicht gleichzeitig in derselben **ID3D11DeviceContext**-Instanz wiedergeben.

Jetzt kann Ihre App zum Erstellen von Ressourcenobjekten auch das [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)-Element verwenden, das problemlos für Multithreading eingesetzt werden kann. Warum also nicht immer **ID3D11Device** anstatt [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) verwenden? Nun, derzeit steht nicht für alle Grafikoberflächen eine Treiberunterstützung für Multithreading zur Verfügung. Sie können das Gerät abfragen und herausfinden, ob es Multithreading unterstützt. Wenn Sie aber die größtmögliche Zielgruppe erreichen möchten, sollten Sie das Singlethread-**ID3D11DeviceContext**-Element zur Verwaltung von Ressourcenobjekten verwenden. Wenn allerdings der Grafikgerätetreiber Multithreading oder Befehlslisten nicht unterstützt, versucht Direct3D 11, den synchronisierten Zugriff auf den Gerätekontext intern zu behandeln. Und wenn keine Befehlslisten unterstützt werden, wird eine Softwareimplementierung bereitgestellt. So können Sie Multithreadcode schreiben, der auf Plattformen mit Grafikoberflächen ausgeführt wird, die keine Treiberunterstützung für den Zugriff auf Multithread-Gerätekontexte bieten.

Wenn Ihre App separate Threads für die Verarbeitung von Befehlslisten und zum Anzeigen von Frames unterstützt, möchten Sie wahrscheinlich, dass der Grafikprozessor (GPU) aktiv bleibt. Er soll die Befehlslisten verarbeiten und gleichzeitig Frames zeitnah ohne Störungen oder Verzögerungen anzeigen. In diesem Fall können Sie ein separates [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)-Element für jeden Thread verwenden und Ressourcen (wie Texturen) freigeben, indem Sie diese mit dem D3D11\_RESOURCE\_MISC\_SHARED-Kennzeichen erstellen. In diesem Szenario muss [**ID3D11DeviceContext::Flush**](https://msdn.microsoft.com/library/windows/desktop/ff476425) für den Verarbeitungsthread aufgerufen werden, um die Ausführung der Befehlsliste abzuschließen, bevor die Ergebnisse der Ressourcenobjektverarbeitung im Anzeigethread angezeigt werden.

## Verzögertes Rendering


Beim verzögerten Rendering werden Grafikbefehle in einer Befehlsliste aufgezeichnet, damit sie zu einem anderen Zeitpunkt ausgeführt werden können. Hierbei wird das Rendering in einem Thread ausgeführt, während die Aufzeichnung der Befehle zum Rendern in zusätzlichen Threads erfolgt. Nachdem diese Befehle abgeschlossen wurden, können sie in dem Thread ausgeführt werden, der das endgültige Anzeigeobjekt (Framepuffer, Textur oder andere Grafikausgabe) generiert.

Erstellen Sie mit [**ID3D11Device::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/ff476505) einen Kontext für verzögertes Rendering (anstatt mit [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) oder [**D3D11CreateDeviceAndSwapChain**](https://msdn.microsoft.com/library/windows/desktop/ff476083), die einen Kontext für das sofortige Rendering erstellen). Weitere Informationen finden Sie im Thema zum [Sofortigen und verzögerten Rendering](https://msdn.microsoft.com/library/windows/desktop/ff476892).

## Verwandte Themen


* [Einführung in das Multithreading in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476891)

 

 







<!--HONumber=Jun16_HO4-->


