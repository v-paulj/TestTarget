---
author: mtoepke
title: Exemplarische Vorgehensweise Portieren einer einfachen Direct3D 9-App zu DirectX 11 und zur universellen Windows-Plattform (UWP)
description: "In dieser Portierungsübung wird veranschaulicht, wie Sie ein einfaches Renderingframework von Direct3D9 auf Direct3D11 und die universelle Windows-Plattform (UWP) umstellen."
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 27c6f82e97e9eb24dedcc5d83a18e6aba6961194

---

# Exemplarische Vorgehensweise Portieren einer einfachen Direct3D 9-App zu DirectX 11 und zur universellen Windows-Plattform (UWP)


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In dieser Portierungsübung wird veranschaulicht, wie Sie ein einfaches Renderingframework von Direct3D9 auf Direct3D11 und die universelle Windows-Plattform (UWP) umstellen.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Initialisieren von Direct3D11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)</p></td>
<td align="left"><p>Hier wird veranschaulicht, wie Sie Direct3D 9-Initialisierungscode in Direct3D 11 konvertieren, und Sie erfahren, wie Sie Handles zum Direct3D-Gerät und zum Gerätekontext abrufen und DXGI zum Einrichten einer Swapchain verwenden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Konvertieren des Renderingframeworks](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)</p></td>
<td align="left"><p>Hier wird veranschaulicht, wie Sie ein einfaches Renderingframework von Direct3D9 in Direct3D11 konvertieren. Sie erfahren, wie Sie Geometriepuffer portieren, HLSL-Shaderprogramme kompilieren und laden und die Renderkette in Direct3D11 implementieren.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Portieren der Spielschleife](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)</p></td>
<td align="left"><p>In diesem Thema wird veranschaulicht, wie Sie ein Fenster für ein UWP-Spiel (Universelle Windows-Plattform) implementieren und die Spielschleife übertragen. Außerdem wird die Erstellung von [<strong>IFrameworkView</strong>] (https://msdn.microsoft.com/library/windows/apps/hh700478) zum Steuern der [<strong>CoreWindow</strong>]-Vollbildansicht (https://msdn.microsoft.com/library/windows/apps/br208225) erläutert.</p></td>
</tr>
</tbody>
</table>

 

Es werden zwei Codepfade durchgegangen, mit denen jeweils die gleiche grundlegende Grafikaufgabe durchgeführt wird: das Anzeigen eines sich drehenden Würfels mit Vertexschattierung. In beiden Fällen wird mit dem Code der folgende Prozess abgedeckt:

1.  Erstellen eines Direct3D-Geräts und einer Swapchain
2.  Erstellen eines Vertexpuffers und eines Indexpuffers zum Darstellen eines farbigen Würfelgitters
3.  Erstellen eines Vertexshaders, mit dem Vertizes in Bildschirmbereiche umgewandelt werden, eines Pixelshaders, mit dem Farbwerte vermischt werden, Kompilieren der Shader und Laden der Shader als Direct3D-Ressourcen
4.  Implementieren der Renderkette und Darstellen des gezeichneten Würfels auf dem Bildschirm
5.  Erstellen eines Fensters, Starten einer Hauptschleife und Durchführen der Verarbeitung von Bildschirmmeldungen

Nach der Durcharbeitung dieser exemplarischen Vorgehensweise sollten Ihnen die folgenden grundlegenden Unterschiede zwischen Direct3D9 und Direct3D11 bekannt sein:

-   Trennung von Gerät, Gerätekontext und Grafikinfrastruktur
-   Kompilierungsprozess für Shader und Laden von Shaderbytecode zur Laufzeit
-   Konfigurieren von Daten pro Vertex für den Eingabeassemblerzustand
-   Verwenden eines [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)-Elements zum Erstellen einer CoreWindow-Ansicht

Beachten Sie, dass in dieser exemplarischen Vorgehensweise der Einfachheit halber [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) verwendet wird. Die XAML-Interoperabilität wird nicht behandelt.

## Voraussetzungen


Führen Sie die Schritte unter [Vorbereiten der Entwicklungsumgebung für die Entwicklung von UWP-DirectX-Spielen](prepare-your-dev-environment-for-windows-store-directx-game-development.md) aus. Sie müssen noch keine Vorlage laden, aber Sie benötigen Microsoft Visual Studio 2015, um die Codebeispiele für diese exemplarische Vorgehensweise zu laden.

Lesen Sie sich die Informationen unter [Konzepte und Aspekte der Portierung](porting-considerations.md) durch, um ein besseres Verständnis der in dieser exemplarischen Vorgehensweise verwendeten Programmierkonzepte für DirectX11 und UWP zu entwickeln.

## Verwandte Themen


**Direct3D**
          [Schreiben von HLSL-Shadern in Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
          

[Erstellen eines neuen DirectX11-Projekts für die UWP](user-interface.md)

**Windows Store**
          [**Microsoft::WRL::ComPtr**
            ](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)
          

[**Handle to Object Operator (^)**]https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx.

 

 







<!--HONumber=Jun16_HO4-->


