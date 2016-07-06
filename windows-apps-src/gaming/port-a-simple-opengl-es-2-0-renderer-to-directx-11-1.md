---
author: mtoepke
title: Portieren eines einfachen OpenGL ES 2.0-Renderers zu Direct3D 11
description: "Bei der ersten Portierungsübung beginnen wir mit den Grundlagen - Umstellen eines einfachen Renderers für einen sich drehenden Würfel mit Vertexschattierungen von OpenGL ES 2.0 auf Direct3D, damit er der Vorlage DirectX 11-App (Universelle Windows-App) aus Visual Studio 2015 entspricht."
ms.assetid: e7f6fa41-ab05-8a1e-a154-704834e72e6d
ms.sourcegitcommit: 814f056eaff5419b9c28ba63cf32012bd82cc554
ms.openlocfilehash: f70d4ec46743d930f8cb45084e55cce2e60e2460

---

# Portieren eines einfachen OpenGL ES 2.0-Renderers zu Direct3D 11


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In dieser Portierungsübung beginnen wir mit den Grundlagen: Umstellen eines einfachen Renderers für einen sich drehenden Würfel mit Vertexschattierungen von OpenGL ES 2.0 auf Direct3D, damit er der Vorlage „DirectX 11-App (Universelle Windows-App)“ aus Visual Studio 2015 entspricht. Beim Durcharbeiten dieses Portierungsprozesses lernen Sie Folgendes:

-   Portieren einer einfachen Gruppe von Vertexpuffern zu Direct3D-Eingabepuffern
-   Portieren von uniform-Elementen und Attributen zu Konstantenpuffern
-   Konfigurieren von Direct3D-Shaderobjekten
-   Verwenden von HLSL-Semantik bei der Entwicklung von Direct3D-Shadern
-   Portieren einfacher GLSL zu HLSL

Dieses Thema beginnt nach der Erstellung eines neuen DirectX 11-Projekts. Informationen zum Erstellen eines neuen DirectX 11-Projekts finden Sie unter [Erstellen eines neuen DirectX 11-Projekts für die Universelle Windows-Plattform (UWP)](user-interface.md).

Das über einen dieser Links erstellte Projekt verfügt über den gesamten vorbereiteten Code für die [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476345)-Infrastruktur. Sie können sofort damit beginnen, den Renderer von OpenGL ES 2.0 zu Direct3D 11 zu portieren.

Es werden zwei Codepfade durchgegangen, mit denen jeweils die gleiche grundlegende Grafikaufgabe durchgeführt wird: das Anzeigen eines sich drehenden Würfels mit Vertexschattierung in einem Fenster. In beiden Fällen wird mit dem Code der folgende Prozess abgedeckt:

1.  Erstellen eines Würfelgitters aus hartcodierten Daten. Dieses Gitter wird als Liste mit Scheitelpunkten (Vertices) dargestellt, wobei jeder Scheitelpunkt (Vertex) über eine Position, einen Normalenvektor und einen Farbvektor verfügt. Dieses Gitter wird in einen Vertexpuffer eingefügt, der von der Schattierungspipeline verarbeitet wird.
2.  Erstellen von Shaderobjekten zum Verarbeiten des Würfelgitters. Es gibt zwei Arten von Shadern: einen Vertex-Shader, der die Scheitelpunkte für die Rasterung verarbeitet, und einen Fragmentshader (oder Pixelshader), mit dem die einzelnen Pixel des Würfels nach der Rasterung eingefärbt werden. Diese Pixel werden zur Anzeige in ein Renderziel geschrieben.
3.  Erstellen der Schattierungssprache, die in den Vertex- bzw. Fragmentshadern jeweils für die Vertex- und Pixelverarbeitung verwendet wird.
4.  Anzeige des gerenderten Würfels auf dem Bildschirm.

![Einfacher OpenGL-Würfel](images/simple-opengl-cube.png)

Nach der Durcharbeitung dieser exemplarischen Vorgehensweise sollten Ihnen die folgenden grundlegenden Unterschiede zwischen OpenGL ES 2.0 und Direct3D 11 bekannt sein:

-   Darstellung der Vertexpuffer und Vertexdaten
-   Prozess der Erstellung und Konfiguration von Shadern
-   Schattierungssprachen und die Ein- und Ausgaben von Shaderobjekten
-   Verhalten beim Zeichnen auf dem Bildschirm

In dieser exemplarischen Vorgehensweise wird auf eine einfache und generische OpenGL-Rendererstruktur verwiesen, die wie folgt definiert ist:

``` syntax
typedef struct 
{
    GLfloat pos[3];        
    GLfloat rgba[4];
} Vertex;

typedef struct
{
  // Integer handle to the shader program object.
  GLuint programObject;

  // The vertex and index buffers
  GLuint vertexBuffer;
  GLuint indexBuffer;

  // Handle to the location of model-view-projection matrix uniform
  GLint  mvpLoc; 
   
  // Vertex and index data
  Vertex  *vertices;
  GLuint   *vertexIndices;
  int       numIndices;

  // Rotation angle used for animation
  GLfloat   angle;

  GLfloat  mvpMatrix[4][4]; // the model-view-projection matrix itself
} Renderer;
```

Diese Struktur verfügt über eine Instanz und enthält alle erforderlichen Komponenten zum Rendern eines sehr einfach aufgebauten Gitters mit Vertexschattierung.

> **Hinweis**  Der in diesem Thema verwendete OpenGL ES 2.0-Code basiert auf der Windows-API-Implementierung der Khronos Group, und es wird die Programmiersyntax von Windows C verwendet.

 

## Wissenswertes


### Technologien

-   [Microsoft Visual C++](http://msdn.microsoft.com/library/vstudio/60k1461a.aspx)
-   OpenGL ES 2.0

### Voraussetzungen

-   Optional. Sehen Sie sich das Thema [Portieren von EGL-Code zu DXGI und Direct3D](moving-from-egl-to-dxgi.md) an. Lesen Sie sich die Informationen dieses Themas durch, um ein besseres Verständnis der von DirectX bereitgestellten Grafikschnittstelle zu entwickeln.

## <span id="keylinks_steps_heading"></span>Schritte


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
<td align="left"><p>[Portieren der Shaderobjekte](port-the-shader-config.md)</p></td>
<td align="left"><p>Beim Portieren des einfachen Renderers aus OpenGL ES 2.0 ist der erste Schritt das Einrichten der äquivalenten Vertex- und Fragmentshaderobjekte in Direct3D 11. Stellen Sie außerdem sicher, dass das Hauptprogramm mit den Shaderobjekten kommunizieren kann, nachdem diese kompiliert wurden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Portieren der Vertexpuffer und -daten](port-the-vertex-buffers-and-data-config.md)</p></td>
<td align="left"><p>In diesem Schritt definieren Sie die Vertexpuffer für die Gitter und die Indexpuffer, mit deren Hilfe die Shader die Scheitelpunkte (Vertices) in einer angegebenen Reihenfolge durchlaufen können.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Portieren des GLSL-Codes](port-the-glsl.md)</p></td>
<td align="left"><p>Nachdem Sie sich um den Code gekümmert haben, mit dem die Puffer und Shaderobjekte erstellt und konfiguriert werden, muss der in diesen Shadern enthaltene Code von der GL Shader Language (GLSL) von OpenGL ES 2.0 in die High-Level Shader Language (HLSL) von Direct3D 11 portiert werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Zeichnen auf den Bildschirm](draw-to-the-screen.md)</p></td>
<td align="left"><p>Schließlich wird der Code portiert, der den sich drehenden Würfel auf den Bildschirm zeichnet.</p></td>
</tr>
</tbody>
</table>

 

## <span id="additional_resources"></span>Weitere Ressourcen


-   [Vorbereiten der Entwicklungsumgebung für die Entwicklung von UWP-DirectX-Spielen](prepare-your-dev-environment-for-windows-store-directx-game-development.md)
-   [Erstellen eines neuen DirectX 11-Projekts für UWP](user-interface.md)
-   [Zuordnen von OpenGL ES 2.0-Konzepten und -Infrastruktur zu Direct3D 11](map-concepts-and-infrastructure.md)

 

 







<!--HONumber=Jun16_HO5-->


