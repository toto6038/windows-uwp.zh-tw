---
author: mtoepke
title: "組合轉譯架構"
description: "現在，我們來了解範例遊戲如何使用該結構和狀態來顯示圖形。"
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 遊戲, 轉譯"
ms.openlocfilehash: 7b97a70094c953e9614a84979c9f98fc91a82451
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="assemble-the-rendering-framework"></a>組合轉譯架構


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

到目前為止，您已經了解如何建立通用 Windows 平台 (UWP) 遊戲的結構與 Windows 執行階段搭配使用，以及如何定義狀態電腦來處理遊戲的流程。 現在，我們來了解範例遊戲如何使用該結構和狀態來顯示圖形。 我們在這裡要看看如何實作轉譯架構，先從使用圖形顯示物件初始化圖形裝置開始。

## <a name="objective"></a>目標


-   了解如何設定基本轉譯架構，以顯示 UWP DirectX 遊戲的圖形輸出。

> **注意** 本主題不會討論下列程式碼檔案，但是會提供本主題所述的類別和方法，並在[本主題的結尾提供其程式碼](#complete-sample-code-for-this-section)：
-   **Animate.h/.cpp**。
-   **BasicLoader.h/.cpp**。 提供已同步和非同步方式載入網格、著色器和紋理的方法。 非常實用！
-   **MeshObject.h/.cpp**、**SphereMesh.h/.cpp**、**CylinderMesh.h/.cpp**、**FaceMesh.h/.cpp** 以及 **WorldMesh.h/.cpp**。 包含遊戲中所使用物件基本類型的定義，例如子彈、圓柱型和圓錐體障礙物，以及射擊場的牆。 (本主題中簡要討論的 **GameObject.cpp** 包含轉譯這些基本類型的方法。)
-   **Level.h/.cpp** 和 **Level\[1-6\].h/.cpp**。 包含 6 個遊戲關卡中每個關卡的設定，包括過關條件以及目標和障礙物的數目和位置。
-   **TargetTexture.h/.cpp**。 包含一組方法，用於繪製點陣圖做為目標上的紋理。

這些檔案包含非 UWP DirectX 遊戲專用的程式碼。 不過，如果您想了解更多實作詳細資料，可以分別檢視這些程式碼。

 

本節涵蓋遊戲範例中的三個關鍵檔案 ([於本主題最後以程式碼提供](#complete-sample-code-for-this-section))：

-   **Camera.h/.cpp**
-   **GameRenderer.h/.cpp**
-   **PrimObject.h/.cpp**

同樣地，我們假設您了解基本的 3D 程式設計概念，像是網格、頂點和紋理。 如需一般 Direct3D 11 程式設計的詳細資訊，請參閱 [Direct3D 11 的程式設計指南](https://msdn.microsoft.com/library/windows/desktop/ff476345)。
接下來，我們來看看將遊戲呈現到螢幕上必須完成的工作。

## <a name="an-overview-of-the-windows-runtime-and-directx"></a>Windows 執行階段與 DirectX 概觀


DirectX 是 Windows 執行階段與 Windows 10 使用經驗的基本部分。 Windows 10 中所有的視覺物件都是採用 DirectX 建立的，而且您可以直接使用相同的低階圖形介面 [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)，為圖形硬體及驅動程式提供抽象層。 所有的 Direct3D 11 API 都能讓您用來與 DXGI 直接交談。 這可以為您的遊戲帶來快速且高效能的圖形，並讓您存取所有最新的圖形硬體功能。

若要在 UWP app 中新增對 DirectX 的支援，您要實作 [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482) 和 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 介面來為 DirectX 資源建立一個檢視提供者。 它們會分別為您的檢視提供者類型提供一個 Factory 模式以及提供 DirectX 檢視提供者實作。 UWP 單例 (由 [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) 物件表示) 會執行這個實作。

在[定義遊戲的 UWP 架構](tutorial--building-the-games-metro-style-app-framework.md)時，我們會先了解轉譯器在遊戲範例 app 架構中所扮演的角色。 現在，我們來看看遊戲轉譯器如何連接到檢視，並建立定義遊戲外觀的圖形。

## <a name="defining-the-renderer"></a>定義轉譯器


**GameRenderer** 抽象類型繼承自 **DirectXBase** 轉譯器類型，加入了 Stereo 3D 的支援，並且為建立及定義圖形基本類型的著色器宣告常數緩衝區和資源。

以下是 **GameRenderer** 的定義。

```cpp
ref class GameRenderer : public DirectXBase
{
internal:
    GameRenderer();

    virtual void Initialize(
        _In_ Windows::UI::Core::CoreWindow^ window,
        float dpi
        ) override;

    virtual void CreateDeviceIndependentResources() override;
    virtual void CreateDeviceResources() override;
    virtual void UpdateForWindowSizeChange() override;
    virtual void Render() override;
    virtual void SetDpi(float dpi) override;

    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    GameInfoOverlay^ InfoOverlay()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f
            );
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f + GameInfoOverlayConstant::Width,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f + GameInfoOverlayConstant::Height
            );
    };

protected private:
    bool                                                m_initialized;
    bool                                                m_gameResourcesLoaded;
    bool                                                m_levelResourcesLoaded;
    GameInfoOverlay^                                    m_gameInfoOverlay;
    GameHud^                                            m_gameHud;
    Simple3DGame^                                       m_game;

    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

因為 Direct3D 11 API 會定義為 COM API，所以您必須提供這些 API 所定義之物件的 [**ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983) 參考。 當 app 終止時，這些物件會在最後一個參考超出範圍時，自動被釋放。

遊戲範例會宣告 4 個特定的常數緩衝區：

-   **m\_constantBufferNeverChanges**。 這個常數緩衝區包含光線參數。 只需設定一次，永遠不再變更。
-   **m\_constantBufferChangeOnResize**。 這個常數緩衝區包含投影矩陣。 投影矩陣取決於視窗的大小和外觀比例。 只有在視窗大小變更時才會更新。
-   **m\_constantBufferChangesEveryFrame**。 這個常數緩衝區包含視圖矩陣。 這個矩陣取決於相機位置和觀看方向 (與投影垂直)，而且每個畫面只變更一次。
-   **m\_constantBufferChangesEveryPrim**。 這個常數緩衝區包含模型矩陣和每個基本類型的內容屬性。 模型矩陣會將頂點從區域座標轉換成全局座標。 這些常數為每個基本類型專用，而且會針對每次繪圖呼叫進行更新。

為多個常數緩衝區採用不同頻率的整體概念，就是為了減少每個畫面必須傳送到 GPU 的資料量。 因此，範例會根據必須更新的頻率，將常數分成不同的緩衝區。 這是 Direct3D 程式設計的最佳做法。

轉譯器包含能夠計算基本類型和紋理的著色器物件：**m\_vertexShader** 和 **m\_pixelShader**。 頂點著色器會處理基本類型和基本光源，而像素著色器 (有時稱為片段著色器) 會處理紋理和任何個別像素的效果。 用來轉譯不同基本類型的這些著色器有兩種版本 (一般和平面)。 平面版本簡單許多，而且不處理反射強光光線或任何每個像素的光線效果。 它們是用來轉譯牆，讓低電量裝置上的轉譯變得更快速。

轉譯器類別包含 [DirectWrite 和 Direct2D](https://msdn.microsoft.com/library/windows/desktop/ff729481) 資源，用於重疊以及平視顯示器 (**GameHud** 物件)。 重疊和 HUD 會在圖形管線中的投射完成時，繪製於轉譯目標之上。

轉譯器還會定義保存基本類型紋理的著色器資源物件。 有些紋理是預先定義的 (世界的牆和地板及子彈的 DDS 紋理)。

現在，讓我們來看看如何建立這個物件！

## <a name="initializing-the-renderer"></a>初始化轉譯器


範例遊戲會呼叫這個 **Initialize** 方法作為 **App::SetWindow** 中 CoreApplication 初始化序列的一部分。

```cpp
void GameRenderer::Initialize(
    _In_ CoreWindow^ window,
    float dpi
    )
{
    if (!m_initialized)
    {
        m_gameHud = ref new GameHud(
            "Windows 8 Samples",
            "DirectX first-person game sample"
            );
        m_gameInfoOverlay = ref new GameInfoOverlay();
        m_initialized = true;
    }

    DirectXBase::Initialize(window, dpi);

    // Initialize could be called multiple times as a result of an error with the hardware device
    // that requires it to be reinitialized.  Because the m_gameInfoOverlay variable has resources that are
    // dependent on the device, it will need to be reinitialized each time with the new device information.
    m_gameInfoOverlay->Initialize(m_d2dDevice.Get(), m_d2dContext.Get(), m_dwriteFactory.Get(), dpi);
}
```

這是相當直接的方法。 它會檢查轉譯器是否之前被初始化過，如果還未初始化，則會具現化 **GameHud** 和 **GameInfoOverlay** 物件。

之後，轉譯器初始化程序會執行 **Initialize** (由所繼承的 **DirectXBase** 類別提供) 的基底實作。

當 DirectXBase 初始化完成時，會初始化 **GameInfoOverlay** 物件。 初始化完成後，現在來看看建立和載入遊戲圖形資源的方法。

## <a name="creating-and-loading-directx-graphics-resources"></a>建立和載入 DirectX 圖形資源


任何遊戲的第一個工作就是建立與圖形介面的連線、建立繪製圖形所需的資源，然後設定轉譯目標以繪製這些圖形。 在遊戲範例 (以及 Microsoft Visual Studio **DirectX 11 應用程式 (通用 Windows)** 範本) 中，這個程序是會以三種方法實作：

-   **CreateDeviceIndependentResources**
-   **CreateDeviceResources**
-   **CreateWindowSizeDependentResources**

現在，在遊戲範例中，我們要覆寫這些方法的其中兩個 (**CreateDeviceIndependentResources** 和 **CreateDeviceResources**)，它們是在 **DirectX 11 應用程式 (Universal Windows)** 範本中實作的 **DirectXBase** 類別上提供的。 對於每個覆寫方法，我們首先呼叫它們覆寫的 **DirectXBase** 實作，然後在遊戲範例中新增更多實作的特定詳細資料。 請注意，遊戲範例中內含的 **DirectXBase** 類別實作已根據 Visual Studio 範例提供的版本加以修改，以便包含立體視覺檢視支援，並包含預先旋轉的 **SwapBuffer** 物件。

**CreateWindowSizeDependentResources** 不是由 **GameRenderer** 物件覆寫。 我們使用 **DirectXBase** 類別中提供的它的實作。

如需這些方法的 **DirectXBase** 基底實作詳細資訊，請參閱[如何設定您的 UWP DirectX 應用程式來顯示檢視](https://msdn.microsoft.com/library/windows/apps/hh465077)。

這些覆寫方法的第一種 **CreateDeviceIndependentResources** 會呼叫 **GameHud::CreateDeviceIndependentResources** 方法來建立 [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) 文字資源，並使用大部分 UWP app 使用的 Segoe UI 字型。

CreateDeviceIndependentResources

```cpp
void GameRenderer::CreateDeviceIndependentResources()
{
    DirectXBase::CreateDeviceIndependentResources();
    m_gameHud->CreateDeviceIndependentResources(m_dwriteFactory.Get(), m_wicFactory.Get());
}
```

```cpp
void GameHud::CreateDeviceIndependentResources(
    _In_ IDWriteFactory* dwriteFactory,
    _In_ IWICImagingFactory* wicFactory
    )
{
    m_dwriteFactory = dwriteFactory;
    m_wicFactory = wicFactory;

    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudBodyPointSize,
            L"en-us",
            &m_textFormatBody
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Symbol",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudBodyPointSize,
            L"en-us",
            &m_textFormatBodySymbol
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Light",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudTitleHeaderPointSize,
            L"en-us",
            &m_textFormatTitleHeader
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Light",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudTitleBodyPointSize,
            L"en-us",
            &m_textFormatTitleBody
            )
        );

    DX::ThrowIfFailed(m_textFormatBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatBodySymbol->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatBodySymbol->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatTitleHeader->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatTitleHeader->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatTitleBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatTitleBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
}
```

這個範例使用四種文字格式子：兩種用於標題標頭和標題內文文字，另兩種則用於內文文字。 這在重疊文字很常使用。

第二種方法 **CreateDeviceResources** 會載入遊戲的特定資源，以便在圖形裝置進行計算。 我們來看看這個方法的程式碼。

CreateDeviceResources

```cpp
oid GameRenderer::CreateDeviceResources()
{
    DirectXBase::CreateDeviceResources();

    m_gameHud->CreateDeviceResources(m_d2dContext.Get());

    if (m_game != nullptr)
    {
        // The initial invocation of CreateDeviceResources occurs
        // before the Game State is initialized when the device is first
        // being created, so that the inital loading screen can be displayed.
        // Subsequent invocations of CreateDeviceResources will be a result
        // of an error with the device that requires the resources to be
        // recreated.  In this case, the game state is already initialized
        // so the game device resources need to be recreated.

        // This sample doesn't gracefully handle all the async recreation
        // of resources so an exception is thrown.
        throw Platform::Exception::CreateException(
            DXGI_ERROR_DEVICE_REMOVED,
            "GameRenderer::CreateDeviceResources - Recreation of resources after TDR not available\n"
            );
    }
}
```

```cpp
void GameHud::CreateDeviceResources(_In_ ID2D1DeviceContext* d2dContext)
{
    auto location = Package::Current->InstalledLocation;
    Platform::String^ path = Platform::String::Concat(location->Path, "\\");
    path = Platform::String::Concat(path, "windows-sdk.png");

    ComPtr<IWICBitmapDecoder> wicBitmapDecoder;
    DX::ThrowIfFailed(
        m_wicFactory->CreateDecoderFromFilename(
            path->Data(),
            nullptr,
            GENERIC_READ,
            WICDecodeMetadataCacheOnDemand,
            &wicBitmapDecoder
            )
        );

    ComPtr<IWICBitmapFrameDecode> wicBitmapFrame;
    DX::ThrowIfFailed(
        wicBitmapDecoder->GetFrame(0, &wicBitmapFrame)
        );

    ComPtr<IWICFormatConverter> wicFormatConverter;
    DX::ThrowIfFailed(
        m_wicFactory->CreateFormatConverter(&wicFormatConverter)
        );

    DX::ThrowIfFailed(
        wicFormatConverter->Initialize(
            wicBitmapFrame.Get(),
            GUID_WICPixelFormat32bppPBGRA,
            WICBitmapDitherTypeNone,
            nullptr,
            0.0,
            WICBitmapPaletteTypeCustom  // The BGRA format has no palette so this value is ignored.
            )
        );

    double dpiX = 96.0f;
    double dpiY = 96.0f;
    DX::ThrowIfFailed(
        wicFormatConverter->GetResolution(&dpiX, &dpiY)
        );

    // Create D2D Resources.
    DX::ThrowIfFailed(
        d2dContext->CreateBitmapFromWicBitmap(
            wicFormatConverter.Get(),
            BitmapProperties(
                PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
                static_cast<float>(dpiX),
                static_cast<float>(dpiY)
                ),
            &m_logoBitmap
            )
        );

    m_logoSize = m_logoBitmap->GetSize();

    DX::ThrowIfFailed(
        d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White),
            &m_textBrush
            )
        );
}
```

這個範例中，在正常執行時，**CreateDeviceResources** 方法會呼叫基底類別方法，然後再呼叫 **GameHud::CreateDeviceResources** 方法 (上面也有列出)。 如果稍後基礎圖形裝置出現問題，可能必須將它重新初始化。 在這種情形下，**CreateDeviceResources** 方法會初始化一組非同步工作來建立遊戲裝置資源。 這是利用一個包含兩個方法的序列來完成的：呼叫 **CreateDeviceResourcesAsync**，然後在它完成時，再呼叫 **FinalizeCreateGameDeviceResources**。

CreateGameDeviceResourcesAsync 和 FinalizeCreateGameDeviceResources

```cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Set the Loading state to wait until any async resources have
    // been loaded before proceeding.
    m_game = game;

    // NOTE: Only the m_d3dDevice is used in this method.  It's expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the m_d3dDevice are free-threaded and are safe while any methods
    // in the m_d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.

    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers.
    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
    bd.CPUAccessFlags = 0;
    bd.ByteWidth = (sizeof(ConstantBufferNeverChanges) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangeOnResize) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangeOnResize)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryFrame) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryFrame)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryPrim) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryPrim)
        );

    D3D11_SAMPLER_DESC sampDesc;
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;
    sampDesc.MinLOD = 0;
    sampDesc.MaxLOD = FLT_MAX;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    m_cylinderTexture = nullptr;
    m_ceilingTexture = nullptr;
    m_floorTexture = nullptr;
    m_wallsTexture = nullptr;

    // Load Game specific Textures.
    tasks.push_back(loader->LoadTextureAsync("seafloor.dds", nullptr, &m_sphereTexture));
    tasks.push_back(loader->LoadTextureAsync("metal_texture.dds", nullptr, &m_cylinderTexture));
    tasks.push_back(loader->LoadTextureAsync("cellceiling.dds", nullptr, &m_ceilingTexture));
    tasks.push_back(loader->LoadTextureAsync("cellfloor.dds", nullptr, &m_floorTexture));
    tasks.push_back(loader->LoadTextureAsync("cellwall.dds", nullptr, &m_wallsTexture));
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Return the task group of all the async tasks for loading the shader and texture assets.
    return when_all(tasks.begin(), tasks.end());
}
```

```cpp
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now, associate all the resources with the appropriate
    // Game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[1] = XMFLOAT4( 3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[2] = XMFLOAT4(-3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[3] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);
    m_d3dContext->UpdateSubresource(m_constantBufferNeverChanges.Get(), 0, nullptr, &constantBufferNeverChanges, 0, 0);

    // For the targets, there are two unique generated textures.
    // Each texture image includes the number of the texture.
    // Make sure the 2-D rendering is occurring on the same thread
    // as the main rendering.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        m_d3dDevice.Get(),
        m_d2dFactory.Get(),
        m_dwriteFactory.Get(),
        m_d2dContext.Get()
        );

    MeshObject^ cylinderMesh = ref new CylinderMesh(m_d3dDevice.Get(), 26);
    MeshObject^ targetMesh = ref new FaceMesh(m_d3dDevice.Get());
    MeshObject^ sphereMesh = ref new SphereMesh(m_d3dDevice.Get(), 26);

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    Material^ sphereMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        50.0f,
        m_sphereTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldFloorMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldCeilingId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_ceilingTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldCeilingMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldWallsId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_wallsTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldWallsMesh(m_d3dDevice.Get()));
        }
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        else if (Sphere^ sphere = dynamic_cast<Sphere^>(*object))
        {
            sphere->Mesh(sphereMesh);
            sphere->NormalMaterial(sphereMaterial);
        }
    }

    // Ensure that the camera has been initialized with the right Projection
    // matrix.  The camera is not created at the time the first window resize event
    // occurs.
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2, m_renderTargetSize.Width / m_renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that Projection matrix has been set in the constant buffer
    // now that all the resources are loaded.
    // DirectXBase is handling screen rotations directly to eliminate an unaligned
    // fullscreen copy.  As a result, it's necessary to post multiply the rotationTransform3D
    // matrix to the camera projection matrix.
    ConstantBufferChangeOnResize changesOnResize;
    XMStoreFloat4x4(
        &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                )
        );

    m_d3dContext->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    m_gameResourcesLoaded = true;
}
```

**CreateDeviceResourcesAsync** 是當作另一組非同步工作執行以載入遊戲資源的方法。 因為它應該是在另一個執行緒上執行，它只能存取 Direct3D 11 裝置方法 (在 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 上定義的方法) 而不能存取裝置內容方法 (在 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 上定義的方法)，所以它可以選擇不執行任何轉譯。 **FinalizeCreateGameDeviceResources** 方法在主執行緒上執行，且無法存取 Direct3D 11 裝置內容方法。

載入遊戲裝置資源的事件序列的進行方式如下。

**CreateDeviceResourcesAsync** 會先為基本類型初始化常數緩衝區。 常數緩衝區是低延遲、寬度固定的緩衝區，保存了著色器在著色器執行期間所使用的資料 (將這些緩衝區想像為在執行特定繪圖呼叫時將資料傳送到常數著色器)。在這個範例中，緩衝區包含著色器將用來執行下列動作的資料：

-   在轉譯器初始化時放置光源及設定其色彩
-   在視窗調整大小時計算視圖矩陣
-   計算每次畫面更新的投影矩陣
-   計算每次轉譯更新時基本類型的轉換

這些常數會接收來源資訊 (頂點)，然後將頂點座標和資料模型空間轉換為裝置空間。 這些資料最後會在轉譯目標中產生圖素座標和像素。

接下來，遊戲轉譯器物件會為將執行計算的著色器建立載入器。 (請參閱特定實作範例中的 **BasicLoader.cpp**)。

然後，**CreateDeviceResourcesAsync** 會初始化非同步工作，將所有紋理資源載入 **ShaderResourceViews**。 這些紋理資源儲存在範例隨附的 DirectDraw Surface (DDS) 紋理中。 DDS 紋理是失真紋理格式，可與 DirectX 材質壓縮 (DXTC) 搭配使用。 我們將這些紋理運用在世界的牆、天花板及地板，以及子彈和柱型障礙物。

最後，它會傳回包含方法建立的所有非同步工作的工作群組。 呼叫函式在等候所有這些非同步工作完成之後，會接著呼叫 **FinalizeCreateGameDeviceResources**。

**FinalizeCreateGameDeviceResources** 使用裝置內容方法呼叫 [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) 將初始資料載入常數緩衝區：`m_deviceContext->UpdateSubresource` 這個方法會為球體、圓柱、表面、遊戲世界物件及其關聯的材質建立網格物件。 接著它會逐個查看遊戲物件清單，將每個物件與適當的裝置資源建立關聯。

環型和編號的目標物件的紋理是使用 **TargetTexture.cpp** 中的程式碼，透過程序產生的。 轉譯器會建立 **TargetTexture** 類型的執行個體，當我們呼叫 **TargetTexture::CreateTextureResourceView** 方法時，它會在遊戲中為目標物件建立點陣圖紋理。 所產生的紋理由彩色的同心環組成，上方還有數字值。 這些產生的資源是與適當的目標遊戲物件關聯的。

最後，**FinalizeCreateGameDeviceResources** 設定 `m_gameResourcesLoaded` 布林值全域變數，指示現在所有資源都已載入。

遊戲具有在目前視窗中顯示圖形的資源，而且可以在視窗變更時重新建立這些資源。 現在，我們來看看用來定義該視窗中玩家場景視野的相機。

## <a name="implementing-the-camera-object"></a>實作相機物件


遊戲有現成的程式碼可以用自己的座標系統來更新世界 (有時候稱為世界空間或場景空間)。 所有物件 (包括相機) 都在這個空間設定放置及方向。 在範例遊戲中，會使用相機的位置以及視角向量 (「視線」向量是從相機直接看到場景，而「仰視」向量則是垂直向上看) 定義相機空間。 投影參數決定最終場景可實際看到的空間量；而視野範圍 (FoV)、外觀比例以及裁剪平面則可定義投影轉換。 頂點著色器則負責使用下列演算法 (其中 V 是向量而 M 是矩陣)，將模型座標轉換為裝置座標的繁重工作：

`              V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)           `.

-   `M(model-to-world)` 是模型座標到全局座標的轉換矩陣。 這是由基本類型提供。 (我們將在這裡的基本類型一節中討論相關資訊)。
-   `M(world-to-view)` 是全局座標到視覺座標的轉換矩陣。 這是由相機的視圖矩陣提供的。
-   `M(view-to-device)` 是視覺座標到裝置座標的轉換矩陣。 這是由相機的投影提供的。

**VertexShader.hlsl** 中的著色器程式碼會使用這些常數緩衝區向量及矩陣進行載入，並為每個頂點執行這個轉換。

**Camera** 物件會定義視圖矩陣和投影矩陣。 我們來看看範例遊戲如何宣告它。

```cpp
ref class Camera
{
internal:
    Camera();

    // Call these from client and use Get*Matrix() to read new matrices.
    // Functions to change camera matrices
    void SetViewParams(_In_ DirectX::XMFLOAT3 eye, _In_ DirectX::XMFLOAT3 lookAt, _In_ DirectX::XMFLOAT3 up);
    void SetProjParams(_In_ float fieldOfView, _In_ float aspectRatio, _In_ float nearPlane, _In_ float farPlane);

    void LookDirection (_In_ DirectX::XMFLOAT3 lookDirection);
    void Eye (_In_ DirectX::XMFLOAT3 position);

    DirectX::XMMATRIX View();
    DirectX::XMMATRIX Projection();
    DirectX::XMMATRIX LeftEyeProjection();
    DirectX::XMMATRIX RightEyeProjection();
    DirectX::XMMATRIX World();
    DirectX::XMFLOAT3 Eye();
    DirectX::XMFLOAT3 LookAt();
    DirectX::XMFLOAT3 Up();
    float NearClipPlane();
    float FarClipPlane();
    float Pitch();
    float Yaw();

protected private:
    DirectX::XMFLOAT4X4 m_viewMatrix;                 // View matrix
    DirectX::XMFLOAT4X4 m_projectionMatrix;           // Projection matrix
    DirectX::XMFLOAT4X4 m_projectionMatrixLeft;       // Projection Matrix for Left Eye Stereo
    DirectX::XMFLOAT4X4 m_projectionMatrixRight;      // Projection Matrix for Right Eye Stereo

    DirectX::XMFLOAT4X4 m_inverseView;

    DirectX::XMFLOAT3 m_eye;                        // Camera eye position
    DirectX::XMFLOAT3 m_lookAt;                     // LookAt position
    DirectX::XMFLOAT3 m_up;                         // Up vector
    float             m_cameraYawAngle;             // Yaw angle of camera
    float             m_cameraPitchAngle;           // Pitch angle of camera

    float             m_fieldOfView;                // Field of view
    float             m_aspectRatio;                // Aspect ratio
    float             m_nearPlane;                  // Near plane
    float             m_farPlane;                   // Far plane
};
```

有兩個 4x4 矩陣 (**m\_viewMatrix** 和 **m\_projectionMatrix**) 可定義視覺座標到投影座標的轉換 (您在立體投影時會使用兩個投影矩陣：分別為每個眼睛的視野)。它們分別使用下列兩種方法進行計算：

-   **SetViewParams**
-   **SetProjParams**

這兩種方法的程式碼看起來像這樣：

```cpp
void Camera::SetViewParams(
    _In_ XMFLOAT3 eye,
    _In_ XMFLOAT3 lookAt,
    _In_ XMFLOAT3 up
    )
{
    m_eye = eye;
    m_lookAt = lookAt;
    m_up = up;

    // Calc the view matrix.
    XMMATRIX view = XMMatrixLookAtLH(
        XMLoadFloat3(&m_eye),
        XMLoadFloat3(&m_lookAt),
        XMLoadFloat3(&m_up)
        );

    XMVECTOR det;
    XMMATRIX inverseView = XMMatrixInverse(&det, view);
    XMStoreFloat4x4(&m_viewMatrix, view);
    XMStoreFloat4x4(&m_inverseView, inverseView);

    // The axis basis vectors and camera position are stored inside the
    // position matrix in the 4 rows of the camera's world matrix.
    // To figure out the yaw/pitch of the camera, we just need the Z basis vector
    XMFLOAT3* zBasis = (XMFLOAT3*)&inverseView._31;

    m_cameraYawAngle = atan2f(zBasis->x, zBasis->z);

    float len = sqrtf(zBasis->z * zBasis->z + zBasis->x * zBasis->x);
    m_cameraPitchAngle = atan2f(zBasis->y, len);
}
//--------------------------------------------------------------------------------------
// Calculates the projection matrix based on input params
//--------------------------------------------------------------------------------------
void Camera::SetProjParams(
    _In_ float fieldOfView,
    _In_ float aspectRatio,
    _In_ float nearPlane,
    _In_ float farPlane
    )
{
    // Set attributes for the projection matrix
    m_fieldOfView = fieldOfView;
    m_aspectRatio = aspectRatio;
    m_nearPlane = nearPlane;
    m_farPlane = farPlane;
    XMStoreFloat4x4(
        &m_projectionMatrix,
        XMMatrixPerspectiveFovLH(
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane
            )
        );

    STEREO_PARAMETERS* stereoParams = nullptr;
    // change the projection matrix
    XMStoreFloat4x4(
        &m_projectionMatrixLeft,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::LEFT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );

    XMStoreFloat4x4(
        &m_projectionMatrixRight,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::RIGHT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );
}
```

我們在 **Camera** 物件上分別呼叫 **View** 和 **Projection** 方法，以取得產生的視覺資料和投影資料。 這些呼叫發生於我們要討論的下一個步驟，遊戲迴圈中呼叫的 **GameRenderer::Render** 方法。

現在，我們來看看遊戲如何建立架構，以使用相機來繪製我們的遊戲圖形。 這包括定義組成遊戲世界及其元素的基本類型。

## <a name="defining-the-primitives"></a>定義基本類型


在遊戲範例程式碼中，我們定義和實作兩個基底類別中的基本類型，並為每個基本類型定義對應的專屬特性。

**MeshObject.h/.cpp** 定義所有網格物件的基底類別。 **SphereMesh.h/.cpp**、**CylinderMesh.h/.cpp**、**FaceMesh.h/.cpp** 以及 **WorldMesh.h/.cpp** 檔案包含的程式碼會將每個基本類型的常數緩衝區填入定義基本類型幾何的頂點和頂點標準資料。 如果您想要了解如何在自己的遊戲 app 中建立 Direct3D 基本類型，這些程式碼檔案是一個很好的起點，但是在本文中我們不會涵蓋這些內容，因為這對於這個遊戲實作而言太過專精。 就目前而言，我們假設已填入每個基本類型的頂點緩衝區，然後看看遊戲範例如何處理這些緩衝區來更新遊戲本身。

代表遊戲觀點之基本類型物件的基底類別會在 **GameObject.h./.cpp** 中定義。 這個類別 **GameObject** 定義所有基本類型中常見行為的欄位及方法。 每個基本物件類型都從它衍生出來。 我們來看看如何定義：

```cpp
ref class GameObject
{
internal:
    GameObject();

    // Expect these two functions to be overloaded by subclasses.
    virtual bool IsTouching(
        DirectX::XMFLOAT3 /* point */,
        float /* radius */,
        _Out_ DirectX::XMFLOAT3 *contact,
        _Out_ DirectX::XMFLOAT3 *normal
        )
    {
        *contact = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);
        *normal = DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f);
        return false;
    };

    void Render(
        _In_ ID3D11DeviceContext *context,
        _In_ ID3D11Buffer *primitiveConstantBuffer
        );

    void Active(bool active);
    bool Active();
    void Target(bool target);
    bool Target();
    void Hit(bool hit);
    bool Hit();
    void OnGround(bool ground);
    bool OnGround();
    void TargetId(int targetId);
    int  TargetId();
    void HitTime(float t);
    float HitTime();

    void     AnimatePosition(_In_opt_ Animate^ animate);
    Animate^ AnimatePosition();

    void         HitSound(_In_ SoundEffect^ hitSound);
    SoundEffect^ HitSound();

    void PlaySound(float impactSpeed, DirectX::XMFLOAT3 eyePoint);

    void Mesh(_In_ MeshObject^ mesh);

    void NormalMaterial(_In_ Material^ material);
    Material^ NormalMaterial();
    void HitMaterial(_In_ Material^ material);
    Material^ HitMaterial();

    void Position(DirectX::XMFLOAT3 position);
    void Position(DirectX::XMVECTOR position);
    void Velocity(DirectX::XMFLOAT3 velocity);
    void Velocity(DirectX::XMVECTOR velocity);
    DirectX::XMMATRIX ModelMatrix();
    DirectX::XMFLOAT3 Position();
    DirectX::XMVECTOR VectorPosition();
    DirectX::XMVECTOR VectorVelocity();
    DirectX::XMFLOAT3 Velocity();

protected private:
    virtual void UpdatePosition() {};
    // Object Data
    bool                m_active;
    bool                m_target;
    int                 m_targetId;
    bool                m_hit;
    bool                m_ground;

    DirectX::XMFLOAT3   m_position;
    DirectX::XMFLOAT3   m_velocity;
    DirectX::XMFLOAT4X4 m_modelMatrix;

    Material^           m_normalMaterial;
    Material^           m_hitMaterial;

    DirectX::XMFLOAT3   m_defaultXAxis;
    DirectX::XMFLOAT3   m_defaultYAxis;
    DirectX::XMFLOAT3   m_defaultZAxis;

    float               m_hitTime;

    Animate^            m_animatePosition;
    MeshObject^         m_mesh;

    SoundEffect^        m_hitSound;
};
```

大部分的欄位都包含有關遊戲世界中基本類型的狀態、視覺屬性或位置的資料。 大部分遊戲特別需要幾個方法：

-   **Mesh**。 取得基本類型的網格幾何 (儲存於 **m\_mesh** 中)。 這個幾何定義在 **MeshObject.h/.cpp** 中。
-   **IsTouching**。 這個方法判斷基本類型是否位於某點的特定距離內，並傳回表面上與該點最靠近的點，以及與該點之物件表面垂直的點。 因為這個範例僅關心子彈基本類型的撞擊，因此這對於遊戲的動態設定已經足夠。 它不是基本類型-基本類型交集函數的一般用途，雖然它可以作為其基本。
-   **AnimatePosition**。 更新基本類型的移動及動畫。
-   **UpdatePosition**。 更新全局座標空間中物件的位置。
-   **Render**。 將基本類型的材質屬性放入基本類型常數緩衝區，然後使用裝置內容轉譯 (繪製) 基本類型幾何。

建立基底物件類型來定義基本類型的最少方法，是一個很好的做法，因為大部分的遊戲都有非常大量的基本類型，而程式碼很快就會變得難於管理。 當更新迴圈可以多種型態的方式處理基本類型時，它同時也簡化了遊戲程式碼，讓物件本身定義自己的更新及轉譯行為。

我們來看看遊戲範例中基本類型的基本轉譯。

## <a name="rendering-the-primitives"></a>轉譯基本類型


遊戲範例中的基本類型使用父系 **GameObject** 類別上實作的基底 **Render** 方法，如這裡所示：

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    m_mesh->Render(context);
}
```

**GameObject::Render** 方法使用指定基本類型的特定資料來更新基本類型常數緩衝區。 遊戲使用多個常數緩衝區，但是只要在每個基本類型更新這些緩衝區一次。

將常數緩衝區想像是為每個基本類型執行的著色器輸入。 有些資料是靜態的 (**m\_constantBufferNeverChanges**)；有些資料在架構上是常數 (**m\_constantBufferChangesEveryFrame**)，像是相機的位置；有些資料是專用於基本類型，像色彩和紋理 (**m\_constantBufferChangesEveryPrim**)。 遊戲轉譯器將這些輸入分成不同的常數緩衝區，以最佳化 CPU 與 GPU 使用的記憶體頻寬。 這種方式也有助於將 GPU 需要追蹤的資料數量減到最少。 請記住，GPU 有很大的命令佇列，而每次遊戲呼叫 **Draw** 時，該命令連同與其關聯的資料一起進入佇列。 當遊戲更新基本類型常數緩衝區並發出下一個 **Draw** 命令時，圖形驅動程式會將下一個命令及關聯的資料新增到佇列。 如果遊戲繪製 100 個基本類型，則佇列中可能會有 100 個常數緩衝區資料的複本。 我們想要將傳送到 GPU 的遊戲資料數量減到最少，所以遊戲會使用不同的基本類型常數緩衝區，其中僅包含每個基本類型的更新。

如果偵測到撞擊 (擊中)，**GameObject::Render** 會檢查目前的內容，該內容會指出子彈是否擊中目標。 如果擊中目標，這個方法會套用擊中材質，反轉目標圓圈的顏色，以指出玩家已成功射中目標。 否則，它會使用相同的方法套用預設材質。 在這兩個情況中，它會呼叫 **Material::RenderSetup** (將適當常數設定到常數緩衝區中) 來設定材質。 接著，它呼叫 [**ID3D11DeviceContext::PSSetShaderResources**](https://msdn.microsoft.com/library/windows/desktop/ff476473) 來設定像素著色器的對應紋理資源，並呼叫 [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) 和 [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) 分別設定頂點著色器和像素著色器物件本身。

這裡提供 **Material::RenderSetup** 如何設定常數緩衝區及指派著色器資源的方式。 再次請您注意，常數緩衝區是專門用來更新對基本類型所作的變更。

> **注意** **Material** 類別定義在 **Material.h/.cpp** 中。

 

```cpp
void Material::RenderSetup(
    _In_ ID3D11DeviceContext* context,
    _Inout_ ConstantBufferChangesEveryPrim* constantBuffer
    )
{
    constantBuffer->meshColor = m_meshColor;
    constantBuffer->specularColor = m_specularColor;
    constantBuffer->specularPower = m_specularExponent;
    constantBuffer->diffuseColor = m_diffuseColor;

    context->PSSetShaderResources(0, 1, m_textureRV.GetAddressOf());
    context->VSSetShader(m_vertexShader.Get(), nullptr, 0);
    context->PSSetShader(m_pixelShader.Get(), nullptr, 0);
}
```

最後，**PrimObject::Render** 會為基礎 **MeshObject** 物件呼叫 **Render** 方法。

```cpp
void MeshObject::Render(_In_ ID3D11DeviceContext *context)
{
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

現在，遊戲範例的 **MeshObject::Render** 方法會佇列繪圖命令，使用目前的場景狀態，在 GPU 上執行著色器。 頂點著色器會將模型座標的幾何 (頂點) 轉換為裝置 (全局) 座標，將相機位置及透視轉換列入考慮。 最後，像素著色器使用之前設定的紋理，將轉換過的三角形轉譯到背景緩衝區。

實際轉譯程序就是這樣！

## <a name="creating-the-vertex-and-pixel-shaders"></a>建立頂點著色器和像素著色器


此時，遊戲範例已定義了要繪製的基本類型，以及定義其轉譯的常數緩衝區。 這些常數緩衝區會做為在圖形裝置上執行的著色器的參數集。 這些著色器程式有兩種類型：

-   頂點著色器執行個別頂點作業，例如頂點轉換和光線。
-   像素 (或稱片段) 著色器執行個別像素作業，例如紋理和個別像素光線。 它們還可以用來執行點陣圖的後處理效果，例如最終轉譯目標。

著色器程式碼是以高階著色器語言 (HLSL) 定義，在 Direct3D 11 中，是從使用與 C 類似之語法建立的程式編譯而成。 (完整的語法可以在[這裡](https://msdn.microsoft.com/library/windows/desktop/bb509635)找到。) 範例遊戲的兩個主要著色器定義在 **PixelShader.hlsl** 和 **VertexShader.hlsl** 中。 (另外也為低電量裝置定義了兩個「低電量」著色器：**PixelShaderFlat.hlsl** 和 **VertexShaderFlat.hlsl**。 這兩個著色器提供降低過的效果，像是不在紋理表面上反射強光光線。) 最後，還有包含常數緩衝區的格式的 .hlsli 檔案 **ConstantBuffers.hlsli**。

**ConstantBuffers.hlsli** 的定義如下所示：

```cpp
Texture2D diffuseTexture : register(t0);
SamplerState linearSampler : register(s0);

cbuffer ConstantBufferNeverChanges : register(b0)
{
    float4 lightPosition[4];
    float4 lightColor;
}

cbuffer ConstantBufferChangeOnResize : register(b1)
{
    matrix projection;
};

cbuffer ConstantBufferChangesEveryFrame : register(b2)
{
    matrix view;
};

cbuffer ConstantBufferChangesEveryPrim : register (b3)
{
    matrix world;
    float4 meshColor;
    float4 diffuseColor;
    float4 specularColor;
    float  specularExponent;
};

struct VertextShaderInput
{
    float4 position : POSITION;
    float4 normal : NORMAL;
    float2 textureUV : TEXCOORD0;
};

struct PixelShaderInput
{
    float4 position : SV_POSITION;
    float2 textureUV : TEXCOORD0;
    float3 vertexToEye : TEXCOORD1;
    float3 normal : TEXCOORD2;
    float3 vertexToLight0 : TEXCOORD3;
    float3 vertexToLight1 : TEXCOORD4;
    float3 vertexToLight2 : TEXCOORD5;
    float3 vertexToLight3 : TEXCOORD6;
};

struct PixelShaderFlatInput
{
    float4 position : SV_POSITION;
    float2 textureUV : TEXCOORD0;
    float4 diffuseColor : TEXCOORD1;
};
```

**VertexShader.hlsl** 的定義如下所示：

VertexShader.hlsl

```cpp
#include "ConstantBuffers.hlsli"

PixelShaderInput main(VertextShaderInput input)
{
    PixelShaderInput output = (PixelShaderInput)0;

    output.position = mul(mul(mul(input.position, world), view), projection);
    output.textureUV = input.textureUV;

    // compute view space normal
    output.normal = normalize (mul(mul(input.normal.xyz, (float3x3)world), (float3x3)view));

    // Vertex pos in view space (normalize in pixel shader)
    output.vertexToEye = -mul(mul(input.position, world), view).xyz;

    // Compute view space vertex to light vectors (normalized)
    output.vertexToLight0 = normalize(mul(lightPosition[0], view ).xyz + output.vertexToEye);
    output.vertexToLight1 = normalize(mul(lightPosition[1], view ).xyz + output.vertexToEye);
    output.vertexToLight2 = normalize(mul(lightPosition[2], view ).xyz + output.vertexToEye);
    output.vertexToLight3 = normalize(mul(lightPosition[3], view ).xyz + output.vertexToEye);

    return output;
}
```

**VertexShader.hlsl** 中的 **main** 函式會執行我們在相機一節中討論的頂點轉換序列。 它在每個頂點上執行一次。 結果輸出會傳遞到像素著色器程式碼，以產生紋理及材質效果。

PixelShader.hlsl

```cpp
#include "ConstantBuffers.hlsli"

float4 main(PixelShaderInput input) : SV_Target
{
    float diffuseLuminance =
        max(0.0f, dot(input.normal, input.vertexToLight0)) +
        max(0.0f, dot(input.normal, input.vertexToLight1)) +
        max(0.0f, dot(input.normal, input.vertexToLight2)) +
        max(0.0f, dot(input.normal, input.vertexToLight3));

    // Normalize view space vertex-to-eye
    input.vertexToEye = normalize(input.vertexToEye);

    float specularLuminance = 
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight0))), specularExponent) +
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight1))), specularExponent) +
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight2))), specularExponent) +
        pow(max(0.0f, dot(input.normal, normalize(input.vertexToEye + input.vertexToLight3))), specularExponent);

    float4 specular;
    specular = specularColor * specularLuminance * 0.5f;

   return diffuseTexture.Sample(linearSampler, input.textureUV) *  diffuseColor * diffuseLuminance * 0.5f + specular;
}
```

**PixelShader.hlsl** 中的 **main** 函式使用場景中每個基本類型的三角形表面 2D 投影，並根據套用的紋理和效果 (在此例中是反射光源)，以計算可見表面上每個像素的色彩值。

現在，我們將這些概念 (基本類型、相機及著色器) 組合在一起，然後看看範例遊戲如何建立完整的轉譯程序。

## <a name="rendering-the-frame-for-output"></a>轉譯輸出畫面


我們在[定義主要遊戲物件](tutorial--defining-the-main-game-loop.md)中簡短討論了這個方法。 現在，我們再深入看看更多細節。

```cpp
void GameRenderer::Render()
{
    int renderingPasses = 1;
    if (m_stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        if (m_stereoEnabled && i > 0)
        {
            // Doing the Right Eye View
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetViewRight.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmapRight.Get());
        }
        else
        {
            // Doing the Mono or Left Eye View
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetView.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
        }

        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (m_stereoEnabled)
            {
                ConstantBufferChangeOnResize changesOnResize;
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                        )
                    );

                m_d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }
            // Update variables that change one time per frame

            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            m_d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup Pipeline

            m_d3dContext->IASetInputLayout(m_vertexLayout.Get());
            m_d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            m_d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            m_d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            // Get the all the objects to render from the Game state.
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(m_d3dContext.Get(), m_constantBufferChangesEveryPrim.Get());
            }
        }
        else
        {
            const float ClearColor[4] = {0.1f, 0.1f, 0.1f, 1.0f};

            // Only need to clear the background when not rendering the full 3-D scene because
            // the 3-D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (m_stereoEnabled && i > 0)
            {
                // Doing the Right Eye View
                m_d3dContext->ClearRenderTargetView(m_renderTargetViewRight.Get(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View
                m_d3dContext->ClearRenderTargetView(m_renderTargetView.Get(), ClearColor);
            }
        }

        m_d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        m_d2dContext->SetTransform(m_rotationTransform2D);

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game, m_d2dContext.Get(), m_windowBounds);
        }

        if (m_gameInfoOverlay->Visible())
        {
            m_d2dContext->DrawBitmap(
                m_gameInfoOverlay->Bitmap(),
                D2D1::RectF(
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f,
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f + GameInfoOverlayConstant::Width,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f + GameInfoOverlayConstant::Height
                    )
                );
        }

        HRESULT hr = m_d2dContext->EndDraw();
        if (hr != D2DERR_RECREATE_TARGET)
        {
            // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
            // D3D device.  All subsequent rendering will be ignored until the device is recreated.
            // This error will be propagated and the appropriate D3D error will be returned from the
            // swapchain->Present(...) call.   At that point, the sample will recreate the device
            // and all associated resources.  As a result the D2DERR_RECREATE_TARGET doesn't
            // need to be handled here.
            DX::ThrowIfFailed(hr);
        }
    }
    Present();
}
```

遊戲具備組合輸出檢視的所有部分：基本類型及其行為規則、提供玩家遊戲世界視野的相機物件，以及用於繪製的圖形資源。

現在，我們來看看將它們組合在一起的程序。

1.  如果啟用 Stereo 3D，請將下列轉譯程序設定為執行兩次，針對每個眼睛執行一次。
2.  整個場景是包含在外圍世界體積中，所以繪製每個像素 (即使是我們不需要的像素) 來清除轉譯目標的色彩平面。 將深度樣板緩衝區重設為預設值。
3.  使用相機的視圖矩陣及資料，更新畫面更新資料的常數緩衝區。
4.  將 Direct3D 內容設定為使用之前定義的 4 個內容緩衝區。
5.  在每個基本類型物件上呼叫 **Render** 方法。 這會讓內容的 **Draw** 或 **DrawIndexed** 呼叫，繪製每個基本類型的幾何。 具體來說，這個 **Draw** 呼叫會將命令和資料排到 GPU 的佇列，由常數緩衝區資料進行參數化處理。 每個繪圖呼叫會在每個頂點上執行頂點著色器一次，然後像素著色器會在基本類型中每個三角形的每個像素中執行一次。 紋理是像素著色器用來進行轉譯之狀態的一部分。
6.  使用 Direct2D 內容繪製 HUD 和重疊。
7.  呼叫 **DirectXBase::Present**。

而遊戲也已經更新了顯示畫面！ 上述的所有程序都實作遊戲圖形架構的基本程序。 當然，您的遊戲越大，就需要放入更多抽像物件來處理其複雜程度，例如物件類型的整個階層和動畫行為，以及更複雜的載入和管理資產 (網格及紋理) 的方法。

## <a name="next-steps"></a>後續步驟


我們再繼續看看遊戲範例中之前簡略討論過的幾個重要部分：[使用者介面重疊](tutorial--adding-a-user-interface.md)、[輸入控制項](tutorial--adding-controls.md)以及[音效](tutorial--adding-sound.md)。

## <a name="complete-sample-code-for-this-section"></a>這個章節的完整範例程式碼


Camera.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class Camera
{
internal:
    Camera();

    // Call these from client and use Get*Matrix() to read new matrices.
    // Functions to change camera matrices.
    void SetViewParams(_In_ DirectX::XMFLOAT3 eye, _In_ DirectX::XMFLOAT3 lookAt, _In_ DirectX::XMFLOAT3 up);
    void SetProjParams(_In_ float fieldOfView, _In_ float aspectRatio, _In_ float nearPlane, _In_ float farPlane);

    void LookDirection (_In_ DirectX::XMFLOAT3 lookDirection);
    void Eye (_In_ DirectX::XMFLOAT3 position);

    DirectX::XMMATRIX View();
    DirectX::XMMATRIX Projection();
    DirectX::XMMATRIX LeftEyeProjection();
    DirectX::XMMATRIX RightEyeProjection();
    DirectX::XMMATRIX World();
    DirectX::XMFLOAT3 Eye();
    DirectX::XMFLOAT3 LookAt();
    DirectX::XMFLOAT3 Up();
    float NearClipPlane();
    float FarClipPlane();
    float Pitch();
    float Yaw();

protected private:
    DirectX::XMFLOAT4X4 m_viewMatrix;                 // View matrix
    DirectX::XMFLOAT4X4 m_projectionMatrix;           // Projection matrix
    DirectX::XMFLOAT4X4 m_projectionMatrixLeft;       // Projection Matrix for Left Eye Stereo
    DirectX::XMFLOAT4X4 m_projectionMatrixRight;      // Projection Matrix for Right Eye Stereo

    DirectX::XMFLOAT4X4 m_inverseView;

    DirectX::XMFLOAT3 m_eye;                        // Camera eye position
    DirectX::XMFLOAT3 m_lookAt;                     // LookAt position
    DirectX::XMFLOAT3 m_up;                         // Up vector
    float             m_cameraYawAngle;             // Yaw angle of camera
    float             m_cameraPitchAngle;           // Pitch angle of camera

    float             m_fieldOfView;                // Field of view
    float             m_aspectRatio;                // Aspect ratio
    float             m_nearPlane;                  // Near plane
    float             m_farPlane;                   // Far plane
};
```

Camera.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Camera.h"
#include "StereoProjection.h"

using namespace DirectX;

#undef min // use __min instead
#undef max // use __max instead
//--------------------------------------------------------------------------------------
// Constructor
//--------------------------------------------------------------------------------------
Camera::Camera()
{
    // Setup the view matrix.
    SetViewParams(
        XMFLOAT3(0.0f, 0.0f, 0.0f),   // default eye position.
        XMFLOAT3(0.0f, 0.0f, 1.0f),   // default look at position.
        XMFLOAT3(0.0f, 1.0f, 0.0f)    // default up vector.
        );

    // Setup the projection matrix.
    SetProjParams(XM_PI / 4, 1.0f, 1.0f, 1000.0f);
}
//--------------------------------------------------------------------------------------
void Camera::LookDirection (_In_ XMFLOAT3 lookDirection)
{
    XMFLOAT3 lookAt;
    lookAt.x = m_eye.x + lookDirection.x;
    lookAt.y = m_eye.y + lookDirection.y;
    lookAt.z = m_eye.z + lookDirection.z;

    SetViewParams(m_eye, lookAt, m_up);
}
//--------------------------------------------------------------------------------------
void Camera::Eye(_In_ XMFLOAT3 eye)
{
    SetViewParams(eye, m_lookAt, m_up);
}
//--------------------------------------------------------------------------------------
void Camera::SetViewParams(
    _In_ XMFLOAT3 eye,
    _In_ XMFLOAT3 lookAt,
    _In_ XMFLOAT3 up
    )
{
    m_eye = eye;
    m_lookAt = lookAt;
    m_up = up;

    // Calculate the view matrix.
    XMMATRIX view = XMMatrixLookAtLH(
        XMLoadFloat3(&m_eye),
        XMLoadFloat3(&m_lookAt),
        XMLoadFloat3(&m_up)
        );

    XMVECTOR det;
    XMMATRIX inverseView = XMMatrixInverse(&det, view);
    XMStoreFloat4x4(&m_viewMatrix, view);
    XMStoreFloat4x4(&m_inverseView, inverseView);

    // The axis basis vectors and camera position are stored inside the
    // position matrix in the four rows of the camera's world matrix.
    // To figure out the yaw/pitch of the camera, we just need the Z basis vector.
    XMFLOAT3 zBasis;
    XMStoreFloat3(&zBasis, inverseView.r[2]);

    m_cameraYawAngle = atan2f(zBasis.x, zBasis.z);

    float len = sqrtf(zBasis.z * zBasis.z + zBasis.x * zBasis.x);
    m_cameraPitchAngle = atan2f(zBasis.y, len);
}
//--------------------------------------------------------------------------------------
// Calculates the projection matrix based on input params.
//--------------------------------------------------------------------------------------
void Camera::SetProjParams(
    _In_ float fieldOfView,
    _In_ float aspectRatio,
    _In_ float nearPlane,
    _In_ float farPlane
    )
{
    // Set attributes for the projection matrix.
    m_fieldOfView = fieldOfView;
    m_aspectRatio = aspectRatio;
    m_nearPlane = nearPlane;
    m_farPlane = farPlane;
    XMStoreFloat4x4(
        &m_projectionMatrix,
        XMMatrixPerspectiveFovLH(
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane
            )
        );

    STEREO_PARAMETERS* stereoParams = nullptr;
    // Change the projection matrix.
    XMStoreFloat4x4(
        &m_projectionMatrixLeft,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::LEFT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );

    XMStoreFloat4x4(
        &m_projectionMatrixRight,
        MatrixStereoProjectionFovLH(
            stereoParams,
            STEREO_CHANNEL::RIGHT,
            m_fieldOfView,
            m_aspectRatio,
            m_nearPlane,
            m_farPlane,
            STEREO_MODE::NORMAL
            )
        );
}
//--------------------------------------------------------------------------------------
DirectX::XMMATRIX Camera::View()
{
    return XMLoadFloat4x4(&m_viewMatrix);
}
DirectX::XMMATRIX Camera::Projection()
{
    return XMLoadFloat4x4(&m_projectionMatrix);
}
DirectX::XMMATRIX Camera::LeftEyeProjection()
{
    return XMLoadFloat4x4(&m_projectionMatrixLeft);
}
DirectX::XMMATRIX Camera::RightEyeProjection()
{
    return XMLoadFloat4x4(&m_projectionMatrixRight);
}
DirectX::XMMATRIX Camera::World()
{
    return XMLoadFloat4x4(&m_inverseView);
}
DirectX::XMFLOAT3 Camera::Eye()
{
    return m_eye;
}
DirectX::XMFLOAT3 Camera::LookAt()
{
    return m_lookAt;
}
float Camera::NearClipPlane()
{
    return m_nearPlane;
}
float Camera::FarClipPlane()
{
    return m_farPlane;
}
float Camera::Pitch()
{
    return m_cameraPitchAngle;
}
float Camera::Yaw()
{
    return m_cameraYawAngle;
}
```

GameRenderer.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "DirectXBase.h"
#include "GameInfoOverlay.h"
#include "GameHud.h"
#include "Simple3DGame.h"

ref class Simple3DGame;
ref class GameHud;

ref class GameRenderer : public DirectXBase
{
internal:
    GameRenderer();

    virtual void Initialize(
        _In_ Windows::UI::Core::CoreWindow^ window,
        float dpi
        ) override;

    virtual void CreateDeviceIndependentResources() override;
    virtual void CreateDeviceResources() override;
    virtual void UpdateForWindowSizeChange() override;
    virtual void Render() override;
    virtual void SetDpi(float dpi) override;

    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    GameInfoOverlay^ InfoOverlay()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f
            );
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(
            (m_windowBounds.Width  - GameInfoOverlayConstant::Width) / 2.0f + GameInfoOverlayConstant::Width,
            (m_windowBounds.Height - GameInfoOverlayConstant::Height) / 2.0f + GameInfoOverlayConstant::Height
            );
    };

protected private:
    bool                                                m_initialized;
    bool                                                m_gameResourcesLoaded;
    bool                                                m_levelResourcesLoaded;
    GameInfoOverlay^                                    m_gameInfoOverlay;
    GameHud^                                            m_gameHud;
    Simple3DGame^                                       m_game;

    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers.
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

GameRenderer.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "DirectXSample.h"
#include "GameRenderer.h"
#include "ConstantBuffers.h"
#include "TargetTexture.h"
#include "BasicLoader.h"
#include "CylinderMesh.h"
#include "FaceMesh.h"
#include "SphereMesh.h"
#include "WorldMesh.h"
#include "Face.h"
#include "Sphere.h"
#include "Cylinder.h"

using namespace concurrency;
using namespace DirectX;
using namespace Microsoft::WRL;
using namespace Windows::UI::Core;

//----------------------------------------------------------------------

GameRenderer::GameRenderer() :
    m_initialized(false),
    m_gameResourcesLoaded(false),
    m_levelResourcesLoaded(false)
{
}

//----------------------------------------------------------------------

void GameRenderer::Initialize(
    _In_ CoreWindow^ window,
    float dpi
    )
{
    if (!m_initialized)
    {
        m_gameHud = ref new GameHud(
            "Windows 8 Samples",
            "DirectX first-person game sample"
            );
        m_gameInfoOverlay = ref new GameInfoOverlay();
        m_initialized = true;
    }

    DirectXBase::Initialize(window, dpi);

    // Initialize could be called multiple times as a result of an error with the hardware device
    // that requires it to be reinitialized.  Since the m_gameInfoOverlay has resources that are
    // dependent on the device, it will need to be reinitialized each time with the new device information.
    m_gameInfoOverlay->Initialize(m_d2dDevice.Get(), m_d2dContext.Get(), m_dwriteFactory.Get(), dpi);
}

//----------------------------------------------------------------------

void GameRenderer::CreateDeviceIndependentResources()
{
    DirectXBase::CreateDeviceIndependentResources();
    m_gameHud->CreateDeviceIndependentResources(m_dwriteFactory.Get(), m_wicFactory.Get());
}

//----------------------------------------------------------------------

void GameRenderer::CreateDeviceResources()
{
    DirectXBase::CreateDeviceResources();

    m_gameHud->CreateDeviceResources(m_d2dContext.Get());

    if (m_game != nullptr)
    {
        // The initial invocation of CreateDeviceResources will occur
        // before the Game State is initialized when the device is first
        // being created, so that the inital loading screen can be displayed.
        // Subsequent invocations of CreateDeviceResources will be a result
        // of an error with the Device that requires the resources to be
        // recreated.  In this case, the game state is already initialized
        // so the game device resources need to be recreated.

        // This sample doesn't gracefully handle all the async recreation
        // of resources, so an exception is thrown.
        throw Platform::Exception::CreateException(
            DXGI_ERROR_DEVICE_REMOVED,
            "GameRenderer::CreateDeviceResources - Recreation of resources after TDR not available\n"
            );
    }
}

//----------------------------------------------------------------------

void GameRenderer::UpdateForWindowSizeChange()
{
    DirectXBase::UpdateForWindowSizeChange();

    m_gameHud->UpdateForWindowSizeChange(m_windowBounds);

    // Update the Projection Matrix and update the associated Constant Buffer.
    if (m_game != nullptr)
    {
        m_game->GameCamera()->SetProjParams(
            XM_PI / 2, m_renderTargetSize.Width / m_renderTargetSize.Height,
            0.01f,
            100.0f
            );
        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                )
            );

        m_d3dContext->UpdateSubresource(
            m_constantBufferChangeOnResize.Get(),
            0,
            nullptr,
            &changesOnResize,
            0,
            0
            );
    }
}

//----------------------------------------------------------------------

void GameRenderer::SetDpi(float dpi)
{
    DirectXBase::SetDpi(dpi);

    if (m_gameInfoOverlay)
    {
        m_gameInfoOverlay->SetDpi(dpi);
    }
}

//----------------------------------------------------------------------

task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Set the Loading state to wait until any async resources have
    // been loaded before proceeding.
    m_game = game;

    // NOTE: Only the m_d3dDevice is used in this method.  It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the m_d3dDevice are free-threaded and are safe while any methods
    // in the m_d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.

    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers,
    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
    bd.CPUAccessFlags = 0;
    bd.ByteWidth = (sizeof(ConstantBufferNeverChanges) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangeOnResize) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangeOnResize)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryFrame) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryFrame)
        );

    bd.ByteWidth = (sizeof(ConstantBufferChangesEveryPrim) + 15) / 16 * 16;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferChangesEveryPrim)
        );

    D3D11_SAMPLER_DESC sampDesc;
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;
    sampDesc.MinLOD = 0;
    sampDesc.MaxLOD = FLT_MAX;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    m_cylinderTexture = nullptr;
    m_ceilingTexture = nullptr;
    m_floorTexture = nullptr;
    m_wallsTexture = nullptr;

    // Load Game specific textures.
    tasks.push_back(loader->LoadTextureAsync("seafloor.dds", nullptr, &m_sphereTexture));
    tasks.push_back(loader->LoadTextureAsync("metal_texture.dds", nullptr, &m_cylinderTexture));
    tasks.push_back(loader->LoadTextureAsync("cellceiling.dds", nullptr, &m_ceilingTexture));
    tasks.push_back(loader->LoadTextureAsync("cellfloor.dds", nullptr, &m_floorTexture));
    tasks.push_back(loader->LoadTextureAsync("cellwall.dds", nullptr, &m_wallsTexture));
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Return the task group of all the async tasks for loading the shader and texture assets.
    return when_all(tasks.begin(), tasks.end());
}

//----------------------------------------------------------------------

void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now, associate all the resources with the appropriate
    // Game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[1] = XMFLOAT4( 3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[2] = XMFLOAT4(-3.5f, 2.5f, -5.5f, 1.0f);
    constantBufferNeverChanges.lightPosition[3] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);
    m_d3dContext->UpdateSubresource(m_constantBufferNeverChanges.Get(), 0, nullptr, &constantBufferNeverChanges, 0, 0);

    // For the targets, there are two unique generated textures.
    // Each texture image includes the number of the texture.
    // Make sure the 2D rendering is occurring on the same thread
    // as the main rendering.

    TargetTexture^ textureGenerator = ref new TargetTexture(
        m_d3dDevice.Get(),
        m_d2dFactory.Get(),
        m_dwriteFactory.Get(),
        m_d2dContext.Get()
        );

    MeshObject^ cylinderMesh = ref new CylinderMesh(m_d3dDevice.Get(), 26);
    MeshObject^ targetMesh = ref new FaceMesh(m_d3dDevice.Get());
    MeshObject^ sphereMesh = ref new SphereMesh(m_d3dDevice.Get(), 26);

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    Material^ sphereMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(0.8f, 0.4f, 0.0f, 1.0f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        50.0f,
        m_sphereTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldFloorMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldCeilingId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_ceilingTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldCeilingMesh(m_d3dDevice.Get()));
        }
        else if ((*object)->TargetId() == GameConstants::WorldWallsId)
        {
           (*object)->NormalMaterial(
               ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_wallsTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
           (*object)->Mesh(ref new WorldWallsMesh(m_d3dDevice.Get()));
        }
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        else if (Sphere^ sphere = dynamic_cast<Sphere^>(*object))
        {
            sphere->Mesh(sphereMesh);
            sphere->NormalMaterial(sphereMaterial);
        }
    }

    // Ensure that the camera has been initialized with the right Projection
    // matrix.  The camera is not created at the time the first window resize event
    // occurs.
    m_game->GameCamera()->SetProjParams(
        XM_PI / 2, m_renderTargetSize.Width / m_renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that Projection matrix has been set in the constant buffer
    // now that all the resources are loaded.
    // DirectXBase is handling screen rotations directly to eliminate an unaligned
    // fullscreen copy.  As a result, it is necessary to post multiply the rotationTransform3D
    // matrix to the camera projection matrix.
    ConstantBufferChangeOnResize changesOnResize;
    XMStoreFloat4x4(
        &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
                XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                )
        );

    m_d3dContext->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    m_gameResourcesLoaded = true;
}

//----------------------------------------------------------------------

task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        // This is where additional async loading of level specific resources
        // would be done.  Because there aren't any to load, just simulate
        // by delaying for some time.
        wait(GameConstants::LevelLoadingDelay);
    });
}

//----------------------------------------------------------------------

void GameRenderer::FinalizeLoadLevelResources()
{
    // After the level specific resources had been loaded, this method is
    // where D3D context specific actions would be handled.  This method
    // runs in the same thread context as the GameRenderer was created.

    m_levelResourcesLoaded = true;
}

//----------------------------------------------------------------------

void GameRenderer::Render()
{
    int renderingPasses = 1;
    if (m_stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        if (m_stereoEnabled && i > 0)
        {
            // Doing the Right Eye View.
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetViewRight.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmapRight.Get());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            m_d3dContext->OMSetRenderTargets(1, m_renderTargetView.GetAddressOf(), m_depthStencilView.Get());
            m_d3dContext->ClearDepthStencilView(m_depthStencilView.Get(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
        }

        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (m_stereoEnabled)
            {
                ConstantBufferChangeOnResize changesOnResize;
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&m_rotationTransform3D))
                        )
                    );

                m_d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }
            // Update variables that change one time per frame.

            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            m_d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Set up Pipeline.

            m_d3dContext->IASetInputLayout(m_vertexLayout.Get());
            m_d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            m_d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            m_d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            m_d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            // Get all the objects to render from the Game state.
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(m_d3dContext.Get(), m_constantBufferChangesEveryPrim.Get());
            }
        }
        else
        {
            const float ClearColor[4] = {0.1f, 0.1f, 0.1f, 1.0f};

            // Only need to clear the background when not rendering the full 3-D scene because
            // the 3-D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (m_stereoEnabled && i > 0)
            {
                // Doing the Right Eye View.
                m_d3dContext->ClearRenderTargetView(m_renderTargetViewRight.Get(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                m_d3dContext->ClearRenderTargetView(m_renderTargetView.Get(), ClearColor);
            }
        }

        m_d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        m_d2dContext->SetTransform(m_rotationTransform2D);

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game, m_d2dContext.Get(), m_windowBounds);
        }

        if (m_gameInfoOverlay->Visible())
        {
            m_d2dContext->DrawBitmap(
                m_gameInfoOverlay->Bitmap(),
                D2D1::RectF(
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f,
                    (m_windowBounds.Width - GameInfoOverlayConstant::Width)/2.0f + GameInfoOverlayConstant::Width,
                    (m_windowBounds.Height - GameInfoOverlayConstant::Height)/2.0f + GameInfoOverlayConstant::Height
                    )
                );
        }

        HRESULT hr = m_d2dContext->EndDraw();
        if (hr != D2DERR_RECREATE_TARGET)
        {
            // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
            // D3D device.  All subsequent rendering will be ignored until the device is recreated.
            // This error will be propagated and the appropriate D3D error will be returned from the
            // swapchain->Present(...) call.   At that point, the sample will recreate the device
            // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
            // need to be handled here.
            DX::ThrowIfFailed(hr);
        }
    }
    Present();
}
```

GameObject.h

```cpp
/// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MeshObject.h"
#include "SoundEffect.h"
#include "Animate.h"
#include "Material.h"

ref class GameObject
{
internal:
    GameObject();

    // Expect these two functions to be overloaded by subclasses.
    virtual bool IsTouching(
        DirectX::XMFLOAT3 /* point */,
        float /* radius */,
        _Out_ DirectX::XMFLOAT3 *contact,
        _Out_ DirectX::XMFLOAT3 *normal
        )
    {
        *contact = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);
        *normal = DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f);
        return false;
    };

    void Render(
        _In_ ID3D11DeviceContext *context,
        _In_ ID3D11Buffer *primitiveConstantBuffer
        );

    void Active(bool active);
    bool Active();
    void Target(bool target);
    bool Target();
    void Hit(bool hit);
    bool Hit();
    void OnGround(bool ground);
    bool OnGround();
    void TargetId(int targetId);
    int  TargetId();
    void HitTime(float t);
    float HitTime();

    void     AnimatePosition(_In_opt_ Animate^ animate);
    Animate^ AnimatePosition();

    void         HitSound(_In_ SoundEffect^ hitSound);
    SoundEffect^ HitSound();

    void PlaySound(float impactSpeed, DirectX::XMFLOAT3 eyePoint);

    void Mesh(_In_ MeshObject^ mesh);

    void NormalMaterial(_In_ Material^ material);
    Material^ NormalMaterial();
    void HitMaterial(_In_ Material^ material);
    Material^ HitMaterial();

    void Position(DirectX::XMFLOAT3 position);
    void Position(DirectX::XMVECTOR position);
    void Velocity(DirectX::XMFLOAT3 velocity);
    void Velocity(DirectX::XMVECTOR velocity);
    DirectX::XMMATRIX ModelMatrix();
    DirectX::XMFLOAT3 Position();
    DirectX::XMVECTOR VectorPosition();
    DirectX::XMVECTOR VectorVelocity();
    DirectX::XMFLOAT3 Velocity();

protected private:
    virtual void UpdatePosition() {};
    // Object Data.
    bool                m_active;
    bool                m_target;
    int                 m_targetId;
    bool                m_hit;
    bool                m_ground;

    DirectX::XMFLOAT3   m_position;
    DirectX::XMFLOAT3   m_velocity;
    DirectX::XMFLOAT4X4 m_modelMatrix;

    Material^           m_normalMaterial;
    Material^           m_hitMaterial;

    DirectX::XMFLOAT3   m_defaultXAxis;
    DirectX::XMFLOAT3   m_defaultYAxis;
    DirectX::XMFLOAT3   m_defaultZAxis;

    float               m_hitTime;

    Animate^            m_animatePosition;
    MeshObject^         m_mesh;

    SoundEffect^        m_hitSound;
};


__forceinline void GameObject::Active(bool active)
{
    m_active = active;
}

__forceinline bool GameObject::Active()
{
    return m_active;
}

__forceinline void GameObject::Target(bool target)
{
    m_target = target;
}

__forceinline bool GameObject::Target()
{
    return m_target;
}

__forceinline void GameObject::Hit(bool hit)
{
    m_hit = hit;
}

__forceinline bool GameObject::Hit()
{
    return m_hit;
}

__forceinline void GameObject::OnGround(bool ground)
{
    m_ground = ground;
}

__forceinline bool GameObject::OnGround()
{
    return m_ground;
}

__forceinline void GameObject::TargetId(int targetId)
{
    m_targetId = targetId;
}

__forceinline int GameObject::TargetId()
{
    return m_targetId;
}

__forceinline void GameObject::HitTime(float t)
{
    m_hitTime = t;
}

__forceinline float GameObject::HitTime()
{
    return m_hitTime;
}

__forceinline void GameObject::Position(DirectX::XMFLOAT3 position)
{
    m_position = position;
    // Update any internal states that are dependent on the position.
    // UpdatePosition is a virtual function that is specific to the derived class.
    UpdatePosition();
}

__forceinline void GameObject::Position(DirectX::XMVECTOR position)
{
    XMStoreFloat3(&m_position, position);
   // Update any internal states that are dependent on the position.
    // UpdatePosition is a virtual function that is specific to the derived class.
    UpdatePosition();
}

__forceinline DirectX::XMFLOAT3 GameObject::Position()
{
    return m_position;
}

__forceinline DirectX::XMVECTOR GameObject::VectorPosition()
{
    return DirectX::XMLoadFloat3(&m_position);
}

__forceinline void GameObject::Velocity(DirectX::XMFLOAT3 velocity)
{
    m_velocity = velocity;
}

__forceinline void GameObject::Velocity(DirectX::XMVECTOR velocity)
{
    XMStoreFloat3(&m_velocity, velocity);
}

__forceinline DirectX::XMFLOAT3 GameObject::Velocity()
{
    return m_velocity;
}

__forceinline DirectX::XMVECTOR GameObject::VectorVelocity()
{
    return DirectX::XMLoadFloat3(&m_velocity);
}

__forceinline void GameObject::AnimatePosition(_In_opt_ Animate^ animate)
{
    m_animatePosition = animate;
}

__forceinline Animate^ GameObject::AnimatePosition()
{
    return m_animatePosition;
}

__forceinline void GameObject::NormalMaterial(_In_ Material^ material)
{
    m_normalMaterial = material;
}

__forceinline Material^ GameObject::NormalMaterial()
{
    return m_normalMaterial;
}

__forceinline void GameObject::HitMaterial(_In_ Material^ material)
{
    m_hitMaterial = material;
}

__forceinline Material^ GameObject::HitMaterial()
{
    return m_hitMaterial;
}

__forceinline void GameObject::Mesh(_In_ MeshObject^ mesh)
{
    m_mesh = mesh;
}

__forceinline void GameObject::HitSound(_In_ SoundEffect^ hitSound)
{
    m_hitSound = hitSound;
}

__forceinline SoundEffect^ GameObject::HitSound()
{
    return m_hitSound;
}

__forceinline DirectX::XMMATRIX GameObject::ModelMatrix()
{
    return DirectX::XMLoadFloat4x4(&m_modelMatrix);
}
```

GameObject.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "GameObject.h"
#include "ConstantBuffers.h"
#include "GameConstants.h"

using namespace DirectX;

//--------------------------------------------------------------------------------

GameObject::GameObject() :
    m_normalMaterial(nullptr),
    m_hitMaterial(nullptr)
{
    m_active          = false;
    m_target          = false;
    m_targetId        = 0;
    m_hit             = false;
    m_ground          = true;

    m_position        = XMFLOAT3(0.0f, 0.0f, 0.0f);
    m_velocity        = XMFLOAT3(0.0f, 0.0f, 0.0f);
    m_defaultXAxis    = XMFLOAT3(1.0f, 0.0f, 0.0f);
    m_defaultYAxis    = XMFLOAT3(0.0f, 1.0f, 0.0f);
    m_defaultZAxis    = XMFLOAT3(0.0f, 0.0f, 1.0f);
    XMStoreFloat4x4(&m_modelMatrix, XMMatrixIdentity());

    m_hitTime         = 0.0f;

    m_animatePosition = nullptr;
}

//----------------------------------------------------------------------

void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    m_mesh->Render(context);
}

//----------------------------------------------------------------------

void GameObject::PlaySound(float impactSpeed, XMFLOAT3 eyePoint)
{
    if (m_hitSound != nullptr)
    {
        // Determine the sound volume adjustment based on velocity.
        float adjustment;
        if (impactSpeed < GameConstants::Sound::MinVelocity)
        {
            adjustment = 0.0f;  // Too soft.  Don't play sound.
        }
        else
        {
            adjustment = min(1.0f, impactSpeed / GameConstants::Sound::MaxVelocity);
            adjustment = GameConstants::Sound::MinAdjustment + adjustment * (1.0f - GameConstants::Sound::MinAdjustment);
        }

        // Compute Distance to eyePoint to adjust the volume based on that distance.
        XMVECTOR cameraToPosition = XMLoadFloat3(&eyePoint) - VectorPosition();
        float distToPositionSquared = XMVectorGetX(XMVector3LengthSq(cameraToPosition));

        float volume = min(distToPositionSquared, 1.0f);
        // Scale
        // Sound is proportional to how hard the ball is hitting.
        volume = adjustment * volume;

        m_hitSound->PlaySound(volume);
    }
}
```

Animate.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Animate:
// This is an abstract class for animations.  It defines a set of
// capabilities for animating XMFLOAT3 variables.  An animation has the following
// characteristics:
//     Start - the time for the animation to start.
//     Duration - the length of time the animation is to run.
//     Continuous - whether the animation loops after duration is reached or just
//         stops.
// There are two query functions:
//     IsActive - determines if the animation is active at time t.
//     IsFinished - determines if the animation is done at time t.
// It is expected that each derived class will provide an Evaluate method for the
// specific kind of animation.

ref class Animate abstract
{
internal:
    Animate();

    virtual DirectX::XMFLOAT3 Evaluate (_In_ float t) = 0;

    bool  IsActive(_In_ float t) { return ((t >= m_startTime) && (m_continuous || (t < (m_startTime + m_duration)))); };
    bool  IsFinished(_In_ float t) { return (!m_continuous && (t >= (m_startTime + m_duration))); }
    float Start();
    void  Start(_In_ float start);
    float Duration();
    void  Duration(_In_ float duration);
    bool  Continuous();
    void  Continuous(_In_ bool continuous);

protected private:
    bool  m_continuous;      // if true means at end cycle back to beginning
    float m_startTime;       // time at which animation begins
    float m_duration;        // for continuous, this is the duration of 1 cycle through path
};

//----------------------------------------------------------------------

// AnimateLinePosition:
// This class is a specialization of Animate that defines an animation of a position vector
// along a straight line defined in world coordinates from startPosition to endPosition.

ref class AnimateLinePosition: public Animate
{
internal:
    AnimateLinePosition(
        _In_ DirectX::XMFLOAT3 startPosition,
        _In_ DirectX::XMFLOAT3 endPosition,
        _In_ float duration,
        _In_ bool continuous
        );
    virtual DirectX::XMFLOAT3 Evaluate(_In_ float t) override;

private:
    DirectX::XMFLOAT3 m_startPosition;
    DirectX::XMFLOAT3 m_endPosition;
    float             m_length;
};

//----------------------------------------------------------------------

struct LineSegment
{
    DirectX::XMFLOAT3 position;
    float             length;
    float             uStart;
    float             uLength;
};

// AnimateLineListPosition:
// This class is a specialization of Animate that defines an animation of a position vector
// along a set of line segments defined by a set of points.  The animation along the path is
// such that the evaluation of the position along the path will be uniform independent of
// the length of each of the line segments.  A continuous loop can be achieved by having the
// first and last points of the list be the same.

ref class AnimateLineListPosition: public Animate
{
internal:
    AnimateLineListPosition(
        _In_ unsigned int count,
        _In_reads_(count) DirectX::XMFLOAT3 position[],
        _In_ float duration,
        _In_ bool continuous
        );
    virtual DirectX::XMFLOAT3 Evaluate(_In_ float t) override;

private:
    unsigned int             m_count;
    float                    m_totalLength;
    std::vector<LineSegment> m_segment;
};

//----------------------------------------------------------------------

// AnimateCirclePosition:
// This class is a specialization of Animate that defines an animation of a position vector
// along a circular path centered at 'center' with a starting point of 'startPosition'.  The
// distance between 'center' and 'startPosition' defines the radius of the circle.  The plane
// of the circle will pass through 'center' and 'startPosition' and have a normal of 'planeNormal'.
// The direction of the animation can be either clockwise or counterclockwise based
// on the 'clockwise' parameter.

ref class AnimateCirclePosition: public Animate
{
internal:
    AnimateCirclePosition(
        _In_ DirectX::XMFLOAT3 center,
        _In_ DirectX::XMFLOAT3 startPosition,
        _In_ DirectX::XMFLOAT3 planeNormal,
        _In_ float duration,
        _In_ bool continuous,
        _In_ bool clockwise
        );
    virtual DirectX::XMFLOAT3 Evaluate(_In_ float t) override;

private:
    DirectX::XMFLOAT4X4 m_rotationMatrix;
    DirectX::XMFLOAT3   m_center;
    DirectX::XMFLOAT3   m_planeNormal;
    DirectX::XMFLOAT3   m_startPosition;
    float               m_radius;
    bool                m_clockwise;
};

//----------------------------------------------------------------------
            
            
```

Animate.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Animate.h"

using namespace DirectX;

//----------------------------------------------------------------------

Animate::Animate():
    m_continuous(false),
    m_startTime(0.0f),
    m_duration(10.0f)
{
}

//----------------------------------------------------------------------

float Animate::Start()
{
    return m_startTime;
}

//----------------------------------------------------------------------

void Animate::Start(_In_ float start)
{
    m_startTime = start;
}

//----------------------------------------------------------------------

float Animate::Duration()
{
    return m_duration;
}

//----------------------------------------------------------------------

void Animate::Duration(_In_ float duration)
{
    m_duration = duration;
}

//----------------------------------------------------------------------

bool Animate::Continuous()
{
    return m_continuous;
}

//----------------------------------------------------------------------

void Animate::Continuous(_In_ bool continuous)
{
    m_continuous = continuous;
}

//----------------------------------------------------------------------

AnimateLinePosition::AnimateLinePosition(
    _In_ XMFLOAT3 startPosition,
    _In_ XMFLOAT3 endPosition,
    _In_ float duration,
    _In_ bool continuous)
{
    m_startPosition = startPosition;
    m_endPosition = endPosition;
    m_duration = duration;
    m_continuous = continuous;

    m_length = XMVectorGetX(
        XMVector3Length(XMLoadFloat3(&endPosition) - XMLoadFloat3(&startPosition))
        );
}

//----------------------------------------------------------------------

XMFLOAT3 AnimateLinePosition::Evaluate(_In_ float t)
{
    if (t <= m_startTime)
    {
        return m_startPosition;
    }

    if ((t >= (m_startTime + m_duration)) && !m_continuous)
    {
        return m_endPosition;
    }

    float startTime = m_startTime;
    if (m_continuous)
    {
        // For continuous operation, move the start time forward to
        // eliminate previous iterations.
        startTime += ((int)((t - m_startTime) / m_duration)) * m_duration;
    }

    float u = (t - startTime) / m_duration;
    XMFLOAT3 currentPosition;
    currentPosition.x = m_startPosition.x + (m_endPosition.x - m_startPosition.x)*u;
    currentPosition.y = m_startPosition.y + (m_endPosition.y - m_startPosition.y)*u;
    currentPosition.z = m_startPosition.z + (m_endPosition.z - m_startPosition.z)*u;

    return currentPosition;
}

//----------------------------------------------------------------------

AnimateLineListPosition::AnimateLineListPosition(
    _In_ unsigned int count,
    _In_reads_(count) XMFLOAT3 position[],
    _In_ float duration,
    _In_ bool continuous)
{
    m_duration = duration;
    m_continuous = continuous;
    m_count = count;

    std::vector<LineSegment> segment(m_count);
    m_segment = segment;
    m_totalLength = 0.0f;

    m_segment[0].position = position[0];
    for (unsigned int i = 1; i < count; i++)
    {
        m_segment[i].position = position[i];
        m_segment[i - 1].length = XMVectorGetX(
            XMVector3Length(
                XMLoadFloat3(&m_segment[i].position) -
                XMLoadFloat3(&m_segment[i - 1].position)
                )
            );
        m_totalLength += m_segment[i - 1].length;
    }

    // Parameterize the segments to ensure uniform evaluation along the path.
    float u = 0.0f;
    for (unsigned int i = 0; i < (count - 1); i++)
    {
        m_segment[i].uStart = u;
        m_segment[i].uLength = (m_segment[i].length / m_totalLength);
        u += m_segment[i].uLength;
    }
    m_segment[count-1].uStart = 1.0f;
}

//----------------------------------------------------------------------

XMFLOAT3 AnimateLineListPosition::Evaluate(_In_ float t)
{
    if (t <= m_startTime)
    {
        return m_segment[0].position;
    }

    if ((t >= (m_startTime + m_duration)) && !m_continuous)
    {
        return m_segment[m_count-1].position;
    }

    float startTime = m_startTime;
    if (m_continuous)
    {
        // For continuous operation, move the start time forward to
        // eliminate previous iterations.
        startTime += ((int)((t - m_startTime) / m_duration)) * m_duration;
    }

    float u = (t - startTime) / m_duration;
    // Find the right segment.
    unsigned int i = 0;
    while (u > m_segment[i + 1].uStart)
    {
        i++;
    }

    u -= m_segment[i].uStart;
    u /= m_segment[i].uLength;

    XMFLOAT3 currentPosition;
    currentPosition.x = m_segment[i].position.x + (m_segment[i + 1].position.x - m_segment[i].position.x)*u;
    currentPosition.y = m_segment[i].position.y + (m_segment[i + 1].position.y - m_segment[i].position.y)*u;
    currentPosition.z = m_segment[i].position.z + (m_segment[i + 1].position.z - m_segment[i].position.z)*u;

    return currentPosition;
}

//----------------------------------------------------------------------

AnimateCirclePosition:: AnimateCirclePosition(
    _In_ XMFLOAT3 center,
    _In_ XMFLOAT3 startPosition,
    _In_ XMFLOAT3 planeNormal,
    _In_ float duration,
    _In_ bool continuous,
    _In_ bool clockwise)
{
    m_center = center;
    m_planeNormal = planeNormal;
    m_startPosition = startPosition;
    m_duration = duration;
    m_continuous = continuous;
    m_clockwise = clockwise;

    XMVECTOR coordX = XMLoadFloat3(&m_startPosition) - XMLoadFloat3(&m_center);
    m_radius = XMVectorGetX(XMVector3Length(coordX));

    XMVector3Normalize(coordX);

    XMVECTOR coordZ = XMLoadFloat3(&m_planeNormal);
    XMVector3Normalize(coordZ);

    XMVECTOR coordY;
    if (m_clockwise)
    {
        coordY = XMVector3Cross(coordZ, coordX);
    }
    else
    {
        coordY = XMVector3Cross(coordX, coordZ);
    }

    XMVECTOR vectorX = XMVectorSet(1.0f, 0.0f, 0.0f, 1.0f);
    XMVECTOR vectorY = XMVectorSet(0.0f, 1.0f, 0.0f, 1.0f);
    XMMATRIX mat1 = XMMatrixIdentity();
    XMMATRIX mat2 = XMMatrixIdentity();

    if (!XMVector3Equal(coordX, vectorX))
    {
        float angle;
        angle = XMVectorGetX(
            XMVector3AngleBetweenVectors(vectorX, coordX)
            );
        if ((angle * angle) > 0.025)
        {
            XMVECTOR axis1 = XMVector3Cross(vectorX, coordX);

            mat1 = XMMatrixRotationAxis(axis1, angle);
            vectorY = XMVector3TransformCoord(vectorY, mat1);
        }
    }
    if (!XMVector3Equal(vectorY, coordY))
    {
        float angle;
        angle = XMVectorGetX(
            XMVector3AngleBetweenVectors(vectorY, coordY)
            );
        if ((angle * angle) > 0.025)
        {
            XMVECTOR axis2 = XMVector3Cross(vectorY, coordY);
            mat2 = XMMatrixRotationAxis(axis2, angle);
        }
    }
    XMStoreFloat4x4(
        &m_rotationMatrix,
        mat1 *
        mat2 *
        XMMatrixTranslation(m_center.x, m_center.y, m_center.z)
        );
}

//----------------------------------------------------------------------

XMFLOAT3 AnimateCirclePosition::Evaluate(_In_ float t)
{
    if (t <= m_startTime)
    {
        return m_startPosition;
    }

    if ((t >= (m_startTime + m_duration)) && !m_continuous)
    {
        return m_startPosition;
    }

    float startTime = m_startTime;
    if (m_continuous)
    {
        // For continuous operation, move the start time forward to
        // eliminate previous iterations.
        startTime += ((int)((t - m_startTime) / m_duration)) * m_duration;
    }

    float u = (t - startTime) / m_duration * XM_2PI;

    XMFLOAT3 currentPosition;
    currentPosition.x = m_radius * cos(u);
    currentPosition.y = m_radius * sin(u);
    currentPosition.z = 0.0f;

    XMStoreFloat3(
        &currentPosition,
        XMVector3TransformCoord(
            XMLoadFloat3(&currentPosition),
            XMLoadFloat4x4(&m_rotationMatrix)
            )
        );

    return currentPosition;
}

//----------------------------------------------------------------------
            
            
```

BasicLoader.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "BasicReaderWriter.h"

// A simple loader class that provides support for loading shaders, textures,
// and meshes from files on disk. Provides synchronous and asynchronous methods.
ref class BasicLoader
{
internal:
    BasicLoader(
        _In_ ID3D11Device* d3dDevice,
        _In_opt_ IWICImagingFactory2* wicFactory = nullptr
        );

    void LoadTexture(
        _In_ Platform::String^ filename,
        _Out_opt_ ID3D11Texture2D** texture,
        _Out_opt_ ID3D11ShaderResourceView** textureView
        );

    concurrency::task<void> LoadTextureAsync(
        _In_ Platform::String^ filename,
        _Out_opt_ ID3D11Texture2D** texture,
        _Out_opt_ ID3D11ShaderResourceView** textureView
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11PixelShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11PixelShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11ComputeShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11ComputeShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11GeometryShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11GeometryShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
        _In_ uint32 numEntries,
        _In_reads_opt_(numStrides) const uint32* bufferStrides,
        _In_ uint32 numStrides,
        _In_ uint32 rasterizedStream,
        _Out_ ID3D11GeometryShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
        _In_ uint32 numEntries,
        _In_reads_opt_(numStrides) const uint32* bufferStrides,
        _In_ uint32 numStrides,
        _In_ uint32 rasterizedStream,
        _Out_ ID3D11GeometryShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11HullShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11HullShader** shader
        );

    void LoadShader(
        _In_ Platform::String^ filename,
        _Out_ ID3D11DomainShader** shader
        );

    concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11DomainShader** shader
        );

    void LoadMesh(
        _In_ Platform::String^ filename,
        _Out_ ID3D11Buffer** vertexBuffer,
        _Out_ ID3D11Buffer** indexBuffer,
        _Out_opt_ uint32* vertexCount,
        _Out_opt_ uint32* indexCount
        );

    concurrency::task<void> LoadMeshAsync(
        _In_ Platform::String^ filename,
        _Out_ ID3D11Buffer** vertexBuffer,
        _Out_ ID3D11Buffer** indexBuffer,
        _Out_opt_ uint32* vertexCount,
        _Out_opt_ uint32* indexCount
        );

private:
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<IWICImagingFactory2> m_wicFactory;
    BasicReaderWriter^ m_basicReaderWriter;

    template <class DeviceChildType>
    inline void SetDebugName(
        _In_ DeviceChildType* object,
        _In_ Platform::String^ name
        );

    Platform::String^ GetExtension(
        _In_ Platform::String^ filename
        );

    void CreateTexture(
        _In_ bool decodeAsDDS,
        _In_reads_bytes_(dataSize) byte* data,
        _In_ uint32 dataSize,
        _Out_opt_ ID3D11Texture2D** texture,
        _Out_opt_ ID3D11ShaderResourceView** textureView,
        _In_opt_ Platform::String^ debugName
        );

    void CreateInputLayout(
        _In_reads_bytes_(bytecodeSize) byte* bytecode,
        _In_ uint32 bytecodeSize,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11InputLayout** layout
        );

    void CreateMesh(
        _In_ byte* meshData,
        _Out_ ID3D11Buffer** vertexBuffer,
        _Out_ ID3D11Buffer** indexBuffer,
        _Out_opt_ uint32* vertexCount,
        _Out_opt_ uint32* indexCount,
        _In_opt_ Platform::String^ debugName
        );
};
```

BasicLoader.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "BasicLoader.h"
#include "BasicShapes.h"
#include "DDSTextureLoader.h"
#include "DirectXSample.h"
#include <memory>

using namespace Microsoft::WRL;
using namespace Windows::Storage;
using namespace Windows::Storage::Streams;
using namespace Windows::Foundation;
using namespace Windows::ApplicationModel;
using namespace std;
using namespace concurrency;

BasicLoader::BasicLoader(
    _In_ ID3D11Device* d3dDevice,
    _In_opt_ IWICImagingFactory2* wicFactory
    ) :
    m_d3dDevice(d3dDevice),
    m_wicFactory(wicFactory)
{
    // Create a new BasicReaderWriter to do raw file I/O.
    m_basicReaderWriter = ref new BasicReaderWriter();
}

template <class DeviceChildType>
inline void BasicLoader::SetDebugName(
    _In_ DeviceChildType* object,
    _In_ Platform::String^ name
    )
{
#if defined(_DEBUG)
    // Only assign debug names in debug builds.

    char nameString[1024];
    int nameStringLength = WideCharToMultiByte(
        CP_ACP,
        0,
        name->Data(),
        -1,
        nameString,
        1024,
        nullptr,
        nullptr
        );

    if (nameStringLength == 0)
    {
        char defaultNameString[] = "BasicLoaderObject";
        DX::ThrowIfFailed(
            object->SetPrivateData(
                WKPDID_D3DDebugObjectName,
                sizeof(defaultNameString) - 1,
                defaultNameString
                )
            );
    }
    else
    {
        DX::ThrowIfFailed(
            object->SetPrivateData(
                WKPDID_D3DDebugObjectName,
                nameStringLength - 1,
                nameString
                )
            );
    }
#endif
}

Platform::String^ BasicLoader::GetExtension(
    _In_ Platform::String^ filename
    )
{
    int lastDotIndex = -1;
    for (int i = filename->Length() - 1; i >= 0 && lastDotIndex == -1; i--)
    {
        if (*(filename->Data() + i) == '.')
        {
            lastDotIndex = i;
        }
    }
    if (lastDotIndex != -1)
    {
        std::unique_ptr<wchar_t[]> extension(new wchar_t[filename->Length() - lastDotIndex]);
        for (unsigned int i = 0; i < filename->Length() - lastDotIndex; i++)
        {
            extension[i] = tolower(*(filename->Data() + lastDotIndex + 1 + i));
        }
        return ref new Platform::String(extension.get());
    }
    return "";
}

void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        DX::ThrowIfFailed(
            resource.As(&texture2D)
            );
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            DX::ThrowIfFailed(
                CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory)
                    )
                );
        }

        ComPtr<IWICStream> stream;
        DX::ThrowIfFailed(
            m_wicFactory->CreateStream(&stream)
            );

        DX::ThrowIfFailed(
            stream->InitializeFromMemory(
                data,
                dataSize
                )
            );

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        DX::ThrowIfFailed(
            m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder
                )
            );

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        DX::ThrowIfFailed(
            bitmapDecoder->GetFrame(0, &bitmapFrame)
            );

        ComPtr<IWICFormatConverter> formatConverter;
        DX::ThrowIfFailed(
            m_wicFactory->CreateFormatConverter(&formatConverter)
            );

        DX::ThrowIfFailed(
            formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom
                )
            );

        uint32 width;
        uint32 height;
        DX::ThrowIfFailed(
            bitmapFrame->GetSize(&width, &height)
            );

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        DX::ThrowIfFailed(
            formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get()
                )
            );

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D
                )
            );

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            DX::ThrowIfFailed(
                m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView
                    )
                );
        }
    }

    SetDebugName(texture2D.Get(), debugName);

    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}

void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        DX::ThrowIfFailed(
            m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout
                )
            );
    }
    else
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout
                )
            );
    }
}

void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            )
        );

    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            )
        );

    SetDebugName(*vertexBuffer, Platform::String::Concat(debugName, "_VertexBuffer"));
    SetDebugName(*indexBuffer, Platform::String::Concat(debugName, "_IndexBuffer"));

    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}

void BasicLoader::LoadTexture(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    Platform::Array<byte>^ textureData = m_basicReaderWriter->ReadData(filename);

    CreateTexture(
        GetExtension(filename) == "dds",
        textureData->Data,
        textureData->Length,
        texture,
        textureView,
        filename
        );
}

task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateVertexShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);

    if (layout != nullptr)
    {
        CreateInputLayout(
            bytecode->Data,
            bytecode->Length,
            layoutDesc,
            layoutDescNumElements,
            layout
            );

        SetDebugName(*layout, filename);
    }
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout
                );

            SetDebugName(*layout, filename);
        }
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreatePixelShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11ComputeShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateComputeShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11ComputeShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateComputeShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11GeometryShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateGeometryShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11GeometryShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateGeometryShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
    _In_ uint32 numEntries,
    _In_reads_opt_(numStrides) const uint32* bufferStrides,
    _In_ uint32 numStrides,
    _In_ uint32 rasterizedStream,
    _Out_ ID3D11GeometryShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateGeometryShaderWithStreamOutput(
            bytecode->Data,
            bytecode->Length,
            streamOutDeclaration,
            numEntries,
            bufferStrides,
            numStrides,
            rasterizedStream,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(numEntries) const D3D11_SO_DECLARATION_ENTRY* streamOutDeclaration,
    _In_ uint32 numEntries,
    _In_reads_opt_(numStrides) const uint32* bufferStrides,
    _In_ uint32 numStrides,
    _In_ uint32 rasterizedStream,
    _Out_ ID3D11GeometryShader** shader
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the streamOutDeclaration array as well as the SemanticName
    // strings, both of which are pointers to data whose lifetimes may be shorter
    // than that of this method's task.
    shared_ptr<vector<D3D11_SO_DECLARATION_ENTRY>> streamOutDeclarationCopy;
    shared_ptr<vector<string>> streamOutDeclarationSemanticNamesCopy;
    if (streamOutDeclaration != nullptr)
    {
        streamOutDeclarationCopy.reset(
            new vector<D3D11_SO_DECLARATION_ENTRY>(
                streamOutDeclaration,
                streamOutDeclaration + numEntries
                )
            );

        streamOutDeclarationSemanticNamesCopy.reset(
            new vector<string>(numEntries)
            );

        for (uint32 i = 0; i < numEntries; i++)
        {
            streamOutDeclarationSemanticNamesCopy->at(i).assign(streamOutDeclaration[i].SemanticName);
        }
    }
    
    // Create a copy of the bufferStrides array, which is a pointer to data
    // whose lifetime may be shorter than that of this method's task.
    shared_ptr<vector<uint32>> bufferStridesCopy;
    if (bufferStrides != nullptr)
    {
        bufferStridesCopy.reset(
            new vector<uint32>(
                bufferStrides,
                bufferStrides + numStrides
                )
            );
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        if (streamOutDeclaration != nullptr)
        {
            // Reassign the SemanticName elements of the streamOutDeclaration array copy to
            // point to the corresponding copied strings. Performing the assignment inside the
            // lambda body ensures that the lambda will take a reference to the shared_ptr
            // that holds the data.  This will guarantee that the data is still valid when
            // CreateGeometryShaderWithStreamOutput is called.
            for (uint32 i = 0; i < numEntries; i++)
            {
                streamOutDeclarationCopy->at(i).SemanticName = streamOutDeclarationSemanticNamesCopy->at(i).c_str();
            }
        }

        DX::ThrowIfFailed(
            m_d3dDevice->CreateGeometryShaderWithStreamOutput(
                bytecode->Data,
                bytecode->Length,
                streamOutDeclaration == nullptr ? nullptr : streamOutDeclarationCopy->data(),
                numEntries,
                bufferStrides == nullptr ? nullptr : bufferStridesCopy->data(),
                numStrides,
                rasterizedStream,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11HullShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateHullShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11HullShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateHullShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadShader(
    _In_ Platform::String^ filename,
    _Out_ ID3D11DomainShader** shader
    )
{
    Platform::Array<byte>^ bytecode = m_basicReaderWriter->ReadData(filename);

    DX::ThrowIfFailed(
        m_d3dDevice->CreateDomainShader(
            bytecode->Data,
            bytecode->Length,
            nullptr,
            shader
            )
        );

    SetDebugName(*shader, filename);
}

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11DomainShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        DX::ThrowIfFailed(
            m_d3dDevice->CreateDomainShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader
                )
            );

        SetDebugName(*shader, filename);
    });
}

void BasicLoader::LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    Platform::Array<byte>^ meshData = m_basicReaderWriter->ReadData(filename);

    CreateMesh(
        meshData->Data,
        vertexBuffer,
        indexBuffer,
        vertexCount,
        indexCount,
        filename
        );
}

task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

MeshObject.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// MeshObject:
// This class is the generic (abstract) representation of D3D11 Indexed triangle
// list.  Each of the derived classes is just the constructor for the specific
// geometry primitive.  This abstract class does not place any requirements on
// the format of the geometry directly.
// The primary method of the MeshObject is Render.  The default implementation
// just sets the IndexBuffer, VertexBuffer, and topology to a TriangleList and
// makes a  DrawIndexed call on the context.  It assumes all other state has
// already been set on the context.

ref class MeshObject abstract
{
internal:
    MeshObject();

    virtual void Render(_In_ ID3D11DeviceContext *context);

protected private:
    Microsoft::WRL::ComPtr<ID3D11Buffer>  m_vertexBuffer;
    Microsoft::WRL::ComPtr<ID3D11Buffer>  m_indexBuffer;
    int                                   m_vertexCount;
    int                                   m_indexCount;
};
            
            
```

MeshObject.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "MeshObject.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

MeshObject::MeshObject():
    m_vertexCount(0),
    m_indexCount(0)
{
}

//--------------------------------------------------------------------------------

void MeshObject::Render(_In_ ID3D11DeviceContext *context)
{
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->DrawIndexed(m_indexCount, 0, 0);
}

//--------------------------------------------------------------------------------
            
            
```

SphereMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// SphereMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent a canonical sphere that is
// positioned at the origin with a radius of 1.0.

#include "MeshObject.h"

ref class SphereMesh: public MeshObject
{
internal:
    SphereMesh(_In_ ID3D11Device *device, uint32 segments);
};
            
            
```

SphereMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "SphereMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

SphereMesh::SphereMesh(_In_ ID3D11Device *device, uint32 segments)
{
    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    uint32 slices = segments / 2;
    uint32 numVertices = (slices + 1) * (segments + 1) + 1;
    uint32 numIndices = slices * segments * 3 * 2;

    std::vector<PNTVertex> point(numVertices);
    std::vector<uint16> index(numIndices);

    // To make the texture look right on the top and bottom of the sphere,
    // each slice will have 'segments + 1' vertices.  The top and bottom
    // vertices will all be coincident, but have different U texture cooordinates.
    uint32 p = 0;
    for (uint32 a = 0; a <= slices; a++)
    {
        float angle1 = static_cast<float>(a) / static_cast<float>(slices) * XM_PI;
        float z = cos(angle1);
        float r = sin(angle1);
        for (uint32 b = 0; b <= segments; b++)
        {
            float angle2 = static_cast<float>(b) / static_cast<float>(segments) * XM_2PI;
            point[p].position = XMFLOAT3(r * cos(angle2), r * sin(angle2), z);
            point[p].normal = point[p].position;
            point[p].textureCoordinate = XMFLOAT2((1.0f-z) / 2.0f, static_cast<float>(b) / static_cast<float>(segments));
            p++;
        }
    }
    m_vertexCount = p;

    p = 0;
    for (uint16 a = 0; a < slices; a++)
    {
        uint16 p1 = a * (segments + 1);
        uint16 p2 = (a + 1) * (segments + 1);

        // Generate two triangles for each segment around the slice.
        for (uint16 b = 0; b < segments; b++)
        {
            if (a < (slices - 1))
            {
                // For all but the bottom slice, add the triangle with one
                // vertex in the a slice and two vertices in the a + 1 slice.
                // Skip it for the bottom slice since the triangle would be
                // degenerate as all the vertices in the bottom slice are coincident.
                index[p] = b + p1;
                index[p+1] = b + p2;
                index[p+2] = b + p2 + 1;
                p = p + 3;
            }
            if (a > 0)
            {
                // For all but the top slice, add the triangle with two
                // vertices in the a slice and one vertex in the a + 1 slice.
                // Skip it for the top slice since the triangle would be
                // degenerate as all the vertices in the top slice are coincident.
                index[p] = b + p1;
                index[p+1] = b + p2 + 1;
                index[p+2] = b + p1 + 1;
                p = p + 3;
            }
        }
    }
    m_indexCount = p;

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = point.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(uint16) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = index.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}
            
            
```

CylinderMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// CylinderMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent a canonical cylinder (capped at
// both ends) that is positioned at the origin with a radius of 1.0,
// a height of 1.0 and with its axis in the +Z direction.

#include "MeshObject.h"

ref class CylinderMesh: public MeshObject
{
internal:
    CylinderMesh(_In_ ID3D11Device *device, uint32 segments);
};
            
            
```

CylinderMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "CylinderMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

CylinderMesh::CylinderMesh(_In_ ID3D11Device *device, uint32 segments)
{
    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    uint32 numVertices = 6 * (segments + 1) + 1;
    uint32 numIndices = 3 * segments * 3 * 2;

    std::vector<PNTVertex> point(numVertices);
    std::vector<uint16> index(numIndices);

    uint32 p = 0;
    // Top center point (multiple points for texture coordinates).
    for (uint32 a = 0; a <= segments; a++)
    {
        point[p].position = XMFLOAT3(0.0f, 0.0f, 1.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, 1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 0.0f);
        p++;
    }
    // Top edge of cylinder: Normals point up for lighting of top surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 1.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, 1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 0.0f);
        p++;
    }
    // Top edge of cylinder: Normals point out for lighting of the side surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 1.0f);
        point[p].normal = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 0.0f);
        p++;
    }
    // Bottom edge of cylinder: Normals point out for lighting of the side surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].normal = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 1.0f);
        p++;
    }
    // Bottom edge of cylinder: Normals point down for lighting of the bottom surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        float angle = static_cast<float>(a) / static_cast<float>(segments) * XM_2PI;
        point[p].position = XMFLOAT3(cos(angle), sin(angle), 0.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, -1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 1.0f);
        p++;
    }
    // Bottom center of cylinder: Normals point down for lighting on the bottom surface.
    for (uint32 a = 0; a <= segments; a++)
    {
        point[p].position = XMFLOAT3(0.0f, 0.0f, 0.0f);
        point[p].normal = XMFLOAT3(0.0f, 0.0f, -1.0f);
        point[p].textureCoordinate = XMFLOAT2(static_cast<float>(a) / static_cast<float>(segments), 1.0f);
        p++;
    }
    m_vertexCount = p;

    p = 0;
    for (uint16 a = 0; a < 6; a += 2)
    {
        uint16 p1 = a*(segments + 1);
        uint16 p2 = (a+1)*(segments + 1);
        for (uint16 b = 0; b < segments; b++)
        {
            if (a < 4)
            {
                index[p] = b + p1;
                index[p+1] = b + p2;
                index[p+2] = b + p2 + 1;
                p = p + 3;
            }
            if (a > 0)
            {
                index[p] = b + p1;
                index[p+1] = b + p2 + 1;
                index[p+2] = b + p1 + 1;
                p = p + 3;
            }
        }
    }
    m_indexCount = p;

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = point.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(uint16) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = index.data();
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}
            
            
```

FaceMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// FaceMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent a canonical face defined as a
// rectangle at the origin extending 1 unit in the +X and
// 1 unit in the +Y direction.
// The face is defined to be two sided, so it is visible from either
// side.

#include "MeshObject.h"

ref class FaceMesh: public MeshObject
{
internal:
    FaceMesh(_In_ ID3D11Device *device);
};
            
            
```

FaceMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "FaceMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

FaceMesh::FaceMesh(_In_ ID3D11Device *device)
{
    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    PNTVertex target_vertices[] =
    {
        {XMFLOAT3(0.0f, 0.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(1.0f, 1.0f)},
        {XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 1.0f)},
        {XMFLOAT3(1.0f, 1.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(1.0f, 0.0f)}
    };
    WORD target_indices[] =
    {
        0, 1, 2,
        0, 2, 3,
        0, 2, 1,
        0, 3, 2
    };

    m_vertexCount = 4;
    m_indexCount = 12;

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = target_vertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = target_indices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}
            
            
```

WorldMesh.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MeshObject.h"

// WorldCeilingMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent the ceiling of the bounding cube
// of the world.
// The vertices are defined by a position, a normal and a single
// 2D texture coordinate.

ref class WorldCeilingMesh: public MeshObject
{
internal:
    WorldCeilingMesh(_In_ ID3D11Device *device);
};

// WorldFloorMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent the floor of the bounding cube
// of the world.

ref class WorldFloorMesh: public MeshObject
{
internal:
    WorldFloorMesh(_In_ ID3D11Device *device);
};

// WorldWallsMesh:
// This class derives from MeshObject and creates a ID3D11Buffer of
// vertices and indices to represent the walls of the bounding cube
// of the world.

ref class WorldWallsMesh: public MeshObject
{
internal:
    WorldWallsMesh(_In_ ID3D11Device *device);
};
            
            
```

WorldMesh.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "WorldMesh.h"
#include "DirectXSample.h"
#include "ConstantBuffers.h"

using namespace Microsoft::WRL;
using namespace DirectX;

WorldCeilingMesh::WorldCeilingMesh(_In_ ID3D11Device *device)
{
    PNTVertex cellVertices[] =
    {
        // CEILING
        {XMFLOAT3(-4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2(-0.15f, 0.0f)},
        {XMFLOAT3( 4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2( 1.25f, 0.0f)},
        {XMFLOAT3(-4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2(-0.15f, 2.1f)},
        {XMFLOAT3( 4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, -1.0f, 0.0f), XMFLOAT2( 1.25f, 2.1f)},
    };

    WORD cellIndices[] = {
        0, 1, 2,
        1, 3, 2,
    };

    m_vertexCount = 4;
    m_indexCount = 6;

    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellVertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellIndices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}

WorldFloorMesh::WorldFloorMesh(_In_ ID3D11Device *device)
{
    PNTVertex cellVertices[] =
    {
        // FLOOR
        {XMFLOAT3(-4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3( 4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(1.0f, 0.0f)},
        {XMFLOAT3(-4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3( 4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 1.0f, 0.0f), XMFLOAT2(1.0f, 1.5f)},
    };

    WORD cellIndices[] = {
        0, 1, 2,
        1, 3, 2,
    };

    m_vertexCount = 4;
    m_indexCount = 6;

    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellVertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellIndices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}

WorldWallsMesh::WorldWallsMesh(_In_ ID3D11Device *device)
{
    PNTVertex cellVertices[] =
    {
        // WALL
        {XMFLOAT3(-4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3( 4.0f,  3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(2.0f, 0.0f)},
        {XMFLOAT3(-4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3( 4.0f, -3.0f,  6.0f), XMFLOAT3(0.0f, 0.0f, -1.0f), XMFLOAT2(2.0f, 1.5f)},
        // WALL
        {XMFLOAT3(4.0f,  3.0f,  6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(4.0f,  3.0f, -6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 0.0f)},
        {XMFLOAT3(4.0f, -3.0f,  6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3(4.0f, -3.0f, -6.0f), XMFLOAT3(-1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 1.5f)},
        // WALL
        {XMFLOAT3( 4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(-4.0f,  3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(2.0f, 0.0f)},
        {XMFLOAT3( 4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3(-4.0f, -3.0f, -6.0f), XMFLOAT3(0.0f, 0.0f, 1.0f), XMFLOAT2(2.0f, 1.5f)},
        // WALL
        {XMFLOAT3(-4.0f,  3.0f, -6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 0.0f)},
        {XMFLOAT3(-4.0f,  3.0f,  6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 0.0f)},
        {XMFLOAT3(-4.0f, -3.0f, -6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(0.0f, 1.5f)},
        {XMFLOAT3(-4.0f, -3.0f,  6.0f), XMFLOAT3(1.0f, 0.0f, 0.0f), XMFLOAT2(3.0f, 1.5f)},
    };

    WORD cellIndices[] = {
         0,  1,  2,
         1,  3,  2,
         4,  5,  6,
         5,  7,  6,
         8,  9, 10,
         9, 11, 10,
        12, 13, 14,
        13, 15, 14,
    };

    m_vertexCount = 16;
    m_indexCount = 24;

    D3D11_BUFFER_DESC bd = {0};
    D3D11_SUBRESOURCE_DATA initData = {0};

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(PNTVertex) * m_vertexCount;
    bd.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellVertices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_vertexBuffer)
        );

    bd.Usage = D3D11_USAGE_DEFAULT;
    bd.ByteWidth = sizeof(WORD) * m_indexCount;
    bd.BindFlags = D3D11_BIND_INDEX_BUFFER;
    bd.CPUAccessFlags = 0;
    initData.pSysMem = cellIndices;
    DX::ThrowIfFailed(
        device->CreateBuffer(&bd, &initData, &m_indexBuffer)
        );
}

            
            
```

Level.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level:
// This is an abstract class from which all of the levels of the game are derived.
// Each level potentially overrides up to four methods:
//     Initialize - (required) takes a list of objects and enables the objects that
//         are active for the level as well as setting their positions and
//         any animations associated with the objects.
//     Update - this method is called once per time step and is expected to
//         determine if the level has been completed.  The Level class provides
//         a 'standard' Update method which checks each object that is a target
//         and disables any active targets that have been hit.  It returns true
//         once there are no active targets remaining.
//     SaveState - method to save any Level specific state.  Default is defined as
//         not saving any state.
//     LoadState - method to restore any Level specific state.  Default is defined
//         as not restoring any state.

#include "GameObject.h"
#include "PersistentState.h"

ref class Level abstract
{
internal:
    virtual void Initialize(
        std::vector<GameObject^> objects
        ) = 0;

    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        );

    virtual void SaveState(PersistentState^ state);
    virtual void LoadState(PersistentState^ state);

    Platform::String^ Objective();
    float TimeLimit();

protected private:
    Platform::String^ m_objective;
    float             m_timeLimit;
};
            
            
```

Level.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level.h"

//----------------------------------------------------------------------

bool Level::Update(
    float /* time */,
    float /* elapsedTime */,
    float /* timeRemaining*/,
    std::vector<GameObject^> objects
    )
{
    int left = 0;

    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && (*object)->Target())
        {
            if ((*object)->Hit())
            {
                (*object)->Active(false);
            }
            else
            {
                left++;
            }
        }
    }
    return (left == 0);
}

//----------------------------------------------------------------------

void Level::SaveState(PersistentState^ /* state */)
{
}

//----------------------------------------------------------------------

void Level::LoadState(PersistentState^ /* state */)
{
}

//----------------------------------------------------------------------

Platform::String^ Level::Objective()
{
    return m_objective;
}

//----------------------------------------------------------------------

float Level::TimeLimit()
{
    return m_timeLimit;
}

//----------------------------------------------------------------------            
            
```

Level1.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level1:
// This class defines the first level of the game.  There are nine active targets.
// Each of the targets is stationary and can be hit in any order.

#include "Level.h"

ref class Level1: public Level
{
internal:
    Level1();
    virtual void Initialize(std::vector<GameObject^> objects) override;
};
            
```

Level1.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level1.h"
#include "Face.h"

using namespace DirectX;

//----------------------------------------------------------------------

Level1::Level1()
{
    m_timeLimit = 20.0f;
    m_objective =  "Hit each of the targets before time runs out.\nTouch to aim.  Tap in right box to fire.  Drag in left box to move.";
}

//----------------------------------------------------------------------

void Level1::Initialize(std::vector<GameObject^> objects)
{
    XMFLOAT3 position[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
        XMFLOAT3( 1.5f,  0.0f, -3.0f),
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
        XMFLOAT3( 2.0f,  0.0f,  0.0f),
        XMFLOAT3( 0.0f,  0.0f,  0.0f),
        XMFLOAT3(-2.0f,  0.0f,  0.0f)
    };

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Active(true);
                target->Target(true);
                target->Hit(false);
                target->AnimatePosition(nullptr);
                target->Position(position[targetCount]);
                targetCount++;
            }
            else
            {
                (*object)->Active(false);
            }
        }
        else
        {
            (*object)->Active(false);
        }
    }
}

//----------------------------------------------------------------------
            
            
```

Level2.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level2:
// This class defines the second level of the game.  It derives from the
// first level.  In this level, the targets must be hit in numeric order.

#include "Level1.h"

ref class Level2: public Level1
{
internal:
    Level2();
    virtual void Initialize(std::vector<GameObject^> objects) override;

    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        ) override;

    virtual void SaveState(PersistentState^ state) override;
    virtual void LoadState(PersistentState^ state) override;

private:
    int m_nextId;
};
            
            
```

Level2.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level2.h"
#include "Face.h"

//----------------------------------------------------------------------

Level2::Level2()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the targets in ORDER before time runs out.";
}

//----------------------------------------------------------------------

void Level2::Initialize(std::vector<GameObject^> objects)
{
    Level1::Initialize(objects);

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Target(targetCount == 0 ? true : false);
                targetCount++;
            }
        }
    }
    m_nextId = 1;
}

//----------------------------------------------------------------------

bool Level2::Update(
    float /* time */,
    float /* elapsedTime */,
    float /* timeRemaining */,
    std::vector<GameObject^> objects
    )
{
    int left = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && ((*object)->TargetId() > 0))
        {
            if ((*object)->Hit()  && ((*object)->TargetId() == m_nextId))
            {
                (*object)->Active(false);
                m_nextId++;
            }
            else
            {
                left++;
            }
        }
        if ((*object)->Active() && ((*object)->TargetId() == m_nextId))
        {
            (*object)->Target(true);
        }
    }
    return (left == 0);
}

//----------------------------------------------------------------------

void Level2::SaveState(PersistentState^ state)
{
    state->SaveInt32(":NextTarget", m_nextId);
}

//----------------------------------------------------------------------

void Level2::LoadState(PersistentState^ state)
{
    m_nextId = state->LoadInt32(":NextTarget", 1);
}

//----------------------------------------------------------------------            
            
```

Level3.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level3:
// This class defines the third level of the game.  In this level, each of the
// nine targets is moving along closed paths and can be hit
// in any order.

#include "Level.h"

ref class Level3: public Level
{
internal:
    Level3();
    virtual void Initialize(std::vector<GameObject^> objects) override;
};
            
            
```

Level3.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level3.h"
#include "Face.h"
#include "Animate.h"

using namespace DirectX;

//----------------------------------------------------------------------

Level3::Level3()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the moving targets before time runs out.";
}

//----------------------------------------------------------------------

void Level3::Initialize(std::vector<GameObject^> objects)
{
    XMFLOAT3 position[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
        XMFLOAT3( 1.5f,  0.0f, -5.5f),
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
        XMFLOAT3( 0.0f, -3.6f,  0.0f),
        XMFLOAT3( 0.0f, -3.6f,  0.0f),
        XMFLOAT3( 0.0f, -3.6f,  0.0f)
    };
    XMFLOAT3 LineList1[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
        XMFLOAT3(-0.5f,  1.0f,  1.0f),
        XMFLOAT3(-0.5f, -2.5f,  1.0f),
        XMFLOAT3(-2.5f, -1.0f, -1.5f),
    };
    XMFLOAT3 LineList2[] =
    {
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
        XMFLOAT3(-2.0f,  2.0f, -1.5f),
        XMFLOAT3(-2.0f, -2.5f, -1.5f),
        XMFLOAT3( 1.5f, -2.5f, -1.5f),
        XMFLOAT3( 1.5f, -2.5f, -3.0f),
        XMFLOAT3(-1.0f,  1.0f, -3.0f),
    };
    XMFLOAT3 LineList3[] =
    {
        XMFLOAT3(1.5f,  0.0f, -5.5f),
        XMFLOAT3(1.5f,  1.0f, -5.5f),
        XMFLOAT3(1.5f, -2.5f, -5.5f),
        XMFLOAT3(1.5f,  0.0f, -5.5f),
    };
    XMFLOAT3 LineList4[] =
    {
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
        XMFLOAT3( 1.0f, -1.0f, -5.5f),
        XMFLOAT3( 1.0f,  1.0f, -5.5f),
        XMFLOAT3(-2.5f,  1.0f, -5.5f),
        XMFLOAT3(-2.5f, -1.0f, -5.5f),
    };
    XMFLOAT3 LineList5[] =
    {
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
        XMFLOAT3( 2.0f, -2.0f, -5.0f),
        XMFLOAT3( 2.0f,  1.0f, -5.0f),
        XMFLOAT3(-2.5f,  1.0f, -5.0f),
        XMFLOAT3(-2.5f, -2.0f, -5.0f),
        XMFLOAT3( 0.5f, -2.0f, -5.0f),
    };
    XMFLOAT3 LineList6[] =
    {
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
        XMFLOAT3(-2.5f, -2.0f, -5.5f),
        XMFLOAT3(-2.5f,  1.0f, -5.5f),
        XMFLOAT3( 1.5f,  1.0f, -5.5f),
        XMFLOAT3( 1.5f, -2.0f, -5.5f),
    };

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Active(true);
                target->Target(true);
                target->Hit(false);
                target->Position(position[targetCount]);
                switch (targetCount)
                {
                case 0:
                    target->AnimatePosition(ref new AnimateLineListPosition(4, LineList1, 10.0f, true));
                    break;
                case 1:
                    target->AnimatePosition(ref new AnimateLineListPosition(6, LineList2, 15.0f, true));
                    break;
                case 2:
                    target->AnimatePosition(ref new AnimateLineListPosition(4, LineList3, 15.0f, true));
                    break;
                case 3:
                    target->AnimatePosition(ref new AnimateLineListPosition(5, LineList4, 15.0f, true));
                    break;
                case 4:
                    target->AnimatePosition(ref new AnimateLineListPosition(6, LineList5, 15.0f, true));
                    break;
                case 5:
                    target->AnimatePosition(ref new AnimateLineListPosition(5, LineList6, 15.0f, true));
                    break;
                case 6:
                    target->AnimatePosition(
                        ref new AnimateCirclePosition(
                            XMFLOAT3(0.0f, -2.5f, 0.0f),
                            XMFLOAT3(0.0f, -3.6f, 0.0f),
                            XMFLOAT3(0.0f,  0.0f, 1.0f),
                            9.0f,
                            true,
                            true
                            )
                        );
                    break;
                case 7:
                    target->AnimatePosition(
                        ref new AnimateCirclePosition(
                            XMFLOAT3(0.0f, -2.5f, 0.0f),
                            XMFLOAT3(0.0f, -3.6f, 0.0f),
                            XMFLOAT3(0.0f,  0.0f, 1.0f),
                            9.0f,
                            true,
                            true
                            )
                        );
                    target->AnimatePosition()->Start(3.0f);
                    break;
                case 8:
                    target->AnimatePosition(
                        ref new AnimateCirclePosition(
                            XMFLOAT3(0.0f, -2.5f, 0.0f),
                            XMFLOAT3(0.0f, -3.6f, 0.0f),
                            XMFLOAT3(0.0f,  0.0f, 1.0f),
                            9.0f,
                            true,
                            true
                            )
                        );
                    target->AnimatePosition()->Start(6.0f);
                    break;
                }
                targetCount++;
            }
            else
            {
                target->Active(false);
            }
        }
        else
        {
            (*object)->Active(false);
        }
    }
}

//----------------------------------------------------------------------
            
            
```

Level4.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level4:
// This class defines the fourth level of the game.  It derives from the
// third level.  The targets must be hit in numeric order.

#include "Level3.h"

ref class Level4: public Level3
{
internal:
    Level4();
    virtual void Initialize(std::vector<GameObject^> objects) override;

    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        ) override;

    virtual void SaveState(PersistentState^ state) override;
    virtual void LoadState(PersistentState^ state) override;

private:
    int m_nextId;
};
            
            
```

Level4.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level4.h"
#include "Face.h"

//----------------------------------------------------------------------

Level4::Level4()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the moving targets in ORDER before time runs out.";
}

//----------------------------------------------------------------------

void Level4::Initialize(std::vector<GameObject^> objects)
{
    Level3::Initialize(objects);

    int targetCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Face^ target = dynamic_cast<Face^>(*object))
        {
            if (targetCount < 9)
            {
                target->Target(targetCount == 0 ? true : false);
                targetCount++;
            }
        }
    }
    m_nextId = 1;
}

//----------------------------------------------------------------------

bool Level4::Update(
    float /* time */,
    float /* elapsedTime */,
    float /* timeRemaining */,
    std::vector<GameObject^> objects
    )
{
    int left = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && ((*object)->TargetId() > 0))
        {
            if ((*object)->Hit() && ((*object)->TargetId() == m_nextId))
            {
                (*object)->Active(false);
                m_nextId++;
            }
            else
            {
                left++;
            }
        }
        if ((*object)->Active() && ((*object)->TargetId() == m_nextId))
        {
            (*object)->Target(true);
        }
    }
    return (left == 0);
}

//----------------------------------------------------------------------

void Level4::SaveState(PersistentState^ state)
{
    state->SaveInt32(":NextTarget", m_nextId);
}

//----------------------------------------------------------------------

void Level4::LoadState(PersistentState^ state)
{
    m_nextId = state->LoadInt32(":NextTarget", 1);
}

//----------------------------------------------------------------------
            
            
```

Level5.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level5:
// This class defines the fifth level of the game.  It derives from the
// third level.  This level introduces obstacles that move into place
// during game play.  The targets may be hit in any order.

#include "Level3.h"

ref class Level5: public Level3
{
internal:
    Level5();
    virtual void Initialize(std::vector<GameObject^> objects) override;
};
            
            
```

Level5.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level5.h"
#include "Cylinder.h"
#include "Animate.h"

using namespace DirectX;

//----------------------------------------------------------------------

Level5::Level5()
{
    m_timeLimit = 30.0f;
    m_objective = "Hit each of the moving targets while avoiding the obstacles before time runs out.";
}

//----------------------------------------------------------------------

void Level5::Initialize(std::vector<GameObject^> objects)
{
    Level3::Initialize(objects);

    XMFLOAT3 obstacleStartPosition[] =
    {
        XMFLOAT3(-4.5f, -3.0f, 0.0f),
        XMFLOAT3(4.5f, -3.0f, 0.0f),
        XMFLOAT3(0.0f, 3.01f, -2.0f),
        XMFLOAT3(-1.5f, -3.0f, -6.5f),
        XMFLOAT3(1.5f, -3.0f, -6.5f)
    };
    XMFLOAT3 obstacleEndPosition[] =
    {
        XMFLOAT3(-2.0f, -3.0f, 0.0f),
        XMFLOAT3(2.0f, -3.0f, 0.0f),
        XMFLOAT3(0.0f, -3.0f, -2.0f),
        XMFLOAT3(-1.5f, -3.0f, -4.0f),
        XMFLOAT3(1.5f, -3.0f, -4.0f)
    };
    float obstacleStartTime[] =
    {
        2.0f,
        5.0f,
        8.0f,
        11.0f,
        14.0f
    };

    int obstacleCount = 0;
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if (Cylinder^ obstacle = dynamic_cast<Cylinder^>(*object))
        {
            if (obstacleCount < 5)
            {
                obstacle->Active(true);
                obstacle->Position(obstacleStartPosition[obstacleCount]);
                obstacle->AnimatePosition(
                    ref new AnimateLinePosition(
                        obstacleStartPosition[obstacleCount],
                        obstacleEndPosition[obstacleCount],
                        10.0,
                        false
                        )
                    );
                obstacle->AnimatePosition()->Start(obstacleStartTime[obstacleCount]);
                obstacleCount ++;
            }
        }
    }
}

//----------------------------------------------------------------------            
            
```

Level6.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Level6:
// This class defines the sixth and final level of the game.  It derives from the
// fifth level.  In this level, the targets do not disappear when they are hit.
// The target will stay highlighted for two seconds.  As this is the last level,
// the only criteria for completion is time expiring.

#include "Level5.h"

ref class Level6: public Level5
{
internal:
    Level6();
    virtual bool Update(
        float time,
        float elapsedTime,
        float timeRemaining,
        std::vector<GameObject^> objects
        ) override;
};
            
            
```

Level6.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Level6.h"

//----------------------------------------------------------------------

Level6::Level6()
{
    m_timeLimit = 20.0f;
    m_objective = "Hit as many moving targets as possible while avoiding the obstacles before time runs out.";
}

//----------------------------------------------------------------------

bool Level6::Update(
    float time,
    float elapsedTime,
    float timeRemaining,
    std::vector<GameObject^> objects
    )
{
    for (auto object = objects.begin(); object != objects.end(); object++)
    {
        if ((*object)->Active() && (*object)->Target())
        {
            if ((*object)->Hit() && ((*object)->HitTime() < (time - 2.0f)))
            {
                (*object)->Hit(false);
            }
        }
    }
    return ((timeRemaining - elapsedTime) <= 0.0f);
}

//----------------------------------------------------------------------            
            
```

TargetTexture.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// TargetTexture:
// This is a helper class to procedurally generate textures for game
// targets.  There are two versions of the textures, one when it is
// hit and the other is when it is not.
// The class creates the necessary resources to draw the texture into
// an off screen resource at initialization time.

ref class TargetTexture
{
internal:
    TargetTexture(
        _In_ ID3D11Device1*      d3dDevice,
        _In_ ID2D1Factory1*      d2dFactory,
        _In_ IDWriteFactory1*    dwriteFactory,
        _In_ ID2D1DeviceContext* d2dContext
        );

    void CreateTextureResourceView(
        _In_ Platform::String^ name,
        _Out_ ID3D11ShaderResourceView** textureResourceView
        );
    void CreateHitTextureResourceView(
        _In_ Platform::String^ name,
        _Out_ ID3D11ShaderResourceView** textureResourceView
        );

protected private:
    Microsoft::WRL::ComPtr<ID3D11Device1>           m_d3dDevice;
    Microsoft::WRL::ComPtr<ID2D1Factory1>           m_d2dFactory;
    Microsoft::WRL::ComPtr<ID2D1DeviceContext>      m_d2dContext;
    Microsoft::WRL::ComPtr<IDWriteFactory1>         m_dwriteFactory;

    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_redBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_blueBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_greenBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_whiteBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_blackBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_yellowBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_clearBrush;

    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry1;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry2;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry3;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry4;
    Microsoft::WRL::ComPtr<ID2D1EllipseGeometry>    m_circleGeometry5;

    Microsoft::WRL::ComPtr<IDWriteTextFormat>       m_textFormat;
};            
            
```

TargetTexture.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "TargetTexture.h"
#include "DirectXSample.h"

using namespace Microsoft::WRL;
using namespace Windows::Graphics::Display;

TargetTexture::TargetTexture(
    _In_ ID3D11Device1* d3dDevice,
    _In_ ID2D1Factory1* d2dFactory,
    _In_ IDWriteFactory1* dwriteFactory,
    _In_ ID2D1DeviceContext* d2dContext
    )
{
    m_d3dDevice = d3dDevice;
    m_d2dFactory = d2dFactory;
    m_dwriteFactory = dwriteFactory;
    m_d2dContext = d2dContext;

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Red, 1.f),
            &m_redBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::CornflowerBlue, 1.0f),
            &m_blueBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Green, 1.f),
            &m_greenBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White, 1.f),
            &m_whiteBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Black, 1.f),
            &m_blackBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Yellow, 1.f),
            &m_yellowBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White, 0.0f),
            &m_clearBrush
            )
        );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                50.0f,
                50.0f
                ),
            &m_circleGeometry1)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                100.0f,
                100.0f
                ),
            &m_circleGeometry2)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                150.0f,
                150.0f
                ),
            &m_circleGeometry3)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                200.0f,
                200.0f
                ),
            &m_circleGeometry4)
            );

    DX::ThrowIfFailed(
        m_d2dFactory->CreateEllipseGeometry(
            D2D1::Ellipse(
                D2D1::Point2F(256.0f, 256.0f),
                250.0f,
                250.0f
                ),
            &m_circleGeometry5)
            );

    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            425,        // fontsize
            L"en-US",   // locale
            &m_textFormat
            )
        );

    // Center the text horizontally.
    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_CENTER)
        );

    // Center the text vertically.
    DX::ThrowIfFailed(
        m_textFormat->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_CENTER)
        );
}
//----------------------------------------------------------------------
void TargetTexture::CreateTextureResourceView(
    _In_ Platform::String^ name,
    _Out_ ID3D11ShaderResourceView** textureResourceView
    )
{
    // Allocate a offscreen D3D surface for D2D to render our 2D content into
    D3D11_TEXTURE2D_DESC texDesc;
    texDesc.ArraySize = 1;
    texDesc.BindFlags = D3D11_BIND_RENDER_TARGET | D3D11_BIND_SHADER_RESOURCE;
    texDesc.CPUAccessFlags = 0;
    texDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    texDesc.Height = 512;
    texDesc.Width = 512;
    texDesc.MipLevels = 1;
    texDesc.MiscFlags = 0;
    texDesc.SampleDesc.Count = 1;
    texDesc.SampleDesc.Quality = 0;
    texDesc.Usage = D3D11_USAGE_DEFAULT;

    ComPtr<ID3D11Texture2D> offscreenTexture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(&texDesc, nullptr, &offscreenTexture)
        );

    // Convert the Direct2D texture into a Shader Resource View
    ComPtr<ID3D11ShaderResourceView> texture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateShaderResourceView(offscreenTexture.Get(), nullptr, &texture)
        );
#if defined(_DEBUG)
    {
        char debugName[100];
        int l = sprintf_s(debugName, sizeof(debugName) / sizeof(debugName[0]), "Simple3DGame Target %ls", name->Data());
        DX::ThrowIfFailed(
            texture->SetPrivateData(WKPDID_D3DDebugObjectName, l, debugName)
            );
    }
#endif

    ComPtr<IDXGISurface> dxgiSurface;
    DX::ThrowIfFailed(
        offscreenTexture.As(&dxgiSurface)
        );

    // Create a D2D render target which can draw into our offscreen D3D
    // surface. Given that we use a constant size for the texture, we
    // fix the DPI at 96.

    D2D1_BITMAP_PROPERTIES1 properties;
    properties.pixelFormat.format = DXGI_FORMAT_B8G8R8A8_UNORM;
    properties.pixelFormat.alphaMode = D2D1_ALPHA_MODE_PREMULTIPLIED;
    properties.dpiX = 96;
    properties.dpiY = 96;
    properties.bitmapOptions = D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW;
    properties.colorContext = nullptr;

    ComPtr<ID2D1Bitmap1> renderTarget;
    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiSurface.Get(),
            &properties,
            &renderTarget
            )
        );

    m_d2dContext->SetTarget(renderTarget.Get());
    float saveDpiX;
    float saveDpiY;

    m_d2dContext->GetDpi(&saveDpiX, &saveDpiY);
    m_d2dContext->SetDpi(96.0f, 96.0f);

    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());

    D2D1_SIZE_F renderTargetSize = renderTarget->GetSize();

    m_d2dContext->Clear(D2D1::ColorF(D2D1::ColorF::White));
    m_d2dContext->FillGeometry(m_circleGeometry5.Get(), m_redBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry4.Get(), m_blueBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry3.Get(), m_greenBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry2.Get(), m_yellowBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry1.Get(), m_blackBrush.Get());
    m_d2dContext->DrawText(
        name->Data(),
        name->Length(),
        m_textFormat.Get(),
        D2D1::RectF(0, 0, renderTargetSize.width, renderTargetSize.height),
        m_whiteBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->SetTarget(nullptr);
    m_d2dContext->SetDpi(saveDpiX, saveDpiY);

    *textureResourceView = texture.Detach();
}
//----------------------------------------------------------------------
void TargetTexture::CreateHitTextureResourceView(
    _In_ Platform::String^ name,
    _Out_ ID3D11ShaderResourceView** textureResourceView
    )
{
    // Allocate a offscreen D3D surface for D2D to render our 2D content into
    D3D11_TEXTURE2D_DESC texDesc;
    texDesc.ArraySize = 1;
    texDesc.BindFlags = D3D11_BIND_RENDER_TARGET | D3D11_BIND_SHADER_RESOURCE;
    texDesc.CPUAccessFlags = 0;
    texDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    texDesc.Height = 512;
    texDesc.Width = 512;
    texDesc.MipLevels = 1;
    texDesc.MiscFlags = 0;
    texDesc.SampleDesc.Count = 1;
    texDesc.SampleDesc.Quality = 0;
    texDesc.Usage = D3D11_USAGE_DEFAULT;

    ComPtr<ID3D11Texture2D> offscreenTexture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(&texDesc, nullptr, &offscreenTexture)
        );

    // Convert the Direct2D texture into a Shader Resource View.
    ComPtr<ID3D11ShaderResourceView> texture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateShaderResourceView(offscreenTexture.Get(), nullptr, &texture)
        );
#if defined(_DEBUG)
    {
        char debugName[100];
        int l = sprintf_s(debugName, sizeof(debugName) / sizeof(debugName[0]), "Simple3DGame HitTarget %ls", name->Data());
        DX::ThrowIfFailed(
            texture->SetPrivateData(WKPDID_D3DDebugObjectName, l, debugName)
            );
    }
#endif

    ComPtr<IDXGISurface> dxgiSurface;
    DX::ThrowIfFailed(
        offscreenTexture.As(&dxgiSurface)
        );

    // Create a D2D render target which can draw into our offscreen D3D
    // surface. Given that we use a constant size for the texture, we
    // fix the DPI at 96.

    D2D1_BITMAP_PROPERTIES1 properties;
    properties.pixelFormat.format = DXGI_FORMAT_B8G8R8A8_UNORM;
    properties.pixelFormat.alphaMode = D2D1_ALPHA_MODE_PREMULTIPLIED;
    properties.dpiX = 96;
    properties.dpiY = 96;
    properties.bitmapOptions = D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW;
    properties.colorContext = nullptr;

    ComPtr<ID2D1Bitmap1> renderTarget;
    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiSurface.Get(),
            &properties,
            &renderTarget
            )
        );

    m_d2dContext->SetTarget(renderTarget.Get());
    float saveDpiX;
    float saveDpiY;

    m_d2dContext->GetDpi(&saveDpiX, &saveDpiY);
    m_d2dContext->SetDpi(96.0f, 96.0f);

    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());

    D2D1_SIZE_F renderTargetSize = renderTarget->GetSize();

    m_d2dContext->Clear(D2D1::ColorF(D2D1::ColorF::Black));
    m_d2dContext->FillGeometry(m_circleGeometry5.Get(), m_yellowBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry4.Get(), m_greenBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry3.Get(), m_blueBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry2.Get(), m_redBrush.Get());
    m_d2dContext->FillGeometry(m_circleGeometry1.Get(), m_whiteBrush.Get());
    m_d2dContext->DrawText(
        name->Data(),
        name->Length(),
        m_textFormat.Get(),
        D2D1::RectF(0, 0, renderTargetSize.width, renderTargetSize.height),
        m_blackBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->SetTarget(nullptr);
    m_d2dContext->SetDpi(saveDpiX, saveDpiY);

    *textureResourceView = texture.Detach();
}
//----------------------------------------------------------------------
            
            
```

Material.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Material:
// This class maintains the properties that represent how an object will
// look when it is rendered.  This includes the color of the object, the
// texture used to render the object, and the vertex and pixel shader that
// should be used for rendering.
// The RenderSetup method sets the appropriate values into the constantBuffer
// and calls the appropriate D3D11 context methods to set up the rendering pipeline
// in the graphics hardware.

#include "ConstantBuffers.h"

ref class Material
{
internal:
    Material(
        DirectX::XMFLOAT4 meshColor,
        DirectX::XMFLOAT4 diffuseColor,
        DirectX::XMFLOAT4 specularColor,
        float specularExponent,
        _In_ ID3D11ShaderResourceView* textureResourceView,
        _In_ ID3D11VertexShader* vertexShader,
        _In_ ID3D11PixelShader* pixelShader
        );

    void RenderSetup(
        _In_ ID3D11DeviceContext* context,
        _Inout_ ConstantBufferChangesEveryPrim* constantBuffer
        );

    void SetTexture(_In_ ID3D11ShaderResourceView* textureResourceView)
    {
        m_textureRV = textureResourceView;
    }

protected private:
    DirectX::XMFLOAT4   m_meshColor;
    DirectX::XMFLOAT4   m_diffuseColor;
    DirectX::XMFLOAT4   m_hitColor;
    DirectX::XMFLOAT4   m_specularColor;
    float               m_specularExponent;

    Microsoft::WRL::ComPtr<ID3D11VertexShader>       m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>        m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView> m_textureRV;
};
            
            
```

Material.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Material.h"
#include "GameConstants.h"

using namespace DirectX;

//--------------------------------------------------------------------------------

Material::Material(
    XMFLOAT4 meshColor,
    XMFLOAT4 diffuseColor,
    XMFLOAT4 specularColor,
    float specularExponent,
    _In_ ID3D11ShaderResourceView* textureResourceView,
    _In_ ID3D11VertexShader* vertexShader,
    _In_ ID3D11PixelShader* pixelShader
    )
{
    m_meshColor = meshColor;
    m_diffuseColor = diffuseColor;
    m_specularColor = specularColor;
    m_specularExponent = specularExponent;

    m_vertexShader = vertexShader;
    m_pixelShader = pixelShader;
    m_textureRV = textureResourceView;
}

//--------------------------------------------------------------------------------

void Material::RenderSetup(
    _In_ ID3D11DeviceContext* context,
    _Inout_ ConstantBufferChangesEveryPrim* constantBuffer
    )
{
    constantBuffer->meshColor = m_meshColor;
    constantBuffer->specularColor = m_specularColor;
    constantBuffer->specularPower = m_specularExponent;
    constantBuffer->diffuseColor = m_diffuseColor;

    context->PSSetShaderResources(0, 1, m_textureRV.GetAddressOf());
    context->VSSetShader(m_vertexShader.Get(), nullptr, 0);
    context->PSSetShader(m_pixelShader.Get(), nullptr, 0);
}            
            
```

> **注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## <a name="related-topics"></a>相關主題


* [使用 DirectX 建立簡單的 UWP 遊戲](tutorial--create-your-first-metro-style-directx-game.md)

 

 




