---
author: mtoepke
title: Exemplarische Vorgehensweise - Implementieren von Schattenvolumen mit Tiefenpuffern in Direct3D 11
description: "In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie Schattenvolumen mit Tiefenkarten unter Verwendung von Direct3D 11 auf Geräten aller Direct3D-Funktionsebenen rendern."
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 4bb1f5c71d83b2dc3271b5b2ceaa9a1156b00004

---

# Exemplarische Vorgehensweise - Implementieren von Schattenvolumen mit Tiefenpuffern in Direct3D 11


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie Schattenvolumen mit Tiefenkarten unter Verwendung von Direct3D 11 auf Geräten aller Direct3D-Funktionsebenen rendern.
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
<td align="left"><p>[Erstellen von Tiefenpuffer-Geräteressourcen](create-depth-buffer-resource--view--and-sampler-state.md)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die zum Unterstützen von Tiefentests für Schattenvolumen erforderlichen Direct3D-Geräteressourcen erstellen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Rendern der Schattenmap zum Tiefenpuffer](render-the-shadow-map-to-the-depth-buffer.md)</p></td>
<td align="left"><p>Führen Sie das Rendern aus dem Blickwinkel durch, aus dem das Licht kommt, um eine zweidimensionale Tiefenkarte zu erstellen, mit der das Schattenvolumen dargestellt wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Rendern der Szene mit Tiefentest](render-the-scene-with-depth-testing.md)</p></td>
<td align="left"><p>Erstellen Sie einen Schatteneffekt, indem Sie dem Vertex-Shader (bzw. Geometry-Shader) und dem Pixel-Shader einen Tiefentest hinzufügen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Unterstützen von Schattenmaps für unterschiedliche Hardware](target-a-range-of-hardware.md)</p></td>
<td align="left"><p>Rendern Sie Schatten in noch besserer Qualität auf schnelleren Geräten und schnellere Schatten auf weniger leistungsfähigen Geräten.</p></td>
</tr>
</tbody>
</table>

 

## Schattenabbildung – Portieren von Direct3D 9-Desktop-Apps


In Windows 8 wurden Featureebene 9\_1 und 9\_3 Funktionen für den Tiefenvergleich hinzugefügt. Jetzt können Sie Renderingcode mit Schattenvolumen zu DirectX 11 migrieren. Der Direct3D 11-Renderer ist abwärtskompatibel mit Geräten der Featureebene 9. In dieser exemplarischen Vorgehensweise zeigen wir, wie herkömmliche Schattenvolumen mit Tiefentests in Direct3D 11-Apps oder -Spielen implementiert werden können. Der Code umfasst die folgenden Prozesse:

1.  Erstellen von Direct3D-Geräteressourcen für die Schattenabbildung
2.  Hinzufügen eines Renderingdurchgangs zum Erstellen der Tiefenkarte
3.  Hinzufügen von Tiefentests zum Hauptrenderingdurchgang
4.  Implementieren des erforderlichen Shadercodes
5.  Optionen für schnelles Rendering auf kompatibler Hardware

Nach Abschluss dieser exemplarischen Vorgehensweise wissen Sie, wie Sie eine einfache kompatible Schattenvolumentechnik in Direct3D 11 implementieren, die mit Funktionsebene 9\_1 und höher kompatibel ist.

## Voraussetzungen


Führen Sie die Schritte unter [Vorbereiten der Entwicklungsumgebung für die Entwicklung von Spielen für die universelle Windows-Plattform (UWP) und DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md) aus. Sie benötigen noch keine Vorlage, aber Microsoft Visual Studio 2015, um das Codebeispiel für diese exemplarische Vorgehensweise erstellen zu können.

## Verwandte Themen


**Direct3D**

* [Schreiben von HLSL-Shadern in Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [Erstellen eines neuen DirectX 11-Projekts für die UWP](user-interface.md)

**Technische Artikel zur Schattenabbildung**

* [Allgemeine Artikel zum Verbessern von Tiefenkarten für Schatten](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [Überlappende Schattenkarten](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 







<!--HONumber=Jun16_HO4-->


