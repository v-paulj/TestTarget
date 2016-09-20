---
author: mtoepke
title: Vergleich der OpenGLES2.0-Shaderpipeline mit Direct3D
description: "Vom Konzept her ist die Direct3D11-Shaderpipeline der in OpenGLES2.0 sehr ähnlich."
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: bc13df5e7f2648897be31b5cda634d23ffae8b6b

---

# Vergleich der OpenGLES2.0-Shaderpipeline mit Direct3D


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [Eingabe-Assembler-Stufe](https://msdn.microsoft.com/library/windows/desktop/bb205116)
-   [Vertex-Shader-Stufe](https://msdn.microsoft.com/library/windows/desktop/bb205146#Vertex_Shader_Stage)
-   [Pixelshader-Stufe](https://msdn.microsoft.com/library/windows/desktop/bb205146#Pixel_Shader_Stage)

Vom Konzept her ist die Direct3D11-Shaderpipeline der in OpenGLES2.0 sehr ähnlich. Hinsichtlich des API-Entwurfs sind die Hauptkomponenten für die Erstellung und Verwaltung der Shaderstufen jedoch Teile der zwei primären Schnittstellen [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) und [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). In diesem Thema versuchen wir, allgemeine Muster der OpenGLES2.0-Shaderpipeline-API den Direct3D11-Entsprechungen in diesen Schnittstellen zuzuordnen.

## Die Direct3D11-Shaderpipeline


Die Shaderobjekte werden mit Methoden der [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)-Schnittstelle wie [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) und [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) erstellt.

Die Direct3D11-Grafikpipeline wird von Instanzen der [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Schnittstelle verwaltet und umfasst die folgenden Stufen:

-   
            [Eingabe-Assembler-Stufe](https://msdn.microsoft.com/library/windows/desktop/bb205116). Die Eingabe-Assembler-Stufe stellt Daten (Dreiecke, Linien und Punkte) für die Pipeline bereit. 
            [
              **ID3D11DeviceContext1**
            ](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Methoden, die diese Stufe unterstützen, haben das Präfix „IA“.
-   
            [Vertex-Shader-Stufe](https://msdn.microsoft.com/library/windows/desktop/bb205146#Vertex_Shader_Stage). Die Vertex-Shader-Stufe verarbeitet Scheitelpunkte und führt dabei in der Regel Vorgänge wie Transformationen, das Anwenden von Skins und Beleuchtung aus. Ein Vertex-Shader verarbeitet immer einen einzigen Eingabescheitelpunkt und erzeugt daraus einen einzigen Ausgabescheitelpunkt. 
            [
              **ID3D11DeviceContext1**
            ](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Methoden, die diese Stufe unterstützen, haben das Präfix „VS“.
-   
            [Datenstrom-Ausgabe-Stufe](https://msdn.microsoft.com/library/windows/desktop/bb205121). Die Datenstrom-Ausgabe-Stufe streamt Grundtypdaten auf dem Weg zum Rasterizer aus der Pipeline in den Arbeitsspeicher. Daten können "ausgestreamt" und/oder in den Rasterizer übergeben werden. In den Arbeitsspeicher gestreamte Daten können als Eingabedaten wieder der Pipeline zugeführt oder von der CPU eingelesen werden. 
            [
              **ID3D11DeviceContext1**
            ](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Methoden, die diese Stufe unterstützen, haben das Präfix „SO“.
-   
            [Rasterizer-Stufe](https://msdn.microsoft.com/library/windows/desktop/bb205125). Der Rasterizer beschneidet Grundtypen, bereitet Grundtypen für den Pixelshader vor und bestimmt, wie Pixelshader aufgerufen werden. Sie können die Rasterung deaktivieren, indem Sie die Pipeline anweisen, dass kein Pixelshader vorhanden ist (setzen Sie die Pixelshader-Stufe mit [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) auf NULL), und die Tiefen- und Schablonentests deaktivieren (setzen Sie „DepthEnable“ und „StencilEnable“ in [**D3D11\_DEPTH\_STENCIL\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476110) auf FALSE). Wenn die Rasterung deaktiviert ist, werden damit zusammenhängende Pipelinezähler nicht aktualisiert.
-   
            [Pixelshader-Stufe](https://msdn.microsoft.com/library/windows/desktop/bb205146#Pixel_Shader_Stage). Die Pixelshader-Stufe empfängt interpolierte Daten für einen Grundtyp und generiert Pro-Pixel-Daten (z.B. die Farbe). 
            [
              **ID3D11DeviceContext1**
            ](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Methoden, die diese Stufe unterstützen, haben das Präfix „PS“.
-   
            [Ausgabezusammenführungs-Stufe](https://msdn.microsoft.com/library/windows/desktop/bb205120). Die Ausgabezusammenführungs-Stufe kombiniert verschiedene Ausgabedaten (Pixelshaderwerte, Tiefen- und Schabloneninformationen) mit dem Inhalt des Renderziels und Tiefen-/Schablonenpuffern, um das endgültige Pipelineergebnis zu generieren. 
            [
              **ID3D11DeviceContext1**
            ](https://msdn.microsoft.com/library/windows/desktop/hh404598)-Methoden, die diese Stufe unterstützen, haben das Präfix „OM“.

(Es gibt auch Stufen für Geometrie-Shader, Hüllen-Shader, Tesselator und Domain-Shader, aber da es dafür in OpenGL ES 2.0 keine Äquivalente gibt, gehen wir hier nicht weiter darauf ein.) Eine vollständige Liste der Methoden für diese Stufen finden Sie auf den Referenzseiten [ **ID3D11DeviceContext** ](https://msdn.microsoft.com/library/windows/desktop/ff476385) und [ **ID3D11DeviceContext1** ](https://msdn.microsoft.com/library/windows/desktop/hh404598). 
            **ID3D11DeviceContext1** erweitert **ID3D11DeviceContext** für Direct3D11.

## Erstellen eines Shaders


In Direct3D werden Shaderressourcen nicht vor dem Kompilieren und Laden erstellt. Stattdessen wird die Ressource beim Laden der HLSL-Datei erstellt. Es gibt daher keine direkt analoge Funktion für „glCreateShader“, die eine initialisierte Shaderressource eines bestimmten Typs erstellt (z.B. GL\_VERTEX\_SHADER oder GL\_FRAGMENT\_SHADER). Shader werden stattdessen nach dem Laden der HLSL-Datei mit spezifischen Funktionen wie [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) und [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) erstellt, für die der Typ und die kompilierte HLSL-Datei als Parameter verwendet werden.

| OpenGL ES2.0  | Direct3D11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | Nachdem das kompilierte Shaderobjekt (Compiled Shader Object, CSO) geladen wurde, rufen Sie [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) und [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) auf. Übergeben Sie die CSO-Datei dabei als Puffer. |

 

## Kompilieren eines Shaders


Direct3D-Shader müssen in UWP-Apps (Universelle Windows-Plattform) als CSO-Dateien vorkompiliert und mit einer der Windows-Runtime-Datei-APIs geladen werden. (Desktop-Apps können die Shader zur Laufzeit aus Textdateien oder Zeichenfolgen kompilieren.) Die CSO-Dateien werden aus beliebigen HLSL-Dateien des Microsoft Visual Studio-Projekts erstellt und behalten ihre Namen (jedoch mit der Dateierweiterung „.cso“). Stellen Sie sicher, dass Sie in Ihrem gelieferten Paket enthalten sind!

| OpenGL ES2.0                          | Direct3D11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | Nicht verfügbar Kompilieren Sie die Shader in Visual Studio in CSO-Dateien, und fügen Sie sie Ihrem Paket hinzu.                                                                                     |
| Verwenden von "glGetShaderiv" für den Kompilierungsstatus | Nicht verfügbar Überprüfen Sie, ob die Kompilierungsausgabe des FX-Compilers (FXC) von Visual Studio Fehler enthält. Bei erfolgreicher Kompilierung wird eine entsprechende CSO-Datei erstellt. |

 

## Laden eines Shaders


Wie im Abschnitt zum Erstellen eines Shaders erwähnt, erstellt Direct3D11 den Shader, wenn die entsprechende CSO-Datei in einen Puffer geladen und an eine der Methoden in der folgenden Tabelle übergeben wird.

| OpenGL ES2.0 | Direct3D11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | Nachdem das kompilierte Shaderobjekt geladen wurde, rufen Sie [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) und [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) auf. |

 

## Einrichten der Pipeline


In OpenGLES2.0 wird das "Shaderprogrammobjekt" verwendet, das mehrere Shader für die Ausführung enthält. Einzelne Shader werden an das Shaderprogrammobjekt angefügt. In Direct3D11 arbeiten Sie jedoch direkt mit dem Renderkontext ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)) und erstellen Shader für den Kontext.

| OpenGL ES2.0   | Direct3D11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | Nicht verfügbar In Direct3D11 wird die Shaderprogrammobjekt-Abstraktion nicht verwendet.                          |
| glLinkProgram   | Nicht verfügbar In Direct3D11 wird die Shaderprogrammobjekt-Abstraktion nicht verwendet.                          |
| glUseProgram    | Nicht verfügbar In Direct3D11 wird die Shaderprogrammobjekt-Abstraktion nicht verwendet.                          |
| glGetProgramiv  | Verwenden Sie den erstellten Verweis auf [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). |

 

Erstellen Sie mit der statischen [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082)-Methode eine Instanz von [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) und [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/dn280493).

``` syntax
Microsoft::WRL::ComPtr<ID3D11Device1>          m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>  m_d3dContext;

// ...

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for Windows Store apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## Festlegen der Viewports


Das Festlegen eines Viewports funktioniert in Direct3D11 und OpenGLES2.0 ähnlich. In Direct3D11 rufen Sie [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) mit einer konfigurierten [**CD3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/jj151722)-Klasse auf.

Direct3D11: Festlegen eines Viewports.

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES2.0 | Direct3D11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | 
            [
              **CD3D11\_VIEWPORT**
            ](https://msdn.microsoft.com/library/windows/desktop/jj151722), [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) |

 

## Konfigurieren der Vertex-Shader


Die Konfiguration eines Vertex-Shaders erfolgt in Direct3D11 beim Laden des Shaders. Uniform-Variablen werden als Konstantenpuffer mit [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh446795) übergeben.

| OpenGL ES2.0                    | Direct3D11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476489)                       |
| glGetUniformfv, glGetUniformiv   | 
            [
              **ID3D11DeviceContext1::VSGetConstantBuffers1**
            ](https://msdn.microsoft.com/library/windows/desktop/hh446793). |

 

## Konfigurieren der Pixelshader


Die Konfiguration eines Pixelshaders erfolgt in Direct3D11 beim Laden des Shaders. Uniform-Variablen werden als Konstantenpuffer mit [**ID3D11DeviceContext1::PSSetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh404649) übergeben.

| OpenGL ES2.0                    | Direct3D11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::PSGetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476468)                       |
| glGetUniformfv, glGetUniformiv   | 
            [
              **ID3D11DeviceContext1::PSGetConstantBuffers1**
            ](https://msdn.microsoft.com/library/windows/desktop/hh404645). |

 

## Generieren der endgültigen Ergebnisse


Wenn die Pipeline abgeschlossen ist, zeichnen Sie die Ergebnisse der Shaderstufen in den Hintergrundpuffer. In Direct3D11 wird dazu wie in OpenGLES2.0 ein Zeichnen-Befehl aufgerufen, um die Ergebnisse als Farbzuordnung in den Hintergrundpuffer auszugeben. Dieser Hintergrundpuffer wird anschließend an die Anzeige gesendet.

| OpenGL ES2.0  | Direct3D11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | 
            [
              **ID3D11DeviceContext1::Draw**
            ](https://msdn.microsoft.com/library/windows/desktop/ff476407), [**ID3D11DeviceContext1::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) (oder andere Draw\*-Methoden für [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/ff476385)). |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)                                                                                                                                                                              |

 

## Portieren von GLSL zu HLSL


GLSL und HLSL unterscheiden sich abgesehen von der Unterstützung komplexer Typen und einiger allgemeiner Syntaxteile nur wenig. Für viele Entwickler besteht die einfachste Methode für die Portierung zwischen den beiden Programmiersprachen darin, Aliase für allgemeine OpenGLES2.0-Anweisungen und -Definitionen zu erstellen und sie ihrem HLSL-Äquivalent zuzuordnen. Beachten Sie, dass Direct3D die Shadermodellversion verwendet, um die von einer Grafikschnittstelle unterstützte HLSL-Funktionsgruppe darzustellen. In OpenGL wird eine andere Versionsspezifikation für HLSL verwendet. Die folgende Tabelle soll Ihnen– in Bezug auf die Version– eine ungefähre Vorstellung von den für Direct3D11 und OpenGLES2.0 definierten Funktionsgruppen der Shaderprogrammiersprachen geben.

| Shaderprogrammiersprache           | GLSL-Funktionsversion                                                                                                                                                                                                      | Direct3D-Shadermodell |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D11 HLSL          | ~4.30                                                                                                                                                                                                                    | SM5.0                |
| GLSLES für OpenGLES2.0 | 1.40. Ältere Implementierungen von GLSLES für OpenGLES2.0 verwenden möglicherweise die Versionen1.10 bis1.30. Überprüfen Sie den ursprünglichen Code mit „glGetString(GL\_SHADING\_LANGUAGE\_VERSION)“ oder „glGetString(SHADING\_LANGUAGE\_VERSION“, um festzustellen, ob dies der Fall ist. | ~SM2.0               |

 

Weitere Informationen zu den Unterschieden zwischen den beiden Shaderprogrammiersprachen und allgemeine Syntaxzuordnungen finden Sie in der [GLSL-zu-HLSL-Referenz](glsl-to-hlsl-reference.md).

## Portieren der systeminternen OpenGL-Funktionen zu HLSL-Semantik


Bei der Direct3D11-HLSL-Semantik handelt es sich um Zeichenfolgen, die wie eine Uniform-Variable oder ein Attributname zum Identifizieren eines zwischen der App und einem Shaderprogramm übergebenen Werts verwendet werden. Obwohl viele verschiedene Zeichenfolgen möglich sind, empfiehlt es sich, eine Zeichenfolge wie POSITION oder COLOR zu verwenden, aus der der Verwendungszweck hervorgeht. Sie weisen diese Semantik zu, wenn Sie einen Konstantenpuffer oder ein Puffereingabelayout erstellen. Sie können auch eine Zahl zwischen 0und7 an die Semantik anfügen, wenn Sie separate Register für ähnliche Werte verwenden möchten. Beispiel: COLOR0, COLOR1, COLOR2...

Bei einer Semantik mit dem Präfix "SVß_" handelt es sich um eine Systemwertsemantik, in die das Shaderprogramm schreibt und die nicht von der App selbst (ausgeführt in der CPU) geändert werden kann. In der Regel enthält diese Semantik Werte, die Ein- oder Ausgaben einer anderen Shaderstufe in der Grafikpipeline sind oder komplett von der GPU generiert werden.

Zudem weist die SV\_-Semantik ein anderes Verhalten auf, wenn damit die Eingabe für eine Shaderstufe oder die Ausgabe einer Shaderstufe angegeben wird. SV\_POSITION (Ausgabe) enthält z.B. die während der Vertex-Shader-Stufe transformierten Vertexdaten. SV\_POSITION (Eingabe) enthält die während der Rasterung interpolierten Pixelpositionswerte.

Die folgende Tabelle enthält einige Zuordnungen für allgemeine systeminterne OpenGLES2.0-Shaderfunktionen:

| OpenGL-Systemwert | Verwenden Sie diese HLSL-Semantik:                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gl\_Position        | POSITION(n) für Scheitelpunkt-Pufferdaten. SV\_POSITION stellt eine Pixelposition für den Pixelshader bereit und kann nicht von der App geschrieben werden.                                        |
| gl\_Normal          | NORMAL(n) für vom Scheitelpunktpuffer bereitgestellte Normalendaten.                                                                                                                 |
| gl\_TexCoord\[n\]   | TEXCOORD(n) für Textur-UV-Koordinatendaten (ST in manchen OpenGL-Dokumentationen), die an einen Shader übergeben werden.                                                                       |
| gl\_FragColor       | COLOR(n) für RGBA-Farbdaten, die an einen Shader übergeben werden. Diese Daten werden wie Koordinatendaten behandelt. Durch die Semantik können Sie nur leichter erkennen, dass es sich um Farbdaten handelt. |
| gl\_FragData\[n\]   | SV\_Target\[n\] zum Schreiben aus einem Pixelshader in eine Zieltextur oder einen anderen Pixelpuffer.                                                                               |

 

Die Methode zum Codieren der Semantik unterscheidet sich von der Verwendung systeminterner Funktionen in OpenGLES2.0. In OpenGL können Sie auf viele der systeminternen Funktionen direkt ohne Konfiguration oder Deklaration zugreifen. In Direct3D müssen Sie ein Feld in einem bestimmten Konstantenpuffer deklarieren, um eine bestimmte Semantik verwenden zu können, oder es als Rückgabewert für die **main()**-Methode eines Shaders deklarieren.

Das folgende Beispiel zeigt die Verwendung einer Semantik in einer Konstantenpufferdefinition:

```cpp
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR0;
};

// The position is interpolated to the pixel value by the system. The per-vertex color data is also interpolated and passed through the pixel shader. 
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Der Code definiert ein Paar einfacher Konstantenpuffer.

Das folgende Beispiel zeigt die Verwendung einer Semantik zum Definieren des Rückgabewerts eines Fragment-Shaders:

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

In diesem Fall ist SV\_TARGET die Position des Renderziels, in das die Pixelfarbe (definiert als Vektor mit vier Floatwerten) bei Abschluss der Shaderausführung geschrieben wird.

Ausführliche Informationen zur Verwendung der Semantik mit Direct3D finden Sie unter [HLSL-Semantik](https://msdn.microsoft.com/library/windows/desktop/bb509647).

 

 







<!--HONumber=Jun16_HO4-->


