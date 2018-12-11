---
title: 將 OpenGL ES 2.0 緩衝區、Uniform、頂點移植到 Direct3D
description: 在從 OpenGL ES 2.0 移植到 Direct3D 11 的程序期間，您必須變更用來在 app 與著色器程式之間傳送資料的語法與 API 行為。
ms.assetid: 9b215874-6549-80c5-cc70-c97b571c74fe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, opengl, direct3d, buffers, uniforms, vertex attributes, 遊戲, 緩衝區, 統一, 頂點屬性
ms.localizationpriority: medium
ms.openlocfilehash: 9a1db1890e47257412a7e2ee8e08c40164d0d927
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8893180"
---
# <a name="compare-opengl-es-20-buffers-uniforms-and-vertex-attributes-to-direct3d"></a>OpenGL ES 2.0 緩衝區、Uniform 及頂點屬性與 Direct3D 的比較




**重要 API**

-   [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)
-   [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)

在從 OpenGL ES 2.0 移植到 Direct3D 11 的程序期間，您必須變更用來在 app 與著色器程式之間傳送資料的語法與 API 行為。

在 OpenGL ES 2.0 中會使用下列幾種方法，將資料傳入和傳出著色器程式：做為常數資料的 Uniform、做為頂點資料的屬性，以及做為其他資源資料 (例如紋理) 的緩衝區物件。 在 Direct3D 11 中，這些大致上會對應到常數緩衝區、頂點緩衝區及子資源。 儘管表面上有共通性，但在使用上它們的處理方式完全不同。

以下是基本對應。

| OpenGL ES 2.0             | Direct3D 11                                                                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniform                   | 常數緩衝區 (**cbuffer**) 欄位。                                                                                                                                                |
| 屬性                 | 頂點緩衝區元素欄位是由輸入配置所指定，並以特定的 HLSL 語意來標示。                                                                                |
| 緩衝區物件             | 緩衝區；請參閱 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 與 [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)，而且適用於一般用法的緩衝區定義。 |
| 框架緩衝區物件 (FBO) | 轉譯目標；請參閱 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 與 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)。                                       |
| 背景緩衝區               | 含有「背景緩衝區」表面的交換鏈結；請參閱含有附加 [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343) 的 [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)。                       |

 

## <a name="port-buffers"></a>移植緩衝區


在 OpenGL ES 2.0 中，建立並繫結任何種類的緩衝區程序通常會遵循這個模式。

-   呼叫 glGenBuffers 以產生一或多個緩衝區，並將控制代碼傳回給它們。
-   呼叫 glBindBuffer 以定義緩衝區的配置，例如 GL\_ELEMENT\_ARRAY\_BUFFER。
-   呼叫 glBufferData，在特定的配置中使用特定資料 (例如，頂點結構、索引資料或色彩資料) 填入緩衝區。

最常見的緩衝區種類是頂點緩衝區，這在部分座標系統中至少會包含頂點的位置。 在典型的用法中，頂點是使用包含位置座標、頂點位置的法向量、頂點位置的正切函數向量，以及紋理查閱 (uv) 座標的結構來表示。 緩衝區包含這些頂點的連續性清單，並以某種順序排列 (例如，三角形清單、條形或扇形)，而且會在您的場景中共同呈現可見的多邊形 (在 Direct3D 11 以及 OpenGL ES 2.0 中，針對每個繪製呼叫提供多個頂點緩衝區是無效率的)。

這裡提供的範例是使用 OpenGL ES 2.0 建立的頂點緩衝區與索引緩衝區：

OpenGL ES 2.0：建立和填入頂點緩衝區和索引緩衝區。

``` syntax
glGenBuffers(1, &renderer->vertexBuffer);
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertex) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);

glGenBuffers(1, &renderer->indexBuffer);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(int) * CUBE_INDICES, renderer->vertexIndices, GL_STATIC_DRAW);
```

其他緩衝區包含像素緩衝區及地圖，例如紋理。 著色器管線可以轉譯為紋理緩衝區 (Pixmap)，也可以轉譯為緩衝區物件，並在未來的著色器階段使用這些緩衝區。 在最簡單的案例中，呼叫流程如下：

-   呼叫 glGenFramebuffers 以產生框架緩衝區物件。
-   呼叫 glBindFramebuffer 以繫結框架緩衝區物件以供寫入。
-   呼叫 glFramebufferTexture2D 以繪製到特定的紋理圖。

在 Direct3D 11 中，緩衝區資料元素會被視為「子資源」，而涵蓋範圍可從個別的頂點資料元素到 MIP 圖紋理。

-   使用緩衝區資料元素的設定填入 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 結構。
-   使用緩衝區中個別元素的大小以及緩衝區類型填入 [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 結構。
-   使用這兩個結構呼叫 [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575)。

Direct3D 11：建立和填入頂點緩衝區和索引緩衝區。

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
    
```

可寫入的像素緩衝區或地圖 (例如，框架緩衝區) 可建立為 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) 物件。 這些都可以當成資源繫結到 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) 或 [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628)，一旦繪製到其中之後，就可以分別使用相關聯的交換鏈結來顯示，或是傳送到著色器。

Direct3D 11：建立框架緩衝區物件。

``` syntax
ComPtr<ID3D11RenderTargetView> m_d3dRenderTargetViewWin;
// ...
ComPtr<ID3D11Texture2D> frameBuffer;

m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&frameBuffer));
m_d3dDevice->CreateRenderTargetView(
  frameBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

## <a name="change-uniforms-and-uniform-buffer-objects-to-direct3d-constant-buffers"></a>將 Uniform 與 Uniform 緩衝區物件變更為 Direct3D 常數緩衝區


在 Open GL ES 2.0 中，Uniform 是提供常數資料給個別著色器程式的機制。 著色器無法變更這個資料。

設定 Uniform 通常包含使用 GPU 中的上傳位置以及應用程式記憶體中資料的指標，以提供某一個 glUniform\* 方法。 在 glUniform\* 方法執行之後，Uniform 資料便會位於 GPU 記憶體，並能讓已宣告該 Uniform 的著色器存取。 您必須確定資料的封裝方式，是可讓著色器基於著色器中的 Uniform 宣告來解譯資料 (透過使用相容的類型)。

OpenGL ES 2.0 建立 Uniform 並將資料上傳給它

``` syntax
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");

// ...

glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);
```

在著色器的 GLSL 中，對應的 Uniform 宣告看起來像這樣：

Open GL ES 2.0：GLSL Uniform 宣告

``` syntax
uniform mat4 u_mvpMatrix;
```

Direct3D 指定 Uniform 資料做為「常數緩衝區」，與 Uniform 一樣，其中包含提供給個別著色器的常數資料。 至於 Uniform 緩衝區，請務必在記憶體中使用與著色器預期用來解譯它的相同方法來封裝常數緩衝區資料。 使用 DirectXMath 類型 (例如 [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608)) 而非平台類型 (例如 **float\*** 或 **float\[4\]**) 來保證資料元素會適當地對齊。

常數緩衝區必須有一個關聯的 GPU 暫存器，以用來在 GPU 上參考該資料。 資料會封裝到緩衝區配置所指定的暫存器位置。

Direct3D 11：建立常數緩衝區並上傳資料給它

``` syntax
struct ModelViewProjectionConstantBuffer
{
     DirectX::XMFLOAT4X4 mvp;
};

// ...

ModelViewProjectionConstantBuffer   m_constantBufferData;

// ...

XMStoreFloat4x4(&m_constantBufferData.mvp, mvpMatrix);

CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);
m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);
```

在著色器的 HLSL 中，對應的常數緩衝區宣告看起來像這樣：

Direct3D 11：常數緩衝區 HLSL 宣告

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

請注意，暫存器必須針對每個常數緩衝區進行宣告。 不同的 Direct3D 功能層級會有不同的可用暫存器數目上限，因此請勿超過您設定為目標的最低功能層級的數目上限。

## <a name="port-vertex-attributes-to-a-direct3d-input-layouts-and-hlsl-semantics"></a>將頂點屬性移植到 Direct3D 輸入配置與 HLSL 語意


由於著色器管線可以修改頂點資料，因此 OpenGL ES 2.0 要求您將它們指定為「屬性」而非 "Uniform" (這在較新版本的 OpenGL 與 GLSL 中已經變更)。頂點特定的資料 (例如，頂點位置、法向量、正切函數及色彩值) 都會當成屬性值來提供給著色器。 這些屬性值會對應到頂點資料中每個元素的特定位移；例如，第一個屬性可以指向個別頂點的位置元件，而第二個指向法向量，依此類推。

將頂點緩衝區資料從主要記憶體移至 GPU 的基本程序看起來像這樣：

-   使用 glBindBuffer 上傳頂點資料。
-   使用 glGetAttribLocation 取得 GPU 上屬性的位置。 針對頂點資料元素中的每個屬性呼叫它。
-   呼叫 glVertexAttribPointer，在個別頂點資料元素內部設定正確的屬性大小與位移。 針對每個屬性執行此動作。
-   使用 glEnableVertexAttribArray 啟用頂點資料配置資訊。

OpenGL ES 2.0：將頂點緩衝區資料上傳到著色器屬性

``` syntax
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->vertexBuffer);
loc = glGetAttribLocation(renderer->programObject, "a_position");

glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), 0);
loc = glGetAttribLocation(renderer->programObject, "a_color");
glEnableVertexAttribArray(loc);

glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);
```

現在，在頂點著色器中，以您在對 glGetAttribLocation 的呼叫中所定義的相同名稱來宣告屬性。

OpenGL ES 2.0：在 GLSL 中宣告屬性

``` syntax
attribute vec4 a_position;
attribute vec4 a_color;                     
```

從某些方面來說，這個相同程序適用於 Direct3D。 輸入緩衝區中提供的是頂點資料，而不是屬性，輸入緩衝區中包含頂點緩衝區及對應的索引緩衝區。 但是，由於 Direct3D 沒有「屬性」宣告，因此，您必須指定輸入配置，在頂點緩衝區與 HLSL 語意中宣告資料元素的個別元件，語意中會指出頂點著色器要在何處及如何解譯這些元件。 HLSL 語意要求您使用特定的字串來定義每個元件的用法，以通知著色器引擎它的相關用途。 例如，頂點位置資料會標示為 POSITION、法向量資料會標示為 NORMAL，而頂點色彩資料會標示為 COLOR (其他著色器階段也需要特定的語意，而那些語意會根據著色器階段而有不同的解譯方式)。如需關於 HLSL 語意的詳細資料，請參閱[移植您的著色器管線](change-your-shader-loading-code.md)與 [HLSL 語意](https://msdn.microsoft.com/library/windows/desktop/bb205574)。

總括來說，設定頂點與索引緩衝區以及設定輸入配置的程序稱為 Direct3D 圖形管線的「輸入組件」(IA) 階段。

Direct3D 11：設定輸入組件階段

``` syntax
// Set up the IA stage corresponding to the current draw operation.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset);

m_d3dContext->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT,
        0);

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

您可以藉由宣告頂點資料元素的格式與每個元件所使用的語意，來宣告輸入配置並與頂點著色器產生關聯。 如同 D3D11\_INPUT\_ELEMENT\_DESC 中所述，您建立的頂點元素資料配置必須對應到對應結構的配置。 您可以在此處建立含有兩個元件的頂點資料配置：

-   頂點位置座標 (在主記憶體中以 XMFLOAT3 來表示)，這是 (x, y, z) 座標的 3 個 32 位元浮點數值的對齊陣列。
-   頂點色彩值 (以 XMFLOAT4 來表示)，這是色彩 (RGBA) 的 4 個 32 位元浮點數值的對齊陣列。

您需為每一個元件指派語意以及格式類型。 然後將描述傳送到 [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)。 當您在執行轉譯方法期間設定輸入組件時，我們會在呼叫 [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) 時使用輸入配置。

Direct3D 11：使用特定語意說明輸入配置

``` syntax
ComPtr<ID3D11InputLayout> m_inputLayout;

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileData->Length,
  &m_inputLayout);

// ...
// When we start the drawing process...

m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

最後，您需要確定著色器能夠藉由宣告輸入來了解輸入資料。 您在配置中指派的語意是用來選取 GPU 記憶體中的正確位置。

Direct3D 11：使用 HLSL 語意宣告著色器輸入資料

``` syntax
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};
```

 

 




