---
author: mtoepke
title: 建立和顯示基本網格
description: 3D 通用 Windows 平台 (UWP) 遊戲一般會使用多邊形來呈現遊戲中的物件與表面。
ms.assetid: bfe0ed5b-63d8-935b-a25b-378b36982b7d
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 網格, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: e3ae6416217efa16d70b65b8ff55e36654a11557
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5937231"
---
# <a name="create-and-display-a-basic-mesh"></a>建立和顯示基本網格



3D 通用 Windows 平台 (UWP) 遊戲一般會使用多邊形來呈現遊戲中的物件與表面。 組成這些多邊形物件與表面結構的一系列頂點則稱為網格。 我們在此建立一個立方體物件的基本網格，並將它提供給著色器管線進行轉譯和顯示。

> **重要**包含的範例程式碼這裡會使用類型 （如 directx:: Xmfloat3 和 directx:: Xmfloat4x4） 及在 DirectXMath.h 中宣告的內嵌方法。 如果您是透過剪下並貼上的方式使用此程式碼，請在您的專案中\#包含 &lt;DirectXMath.h&gt;。

 

## <a name="what-you-need-to-know"></a>您需要知道的事項


### <a name="technologies"></a>技術

-   [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh769064)

### <a name="prerequisites"></a>先決條件

-   線性代數與 3D 座標系統的基本知識
-   Visual Studio 2015 或更新版本的 Direct3D 範本

## <a name="instructions"></a>指示

下列步驟將示範如何建立基本的網格立方體。 


如果您想要這些概念的談話式說明，請觀賞此影片。
</br>
</br>
<iframe src="https://channel9.msdn.com/Series/Introduction-to-C-and-DirectX-Game-Development/03/player#time=7m39s:paused" width="600" height="338" allowFullScreen frameBorder="0"></iframe>


### <a name="step-1-construct-the-mesh-for-the-model"></a>步驟 1：建構模型的網格

在大多數的遊戲中，遊戲物件的網格會從包含特定頂點資料的檔案中載入。 這些頂點的順序取決於應用程式，但通常會序列化為條形或扇形。 頂點資料可以來自任一軟體來源，也可以手動建立。 您的遊戲可以決定要使用哪種讓頂點著色器能夠有效處理資料的方式來解譯資料。

在範例中，我們在立方體使用簡單的網格。 和管線中這個階段的任一個物件網格一樣，立方體會使用自己的座標系統呈現。 頂點著色器使用其座標並套用您提供的轉換矩陣，然後在同質的座標系統中傳回最終的 2D 檢視投影。

定義立方體的網格 (或從檔案載入。 由您自行決定！)

```cpp
SimpleCubeVertex cubeVertices[] =
{
    { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
    { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 0.0f) },
    { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 1.0f) },
    { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 1.0f) },

    { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
    { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 1.0f) },
    { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f) },
    { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f) },
};
```

立方體的座標系統會將立方體的中心點放置於原點，並使用慣用左手的座標系統搭配座標值由上而下排列的 Y 軸。 座標值以介於 -1 和 1 之間的 32 位元浮點值表示。

在每個以括號括住的配對值中，第二個 DirectX::XMFLOAT3 值群組以 RGB 值指定與頂點關聯的色彩。 例如，在 (-0.5, 0.5, -0.5) 的第一個頂點色彩為全綠色 (G 值設為 1.0，R 與 B 值設為 0)。

因此，您有 8 個頂點，每個頂點各有一個指定的色彩。 在範例中，每個頂點/色彩配對就表示一個頂點的完整資料。 當您指定我們的頂點緩衝區時，必須記住這個特殊的配置。 我們會將此輸入配置提供給頂點著色器，讓它可以了解您的頂點資料。

### <a name="step-2-set-up-the-input-layout"></a>步驟 2：設定輸入配置

現在您的記憶體中已經有頂點。 但是您的圖形裝置有自己的記憶體，而您使用 Direct3D 進行存取。 為了將頂點資料送入圖形裝置以進行處理，您必須想以往一樣執行一些前置作業：您必須宣告如何配置頂點資料，圖形裝置才能在從遊戲取得頂點資料時解譯這些資料。 若要這樣做，可以使用 [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575)。

宣告和設定頂點緩衝區的輸入配置。

```cpp
const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

ComPtr<ID3D11InputLayout> inputLayout;
m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                vertexShaderBytecode->Data,
                vertexShaderBytecode->Length,
                &inputLayout)
);
```

在此程式碼中，您指定了頂點的配置，更具體地說，就是頂點清單中每個元素包含的資料。 在這裡的 **basicVertexLayoutDesc** 中，您指定兩個資料部分：

-   **POSITION**：這是一個 HLSL 語意，會將位置資料提供給著色器。 在此程式碼中是 DirectX::XMFLOAT3，或者更具體地說，這是與 3D 座標 (x, y, z) 對應的 3 個 32 位元浮點值的結構。 如果您要提供同質的 "w" 座標，也可以使用 float4，在此情況下，必須指定 DXGI\_FORMAT\_R32G32B32A32\_FLOAT。 要使用 DirectX::XMFLOAT3 還是 float4 會取決於遊戲的特殊需求。 您唯一要確定的是網格的頂點資料是否能夠正確地對應您使用的格式！

    在物件的座標空間中，每個座標值都會以介於 -1 和 1 之間的浮點值表示。 當頂點著色器完成時，轉換的頂點會位於同質 (已修正透視) 的檢視投影空間中。

    您聰明的注意到 「但列舉值所指為 RGB，而非 XYZ」。 好眼力！ 在色彩資料與座標資料這兩種情況中，您通常會使用 3 或 4 個值來組成，那麼何不在這兩種情況中使用相同的格式呢？ HLSL 語意 (不是格式名稱) 會指示著色器處理資料的方式。

-   **COLOR**：這是色彩資料的 HLSL 語意。 與 **POSITION** 一樣，它包含 3 個 32 位元的浮點值 (DirectX::XMFLOAT3)。 每個值各包含一個色彩元件：紅 (r)、藍 (b) 或綠 (g)，以一個介於 0 和 1 之間的浮點數字表示。

    **COLOR** 值通常會在著色器管線尾端以一個有 4 個組成部分的 RGBA 值傳回。 例如，您會在所有像素的著色器管線中，將 "A" Alpha 值設為 1.0 (最大不透明度)。

如需格式的完整清單，請參閱 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)。 如需 HLSL 語意的完整清單，請參閱[語意](https://msdn.microsoft.com/library/windows/desktop/bb509647)。

在 Direct3D 裝置上呼叫 [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) 並建立輸入配置。 現在，您必須建立可以實際容納資料的緩衝區！

### <a name="step-3-populate-the-vertex-buffers"></a>步驟 3：填入頂點緩衝區

頂點緩衝區包含網格中每個三角形的頂點清單。 每個頂點在清單中必須是唯一的。 在範例中，立方體有 8 個頂點。 頂點著色器會在圖形裝置上執行並讀取頂點緩衝區的內容，而且會依據您在先前步驟中指定的輸入配置解譯資料。

在下一個範例中，您會提供緩衝區的描述和子資源以告知 Direct3D 關於頂點資料實際對應的一些資訊，以及如何在圖形裝置的記憶體中處理它。 由於您使用的是可包含任何項目的一般 [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351)，因此這是必要的動作。 提供 [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 與 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 結構可確保 Direct3D 了解緩衝區的實體記憶體配置，包括緩衝區中每個頂點元素的大小，以及頂點清單的大小上限。 您也可以在此控制緩衝區記憶體的存取和如何進行周遊，但這不在本教學課程的範圍內。

在設定緩衝區後，您要呼叫 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) 才能實際建立它。 顯而易見地，如果您有多個的物件，就必須為每個不同的模型建立緩衝區。

宣告與建立頂點緩衝區。

```cpp
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
m_d3dDevice->CreateBuffer(
                &vertexBufferDesc,
                &vertexBufferData,
                &vertexBuffer);
```

頂點已載入。 但處理這些頂點的順序為何？ 這會在您提供索引清單給頂點時處理—這些索引的順序便是頂點著色器處理它們的順序。

### <a name="step-4-populate-the-index-buffers"></a>步驟 4：填入索引緩衝區

現在，您要提供包含每個頂點的索引清單。 這些索引會與頂點緩衝區中的頂點位置對應，由 0 開始。 為協助您視覺化呈現此概念，請想像網格中每個唯一頂點都被指派了唯一號碼，就像識別碼一樣。 此識別碼是頂點緩衝區中頂點的整數位置。

![具有八個已編號頂點的立方體](images/cube-mesh-1.png)

在立方體範例中，您有 8 個頂點，每一面會建立 6 個象限。 您將象限分割為三角形，總共有 12 個三角形使用我們的 8 個頂點。 因為每個三角形有 3 個頂點，所以我們的索引緩衝區共有 36 個項目。 在範例中，此索引樣式稱為三角形清單，而當您設定基本拓撲時，您會在 Direct3D 中將它指示為 **D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLELIST**。

以這種方式列出索引非常沒有效率，因為三角形共用點和邊時會產生多餘的索引元素。 例如，當三角形共用菱形的某一邊時，您會為 4 個頂點列出 6 個索引，就像這樣：

![建構菱形時的索引順序](images/rhombus-surface-1.png)

-   三角形 1：\[0, 1, 2\]
-   三角形 2：\[0, 2, 3\]

在條形或扇形的拓撲中，您會在排列頂點順序時排除周遊期間多餘的邊 (如影像中索引 0 到索引 2 的邊)。對於大型的網格而言，這會大幅減少執行頂點著色器的次數，並能大幅改善效能。 不過，我們將持續以最簡單的方式說明三角形清單。

將頂點緩衝區的索引宣告為簡單的三角形清單拓撲。

```cpp
unsigned short cubeIndices[] =
{   0, 1, 2,
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
    0, 4, 7 };
```

當您只有 8 個頂點時，緩衝區中 36 個索引元素的數目就顯得太多。 如果您選擇排除部分多餘的索引元素並使用不同的頂點清單類型 (如條形或扇形)，則必須在將特定的 [**D3D11\_PRIMITIVE\_TOPOLOGY**](https://msdn.microsoft.com/library/windows/desktop/ff476189) 值提供給 [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) 方法時指定該類型。

如需不同索引清單技巧的詳細資訊，請參閱[基本拓撲](https://msdn.microsoft.com/library/windows/desktop/bb205124)。

### <a name="step-5-create-a-constant-buffer-for-your-transformation-matrices"></a>步驟 5：為您的轉換矩陣建立常數緩衝區

在開始處理頂點之前，您必須先提供執行時將套用 (相乘) 到每個頂點的轉換矩陣。 對於大多數的 3D 遊戲而言，它們共有三種：

-   從物件 (模型) 座標系統轉換成整體世界座標系統的 4x4 矩陣。
-   從世界座標系統轉換成相機 (視圖) 座標系統的 4x4 矩陣。
-   從相機座標系統轉換成 2D 檢視投影座標系統的 4x4 矩陣。

這些矩陣會傳送到 *「常數緩衝區」* 中的著色器。 常數緩衝區是會在著色器管線下一階段的執行期間維持一致的記憶體區域，著色器可從您的 HLSL 程式碼直接存取它。 您要為每個常數緩衝區定義兩次：第一次是在遊戲的 C++ 程式碼中，而 (至少) 一次是在您著色器程式碼的類 C 程式語言的 HLSL 語法中。 兩種宣告在類型和資料對齊方面都必須直接對應。 當著色器使用 HLSL 宣告來解譯以 C++ 宣告的資料，但類型不符或資料對齊不一致時，這種錯誤很容易出現，但又不容易被發現。

HLSL 不會變更常數緩衝區。 您可以在遊戲更新特定資料時變更它們。 遊戲開發人員通常會建立 4 種類別的常數緩衝區：一種用於依畫面更新；一種用於依模型/物件更新；一種用於依遊戲狀態重新整理更新；還有一種用於遊戲生命週期內均未曾變更的資料。

在此範例中，我們只使用未曾變更資料的類型：三個矩陣的 DirectX::XMFLOAT4X4 資料。

> **注意：** 此處提供的範例程式碼使用以行為主的矩陣。 若要改用以列為主的矩陣，您可以在 HLSL 中使用 **row\_major** 關鍵字，並確定您的原始矩陣資料也是以列為主。 DirectXMath 使用以列為主的矩陣，並且可以直接搭配以 **row\_major** 關鍵字定義的 HLSL 矩陣使用。

 

為您用於轉換每個頂點的三個矩陣宣告和建立常數緩衝區。

```cpp
struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};
ComPtr<ID3D11Buffer> m_constantBuffer;
ConstantBuffer m_constantBufferData;

// ...

// Create a constant buffer for passing model, view, and projection matrices
// to the vertex shader.  This allows us to rotate the cube and apply
// a perspective projection to it.

D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags = 0;
constantBufferDesc.MiscFlags = 0;
constantBufferDesc.StructureByteStride = 0;
m_d3dDevice->CreateBuffer(
                &constantBufferDesc,
                nullptr,
                &m_constantBuffer
             );

m_constantBufferData.model = DirectX::XMFLOAT4X4( // Identity matrix, since you are not animating the object
            1.0f, 0.0f, 0.0f, 0.0f,
            0.0f, 1.0f, 0.0f, 0.0f,
            0.0f, 0.0f, 1.0f, 0.0f,
            0.0f, 0.0f, 0.0f, 1.0f);

);
// Specify the view (camera) transform corresponding to a camera position of
// X = 0, Y = 1, Z = 2.  

m_constantBufferData.view = DirectX::XMFLOAT4X4(
            -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
             0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
             0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
             0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f);
```

> **注意：** 您通常會宣告投影矩陣當您設定裝置特定的資源，因為與其相乘的結果必須符合目前的 2d 檢視區大小參數 (這通常與像素高度與寬度對應顯示）。 如果變更了值，您也必須適當地調整 x 與 y 座標值。

 

```cpp
// Finally, update the constant buffer perspective projection parameters
// to account for the size of the application window.  In this sample,
// the parameters are fixed to a 70-degree field of view, with a depth
// range of 0.01 to 100.  

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

到了這裡，請在[ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476149) 上設定頂點和索引緩衝區，還有您正在使用的拓撲。

```cpp
// Set the vertex and index buffers, and specify the way they define geometry.
UINT stride = sizeof(SimpleCubeVertex);
UINT offset = 0;
m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset);

m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0);

 m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
```

好！ 輸入組件完成。 轉譯的一切準備工作都已就緒。 讓我們開始執行這個頂點著色器。

### <a name="step-6-process-the-mesh-with-the-vertex-shader"></a>步驟 6：使用頂點著色器處理網格

有了定義網格頂點的頂點緩衝區以及定義處理頂點之順序的索引緩衝區之後，您要將它們傳送給頂點著色器。 頂點著色器程式碼 (以編譯的高階著色器語言表示) 會針對頂點緩衝區中的每個頂點執行一次，讓您可以執行每一頂點的轉換。 最終的結果通常是 2D 投影。

(您是否已載入頂點著色器？ 如果尚未載入，請檢閱[如何在您的 DirectX 遊戲中載入資源](load-a-game-asset.md)。)

在這裡，您要建立頂點著色器...

``` syntax
// Set the vertex and pixel shader stage state.
m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0);
```

...並設定常數緩衝區。

``` syntax
m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf());
```

下列是處理從物件座標轉換成世界座標，再轉換成 2D 檢視投影座標系統的頂點著色器程式碼。 您也會在每個頂點套用一些簡單的光源以美化結果。 將以下程式碼納入您頂點著色器的 HLSL 檔案 (此範例中為 SimplerVertexShader.hlsl) 中。

``` syntax
cbuffer simpleConstantBuffer : register( b0 )
{
    matrix model;
    matrix view;
    matrix projection;
};

struct VertexShaderInput
{
    DirectX::XMFLOAT3 pos : POSITION;
    DirectX::XMFLOAT3 color : COLOR;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
    float4 color : COLOR;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projection space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    vertexShaderOutput.pos = pos;

    // Pass the vertex color through to the pixel shader.
    vertexShaderOutput.color = float4(input.color, 1.0f);

    return vertexShaderOutput;
}
```

有看到最上方的 **cbuffer** 嗎？ 那是與我們先前在 C++ 程式碼中宣告的常數緩衝區類似的 HLSL。 也看到 **VertexShaderInputstruct** 嗎？ 它看來就像是您的輸入配置和頂點資料宣告！ 您 C++ 程式碼中的常數緩衝區和頂點資料宣告必須符合 HLSL 程式碼中的宣告，這非常重要—而那包含了簽章、類型及資料對齊。

**PixelShaderInput** 會指定頂點著色器的主要函式所傳回的資料配置。 當處理完頂點時，將會傳回 2D 投影空間中的頂點位置，以及用於每一頂點光源的色彩。 圖形卡使用著色器輸出的資料來計算在管線的下一階段執行像素著色器時必須著色的「片段」(可能的像素)。

### <a name="step-7-passing-the-mesh-through-the-pixel-shader"></a>步驟 7：透過像素著色器傳送網格

通常在圖形管線的這個階段中，您要在物件可見的投影表面執行每一像素作業。 (大家都愛紋理)。但在這個範例中，您只要透過此階段傳送它。

首先，先建立像素著色器的執行個體。 像素著色器會針對您場景中 2D 投影的每個像素執行，並指派一種色彩給該像素。 在此例中，我們會直接傳送頂點著色器傳回的像素色彩。

設定像素著色器。

``` syntax
m_d3dDeviceContext->PSSetShader( pixelShader.Get(), nullptr, 0 );
```

在 HLSL 中定義通道像素著色器。

``` syntax
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

將此程式碼放在不同於頂點著色器 HLSL (如 SimplePixelShader.hlsl) 的 HLSL 檔案中。 此程式碼會針對檢視區 (您要繪製的畫面區域的記憶體內部表示法) 中的每個可見像素執行一次。在此例中，檢視區對應整個畫面。 現在，您的圖形管線已經完整定義了！

### <a name="step-8-rasterizing-and-displaying-the-mesh"></a>步驟8：點陣化和顯示網格

我們來執行管線吧。 很簡單：呼叫 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/bb173565)。

繪製該立方體！

```cpp
// Draw the cube.
m_d3dDeviceContext->DrawIndexed( ARRAYSIZE(cubeIndices), 0, 0 );
            
```

在圖形卡內，每個頂點會依照您在索引緩衝區中指定的順序處理。 在您的程式碼執行頂點著色器並定義 2D 片段後，就會叫用像素著色器，並將三角形著色。

現在，將立方體放到畫面。

將框架緩衝區顯示到顯示器。

```cpp
// Present the rendered image to the window.  Because the maximum frame latency is set to 1,
// the render loop is generally  throttled to the screen refresh rate, typically around
// 60 Hz, by sleeping the app on Present until the screen is refreshed.

m_swapChain->Present(1, 0);
```

大功告成！ 如需含大量模型的場景，請使用多個頂點和索引緩衝區，甚至可以在不同的模型類型使用不同的著色器。 請記住，每個模型都有自己的座標系統，您必須使用您在常數緩衝區中定義的矩陣，將它們轉換成共用的世界座標系統。

## <a name="remarks"></a>備註

本主題說明如何建立和顯示您自己建立的簡單幾何圖形。 如需從檔案載入更複雜的幾何圖形並將其轉換成範例特定的頂點緩衝區物件 (.vbo) 格式的詳細資訊，請參閱[如何在您的 DirectX 遊戲中載入資源](load-a-game-asset.md)。  

 

## <a name="related-topics"></a>相關主題


* [如何在您的 DirectX 遊戲中載入資源](load-a-game-asset.md)

 

 




