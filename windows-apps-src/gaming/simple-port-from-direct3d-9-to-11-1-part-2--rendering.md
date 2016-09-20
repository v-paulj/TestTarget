---
author: mtoepke
title: Konvertieren des Renderingframeworks
description: Hier wird veranschaulicht, wie Sie ein einfaches Renderingframework von Direct3D 9 in Direct3D 11 konvertieren. Sie erfahren, wie Sie Geometriepuffer portieren, HLSL-Shaderprogramme kompilieren und laden und die Renderkette in Direct3D 11 implementieren.
ms.assetid: f6ca1147-9bb8-719a-9a2c-b7ee3e34bd18
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5cfdce2a62f6b5761ebf820418762a307dd051bb

---

# Konvertieren des Renderingframeworks


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Zusammenfassung**

-   [Teil 1: Initialisieren von Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   Teil 2: Konvertieren des Renderingframeworks
-   [Teil 3: Portieren der Spielschleife](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Hier wird veranschaulicht, wie Sie ein einfaches Renderingframework von Direct3D9 in Direct3D11 konvertieren. Sie erfahren, wie Sie Geometriepuffer portieren, HLSL-Shaderprogramme kompilieren und laden und die Renderkette in Direct3D11 implementieren. Teil2 der exemplarischen Vorgehensweise [Portieren einer einfachen Direct3D9-App zu DirectX11 und UWP (Universelle Windows-Plattform)](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## Konvertieren der Effekte in HLSL-Shader


Das folgende Beispiel ist ein einfaches D3DX-Verfahren, das für die ältere Effekt-API, die Hardware-Vertextransformation und Pass-Through-Farbdaten geschrieben wurde.

Direct3D9-Shadercode

```cpp
// Global variables
matrix g_mWorld;        // world matrix for object
matrix g_View;          // view matrix
matrix g_Projection;    // projection matrix

// Shader pipeline structures
struct VS_OUTPUT
{
    float4 Position   : POSITION;   // vertex position
    float4 Color      : COLOR0;     // vertex diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : COLOR0;  // Pixel color    
};

// Vertex shader
VS_OUTPUT RenderSceneVS(float3 vPos : POSITION, 
                        float3 vColor : COLOR0)
{
    VS_OUTPUT Output;
    
    float4 pos = float4(vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, g_mWorld);
    pos = mul(pos, g_View);
    pos = mul(pos, g_Projection);

    Output.Position = pos;
    
    // Just pass through the color data
    Output.Color = float4(vColor, 1.0f);
    
    return Output;
}

// Pixel shader
PS_OUTPUT RenderScenePS(VS_OUTPUT In) 
{ 
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}

// Technique
technique RenderSceneSimple
{
    pass P0
    {          
        VertexShader = compile vs_2_0 RenderSceneVS();
        PixelShader  = compile ps_2_0 RenderScenePS(); 
    }
}
```

In Direct3D11 können Sie weiterhin HLSL-Shader verwenden. Jeder Shader wird in eine eigene HLSL-Datei eingefügt, damit diese von Visual Studio in separate Dateien kompiliert werden. Später werden sie dann als separate Direct3D-Ressourcen geladen. Wir legen die Zielebene auf [Shadermodell4 Ebene9\_1 (/4\_0\_level\_9\_1)](https://msdn.microsoft.com/library/windows/desktop/ff476876) fest, weil diese Shader für DirectX9.1-GPUs geschrieben wurden.

Beim Definieren des Eingabelayouts haben wir sichergestellt, dass die gleiche Datenstruktur abgebildet wurde, die zum Speichern der Daten pro Vertex im Systemspeicher und im GPU-Speicher verwendet wurde. Ebenso sollte die Ausgabe eines Vertex-Shaders mit der Struktur übereinstimmen, die als Eingabe für den Pixelshader verwendet wird. Die Regeln entsprechen dabei nicht den Regeln zum Übergeben von Daten aus einer Funktion in eine andere unter C++. Sie können nicht verwendete Variablen am Ende der Struktur weglassen. Die Reihenfolge kann jedoch nicht erneuert werden, und Sie können Inhalte in der Mitte der Datenstruktur nicht überspringen.

> **Hinweis**  
Die Regeln in Direct3D 9 zum Binden von Vertex-Shadern an Pixelshader waren nicht so strikt wie die Regeln in Direct3D 11. Die Direct3D9-Anordnung war zwar flexibel, aber ineffizient.

 

Es ist möglich, dass von Ihren HLSL-Dateien ältere Syntax für die Shadersemantik verwendet wird, beispielsweise COLOR anstelle von SV\_TARGET. In diesem Fall müssen Sie den HLSL-Kompatibilitätsmodus (Compileroption "/Gec") aktivieren oder die Shader[semantik](https://msdn.microsoft.com/library/windows/desktop/bb509647) auf die aktuelle Syntax aktualisieren. Der Vertex-Shader in diesem Beispiel wurde mit der aktuellen Syntax aktualisiert.

Unten ist der Vertex-Shader für die Hardwaretransformation in einer eigenen Datei definiert.

> 
            **Hinweis**  Vertexshader sind erforderlich, um die SV\_POSITION-Systemwertsemantik auszugeben. Mit dieser Semantik werden die Vertexpositionsdaten zu Koordinatenwerten aufgelöst, wobei "x" zwischen-1 und1 und "y" zwischen-1 und1 liegt. "z" wird durch den ursprünglichen homogenen Koordinatenwert"w" dividiert (z/w), und "w" ist1 dividiert durch den Originalwert"w" (1/w).

 

HLSL-Vertex-Shader (Featureebene9.1)

```cpp
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
    matrix mWorld;       // world matrix for object
    matrix View;        // view matrix
    matrix Projection;  // projection matrix
};

struct VS_INPUT
{
    float3 vPos   : POSITION;
    float3 vColor : COLOR0;
};

struct VS_OUTPUT
{
    float4 Position : SV_POSITION; // Vertex shaders must output SV_POSITION
    float4 Color    : COLOR0;
};

VS_OUTPUT main(VS_INPUT input) // main is the default function name
{
    VS_OUTPUT Output;

    float4 pos = float4(input.vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, mWorld);
    pos = mul(pos, View);
    pos = mul(pos, Projection);
    Output.Position = pos;

    // Just pass through the color data
    Output.Color = float4(input.vColor, 1.0f);

    return Output;
}
```

Dies ist alles, was wir für den Pass-Through-Pixelshader brauchen. Obwohl die Bezeichnung "Pass-Through" verwendet wird, werden für jedes Pixel jeweils für die Perspektive passende interpolierte Farbdaten abgerufen. Beachten Sie, dass die SV\_TARGET-Systemwertsemantik hier vom Pixelshader auf die Farbwertausgabe angewendet wird, wie dies für die API erforderlich ist.

> 
            **Hinweis**  Pixelshader der Shaderebene 9\_x können nicht aus der SV\_POSITION-Systemwertsemantik auslesen. Für Pixelshader des Modells4.0 (oder höher) kann das SV\_POSITION-Element zum Abrufen der Pixelposition auf dem Bildschirm verwendet werden, wobei "x" zwischen0 und der Breite des Renderziels und "y" zwischen0 und der Höhe des Renderziels liegt (jeweils um den Wert0,5 versetzt).

 

Die meisten Pixelshader sind viel komplexer als ein Pass-Through-Element aufgebaut. Beachten Sie, dass höhere Direct3D-Featureebenen auch eine deutlich größere Anzahl an Berechnungen pro Shaderprogramm ermöglichen.

HLSL-Pixelshader (Featureebene9.1)

```cpp
struct PS_INPUT
{
    float4 Position : SV_POSITION;  // interpolated vertex position (system value)
    float4 Color    : COLOR0;       // interpolated diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : SV_TARGET;  // pixel color (your PS computes this system value)
};

PS_OUTPUT main(PS_INPUT In)
{
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}
```

## Kompilieren und Laden von Shadern


In Direct3D 9-Spielen wurde die Effektbibliothek häufig als bequeme Möglichkeit zum Implementieren programmierbarer Pipelines genutzt. Effekte können zur Laufzeit mit der [**D3DXCreateEffectFromFile function**](https://msdn.microsoft.com/library/windows/desktop/bb172768)-Methode kompiliert werden.

Laden eines Effekts in Direct3D 9

```cpp
// Turn off preshader optimization to keep calculations on the GPU
DWORD dwShaderFlags = D3DXSHADER_NO_PRESHADER;

// Only enable debug info when compiling for a debug target
#if defined (DEBUG) || defined (_DEBUG)
dwShaderFlags |= D3DXSHADER_DEBUG;
#endif

D3DXCreateEffectFromFile(
    m_pd3dDevice,
    L"CubeShaders.fx",
    NULL,
    NULL,
    dwShaderFlags,
    NULL,
    &m_pEffect,
    NULL
    );
```

In Direct3D 11 können Shaderprogramme als binäre Ressourcen verwendet werden. Shader werden kompiliert, wenn das Projekt erstellt wird, und dann als Ressourcen behandelt. In diesem Beispiel wird der Shader-Bytecode in den Systemspeicher geladen, die Direct3D-Geräteschnittstelle zum Erstellen einer Direct3D-Ressource für jeden Shader verwendet und jeweils auf die Direct3D-Shaderressourcen verwiesen, wenn ein Frame eingerichtet wird.

Laden einer Shaderressource in Direct3D11

```cpp
// BasicReaderWriter is a tested file loader used in SDK samples.
BasicReaderWriter^ readerWriter = ref new BasicReaderWriter();


// Load vertex shader:
Platform::Array<byte>^ vertexShaderData =
    readerWriter->ReadData("CubeVertexShader.cso");

// This call allocates a device resource, validates the vertex shader 
// with the device feature level, and stores the vertex shader bits in 
// graphics memory.
m_d3dDevice->CreateVertexShader(
    vertexShaderData->Data,
    vertexShaderData->Length,
    nullptr,
    &m_vertexShader
    );
```

Fügen Sie zum Einbinden von Shader-Bytecode in das kompilierte App-Paket dem Visual Studio-Projekt einfach die HLSL-Datei hinzu. In Visual Studio wird das [Effektcompiler-Tool](https://msdn.microsoft.com/library/windows/desktop/bb232919) (FXC) verwendet, um HLSL-Dateien in kompilierte Shaderobjekte (CSO-Dateien) zu kompilieren und in das App-Paket einzubinden.

> 
            **Hinweis**  Stellen Sie sicher, dass Sie die richtige Zielfeatureebene für den HLSL-Compiler festlegen: Klicken Sie in Visual Studio mit der rechten Maustaste auf die HLSL-Quelldatei, wählen Sie „Eigenschaften“, und ändern Sie unter **HLSL Compiler -&gt; General** die Einstellung **Shader Model**. In Direct3D wird diese Eigenschaft anhand der Hardwarefunktionen überprüft, wenn von der App die Direct3D-Shaderressource erstellt wird.

 

![HLSL-Shadereigenschaften](images/hlslshaderpropertiesmenu.png)![HLSL-Shadertyp](images/hlslshadertypeproperties.png)

Dies ist ein guter Ort zum Erstellen des Eingabelayouts, welches der Deklaration des Vertexstreams in Direct3D 9 entspricht. Die Datenstruktur pro Vertex muss mit der Struktur übereinstimmen, die vom Vertex-Shader verwendet wird. In Direct3D11 ist eine bessere Steuerung des Eingabelayouts möglich. Sie können die Arraygröße und Bitlänge von Gleitkommavektoren definieren und die Semantik für den Vertex-Shader angeben. Wir erstellen eine [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180)-Struktur und verwenden sie, um für Direct3D anzugeben, wie die Daten pro Vertex aussehen sollen. Wir haben das Laden des Vertex-Shaders abgewartet, bevor das Eingabelayout definiert wurde. Der Grund ist, dass das Eingabelayout von der API anhand der Vertex-Shaderressource überprüft wird. Wenn das Eingabelayout nicht kompatibel ist, löst Direct3D eine Ausnahme aus.

Daten pro Vertex müssen im Systemspeicher in Form von kompatiblen Typen gespeichert werden. Hierbei können DirectXMath-Datentypen hilfreich sein. DXGI\_FORMAT\_R32G32B32\_FLOAT entspricht beispielsweise [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475).

> 
            **Hinweis**  Von Konstantenpuffern wird ein festes Eingabelayout mit gleichzeitiger Ausrichtung für vier Gleitkommazahlen verwendet. 
            [
              **XMFLOAT4**
            ](https://msdn.microsoft.com/library/windows/desktop/ee419608) (und die Ableitungen) wird für die Daten von Konstantenpuffern empfohlen.

 

Festlegen des Eingabelayouts in Direct3D11

```cpp
// Create input layout:
const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT,
        0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },

    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 
        0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
```

## Erstellen von Geometrieressourcen


In Direct3D9 wurden Geometrieressourcen gespeichert, indem Puffer auf dem Direct3D-Gerät erstellt wurden, der Speicher gesperrt wurde und Daten aus dem CPU-Speicher in den GPU-Speicher kopiert wurden.

Direct3D9

```cpp
// Create vertex buffer:
VOID* pVertices;

// In Direct3D 9 we create the buffer, lock it, and copy the data from 
// system memory to graphics memory.
m_pd3dDevice->CreateVertexBuffer(
    sizeof(CubeVertices),
    0,
    D3DFVF_XYZ | D3DFVF_DIFFUSE,
    D3DPOOL_MANAGED,
    &pVertexBuffer,
    NULL);

pVertexBuffer->Lock(
    0,
    sizeof(CubeVertices),
    &pVertices,
    0);

memcpy(pVertices, CubeVertices, sizeof(CubeVertices));
pVertexBuffer->Unlock();
```

Für DirectX11 wird ein einfacherer Prozess genutzt. Von der API werden die Daten automatisch aus dem Systemspeicher in die GPU kopiert. Wir können intelligente COM-Zeiger verwenden, um die Programmierung zu vereinfachen.

DirectX11

```cpp
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = CubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(
    sizeof(CubeVertices),
    D3D11_BIND_VERTEX_BUFFER);
  
// This call allocates a device resource for the vertex buffer and copies
// in the data.
m_d3dDevice->CreateBuffer(
    &vertexBufferDesc,
    &vertexBufferData,
    &m_vertexBuffer
    );
```

## Implementieren der Renderkette


Für Direct3D9-Spiele wurde häufig eine auf Effekten basierende Renderkette genutzt. Mit dieser Art von Renderkette wird das Effektobjekt eingerichtet, die benötigten Ressourcen werden bereitgestellt, und die einzelnen Durchläufe werden gerendert.

Direct3D9-Renderkette

```cpp
// Clear the render target and the z-buffer.
m_pd3dDevice->Clear(
    0, NULL,
    D3DCLEAR_TARGET | D3DCLEAR_ZBUFFER,
    D3DCOLOR_ARGB(0, 45, 50, 170),
    1.0f, 0
    );

// Set the effect technique
m_pEffect->SetTechnique("RenderSceneSimple");

// Rotate the cube 1 degree per frame.
D3DXMATRIX world;
D3DXMatrixRotationY(&world, D3DXToRadian(m_frameCount++));


// Set the matrices up using traditional functions.
m_pEffect->SetMatrix("g_mWorld", &world);
m_pEffect->SetMatrix("g_View", &m_view);
m_pEffect->SetMatrix("g_Projection", &m_projection);

// Render the scene using the Effects library.
if(SUCCEEDED(m_pd3dDevice->BeginScene()))
{
    // Begin rendering effect passes.
    UINT passes = 0;
    m_pEffect->Begin(&passes, 0);
    
    for (UINT i = 0; i < passes; i++)
    {
        m_pEffect->BeginPass(i);
        
        // Send vertex data to the pipeline.
        m_pd3dDevice->SetFVF(D3DFVF_XYZ | D3DFVF_DIFFUSE);
        m_pd3dDevice->SetStreamSource(
            0, pVertexBuffer,
            0, sizeof(VertexPositionColor)
            );
        m_pd3dDevice->SetIndices(pIndexBuffer);
        
        // Draw the cube.
        m_pd3dDevice->DrawIndexedPrimitive(
            D3DPT_TRIANGLELIST,
            0, 0, 8, 0, 12
            );
        m_pEffect->EndPass();
    }
    m_pEffect->End();
    
    // End drawing.
    m_pd3dDevice->EndScene();
}

// Present frame:
// Show the frame on the primary surface.
m_pd3dDevice->Present(NULL, NULL, NULL, NULL);
```

Mit der DirectX11-Renderkette werden diese Aufgaben auch weiterhin ausgeführt, aber die Renderdurchläufe müssen anders implementiert werden. Anstatt die speziellen Angaben in FX-Dateien einzufügen und die Renderverfahren für den C++-Code mehr oder weniger undurchschaubar zu machen, werden alle Rendervorgänge unter C++ eingerichtet.

Unten ist die Renderkette angegeben. Wir müssen das Eingabelayout angeben, das wir nach dem Laden des Vertex-Shaders erstellt haben, die einzelnen Shaderobjekte bereitstellen und angeben, welche Konstantenpuffer von den einzelnen Shadern verwendet werden sollen. Dieses Beispiel enthält nicht mehrere Renderdurchläufe. Wäre dies der Fall, würden wir für jeden Durchlauf eine ähnliche Renderkette erstellen und das Setup jeweils je nach Bedarf ändern.

Direct3D11-Renderkette

```cpp
// Clear the back buffer.
const float midnightBlue[] = { 0.098f, 0.098f, 0.439f, 1.000f };
m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    midnightBlue
    );

// Set the render target. This starts the drawing operation.
m_d3dContext->OMSetRenderTargets(
    1,  // number of render target views for this drawing operation.
    m_renderTargetView.GetAddressOf(),
    nullptr
    );


// Rotate the cube 1 degree per frame.
XMStoreFloat4x4(
    &m_constantBufferData.model, 
    XMMatrixTranspose(XMMatrixRotationY(m_frameCount++ * XM_PI / 180.f))
    );

// Copy the updated constant buffer from system memory to video memory.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,      // update the 0th subresource
    NULL,   // use the whole destination
    &m_constantBufferData,
    0,      // default pitch
    0       // default pitch
    );


// Send vertex data to the Input Assembler stage.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;

m_d3dContext->IASetVertexBuffers(
    0,  // start with the first vertex buffer
    1,  // one vertex buffer
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset
    );

m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0   // no offset
    );

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());


// Set the vertex shader.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0
    );

// Set the vertex shader constant buffer data.
m_d3dContext->VSSetConstantBuffers(
    0,  // register 0
    1,  // one constant buffer
    m_constantBuffer.GetAddressOf()
    );


// Set the pixel shader.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0
    );


// Draw the cube.
m_d3dContext->DrawIndexed(
    m_indexCount,
    0,  // start with index 0
    0   // start with vertex 0
    );
```

Die Swapchain ist Teil der Grafikinfrastruktur, sodass wir unsere DXGI-Swapchain verwenden, um den fertigen Frame darzustellen. DXGI blockiert den Aufruf bis zum nächsten vsync-Vorgang. Dann erfolgt die Rückgabe, und die Spielschleife kann mit der nächsten Iteration fortfahren.

Darstellen eines Frames auf dem Bildschirm mithilfe von DirectX11

```cpp
m_swapChain->Present(1, 0);
```

Die gerade erstellte Renderkette wird von einer Spielschleife aufgerufen, die in der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode implementiert ist. Dies wird unter [Teil3: Viewport und Spielschleife](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md) veranschaulicht.

 

 







<!--HONumber=Jun16_HO4-->


