---
author: mtoepke
title: 將紋理套用到基本型別
description: 以下我們將使用「在基本型別上使用深度和效果」中建立的立方體，以載入原始紋理資料並將該資料套用到 3D 基本型別。
ms.assetid: aeed09e3-c47a-4dd9-d0e8-d1b8bdd7e9b4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 紋理, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 252613bbea7f4cdb720758d3435cf0920dd93efa
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5870629"
---
# <a name="apply-textures-to-primitives"></a>將紋理套用到原始物件



以下我們將使用[在基本型別上使用深度和效果](using-depth-and-effects-on-primitives.md)中建立的立方體，以載入原始紋理資料並將該資料套用到 3D 基本型別。 我們還會介紹一個簡單的內積光源模型，其中立方體的表面會根據它們與光源的相對距離和角度呈現較淺或較深的顏色。

**目標：** 將紋理套用到基本型別。

## <a name="prerequisites"></a>先決條件


我們假設您熟悉 C++。 您還需要圖形程式設計概念的基本經驗。

我們也假設您已經看過[快速入門：設定 DirectX 資源及顯示影像](setting-up-directx-resources.md)、[建立著色器及繪製基本型別](creating-shaders-and-drawing-primitives.md)，以及[在基本型別上使用深度和效果](using-depth-and-effects-on-primitives.md)。

**完成所需的時間：** 20 分鐘。

<a name="instructions"></a>指示
------------

### <a name="1-defining-variables-for-a-textured-cube"></a>1. 為紋理立方體定義變數

首先，我們需要為紋理立方體定義 **BasicVertex** 和 **ConstantBuffer** 結構。 這些結構指定了立方體的頂點位置、方向及紋理，以及檢視立方體的方式。 否則，我們就需要使用類似先前[在基本型別上使用深度和效果](using-depth-and-effects-on-primitives.md)教學課程中的方式宣告變數。

```cpp
struct BasicVertex
{
    DirectX::XMFLOAT3 pos;  // Position
    DirectX::XMFLOAT3 norm; // Surface normal vector
    DirectX::XMFLOAT2 tex;  // Texture coordinate
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

### <a name="2-creating-vertex-and-pixel-shaders-with-surface-and-texture-elements"></a>2. 使用表面和紋理元素建立頂點和像素著色器

以下我們將建立比先前 [在基本型別上使用深度和效果](using-depth-and-effects-on-primitives.md)教學課程中還要複雜的頂點和像素著色器。 這個 app 的頂點著色器會將每個頂點位置轉換成投影空間，並將頂點紋理座標傳遞給像素著色器。

描述頂點著色器程式碼配置的 App [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) 結構陣列具有三個配置元素：一個元素定義頂點位置，另一個元素定義表面法線向量 (表面一般面對的方向)，而第三個元素定義紋理座標。

我們會建立頂點緩衝區、索引緩衝區及常數緩衝區來定義環繞移動的紋理立方體。

**定義環繞移動的紋理立方體**

1.  首先，我們定義立方體。 每個頂點都會被指派一個位置、表面法線向量及紋理座標。 我們會針對每個角落使用多個頂點，以允許為每個面定義不同的法線向量和紋理座標。
2.  接著，我們會使用立方體定義來描述頂點和索引緩衝區 ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) 和 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220))。 我們會為每個緩衝區呼叫 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) 一次。
3.  接著，我們會建立常數緩衝區 ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)) 來傳遞模型、檢視及投影陣列給頂點著色器。 我們可以稍後使用常數緩衝區來旋轉立方體，並在立方體上套用透視投影。 我們會呼叫 [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) 來建立常數緩衝區。
4.  最後，我們會指定對應 X = 0、Y = 1、Z = 2 相機位置的檢視轉換。

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
          // These correspond to the elements of the BasicVertex struct defined above.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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
          // multiple vertices are used for each corner to allow different normal vectors and
          // texture coordinates to be defined for each face.
          BasicVertex cubeVertices[] =
          {
              { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Y (top face)
              { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Y (bottom face)
              { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +X (right face)
              { DirectX::XMFLOAT3(0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -X (left face)
              { DirectX::XMFLOAT3(-0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Z (front face)
              { DirectX::XMFLOAT3( 0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3( 0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Z (back face)
              { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },
          };

          unsigned short cubeIndices[] =
          {
              0, 1, 2,
              0, 2, 3,

              4, 5, 6,
              4, 6, 7,

              8, 9, 10,
              8, 10, 11,

              12, 13, 14,
              12, 14, 15,

              16, 17, 18,
              16, 18, 19,

              20, 21, 22,
              20, 22, 23
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(BasicVertex) * ARRAYSIZE(cubeVertices);
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
```

### <a name="3-creating-textures-and-samplers"></a>3. 建立紋理和取樣器

以下我們會將紋理資料套用到立方體，而不是像在先前[在基本型別上使用深度和效果](using-depth-and-effects-on-primitives.md)教學課程中那樣套用色彩。

我們會使用原始紋理資料來建立紋理。

**建立紋理和取樣器**

1.  首先，我們會從磁碟上的 texturedata.bin 檔案讀取原始紋理資料。
2.  接著，我們會建構參考該原始紋理資料的 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 結構。
3.  然後，我們會填入 [**D3D11\_TEXTURE2D\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476253) 結構來描述紋理。 再接著，會在 [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521) 呼叫中傳遞 [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) 和 **D3D11\_TEXTURE2D\_DESC** 結構來建立紋理。
4.  接著，我們會建立紋理的著色器資源檢視，以便讓著色器能夠使用紋理。 為了建立著色器資源檢視，我們會填入一個 [**D3D11\_SHADER\_RESOURCE\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476211) 來描述著色器資源檢視，並將著色器資源檢視描述和紋理傳遞給 [**ID3D11Device::CreateShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476519)。 一般而言，您會讓檢視描述與紋理描述相符。
5.  接著，我們會建立紋理的取樣器狀態。 這個取樣器狀態會使用相關的紋理資料，以定義如何判斷特定紋理座標的色彩。 我們會填入 [**D3D11\_SAMPLER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476207) 結構來描述取樣器狀態。 然後我們會在 [**ID3D11Device::CreateSamplerState**](https://msdn.microsoft.com/library/windows/desktop/ff476518) 呼叫中傳遞 **D3D11\_SAMPLER\_DESC** 結構來建立取樣器狀態。
6.  最後，我們會宣告一個 *degree* 變數，我們將使用這個變數藉由在每個畫面旋轉立方體來製作動畫。

```cpp
        
        // Load the raw texture data from disk and construct a subresource description that references it.
        auto loadTDTask = DX::ReadDataAsync(L"texturedata.bin");
          
        auto constructSubresourceTask = loadTDTask.then([this](const std::vector<byte>& vertexShaderBytecode) {  
        
          D3D11_SUBRESOURCE_DATA textureSubresourceData = {0};
          textureSubresourceData.pSysMem = textureData->Data;

          // Specify the size of a row in bytes, known as a priori about the texture data.
          textureSubresourceData.SysMemPitch = 1024;

          // As this is not a texture array or 3D texture, this parameter is ignored.
          textureSubresourceData.SysMemSlicePitch = 0;

          // Create a texture description from information known as a priori about the data.
          // Generalized texture loading code can be found in the Resource Loading sample.
          D3D11_TEXTURE2D_DESC textureDesc = {0};
          textureDesc.Width = 256;
          textureDesc.Height = 256;
          textureDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
          textureDesc.Usage = D3D11_USAGE_DEFAULT;
          textureDesc.CPUAccessFlags = 0;
          textureDesc.MiscFlags = 0;

          // Most textures contain more than one MIP level.  For simplicity, this sample uses only one.
          textureDesc.MipLevels = 1;

          // As this will not be a texture array, this parameter is ignored.
          textureDesc.ArraySize = 1;

          // Don't use multi-sampling.
          textureDesc.SampleDesc.Count = 1;
          textureDesc.SampleDesc.Quality = 0;

          // Allow the texture to be bound as a shader resource.
          textureDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE;

          ComPtr<ID3D11Texture2D> texture;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateTexture2D(
                  &textureDesc,
                  &textureSubresourceData,
                  &texture
                  )
              );

          // Once the texture is created, we must create a shader resource view of it
          // so that shaders may use it.  In general, the view description will match
          // the texture description.
          D3D11_SHADER_RESOURCE_VIEW_DESC textureViewDesc;
          ZeroMemory(&textureViewDesc, sizeof(textureViewDesc));
          textureViewDesc.Format = textureDesc.Format;
          textureViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
          textureViewDesc.Texture2D.MipLevels = textureDesc.MipLevels;
          textureViewDesc.Texture2D.MostDetailedMip = 0;

          ComPtr<ID3D11ShaderResourceView> textureView;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateShaderResourceView(
                  texture.Get(),
                  &textureViewDesc,
                  &textureView
                  )
              );

          // Once the texture view is created, create a sampler.  This defines how the color
          // for a particular texture coordinate is determined using the relevant texture data.
          D3D11_SAMPLER_DESC samplerDesc;
          ZeroMemory(&samplerDesc, sizeof(samplerDesc));

          samplerDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;

          // The sampler does not use anisotropic filtering, so this parameter is ignored.
          samplerDesc.MaxAnisotropy = 0;

          // Specify how texture coordinates outside of the range 0..1 are resolved.
          samplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
          samplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
          samplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;

          // Use no special MIP clamping or bias.
          samplerDesc.MipLODBias = 0.0f;
          samplerDesc.MinLOD = 0;
          samplerDesc.MaxLOD = D3D11_FLOAT32_MAX;

          // Don't use a comparison function.
          samplerDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;

          // Border address mode is not used, so this parameter is ignored.
          samplerDesc.BorderColor[0] = 0.0f;
          samplerDesc.BorderColor[1] = 0.0f;
          samplerDesc.BorderColor[2] = 0.0f;
          samplerDesc.BorderColor[3] = 0.0f;

          ComPtr<ID3D11SamplerState> sampler;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateSamplerState(
                  &samplerDesc,
                  &sampler
                  )
              );
        });

        // This value will be used to animate the cube by rotating it every frame;
        float degree = 0.0f;
```

### <a name="4-rotating-and-drawing-the-textured-cube-and-presenting-the-rendered-image"></a>4. 旋轉及繪製紋理立方體並呈現轉譯的影像

如先前教學課程所示，我們會進入一個無限迴圈來不斷轉譯並顯示場景。 我們會呼叫 **rotationY** 內嵌函式 (BasicMath.h) 搭配一個旋轉量，以設定將會把立方體的模型矩陣繞著 Y 軸旋轉的值。 然後我們會呼叫 [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) 來更新常數緩衝區並旋轉立方體模型。 接著，我們會呼叫 [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) 來指定轉譯目標和深度樣板檢視。 我們會呼叫 [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) 來將轉譯目標清除成純藍色，並且呼叫 [**ID3D11DeviceContext::ClearDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476387) 來清除深度緩衝區。

在無限迴圈中，我們也會在藍色表面上繪製紋理立方體。

**繪製紋理立方體**

1.  首先，我們會呼叫 [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) 來描述頂點緩衝區資料如何串流處理到輸入組合器階段。
2.  接著，我們會呼叫 [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) 和 [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) 來將頂點和索引緩衝區繫結到輸入組合階段。
3.  接著，我們會呼叫 [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) 搭配要為輸入組合階段指定的 [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) 值，以將頂點資料解譯為三角形區域。
4.  接著，我們會呼叫 [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) 以使用頂點著色器程式碼起始頂點著色階段，並且呼叫 [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) 以使用像素著色器程式碼起始像素著色階段。
5.  接著，我們會呼叫 [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) 來設定頂點著色器管線階段所使用的常數緩衝區。
6.  接著，我們會呼叫 [**PSSetShaderResources**](https://msdn.microsoft.com/library/windows/desktop/ff476473) 來將紋理的著色器資源檢視繫結到像素著色器管線階段。
7.  接著，我們會呼叫 [**PSSetSamplers**](https://msdn.microsoft.com/library/windows/desktop/ff476471) 來將取樣器狀態設成像素著色器管線階段。
8.  最後，我們會呼叫 [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) 來繪製立方體並將它提交給轉譯管線。

如先前教學課程中所示，我們會呼叫 [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) 來將轉譯的影像呈現到視窗。

```cpp
            // Update the constant buffer to rotate the cube model.
            m_constantBufferData.model = DirectX::XMMatrixRotationY(-degree);
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
            UINT stride = sizeof(BasicVertex);
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

            m_d3dDeviceContext->PSSetShaderResources(
                0,
                1,
                textureView.GetAddressOf()
                );

            m_d3dDeviceContext->PSSetSamplers(
                0,
                1,
                sampler.GetAddressOf()
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

## <a name="summary"></a>摘要


我們載入了原始紋理資料並將該資料套用到 3D 基本型別。

 

 




