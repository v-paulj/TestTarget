---
author: mtoepke
title: "Tools für die Grafikdiagnose"
description: "Hier erfahren Sie, wie Sie in Visual Studio die Grafikdiagnosefeatures, einschließlich Grafikdebugging, Analyse von Grafikframes und GPU-Verwendung, abrufen und verwenden."
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: f266cb50893fd37162f21be169d6daf6e37c6bb9

---

# Tools für die Grafikdiagnose


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Unter Windows 10 stehen die Grafikdiagnosetools jetzt als optionales Feature in Windows zur Verfügung. Installieren Sie das optionale Grafiktoolfeature, um die in der Runtime und in Visual Studio bereitgestellten Grafikdiagnosefeatures für die Entwicklung von DirectX-Apps oder -Spielen zu nutzen:

1.  Klicken Sie unter **Einstellungen** >**System** > **Optionale Features** auf **Feature hinzufügen**. Klicken Sie unter **Einstellungen** > **System** > **Apps & Features** > **Optionale Features verwalten** auf **Feature hinzufügen**.
2.  Klicken Sie in der Liste **Feature hinzufügen** auf **Grafiktools**.

Die Grafikdiagnosefeatures ermöglichen das Erstellen von Direct3D-Debugging-Geräten (über Direct3D-SDK-Ebenen) in der DirectX-Runtime sowie das Grafikdebugging, die Frame-Analyse und die GPU-Verwendung.

-   Mit dem Grafikdebugging können Sie Direct3D-Aufrufe Ihrer App nachverfolgen. Anschließend können Sie diese Aufrufe wiedergeben, Parameter untersuchen, Shader debuggen und mit ihnen experimentieren sowie Grafikobjekte visualisieren, um Renderingprobleme zu diagnostizieren. Protokolle können auf Windows-PCs, in Simulatoren oder auf Geräten erstellt und auf anderer Hardware wiedergegeben werden.
-   Die Analyse von Grafikframes in Visual Studio wird für ein Grafikdebuggingprotokoll ausgeführt und erfasst Details der Grundlinienzeitsteuerung für die Direct3D-Draw-Aufrufe. Anschließend führt sie eine Reihe von Versuchen aus, indem sie verschiedene Grafikeinstellungen ändert, und generiert eine Tabelle der Zeitsteuerungsergebnisse. Diese Daten können Aufschluss über Probleme mit der Grafikleistung Ihrer App geben, und anhand der Ergebnisse der verschiedenen Versuche können Sie feststellen, wie sich die Leistung verbessern lässt.
-   Anhand der GPU-Verwendung in Visual Studio können Sie die GPU-Verwendung in Echtzeit überwachen. Sie erfasst und analysiert die Zeitdaten der Arbeitsauslastungen, die von der CPU und GPU verarbeitet werden, und ermöglicht so die Erkennung von Engpässen.

## Verwandte Themen


[Übersicht über die Grafikdiagnose in Visual Studio](http://go.microsoft.com/fwlink/p/?LinkID=526382)

 

 







<!--HONumber=Jun16_HO4-->


