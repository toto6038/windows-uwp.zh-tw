---
title: 設定遊戲專案
description: 組合遊戲的第一步是在 Microsoft Visual Studio 中設定一個專案，透過這種方式可以將所需的程式碼基礎結構數量減到最少。
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 設定, directx
ms.localizationpriority: medium
ms.openlocfilehash: 252d7ccb8e50e773a19282afaf19bb18d4c5d5a6
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8944582"
---
# <a name="set-up-the-game-project"></a>設定遊戲專案

本主題說明如何使用 Visual Studio 中的範本設定簡單的 UWP DirectX 遊戲。 組合遊戲的第一步是在 Microsoft Visual Studio 中設定一個專案，透過這種方式可以將所需的程式碼基礎結構數量減到最少。 學習使用正確的範本並設定專用於遊戲開發的專案，可以節省很多時間和麻煩。

## <a name="objectives"></a>目標

* 使用範本在 Visual Studio 中設定 Direct3D 遊戲專案。
* 透過檢查 **App** 來源檔案，了解遊戲的主要進入點
* 檢視專案的 **package.appxmanifest** 檔案
* 了解專案中包含哪些遊戲開發工具與支援

## <a name="how-to-set-up-the-game-project"></a>如何設定遊戲專案

如果您是通用 Windows 平台 (UWP) 開發的新手，我們建議使用 Visual Studio 中的範本設定基本程式碼結構。

>[!Note]
>本文是根據遊戲範例的教學課程系列的一部分。 您可以在 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)取得最新的程式碼。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

### <a name="use-directx-template-to-create-a-project"></a>使用 DirectX 範本建立專案

Visual Studio 範本所包含的設定集合和程式碼檔案，是專門針對使用慣用語言和技術的特定應用程式類型所設計。 在 Microsoft Visual Studio2017，您會發現很多範本能夠讓遊戲和圖形的應用程式開發變得非常容易。 如果您不使用範本，就必須自己開發許多基本圖形轉譯和顯示架構，對遊戲開發初學者而言可能有點困難。

用於此教學課程的範本名為 **DirectX 11 App (通用 Windows)**。 

在 Visual Studio 中建立 DirectX 11 遊戲專案的步驟：
1.  選取 **\[檔案...\]**&gt;**\[新增\]**&gt;**\[專案...\]**
2.  在左窗格中，選取 **\[已安裝\]**&gt;**\[範本\]**&gt;**\[Visual C++\]**&gt;**\[Windows 通用\]**
3.  在中央窗格中，選取 **\[DirectX 11 應用程式 (通用 Windows)\]**。
4.  提供遊戲專案名稱，然後按一下 **\[確定\]**。

![顯示如何選取 directx11 範本以建立新的遊戲專案的螢幕擷取畫面](images/simple-dx-game-setup-new-project.png)

這個範本為您提供使用 DirectX 搭配 C++ 的 UWP app 的基本架構。 按一下 F5 來建置並執行它。 查看那個粉藍色螢幕。 此範本會建立多個程式碼檔案，其中包含使用 DirectX 搭配 C++ 的 UWP app 所適用的基本功能。

## <a name="review-the-apps-main-entry-point-by-understanding-iframeworkview"></a>了解 IFrameworkView 來檢視 App 的主要進入點

**App** 類別繼承自 **IFrameworkView** 類別。

### <a name="inspect-apph"></a>檢查 **App.h**。

讓我們快速查看在實作定義檢視提供者的 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469) 介面時在 **App.h** &mdash; 中的 5 個方法：[**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)、[**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)、[**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)、[**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 和 [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)。 這些方法是由遊戲啟動時建立的應用程式單例執行，而且會載入您 app 的所有資源，以及連接適當的事件處理常式。

```cpp
    // Main entry point for our app. Connects the app with the Windows shell and handle application lifecycle events.
    ref class App sealed : public Windows::ApplicationModel::Core::IFrameworkView
    {
    public:
        App();

        // IFrameworkView Methods.
        virtual void Initialize(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
        virtual void SetWindow(Windows::UI::Core::CoreWindow^ window);
        virtual void Load(Platform::String^ entryPoint);
        virtual void Run();
        virtual void Uninitialize();

    protected:
        ...
    };
```

### <a name="inspect-appcpp"></a>檢查 **App.cpp**

以下是 **App.cpp** 來源檔中的 **main** 方法：

```cpp
//The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

在此方法中，它會從檢視提供者 Factory (**App.h** 中定義的 **Direct3DApplicationSource**) 來建立 Direct3D 檢視提供者的執行個體，並將它傳遞給應用程式單例執行 (透過呼叫 [**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469))。 這表示遊戲的起點將位於 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法實作的內文，在這個情況就是 **App::Run**。 

在 **App.cpp** 中捲動以尋找 **App::Run** 方法。 以下是程式碼：

```cpp
//This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

此方法的執行內容：如果遊戲的視窗沒有關閉，它會分派所有事件、更新計時器，然後轉譯和顯示圖形管線的結果。 我們將會在[定義 UWP app 架構](tutorial--building-the-games-uwp-app-framework.md)、[轉譯架構 I：轉譯簡介](tutorial--assembling-the-rendering-pipeline.md) 和 [轉譯架構 II：遊戲轉譯](tutorial-game-rendering.md)中更詳細討論。 現在，您應該對 UWP DirectX 遊戲的基本程式碼結構有大致的了解。

## <a name="review-and-update-the-packageappxmanifest-file"></a>檢視並更新 package.appxmanifest 檔案


範本不只有程式碼檔案。 **Package.appxmanifest** 檔案包含有關您專案的中繼資料，可用來封裝和啟動您的遊戲，以及提交到 Microsoft Store。 它也包含玩家系統的重要資訊，用來存取遊戲執行所需的系統資源。

在 **\[方案總管\]** 中的 **Package.appxmanifest** 檔案上按兩下，啟動 **\[資訊清單設計工具\]**。

![package.appx 資訊清單編輯器的螢幕擷取畫面。](images/simple-dx-game-setup-app-manifest.png)

如需 **package.appxmanifest** 檔案和封裝的詳細資訊，請參閱[資訊清單設計工具](https://msdn.microsoft.com/library/windows/apps/br230259.aspx)。 現在，看看 **\[功能\]** 索引標籤，以及其中提供的選項。

![direct3d App 的預設功能的螢幕擷取畫面。](images/simple-dx-game-setup-capabilities.png)

如果您未選取遊戲所使用的功能 (例如透過 **\[網際網路\]** 存取全球高分板)，就無法存取對應的資源或功能。 建立新遊戲時，記得選取遊戲執行所需的功能！

現在，讓我們看看隨附於 **DirectX 11 App (通用 Windows)** 範本的其餘檔案。

## <a name="review-the-included-libraries-and-headers"></a>檢視包含的程式庫和標頭

有幾個檔案我們尚未討論。 這些檔案提供 Direct3D 遊戲開發案例中常用的其他工具和支援。 如需基本 DirectX 遊戲專案隨附之檔案的完整清單，請參閱 [DirectX 遊戲專案範本](user-interface.md#template-structure)。

| 範本來源檔         | 檔案資料夾            | 描述 |
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DeviceResources.h/.cpp       | 通用                 | 定義控制所有 DirectX [裝置資源](tutorial--assembling-the-rendering-pipeline.md#resource) 的類別物件。 它也會包括您的應用程式介面，其擁有裝置遺失或建立時要通知的 DeviceResources。                                                |
| DirectXHelper.h              | 通用                 | 實作方法包括**DX::ThrowIfFailed**、**ReadDataAsync** 和 **ConvertDipsToPixels。 **DX::ThrowIfFailed** 將 DirectX Win32 API 傳回的錯誤 HRESULT 值轉換成 Windows 執行階段例外狀況。 使用這個方法放置偵錯 DirectX 錯誤的中斷點。 如需詳細資訊，請參閱 [ThrowIfFailed](https://github.com/Microsoft/DirectXTK/wiki/ThrowIfFailed)。 **ReadDataAsync** 會非同步讀取二進位檔案。 **ConvertDipsToPixels** 會將裝置獨立像素 (DIP) 中的長度轉換為實體像素的長度。 |
| StepTimer.h                  | 通用                 | 定義高解析度的計時器，對於遊戲或互動式轉譯應用程式非常實用。   |
| Sample3DSceneRenderer.h/.cpp | 內容                | 定義起始基本轉譯管線的類別物件。 它建立基本轉譯器實作，將 Direct3D 交換鏈結與圖形卡連接至您的使用 DirectX 的 UWP。   |
| SampleFPSTextRenderer.h/.cpp | 內容                | 定義使用 Direct2D 和 DirectWrite 轉譯螢幕右下角中目前每秒畫面格數 (FPS) 值的類別物件。  |
| SamplePixelShader.hlsl       | 內容                | 包含最基本的像素著色器的高階著色器語言 (HLSL) 程式碼。                                            |
| SampleVertexShader.hlsl      | 內容                | 包含最基本的頂點著色器的高階著色器語言 (HLSL) 程式碼。                                           |
| ShaderStructures.h           | 內容                | 包含可用於傳送 MVP 矩陣及每個頂點資料至頂點著色器的著色器結構。  |
| pch.h/.cpp                   | 主要                   | 包含針對 Direct3D 應用程式所使用之 API (包括 DirectX 11 API) 所含的所有 Windows 系統。| 

### <a name="next-steps"></a>後續步驟

此時，您已學會如何使用 **DirectX 11 App (通用 Windows)** 範本建立 UWP DirectX 遊戲專案，並引進此專案提供的幾個元件和檔案。

下一節是[定義遊戲的 UWP 架構](tutorial--building-the-games-uwp-app-framework.md)。 我們將會檢查此遊戲如何使用和延伸許多範本提供的概念和元件。

 

 




