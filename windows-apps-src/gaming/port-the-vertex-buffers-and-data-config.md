---
title: 移植頂點緩衝區與資料
description: 在這個步驟中，您將定義包含網格的頂點緩衝區，以及允許著色器以特定順序周遊頂點的索引緩衝區。
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, port, vertex buffers, data, direct3d, 遊戲, 連接埠, 頂點緩衝區, 資料, 移植
ms.localizationpriority: medium
ms.openlocfilehash: 4c961a8852fb1e03e4e86209f62bda821b980f8c
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8896956"
---
# <a name="port-the-vertex-buffers-and-data"></a>移植頂點緩衝區與資料




**重要 API**

-   [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)
-   [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588)

在這個步驟中，您將定義包含網格的頂點緩衝區，以及允許著色器以特定順序周遊頂點的索引緩衝區。

這時來檢視我們所使用的立方體網格的硬體編碼模型。 這兩種表示法都會將頂點組成三角形清單 (相較於帶狀線或其他更有效率的三角形配置)。 這兩種表示法中的所有頂點也都具有相關聯的索引與色彩值。 本主題中大部分的 Direct3D 程式碼會參考 Direct3D 專案中定義的變數與物件。

這裡是可供 OpenGL ES 2.0 處理的立方體。 在範例實作中，每個頂點都是 7 個浮點數值：3 個位置座標，後面接著 4 個 RGBA 色彩值。

```cpp
#define CUBE_INDICES 36
#define CUBE_VERTICES 8

GLfloat cubeVertsAndColors[] = 
{
  -0.5f, -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, 1.0f,
  -0.5f, -0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f,
  -0.5f,  0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 1.0f,
  -0.5f,  0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 1.0f,
  0.5f, -0.5f,  0.5f, 1.0f, 0.0f, 1.0f, 1.0f,
  0.5f, -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, 1.0f,  
  0.5f,  0.5f,  0.5f, 1.0f, 1.0f, 1.0f, 1.0f,
  0.5f,  0.5f, -0.5f, 1.0f, 1.0f, 0.0f, 1.0f
};

GLuint cubeIndices[] = 
{
  0, 1, 2, // -x
  1, 3, 2,

  4, 6, 5, // +x
  6, 7, 5,

  0, 5, 1, // -y
  5, 6, 1,

  2, 6, 3, // +y
  6, 7, 3,

  0, 4, 2, // +z
  4, 6, 2,

  1, 7, 3, // -z
  5, 7, 1
};
```

而這裡是可供 Direct3D 11 處理的相同立方體。

```cpp
VertexPositionColor cubeVerticesAndColors[] = 
// struct format is position, color
{
  {XMFLOAT3(-0.5f, -0.5f, -0.5f), XMFLOAT3(0.0f, 0.0f, 0.0f)},
  {XMFLOAT3(-0.5f, -0.5f,  0.5f), XMFLOAT3(0.0f, 0.0f, 1.0f)},
  {XMFLOAT3(-0.5f,  0.5f, -0.5f), XMFLOAT3(0.0f, 1.0f, 0.0f)},
  {XMFLOAT3(-0.5f,  0.5f,  0.5f), XMFLOAT3(0.0f, 1.0f, 1.0f)},
  {XMFLOAT3( 0.5f, -0.5f, -0.5f), XMFLOAT3(1.0f, 0.0f, 0.0f)},
  {XMFLOAT3( 0.5f, -0.5f,  0.5f), XMFLOAT3(1.0f, 0.0f, 1.0f)},
  {XMFLOAT3( 0.5f,  0.5f, -0.5f), XMFLOAT3(1.0f, 1.0f, 0.0f)},
  {XMFLOAT3( 0.5f,  0.5f,  0.5f), XMFLOAT3(1.0f, 1.0f, 1.0f)},
};
        
unsigned short cubeIndices[] = 
{
  0, 2, 1, // -x
  1, 2, 3,

  4, 5, 6, // +x
  5, 7, 6,

  0, 1, 5, // -y
  0, 5, 4,

  2, 6, 7, // +y
  2, 7, 3,

  0, 4, 6, // -z
  0, 6, 2,

  1, 3, 7, // +z
  1, 7, 5
};
```

檢閱這個程式碼時，您會注意到 OpenGL ES 2.0 程式碼中的立方體是以右旋座標系統代表，而 Direct3D 特定程式碼中的立方體是以左旋座標系統代表。 匯入您自己的網格資料時，必須針對您的模型來反轉 z 軸座標，並據此變更每個網格的索引，以根據座標系統的變更來周遊三角形。

假設已順利將立方體網格從右旋的 OpenGL ES 2.0 座標系統移至左旋的 Direct3D 座標系統，讓我們來查看如何載入立方體資料以便在這兩個模型中進行處理。

## <a name="instructions"></a>指示

### <a name="step-1-create-an-input-layout"></a>步驟 1：建立輸入配置

在 OpenGL ES 2.0 中，頂點資料是做為屬性提供，且將提供給著色器物件並由其讀取。 您通常會將包含屬性名稱 (用於著色器 GLSL) 的字串提供給著色器程式物件，並取回您可提供給著色器的記憶體位置。 在這個範例中，頂點緩衝區物件包含自訂頂點結構的清單，其定義與格式化方式如下：

OpenGL ES 2.0：設定包含每個頂點資訊的屬性。

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

在 OpenGL ES 2.0 中，輸入配置是隱含的；您可以設定一般用途的 GL\_ELEMENT\_ARRAY\_BUFFER，並提供分散與位移，如此頂點著色器便能在上傳資料之後加以解譯。 您會在轉譯之前通知著色器，哪些屬性會對應到每個含 **glVertexAttribPointer** 的頂點資料區塊的哪些部分。

在 Direct3D 中，當您建立緩衝區時 (而不是在繪製幾何之前)，必須提供輸入配置，以說明頂點緩衝區中頂點資料的結構。 若要這樣做，您可以使用輸入配置，而這個配置會對應到記憶體中個別頂點的資料配置。 請務必準確指定此資訊！

您可以在此處建立輸入描述，以做為 [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 結構的陣列。

Direct3D：定義輸入配置描述。

``` syntax
struct VertexPositionColor
{
  DirectX::XMFLOAT3 pos;
  DirectX::XMFLOAT3 color;
};

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

```

這個輸入描述會將頂點定義為一對 2 個 3 座標的向量：一個 3D 向量用於儲存模型座標中頂點的位置，而另一個 3D 向量用於儲存與頂點相關聯的 RGB 色彩值。 您在這個案例中使用 3x32 位元的浮點數格式，我們在程式碼中會以 `XMFLOAT3(X.Xf, X.Xf, X.Xf)` 來表示其中的元素。 每當您處理著色器將使用的資料時，應該使用來自 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415574) 程式庫的類型，因為它可確保會適當封裝與對齊該資料 (例如，針對向量資料使用 [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475) 或 [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608)，以及針對矩陣使用 [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621))。

如需所有可能的格式類型清單，請參閱 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059)。

您可以使用所定義的每個頂點輸入配置來建立配置物件。 在下列程式碼中，您會將它寫入 **m\_inputLayout**，此為類型 **ComPtr** 的變數 (指向類型 [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575) 的物件)。 **fileData** 包含來自上一個步驟 ([移植著色器](port-the-shader-config.md)) 的已編譯的頂點著色器物件。

Direct3D：建立頂點緩衝區所使用的輸入配置。

``` syntax
Microsoft::WRL::ComPtr<ID3D11InputLayout>      m_inputLayout;

// ...

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileShaderData->Length,
  &m_inputLayout
);
```

我們已定義輸入配置。 現在來建立使用這個配置的緩衝區，並使用立方體網格資料載入它。

### <a name="step-2-create-and-load-the-vertex-buffers"></a>步驟 2：建立和載入頂點緩衝區

在 OpenGL ES 2.0 中，您會建立一對緩衝區，一個用於位置資料，而另一個用於色彩資料 (您也可以建立一個包含這兩者的結構和一個單一緩衝區)。您繫結每一個緩衝區，並將位置與色彩資料寫入它們。 稍後您會在執行轉譯功能期間再次繫結緩衝區，然後使用緩衝區中的資料格式提供著色器，如此就能正確解譯它。

OpenGL ES 2.0：繫結頂點緩衝區

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

在 Direct3D 中，著色器可存取的緩衝區會以 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 結構來表示。 若要將這個緩衝區的位置繫結到著色器物件，您需要使用 [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) 為每個緩衝區建立 CD3D11\_BUFFER\_DESC 結構，然後呼叫緩衝區類型特定的 Set 方法 (例如 [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)) 來設定 Direct3D 裝置上下文的緩衝區。

當您設定緩衝區時，必須從緩衝區開頭設定分散 (個別頂點的資料元素大小) 以及位移 (頂點資料陣列實際開始之處)。

請注意，我們將 **vertexIndices** 陣列的指標指派給 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 結構的 **pSysMem** 欄位。 如果這不正確，您的網格將會毀損或空白！

Direct3D：建立和設定頂點緩衝區

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
        
// ...

UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
  0,
  1,
  m_vertexBuffer.GetAddressOf(),
  &stride,
  &offset);
```

### <a name="step-3-create-and-load-the-index-buffer"></a>步驟 3：建立和載入索引緩衝區

索引緩衝區是允許頂點著色器查詢個別頂點的有效方式。 儘管它們不是必要項目，但是我們會在這個範例轉譯器中使用它們。 就像 OpenGL ES 2.0 中的頂點緩衝區，我們會建立索引緩衝區並繫結為一般用途的緩衝區，並將您稍早建立的頂點索引複製到其中。

當您準備好進行繪製時，會再次繫結頂點與索引緩衝區，並呼叫 **glDrawElements**。

OpenGL ES 2.0：將索引順序傳送到繪製呼叫。

``` syntax
GLuint indexBuffer;

// ...

glGenBuffers(1, &renderer->indexBuffer);    
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);   
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 
  sizeof(GLuint) * CUBE_INDICES, 
  renderer->vertexIndices, 
  GL_STATIC_DRAW);

// ...
// Drawing function

// Bind the index buffer
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glDrawElements (GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);
```

雖然有點好為人師，不過使用 Direct3D 是有點非常類似的程序。 當您設定 Direct3D 時，為您建立的 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 提供索引緩衝區做為 Direct3D 子資源。 您可以使用針對索引陣列設定的子資源來呼叫 [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588)，藉以執行此動作，如下所示 (再次提醒，請注意，您將 **cubeIndices** 陣列的指標指派給 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 結構的 **pSysMem** 欄位)。

Direct3D：建立索引緩衝區。

``` syntax
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

// ...

m_d3dContext->IASetIndexBuffer(
  m_indexBuffer.Get(),
  DXGI_FORMAT_R16_UINT,
  0);
```

稍後您將呼叫 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) (或 [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407)，適用於未編製索引的頂點) 來繪製三角形，如下所示 (如需詳細資訊，請跳到[繪製到螢幕](draw-to-the-screen.md))。

Direct3D：繪製已編製索引的頂點。

``` syntax
m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());

// ...

m_d3dContext->DrawIndexed(
  m_indexCount,
  0,
  0);
```

## <a name="previous-step"></a>上一步


[移植著色器物件](port-the-shader-config.md)

## <a name="next-step"></a>下一步

[移植 GLSL](port-the-glsl.md)

## <a name="remarks"></a>備註

建構您的 Direct3D 時，請將呼叫 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 上方法的程式碼區分為每當需要重新建立裝置資源時所呼叫的方法 (在 Direct3D 專案範本中，這個程式碼位於轉譯器物件的 **CreateDeviceResource** 方法中)。 另一方面，更新裝置上下文的程式碼 ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) 會放置於 **Render** 方法中，因為這是您實際建構著色器階段和繫結資料的地方。

## <a name="related-topics"></a>相關主題


* [使用方法：將簡單的 OpenGL ES 2.0 轉譯器移植到 Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [移植著色器物件](port-the-shader-config.md)
* [移植頂點緩衝區與資料](port-the-vertex-buffers-and-data-config.md)
* [移植 GLSL](port-the-glsl.md)

 

 




