---
author: mtoepke
title: 將陰影圖轉譯為深度緩衝區
description: 從光線的視角轉譯，以建立代表陰影體的二維深度圖。
ms.assetid: 7f3d0208-c379-8871-cc48-027047c6c2d0
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, rendering, shadow map, depth buffer, direct3d, 遊戲, 轉譯, 陰影圖, 深度緩衝區
ms.localizationpriority: medium
ms.openlocfilehash: a73754fef6d87505751460ec134d853c6bca0530
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7287832"
---
# <a name="render-the-shadow-map-to-the-depth-buffer"></a>將陰影圖轉譯為深度緩衝區




從光線的視角轉譯，以建立代表陰影體的二維深度圖。 深度圖會為要在陰影中轉譯的空間設定遮罩。 [逐步解說：使用 Direct3D 11 中的深度緩衝區實作陰影體](implementing-depth-buffers-for-shadow-mapping.md)的第二部分。

## <a name="clear-the-depth-buffer"></a>清除深度緩衝區


在轉譯為深度緩衝區之前，一律先行清除該緩衝區。

```cpp
context->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), DirectX::Colors::CornflowerBlue);
context->ClearDepthStencilView(m_shadowDepthView.Get(), D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
```

## <a name="render-the-shadow-map-to-the-depth-buffer"></a>將陰影圖轉譯為深度緩衝區


針對陰影轉譯階段，請指定深度緩衝區，但是不要指定轉譯目標。

指定光線檢視區、頂點著色器，並設定光線空間常數緩衝區。 針對這個階段使用正面剔除，以便將陰影緩衝區中的深度值最佳化。

請注意，在大部分的裝置上，您可以為像素著色器指定 nullptr (或者完全略過指定像素著色器的部分)。 但是，部分驅動程式可能會在您使用設為 null 的像素著色器，於 Direct3D 裝置上呼叫繪製時擲回例外狀況。 若要避免發生這個例外狀況，您可以為陰影轉譯階段設定最低像素著色器。 這個著色器的輸出會捨棄；它可以在每個像素上呼叫 [**discard**](https://msdn.microsoft.com/library/windows/desktop/bb943995)。

轉譯可以轉換陰影的物件，但是不必轉譯無法轉換陰影的幾何圖形 (像是房間中的地板，或者基於最佳化因素而從陰影階段中移除的物件)。

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

**最佳化檢視範圍：**  確定您的實作會運算緊密的檢視範圍，讓您可以從深度緩衝區中取得最佳的精確度。 如需更多關於陰影技術的提示，請參閱[改善陰影深度圖的常見技術](https://msdn.microsoft.com/library/windows/desktop/ee416324)。

## <a name="vertex-shader-for-shadow-pass"></a>適用於陰影階段的頂點著色器


使用簡化的頂點著色器版本，只轉譯光線空間中的頂點位置。 請勿包含任何光源法向量、次要轉換等。

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

在這個逐步解說的下一個部分，您將了解如何透過[使用深度測試進行轉譯](render-the-scene-with-depth-testing.md)來新增陰影。

 

 




