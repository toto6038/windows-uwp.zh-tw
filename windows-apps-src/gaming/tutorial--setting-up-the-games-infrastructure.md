---
author: mtoepke
title: "設定遊戲專案"
description: "組合遊戲的第一步是在 Microsoft Visual Studio 中設定一個專案，透過這種方式可以將所需的程式碼基礎結構數量減到最少。"
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d4d7864f9689df0919b53ee70b8e18f8d812b2b0

---

# 設定遊戲專案


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

組合遊戲的第一步是在 Microsoft Visual Studio 中設定一個專案，透過這種方式可以將所需的程式碼基礎結構數量減到最少。 使用正確的範本並設定專用於遊戲開發的專案，可以為您節省很多時間和麻煩。 我們會逐步引導您如何準備和設定簡單的遊戲專案。

## 目標


-   了解如何在 Visual Studio 中設定 Direct3D 遊戲專案。

## 設定遊戲專案


您可以從頭開始撰寫遊戲，只需要一個好用的文字編輯器、幾個範例，以及足夠的腦力。 但是這恐怕不是利用您的時間最有效的方式。 如果您是通用 Windows 平台 (UWP) 開發的初學者，為什麼不讓 Visual Studio 為您減輕一些負擔呢？ 以下是讓您的專案熱烈展開所需要做的事。

## 1. 挑選正確的範本


Visual Studio 範本所包含的設定集合和程式碼檔案，是專門針對使用慣用語言和技術的特定應用程式類型所設計。 在 Microsoft Visual Studio 2015 中，您將發現很多範本能夠讓遊戲和圖形應用程式開發變得非常容易。 如果您不使用範本，就必須自己開發許多基本圖形轉譯和顯示架構，對遊戲開發初學者而言可能有點困難。

這個教學課程的正確範本是名為「DirectX 11 應用程式 (通用 Windows)」的範本。 在 Visual Studio 2015 中，按一下 \[檔案\] \[新增專案\]然後：

1.  從 \[範本\] 中，依序選取 \[Visual C++\]、\[Windows\]、\[通用\]。
2.  在中央窗格中，選取 \[DirectX 11 應用程式 (通用 Windows)\]。
3.  提供遊戲專案名稱，然後按一下 \[確定\]。

![選取 direct3d 應用程式範本](images/simple-dx-game-vs-new-proj.png)

這個範本為您提供使用 DirectX 搭配 C++ 的 UWP app 的基本架構。 按下 F5 開始建置和執行吧！ 看看那個粉藍色螢幕。 花點時間看看範本提供的程式碼。 此範本會建立多個程式碼檔案，其中包含使用 DirectX 搭配 C++ 的 UWP 應用程式所適用的基本功能。 我們將在[步驟 3](#3-review-the-included-libraries-and-headers) 中進一步討論其他程式碼檔案。 現在，我們很快地看一下 **App.h**。

```cpp
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
        // Application lifecycle event handlers.
        void OnActivated(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView, Windows::ApplicationModel::Activation::IActivatedEventArgs^ args);
        void OnSuspending(Platform::Object^ sender, Windows::ApplicationModel::SuspendingEventArgs^ args);
        void OnResuming(Platform::Object^ sender, Platform::Object^ args);

        // Window event handlers.
        void OnWindowSizeChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::WindowSizeChangedEventArgs^ args);
        void OnVisibilityChanged(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::VisibilityChangedEventArgs^ args);
        void OnWindowClosed(Windows::UI::Core::CoreWindow^ sender, Windows::UI::Core::CoreWindowEventArgs^ args);

        // DisplayInformation event handlers.
        void OnDpiChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnOrientationChanged(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);
        void OnDisplayContentsInvalidated(Windows::Graphics::Display::DisplayInformation^ sender, Platform::Object^ args);

    private:
        std::shared_ptr<DX::DeviceResources> m_deviceResources;
        std::unique_ptr<MyAwesomeGameMain> m_main;
        bool m_windowClosed;
        bool m_windowVisible;
    };
```

在實作定義檢視提供者的 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700469) 介面時，您要建立以下 5 個方法：[**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)、[**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)、[**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)、[**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 以及 [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)。 這些方法是由遊戲啟動時建立的應用程式單例執行，而且會載入您 app 的所有資源，以及連接適當的事件處理常式。

您的 **main** 方法是在 **App.cpp** 來源檔中。 它的外觀如下：

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

現在，它會從檢視提供者 Factory (**App.h** 定義的 **Direct3DApplicationSource**) 來建立 Direct3D 檢視提供者的執行個體，並將它傳遞給應用程式單例執行 ([**CoreApplication::Run**](https://msdn.microsoft.com/library/windows/apps/hh700469))。 這表示遊戲的起點將位於 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法實作的內文，在這個情況就是 **App::Run**。 以下是程式碼：

```cpp
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

如果遊戲的視窗沒有關閉，它會分派所有事件、更新計時器以及轉譯和顯示圖形管線的結果。 我們會在[定義遊戲的 UWP 架構](tutorial--building-the-games-metro-style-app-framework.md)和[組合轉譯管線](tutorial--assembling-the-rendering-pipeline.md)中詳細討論。 現在，您應該對 UWP DirectX 遊戲的基本程式碼結構有大致的了解。

## 2. 檢視並更新 package.appxmanifest 檔案


範本不只有程式碼檔案。 
            **package.appxmanifest** 檔案包含有關您專案的中繼資料，可用來封裝和啟動您的遊戲，以及提交到 Windows 市集。 它也包含玩家系統的重要資訊，用來存取遊戲執行所需的系統資源。

在 \[方案總管\] 中的 package.appxmanifest 檔案上按兩下，啟動 \[資訊清單設計工具\]。 您會看到這個檢視：

![package.appx 資訊清單編輯器。](images/simple-dx-game-vs-app-manifest.png)

如需 **package.appxmanifest** 檔案和封裝的詳細資訊，請參閱[資訊清單設計工具](https://msdn.microsoft.com/library/windows/apps/br230259.aspx)。 現在，看看 \[功能\] 索引標籤，以及其中提供的選項。

![direct3d 應用程式的預設功能。](images/simple-dx-game-vs-capabilities.png)

如果您未選取遊戲所使用的功能 (例如透過 \[網際網路\] 存取全球高分板)，就無法存取對應的資源或功能。 建立新遊戲時，記得選取遊戲執行所需的功能！

現在，讓我們看看隨附於 **DirectX 11 應用程式 (通用 Windows)** 範本的其餘檔案。

## 3. 檢視包含的類別庫和標頭


有幾個檔案我們尚未討論。 這些檔案提供 Direct3D 遊戲開發案例中常用的其他工具和支援。

| 範本來源檔         | 說明                                                                                                                                                                                                            |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StepTimer.h                  | 定義高解析度的計時器，對於遊戲或互動式轉譯應用程式非常實用。                                                                                                                                       |
| Sample3DSceneRenderer.h/.cpp | 定義基本轉譯器實作，將 Direct3D 交換鏈結與圖形卡連接至您的使用 DirectX 的 UWP 應用程式。                                                                                            |
| DirectXHelper.h              | 實作單一方法 **DX::ThrowIfFailed**，將 DirectX API 傳回的錯誤 HRESULT 值轉換成 Windows 執行階段例外狀況。 使用這個方法放置偵錯 DirectX 錯誤的中斷點。 |
| pch.h/.cpp                   | 包含針對 Direct3D 應用程式所使用之 API (包括 DirectX 11 API) 所含的所有 Windows 系統。                                                                                                           |
| SamplePixelShader.hlsl       | 包含最基本的像素著色器的高階著色器語言 (HLSL) 程式碼。                                                                                                                                     |
| SampleVertexShader.hlsl      | 包含最基本的頂點著色器的高階著色器語言 (HLSL) 程式碼。                                                                                                                                    |

 

### 後續步驟

現在，您已可建立使用 DirectX 的 UWP 遊戲專案，並識別 DirectX 11 應用程式 (通用 Windows) 範本提供的元件和檔案。

在下一個教學課程[定義遊戲的 UWP 架構](tutorial--building-the-games-metro-style-app-framework.md)中，我們會使用一個已完成的遊戲，並檢驗它如何使用和擴充範本所提供的許多概念和元件。

 

 







<!--HONumber=Jun16_HO4-->


