---
title: 設定
description: 了解如何組合轉譯管線來顯示圖形。 遊戲轉譯、設定及準備資料。
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 轉譯
ms.localizationpriority: medium
ms.openlocfilehash: 1a3f689f86e629ce81946927fa732a3ab692b219
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409507"
---
# <a name="rendering-framework-ii-game-rendering"></a>轉譯架構 II：遊戲轉譯

> [!NOTE]
> 本主題是使用 DirectX 教學課程系列[建立簡單的通用 Windows 平臺（UWP）遊戲](tutorial--create-your-first-uwp-directx-game.md)的一部分。 該連結的主題會設定數列的內容。

在[轉譯架構 I](tutorial--assembling-the-rendering-pipeline.md) 中，我們已討論如何採用場景資訊，然後呈現中顯示畫面中。 現在，我們將回顧以了解如何準備要轉譯的資料。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至[Direct3D 範例遊戲](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

快速複習一下目標重點。 了解如何設定基本轉譯架構，以顯示 UWP DirectX 遊戲的圖形輸出。 我們可以將這些重點彈性分成三個步驟。

 1. 連接到我們的圖形介面
 2. 準備：建立繪製圖形所需的資源
 3. 顯示圖形：轉譯框架

[轉譯架構 I：簡介轉譯](tutorial--assembling-the-rendering-pipeline.md)說明如何轉譯圖形，涵蓋步驟 1 到 3。 

這篇文章說明如何設定此架構的其他部分，以及如何在發生轉譯之前準備所需的資料，也就是此程序的步驟 2。

## <a name="design-the-renderer"></a>設計轉譯

轉譯負責器建立及維護所有用於產生遊戲視覺效果之 D3D11 和 D2D 物件。 __GameRenderer__ 類別是此範例遊戲的轉譯器，其設計目的是為符合遊戲的轉譯需求。

以下是可以應用在設計遊戲轉譯器的一些概念：
* 因為 Direct3D 11 API 會定義為 [COM](/windows/desktop/com/the-component-object-model) API，所以您必須提供這些 API 所定義之物件的 [ComPtr](/cpp/windows/comptr-class) 參考。 當 app 終止時，這些物件會在最後一個參考超出範圍時，自動被釋放。 如需詳細資訊，請參閱 [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr)。 這些物件的範例：常數緩衝區、著色器物件 - [頂點著色器](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders)、[像素著色器](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders)，以及著色器資源物件。
* 常數緩衝區定義於此類別以保留各種轉譯所需的資料。
    * 使用採用不同頻率的多個常數緩衝區，以減少每個畫面必須傳送到 GPU 的資料量。 此範例會根據必須更新的頻率，將常數分成不同的緩衝區。 這是 Direct3D 程式設計的最佳做法。 
    * 在此範例遊戲中，會定義4個常數緩衝區。
        1. __m \_ constantBufferNeverChanges__包含光源參數。 將其在 __FinalizeCreateGameDeviceResources__ 方法中設定一次，之後不再改變。
        2. __m \_ constantBufferChangeOnResize__包含投影矩陣。 投影矩陣取決於視窗的大小和外觀比例。 其設定在 [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) 中，然後在資源載入到 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 方法中之後進行更新。 如果以 3D 功能轉譯，也會每個畫面變更兩次。
        3. __m \_ constantBufferChangesEveryFrame__包含視圖矩陣。 這個矩陣取決於相機位置和觀看方向 (與投影垂直)，而且以 __Render__ 方法在每個畫面變更一次。 這稍早已在__轉譯架構 I：轉譯簡介__ (在 [__GameRenderer::Render__方法](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method) 下) 中討論過了。
        4. __m \_ constantBufferChangesEveryPrim__包含每個基本類型的模型矩陣和材質屬性。 模型矩陣會將頂點從區域座標轉換成全局座標。 這些常數為每個基本類型專用，而且會針對每次繪圖呼叫進行更新。 這稍早已在 __轉譯架構 I：轉譯簡介__ (在 [Primitive rendering](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering) 下) 中討論過了。
* 在此類別中還會定義保存基本類型紋理的著色器資源物件。
    * 某些紋理已預先定義 ([DDS](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) 是可以用來儲存已壓縮和解壓縮紋理的檔案格式。 DDS 紋理用於世界各地的牆與地板，以及子彈)。
    * 在此範例遊戲中，著色器資源物件為： __m \_ sphereTexture__、 __m \_ cylinderTexture__、 __m \_ ceilingTexture__、 __m \_ floorTexture__、 __m \_ wallsTexture__。
* 著色器物件定義於此類別來計算我們的基本類型和紋理。 
    * 在此範例遊戲中，著色器物件__為 \_ m vertexShader__、 __m \_ vertexShaderFlat__和__m \_ 無效__， __m \_ pixelShaderFlat__。
    * 頂點著色器會處理基本類型和基本光源，而像素著色器 (有時稱為片段著色器) 會處理紋理和任何個別像素的效果。
    * 用來轉譯不同基本類型的這些著色器有兩種版本 (一般和平面)。 我們有不同版本的原因是平面版本簡單許多，而且不處理反射強光光線或任何每個像素的光線效果。 它們是用來轉譯牆，讓低電量裝置上的轉譯變得更快速。

## <a name="gamerendererh"></a>GameRenderer.h

現在讓我們來看一下範例遊戲轉譯器類別物件中的程式碼。

```cppwinrt
// Class handling the rendering of the game
class GameRenderer : public std::enable_shared_from_this<GameRenderer>
{
public:
    GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources);

    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();
    // --- end of async related methods section

    winrt::Windows::Foundation::IAsyncAction CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game);
    void FinalizeCreateGameDeviceResources();
    winrt::Windows::Foundation::IAsyncAction LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    Simple3DGameDX::IGameUIControl* GameUIControl() { return &m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay.Visible(); }
    // --- end of rendering overlay section
...
private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>        m_deviceResources;

    ...

    // Shader resource objects
    winrt::com_ptr<ID3D11ShaderResourceView>    m_sphereTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_cylinderTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_ceilingTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_floorTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant buffers
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferNeverChanges;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    winrt::com_ptr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShader;
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShaderFlat;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShader;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShaderFlat;
    winrt::com_ptr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>建構函式

接下來，讓我們檢查範例遊戲的__GameRenderer__函式，並將它與 DirectX 11 應用程式範本中提供的__Sample3DSceneRenderer__函式做比較。

```cppwinrt
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
    m_gameInfoOverlay(deviceResources),
    m_gameHud(deviceResources, L"Windows platform samples", L"DirectX first-person game sample")
{
    // m_gameInfoOverlay is a GameHud object to render text in the top left corner of the screen.
    // m_gameHud is Game info rendered as an overlay on the top-right corner of the screen,
    // for example hits, shots, and time.

    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>建立和載入 DirectX 圖形資源

在範例遊戲中（在 Visual Studio 的__DirectX 11 應用程式（通用 Windows）__ 範本）中，建立和載入遊戲資源是使用從__GameRenderer__函式呼叫的這兩種方法來執行：

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method)

## <a name="createdevicedependentresources-method"></a>CreateDeviceDependentResources 方法

在 DirectX 11 應用程式範本，此方法用於非同步載入頂點和像素著色器、建立著色器和常數緩衝區，建立具有包含位置和色彩資訊之頂點的網格。 

在遊戲範例中，這些場景物件的操作會改為在 [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) 和 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) 方法之間分段。 

針對此範例遊戲，這種方法會如何？

* 已具現化的變數（__m \_ gameResourcesLoaded__ = false 和__m \_ levelResourcesLoaded__ = false），指出是否已載入資源，再繼續轉譯，因為我們是以非同步方式載入。 
* 由於 HUD 和重疊轉譯是在不同的類別物件，請在此呼叫 __GameHud::CreateDeviceDependentResources__ 和 __GameInfoOverlay::CreateDeviceDependentResources__ 方法。

以下是 __GameRenderer::CreateDeviceDependentResources__ 的程式碼。

```cppwinrt
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate whether resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud.CreateDeviceDependentResources();
    m_gameInfoOverlay.CreateDeviceDependentResources();
}
```

以下是用來建立和載入資源的方法清單。

- CreateDeviceDependentResources
  - CreateGameDeviceResourcesAsync （已新增）
  - FinalizeCreateGameDeviceResources （已新增）
- CreateWindowSizeDependentResources

在探究用來建立和載入資源的其他方法之前，請先建立轉譯器，然後查看其如何融入遊戲迴圈。

## <a name="create-the-renderer"></a>建立轉譯器

__GameRenderer__ 建立在 __GameMain__ 的建構函式。 它也會呼叫其他兩個方法，即 [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) 和 [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method)，其加入的目的是要協助建立和載入資源。

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // Creation of GameRenderer
    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);

    ...

    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    ...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>CreateGameDeviceResourcesAsync 方法

__CreateGameDeviceResourcesAsync__是從 [__建立 \_ __工作迴圈] 中的 [ __GameMain__ ] [函式] 方法呼叫，因為我們是以非同步方式載入遊戲資源。
        
__CreateDeviceResourcesAsync__ 是當作另一組非同步工作執行以載入遊戲資源的方法。 因為它應該是在另一個執行緒上執行，它只能存取 Direct3D 11 裝置方法 (在 __ID3D11Device__ 上定義的方法) 而不能存取裝置內容方法 (在 __ID3D11DeviceContext__ 上定義的方法)，所以它不執行任何轉譯。

__FinalizeCreateGameDeviceResources__ 方法在主執行緒上執行，且無法存取 Direct3D 11 裝置內容方法。

原則：
* 僅限使用 __CreateGameDeviceResourcesAsync__ 中的 __ID3D11Device__ 方法，因為它們是自由不受限的執行緒，表示它們可以在任何執行緒上執行。 預料之中的是，它們無法在和建立 __GameRenderer__ 的相同執行緒上執行。 
* 請勿使用此處 __ID3D11DeviceContext__ 中的方法，因為它們必須在單一執行緒和與 __GameRenderer__ 相同的執行緒上執行。
* 您可以使用此方法來建立常數緩衝區。
* 使用此方法將紋理 (例如 .dds 檔案) 及著色器資訊 (例如 .cso 檔案) 載入[著色器](tutorial--assembling-the-rendering-pipeline.md#shaders)。

這個方法用於：
* 建立4個[常數緩衝區](tutorial--assembling-the-rendering-pipeline.md#buffer)： __m \_ constantBufferNeverChanges__、 __m \_ constantBufferChangeOnResize__、 __m \_ constantBufferChangesEveryFrame__、 __m \_ constantBufferChangesEveryPrim__
* 建立封裝紋理取樣資訊的[樣本狀態](tutorial--assembling-the-rendering-pipeline.md#sampler-state)物件
* 建立包含方法所建立之所有非同步工作的工作群組。 它會等候所有這些非同步工作完成之後，接著呼叫 __FinalizeCreateGameDeviceResources__。
* 使用[基本載入器](tutorial--assembling-the-rendering-pipeline.md#the-basicloader-class)建立載入器。 新增載入器的非同步載入操作，當做工作加入到稍早建立的工作群組。
* 像 __BasicLoader::LoadShaderAsync__ 和 __BasicLoader::LoadTextureAsync__ 這類的方法用於載入：
    * 編譯的著色器物件 (VertextShader.cso、VertexShaderFlat.cso、PixelShader.cso 以及 PixelShaderFlat.cso)。 如需詳細資訊，請移至[各種著色器檔案格式](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats)。
    * 遊戲特定材質（資產 metal_texture seafloor，dds，cellceiling、cellfloor、dds、 \\ cellwall）。

```cppwinrt
IAsyncAction GameRenderer::CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game)
{
    auto lifetime = shared_from_this();

    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method. It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC. See
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_buffer_desc
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers.
    bd.Usage = D3D11_USAGE_DEFAULT;
    ...

    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11device-createbuffer.
    winrt::check_hresult(
        d3dDevice->CreateBuffer(&bd, nullptr, m_constantBufferNeverChanges.put())
        );

    ...

    // Define D3D11_SAMPLER_DESC. For API ref, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_sampler_desc.
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. For API ref, see
    // https://docs.microsoft.com/previous-versions/windows/desktop/legacy/aa366920(v=vs.85).
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    ...

    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    winrt::check_hresult(
        d3dDevice->CreateSamplerState(&sampDesc, m_samplerLinear.put())
        );

    // Start the async tasks to load the shaders and textures.

    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. For more info, see
    // https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader.
    BasicLoader loader{ d3dDevice };

    std::vector<IAsyncAction> tasks;

    uint32_t numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the
    // BasicLoader::LoadShaderAsync method. Push these method calls into a list of tasks.
    tasks.push_back(loader.LoadShaderAsync(L"VertexShader.cso", PNTVertexLayout, numElements, m_vertexShader.put(), m_vertexLayout.put()));
    tasks.push_back(loader.LoadShaderAsync(L"VertexShaderFlat.cso", nullptr, numElements, m_vertexShaderFlat.put(), nullptr));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShader.cso", m_pixelShader.put()));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShaderFlat.cso", m_pixelShaderFlat.put()));

    // Make sure the previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds,
    // cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks.
    tasks.push_back(loader.LoadTextureAsync(L"Assets\\seafloor.dds", nullptr, m_sphereTexture.put()));
    ...

    // Simulate loading additional resources by introducing a delay.
    tasks.push_back([]() -> IAsyncAction { co_await winrt::resume_after(GameConstants::InitialLoadingDelay); }());

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    for (auto&& task : tasks)
    {
        co_await task;
    }
}
```

## <a name="finalizecreategamedeviceresources-method"></a>FinalizeCreateGameDeviceResources 方法

會在 __CreateGameDeviceResourcesAsync__ 方法中的所有負載資源工作完成時，呼叫 __FinalizeCreateGameDeviceResources__ 方法。 

* 初始化 constantBufferNeverChanges 光源位置與色彩。 使用裝置內容方法呼叫 __ID3D11DeviceContext::UpdateSubresource__ 將初始資料載入常數緩衝區。
* 由於非同步載入的資源已完成載入，此時正是為這些資源與適當遊戲物件建立關聯的時機。
* 針對每個遊戲物件，使用已載入之紋理建立網格和材料。 然後，將網格和材料關聯到遊戲物件。
* 針對目標遊戲物件，由彩色的同心環組成的紋理和最上方的數值無法從紋理檔案載入。 它反而是使用 __TargetTexture.cpp__ 中的程式碼按照程序產生。 __TargetTexture__ 類別建立所需的資源，在初始化時將紋理併入幕後資源。 所產生的紋理接著就會與適當的目標遊戲物件建立關聯。

__FinalizeCreateGameDeviceResources__ 與 [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) 共用程式碼類似部分，因為它們：
* 使用 __SetProjParams__，確保相機擁有正確投影矩陣。 如需詳細資訊，請移至[相機和座標空間](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space)。
* 透過將 3D 旋轉矩陣與相機投影矩陣後乘來處理螢幕旋轉。 然後使用產生的投影矩陣更新 __ConstantBufferChangeOnResize__ 常數緩衝區。
* 將__m \_ gameResourcesLoaded__ __布林值__全域變數設定為，表示資源現在已載入緩衝區中，準備好進行下一個步驟。 之前提到，我們第一次透過 __GameRenderer::CreateDeviceDependentResources__ 方法，將此變數初始化為 __FALSE__ in the __GameRenderer__ 的建構函式方法。 
* 當此__m \_ GameResourcesLoaded__為__TRUE__時，可能會發生場景物件的呈現。 此已涵蓋在__轉譯架構 I：轉譯簡介__ 文章中 (在 [__GameRenderer::Render 方法__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method) 下)。

```cppwinrt
// This method is called from the GameMain constructor.
// Make sure that 2D rendering is occurring on the same thread as the main rendering.
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4(3.5f, 2.5f, 5.5f, 1.0f);
    ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.get(),
        0,
        nullptr,
        &constantBufferNeverChanges,
        0,
        0
        );

    // For the objects that function as targets, they have two unique generated textures.
    // One version is used to show that they have never been hit and the other is 
    // used to show that they have been hit.
    // TargetTexture is a helper class to procedurally generate textures for game
    // targets. The class creates the necessary resources to draw the texture into 
    // an off screen resource at initialization time.

    TargetTexture textureGenerator(
        d3dDevice,
        m_deviceResources->GetD2DFactory(),
        m_deviceResources->GetDWriteFactory(),
        m_deviceResources->GetD2DDeviceContext()
        );

    // CylinderMesh is a class derived from MeshObject and creates a ID3D11Buffer of
    // vertices and indices to represent a canonical cylinder (capped at
    // both ends) that is positioned at the origin with a radius of 1.0,
    // a height of 1.0 and with its axis in the +Z direction.
    // In the game sample, there are various types of mesh types:
    // CylinderMesh (vertical rods), SphereMesh (balls that the player shoots), 
    // FaceMesh (target objects), and WorldMesh (Floors and ceilings that define the enclosed area)

    auto cylinderMesh = std::make_shared<CylinderMesh>(d3dDevice, (uint16_t)26);
    ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    auto cylinderMaterial = std::make_shared<Material>(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.get(),
        m_vertexShader.get(),
        m_pixelShader.get()
        );

    ...

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto&& object : m_game->RenderObjects())
    {
        if (object->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            object->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.get(),
                    m_vertexShaderFlat.get(),
                    m_pixelShaderFlat.get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            object->Mesh(std::make_shared<WorldFloorMesh>(d3dDevice));
        }
        ...
        else if (auto cylinder = dynamic_cast<Cylinder*>(object.get()))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (auto target = dynamic_cast<Face*>(object.get()))
        {
            const int bufferLength = 16;
            wchar_t str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            auto string{ winrt::hstring(str, len) };

            winrt::com_ptr<ID3D11ShaderResourceView> texture;
            textureGenerator.CreateTextureResourceView(string, texture.put());
            target->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            texture = nullptr;
            textureGenerator.CreateHitTextureResourceView(string, texture.put());
            target->HitMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            target->Mesh(targetMesh);
        }
        ...
    }

    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera().SetProjParams(
        XM_PI / 2,
        renderTargetSize.Width / renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that the correct projection matrix is set in the ConstantBufferChangeOnResize buffer.

    // Get the 3D rotation transform matrix. We are handling screen rotations directly to eliminate an unaligned 
    // fullscreen copy. So it is necessary to post multiply the 3D rotation matrix to the camera's projection matrix
    // to get the projection matrix that we need.

    auto orientation = m_deviceResources->GetOrientationTransform3D();

    ConstantBufferChangeOnResize changesOnResize;

    // The matrices are transposed due to the shader code expecting the matrices in the opposite
    // row/column order from the DirectX math library.

    // XMStoreFloat4x4 takes a matrix and writes the components out to sixteen single-precision floating-point values at the given address. 
    // The most significant component of the first row vector is written to the first four bytes of the address, 
    // followed by the second most significant component of the first row, and so on. The second row is then written out in a 
    // like manner to memory beginning at byte 16, followed by the third row to memory beginning at byte 32, and finally 
    // the fourth row to memory beginning at byte 48. For more API ref info, go to: 
    // https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.storing.xmstorefloat4x4.aspx

    XMStoreFloat4x4(
        &changesOnResize.projection,
        XMMatrixMultiply(
            XMMatrixTranspose(m_game->GameCamera().Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    // Finally we set the m_gameResourcesLoaded as TRUE, so we can start rendering.
    m_gameResourcesLoaded = true;
}
```

## <a name="createwindowsizedependentresource-method"></a>CreateWindowSizeDependentResource method

每次視窗大小、方向、啟用 Stereo 轉譯或解析度變更，都會呼叫 CreateWindowSizeDependentResources 方法。 在範例遊戲中，它會更新__ConstantBufferChangeOnResize__中的投影矩陣。

視窗大小資源以此方式更新： 
* 應用程式架構取得指出視窗狀態變更的數個可能事件之一。 
* 然後您的主要遊戲迴圈會收到有關事件的通知，並且在主要類型 (__GameMain__) 執行個體呼叫 __CreateWindowSizeDependentResources__，其接著呼叫在遊戲轉譯器 (__GameRenderer__) 類別中的 __CreateWindowSizeDependentResources__ 實作。
* 此方法的主要工作是確定視覺效果不會因為視窗屬性變更而變得混淆或不正確。

針對此範例遊戲，有數個方法呼叫與[__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method)方法相同。 如需程式碼逐步解說，請移至前一節。

遊戲 HUD 和重疊視窗大小轉譯調整涵蓋在[新增使用者介面](tutorial--adding-a-user-interface.md)下。

```cppwinrt
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{
    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud.CreateWindowSizeDependentResources();

    ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    ...

    m_gameInfoOverlay.CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera().SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera().Projection()),
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
}
```

## <a name="next-steps"></a>後續步驟

這是實作遊戲圖形轉譯架構的基本程序。 遊戲規模越大，就必須放入更多抽象概念來處理物件類型和動畫行為的階層。 您必須實作更複雜的方法來載入和管理如網格及紋理等資產。 接下來，讓我們了解如何[[新增使用者介面](tutorial--adding-a-user-interface.md)。