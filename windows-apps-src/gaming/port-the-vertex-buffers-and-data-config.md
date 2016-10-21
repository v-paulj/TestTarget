---
author: mtoepke
title: Portieren der Vertexpuffer und -daten
description: "In diesem Schritt definieren Sie die Vertexpuffer für die Gitter und die Indexpuffer, mit deren Hilfe die Shader die Vertices in einer angegebenen Reihenfolge durchlaufen können."
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ee8b3f693e40d9c0fba679a44ebcd4986d06d7ac

---

# Portieren der Vertexpuffer und -Daten


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)
-   [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588)

In diesem Schritt definieren Sie die Vertexpuffer für die Gitter und die Indexpuffer, mit deren Hilfe die Shader die Scheitelpunkte (Vertices) in einer angegebenen Reihenfolge durchlaufen können.

Zuerst untersuchen wir das hartcodierte Modell für das Würfelgitter, das verwendet werden soll. Für beide Darstellungen wurden die Scheitelpunkte in Form einer Dreiecksliste organisiert (im Gegensatz zu einer Kette oder einem anderen effizienteren Dreieckslayout). Alle Scheitelpunkte in beiden Darstellungen verfügen außerdem über zugeordnete Indizes und Farbwerte. Ein Großteil des Direct3D-Codes in diesem Thema bezieht sich auf Variablen und Objekte, die im Direct3D-Projekt definiert wurden.

Dies ist der Würfel für die Verarbeitung mit OpenGLES2.0. In der Beispielimplementierung besteht jeder Scheitelpunkt (Vertex) aus sieben Gleitkommawerten: drei Positionskoordinaten gefolgt von vier RGBA-Farbwerten.

```cpp
#define CUBE_INDICES 36
#define CUBE_VERTICES 8

GLfloat cubeVertsAndColors[] = 
{
  -0.5f, -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, 1.0f,
  -0.5f, -0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f,
  -0.5f,  0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 1.0f,
  -0.5f,  0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 1.0f,
  0.5f, -0.5f,  0.5f, 1.0f, 0.0f, 1.0f, 1.0f,
  0.5f, -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, 1.0f,  
  0.5f,  0.5f,  0.5f, 1.0f, 1.0f, 1.0f, 1.0f,
  0.5f,  0.5f, -0.5f, 1.0f, 1.0f, 0.0f, 1.0f
};

GLuint cubeIndices[] = 
{
  0, 1, 2, // -x
  1, 3, 2,

  4, 6, 5, // +x
  6, 7, 5,

  0, 5, 1, // -y
  5, 6, 1,

  2, 6, 3, // +y
  6, 7, 3,

  0, 4, 2, // +z
  4, 6, 2,

  1, 7, 3, // -z
  5, 7, 1
};
```

Dies ist der gleiche Würfel für die Verarbeitung mit Direct3D11.

```cpp
VertexPositionColor cubeVerticesAndColors[] = 
// struct format is position, color
{
  {XMFLOAT3(-0.5f, -0.5f, -0.5f), XMFLOAT3(0.0f, 0.0f, 0.0f)},
  {XMFLOAT3(-0.5f, -0.5f,  0.5f), XMFLOAT3(0.0f, 0.0f, 1.0f)},
  {XMFLOAT3(-0.5f,  0.5f, -0.5f), XMFLOAT3(0.0f, 1.0f, 0.0f)},
  {XMFLOAT3(-0.5f,  0.5f,  0.5f), XMFLOAT3(0.0f, 1.0f, 1.0f)},
  {XMFLOAT3( 0.5f, -0.5f, -0.5f), XMFLOAT3(1.0f, 0.0f, 0.0f)},
  {XMFLOAT3( 0.5f, -0.5f,  0.5f), XMFLOAT3(1.0f, 0.0f, 1.0f)},
  {XMFLOAT3( 0.5f,  0.5f, -0.5f), XMFLOAT3(1.0f, 1.0f, 0.0f)},
  {XMFLOAT3( 0.5f,  0.5f,  0.5f), XMFLOAT3(1.0f, 1.0f, 1.0f)},
};
        
unsigned short cubeIndices[] = 
{
  0, 2, 1, // -x
  1, 2, 3,

  4, 5, 6, // +x
  5, 7, 6,

  0, 1, 5, // -y
  0, 5, 4,

  2, 6, 7, // +y
  2, 7, 3,

  0, 4, 6, // -z
  0, 6, 2,

  1, 3, 7, // +z
  1, 7, 5
};
```

Es ist erkennbar, dass der Würfel im OpenGLES2.0-Code in einem "rechtshändigen" Koordinatensystem dargestellt wird, während der Würfel im Direct3D-spezifischen Code in einem "linkshändigen" Koordinatensystem dargestellt wird. Beim Importieren Ihrer eigenen Gitterdaten müssen Sie die Koordinaten der z-Achse für Ihr Modell umkehren und die Indizes für die einzelnen Gitter entsprechend ändern, um die Dreiecke gemäß der Änderung im Koordinatensystem zu durchlaufen.

Wir setzen voraus, dass das Verschieben des Würfelgitters aus dem rechtshändigen OpenGL ES 2.0-Koordinatensystem in das linkshändige Direct3D-Koordinatensystem erfolgreich war. Als Nächstes sehen wir uns an, wie die Würfeldaten für die Verarbeitung in beiden Modellen geladen werden.

## Anweisungen

### Schritt 1: Erstellen eines Eingabelayouts

In OpenGL ES 2.0 werden die Vertexdaten als Attribute angegeben, die für die Shaderobjekte bereitgestellt und von diesen gelesen werden. Normalerweise geben Sie eine Zeichenfolge, die den im GLSL-Code des Shaders verwendeten Attributnamen enthält, für das Shaderprogrammobjekt an und erhalten einen Speicherbereich zurück, den Sie für den Shader bereitstellen können. In diesem Beispiel enthält ein Vertexpufferobjekt eine Liste mit benutzerdefinierten Vertexstrukturen, die wie folgt definiert und formatiert sind:

OpenGLES2.0: Konfigurieren der Attribute, in denen die Informationen pro Vertex enthalten sind

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

In OpenGL ES 2.0 gelten Eingabelayouts implizit. Sie verwenden eine normale GL\_ELEMENT\_ARRAY\_BUFFER-Struktur und geben die Breite und den Versatz an, damit der Vertex-Shader die Daten nach dem Hochladen interpretieren kann. Sie informieren den Shader mithilfe von **glVertexAttribPointer**, bevor gerendert wird, welche Attribute welchen Teilen der einzelnen Vertexdatenblöcke zugeordnet werden.

In Direct3D müssen Sie beim Erstellen des Puffers ein Eingabelayout angeben, um die Struktur der Vertexdaten im Vertexpuffer zu beschreiben, und nicht vor dem Zeichnen der Geometrie. Dazu verwenden Sie ein Eingabelayout, das dem Layout der Daten für die einzelnen Scheitelpunkte im Speicher entspricht. Es ist sehr wichtig, dies genau anzugeben!

Hier erstellen Sie eine Eingabebeschreibung als Array mit [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180)-Strukturen.

Direct3D: Definieren der Beschreibung eines Eingabelayouts

``` syntax
struct VertexPositionColor
{
  DirectX::XMFLOAT3 pos;
  DirectX::XMFLOAT3 color;
};

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

```

Mit dieser Eingabebeschreibung wird ein Vertex als ein Vektorpaar mit jeweils dreiKoordinaten definiert: ein 3D-Vektor zum Speichern der Vertexposition in den Modellkoordinaten und ein anderer 3D-Vektor zum Speichern des RGB-Farbwerts, der dem Vertex zugeordnet ist. In diesem Fall verwenden Sie das 3x32-Bit-Gleitkommaformat. Elemente dieses Typs sind im Code als `XMFLOAT3(X.Xf, X.Xf, X.Xf)` dargestellt. Sie sollten Typen aus der [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415574)-Bibliothek immer bei der Behandlung von Daten verwenden, die von einem Shader genutzt werden. So wird die richtige Verpackung und Ausrichtung der Daten sichergestellt. (Verwenden Sie beispielsweise [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475) oder [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) für Vektordaten und [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) für Matrizen.)

Eine Liste aller möglichen Formattypen finden Sie unter [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059).

Nachdem das Layout für die Eingabe pro Vertex definiert wurde, erstellen Sie das Layoutobjekt. Im folgenden Code schreiben Sie es in **m\_inputLayout**, eine Variable vom Typ **ComPtr** (die auf ein Objekt vom Typ [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575) verweist). **fileData** enthält das kompilierte Vertexshaderobjekt aus dem vorherigen Schritt [Portieren der Shader](port-the-shader-config.md).

Direct3D: Erstellen des vom Vertexpuffer verwendeten Eingabelayouts

``` syntax
Microsoft::WRL::ComPtr<ID3D11InputLayout>      m_inputLayout;

// ...

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileShaderData->Length,
  &m_inputLayout
);
```

Wir haben das Eingabelayout definiert. Als Nächstes erstellen wir einen Puffer, der dieses Layout nutzt, und laden die Daten des Würfelgitters in den Puffer.

### Schritt 2: Erstellen und Laden der Vertexpuffer

Erstellen Sie in OpenGL ES 2.0 ein Pufferpaar: einen für die Positionsdaten und einen für die Farbdaten. (Sie können auch eine Struktur erstellen, die beide zusammen mit einem einzelnen Puffer enthält.) Binden Sie jeden der Puffer sowie die Schreibposition und die Farbdaten darin ein. Später während der Renderfunktion binden Sie die Puffer erneut und stellen das Format der Daten im Puffer für den Shader bereit, damit diese richtig interpretiert werden können.

OpenGLES2.0: Binden der Vertexpuffer

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

In Direct3D werden von Shadern aufrufbare Puffer als [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)-Strukturen dargestellt. Zum Binden dieses Puffers an ein Shaderobjekt erstellen Sie mit [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) eine CD3D11\_BUFFER\_DESC-Struktur für jeden Puffer. Legen Sie anschließend den Puffer des Direct3D-Gerätekontexts fest, indem Sie eine für den Puffertyp spezifische set-Methode wie [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) aufrufen.

Geben Sie beim Festlegen des Puffers die Breite (die Größe des Datenelements für einen einzelnen Vertex) sowie den Versatz (wo die eigentlichen Vertexdaten beginnen) zum Anfang des Puffers an.

Beachten Sie, dass der Zeiger auf das **vertexIndices**-Array dem **pSysMem**-Feld der [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)-Struktur zugewiesen wird. Wenn dies nicht korrekt angegeben wird, ist das Gitter beschädigt oder leer!

Direct3D: Erstellen und Festlegen des Vertexpuffers

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
        
// ...

UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
  0,
  1,
  m_vertexBuffer.GetAddressOf(),
  &stride,
  &offset);
```

### Schritt 3: Erstellen und Laden des Indexpuffers

Indexpuffer stellen eine effiziente Möglichkeit dar, für den Vertex-Shader das Suchen nach einzelnen Scheitelpunkten (Vertices) zuzulassen. Obwohl sie nicht zwingend erforderlich sind, werden sie in diesem Beispielrenderer verwendet. Wie bei Vertexpuffern in OpenGLES2.0 auch, wird ein Indexpuffer erstellt und als allgemeiner Puffer gebunden. Anschließend werden die bereits erstellten Indizes in den Puffer kopiert.

Wenn Sie bereit zum Zeichnen sind, binden Sie sowohl den Vertexpuffer als auch den Indexpuffer erneut und rufen **glDrawElements** auf.

OpenGL ES 2.0: Senden der Indexreihenfolge an den Draw-Aufruf

``` syntax
GLuint indexBuffer;

// ...

glGenBuffers(1, &renderer->indexBuffer);    
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);   
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 
  sizeof(GLuint) * CUBE_INDICES, 
  renderer->vertexIndices, 
  GL_STATIC_DRAW);

// ...
// Drawing function

// Bind the index buffer
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glDrawElements (GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);
```

Für Direct3D ist der Prozess sehr ähnlich, jedoch etwas didaktischer gestaltet. Stellen Sie den Indexpuffer als Direct3D-Unterressource für die [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)-Schnittstelle bereit, die Sie beim Konfigurieren von Direct3D erstellt haben. Rufen Sie zu diesem Zweck [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588) mit der konfigurierten Unterressource wie folgt für das Indexarray auf. (Beachten Sie auch hier, dass der Zeiger auf das **cubeIndices**-Array dem **pSysMem**-Feld der [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)-Struktur zugewiesen wird.)

Direct3D: Erstellen des Indexpuffers

``` syntax
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

// ...

m_d3dContext->IASetIndexBuffer(
  m_indexBuffer.Get(),
  DXGI_FORMAT_R16_UINT,
  0);
```

Später werden Sie die Dreiecke wie folgt mit einem Aufruf von [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) (bzw. [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) für nicht indizierte Scheitelpunkte) zeichnen. (Ausführlichere Informationen erhalten Sie bereits jetzt unter [Zeichnen auf den Bildschirm](draw-to-the-screen.md).)

Direct3D: Zeichnen der indizierten Scheitelpunkte

``` syntax
m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());

// ...

m_d3dContext->DrawIndexed(
  m_indexCount,
  0,
  0);
```

## Vorheriger Schritt


[Portieren der Shaderobjekte](port-the-shader-config.md)

## Nächster Schritt

[Portieren des GLSL-Codes](port-the-glsl.md)

## Anmerkungen

Fügen Sie den Code, mit dem unter [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) Methoden aufgerufen werden, beim Strukturieren von Direct3D jeweils in eine Methode ein, die aufgerufen wird, wenn die Geräteressourcen neu erstellt werden müssen. (In der Direct3D-Projektvorlage befindet sich dieser Code in den **CreateDeviceResource**-Methoden des Rendererobjekts. Der Code zum Aktualisieren des Gerätekontexts ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) wird jedoch in der **Render**-Methode platziert, da dort die Shaderphasen tatsächlich erstellt und die Daten gebunden werden.

## Verwandte Themen


* [Portieren eines einfachen OpenGL ES 2.0-Renderers zu Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Portieren der Shaderobjekte](port-the-shader-config.md)
* [Portieren der Vertexpuffer und -daten](port-the-vertex-buffers-and-data-config.md)
* [Portieren des GLSL-Codes](port-the-glsl.md)

 

 







<!--HONumber=Aug16_HO3-->


