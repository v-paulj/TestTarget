---
author: mtoepke
title: GLSL-zu-HLSL-Referenz
description: "Sie portieren Ihren OpenGL Shader Language (GLSL)-Code zu Microsoft High Level Shader Language (HLSL)-Code, wenn Sie Ihre Grafikarchitektur von OpenGL ES 2.0 zu Direct3D 11 portieren, um ein Spiel für die universelle Windows-Plattform (UWP) zu erstellen."
ms.assetid: 979d19f6-ef0c-64e4-89c2-a31e1c7b7692
translationtype: Human Translation
ms.sourcegitcommit: ba620bc89265cbe8756947e1531759103c3cafef
ms.openlocfilehash: 02a3ba1768b6fa7b09b6c9f637a72d88c0cef604

---

# GLSL-zu-HLSL-Referenz


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Sie portieren Ihren OpenGL Shader Language (GLSL)-Code zu Microsoft High Level Shader Language (HLSL)-Code, wenn Sie Ihre [Grafikarchitektur von OpenGL ES 2.0 zu Direct3D 11 portieren](port-from-opengl-es-2-0-to-directx-11-1.md), um ein Spiel für die universelle Windows-Plattform (UWP) zu erstellen. Die hier verwendete GLSL ist kompatibel mit OpenGL ES 2.0, und die HLSL ist kompatibel mit Direct3D 11. Informationen zu den Unterschieden zwischen Direct3D 11 und Vorgängerversionen von Direct3D finden Sie unter [Funktionszuordnung](feature-mapping.md).

-   [Vergleich zwischen OpenGL ES 2.0 und Direct3D 11](#compare)
-   [Portieren von GLSL-Variablen zu HLSL](#variables)
-   [Portieren von GLSL-Typen zu HLSL](#types)
-   [Portieren von vordefinierten globalen GLSL-Variablen zu HLSL](#porting_glsl_pre-defined_global_variables_to_hlsl)
-   [Beispiele für das Portieren von GLSL-Variablen zu HLSL](#example1)
    -   [Uniform-Variable, Attribut und variierende Variable in GLSL](#uniform___attribute__and_varying_in_glsl)
    -   [Konstantenpuffer und Datenübertragungen in HLSL](#constant_buffers_and_data_transfers_in_hlsl)
-   [Beispiele für das Portieren von OpenGL-Renderingcode zu Direct3D](#example2)
-   [Verwandte Themen](#related_topics)

## Vergleich zwischen OpenGL ES 2.0 und Direct3D 11


OpenGL ES 2.0 und Direct3D 11 sind sich in vielen Bereichen ähnlich. Beide verwenden ähnliche Renderingpipelines und Grafikfunktionen. Bei Direct3D 11 handelt es sich jedoch um eine Renderingimplementierung und API und nicht um eine Spezifikation. OpenGL ES 2.0 ist dagegen eine Renderingspezifikation und API und keine Implementierung. Folgende allgemeine Unterschiede bestehen zwischen Direct3D 11 und OpenGL ES 2.0:

| OpenGL ES 2.0                                                                                         | Direct3D 11                                                                                                            |
|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Hardware- und betriebssystemagnostische Spezifikation mit vom Anbieter bereitgestellten Implementierungen.             | Microsoft-Implementierung der Hardwareabstraktion und -zertifizierung auf Windows-Plattformen.                                |
| Allgemein gehalten, um unterschiedliche Hardware zu unterstützen. Die meisten Ressourcen werden von der Runtime verwaltet.                                     | Direkter Zugriff auf das Hardwarelayout. Ressourcen und Verarbeitung können von der App verwaltet werden.                                              |
| Stellt Module auf höherer Ebene über Drittanbieterbibliotheken bereit (z. B. Simple DirectMedia Layer, SDL). | Module auf höherer Ebene wie Direct2D werden basierend auf Modulen auf niedrigerer Eben erstellt, um die Entwicklung für Windows-Apps zu vereinfachen.             |
| Unterscheidung von Hardwareanbietern anhand von Erweiterungen.                                                         | Microsoft fügt der API optionale Funktionen in generischer Form hinzu, sodass sie nicht speziell für einen bestimmten Hardwareanbieter gelten. |

 

Folgende allgemeine Unterschiede bestehen zwischen GLSL und HLSL:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL</th>
<th align="left">HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Prozedural, auf Schritte ausgerichtet (ähnlich wie C)</td>
<td align="left">Objektorientiert, auf Daten ausgerichtet (ähnlich wie C++)</td>
</tr>
<tr class="even">
<td align="left">In die Grafik-API integrierte Shaderkompilierung</td>
<td align="left">Der HLSL-Compiler [kompiliert den Shader](https://msdn.microsoft.com/library/windows/desktop/bb509633) in eine binäre Zwischendarstellung, bevor er von Direct3D an den Treiber übergeben wird.
<div class="alert">
<strong>Hinweis</strong>  Diese binäre Darstellung ist hardwareunabhängig. Sie wird normalerweise beim Erstellen der App und nicht zur Laufzeit kompiliert.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">[Variable](#variables) Speichermodifizierer</td>
<td align="left">Konstantenpuffer und Datenübertragungen über Eingabelayoutdeklarationen</td>
</tr>
<tr class="even">
<td align="left"><p>[Typen](#types)</p>
<p>Typischer Vektortyp: vec2/3/4</p>
<p>lowp, mediump, highp</p></td>
<td align="left"><p>Typischer Vektortyp: float2/3/4</p>
<p>min10float, min16float</p></td>
</tr>
<tr class="odd">
<td align="left">texture2D [Funktion]</td>
<td align="left">[texture.Sample](https://msdn.microsoft.com/library/windows/desktop/bb509695) [Datentyp.Funktion]</td>
</tr>
<tr class="even">
<td align="left">sampler2D [Datentyp]</td>
<td align="left">[Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525) [Datentyp]</td>
</tr>
<tr class="odd">
<td align="left">Zeilenmatrizen (Standard)</td>
<td align="left">Spaltenmatrizen (Standard)
<div class="alert">
<strong>Hinweis</strong>   Verwenden Sie den <strong>row_major</strong>-Typmodifizierer, um das Layout für eine Variable zu ändern. Weitere Informationen finden Sie unter [Variablensyntax](https://msdn.microsoft.com/library/windows/desktop/bb509706). Sie können auch ein Compilerkennzeichen oder ein Pragma angeben, um den globalen Standardwert zu ändern.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Fragment-Shader</td>
<td align="left">Pixelshader</td>
</tr>
</tbody>
</table>

 

> **Hinweis**  In HLSL sind Texturen und Sampler zwei separate Objekte. In GLSL ist die Texturbindung wie bei Direct3D 9 Teil des Samplerstatus.

 

In GLSL stellen Sie einen Großteil des OpenGL-Status in Form von vordefinierten globalen Variablen dar. Bei GLSL verwenden Sie z. B. die **gl\_Position**-Variable zum Angeben der Vertexposition und die **gl\_FragColor**-Variable zum Angeben der Fragmentfarbe. In HLSL übergeben Sie den Direct3D-Status explizit vom App-Code an den Shader. Bei Direct3D und HLSL muss die Eingabe für den Vertex-Shader z. B. dem Datumsformat im Scheitelpunktpuffer entsprechen, und die Struktur eines Konstantenpuffers im App-Code muss mit der Struktur eines Konstantenpuffers ([cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581)) im Shadercode übereinstimmen.

## Portieren von GLSL-Variablen zu HLSL


In GLSL wenden Sie Modifizierer (Qualifizierer) auf eine globale Shadervariablendeklaration an, um in den Shadern ein bestimmtes Verhalten der Variable zu erhalten. In HLSL sind diese Modifizierer nicht erforderlich, weil Sie den Shaderfluss mit den Argumenten definieren, die Sie an den Shader übergeben und vom Shader zurückgeben.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Verhalten von GLSL-Variablen</th>
<th align="left">HLSL-Äquivalent</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>uniform-Variable</strong></p>
<p>Sie übergeben eine Uniform-Variable vom App-Code in die Vertex- und/oder Fragment-Shader. Sie müssen die Werte aller Uniform-Variablen festlegen, bevor Sie Dreiecke mit den Shadern zeichnen, damit ihre Werte gleich bleiben, während ein Dreieckgitter gezeichnet wird. Diese Werte sind einheitlich. Einige Uniform-Variablen werden für den gesamten Frame festgelegt und andere speziell für ein bestimmtes Vertex-/Pixelshaderpaar.</p>
<p>Uniform-Variablen sind Variablen, die pro Polygon gelten.</p></td>
<td align="left"><p>Verwenden Sie einen Konstantenpuffer.</p>
<p>Siehe [So wird's gemacht: Erstellen eines Konstantenpuffers](https://msdn.microsoft.com/library/windows/desktop/ff476896) und [Shaderkonstanten](https://msdn.microsoft.com/library/windows/desktop/bb509581).</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>variierende Variable</strong></p>
<p>Sie initialisieren eine variierende Variable im Vertex-Shader und übergeben sie an eine identisch benannte variierende Variable im Fragment-Shader. Da der Vertex-Shader nur den Wert der variierenden Variablen an jedem Scheitelpunkt festlegt, werden diese Werte vom Rasterizer (perspektivisch korrekt) interpoliert, um Pro-Fragment-Werte zu generieren, die dann in den Fragment-Shader übergeben werden. Diese Variablen sind bei jedem Dreieck unterschiedlich.</p></td>
<td align="left">Verwenden Sie die vom Vertex-Shader zurückgegebene Struktur als Eingabe für den Pixelshader. Stellen Sie sicher, dass die Semantikwerte übereinstimmen.</td>
</tr>
<tr class="odd">
<td align="left"><p><strong>Attribut</strong></p>
<p>Ein Attribut ist Teil der Beschreibung eines Scheitelpunkts, die Sie vom App-Code an den Vertex-Shader übergeben. Anders als bei einer Uniform-Variable legen Sie den Wert jedes Attributs für jeden Scheitelpunkt fest, sodass jeder Scheitelpunkt einen anderen Wert haben kann. Attributvariablen sind Variablen, die pro Scheitelpunkt gelten.</p></td>
<td align="left"><p>Definieren Sie einen Scheitelpunktpuffer in Ihrem Direct3D-App-Code, und passen Sie ihn an die im Vertex-Shader definierte Scheitelpunkteingabe an. Optional können Sie einen Indexpuffer definieren. Siehe [So wird's gemacht: Erstellen eines Scheitelpunktpuffers](https://msdn.microsoft.com/library/windows/desktop/ff476899) und [So wird's gemacht: Erstellen eines Indexpuffers](https://msdn.microsoft.com/library/windows/desktop/ff476897).</p>
<p>Erstellen Sie ein Eingabelayout in Ihrem Direct3D-App-Code, und passen Sie die Semantikwerte an die Werte in der Scheitelpunkteingabe an. Siehe [Erstellen des Eingabelayouts](https://msdn.microsoft.com/library/windows/desktop/bb205117#Create_the_Input_Layout).</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>const</strong></p>
<p>Konstanten werden in den Shader kompiliert und ändern sich nie.</p></td>
<td align="left">Verwenden Sie <strong>static const</strong>. <strong>static</strong> bedeutet, dass der Wert nicht für Konstantenpuffer verfügbar gemacht wird, und <strong>const</strong> bedeutet, dass der Wert nicht vom Shader geändert werden kann. Der Wert wird also zur Kompilierzeit anhand seines Initialisierers bekannt gemacht.</td>
</tr>
</tbody>
</table>

 

In GLSL sind Variablen ohne Modifizierer einfach normale globale Variablen, die für jeden Shader privat sind.

Wenn Sie Daten an Texturen ([Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525) in HLSL) und ihre zugehörigen Sampler ([SamplerState](https://msdn.microsoft.com/library/windows/desktop/bb509644) in HLSL) übergeben, deklarieren Sie sie in der Regel als globale Variablen im Pixelshader.

## Portieren von GLSL-Typen zu HLSL


Ziehen Sie beim Portieren Ihrer GLSL-Typen zu HLSL die folgende Tabelle zurate.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL-Typ</th>
<th align="left">HLSL-Typ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Skalare Typen: float, int, bool</td>
<td align="left"><p>Skalare Typen: float, int, bool</p>
<p>also, uint, double</p>
<p>Weitere Informationen finden Sie unter [Skalare Typen](https://msdn.microsoft.com/library/windows/desktop/bb509646).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Vektortyp</p>
<ul>
<li>Gleitkommavektor: vec2, vec3, vec4</li>
<li>Boolescher Vektor: bvec2, bvec3, bvec4</li>
<li>Ganzzahlvektor mit Vorzeichen: ivec2, ivec3, ivec4</li>
</ul></td>
<td align="left"><p>Vektortyp</p>
<ul>
<li>float2, float3, float4 und float1</li>
<li>bool2, bool3, bool4 und bool1</li>
<li>int2, int3, int4 und int1</li>
<li><p>Die folgenden Typen haben zudem Vektorerweiterungen, die "float", "bool" und "int" ähneln:</p>
<ul>
<li>uint</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>Weitere Informationen finden Sie unter [Vektortyp](https://msdn.microsoft.com/library/windows/desktop/bb509707) und [Schlüsselwörter](https://msdn.microsoft.com/library/windows/desktop/bb509568)</p>
<p>Der Vektor verfügt auch über eine Typdefinition "float4" (typedef vector &lt;float, 4&gt; vector;). Weitere Informationen finden Sie unter [Benutzerdefinierter Typ](https://msdn.microsoft.com/library/windows/desktop/bb509702).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Matrixtyp</p>
<ul>
<li>mat2: 2x2-Float-Matrix</li>
<li>mat3: 3x3-Float-Matrix</li>
<li>mat4: 4x4-Float-Matrix</li>
</ul></td>
<td align="left"><p>Matrixtyp</p>
<ul>
<li>float2x2</li>
<li>float3x3</li>
<li>float4x4</li>
<li>Auch: float1x1, float1x2, float1x3, float1x4, float2x1, float2x3, float2x4, float3x1, float3x2, float3x4, float4x1, float4x2, float4x3</li>
<li><p>Die folgenden Typen haben zudem Matrixerweiterungen, die "float" ähneln:</p>
<ul>
<li>int, uint, bool</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>Sie können auch den [Matrixtyp](https://msdn.microsoft.com/library/windows/desktop/bb509623) verwenden, um eine Matrix zu definieren.</p>
<p>Beispiel: matrix &lt;float, 2, 2&gt; fMatrix = {0.0f, 0.1, 2.1f, 2.2f};</p>
<p>Die Matrix verfügt auch über eine Typdefinition "float4x4" (typedef matrix &lt;float, 4, 4&gt; matrix;). Weitere Informationen finden Sie unter [Benutzerdefinierter Typ](https://msdn.microsoft.com/library/windows/desktop/bb509702).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Genauigkeitsqualifizierer für "float", "int" und "sampler"</p>
<ul>
<li><p>highp</p>
<p>Dieser Qualifizierer dient für minimale Genauigkeitsanforderungen, die größer sind als die Anforderungen von "min16float" und kleiner als ein ganzer 32-Bit-Float-Wert. Äquivalent in HLSL:</p>
<p>highp float -&gt; float</p>
<p>highp int -&gt; int</p></li>
<li><p>mediump</p>
<p>Dieser auf float und int angewendete Qualifizierer entspricht min16float und min12int in HLSL. Mindestens 10 Bits Mantisse (nicht wie "min10float").</p></li>
<li><p>lowp</p>
<p>Dieser auf "float" angewendete Qualifizierer bietet einen Gleitkommabereich von -2 bis 2. Entspricht „min10float“ in HLSL.</p></li>
</ul></td>
<td align="left"><p>Genauigkeitstypen</p>
<ul>
<li>min16float: min. 16-Bit-Gleitkommawert</li>
<li><p>min10float</p>
<p>Min. 2.8-Bit-Festpunktwert mit Vorzeichen (2 Bits ganze Zahl und 8 Bits Nachkommakomponente). Die 8-Bit-Nachkommakomponente kann inklusive 1 sein (anstelle von exklusive), um den kompletten Bereich von -2 bis 2 zu verwenden.</p></li>
<li>min16int: min. 16-Bit-Ganzzahl mit Vorzeichen</li>
<li><p>min12int: min. 12-Bit-Ganzzahl mit Vorzeichen</p>
<p>Dieser Typ dient für "10Level9" ([9_x-Funktionsebenen](https://msdn.microsoft.com/library/windows/desktop/ff476876)). Ganze Zahlen werden dort durch Gleitkommazahlen dargestellt. Dies ist die Genauigkeit, die Sie erhalten, wenn Sie eine ganze Zahl mit einer 16-Bit-Gleitkommazahl emulieren.</p></li>
<li>min16uint: min. 16-Bit-Ganzzahl ohne Vorzeichen</li>
</ul>
<p>Weitere Informationen finden Sie unter [Skalare Typen](https://msdn.microsoft.com/library/windows/desktop/bb509646) und [Verwenden der HLSL-Mindestgenauigkeit](https://msdn.microsoft.com/library/windows/desktop/hh968108).</p></td>
</tr>
<tr class="odd">
<td align="left">sampler2D</td>
<td align="left">[Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525)</td>
</tr>
<tr class="even">
<td align="left">samplerCube</td>
<td align="left">[TextureCube](https://msdn.microsoft.com/library/windows/desktop/bb509700)</td>
</tr>
</tbody>
</table>

 

## Portieren von vordefinierten globalen GLSL-Variablen zu HLSL


Ziehen Sie beim Portieren von vordefinierten globalen GLSL-Variablen zu HLSL die folgende Tabelle zurate.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Vordefinierte globale GLSL-Variable</th>
<th align="left">HLSL-Semantik</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>gl_Position</strong></p>
<p>Diese Variable ist vom Typ <strong>vec4</strong>.</p>
<p>Scheitelpunktposition</p>
<p>Beispiel: - gl_Position = position;</p></td>
<td align="left"><p>SV_Position</p>
<p>POSITION in Direct3D 9</p>
<p>Diese Semantik ist vom Typ <strong>float4</strong>.</p>
<p>Ausgabe des Vertex-Shaders</p>
<p>Scheitelpunktposition</p>
<p>Beispiel: - float4 vPosition : SV_Position;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_PointSize</strong></p>
<p>Diese Variable ist vom Typ <strong>float</strong>.</p>
<p>Punktgröße</p></td>
<td align="left"><p>PSIZE</p>
<p>Ohne Bedeutung, sofern nicht Direct3D 9 das Ziel ist.</p>
<p>Diese Semantik ist vom Typ <strong>float</strong>.</p>
<p>Ausgabe des Vertex-Shaders</p>
<p>Punktgröße</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragColor</strong></p>
<p>Diese Variable ist vom Typ <strong>vec4</strong>.</p>
<p>Fragmentfarbe</p>
<p>Beispiel: - gl_FragColor = vec4(colorVarying, 1.0);</p></td>
<td align="left"><p>SV_Target</p>
<p>COLOR in Direct3D 9</p>
<p>Diese Semantik ist vom Typ <strong>float4</strong>.</p>
<p>Ausgabe des Pixelshaders</p>
<p>Pixelfarbe</p>
<p>Beispiel: - float4 Color[4] : SV_Target;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragData[n]</strong></p>
<p>Diese Variable ist vom Typ <strong>vec4</strong>.</p>
<p>Fragmentfarbe für die Farbzuordnung „n“</p></td>
<td align="left"><p>SV_Target[n]</p>
<p>Diese Semantik ist vom Typ <strong>float4</strong>.</p>
<p>Ausgabewert des Pixelshaders, der im Renderziel "n" gespeichert wird, wobei 0 &lt;= n &lt;= 7.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragCoord</strong></p>
<p>Diese Variable ist vom Typ <strong>vec4</strong>.</p>
<p>Fragmentposition im Framepuffer</p></td>
<td align="left"><p>SV_Position</p>
<p>Nicht verfügbar in Direct3D 9</p>
<p>Diese Semantik ist vom Typ <strong>float4</strong>.</p>
<p>Eingabe des Pixelshaders</p>
<p>Koordinaten des Bildschirmbereichs</p>
<p>Beispiel: - float4 screenSpace : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FrontFacing</strong></p>
<p>Diese Variable ist vom Typ <strong>bool</strong>.</p>
<p>Bestimmt, ob ein Fragment zu einem Vorderseitengrundtyp gehört.</p></td>
<td align="left"><p>SV_IsFrontFace</p>
<p>VFACE in Direct3D 9</p>
<p>„SV_IsFrontFace“ ist vom Typ <strong>bool</strong>.</p>
<p>VFACE ist vom Typ <strong>float</strong>.</p>
<p>Eingabe des Pixelshaders</p>
<p>Grundtypseite</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_PointCoord</strong></p>
<p>Diese Variable ist vom Typ <strong>vec2</strong>.</p>
<p>Fragmentposition innerhalb eines Punkts (nur Punktrasterung)</p></td>
<td align="left"><p>SV_Position</p>
<p>VPOS in Direct3D 9</p>
<p>„SV_Position“ ist vom Typ <strong>float4</strong>.</p>
<p>VPOS ist vom Typ <strong>float2</strong>.</p>
<p>Eingabe des Pixelshaders</p>
<p>Pixel- oder Bildpunktposition im Bildschirmbereich</p>
<p>Beispiel: - float4 pos : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragDepth</strong></p>
<p>Diese Variable ist vom Typ <strong>float</strong>.</p>
<p>Tiefenpufferdaten</p></td>
<td align="left"><p>SV_Depth</p>
<p>DEPTH in Direct3D 9</p>
<p>„SV_Depth“ ist vom Typ <strong>float</strong>.</p>
<p>Ausgabe des Pixelshaders</p>
<p>Tiefenpufferdaten</p></td>
</tr>
</tbody>
</table>

 

Geben Sie die Position, Farbe usw. für die Eingabe des Vertex-Shaders und die Eingabe des Pixelshaders mit Semantikwerten an. Die Semantikwerte im Eingabelayout müssen der Eingabe des Vertex-Shaders entsprechen. Beispiele finden Sie unter [Beispiele für das Portieren von GLSL-Variablen zu HLSL](#example1). Weitere Informationen zur HLSL-Semantik finden Sie unter [Semantik](https://msdn.microsoft.com/library/windows/desktop/bb509647).

## Beispiele für das Portieren von GLSL-Variablen zu HLSL


Im Folgenden zeigen wir Ihnen Beispiele für die Verwendung von GLSL-Variablen in OpenGL/GLSL-Code und anschließend das entsprechende Beispiel in Direct3D/HLSL-Code.

### Uniform-Variable, Attribut und variierende Variable in GLSL

OpenGL-App-Code

``` syntax
// Uniform values can be set in app code and then processed in the shader code.
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

// Incoming position of vertex
attribute vec4 position;
 
// Incoming color for the vertex
attribute vec3 color;
 
// The varying variable tells the shader pipeline to pass it  
// on to the fragment shader.
varying vec3 colorVarying;
```

GLSL-Vertex-Shader-Code

``` syntax
//The shader entry point is the main method.
void main()
{
colorVarying = color; //Use the varying variable to pass the color to the fragment shader
gl_Position = position; //Copy the position to the gl_Position pre-defined global variable
}
```

GLSL-Fragment-Shader-Code

``` syntax
void main()
{
//Pad the colorVarying vec3 with a 1.0 for alpha to create a vec4 color
//and assign that color to the gl_FragColor pre-defined global variable
//This color then becomes the fragment's color.
gl_FragColor = vec4(colorVarying, 1.0);
}
```

### Konstantenpuffer und Datenübertragungen in HLSL

Das folgende Beispiel zeigt, wie Sie Daten an den HLSL-Vertex-Shader übergeben, die anschließend zum Pixelshader übertragen werden. Definieren Sie in Ihrem App-Code einen Scheitelpunkt und einen Konstantenpuffer. Definieren Sie anschließend im Vertex-Shader-Code den Konstantenpuffer als [cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581), und speichern Sie die Daten für einzelne Scheitelpunkte und die Eingabedaten des Pixelshaders. Hier verwenden als **VertexShaderInput** und **PixelShaderInput** bezeichnete Strukturen.

Direct3D-App-Code

```cpp
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;
};
struct SimpleCubeVertex
{
    XMFLOAT3 pos;   // position
    XMFLOAT3 color; // color
};

 // Create an input layout that matches the layout defined in the vertex shader code.
 const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
 {
     { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
     { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
 };

// Create vertex and index buffers that define a geometry.
```

HLSL-Vertex-Shader-Code

``` syntax
cbuffer ModelViewProjectionCB : register( b0 )
{
    matrix model; 
    matrix view;
    matrix projection;
};
// The POSITION and COLOR semantics must match the semantics in the input layout Direct3D app code.
struct VertexShaderInput
{
    float3 pos : POSITION; // Incoming position of vertex 
    float3 color : COLOR; // Incoming color for the vertex
};

struct PixelShaderInput
{
    float4 pos : SV_Position; // Copy the vertex position.
    float4 color : COLOR; // Pass the color to the pixel shader.
};

PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // shader source code

    return vertexShaderOutput;
}
```

HLSL-Pixelshader-Code

``` syntax
// Collect input from the vertex shader. 
// The COLOR semantic must match the semantic in the vertex shader code.
struct PixelShaderInput
{
    float4 pos : SV_Position;
    float4 color : COLOR; // Color for the pixel
};

// Set the pixel color value for the renter target. 
float4 main(PixelShaderInput input) : SV_Target
{
    return input.color;
}
```

## Beispiele für das Portieren von OpenGL-Renderingcode zu Direct3D


Im Folgenden zeigen wir Ihnen ein Beispiel für das Rendering in OpenGL ES 2.0-Code und anschließend das entsprechende Beispiel in Direct3D 11-Code.

OpenGL-Renderingcode

``` syntax
// Bind shaders to the pipeline. 
// Both vertex shader and fragment shader are in a program.
glUseProgram(m_shader->getProgram());
 
// Input asssembly 
// Get the position and color attributes of the vertex.

m_positionLocation = glGetAttribLocation(m_shader->getProgram(), "position");
glEnableVertexAttribArray(m_positionLocation);

m_colorLocation = glGetAttribColor(m_shader->getProgram(), "color");
glEnableVertexAttribArray(m_colorLocation);
 
// Bind the vertex buffer object to the input assembler.
glBindBuffer(GL_ARRAY_BUFFER, m_geometryBuffer);
glVertexAttribPointer(m_positionLocation, 4, GL_FLOAT, GL_FALSE, 0, NULL);
glBindBuffer(GL_ARRAY_BUFFER, m_colorBuffer);
glVertexAttribPointer(m_colorLocation, 3, GL_FLOAT, GL_FALSE, 0, NULL);
 
// Draw a triangle with 3 vertices.
glDrawArray(GL_TRIANGLES, 0, 3);
```

Direct3D-Renderingcode

```cpp
// Bind the vertex shader and pixel shader to the pipeline.
m_d3dDeviceContext->VSSetShader(vertexShader.Get(),nullptr,0);
m_d3dDeviceContext->PSSetShader(pixelShader.Get(),nullptr,0);
 
// Declare the inputs that the shaders expect.
m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());
m_d3dDeviceContext->IASetVertexBuffers(0, 1, vertexBuffer.GetAddressOf(), &stride, &offset);

// Set the primitive's topology.
m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

// Draw a triangle with 3 vertices. triangleVertices is an array of 3 vertices.
m_d3dDeviceContext->Draw(ARRAYSIZE(triangleVertices),0);
```

## Verwandte Themen


* [Portieren von OpenGL ES 2.0 zu Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)

 

 







<!--HONumber=Jun16_HO4-->


