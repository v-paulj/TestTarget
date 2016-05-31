---
author: mtoepke
title: Rendern der Schattenmap zum Tiefenpuffer
description: Führen Sie das Rendern aus dem Blickwinkel durch, aus dem das Licht kommt, um eine zweidimensionale Tiefenkarte zu erstellen, mit der das Schattenvolumen dargestellt wird.
ms.assetid: 7f3d0208-c379-8871-cc48-027047c6c2d0
---

# Rendern der Schattenmap für den Tiefenpuffer


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Führen Sie das Rendern aus dem Blickwinkel durch, aus dem das Licht kommt, um eine zweidimensionale Tiefenkarte zu erstellen, mit der das Schattenvolumen dargestellt wird. Mithilfe der Tiefenmap wird eine Maske für den Raum erstellt, der im Schatten gerendert wird. Teil 2 von [Exemplarische Vorgehensweise: Implementieren von Schattenvolumes mithilfe von Tiefenpuffern in Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## Löschen des Tiefenpuffers


Löschen Sie den Tiefenpuffer stets, bevor Sie in den Puffer rendern.

```cpp
context->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), DirectX::Colors::CornflowerBlue);
context->ClearDepthStencilView(m_shadowDepthView.Get(), D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
```

## Rendern der Schattenmap zum Tiefenpuffer


Geben Sie für den Schattenrenderdurchlauf einen Tiefenpuffer an, aber geben Sie kein Renderziel an.

Geben Sie den Viewport für das Licht und einen Vertex-Shader an, und legen Sie die Konstantenpuffer für den Lichtraum fest. Verwenden Sie für diesen Durchlauf das Vorderseiten-Culling, um die in den Schattenpuffer eingefügten Tiefenwerte zu optimieren.

Beachten Sie, dass Sie auf den meisten Geräten "nullptr" für den Pixelshader angeben können (oder die Angabe eines Pixelshaders ganz weglassen können). Für einige Treiber wird jedoch möglicherweise eine Ausnahme ausgelöst, wenn Sie auf dem Direct3D-Gerät einen Draw-Aufruf durchführen, während ein Null-Pixelshader festgelegt ist. Sie können einen minimalen Pixel-Shader für den Schattenrenderdurchlauf festlegen, um diese Ausnahme zu vermeiden. Die Ausgabe dieses Shaders wird nicht aufbewahrt. Für jedes Pixel kann [**discard**](https://msdn.microsoft.com/library/windows/desktop/bb943995) aufgerufen werden.

Rendern Sie die Objekte, die Schatten werfen können. Um Geometrieelemente, die keinen Schatten werfen können, brauchen Sie sich jedoch nicht zu kümmern (z. B. ein Boden in einem Raum oder Objekte, die zum Zweck der Optimierung aus dem Schattendurchlauf entfernt werden).

```cpp
void ShadowSceneRenderer::RenderShadowMap()
{
    auto context = m_deviceResources->GetD3DDeviceContext();

    // Render all the objects in the scene that can cast shadows onto themselves or onto other objects.

    // Only bind the ID3D11DepthStencilView for output.
    context->OMSetRenderTargets(
        0,
        nullptr,
        m_shadowDepthView.Get()
        );

    // Note that starting with the second frame, the previous call will display
    // warnings in VS debug output about forcing an unbind of the pixel shader
    // resource. This warning can be safely ignored when using shadow buffers
    // as demonstrated in this sample.

    // Set rendering state.
    context->RSSetState(m_shadowRenderState.Get());
    context->RSSetViewports(1, &m_shadowViewport);

    // Each vertex is one instance of the VertexPositionTexNormColor struct.
    UINT stride = sizeof(VertexPositionTexNormColor);
    UINT offset = 0;
    context->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset
        );

    context->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT, // Each index is one 16-bit unsigned integer (short).
        0
        );

    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->IASetInputLayout(m_inputLayout.Get());

    // Attach our vertex shader.
    context->VSSetShader(
        m_simpleVertexShader.Get(),
        nullptr,
        0
        );

    // Send the constant buffers to the Graphics device.
    context->VSSetConstantBuffers(
        0,
        1,
        m_lightViewProjectionBuffer.GetAddressOf()
        );

    context->VSSetConstantBuffers(
        1,
        1,
        m_rotatedModelBuffer.GetAddressOf()
        );

    // In some configurations, it's possible to avoid setting a pixel shader
    // (or set PS to nullptr). Not all drivers are tolerant of this, so to be
    // safe set a minimal shader here.
    //
    // Direct3D will discard output from this shader because the render target
    // view is unbound.
    context->PSSetShader(
        m_textureShader.Get(),
        nullptr,
        0
        );

    // Draw the objects.
    context->DrawIndexed(
        m_indexCountCube,
        0,
        0
        );
}
```

**Optimieren des Frustums:**  Stellen Sie sicher, dass in Ihrer Implementierung ein exaktes Frustum berechnet wird, damit Sie mit dem Tiefenpuffer die größtmögliche Präzision erzielen. Weitere Tipps zu Schattenmethoden finden Sie unter [Häufig verwendete Methoden zur Verbesserung von Tiefenmaps für Schatten](https://msdn.microsoft.com/library/windows/desktop/ee416324).

## Vertex-Shader für Schattendurchlauf


Verwenden Sie eine vereinfachte Version Ihres Vertex-Shaders, um nur die Vertexposition im Lichtraum zu rendern. Beziehen Sie keine Beleuchtungsnormalen, sekundären Transformationen usw. ein.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    return output;
}
```

Im nächsten Teil dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie Schatten hinzufügen, indem Sie [mithilfe des Tiefentests rendern](render-the-scene-with-depth-testing.md).

 

 






<!--HONumber=May16_HO2-->


