---
author: mtoepke
title: "轉換轉譯架構"
description: "示範如何將簡單的轉譯架構從 Direct3D 9 轉換到 Direct3D 11，包含如何移植幾何緩衝區、如何編譯和載入 HLSL 著色器程式，以及如何在 Direct3D 11 中實作轉譯鏈結。"
ms.assetid: f6ca1147-9bb8-719a-9a2c-b7ee3e34bd18
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5cfdce2a62f6b5761ebf820418762a307dd051bb

---

# 轉換轉譯架構


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**摘要**

-   [第一部分：初始化 Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   第二部分：轉換轉譯架構
-   [第三部分：移植遊戲迴圈](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


示範如何將簡單的轉譯架構從 Direct3D 9 轉換到 Direct3D 11，包含如何移植幾何緩衝區、如何編譯和載入 HLSL 著色器程式，以及如何在 Direct3D 11 中實作轉譯鏈結。 [將簡單的 Direct3D 9 app 移植到 DirectX 11 和通用 Windows 平台 (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 逐步解說的第二部分。

## 將效果轉換到 HLSL 著色器


下列範例是簡單的 D3DX 技術，是針對舊有的效果 API 所撰寫，適用於硬體頂點轉換與傳遞色彩資料。

Direct3D 9 著色器程式碼

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

在 Direct3D 11 中，我們仍然可以使用 HLSL 著色器。 我們將每個著色器放在本身的 HLSL 檔案中，讓 Visual Studio 將它們編譯成個別檔案，而稍後我們會將它們當作個別 Direct3D 資源載入。 我們將目標層級設為[著色器模型 4 層級 9\_1 (/4\_0\_level\_9\_1)](https://msdn.microsoft.com/library/windows/desktop/ff476876)，因為這些著色器是針對 DirectX 9.1 GPU 所撰寫。

當我們定義輸入配置時，已確定它代表我們在系統記憶體中與 GPU 記憶體中，用來儲存每個頂點資料的相同資料結構。 同理，頂點著色器的輸出應該符合用來做為像素著色器輸入的結構。 規則與 C++ 中將資料從某一個函式傳遞到另一個函式不同；您可以在結構的結尾略過未使用的變數。 但是順序無法重新安排，且您無法略過資料結構中間的內容。

> **注意**  
Direct3D 9 中用來將頂點著色器繫結到像素著色器的規則比 Direct3D 11 中的規則還要寬鬆。 Direct3D 9 排列方式具有彈性，但沒有效率。

 

您的 HLSL 檔案有可能針對著色器語意來使用較舊的語法 - 例如，使用 COLOR 而非 SV\_TARGET。 如果這樣，您將需要啟用 HLSL 相容性模式 (/Gec 編譯器選項) 或將著色器[語意](https://msdn.microsoft.com/library/windows/desktop/bb509647)更新成目前的語法。 這個範例中的頂點著色器已使用目前的語法來更新。

以下為我們的硬體轉換頂點著色器，這次是定義於它自己的檔案中。

> **注意：**需要有頂點著色器，才能輸出 SV\_POSITION 系統值語意。 這個語意可以將頂點位置資料解析成座標值，其中 x 介於 -1 與 1 之間、y 介於 -1 與 1 之間、z 會除以原始同質座標 w 值 (z/w)，而 w 是 1 除以原始 w 值 (1/w)。

 

HLSL 頂點著色器 (功能層級 9.1)

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

我們的傳遞像素著色器只需要這個項目。 儘管我們將它稱為傳遞，但它實際上可以針對每個像素取得透視修正的插補色彩資料。 請注意，SV\_TARGET 系統值語意已在 API 要求時由我們的像素著色器套用到色彩值輸出。

> **注意：**著色器層級 9\_x 像素著色器無法從 SV\_POSITION 系統值語意中讀取。 模型 4.0 (及更高) 像素著色器可以使用 SV\_POSITION 來擷取螢幕上的像素位置，其中 x 介於 0 與轉譯目標寬度之間，而 y 介於 0 與轉譯目標高度之間 (每個都會位移 0.5)。

 

多數像素著色器都會比傳遞來得更複雜；請注意，更高的 Direct3D 功能層級允許針對每個著色器程式進行大量計算。

HLSL 像素著色器 (功能層級 9.1)

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

## 編譯和載入著色器


Direct3D 9 遊戲通常會使用效果程式庫來做為實作可程式化管線的簡便方式。 效果可以使用 [**D3DXCreateEffectFromFile function**](https://msdn.microsoft.com/library/windows/desktop/bb172768) 方法在執行階段期間進行編譯。

在 Direct3D 9 中載入效果

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

Direct3D 11 使用著色器程式做為二進位資源。 著色器會在建置專案時進行編譯，然後被視為資源。 因此，我們的範例會將著色器位元組程式碼載入系統記憶體，使用 Direct3D 裝置介面來為每個著色器建立 Direct3D 資源，並在我們設定每個框架時指向 Direct3D 著色器資源。

在 Direct3D 11 中載入著色器資源

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

若要在編譯的應用程式套件中包含著色器位元組程式碼，只需將 HLSL 檔案新增到 Visual Studio 專案。 Visual Studio 將使用[效果編譯器工具](https://msdn.microsoft.com/library/windows/desktop/bb232919) (FXC)，將 HLSL 檔案編譯到編譯著色器物件 (.CSO 檔案)，並在應用程式套件中包含它們。

> **注意：**請確定為 HLSL 編譯器設定正確的目標功能層級：在 Visual Studio 中使用滑鼠右鍵按一下 HLSL 來源檔、選取 [屬性]，然後在 [HLSL 編譯器] -&gt; [一般]**** 下方變更 [著色器模型]**** 設定。 當您的 app 建立 Direct3D 著色器資源時，Direct3D 會針對硬體功能來檢查這個屬性。

 

![HLSL 著色器屬性](images/hlslshaderpropertiesmenu.png)![HLSL 著色器類型](images/hlslshadertypeproperties.png)

這裡很適合用來建立輸入配置，此輸入配置會對應到 Direct3D 9 中的頂點串流宣告。 每個頂點的資料結構需要符合頂點著色器所使用的內容；在 Direct3D 11 中，我們對於輸入配置有更多的控制權；我們可以定義浮點數向量的陣列大小與位元長度，以及指定頂點著色器的語意。 我們建立 [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 結構，並使用它來通知 Direct3D 每個頂點資料的外觀。 我們要等到已載入頂點著色器來定義輸入配置之後，因為 API 會根據頂點著色器資源來驗證輸入配置。 如果輸入配置不相容，則 Direct3D 會擲回例外狀況。

每個頂點的資料都必須以相容的類型儲存在系統記憶體中。 例如，DirectXMath 資料類型可以協助 DXGI\_FORMAT\_R32G32B32\_FLOAT 對應到 [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475)。

> **注意：**常數緩衝區會使用一次對齊到四個浮點數的固定輸入配置。 建議針對常數緩衝區資料使用 [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) (及其衍生項目)。

 

在 Direct3D 11 中設定輸入配置

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

## 建立幾何資源


在 Direct3D 9 中，我們會在 Direct3D 裝置上建立緩衝區、鎖定記憶體，並將資料從 CPU 記憶體複製到 GPU 記憶體，以儲存幾何資源。

Direct3D 9

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

DirectX 11 遵循一個較簡單的程序。 API 會自動將資料從系統記憶體複製到 GPU。 我們可以使用 COM 智慧型指標，協助讓程式設計變得更簡單。

DirectX 11

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

## 實作轉譯鏈結


Direct3D 9 遊戲通常會使用以效果為基礎的轉譯鏈結。 這個轉譯鏈結類型會設定效果物件、為它提供所需的資源，然後讓它轉譯每個階段。

Direct3D 9 轉譯鏈結

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

DirectX 11 轉譯鏈結仍會執行相同工作，但是轉譯階段需要以不同方式來實作。 我們將在 C++ 中設定所有的轉譯動作，而不是將特定項目放置於 FX 檔案中，讓轉譯技術對 C++ 程式碼而言多少有點不透明。

以下為轉譯鏈結的外觀。 我們需要在載入頂點著色器之後提供我們建立的輸入配置、提供每一個著色器物件，並針對每個要使用的著色器指定常數緩衝區。 這個範例沒有包含多個轉譯階段，但是如果包含，我們就需要針對每個階段執行類似的轉譯鏈結，並視需要變更設定。

Direct3D 11 轉譯鏈結

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

交換鏈結是圖形基礎結構的一部分，所以我們使用 DXGI 交換鏈結來呈現完成的框架。 DXGI 會在下一個 vsync 之前封鎖呼叫；接著它會返回，然後我們的遊戲迴圈就可以繼續執行下一個反覆項目。

使用 DirectX 11 將框架呈現到螢幕

```cpp
m_swapChain->Present(1, 0);
```

我們剛建立的轉譯鏈結將會從 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法中實作的遊戲迴圈進行呼叫。 這將於[第三部分：檢視區與遊戲迴圈](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)中說明。

 

 







<!--HONumber=Jun16_HO4-->


