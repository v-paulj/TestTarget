---
title: Vergleichen von OpenGL ES 2.0-Puffern, „uniform“-Elementen und Vertexattributen mit Direct3D
description: Während des Portierens zu Direct3D 11 aus OpenGL ES 2.0 müssen Sie die Syntax und das API-Verhalten zum Übergeben von Daten zwischen der App und den Shaderprogrammen ändern.
ms.assetid: 9b215874-6549-80c5-cc70-c97b571c74fe
---

# Vergleichen von OpenGL ES 2.0-Puffern, „uniform“-Elementen und Vertexattributen mit Direct3D


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)
-   [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)

Während des Portierens zu Direct3D 11 aus OpenGL ES 2.0 müssen Sie die Syntax und das API-Verhalten zum Übergeben von Daten zwischen der App und den Shaderprogrammen ändern.

In OpenGL ES 2.0 werden Daten an und von Shaderprogrammen auf die folgenden Arten übergeben: als uniform-Elemente für Konstantendaten, als Attribute für Vertexdaten und als Pufferobjekte für andere Ressourcendaten (z. B. Texturen). In Direct3D 11 entsprechen diese Elemente grob Konstantenpuffern, Vertexpuffern und Unterressourcen. Obwohl auf den ersten Blick Ähnlichkeiten bestehen, unterscheiden sich diese Elemente in der Nutzung jedoch relativ stark.

Unten ist die grundlegende Zuordnung angegeben.

| OpenGL ES 2.0             | Direct3D 11                                                                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniform-Variable                   | Konstantenpufferfeld (**cbuffer**)                                                                                                                                                |
| Attribut                 | Vertexpufferelement-Feld mit Eingabelayout und Kennzeichnung durch spezielle HLSL-Semantik                                                                                |
| buffer-Objekt             | Puffer; allgemeine Pufferdefinitionen unter [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) und [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) |
| Framepufferobjekt (Frame Buffer Object, FBO) | Renderziel(e); siehe [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) mit [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)                                       |
| Hintergrundpuffer               | Swapchain mit „Hintergrundpuffer“-Oberfläche; siehe [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) mit [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343) als Anhang                       |

 

## Portieren von Puffern


In OpenGL ES 2.0 wird zum Erstellen und Binden jeglicher Arten von Puffern in der Regel das folgende Muster verwendet:

-   Aufrufen von glGenBuffers, um einen oder mehrere Puffer zu erzeugen und dafür Handles zurückzugeben
-   Aufrufen von „glBindBuffer“, um das Layout eines Puffers zu definieren, z. B. GL\_ELEMENT\_ARRAY\_BUFFER
-   Aufrufen von „glBufferData“, um den Puffer mit speziellen Daten (z. B. Vertexstrukturen, Indexdaten oder Farbdaten) in einem speziellen Layout aufzufüllen

Der am häufigsten verwendete Puffer ist der Vertexpuffer, der jeweils mindestens die Positionen der Scheitelpunkte (Vertices) in einem Koordinatensystem enthält. Normalerweise wird ein Vertex mithilfe einer Struktur dargestellt, in der die Positionskoordinaten, ein Normalenvektor zur Vertexposition, ein Tangentenvektor zur Vertexposition und Koordinaten für die Textursuche (uv) enthalten sind. Der Puffer enthält eine zusammenhängende Liste dieser Scheitelpunkte in einer bestimmten Reihenfolge (z. B. Dreiecksliste, kettenförmig oder fächerförmig), von denen zusammen die sichtbaren Polygone der Szene dargestellt werden. (In Direct3D 11 und in OpenGL ES 2.0 ist es ineffizient, mehrere Vertexpuffer pro Draw-Aufruf zu verwenden.)

Dies ist ein Beispiel für einen Vertexpuffer und einen Indexpuffer, die mit OpenGL ES 2.0 erstellt wurden:

OpenGL ES 2.0: Erstellen und Auffüllen eines Vertexpuffers und eines Indexpuffers

``` syntax
glGenBuffers(1, &renderer->vertexBuffer);
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertex) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);

glGenBuffers(1, &renderer->indexBuffer);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(int) * CUBE_INDICES, renderer->vertexIndices, GL_STATIC_DRAW);
```

Weitere Puffer sind Pixelpuffer und Zuordnungen, z. B. Texturen Für die Shaderpipeline ist das Rendern in Texturpuffer (pixmaps) oder das Rendern von Pufferobjekten zur Verwendung in nachfolgenden Shaderdurchläufen möglich. Im einfachsten Fall lautet der Fluss für den Aufruf wie folgt:

-   Aufrufen von glGenFramebuffers zum Erzeugen eines Framepufferobjekts
-   Aufrufen von glBindFramebuffer zum Binden des Framepufferobjekts für Schreibvorgänge
-   Aufrufen von glFramebufferTexture2D zum Zeichnen in eine angegebene Texturmap

In Direct3D 11 werden Pufferdatenelemente als „Unterressourcen“ angesehen und können von einzelnen Vertexdatenelementen bis hin zu MIP-Map-Texturen reichen.

-   Auffüllen einer [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)-Struktur mit der Konfiguration für ein Pufferdatenelement
-   Auffüllen einer [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)-Struktur mit der Größe der einzelnen Elemente im Puffer und mit dem Puffertyp
-   Aufrufen von [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575) mit diesen beiden Strukturen

Direct3D 11: Erstellen und Auffüllen eines Vertexpuffers und eines Indexpuffers

``` syntax
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(sizeof(cubeVertices), D3D11_BIND_VERTEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &vertexBufferDesc,
  &vertexBufferData,
  &m_vertexBuffer);

m_indexCount = ARRAYSIZE(cubeIndices);

D3D11_SUBRESOURCE_DATA indexBufferData = {0};
indexBufferData.pSysMem = cubeIndices;
indexBufferData.SysMemPitch = 0;
indexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC indexBufferDesc(sizeof(cubeIndices), D3D11_BIND_INDEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &indexBufferDesc,
  &indexBufferData,
  &m_indexBuffer);
    
```

Beschreibbare Pixelpuffer oder Maps, z. B. ein Framepuffer, können als [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)-Objekte erstellt werden. Diese können als Ressourcen an eine [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)- oder [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628)-Schnittstelle gebunden werden und nach dem Zeichnen mit der zugeordneten Swapchain angezeigt oder an einen Shader übergeben werden.

Direct3D 11: Erstellen eines Framepufferobjekts

``` syntax
ComPtr<ID3D11RenderTargetView> m_d3dRenderTargetViewWin;
// ...
ComPtr<ID3D11Texture2D> frameBuffer;

m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&frameBuffer));
m_d3dDevice->CreateRenderTargetView(
  frameBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

## Ändern von uniform-Elementen und uniform-Pufferobjekten in Direct3D-Konstantenpuffer


In OpenGL ES 2.0 sind uniform-Elemente der Mechanismus zum Bereitstellen von Konstantendaten für einzelne Shaderprogramme. Diese Daten können von den Shadern nicht geändert werden.

Zum Festlegen eines „uniform“-Elements werden normalerweise eine der „glUniform\*“-Methoden mit dem GPU-Speicherbereich für den Upload sowie ein Zeiger auf die Daten im App-Speicher angegeben. Nach dem Ausführen der „glUniform\*“-Methode befinden sich die „uniform“-Daten im GPU-Speicher. Von den Shadern, die dieses „uniform“-Element deklariert haben, kann darauf zugegriffen werden. Sie müssen sicherstellen, dass die Daten so verpackt sind, dass sie vom Shader basierend auf der uniform-Deklaration im Shader (mithilfe kompatibler Typen) interpretiert werden können.

OpenGL ES 2.0: Erstellen eines uniform-Elements und Durchführen von Uploads in das Element

``` syntax
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");

// ...

glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);
```

Im GLSL-Code eines Shaders sieht die entsprechende uniform-Deklaration wie folgt aus:

OpenGL ES 2.0: uniform-Deklaration für GLSL

``` syntax
uniform mat4 u_mvpMatrix;
```

Unter Direct3D werden uniform-Daten als "Konstantenpuffer" bezeichnet, die – wie uniform-Elemente auch – für die einzelnen Shader bereitgestellte Konstantendaten enthalten. Genauso wie bei uniform-Puffern ist es wichtig, die Konstantenpufferdaten exakt so im Speicher zu verpacken, wie sie vom Shader für die Interpretation erwartet werden. Bei Verwendung von DirectXMath-Typen (wie [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608)) anstelle von Plattformtypen (wie **float\*** oder **float\[4\]**) ist die richtige Ausrichtung von Datenelementen sichergestellt.

Konstantenpuffer müssen über ein zugeordnetes GPU-Register verfügen, mit dem für die GPU auf diese Daten verwiesen wird. Die Daten werden im Registerbereich so verpackt, wie dies vom Layout des Puffers vorgegeben wird.

Direct3D 11: Erstellen eines Konstantenpuffers und Durchführen des Datenuploads in den Puffer

``` syntax
struct ModelViewProjectionConstantBuffer
{
     DirectX::XMFLOAT4X4 mvp;
};

// ...

ModelViewProjectionConstantBuffer   m_constantBufferData;

// ...

XMStoreFloat4x4(&m_constantBufferData.mvp, mvpMatrix);

CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);
m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);
```

Im HLSL-Code eines Shaders sieht die entsprechende Konstantenpufferdeklaration wie folgt aus:

Direct3D 11: Deklaration des Konstantenpuffers für HLSL

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Beachten Sie, dass für jeden Konstantenpuffer ein Register deklariert werden muss. Unterschiedliche Direct3D-Featureebenen verfügen über unterschiedliche Höchstgrenzen für maximal verfügbare Register. Achten Sie daher darauf, dass Sie die Höchstzahl für die niedrigste von Ihnen genutzte Featureebene einhalten.

## Portieren von Vertexattributen zu einem Direct3D-Eingabelayout und zur HLSL-Semantik


Da Vertexdaten von der Shaderpipeline geändert werden können, müssen sie unter OpenGL ES 2.0 als "Attribute" angegeben werden, anstatt als "uniform-Elemente". (Dies wurde in neueren Versionen von OpenGL und GLSL geändert.) Vertexspezifische Daten wie die Vertexposition, Normalen, Tangenten und Farbwerte werden für Shader als Attributwerte bereitgestellt. Diese Attributwerte entsprechen speziellen Versatzwerten für die einzelnen Elemente in den Vertexdaten. Beispielsweise kann das erste Attribut auf die Positionskomponente eines einzelnen Vertex zeigen, das zweite Attribut auf die Normale usw.

Der grundlegende Prozess zum Verschieben der Vertexpufferdaten aus dem Hauptspeicher in die GPU lautet wie folgt:

-   Hochladen der Vertexdaten mit glBindBuffer
-   Abrufen des Bereichs der Attribute in der GPU mit „glGetAttribLocation“ Aufrufen für jedes Attribut im Vertexdatenelement
-   Aufrufen von glVertexAttribPointer zum Festlegen der richtigen Attributgröße und des Versatzes innerhalb eines einzelnen Vertexdatenelements; Durchführen dieses Schritts für jedes Attribut
-   Aktivieren der Informationen zum Vertexdatenlayout mit glEnableVertexAttribArray

OpenGL ES 2.0: Hochladen von Vertexpufferdaten in das Shaderattribut

``` syntax
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->vertexBuffer);
loc = glGetAttribLocation(renderer->programObject, "a_position");

glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), 0);
loc = glGetAttribLocation(renderer->programObject, "a_color");
glEnableVertexAttribArray(loc);

glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);
```

Im Vertex-Shader deklarieren Sie Attribute unter den gleichen Namen, die Sie im Aufruf von glGetAttribLocation deklariert haben.

OpenGL ES 2.0: Deklarieren eines Attributs in GLSL

``` syntax
attribute vec4 a_position;
attribute vec4 a_color;                     
```

Teilweise gilt für Direct3D der gleiche Prozess. Anstelle von Attributen werden Vertexdaten in Eingabepuffern bereitgestellt. Dazu gehören auch Vertexpuffer und die entsprechenden Indexpuffer. Da Direct3D nicht über die "Attributdeklaration" verfügt, müssen Sie ein Eingabelayout angeben. Damit werden die individuelle Komponente der Datenelemente im Vertexpuffer und die HLSL-Semantik deklariert, mit der angegeben wird, wo und wie diese Komponenten vom Vertex-Shader interpretiert werden sollen. Für die HLSL-Semantik ist es erforderlich, die Nutzung der einzelnen Komponenten mit einer speziellen Zeichenfolge zu definieren, über die das Shadermodul über ihren Zweck informiert wird. Beispielsweise werden Vertexpositionsdaten als POSITION, Normalendaten als NORMAL und Vertexfarbdaten als COLOR gekennzeichnet. (Andere Shaderphasen erfordern auch jeweils eine spezielle Semantik. Diese Semantiken verfügen basierend auf der Shaderphase über unterschiedliche Interpretationen.). Weitere Informationen zur HLSL-Semantik finden Sie unter [Portieren der Shaderpipeline](change-your-shader-loading-code.md) und [HLSL-Semantik](https://msdn.microsoft.com/library/windows/desktop/bb205574).

Der Prozess zum Festlegen der Vertex- und Indexpuffer und das Festlegen des Eingabelayouts wird zusammenfassend als "Eingabeassembly"-Phase (Input Assembly, IA) der Direct3D-Grafikpipeline bezeichnet.

Direct3D 11: Konfigurieren der Eingabeassembly-Phase

``` syntax
// Set up the IA stage corresponding to the current draw operation.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset);

m_d3dContext->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT,
        0);

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

Ein Eingabelayout wird deklariert, und es wird ein Vertex-Shader zugeordnet, indem das Format des Vertexdatenelements und die für die einzelnen Komponenten zu verwendende Semantik deklariert wird. Das von Ihnen erstellte Vertexelement-Datenlayout, das unter D3D11\_INPUT\_ELEMENT\_DESC beschrieben wird, muss dem Layout der jeweiligen Struktur entsprechen. Hier erstellen Sie ein Layout für Vertexdaten, die über zwei Komponenten verfügen:

-   Eine Vertexpositionskoordinate, die im Hauptspeicher als XMFLOAT3 dargestellt wird und bei der es sich um ein ausgerichtetes Array mit drei 32-Bit-Gleitkommawerten für die Koordinaten (x, y, z) handelt.
-   Ein Vertexfarbwert, der als XMFLOAT4 dargestellt wird und bei dem es sich um ein ausgerichtetes Array mit vier 32-Bit-Gleitkommawerten für die Farbe (RGBA) handelt.

Sie weisen jeweils eine Semantik und einen Formattyp zu. Anschließend übergeben Sie die Beschreibung an [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512). Das Eingabelayout wird beim Aufrufen von [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) verwendet, wenn Sie während der Rendermethode die Eingabeassembly einrichten.

Direct3D 11: Beschreiben eines Eingabelayouts mit spezieller Semantik

``` syntax
ComPtr<ID3D11InputLayout> m_inputLayout;

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileData->Length,
  &m_inputLayout);

// ...
// When we start the drawing process...

m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

Stellen Sie im letzten Schritt sicher, dass die Eingabedaten vom Shader verstanden werden, indem Sie die Eingabe deklarieren. Die im Layout zugewiesene Semantik wird verwendet, um im GPU-Speicher die richtigen Bereiche auszuwählen.

Direct3D 11: Deklarieren von Daten für die Shadereingabe mit HLSL-Semantik

``` syntax
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};
```

 

 






<!--HONumber=Mar16_HO1-->


