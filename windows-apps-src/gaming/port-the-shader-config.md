---
author: mtoepke
title: Portieren der Shaderobjekte
description: "Beim Portieren des einfachen Renderers aus OpenGL ES 2.0 ist der erste Schritt das Einrichten der äquivalenten Vertex- und Fragmentshaderobjekte in Direct3D 11. Stellen Sie außerdem sicher, dass das Hauptprogramm mit den Shaderobjekten kommunizieren kann, nachdem diese kompiliert wurden."
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 17d66e217e40eca0653078820746746eb23185e1

---

# Portieren der Shaderobjekte


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)
-   [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)

Beim Portieren des einfachen Renderers aus OpenGL ES 2.0 ist der erste Schritt das Einrichten der äquivalenten Vertex- und Fragmentshaderobjekte in Direct3D 11. Stellen Sie außerdem sicher, dass das Hauptprogramm mit den Shaderobjekten kommunizieren kann, nachdem diese kompiliert wurden.

> 
            **Hinweis**  Haben Sie ein neues Direct3D-Projekt erstellt? Falls nicht, befolgen Sie die Anleitung unter [Erstellen eines neuen DirectX 11-Projekts für die Universelle Windows-Plattform (UWP)](user-interface.md). Bei dieser exemplarischen Vorgehensweise wird vorausgesetzt, dass Sie die DXGI- und Direct3D-Ressourcen zum Zeichnen auf den Bildschirm erstellt haben, die auch in der Vorlage bereitgestellt werden.

 

Wie unter OpenGLES2.0 auch, müssen die kompilierten Shader in Direct3D einem Kontext für das Zeichnen zugeordnet werden. Direct3D verfügt jedoch nicht per se über das Konzept eines Shaderprogrammobjekts. Daher weisen Sie die Shader direkt einem [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) zu. Bei diesem Schritt wird der OpenGL ES 2.0-Prozess zum Erstellen und Binden von Shaderobjekten eingehalten, und es werden die entsprechenden API-Verhalten in Direct3D bereitgestellt.

Anweisungen
------------

### Schritt 1: Kompilieren der Shader

In diesem einfachen OpenGL ES 2.0-Beispiel werden die Shader als Textdateien gespeichert und als Zeichenfolgendaten für die Laufzeitkompilierung geladen.

OpenGLES2.0: Kompilieren eines Shaders

``` syntax
GLuint __cdecl CompileShader (GLenum shaderType, const char *shaderSrcStr)
// shaderType can be GL_VERTEX_SHADER or GL_FRAGMENT_SHADER. Returns 0 if compilation fails.
{
  GLuint shaderHandle;
  GLint compiledShaderHandle;
   
  // Create an empty shader object.
  shaderHandle = glCreateShader(shaderType);

  if (shaderHandle == 0)
  return 0;

  // Load the GLSL shader source as a string value. You could obtain it from
  // from reading a text file or hardcoded.
  glShaderSource(shaderHandle, 1, &shaderSrcStr, NULL);
   
  // Compile the shader.
  glCompileShader(shaderHandle);

  // Check the compile status
  glGetShaderiv(shaderHandle, GL_COMPILE_STATUS, &compiledShaderHandle);

  if (!compiledShaderHandle) // error in compilation occurred
  {
    // Handle any errors here.
              
    glDeleteShader(shaderHandle);
    return 0;
  }

  return shaderHandle;

}
```

In Direct3D werden Shader nicht während der Laufzeit kompiliert. Sie werden immer in CSO-Dateien kompiliert, wenn auch die restlichen Daten des Programms kompiliert werden. Wenn Sie die App mit Microsoft Visual Studio kompilieren, werden die HLSL-Dateien in CSO-Dateien (.cso) kompiliert, die von der App geladen werden müssen. Achten Sie darauf, diese CSO-Dateien der App beim Verpacken hinzuzufügen!

> 
            **Hinweis**  Im folgenden Beispiel wird das asynchrone Laden und Kompilieren des Shaders mit dem **auto**-Schlüsselwort und der Lambdasyntax durchgeführt. ReadDataAsync() ist eine für die Vorlage implementierte Methode, mit der eine CSO-Datei als Array mit Bytedaten (fileData) eingelesen wird.

 

Direct3D 11: Kompilieren eines Shaders

``` syntax
auto loadVSTask = DX::ReadDataAsync(m_projectDir + "SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(m_projectDir + "SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](Platform::Array<byte>^ fileData) {

m_d3dDevice->CreateVertexShader(
  fileData->Data,
  fileData->Length,
  nullptr,
  &m_vertexShader);

auto createPSTask = loadPSTask.then([this](Platform::Array<byte>^ fileData) {
  m_d3dDevice->CreatePixelShader(
    fileData->Data,
    fileData->Length,
    nullptr,
    &m_pixelShader;
};
```

### Schritt 2: Erstellen und Laden der Vertex- und Fragmentshader (Pixelshader)

Unter OpenGL ES 2.0 wird ein so genanntes Shaderprogramm verwendet, das als Schnittstelle zwischen dem Hauptprogramm, das über die CPU ausgeführt wird, und den Shadern fungiert, die über die GPU ausgeführt werden. Shader werden kompiliert (oder aus kompilierten Quellen geladen) und einem Programm zugeordnet, das die Ausführung über die GPU ermöglicht.

OpenGLES2.0: Laden der Vertex- und Fragmentshader in ein Schattierungsprogramm

``` syntax
GLuint __cdecl LoadShaderProgram (const char *vertShaderSrcStr, const char *fragShaderSrcStr)
{
  GLuint programObject, vertexShaderHandle, fragmentShaderHandle;
  GLint linkStatusCode;

  // Load the vertex shader and compile it to an internal executable format.
  vertexShaderHandle = CompileShader(GL_VERTEX_SHADER, vertShaderSrcStr);
  if (vertexShaderHandle == 0)
  {
    glDeleteShader(vertexShaderHandle);
    return 0;
  }

   // Load the fragment/pixel shader and compile it to an internal executable format.
  fragmentShaderHandle = CompileShader(GL_FRAGMENT_SHADER, fragShaderSrcStr);
  if (fragmentShaderHandle == 0)
  {
    glDeleteShader(fragmentShaderHandle);
    return 0;
  }

  // Create the program object proper.
  programObject = glCreateProgram();
   
  if (programObject == 0)    return 0;

  // Attach the compiled shaders
  glAttachShader(programObject, vertexShaderHandle);
  glAttachShader(programObject, fragmentShaderHandle);

  // Compile the shaders into binary executables in memory and link them to the program object..
  glLinkProgram(programObject);

  // Check the project object link status and determine if the program is available.
  glGetProgramiv(programObject, GL_LINK_STATUS, &linkStatusCode);

  if (!linkStatusCode) // if link status <> 0
  {
    // Linking failed; delete the program object and return a failure code (0).

    glDeleteProgram (programObject);
    return 0;
  }

  // Deallocate the unused shader resources. The actual executables are part of the program object.
  glDeleteShader(vertexShaderHandle);
  glDeleteShader(fragmentShaderHandle);

  return programObject;
}

// ...

glUseProgram(renderer->programObject);
```

In Direct3D wird kein Shaderprogrammobjekt verwendet. Stattdessen werden die Shader erstellt, wenn eine der Shadererstellungsmethoden auf der [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)-Schnittstelle (z. B. [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) oder [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)) aufgerufen wird. Um die Shader für den aktuellen Zeichnungskontext festzulegen, werden diese einer entsprechenden [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)-Schnittstelle mit einer SetShader-Methode bereitgestellt, z. B. [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) für den Vertex-Shader oder [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) für den Fragmentshader.

Direct3D 11: Festlegen der Shader für den Kontext des Grafikgeräts für das Zeichnen

``` syntax
m_d3dContext->VSSetShader(
  m_vertexShader.Get(),
  nullptr,
  0);

m_d3dContext->PSSetShader(
  m_pixelShader.Get(),
  nullptr,
  0);
```

### Schritt 3: Definieren der Daten für die Shader

Im OpenGL ES 2.0-Beispiel wird ein **uniform**-Element für die Shaderpipeline deklariert:

-   
            **u\_mvpMatrix**: Ein 4x4-Array mit Float-Werten, welches die endgültige Modell-Ansicht-Projektion-Transformationsmatrix darstellt, das die Modellkoordinaten für den Würfel verwendet und sie in 2D-Projektionskoordinaten für die Scankonvertierung transformiert.

Zusätzlich zwei **attribute**-Werte für die Vertexdaten:

-   
            **a\_position**: Ein 4-Float-Vektor für die Modellkoordinaten eines Vertex.
-   
            **a\_color**: Ein 4-Float-Vektor für den RGBA-Farbwert, der dem Vertex zugeordnet ist.

OpenGLES2.0: GLSL-Definitionen für die uniform-Elemente und Attribute

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

Die entsprechenden Variablen des Hauptprogramms werden in diesem Fall als Felder des Rendererobjekts definiert. (Weitere Informationen finden Sie unter [So wird's gemacht: Portieren eines einfachen OpenGL ES 2.0-Renderers zu Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md).) Danach müssen die Speicherorte im Speicher angegeben werden, an denen das Hauptprogramm diese Werte für die Shaderpipeline bereitstellt. Dies erfolgt normalerweise direkt vor einem Draw-Aufruf:

OpenGLES2.0: Kennzeichnen des Speicherorts der uniform- und Attributdaten

``` syntax

// Inform the shader of the attribute locations
loc = glGetAttribLocation(renderer->programObject, "a_position");
glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), 0);
glEnableVertexAttribArray(loc);

loc = glGetAttribLocation(renderer->programObject, "a_color");
glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);


// Inform the shader program of the uniform location
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");
```

In Direct3D werden "Attribute" oder "uniform-Elemente" in dieser Form nicht verwendet (bzw. wird diese Syntax nicht freigegeben). Stattdessen werden Konstantenpuffer verwendet, die als Direct3D-Unterressourcen dargestellt werden. Diese Ressourcen werden vom Hauptprogramm und den Shaderprogrammen gemeinsam genutzt. Einige dieser Unterressourcen, wie Vertexpositionen und -farben, werden in Form von HLSL-Semantik beschrieben. Weitere Informationen zu Konstantenpuffern und zur HLSL-Semantik und deren Beziehung zu OpenGLES2.0-Konzepten finden Sie unter [Portieren von Framepufferobjekten, uniform-Elementen und Attributen](porting-uniforms-and-attributes.md).

Wenn dieser Prozess unter Direct3D durchgeführt werden soll, wird das „uniform“-Element in einen Direct3D-Konstantenpuffer („cbuffer“) konvertiert und erhält ein Register für die Suche per **register**-HLSL-Semantik. Die beiden Vertexattribute werden als Eingabeelemente für die Shaderpipelinephasen behandelt und ebenfalls mit [HLSL-Semantik](https://msdn.microsoft.com/library/windows/desktop/bb205574) (POSITION und COLOR0) zum Informieren der Shader versehen. Vom Pixelshader wird ein SV\_POSITION-Element verwendet, wobei mit dem Präfix „SV\_“ angegeben wird, dass es sich um einen von der GPU erzeugten Systemwert handelt. (In diesem Fall ist dies eine Pixelposition, die während der Scankonvertierung erzeugt wurde.) Die Elemente VertexShaderInput und PixelShaderInput werden nicht als Konstantenpuffer deklariert, weil VertexShaderInput zum Definieren des Vertexpuffers verwendet wird (siehe [Portieren der Vertexpuffer und -daten](port-the-vertex-buffers-and-data-config.md)). Die Daten für PixelShaderInput werden als Folge einer vorherigen Phase der Pipeline erzeugt. In diesem Fall ist dies der Vertex-Shader.

Direct3D: HLSL-Definitionen für die Konstantenpuffer und Vertexdaten

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float4 pos : POSITION;
  float4 color : COLOR0;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Weitere Informationen zur Portierung zu Konstantenpuffern und zur Anwendung der HLSL-Semantik finden Sie unter [Portieren von Framepufferobjekten, uniform-Elementen und Attributen](porting-uniforms-and-attributes.md).

Dies sind die Strukturen für das Layout der Daten, die mit einem Konstanten- oder Vertexpuffer an die Shaderpipeline übergeben werden.

Direct3D 11: Deklarieren des Layouts für Konstanten- und Vertexpuffer

``` syntax
// Constant buffer used to send MVP matrices to the vertex shader.
struct ModelViewProjectionConstantBuffer
{
  DirectX::XMFLOAT4X4 modelViewProjection;
};

// Used to send per-vertex data to the vertex shader.
struct VertexPositionColor
{
  DirectX::XMFLOAT4 pos;
  DirectX::XMFLOAT4 color;
};
```

Verwenden Sie die DirectXMath-Typen „XM\*“ für die Konstantenpufferelemente, da sie eine gute Verpackung und Ausrichtung der Inhalte ermöglichen, wenn diese an die Shaderpipeline gesendet werden. Wenn Sie standardmäßige Gleitkommatypen und Arrays der Windows-Plattform verwenden, müssen Sie die Verpackung und Ausrichtung selbst durchführen.

Erstellen Sie zum Binden eines Konstantenpuffers eine Layoutbeschreibung als [**CD3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/jj151620)-Struktur, und übergeben Sie diese an [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501). Übergeben Sie den Konstantenpuffer in der Rendermethode dann vor dem Zeichnen an [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486).

Direct3D 11: Binden des Konstantenpuffers

``` syntax
CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);

m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);

// ...

// Only update shader resources that have changed since the last frame.
m_d3dContext->UpdateSubresource(
  m_constantBuffer.Get(),
  0,
  NULL,
  &m_constantBufferData,
  0,
  0);
```

Der Vertexpuffer wird auf ähnliche Weise erstellt und aktualisiert. Dies ist im nächsten Schritt, [Portieren der Vertexpuffer und -daten](port-the-vertex-buffers-and-data-config.md), beschrieben.

Nächster Schritt
---------

[Portieren der Vertexpuffer und -daten](port-the-vertex-buffers-and-data-config.md)
## Verwandte Themen


[Portieren eines einfachen OpenGL ES 2.0-Renderers zu Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Portieren der Vertexpuffer und -daten](port-the-vertex-buffers-and-data-config.md)

[Portieren des GLSL-Codes](port-the-glsl.md)

[Zeichnen auf den Bildschirm](draw-to-the-screen.md)

 

 







<!--HONumber=Jun16_HO4-->


