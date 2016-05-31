---
author: mtoepke
title: Erstellen von Shadern und Zeichnen von Grundtypen
description: Im Folgenden wird das Kompilieren und Erstellen von Shadern mit HLSL-Quelldateien veranschaulicht, die zum Zeichnen von Grundtypen auf dem Bildschirm verwendet werden.
ms.assetid: 91113bbe-96c9-4ef9-6482-39f1ff1a70f4
---

# Erstellen von Shadern und Zeichnen von Grundtypen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Im Folgenden wird das Kompilieren und Erstellen von Shadern mit HLSL-Quelldateien veranschaulicht, die zum Zeichnen von Grundtypen auf dem Bildschirm verwendet werden.

Ein gelbes Dreieck wird mit Vertex- und Pixelshadern erstellt. Nach dem Erstellen des Direct3D-Geräts, der Swapchain und der Renderingzielansicht werden Daten aus Binärshaderobjektdateien auf dem Datenträger gelesen.

**Ziel:**  Shader erstellen und Grundtypen zeichnen.

## Voraussetzungen


Es wird davon ausgegangen, dass Sie mit C+ vertraut sind. Sie müssen außerdem mit den grundlegenden Konzepten der Grafikprogrammierung vertraut sein.

Ferner wird davon ausgegangen, dass Sie sich mit dem Dokument [Schnellstart: Einrichten von DirectX-Ressourcen und Anzeigen eines Bilds](setting-up-directx-resources.md) vertraut gemacht haben.

**Zeitaufwand:** 20 Minuten.

## Anweisungen

### 1. Kompilieren von HLSL-Quelldateien

Microsoft Visual Studio verwendet den HLSL-Codecompiler [fxc.exe](https://msdn.microsoft.com/library/windows/desktop/bb232919), um die HLSL-Quelldateien („SimpleVertexShader.hlsl“ und „SimplePixelShader.hlsl“) in CSO-Binärshaderobjektdateien („SimpleVertexShader.cso“ und „SimplePixelShader.cso“) zu kompilieren. Weitere Infos über den HLSL-Codecompiler finden Sie im Effektcompiler-Tool. Weitere Infos über das Kompilieren von Shadercode finden unter [Kompilieren von Shadern](https://msdn.microsoft.com/library/windows/desktop/bb509633).

Nachfolgend finden Sie den Code in der Datei „SimpleVertexShader.hlsl“:

```hlsl
struct VertexShaderInput
{
    DirectX::XMFLOAT2 pos : POSITION;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // For this lesson, set the vertex depth value to 0.5, so it is guaranteed to be drawn.
    vertexShaderOutput.pos = float4(input.pos, 0.5f, 1.0f);

    return vertexShaderOutput;
}
```

Nachfolgend finden Sie den Code in der Datei „SimplePixelShader.hlsl“:

```hlsl
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

### 2. Lesen von Daten von einem Datenträger

Hier wird die DX::ReadDataAsync-Funktion aus „DirectXHelper.h“ in der DirectX 11-App-Vorlage (Universal Windows) verwendet, um asynchron Daten aus einer Datei auf dem Datenträger zu lesen.

### 3. Erstellen von Vertex- und Pixelshadern

Zunächst werden Daten aus der Datei „SimpleVertexShader.cso“ gelesen. Anschließend werden die Daten dem Bytearray *vertexShaderBytecode* zugewiesen. [
            **ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) wird mit dem Bytearray aufgerufen, um den Vertexshader ([**ID3D11VertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476641)) zu erstellen. Die Vertextiefe wird in der Quelldatei „SimpleVertexShader.hlsl“ auf den Wert 0,5 festgelegt, um zu garantieren, dass das Dreieck gezeichnet wird. Ein Array von [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180)-Strukturen wird gefüllt, um das Layout des Vertexshadercodes zu beschreiben. Im Anschluss wird [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) aufgerufen, um das Layout zu erstellen. Das Array verfügt über ein Layoutelement, das die Vertexposition definiert. Zunächst werden Daten aus der Datei „SimplePixelShader.cso“ gelesen. Anschließend werden die Daten dem Bytearray *pixelShaderBytecode* zugewiesen. [
            **ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) wird mit dem Bytearray aufgerufen, um den Pixelshader ([**ID3D11PixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476576)) zu erstellen. Der Pixelwert wird in der Quelldatei „SimplePixelShader.hlsl“ auf (1,1,1,1) festgelegt, damit das Dreieck gelb wird. Wenn Sie die Farbe ändern möchten, können Sie diesen Wert ändern.

Vertex- und Indexpuffer werden erstellt, um ein einfaches Dreieck zu definieren. Zu diesem Zweck wird zunächst das Dreieck definiert. Anschließend werden die Vertex- und Indexpuffer ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) und [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)) mithilfe der Dreieckdefinition beschrieben, bevor dann [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) einmal für jeden Puffer aufgerufen wird.

```cpp
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        // Load the raw vertex shader bytecode from disk and create a vertex shader with it.
        auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode) {


          ComPtr<ID3D11VertexShader> vertexShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateVertexShader(
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  nullptr,
                  &vertexShader
                  )
              );

          // Create an input layout that matches the layout defined in the vertex shader code.
          // For this lesson, this is simply a DirectX::XMFLOAT2 vector defining the vertex position.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
          };

          ComPtr<ID3D11InputLayout> inputLayout;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateInputLayout(
                  basicVertexLayoutDesc,
                  ARRAYSIZE(basicVertexLayoutDesc),
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  &inputLayout
                  )
              );
        });
        
        // Load the raw pixel shader bytecode from disk and create a pixel shader with it.
        auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode) {
          ComPtr<ID3D11PixelShader> pixelShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreatePixelShader(
                  pixelShaderBytecode->Data,
                  pixelShaderBytecode->Length,
                  nullptr,
                  &pixelShader
                  )
              );
        });

        // Create vertex and index buffers that define a simple triangle.
        auto createTriangleTask = (createPSTask && createVSTask).then([this] () {

          DirectX::XMFLOAT2 triangleVertices[] =
          {
              float2(-0.5f, -0.5f),
              float2( 0.0f,  0.5f),
              float2( 0.5f, -0.5f),
          };

          unsigned short triangleIndices[] =
          {
              0, 1, 2,
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(float2) * ARRAYSIZE(triangleVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = triangleVertices;
          vertexBufferData.SysMemPitch = 0;
          vertexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> vertexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &vertexBufferDesc,
                  &vertexBufferData,
                  &vertexBuffer
                  )
              );

          D3D11_BUFFER_DESC indexBufferDesc;
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(triangleIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = triangleIndices;
          indexBufferData.SysMemPitch = 0;
          indexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> indexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &indexBufferDesc,
                  &indexBufferData,
                  &indexBuffer
                  )
              );
        });
```

Mithilfe der Vertex- und Pixelshader, des Vertexshaderlayouts sowie der Vertex- und Indexpuffer wird dann ein gelbes Dreieck gezeichnet.

### 4. Zeichnen des Dreiecks und Darstellen des gerenderten Bilds

Wir geben eine endlose Schleife zum fortlaufenden Rendern und Anzeigen der Szene ein. Wir rufen [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) auf, um das Renderziel als Ausgabeziel anzugeben. [
            **ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) wird mit dem Wert „{ 0.071f, 0.04f, 0.561f, 1.0f }“ aufgerufen, um das Renderingziel durchgehend blau anzuzeigen.

In der Endlosschleife wird ein gelbes Dreieck auf der blauen Oberfläche gezeichnet.

**So zeichnen Sie ein gelbes Dreieck**

1.  Wir rufen zunächst [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) auf, um das Streamen der Vertexpufferdaten in die Eingabeassemblerphase zu beschreiben.
2.  Als Nächstes rufen wir [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) und [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) auf, um die Vertex- und Indexpuffer an den Eingabeassemblerzustand zu binden.
3.  Nun rufen wir [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) mit dem [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP)-Wert auf, um für den Eingabeassemblerzustand anzugeben, dass die Vertexdaten als Dreieckskette interpretiert werden.
4.  Im nächsten Schritt rufen wir [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) auf, um die Vertexshaderphase mit dem Vertexshadercode zu initialisieren. Zudem rufen wir [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) auf, um die Pixelshaderphase mit dem Pixelshadercode zu initialisieren.
5.  Abschließend wird [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) aufgerufen, um das Dreieck zu zeichnen und an die Renderingpipeline zu senden.

Wir rufen [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) auf, um das gerenderte Bild im Fenster darzustellen.

```cpp
            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(float2);
            UINT offset = 0;
            m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset
                );

            m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0
                );

            m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

            // Set the vertex and pixel shader stage state.
            m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(triangleIndices),
                0,
                0
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
```

## Zusammenfassung und nächste Schritte


In diesem Thema wurde ein gelbes Dreieck mit Vertex- und Pixelshadern erstellt und gezeichnet.

Als Nächstes wird ein kreisender 3D-Würfel erstellt, und Lichteffekte werden auf den Würfel angewendet.

[Verwenden von Tiefe und Effekten auf Grundtypen](using-depth-and-effects-on-primitives.md)

 

 






<!--HONumber=May16_HO2-->


