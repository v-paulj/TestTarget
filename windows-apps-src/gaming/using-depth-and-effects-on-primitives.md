---
author: mtoepke
title: Verwenden von Tiefen und Effekten in Primitiven
description: Hier zeigen wir Ihnen die Verwendung von Tiefen, Perspektiven, Farben und anderen Effekten in Primitiven.
ms.assetid: 71ef34c5-b4a3-adae-5266-f86ba257482a
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 47547a226a69b2631313972ed67b39b1f29de402

---

# Verwenden von Tiefen und Effekten in Primitiven


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Hier zeigen wir Ihnen die Verwendung von Tiefen, Perspektiven, Farben und anderen Effekten in Primitiven.

**Ziel:** Erstellen eines 3D-Objekts und Anwenden einer einfachen Vertexbeleuchtung und -färbung

## Voraussetzungen


Es wird davon ausgegangen, dass Sie mit C+ vertraut sind. Sie müssen außerdem mit den grundlegenden Konzepten der Grafikprogrammierung vertraut sein.

Des Weiteren gehen wir davon aus, dass Sie die Themen [Schnellstart: Einrichten von DirectX-Ressourcen und Anzeigen von Bildern](setting-up-directx-resources.md) sowie [Erstellen von Shadern und Zeichnen von Primitiven](creating-shaders-and-drawing-primitives.md) bearbeitet haben.

**Zeitaufwand:** 20 Minuten.

Anweisungen
------------

### 1. Definieren von Würfelvariablen

Zunächst müssen wir die **SimpleCubeVertex**-Struktur und die **ConstantBuffer**-Struktur für den Würfel definieren. In diesen Strukturen werden die Vertexpositionen und -farben für den Würfel sowie die Darstellung des Würfels angegeben. Wir deklarieren [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) und [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351) mit [**ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) und eine Instanz von **ConstantBuffer**.

```cpp
struct SimpleCubeVertex
{
    DirectX::XMFLOAT3 pos;   // Position
    DirectX::XMFLOAT3 color; // Color
};

struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};

// This class defines the application as a whole.
ref class Direct3DTutorialFrameworkView : public IFrameworkView
{
private:
    Platform::Agile<CoreWindow> m_window;
    ComPtr<IDXGISwapChain1> m_swapChain;
    ComPtr<ID3D11Device1> m_d3dDevice;
    ComPtr<ID3D11DeviceContext1> m_d3dDeviceContext;
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    ComPtr<ID3D11DepthStencilView> m_depthStencilView;
    ComPtr<ID3D11Buffer> m_constantBuffer;
    ConstantBuffer m_constantBufferData;
```

### 2. Erstellen einer Tiefenschablonenansicht

Zusätzlich zu der Renderzielansicht erstellen wir eine Tiefenschablonenansicht. Die Tiefenschablonenansicht ermöglicht Direct3D das effiziente Rendern von sich näher an der Kamera befindlichen Objekten vor Objekten, die sich weiter weg von der Kamera befinden. Bevor wir eine Ansicht für einen Tiefenschablonenpuffer erstellen können, müssen wir den Tiefenschablonenpuffer erstellen. Dazu füllen wir eine [**D3D11\_TEXTURE2D\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476253) zur Beschreibung des Tiefenschablonenpuffers auf und rufen anschließend [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521) zum Erstellen des Tiefenschablonenpuffers auf. Zum Erstellen der Tiefenschablonenansicht füllen wir eine [**D3D11\_DEPTH\_STENCIL\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476112) zur Beschreibung der Tiefenschablonenansicht auf und übergeben diese Beschreibung zusammen mit dem Tiefenschablonenpuffer an [**ID3D11Device::CreateDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476507).

```cpp
        // Once the render target view is created, create a depth stencil view.  This
        // allows Direct3D to efficiently render objects closer to the camera in front
        // of objects further from the camera.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_TEXTURE2D_DESC depthStencilDesc;
        depthStencilDesc.Width = backBufferDesc.Width;
        depthStencilDesc.Height = backBufferDesc.Height;
        depthStencilDesc.MipLevels = 1;
        depthStencilDesc.ArraySize = 1;
        depthStencilDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
        depthStencilDesc.SampleDesc.Count = 1;
        depthStencilDesc.SampleDesc.Quality = 0;
        depthStencilDesc.Usage = D3D11_USAGE_DEFAULT;
        depthStencilDesc.BindFlags = D3D11_BIND_DEPTH_STENCIL;
        depthStencilDesc.CPUAccessFlags = 0;
        depthStencilDesc.MiscFlags = 0;
        ComPtr<ID3D11Texture2D> depthStencil;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateTexture2D(
                &depthStencilDesc,
                nullptr,
                &depthStencil
                )
            );

        D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
        depthStencilViewDesc.Format = depthStencilDesc.Format;
        depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
        depthStencilViewDesc.Flags = 0;
        depthStencilViewDesc.Texture2D.MipSlice = 0;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateDepthStencilView(
                depthStencil.Get(),
                &depthStencilViewDesc,
                &m_depthStencilView
                )
            );
```

### 3. Aktualisieren der Perspektive mit dem Fenster

Wir aktualisieren die Perspektivenprojektionsparameter für den Konstantenpuffer gemäß Fensterabmessungen. Wir fixieren die Parameter auf ein 70-Grad-Sichtfeld mit einem Tiefenbereich von 0,01 bis 100.

```cpp
        // Finally, update the constant buffer perspective projection parameters
        // to account for the size of the application window.  In this sample,
        // the parameters are fixed to a 70-degree field of view, with a depth
        // range of 0.01 to 100.  For a generalized camera class, see Lesson 5.

        float xScale = 1.42814801f;
        float yScale = 1.42814801f;
        if (backBufferDesc.Width > backBufferDesc.Height)
        {
            xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
        }
        else
        {
            yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
        }

        m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

### 4. Erstellen von Vertex- und Pixelshadern mit Farbelementen

In dieser App erstellen wir Vertex- und Pixelshader, die im Vergleich zu der Beschreibung im vorherigen Lernprogramm [Erstellen von Shadern und Zeichnen von Primitiven](creating-shaders-and-drawing-primitives.md) einen höheren Komplexitätsgrad aufweisen. Der Vertexshader der App transformiert die Position der einzelnen Vertizes in den Projektionsbereich und übergibt die Vertexfarbe an den Pixelshader.

Das App-Array der [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180)-Strukturen zur Beschreibung des Layouts des Vertexshadercodes enthält zwei Layoutelemente: Ein Element definiert die Vertexposition, und das andere Element definiert die Farbe.

Wir erstellen Vertex-, Index- und Konstantenpuffer, um einen kreisenden Würfel zu definieren.

**So definieren Sie einen kreisenden Würfel**

1.  Zunächst definieren wir den Würfel. Wir weisen jedem Vertex neben einer Position auch eine Farbe zu. Dies ermöglicht dem Pixelshader eine unterschiedliche Farbgebung der einzelnen Flächen, sodass sie unterschieden werden können.
2.  Als Nächstes beschreiben wir mithilfe der Würfeldefinition die Vertex- und Indexpuffer ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) und [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)). Wir rufen [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) einmal für jeden Puffer auf.
3.  Nun erstellen wir einen Konstantenpuffer ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)) zum Übergeben der Modell-, Ansichts- und Projektionsmatrizen an den Vertexshader. Später können wir den Konstantenpuffer zum Drehen des Würfels und zum Anwenden einer Perspektivenprojektion auf den Würfel verwenden. Wir rufen [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) auf, um den Konstantenpuffer zu erstellen.
4.  Als Nächstes geben wir die Ansichtstransformation an, die einer Kameraposition mit X = 0, Y = 1, Z = 2 entspricht.
5.  Zum Schluss deklarieren wir eine *degree*-Variable, mit deren Hilfe wir den Würfel animieren werden, indem er in jedem Frame gedreht wird.

```cpp
        
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        
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
          // For this lesson, this is simply a DirectX::XMFLOAT3 vector defining the vertex position, and
          // a DirectX::XMFLOAT3 vector defining the vertex color.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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
        
        
        // Create vertex and index buffers that define a simple unit cube.
        auto createCubeTask = (createPSTask && createVSTask).then([this] () {

          // In the array below, which will be used to initialize the cube vertex buffers,
          // each vertex is assigned a color in addition to a position.  This will allow
          // the pixel shader to color each face differently, enabling them to be distinguished.
          SimpleCubeVertex cubeVertices[] =
          {
              { float3(-0.5f, 0.5f, -0.5f), float3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
              { float3( 0.5f, 0.5f, -0.5f), float3(1.0f, 1.0f, 0.0f) },
              { float3( 0.5f, 0.5f,  0.5f), float3(1.0f, 1.0f, 1.0f) },
              { float3(-0.5f, 0.5f,  0.5f), float3(0.0f, 1.0f, 1.0f) },

              { float3(-0.5f, -0.5f,  0.5f), float3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
              { float3( 0.5f, -0.5f,  0.5f), float3(1.0f, 0.0f, 1.0f) },
              { float3( 0.5f, -0.5f, -0.5f), float3(1.0f, 0.0f, 0.0f) },
              { float3(-0.5f, -0.5f, -0.5f), float3(0.0f, 0.0f, 0.0f) },
          };

          unsigned short cubeIndices[] =
          {
              0, 1, 2,
              0, 2, 3,

              4, 5, 6,
              4, 6, 7,

              3, 2, 5,
              3, 5, 4,

              2, 1, 6,
              2, 6, 5,

              1, 7, 6,
              1, 0, 7,

              0, 3, 4,
              0, 4, 7
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = cubeVertices;
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
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(cubeIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = cubeIndices;
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


          // Create a constant buffer for passing model, view, and projection matrices
          // to the vertex shader.  This will allow us to rotate the cube and apply
          // a perspective projection to it.

          D3D11_BUFFER_DESC constantBufferDesc = {0};
          constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
          constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
          constantBufferDesc.CPUAccessFlags = 0;
          constantBufferDesc.MiscFlags = 0;
          constantBufferDesc.StructureByteStride = 0;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &constantBufferDesc,
                  nullptr,
                  &m_constantBuffer
                  )
              );

          // Specify the view transform corresponding to a camera position of
          // X = 0, Y = 1, Z = 2.  For a generalized camera class, see Lesson 5.

          m_constantBufferData.view = DirectX::XMFLOAT4X4(
              -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
               0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
               0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
               0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f
              );

        });
        
        // This value will be used to animate the cube by rotating it every frame.
        float degree = 0.0f;
        
```

### 5. Drehen und Zeichnen des Würfels und Darstellen des gerenderten Bilds

Wir rufen eine Endlosschleife auf, um die Szene fortlaufend zu rendern und anzuzeigen. Wir rufen die **rotationY**-Inlinefunktion (BasicMath.h) mit einer Rotationsmenge auf, um die Werte festzulegen, mit deren Hilfe sich die Modellmatrix des Würfels um die Y-Achse dreht. Anschließend rufen wir [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) auf, um den Konstantenpuffer zu aktualisieren und das Würfelmodell zu drehen. Wir rufen [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) auf, um das Renderziel aus Ausgabeziel anzugeben. In diesem **OMSetRenderTargets**-Aufruf übergeben wir die Tiefenschablonenansicht. Wir rufen [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) auf, um das Renderziel in eine blaue Volltonfarbe zu bereinigen. Anschließend rufen wir [**ID3D11DeviceContext::ClearDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476387) auf, um den Tiefenpuffer zu leeren.

In der Endlosschleife zeichnen wir den Würfel ebenfalls auf der blauen Oberfläche.

**So zeichnen Sie den Würfel**

1.  Wir rufen zunächst [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) auf, um das Streamen der Vertexpufferdaten in den Eingabeassemblerzustand zu beschreiben.
2.  Als Nächstes rufen wir [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) und [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) auf, um die Vertex- und Indexpuffer an den Eingabeassemblerzustand zu binden.
3.  Nun rufen wir [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) mit dem [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP)-Wert auf, um für den Eingabeassemblerzustand anzugeben, dass die Vertexdaten als Dreieckskette interpretiert werden.
4.  Im nächsten Schritt rufen wir [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) auf, um die Vertexshaderphase mit dem Vertexshadercode zu initialisieren. Zudem rufen wir [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) auf, um die Pixelshaderphase mit dem Pixelshadercode zu initialisieren.
5.  Dann rufen wir [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) auf, um den im Vertexshader-Pipelinezustand verwendeten Konstantenpuffer festzulegen.
6.  Schließlich rufen wir [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) auf, um den Würfel zu zeichnen und ihn an die Renderpipeline zu übermitteln.

Wir rufen [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) auf, um das gerenderte Bild im Fenster darzustellen.

```cpp
            // Update the constant buffer to rotate the cube model.
            m_constantBufferData.model = XMMatrixRotationY(-degree);
            degree += 1.0f;

            m_d3dDeviceContext->UpdateSubresource(
                m_constantBuffer.Get(),
                0,
                nullptr,
                &m_constantBufferData,
                0,
                0
                );

            // Specify the render target and depth stencil we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                m_depthStencilView.Get()
                );

            // Clear the render target to a solid color, and reset the depth stencil.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->ClearDepthStencilView(
                m_depthStencilView.Get(),
                D3D11_CLEAR_DEPTH,
                1.0f,
                0
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(SimpleCubeVertex);
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

            m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf()
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(cubeIndices),
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


Wir haben Tiefen, Perspektiven, Farben und andere Effekte in Primitiven verwendet.

Als Nächstes wenden wir Texturen auf Primitive an.

[Anwenden von Texturen auf Primitive](applying-textures-to-primitives.md)

 

 







<!--HONumber=Jun16_HO4-->


