---
author: joannaleecy
title: 轉譯簡介
description: 了解如何組合轉譯管線來顯示圖形。 轉譯簡介。
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 轉譯
ms.localizationpriority: medium
ms.openlocfilehash: 7e8df200e8e989015834608d38cb8dfb0d36917b
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5560481"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>轉譯架構 I：轉譯簡介

我們已經在稍早的主題中討論過如何建立通用 Windows 平台 (UWP) 遊戲的結構，以及如何定義狀態電腦來處理遊戲的流程。 現在，是時候了解如何組合轉譯架構。 讓我們看看範例遊戲如何轉譯遊戲場景使用 Direct3D11 （通常稱為 DirectX 11）。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

Direct3D 11 包含 API 的設定，可讓您存取高效能圖形硬體的進階功能，此功能可用於建立像遊戲一樣大量圖形應用程式的 3D 圖形。

螢幕轉譯遊戲圖形，基本上是轉譯螢幕一系列的畫面。 每個畫面中，您必須根據檢視轉譯螢幕中可看到的物件。 

為了轉譯畫面，您必須將所需的場景資訊傳遞到硬體，讓它可以顯示在螢幕上。 如果您希望在螢幕上顯示任何項目，則需要在遊戲開始執行時立即開始轉譯。

## <a name="objective"></a>目標

若要設定基本轉譯架構，以顯示 UWP DirectX 遊戲的圖形輸出，您可以將這些重點彈性分成三個步驟。

 1. 建立連接到此圖形介面
 2. 建立繪製圖形所需的資源
 3. 轉譯畫面來顯示圖形

這篇文章說明轉譯圖形的方式，涵蓋步驟 1 至 3。

[轉譯架構 II：遊戲轉譯](tutorial-game-rendering.md)涵蓋步驟 2；如何設定轉譯架構，以及如何在轉譯前先準備好資料。

## <a name="get-started"></a>開始使用

在您開始之前，應該要熟悉基本圖形和轉譯概念。 如果您是 Direct3D 和轉譯的新手，請參閱[詞彙及概念](#terms-and-concepts)了解本文中使用的圖形簡短描述和轉譯詞彙。

針對此遊戲，__GameRenderer__類別物件代表適用於此範例遊戲的轉譯器。  其負責建立及維護所有用於產生遊戲視覺效果之 Direct3D 11 和 Direct2D 物件。  它也可保留用於擷取物件清單來轉譯，以及適用於平視顯示器（HUD）的遊戲狀態的 __Simple3DGame__參考。 

教學課程的此部分中，我們會著重在轉譯遊戲中的 3D 物件。

## <a name="establish-a-connection-to-the-graphics-interface"></a>建立連接到此圖形介面

若要存取硬體用於轉譯，請參閱在[__App::Initialize__](tutorial--building-the-games-uwp-app-framework.md#appinitialize-method)的 UWP 架構文章。

__make\_shared function__，如[下方](#appinitialize-method)顯示，用來將__shared\_ptr__建立到[__DX::DeviceResources__](#dxdeviceresources)，也提供裝置的存取權。 

Direct3D 11 中，[裝置](#device)用於配置與摧毀物件、轉譯基本類型，並透過圖形驅動程式與圖形卡通訊。

### <a name="appinitialize-method"></a>App::Initialize 方法

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    //...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>轉譯畫面來顯示圖形

啟動遊戲時，遊戲場景需要轉譯。 轉譯的指示在 [__GameMain::Run__](#gameamainrun-method)方法中開始，如下所示。

簡單流程為：
1. __Update__
2. __Render__
3. __目前__

### <a name="gamemainrun-method"></a>GameMain::Run 方法

```cpp
// Comparing this to App::Run() in the DirectX 11 App template
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            // Added: Switching different game states since this game sample is more complex
            switch (m_updateState) 
            {

                // ...
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                // 1. Update
                // Also exist in DirectX 11 App template: Update loop to get the latest game changes
                Update();
                
                // 2. Render
                // Present also in DirectX 11 App template: Prepare to render
                m_renderer->Render();
                
                // 3. Present
                // Present also in DirectX 11 App template: Present the 
                // contents of the swap chain to the screen.
                m_deviceResources->Present(); 
                
                // Added: resetting a variable we've added to indicate that 
                // render has been done and no longer needed to render
                m_renderNeeded = false; 
            }
        }
        else
        {   
            // Present also in DirectX 11 App template
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending); 
        }
    }
    m_game->OnSuspending();  // exiting due to window close. Make sure to save state.
}
```

### <a name="update"></a>Update

請參閱[管理遊戲流程](tutorial-game-flow-management.md)文章，了解更多有關如何以[__App::Update__和__GameMain::Update__](tutorial-game-flow-management.md#appupdate-method)方法更新遊戲狀態。

### <a name="render"></a>Render

呼叫__GameMain::Run__中的[__GameRenderer::Render__](#gamerendererrender-method)方法實作轉譯。

如果啟用了[立體著色運算](#stereo-rendering)，會有兩個轉譯階段：一個用於右眼，一個用於左眼。 在每個轉譯階段中，我們將轉譯目標與 [深度樣板檢視](#depth-stencil-view) 繫結至裝置。 我們稍後也會說明深度樣板檢視。

> [!Note]
> 可以使用其他方法，例如使用端點執行個體或幾何著色器的一階段，來達成立體著色運算。 兩個轉譯階段方法是個較慢但較方便的方式，達成立體著色運算。

一旦遊戲存在且載入資源，則更新[投影矩陣](#projection-transform-matrix)，每一轉譯階段執行一次。 物件與每個檢視有些許不同。 接著，請設定[圖形轉譯管線](#rendering-pipeline)。 

> [!Note]
> 請參閱[建立與載入 DirectX 圖形資源](tutorial-game-rendering.md#create-and-load-directx-graphic-resources)，獲得如何載入資源的詳細資訊。

在這個遊戲範例中，設計轉譯以使用所有物件的一般端點配置。 這可簡化著色器設計且允許著色器間的輕鬆變更，與物件的幾何無關。

#### <a name="gamerendererrender-method"></a>GameRenderer::Render 方法

設定 Direct3D 內容以使用輸入頂點配置。 輸入配置物件描述如何將頂點緩衝區資料傳送到[轉譯管線](#rendering-pipeline)。 

接下來，我們設定 Direct3D 內容，以使用之前所定義的[常數緩衝區](#constant-buffers)，由[頂點著色器](#vertex-shaders-and-pixel-shaders)管線階段和[像素著色器](#vertex-shaders-and-pixel-shaders)使用。 

> [!Note]
> 請參閱[轉譯架構 II：遊戲轉譯](tutorial-game-rendering.md)獲得有關常數緩衝區定義的詳細資訊。

因為相同輸入配置與常數緩衝區設定適用於在管線裡的所有著色器，所以每個畫面設定一次。

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx

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
            
            // d2d -- Discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() }; 
            
            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap()); 
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
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, 
            // go to https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476454.aspx
            d3dContext->IASetInputLayout(m_vertexLayout.Get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476491.aspx

            d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476470.aspx

            d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); /
            }
        }
        else
        {
            const float ClearColor[4] = {0.5f, 0.5f, 0.8f, 1.0f};

            // Only need to clear the background when not rendering the full 3D scene since
            // the 3D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (i > 0)
            {
                // Doing the Right Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), ClearColor);
            }
        }

        // Start of 2D rendering
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // ...
    }
}
```

### <a name="primitive-rendering"></a>基本類型轉譯

轉譯場景時，您逐一迴圈的所有物件，需經過轉譯。 每個物件 (基本類型) 皆重複以下的步驟。

* 使用模型的[世界轉換矩陣](#world-transform-matrix)和資料資訊。更新常數緩衝區 (__m\_constantBufferChangesEveryPrim__)。
* __m\_constantBufferChangesEveryPrim__包含每個物件的參數。  它包括世界轉換矩陣的物件以及資料屬性，例如色彩和光線計算的反射指數
* 設定 Direct3D 內容以使用網格物件資料的輸入頂點配置，將其串流到[轉譯管線](#rendering-pipeline)的輸入組合 (IA) 階段
* 設定 Direct3D 內容以使用 IA 階段的[索引緩衝](#index-buffer)。 提供基本類型資訊：類型、資料訂單。
* 送出繪製呼叫，繪製已編製索引的非執行個體基本類型。 The __GameObject::Render__ 方法使用指定基本類型的特定資料來更新基本類型 [常數緩衝區](#constant-buffer-or-shader-constant-buffer)。 這會讓內容的 __DrawIndexed__ 呼叫，繪製每個基本類型的幾何。 具體來說，這個 Draw 呼叫會將命令和資料排到圖形處理單元 GPU 的佇列，由常數緩衝區資料進行參數化處理。 每個繪圖呼叫會在每個頂點上執行[頂點著色器](#vertex-shaders-and-pixel-shaders) 一次，然後[像素著色器](#vertex-shaders-and-pixel-shaders) 會在基本類型中每個三角形的每個像素中執行一次。 紋理是像素著色器用來進行轉譯之狀態的一部分。

多個常數緩衝區的原因：
    * 遊戲使用多個常數緩衝區，但是只要在每個基本類型更新這些緩衝區一次。 如先前所述，常數緩衝區就像是為每個基本類型執行的著色器輸入。 有些資料是靜態的 (__m\_constantBufferNeverChanges__)；有些資料在架構上是常數 (__m\_constantBufferChangesEveryFrame__)，像是相機的位置；有些資料是專用於基本類型，像色彩和紋理 (__m\_constantBufferChangesEveryPrim__)。
    * 遊戲[轉譯器](#renderer)將這些輸入分成不同的常數緩衝區，以最佳化 CPU 與 GPU 使用的記憶體頻寬。 這種方式也有助於將 GPU 需要追蹤的資料數量減到最少。 GPU 有很大的命令佇列，而每次遊戲呼叫 __Draw__ 時，該命令連同與其關聯的資料一起進入佇列。 當遊戲更新基本類型常數緩衝區並發出下一個 __Draw__ 命令時，圖形驅動程式會將下一個命令及關聯的資料新增到佇列。 如果遊戲繪製 100 個基本類型，則佇列中可能會有 100 個常數緩衝區資料的複本。 若要將傳送到 GPU 的遊戲資料數量減到最少，遊戲會使用不同的基本類型常數緩衝區，其中僅包含每個基本類型的更新。

#### <a name="gameobjectrender-method"></a>GameObject::Render 方法

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m\_active || (m\_mesh == nullptr) || (m_normalMaterial == nullptr))
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

#### MeshObject::Render method

void MeshObject::Render(\_In\_ ID3D11DeviceContext *context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476453.aspx
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476455.aspx
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476409.aspx
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="present"></a>目前

我們呼叫__DX::DeviceResources::Present__方法，將已經放置於緩衝區的內容放入，並顯示。

我們使用「交換鏈結」一詞，針對用於為使用者顯示畫面的緩衝區集合。 每次應用程式提供新畫面供顯示時，交換鏈結中的第一個緩衝區會取代顯示之緩衝區的位置。 這個程序稱為交換或翻轉。 如需詳細資訊，請參閱[交換鏈結](../graphics-concepts/swap-chains.md)。

* __IDXGISwapChain1__介面的__Present__方法指示 [DXGI](#dxgi)封鎖，直到垂直同步處理 (VSync)，直到下一個 VSync 前讓應用程式進入睡眠狀態。 這樣可確保您不浪費任何循環轉譯畫面，永遠不會在螢幕中顯示。
* __ID3D11DeviceContext3__介面的__DiscardView__方法捨棄[轉譯目標](#render-target)的內容。 只有在完全複寫現有的內容時，這才是有效的作業。 如果不正常或使用捲動 rects，則必須移除此呼叫。
* 使用同一個__DiscardView__方法，捨棄[深度樣板](#depth-stencil)的內容。
* __HandleDeviceLost__方法用來管理案例如果[裝置](#device)中移除。 如果已透過中斷或升級驅動程式來移除裝置，您必須重新建立所有裝置資源。 如需詳細資訊，請參閱[處理 Direct3D 11 中的裝置已移除案例](handling-device-lost-scenarios.md)。

> [!Tip]
> 若要達到流暢畫面播放速率，您必須先確定轉譯畫面的工作量是否符合 VSyncs 之間的時間。

```cpp
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
    m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());

    // Discard the contents of the depth-stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    // For more info about how to handle a device lost scenario, go to:
    // https://docs.microsoft.com/windows/uwp/gaming/handling-device-lost-scenarios
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        DX::ThrowIfFailed(hr);
    }
}
```

## <a name="next-steps"></a>後續步驟

本文說明如何在螢幕上轉譯圖形，並提供一些轉譯詞彙所使用的簡短描述。 深入了解在[轉譯架構 II：遊戲轉譯](tutorial-game-rendering.md)文章中的轉譯，並了解如何在轉譯之前準備所需的資料。

## <a name="terms-and-concepts"></a>詞彙與概念

### <a name="simple-game-scene"></a>簡易遊戲場景

簡易的遊戲場景由幾個光源物件所組成。

透過一組 X、Y、Z 空間座標定義物件的圖形。 遊戲世界裡的真實轉譯位置可藉由套用位置 X、Y、Z 座標的轉換矩陣來判斷。 它也可能有一組紋理座標 - 指定如何套用材質到物件的 U 和 V。 這定義物件的表面屬性，並可讓您查看物件是否有像網球的粗糙表面或像保齡球的平滑光面表面。

轉譯架構使用場景和物件的資訊逐格重新建立場景，使其在您的顯示器成形。

### <a name="rendering-pipeline"></a>轉譯管線

轉譯管線是將 3D 場景資訊轉譯成圖像顯示在場景中的程序。 Direct3D 11 中，這個管線是可程式化的。 您可以調整此階段，支援您的轉譯需求 搭配常見著色器核心的階段可使用 HLSL 程式語言進行程式設計。 它也稱為圖形轉譯管線或簡稱為管線。

若要協助您建立這個管線，您需要熟悉：
* [HLSL](#HLSL)。 建議您使用 HLSL 著色器模型 5.1 和更新版本用於 UWP DirectX 遊戲。
* [著色器](#Shaders)
* [頂點著色器和像素著色器](#vertext-shaders-pixel-shaders)
* [著色器階段](#shader-stages)
* [各種不同的著色器檔案格式](#various-shader-file-formats)

如需詳細資訊，請參閱[了解 Direct3D 11 轉譯管線](https://msdn.microsoft.com/library/windows/desktop/dn643746.aspx)和[圖形管線](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx)。

#### <a name="hlsl"></a>HLSL

HLSL 是適用於 DirectX 的高階著色器語言。 使用 HLSL，您可以建立像可程式化著色器的 C 用於 Direct3D 管線。 如需詳細資訊，請參閱 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561.aspx)。

#### <a name="shaders"></a>著色器

著色器可以視為一組在轉譯時判斷物件的表面會如何顯示的指示。 使用 HLSL 又稱為 HLSL 著色器來程式設計這些。 [HLSL])(#hlsl) 著色器的原始程式碼檔案有 .hlsl 檔案副檔名。 在撰寫時間或執行階段可使用這些著色器，並在適當的管線階段中設定執行階段；所使用的著色器物件有 .cso 檔案副檔名。

您可以使用著色器模型 1、著色器模型 2 及著色器模型 3 設計 Direct3D 9 著色器；只能在著色器模型 4 上設計 Direct3D 10 著色器。 可以在著色器模型 5 上設計 Direct3D 11 著色器。 可以在著色器模型 5.1 上設計 Direct3D 11.3 和 Direct3D 12，且也可以在著色器模型 6 上設計 Direct3D 12。

#### <a name="vertex-shaders-and-pixel-shaders"></a>頂點著色器和像素著色器

資料進入圖形管線做為基本類型的串流，並透過各種著色器例如頂點著色器和像素著色器進行處理。 

頂點著色器處理頂點，通常執行轉換、皮膚形變與光照等作業。  像素著色器可啟用豐富的陰影技術，例如個別像素光線和後處理。 其結合常數變數、紋理資料、插補每個頂點值和其他資料的程式，以產生每個像素的輸出。 

#### <a name="shader-stages"></a>著色器階段

定義一系列這些各式著色器處理這個在轉譯管線中稱為著色器階段的基本類型串流。 實際階段取決於 Direct3D 的版本，但通常會包含頂點、幾何及像素階段。 還有其他階段，例如鑲嵌的輪廓以及網域著色器，和計算著色器。 所有這些階段可使用 [HLSL])(#hlsl) 完全程式化。 如需詳細資訊，請參閱[圖形管線](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx)。

#### <a name="various-shader-file-formats"></a>各種不同的著色器檔案格式

著色器程式碼檔案擴充功能：
    * 有 .hlsl 副檔名的檔案會保留 [HLSL])(#hlsl) 來源程式碼。
    * 有 .Cso 副檔名的檔案會保留所使用的著色器物件。
    * 有 .h 副檔名的檔案是標頭檔案，但在著色器程式碼操作中，此標頭檔案定義位元組陣列，其保留著色器資料。
    * 有 .hlsli 副檔名的檔案包含常數緩衝區的格式。 在遊戲範例中，檔案是__Shaders__>__ConstantBuffers.hlsli__。

> [!Note]
> 您可以透過在執行階段載入 .cso 檔案或在可執行的程式碼中新增 .h 檔案來內嵌著色器。 但是，您不會把兩個同時用在相同的著色器上。

### <a name="deeper-understanding-of-directx"></a>深入了解 DirectX

Direct3D 11 是一組 API，可協助我們建立圖形，用於像遊戲一樣的大量圖形的應用程式，其中我們要用很好的圖形卡來處理大量計算。 本章節簡短說明 Direct3D 11 圖形的程式設計蓋瑱：資源、子資源、裝置以及裝置操作。

#### <a name="resource"></a>資源

適用於這些新的人員，您可以思考資源（也稱為裝置資源）如何做為轉譯物件，例如材質、位置、色彩的資訊。 資源將資料提供給管線並定義您場景期間轉譯的項目。 從您的遊戲媒體載入或在執行階段動態建立資源。

事實上，資源是 Direct3D [管線](#rendering-pipeline)可以存取的記憶體中的區域。 為了讓管線有效地存取記憶體，提供給管線的資料 (例如，輸入幾何、著色器資源及紋理) 必須儲存在資源中。 所有 Direct3D 資源衍生兩種類型的資源︰緩衝區或紋理。 每個管線階段可使用多達 128 種資源。 如需詳細資訊，請參閱[資源](../graphics-concepts/resources.md)。

#### <a name="subresource"></a>子資源

子資源這個詞彙指的是資源的子集。 Direct3D 可以參考整個資源，或參考資源的子集。 如需詳細資訊，請參閱[子資源](../graphics-concepts/resource-types.md#subresources)。

#### <a name="depth-stencil"></a>深度樣板

深度樣板資源包含格式及緩衝區，以保留深度和樣板資訊。 使用紋理資源建立它。 如需如何建立深度樣板資源的詳細資訊，請參閱[設定深度樣板功能](https://msdn.microsoft.com/library/windows/desktop/bb205074.aspx)。 我們透過使用 [ID3D11DepthStencilView](https://msdn.microsoft.com/library/windows/desktop/ff476377.aspx)介面實作深度樣板檢視來存取深度樣板資源。

深度資訊告訴我們所呈現而不是隱藏的多邊形區域。 樣板資訊可告訴我們遮罩哪一個像素。 它可以用來製作特效，因為它會判斷是否繪製像素；設定 1 或 0 的位元。 

如需詳細資訊，請參閱：[深度樣板檢視](../graphics-concepts/depth-stencil-view--dsv-.md)，[深度緩衝區](../graphics-concepts/depth-buffers.md)，與[樣板緩衝區](../graphics-concepts/stencil-buffers.md)。

#### <a name="render-target"></a>轉譯目標

轉譯目標是我們可以在轉譯階段結尾撰寫的資源。 常使用[ID3D11Device::CreateRenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476517.aspx)方法建立它，該方法使用交換鏈結背景緩衝區（這也是資源）做為輸入參數。 

每個轉譯目標也該有對應深度樣板檢視，因為在使用它之前，當我們使用[OMSetRenderTargets](https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx)來設定轉譯目標時，它也需要深度樣板檢視。 我們透過使用[ID3D11RenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476582.aspx)介面實作轉譯目標來存取轉譯目標資源。 

#### <a name="device"></a>裝置

針對 Direct3D 11 的新手，您可以想像一個裝置用於配置與摧毀物件、轉譯基本類型，並透過圖形驅動程式與圖形卡通訊。 

更精確的說明，Direct3D 裝置是 Direct3D 的轉譯元件。 裝置封裝並儲存呈現狀態、執行轉換照明作業，並將影像點陣化到表面。 如需詳細資訊，請參閱 [裝置](../graphics-concepts/devices.md)。

裝置會以[ID3D11Device](https://msdn.microsoft.com/library/windows/desktop/ff476379.aspx)介面顯示。 換言之，ID3D11Device 介面代表虛擬顯示卡，並用來建立裝置擁有的資源。 

請注意，有不同版本的 ID3D11Device，[ID3D11Device5](https://msdn.microsoft.com/library/windows/desktop/mt492478.aspx)是最新版本，並在 ID3D11Device4 中新增新的方法。 如需 Direct3D 如何與基礎硬體通訊的詳細資訊，請參閱[Windows 裝置的驅動程式模型 (WDDM) 架構](https://docs.microsoft.com/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture)。

每個應用程式必須至少有一個裝置，大部分的應用程式只能建立一個裝置。 為其中一個透過呼叫__D3D11CreateDevice__或__D3D11CreateDeviceAndSwapChain__來安裝在您電腦上的硬體驅動程式建立一個裝置，並使用 D3D\_DRIVER\_TYPE 旗標指定驅動程式類型。 每個裝置可以使用一或多個裝置內容，視所需的功能而定。 如需詳細資訊，請參閱 [D3D11CreateDevice 函式](https://msdn.microsoft.com/library/windows/desktop/ff476082.aspx)。

#### <a name="device-context"></a>裝置內容

裝置內容用來設定 [管線](#rendering-pipeline) 狀態以及使用[裝置](#device)擁有的[資源](#resource)來產生轉譯命令。 

Direct3D 11 實作兩種類型的裝置內容，一種為立即轉譯，另一種為延遲轉譯；這兩個內容都可以使用[ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476385.aspx)介面顯示。  

__ID3D11DeviceContext__介面有不同的版本；__ID3D11DeviceContext4__將新方法新增至__ID3D11DeviceContext3__中。

注意：__ID3D11DeviceContext4__在 Windows 10 Creators Update 中導入，而且是最新版本的__ID3D11DeviceContext__介面。 Windows 10 Creators Update 為目標的應用程式應該使用此介面而不是較舊版本。 如需詳細資訊，請參閱 [ID3D11DeviceContext4](https://msdn.microsoft.com/library/windows/desktop/mt492481.aspx)。

#### <a name="dxdeviceresources"></a>DX::DeviceResources

__DX::DeviceResources__類別在__DeviceResources.cpp__/__.h__檔案中且控制所有 DirectX 裝置資源。 在範例遊戲專案和 DirectX 11 應用程式範本專案中，這些檔案會在__Commons__資料夾。 當您在 Visual Studio 2015 或更新版本中建立新的 DirectX 11 應用程式範本專案，您可以取得這些檔案的最新版本。

### <a name="buffer"></a>緩衝區

緩衝區資源是一個完整輸入的資料集合，由元素所組成。 您可以使用緩衝區存放各式各樣的資料，包含位置向量、法向向量、頂點緩衝區中的紋理座標、索引緩衝區中的索引、或裝置狀態。 緩衝元素可以包含封裝資料值 (像是 R8G8B8A8 面值)、單一 8 位整數，或四個 32 位元浮點數值。

有三種類型的緩衝區可用：頂點緩衝區、索引緩衝區，與常數緩衝區。

#### <a name="vertex-buffer"></a>頂點緩衝區

包含用來定義您的幾何頂點資料。 頂點資料包括位置座標、色彩資料、紋理座標資料、一般資料等等。 

#### <a name="index-buffer"></a>索引緩衝區

包含整數位移至頂點緩衝區，可用於更有效率地呈現基本類型。 索引緩衝區包含連續的 16 位元或 32 位元索引集，而每個索引用來識別頂點緩衝區中的頂點。

#### <a name="constant-buffer-or-shader-constant-buffer"></a>常數緩衝區或著色器常數緩衝區。

可讓您有效率地提供著色器資料至管線。 您可以使用常數緩衝區做為著色器的輸入，其執行每個基本類別，並儲存轉譯管線資料流輸出階段的結果。 概念上，常數緩衝區看起來很像單一元素頂點緩衝區。

#### <a name="design-and-implementation-of-buffers"></a>緩衝區的設計和實作

您可以根據資料類型設計緩衝區，例如，像是在遊戲範例中，為靜態資料建立一個緩衝區，另一個給架構上的常數資料，另一個則適用於特定基本類別。

所有緩衝區類型都藉由__ID3D11Buffer__介面進行封裝，且您可以藉由呼叫__ID3D11Device::CreateBuffer__建立緩衝區資源。 但必須將緩衝區繫結至管線才能存取。 緩衝區可以同時繫結到多個管線階段，以供讀取。 緩衝區可以也繫結至單一管線階段來寫入；不過，相同的緩衝區無法同時繫結來讀取和寫入。

繫結緩衝區至；
    * 藉由呼叫 __ID3D11DeviceContext__ 方法，像 __ID3D11DeviceContext::IASetVertexBuffers__ 與 __ID3D11DeviceContext::IASetIndexBuffer__ 一樣的輸入組合階段
    * 藉由呼叫__ID3D11DeviceContext::SOSetTargets__的資料流輸出階段
    * 藉由呼叫著色器方法，像__ID3D11DeviceContext::VSSetConstantBuffers__的著色器階段

如需詳細資訊，請參閱 [Direct3D 11 中的緩衝區簡介](https://msdn.microsoft.com/library/windows/desktop/ff476898.aspx)。

### <a name="dxgi"></a>DXGI

Microsoft DirectX Graphics Infrastructure (DXGI) 是新的子系統，封裝部分 Direct3D 10 所需低層級工作的 WindowsVista 中引進 10.1、 11 和 11.1。 在多執行緒應用程式中使用 DXGI以確保不會發生死結，要特別注意。 如需詳細資訊，請參閱[DirectX Graphics Infrastructure (DXGI)： 最佳做法- 多執行緒](https://msdn.microsoft.com/library/windows/desktop/ee417025.aspx#multithreading_and_dxgi)

### <a name="feature-level"></a>功能層級

功能層級是在 Direct3D 11 中導入的概念，以處理新的和現有的電腦中各式各樣的視訊卡。 功能層級是一組妥善定義的圖形處理單元 GPU 功能。 

每個視訊卡實作一個特定層級的 DirectX 功能，依照所安裝的 GPU 而定。 在 Microsoft Direct3D 舊版中，可以找出視訊卡實作的 Direct3D 版本，然後隨之程式設計您的應用程式。 

使用功能層級，當您建立裝置，您可以嘗試建立您所要求的功能層級的裝置。 如果裝置建立運作，該功能層級則存在，否則，硬體不支援該功能層級。 您可以嘗試重新建立較低功能層級的裝置，或您可以選擇離開應用程式。 例如，12\_0 功能層級需要 Direct3D 11.3 或 Direct3D 12 及著色器模型 5.1。 如需詳細資訊，請參閱[Direct3D 功能層級：每項功能層級的概觀](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx#Overview)。

使用功能層級，您可以開發的應用程式 Direct3D9、 Microsoft Direct3D10 或 Direct3D11，並接著在 9、 10 或 11 硬體 （有一些例外） 上執行。 如需詳細資訊，請參閱[Direct3D 功能層級](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx)。

### <a name="stereo-rendering"></a>立體著色運算

立體著色運算用來提升深度的視覺效果。 它使用兩個圖像，一個從左眼，另一個從右眼，在螢幕上顯示場景。 

我們從數學觀點來套用立體著色運算矩陣，也就是稍微水平位移至右側和左邊，以規則單投影矩陣來達成。

我們在此遊戲範例中執行兩個轉譯階段來達成立體著色運算：
* 繫結至右邊轉譯目標，適用於右側投影，然後繪製的基本類型物件。
* 繫結至左邊轉譯目標，適用於左側投影，然後繪製的基本類型物件。

### <a name="camera-and-coordinate-space"></a>相機和座標空間

遊戲有現成的程式碼可以用自己的座標系統來更新世界 (有時候稱為世界空間或場景空間)。 所有物件 (包括相機) 都在這個空間設定放置及方向。 如需詳細資訊，請參閱[座標系統](../graphics-concepts/coordinate-systems.md).

頂點著色器則負責使用下列演算法 (其中 V 是向量而 M 是矩陣)，將模型座標轉換為裝置座標的繁重工作。

V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device).

其中： 
* M(model-to-world) 為世界座標適用於模型座標的轉換矩陣，亦稱為[世界轉換矩陣](#world-transform-matrix)。 這是由基本類型提供。
* M(world-to-view) 為檢視座標適用於世界座標的轉換矩陣，亦稱為[檢視轉換矩陣](#view-transform-matrix)。
    * 這是由相機的視圖矩陣提供的。 藉由相機的位置以及視角向量 (「視線」向量是從相機直接看到場景，而「仰視」向量則是垂直向上看) 來定義。
    * 範例遊戲中，__m\_viewMatrix__為檢視轉換矩陣，且使用__Camera::SetViewParams__來計算 
* M(view-to-device) 為裝置座標適用於檢視座標的轉換矩陣，亦稱為[投影轉換矩陣](#projection-transform-matrix)。
    * 這是由相機的投影提供的。 提供在最終場景中有多少空間是實際可見的資訊。 視野範圍（FoV）、外觀比例，以及裁剪平面定義投影轉換矩陣。
    * 在範例遊戲中__m\_projectionMatrix__定義轉換為投影座標，使用__Camera::SetProjParams__ 來計算 (您在立體投影時會使用兩個投影矩陣：分別為每個眼睛的視野。) 

從常數緩衝區使用這些向量及矩陣進行載入 VertexShader.hlsl 中的著色器程式碼，並為每個頂點執行這個轉換。

### <a name="coordinate-transformation"></a>座標轉換

Direct3D 使用三個轉換在像素座標（螢幕空間）中變更您的 3D 模型座標。 這些轉換為世界轉換、檢視轉換和投影轉換。 如需詳細資訊，請前往[轉換概觀](../graphics-concepts/transform-overview.md)。

#### <a name="world-transform-matrix"></a>世界轉換矩陣

世界矩陣轉換將座標從模型空間 (其中頂點是相對於模型的區域原點定義的) 變更為世界空間，其中頂點是相對於通用於所有場景物件的原點定義的。 簡單來說，世界矩陣轉換將模型放置到世界中；因此成為它的名字。 如需詳細資訊，請參閱 [世界矩陣轉換](../graphics-concepts/world-transform.md)。

####  <a name="view-transform-matrix"></a>檢視轉換矩陣

檢視轉換將檢視器放置在世界空間中，並將頂點轉換成相機空間。 在相機空間，相機或檢視器位於原點，朝著正 z 方向看。 如需詳細資訊，請前往[檢視轉換](../graphics-concepts/view-transform.md)。

####  <a name="projection-transform-matrix"></a>投影轉換矩陣

投影轉換會將檢視範圍轉換成立方體形狀。 檢視範圍是場景中相對於檢視區相機放置的 3D 體積。 檢視區是 3D 場景投影到此的 2D 矩形。 如需詳細資訊，請參閱 [檢視區和裁剪](../graphics-concepts/viewports-and-clipping.md)。

由於檢視範圍的近端小於遠端，這會影響靠近相機之物體的展開；這也是透視套用到場景的方式。 如此，物件會越靠近玩家會越大；物件離得越遠則越小。

從數學觀點來看，投影轉換是矩陣，通常是縮放和透視投影。 就像相機鏡頭的功能。 如需詳細資訊，請參閱 [投影轉換](../graphics-concepts/projection-transform.md)。

### <a name="sampler-state"></a>取樣器狀態

取樣器狀態使用紋理定址模式、篩選以及詳細層級來判斷如何採樣紋理資料。 每次完成取樣紋理像素或紋素，可從紋理讀取。

紋理包含紋素的陣列或紋理像素。 由 (u,v) 表示每個紋素的位置，其中 u 是寬度，而 v 高度，並根據紋理的寬度與高度對應介於 0 和 1。 紋理座標的結果在取樣紋理時用來定址紋素。

紋理座標在 0 以下或 1 以上時，紋理位址模式定義紋理座標如何定址紋素的位置。 例如，使用__TextureAddressMode.Clamp__時，在取樣前，任何 0-1 範圍以外的座標會限制最大值為 1，以及最小值為 0。

針對多邊形，如果紋理太大或太小，會篩選掉紋理以符合的空間。 放大篩選會放大紋理，縮小比例篩選會縮小紋理以符合較小的區域。 紋理放大重複範例紋素給一或多個產生模糊圖像的位址。 紋理縮小更為複雜，因為需要將多個紋素值結合成一個值。 取決於紋理資料，這可能會造成鋸齒化或鋸齒狀的邊緣。 縮小最常用的方法是使用 Mipmap。 Mipmap 是多層級紋理。 每個層級的大小是比上層小二，向下至 1x1 的紋理。 使用縮小時，遊戲選擇 mipmap 層級，最靠近轉譯時間所需的大小。 

### <a name="basicloader"></a>BasicLoader

__BasicLoader__是簡易載入器類別，提供支援從磁碟上的檔案下載著色器、紋理和網格。 它提供同步和非同步的方法。 在這個遊戲範例中，在 __Commons__ 資料夾裡找到 BasicLoader.h/.cpp 檔案。

如需詳細資訊，請參閱 [基本載入器](complete-code-for-basicloader.md)。