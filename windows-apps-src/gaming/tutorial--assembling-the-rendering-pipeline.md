---
title: 轉譯簡介
description: 瞭解如何開發呈現管線以顯示圖形。 轉譯簡介。
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 轉譯
ms.localizationpriority: medium
ms.openlocfilehash: d1abb324c5e9e16babbbf8d3650adc39cb995137
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409637"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>轉譯架構 I：轉譯簡介

> [!NOTE]
> 本主題是使用 DirectX 教學課程系列[建立簡單的通用 Windows 平臺（UWP）遊戲](tutorial--create-your-first-uwp-directx-game.md)的一部分。 該連結的主題會設定數列的內容。

到目前為止，我們已經討論過如何建立通用 Windows 平臺（UWP）遊戲的結構，以及如何定義狀態機器來處理遊戲的流程。 現在正是學習如何開發轉譯架構的時候了。 我們來看一下範例遊戲如何使用 Direct3D 11 來呈現遊戲場景。

Direct3D 11 包含一組 Api，可讓您存取高效能圖形硬體的先進功能，以用來為需要大量圖形的應用程式（例如遊戲）建立3D 圖形。

在螢幕上轉譯遊戲圖形表示基本上會在螢幕上呈現一系列畫面格。 每個畫面中，您必須根據檢視轉譯螢幕中可看到的物件。

為了轉譯畫面，您必須將所需的場景資訊傳遞到硬體，讓它可以顯示在螢幕上。 如果您希望在螢幕上顯示任何項目，則需要在遊戲開始執行時立即開始轉譯。

## <a name="objectives"></a>目標

設定基本轉譯架構，以顯示 UWP DirectX 遊戲的圖形輸出。 您可以將它鬆散細分成這三個步驟。

1. 建立圖形介面的連接。
2. 建立繪製圖形所需的資源。
3. 藉由呈現畫面格來顯示圖形。

本主題說明如何呈現圖形，涵蓋步驟1和3。

轉譯[架構 II：遊戲](tutorial-game-rendering.md)轉譯涵蓋步驟 2 &mdash; 如何設定轉譯架構，以及如何在可能發生轉譯之前準備資料。

## <a name="get-started"></a>開始使用

讓自己熟悉基本的圖形和轉譯概念是個不錯的主意。 如果您不熟悉 Direct3D 和轉譯，請參閱[術語和概念](#terms-and-concepts)，以取得本主題所使用之圖形和轉譯詞彙的簡短描述。

針對此遊戲， **GameRenderer**類別代表此範例遊戲的轉譯器。 其負責建立及維護所有用於產生遊戲視覺效果之 Direct3D 11 和 Direct2D 物件。 它也會維護用來抓取要轉譯之物件清單的**Simple3DGame**物件的參考，以及用於列印頭顯示（抬頭顯示器）遊戲的狀態。

教學課程的此部分中，我們會著重在轉譯遊戲中的 3D 物件。

## <a name="establish-a-connection-to-the-graphics-interface"></a>建立連接到此圖形介面

如需存取硬體以進行轉譯的詳細資訊，請參閱[定義遊戲的 UWP 應用程式架構](tutorial--building-the-games-uwp-app-framework.md#the-appinitialize-method)主題。

### <a name="the-appinitialize-method"></a>App：： Initialize 方法

**Std：： make_shared**函式（如下所示）用來建立**shared_ptr**至**DX：:D eviceresources**，這也會提供裝置的存取權。

Direct3D 11 中，[裝置](#device)用於配置與摧毀物件、轉譯基本類型，並透過圖形驅動程式與圖形卡通訊。

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    ...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>轉譯畫面來顯示圖形

啟動遊戲時，遊戲場景需要轉譯。 在[**GameMain：： Run**](#gamemainrun-method)方法中開始轉譯的指示，如下所示。

簡單的流程就是這樣。

1. **更新**
2. **轉譯**
3. **目前**

### <a name="gamemainrun-method"></a>GameMain::Run 方法

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            switch (m_updateState)
            {
            ...
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

### <a name="update"></a>更新

如需有關如何在[**GameMain：： Update**](tutorial-game-flow-management.md#the-gamemainupdate-method)方法中更新遊戲狀態的詳細資訊，請參閱[遊戲流程管理](tutorial-game-flow-management.md)主題。

### <a name="render"></a>轉譯

轉譯的執行方式是從**GameMain：： Run**呼叫[**GameRenderer：： Render**](#gamerendererrender-method)方法。

如果已啟用[身歷聲](#stereo-rendering)轉譯，則會有兩個轉譯傳遞 &mdash; 一個用於左眼，另一個用於右邊。 在每個轉譯階段中，我們將轉譯目標與 深度樣板檢視 繫結至裝置。 我們稍後也會說明深度樣板檢視。

> [!NOTE]
> 可以使用其他方法，例如使用端點執行個體或幾何著色器的一階段，來達成立體著色運算。 兩個轉譯傳遞方法是較慢但更方便的方式來達到身歷聲轉譯。

一旦遊戲執行，並載入資源之後，我們會針對每個轉譯階段更新[投影矩陣](#projection-transform-matrix)一次。 物件與每個檢視有些許不同。 接下來，我們會設定[圖形轉譯管線](#rendering-pipeline)。 

> [!NOTE]
> 請參閱[建立與載入 DirectX 圖形資源](tutorial-game-rendering.md#create-and-load-directx-graphic-resources)，獲得如何載入資源的詳細資訊。

在這個範例遊戲中，轉譯器是設計成在所有物件上使用標準的頂點配置。 這會簡化著色器設計，並允許在著色器之間輕鬆變更，與物件的幾何無關。

#### <a name="gamerendererrender-method"></a>GameRenderer::Render 方法

我們會將 Direct3D 內容設定為使用輸入頂點版面配置。 輸入配置物件描述如何將頂點緩衝區資料傳送到[轉譯管線](#rendering-pipeline)。 

接下來，我們將 Direct3D 內容設定為使用稍早定義的常數緩衝區，這是由[頂點著色器](#vertex-shaders-and-pixel-shaders)管線階段和[圖元著色器](#vertex-shaders-and-pixel-shaders)管線階段所使用。 

> [!NOTE]
> 請參閱[轉譯架構 II：遊戲轉譯](tutorial-game-rendering.md)獲得有關常數緩衝區定義的詳細資訊。

因為相同輸入配置與常數緩衝區設定適用於在管線裡的所有著色器，所以每個畫面設定一次。

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled.
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets

            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

            // Clears the depth stencil view.
            // A depth stencil view contains the format and buffer to hold depth and stencil info.
            // For more info about depth stencil view, go to: 
            // https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-stencil-view--dsv-
            // A depth buffer is used to store depth information to control which areas of 
            // polygons are rendered rather than hidden from view. To learn more about a depth buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-buffers
            // A stencil buffer is used to mask pixels in an image, to produce special effects. 
            // The mask determines whether a pixel is drawn or not,
            // by setting the bit to a 1 or 0. To learn more about a stencil buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/stencil-buffers

            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // Direct2D -- discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() };

            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap());
        }

        const float clearColor[4] = { 0.5f, 0.5f, 0.8f, 1.0f };

        // Only need to clear the background when not rendering the full 3D scene since
        // the 3D world is a fully enclosed box and the dynamics prevents the camera from
        // moving outside this space.
        if (i > 0)
        {
            // Doing the Right Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), clearColor);
        }
        else
        {
            // Doing the Mono or Left Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), clearColor);
        }

        // Render the scene objects
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (stereoEnabled)
            {
                // When doing stereo, it is necessary to update the projection matrix once per rendering pass.

                auto orientation = m_deviceResources->GetOrientationTransform3D();

                ConstantBufferChangeOnResize changesOnResize;
                // Apply either a left or right eye projection, which is an offset from the middle
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera().LeftEyeProjection() :
                            m_game->GameCamera().RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrameValue;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrameValue.view,
                XMMatrixTranspose(m_game->GameCamera().View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrameValue,
                0,
                0
                );

            // Set up the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, see
            // https://docs.microsoft.com/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout
            d3dContext->IASetInputLayout(m_vertexLayout.get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers

            ID3D11Buffer* constantBufferNeverChanges{ m_constantBufferNeverChanges.get() };
            d3dContext->VSSetConstantBuffers(0, 1, &constantBufferNeverChanges);
            ID3D11Buffer* constantBufferChangeOnResize{ m_constantBufferChangeOnResize.get() };
            d3dContext->VSSetConstantBuffers(1, 1, &constantBufferChangeOnResize);
            ID3D11Buffer* constantBufferChangesEveryFrame{ m_constantBufferChangesEveryFrame.get() };
            d3dContext->VSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            ID3D11Buffer* constantBufferChangesEveryPrim{ m_constantBufferChangesEveryPrim.get() };
            d3dContext->VSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers

            d3dContext->PSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            d3dContext->PSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);
            ID3D11SamplerState* samplerLinear{ m_samplerLinear.get() };
            d3dContext->PSSetSamplers(0, 1, &samplerLinear);

            for (auto&& object : m_game->RenderObjects())
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        // Start of 2D rendering
        ...
    }
}
```

### <a name="primitive-rendering"></a>基本類型轉譯

轉譯場景時，您逐一迴圈的所有物件，需經過轉譯。 每個物件 (基本類型) 皆重複以下的步驟。

- 使用模型的「[世界轉換矩陣](#world-transform-matrix)」和「材質」資訊來更新常數緩衝區（**m_constantBufferChangesEveryPrim**）。
- **M_constantBufferChangesEveryPrim**包含每個物件的參數。 其中包含物件對世界的轉換矩陣，以及材質屬性（例如光源計算的色彩和反射指數）。
- 將 Direct3D 內容設定為使用網格物件資料的輸入頂點配置，以串流至轉譯[管線](#rendering-pipeline)的輸入組合器（IA）階段。
- 將 Direct3D 內容設定為使用 IA 階段中的[索引緩衝區](#index-buffer)。 提供基本類型資訊：類型、資料訂單。
- 送出繪製呼叫，繪製已編製索引的非執行個體基本類型。 The **GameObject::Render** 方法使用指定基本類型的特定資料來更新基本類型 [常數緩衝區](#constant-buffer-or-shader-constant-buffer)。 這會讓內容的 **DrawIndexed** 呼叫，繪製每個基本類型的幾何。 具體來說，這個 Draw 呼叫會將命令和資料排到圖形處理單元 GPU 的佇列，由常數緩衝區資料進行參數化處理。 每個繪圖呼叫會在每個頂點上執行頂點著色器 一次，然後[像素著色器](#vertex-shaders-and-pixel-shaders) 會在基本類型中每個三角形的每個像素中執行一次。 紋理是像素著色器用來進行轉譯之狀態的一部分。

以下是使用多個常數緩衝區的原因。

- 遊戲會使用多個常數緩衝區，但它只需要每個基本的一次更新這些緩衝區。 如先前所述，常數緩衝區就像是為每個基本類型執行的著色器輸入。 有些資料是靜態的（**m_constantBufferNeverChanges**）;有些資料在框架（**m_constantBufferChangesEveryFrame**）上是固定的，例如相機的位置;有些資料是基本型別特有的，例如色彩和材質（**m_constantBufferChangesEveryPrim**）。
- 遊戲轉譯器將這些輸入分成不同的常數緩衝區，以最佳化 CPU 與 GPU 使用的記憶體頻寬。 這種方法也有助於將 GPU 追蹤所需的資料量減到最少。 GPU 有很大的命令佇列，而每次遊戲呼叫 **Draw** 時，該命令連同與其關聯的資料一起進入佇列。 當遊戲更新基本類型常數緩衝區並發出下一個 **Draw** 命令時，圖形驅動程式會將下一個命令及關聯的資料新增到佇列。 如果遊戲繪製 100 個基本類型，則佇列中可能會有 100 個常數緩衝區資料的複本。 若要將傳送到 GPU 的遊戲資料數量減到最少，遊戲會使用不同的基本類型常數緩衝區，其中僅包含每個基本類型的更新。

#### <a name="gameobjectrender-method"></a>GameObject::Render 方法

```cppwinrt
void GameObject::Render(
    _In_ ID3D11DeviceContext* context,
    _In_ ID3D11Buffer* primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    // Put the model matrix info into a constant buffer, in world matrix.
    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    // Check to see which material to use on the object.
    // If a collision (a hit) is detected, GameObject::Render checks the current context, which 
    // indicates whether the target has been hit by an ammo sphere. If the target has been hit, 
    // this method applies a hit material, which reverses the colors of the rings of the target to 
    // indicate a successful hit to the player. Otherwise, it applies the default material 
    // with the same method. In both cases, it sets the material by calling Material::RenderSetup, 
    // which sets the appropriate constants into the constant buffer. Then, it calls 
    // ID3D11DeviceContext::PSSetShaderResources to set the corresponding texture resource for the 
    // pixel shader, and ID3D11DeviceContext::VSSetShader and ID3D11DeviceContext::PSSetShader 
    // to set the vertex shader and pixel shader objects themselves, respectively.

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }

    // Update the primitive constant buffer with the object model's info.
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    // Render the mesh.
    // See MeshObject::Render method below.
    m_mesh->Render(context);
}
```

#### <a name="meshobjectrender-method"></a>MeshObject：： Render 方法

```cppwinrt
void MeshObject::Render(_In_ ID3D11DeviceContext* context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32_t stride{ sizeof(PNTVertex) };
    uint32_t offset{ 0 };

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    ID3D11Buffer* vertexBuffer{ m_vertexBuffer.get() };
    context->IASetVertexBuffers(0, 1, &vertexBuffer, &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer.
    context->IASetIndexBuffer(m_indexBuffer.get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology.
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed.
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="deviceresourcespresent-method"></a>DeviceResources：:P 重發的方法

我們會呼叫**DeviceResources：:P 重發**的方法，以顯示我們在緩衝區中放置的內容。

我們使用「交換鏈結」一詞，針對用於為使用者顯示畫面的緩衝區集合。 每次應用程式提供新畫面供顯示時，交換鏈結中的第一個緩衝區會取代顯示之緩衝區的位置。 這個程序稱為交換或翻轉。 如需詳細資訊，請參閱[交換鏈結](../graphics-concepts/swap-chains.md)。

- **IDXGISwapChain1**介面的**現有**方法會指示[DXGI](#dxgi)封鎖，直到發生垂直同步處理（VSync）為止，讓應用程式進入睡眠狀態，直到下一個 VSync 為止。 這可確保您不會浪費任何迴圈，呈現永遠不會顯示在螢幕上的畫面。
- **ID3D11DeviceCoNtext3**介面的**DiscardView**方法會捨棄[呈現目標](#render-target)的內容。 只有在完全複寫現有的內容時，這才是有效的作業。 如果使用了 dirty 或 scroll 矩形，則應該移除此呼叫。
* 使用同一個**DiscardView**方法，捨棄[深度樣板](#depth-stencil)的內容。
* **HandleDeviceLost**方法是用來管理要移除之[裝置](#device)的案例。 如果裝置已被中斷連線或驅動程式升級而移除，則您必須重新建立所有裝置資源。 如需詳細資訊，請參閱[處理 Direct3D 11 中的裝置已移除案例](handling-device-lost-scenarios.md)。

> [!TIP]
> 若要達到平滑的畫面播放速率，您必須確定呈現畫面格的工作數量會符合 VSyncs 之間的時間。

```cppwinrt
// Present the contents of the swap chain to the screen.
void DX::DeviceResources::Present()
{
    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    HRESULT hr = m_swapChain->Present(1, 0);

    // Discard the contents of the render target.
    // This is a valid operation only when the existing contents will be entirely
    // overwritten. If dirty or scroll rects are used, this call should be removed.
    m_d3dContext->DiscardView(m_d3dRenderTargetView.get());

    // Discard the contents of the depth stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        winrt::check_hresult(hr);
    }
}
```

## <a name="next-steps"></a>後續步驟

本主題說明如何在顯示上轉譯圖形，並提供一些使用的轉譯詞彙的簡短描述（如下所示）。 深入瞭解轉譯[架構 II：遊戲](tutorial-game-rendering.md)轉譯主題中的轉譯，並瞭解如何準備呈現前所需的資料。

## <a name="terms-and-concepts"></a>詞彙和概念

### <a name="simple-game-scene"></a>簡易遊戲場景

簡易的遊戲場景由幾個光源物件所組成。

透過一組 X、Y、Z 空間座標定義物件的圖形。 遊戲世界裡的真實轉譯位置可藉由套用位置 X、Y、Z 座標的轉換矩陣來判斷。 它也可以有一組材質座標 &mdash; 和 V，以 &mdash; 指定如何將材質套用至物件。 這會定義物件的表面屬性，並可讓您查看物件是否有粗糙表面（例如網球）或平滑的光澤表面（例如保齡球球）。

轉譯架構會使用場景和物件資訊，以框架的方式重新建立場景框架，使其在您的顯示監視器上處於作用中狀態。

### <a name="rendering-pipeline"></a>轉譯管線

呈現管線是將3D 場景資訊轉譯成螢幕上所顯示影像的程式。 Direct3D 11 中，這個管線是可程式化的。 您可以調整此階段，支援您的轉譯需求 搭配常見著色器核心的階段可使用 HLSL 程式語言進行程式設計。 它也稱為*圖形轉譯管線*，或只是*管線*。

為了協助您建立此管線，您必須熟悉這些詳細資料。

- [HLSL](#hlsl)。 建議您使用 HLSL 著色器模型 5.1 和更新版本用於 UWP DirectX 遊戲。
- [著色](#shaders)器。
- [頂點著色器和圖元著色](#vertex-shaders-and-pixel-shaders)器。
- [著色器階段](#shader-stages)。
- [各種著色器檔案格式](#various-shader-file-formats)。

如需詳細資訊，請參閱[了解 Direct3D 11 轉譯管線](/windows/desktop/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline)和[圖形管線](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline)。

#### <a name="hlsl"></a>HLSL

HLSL 是 DirectX 的高階著色器語言。 使用 HLSL，您可以建立適用于 Direct3D 管線的類似 C 的可程式化著色器。 如需詳細資訊，請參閱 [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)。

#### <a name="shaders"></a>著色器

您可以將著色器視為一組指示，以決定物件介面呈現時的顯示方式。 使用 HLSL 又稱為 HLSL 著色器來程式設計這些。 [HLSL]）（#hlsl）著色器的原始程式碼檔具有 `.hlsl` 副檔名。 這些著色器可以在組建階段或執行時間進行編譯，並在執行時間設定為適當的管線階段。 已編譯的著色器物件具有 `.cso` 副檔名。

Direct3D 9 著色器可以使用著色器模型1、著色器模型2和著色器模型3來設計;Direct3D 10 著色器只能在著色器模型4上設計。 可以在著色器模型 5 上設計 Direct3D 11 著色器。 可以在著色器模型 5.1 上設計 Direct3D 11.3 和 Direct3D 12，且也可以在著色器模型 6 上設計 Direct3D 12。

#### <a name="vertex-shaders-and-pixel-shaders"></a>頂點著色器和像素著色器

資料會輸入圖形管線做為基本類型的資料流程，並由各種著色器（例如頂點著色器和圖元著色器）處理。 

頂點著色器處理頂點，通常執行轉換、皮膚形變與光照等作業。 像素著色器可啟用豐富的陰影技術，例如個別像素光線和後處理。 其結合常數變數、紋理資料、插補每個頂點值和其他資料的程式，以產生每個像素的輸出。 

#### <a name="shader-stages"></a>著色器階段

定義一系列這些各式著色器處理這個在轉譯管線中稱為著色器階段的基本類型串流。 實際階段取決於 Direct3D 的版本，但通常會包含頂點、幾何及像素階段。 還有其他階段，例如鑲嵌的輪廓以及網域著色器，和計算著色器。 所有這些階段都是使用[HLSL](#hlsl)完全可程式化的。 如需詳細資訊，請參閱[圖形管線](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline)。

#### <a name="various-shader-file-formats"></a>各種不同的著色器檔案格式

以下是著色器程式碼副檔名。

- 副檔名為的檔案會 `.hlsl` 保存 [HLSL]）（#hlsl）原始程式碼。
- 副檔名為的檔案會 `.cso` 保存已編譯的著色器物件。
- 副檔名為的檔案 `.h` 是標頭檔，但在著色器程式碼內容中，此標頭檔會定義保存著色器資料的位元組陣列。
- 副檔名為的檔案 `.hlsli` 包含常數緩衝區的格式。 在範例遊戲中，檔案是**著色**器  >  **ConstantBuffers. hlsli**。

> [!NOTE]
> 您可以在 `.cso` 執行時間載入檔案，或 `.h` 在可執行檔程式碼中加入檔案，以內嵌著色器。 但您不能同時針對相同的著色器使用這兩者。

### <a name="deeper-understanding-of-directx"></a>深入了解 DirectX

Direct3D 11 是一組 Api，可協助我們建立圖形密集應用程式（例如遊戲）的圖形，我們想要有良好的圖形卡來處理密集的計算。 本章節簡短說明 Direct3D 11 圖形的程式設計蓋瑱：資源、子資源、裝置以及裝置操作。

#### <a name="resource"></a>資源

您可以將資源（也稱為裝置資源）視為如何呈現物件的相關資訊，例如材質、位置或色彩。 資源會將資料提供給管線，並定義在場景中呈現的內容。 資源可以從您的遊戲媒體載入，或在執行時間以動態方式建立。

事實上，資源是 Direct3D [管線](#rendering-pipeline)可以存取的記憶體中的區域。 為了讓管線有效地存取記憶體，提供給管線的資料 (例如，輸入幾何、著色器資源及紋理) 必須儲存在資源中。 所有 Direct3D 資源衍生兩種類型的資源︰緩衝區或紋理。 每個管線階段可使用多達 128 種資源。 如需詳細資訊，請參閱[資源](../graphics-concepts/resources.md)。

#### <a name="subresource"></a>子資源

子資源這個詞彙指的是資源的子集。 Direct3D 可以參考整個資源，或可參考資源的子集。 如需詳細資訊，請參閱[子資源](../graphics-concepts/resource-types.md#subresources)。

#### <a name="depth-stencil"></a>深度樣板

深度樣板資源包含格式及緩衝區，以保留深度和樣板資訊。 使用紋理資源建立它。 如需如何建立深度樣板資源的詳細資訊，請參閱[設定深度樣板功能](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-depth-stencil)。 我們透過使用 [ID3D11DepthStencilView](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview)介面實作深度樣板檢視來存取深度樣板資源。

[深度資訊] 告訴我們多邊形的哪些區域位於其他地方，讓我們能夠判斷哪些是隱藏的。 樣板資訊可告訴我們遮罩哪一個像素。 它可以用來製作特效，因為它會判斷是否繪製像素；設定 1 或 0 的位元。 

如需詳細資訊，請參閱[深度樣板視圖](../graphics-concepts/depth-stencil-view--dsv-.md)、[深度緩衝區](../graphics-concepts/depth-buffers.md)和樣板[緩衝區](../graphics-concepts/stencil-buffers.md)。

#### <a name="render-target"></a>轉譯目標

轉譯目標是我們可以在轉譯階段結尾撰寫的資源。 常使用[ID3D11Device::CreateRenderTargetView](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview)方法建立它，該方法使用交換鏈結背景緩衝區（這也是資源）做為輸入參數。 

每個轉譯目標也該有對應深度樣板檢視，因為在使用它之前，當我們使用[OMSetRenderTargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets)來設定轉譯目標時，它也需要深度樣板檢視。 我們透過使用[ID3D11RenderTargetView](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)介面實作轉譯目標來存取轉譯目標資源。 

#### <a name="device"></a>裝置

您可以想像裝置為配置和終結物件、轉譯基本專案，以及透過圖形驅動程式與圖形配接器通訊的方式。 

更精確的說明，Direct3D 裝置是 Direct3D 的轉譯元件。 裝置封裝並儲存呈現狀態、執行轉換照明作業，並將影像點陣化到表面。 如需詳細資訊，請參閱 [裝置](../graphics-concepts/devices.md)。

裝置是由[**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)介面表示。 換句話說， **ID3D11Device**介面代表虛擬顯示介面卡，用來建立裝置所擁有的資源。 

ID3D11Device 有不同的版本。 [**ID3D11Device5**](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11device5)是最新版本，並將新方法新增至**ID3D11Device4**中的。 如需 Direct3D 如何與基礎硬體通訊的詳細資訊，請參閱[Windows 裝置的驅動程式模型 (WDDM) 架構](/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture)。

每個應用程式至少必須有一個裝置;大部分的應用程式只會建立一個。 藉由呼叫**D3D11CreateDevice**或**D3D11CreateDeviceAndSwapChain** ，並使用**D3D_DRIVER_TYPE**旗標指定驅動程式類型，為安裝在電腦上的其中一個硬體驅動程式建立裝置。 每個裝置可以使用一或多個裝置內容，視所需的功能而定。 如需詳細資訊，請參閱 [D3D11CreateDevice 函式](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)。

#### <a name="device-context"></a>裝置內容

裝置內容是用來設定[管線](#rendering-pipeline)狀態，並使用[裝置](#device)所擁有的[資源](#resource)來產生轉譯命令。 

Direct3D 11 實作兩種類型的裝置內容，一種為立即轉譯，另一種為延遲轉譯；這兩個內容都可以使用[ID3D11DeviceContext](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)介面顯示。 

**ID3D11DeviceContext**介面有不同的版本；**ID3D11DeviceContext4**將新方法新增至**ID3D11DeviceContext3**中。

**ID3D11DeviceCoNtext4**是在 Windows 10 建立者更新中引進，而是**ID3D11DeviceCoNtext**介面的最新版本。 以 Windows 10 建立者更新（含）以後版本為目標的應用程式應該使用此介面，而不是舊版。 如需詳細資訊，請參閱 [ID3D11DeviceContext4](/windows/desktop/api/d3d11_3/nn-d3d11_3-id3d11devicecontext4)。

#### <a name="dxdeviceresources"></a>DX::DeviceResources

**DX：:D eviceresources**類別位於 DeviceResources 檔案中**DeviceResources.cpp** / **.h** ，並控制所有 DirectX 裝置資源。

### <a name="buffer"></a>Buffer

緩衝區資源是一個完整輸入的資料集合，由元素所組成。 您可以使用緩衝區存放各式各樣的資料，包含位置向量、法向向量、頂點緩衝區中的紋理座標、索引緩衝區中的索引、或裝置狀態。 Buffer 元素可以包含已壓縮的資料值（例如**R8G8B8A8**介面值）、單一8位整數或 4 32 位浮點值。

有三種可用的緩衝區類型： [頂點緩衝區]、[索引緩衝區] 和 [常數緩衝區]。

#### <a name="vertex-buffer"></a>頂點緩衝區

包含用來定義您的幾何頂點資料。 頂點資料包括位置座標、色彩資料、紋理座標資料、一般資料等等。 

#### <a name="index-buffer"></a>索引緩衝區

包含整數位移至頂點緩衝區，可用於更有效率地呈現基本類型。 索引緩衝區包含連續的 16 位元或 32 位元索引集，而每個索引用來識別頂點緩衝區中的頂點。

#### <a name="constant-buffer-or-shader-constant-buffer"></a>常數緩衝區或著色器-常數緩衝區

可讓您有效率地提供著色器資料至管線。 您可以使用常數緩衝區做為著色器的輸入，其執行每個基本類別，並儲存轉譯管線資料流輸出階段的結果。 概念上，常數緩衝區看起來很像單一元素頂點緩衝區。

#### <a name="design-and-implementation-of-buffers"></a>緩衝區的設計和實作

您可以根據資料類型來設計緩衝區，例如，在我們的範例遊戲中，會為靜態資料建立一個緩衝區，另一個則是針對在框架上固定的資料，而另一個則用於基本的特定資料。

所有緩衝區類型都藉由**ID3D11Buffer**介面進行封裝，且您可以藉由呼叫**ID3D11Device::CreateBuffer**建立緩衝區資源。 但必須將緩衝區繫結至管線才能存取。 緩衝區可以同時繫結到多個管線階段，以供讀取。 緩衝區也可以系結至單一管線階段進行寫入;不過，相同的緩衝區無法同時進行讀取和寫入。

您可以利用這些方式來系結緩衝區。

- 藉由呼叫**ID3D11DeviceCoNtext**方法（例如**ID3D11DeviceCoNtext：： IASetVertexBuffers**和**ID3D11DeviceCoNtext：： IASetIndexBuffer**）到輸入組譯工具階段。
- 藉由呼叫**ID3D11DeviceCoNtext：： SOSetTargets**，對資料流程輸出階段進行。
- 藉由呼叫著色器方法（例如**ID3D11DeviceCoNtext：： VSSetConstantBuffers**）至著色器階段。

如需詳細資訊，請參閱 [Direct3D 11 中的緩衝區簡介](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)。

### <a name="dxgi"></a>DXGI

Microsoft DirectX 圖形基礎結構（DXGI）是一個子系統，其封裝了 Direct3D 所需的一些低層級工作。 在多執行緒應用程式中使用 DXGI 時必須特別小心，以確保不會發生鎖死。 如需詳細資訊，請參閱多[執行緒和 DXGI](/windows/win32/direct3darticles/dxgi-best-practices#multithreading-and-dxgi)

### <a name="feature-level"></a>功能層級

功能層級是在 Direct3D 11 中導入的概念，以處理新的和現有的電腦中各式各樣的視訊卡。 功能層級是一組妥善定義的圖形處理單元（GPU）功能。 

每個視訊卡實作一個特定層級的 DirectX 功能，依照所安裝的 GPU 而定。 在 Microsoft Direct3D 舊版中，可以找出視訊卡實作的 Direct3D 版本，然後隨之程式設計您的應用程式。 

使用功能層級，當您建立裝置，您可以嘗試建立您所要求的功能層級的裝置。 如果裝置建立運作，該功能層級則存在，否則，硬體不支援該功能層級。 您可以嘗試在較低的功能層級重新建立裝置，也可以選擇結束應用程式。 例如，12_0 功能層級需要 Direct3D 11.3 或 Direct3D 12，以及著色器模型5.1。 如需詳細資訊，請參閱[Direct3D 功能層級：每項功能層級的概觀](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)。

使用功能層級，您可以開發 Direct3D 9、Microsoft Direct3D 10 或 Direct3D 11 的應用程式，並且在 9、10 或 11 硬體（有一些例外）上執行。 如需詳細資訊，請參閱[Direct3D 功能層級](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)。

### <a name="stereo-rendering"></a>立體著色運算

立體著色運算用來提升深度的視覺效果。 它使用兩個圖像，一個從左眼，另一個從右眼，在螢幕上顯示場景。 

我們從數學觀點來套用立體著色運算矩陣，也就是稍微水平位移至右側和左邊，以規則單投影矩陣來達成。

我們進行了兩個轉譯階段，以在此範例遊戲中達到身歷聲轉譯。

- 繫結至右邊轉譯目標，適用於右側投影，然後繪製的基本類型物件。
- 繫結至左邊轉譯目標，適用於左側投影，然後繪製的基本類型物件。

### <a name="camera-and-coordinate-space"></a>相機和座標空間

遊戲有現成的程式碼可以用自己的座標系統來更新世界 (有時候稱為世界空間或場景空間)。 所有物件 (包括相機) 都在這個空間設定放置及方向。 如需詳細資訊，請參閱[座標系統](../graphics-concepts/coordinate-systems.md).

頂點著色器則負責使用下列演算法 (其中 V 是向量而 M 是矩陣)，將模型座標轉換為裝置座標的繁重工作。

`V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)`

- `M(model-to-world)`這是模型座標到全局座標的轉換矩陣，也稱為「[世界轉換矩陣](#world-transform-matrix)」。 這是由基本類型提供。
- `M(world-to-view)`這是全局座標的轉換矩陣，用於視圖座標，也稱為「[視圖轉換矩陣](#view-transform-matrix)」。
  - 這是由相機的視圖矩陣提供的。 它是由相機的位置定義，並包含外觀向量（[*外觀*] 是從相機直接指向場景的向量，以及與它垂直的「*查閱*」向量）。
  - 在範例遊戲中， **m_viewMatrix**是「視圖」轉換矩陣，並使用**攝影機：： SetViewParams**來計算。
- `M(view-to-device)`這是 view 座標到裝置座標的轉換矩陣，也稱為[投射轉換矩陣](#projection-transform-matrix)。
  - 這是由相機的投影提供的。 它會提供在最後場景中實際顯示該空間大小的相關資訊。 「視圖」（FoV）、「外觀比例」和「裁剪」平面的欄位會定義投射轉換矩陣。
  - 在範例遊戲中， **m_projectionMatrix**定義轉換成投影座標，使用**攝影機：： SetProjParams**來計算（針對身歷聲投影，您會使用兩個投射矩陣，分別 &mdash; 用於每個眼睛的觀點）。

中的著色器 `VertexShader.hlsl` 程式碼會從常數緩衝區載入這些向量和矩陣，並針對每個頂點執行此轉換。

### <a name="coordinate-transformation"></a>座標轉換

Direct3D 使用三個轉換在像素座標（螢幕空間）中變更您的 3D 模型座標。 這些轉換為世界轉換、檢視轉換和投影轉換。 如需詳細資訊，請參閱[轉換總覽](../graphics-concepts/transform-overview.md)。

#### <a name="world-transform-matrix"></a>世界轉換矩陣

世界矩陣轉換將座標從模型空間 (其中頂點是相對於模型的區域原點定義的) 變更為世界空間，其中頂點是相對於通用於所有場景物件的原點定義的。 簡單來說，世界矩陣轉換將模型放置到世界中；因此成為它的名字。 如需詳細資訊，請參閱 [世界矩陣轉換](../graphics-concepts/world-transform.md)。

#### <a name="view-transform-matrix"></a>檢視轉換矩陣

檢視轉換將檢視器放置在世界空間中，並將頂點轉換成相機空間。 在相機空間，相機或檢視器位於原點，朝著正 z 方向看。 如需詳細資訊，請前往[檢視轉換](../graphics-concepts/view-transform.md)。

####  <a name="projection-transform-matrix"></a>投影轉換矩陣

投影轉換會將檢視範圍轉換成立方體形狀。 檢視範圍是場景中相對於檢視區相機放置的 3D 體積。 檢視區是 3D 場景投影到此的 2D 矩形。 如需詳細資訊，請參閱 [檢視區和裁剪](../graphics-concepts/viewports-and-clipping.md)。

由於檢視範圍的近端小於遠端，這會影響靠近相機之物體的展開；這也是透視套用到場景的方式。 因此，接近播放程式的物件會變大;進一步顯示的物件會變小。

在數學上，投射轉換是一個矩陣，通常都是尺規和透視圖投影。 就像相機鏡頭的功能。 如需詳細資訊，請參閱 [投影轉換](../graphics-concepts/projection-transform.md)。

### <a name="sampler-state"></a>取樣器狀態

取樣器狀態使用紋理定址模式、篩選以及詳細層級來判斷如何採樣紋理資料。 每次從材質讀取材質圖元（或材質）時，就會完成取樣。

材質包含材質的陣列。 每個材質的位置都是以表示 `(u,v)` ，其中 `u` 是寬度，而 `v` 是高度，並根據材質寬度和高度對應0和1。 紋理座標的結果在取樣紋理時用來定址紋素。

紋理座標在 0 以下或 1 以上時，紋理位址模式定義紋理座標如何定址紋素的位置。 例如，使用**TextureAddressMode.Clamp**時，在取樣前，任何 0-1 範圍以外的座標會限制最大值為 1，以及最小值為 0。

如果材質太大或太小而無法用於多邊形，則會篩選材質以符合空間。 放大篩選會放大紋理，縮小比例篩選會縮小紋理以符合較小的區域。 紋理放大重複範例紋素給一或多個產生模糊圖像的位址。 材質縮制比較複雜，因為它需要將一個以上的材質值結合成單一值。 取決於紋理資料，這可能會造成鋸齒化或鋸齒狀的邊緣。 縮小最常用的方法是使用 Mipmap。 Mipmap 是多層級紋理。 每個層級的大小為2的乘冪，小於上一個層級到1x1 材質。 使用縮小時，遊戲選擇 mipmap 層級，最靠近轉譯時間所需的大小。 

### <a name="the-basicloader-class"></a>BasicLoader 類別

**BasicLoader**是簡易載入器類別，提供支援從磁碟上的檔案下載著色器、紋理和網格。 它提供同步和非同步的方法。 在此範例遊戲中，您 `BasicLoader.h/.cpp` 可以在 [**公用程式**] 資料夾中找到這些檔案。

如需詳細資訊，請參閱 [基本載入器](complete-code-for-basicloader.md)。