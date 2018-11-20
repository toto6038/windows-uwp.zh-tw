---
author: mtoepke
title: 移植遊戲迴圈
description: 說明如何為通用 Windows 平台 (UWP) 遊戲實作視窗，以及如何帶入遊戲迴圈，其中包含如何建置 IFrameworkView 以控制全螢幕的 CoreWindow。
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 移植, 遊戲迴圈, direct3d 9, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 4db2ed74144ead22643ece17a7496b6267f7e6b8
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7297962"
---
# <a name="port-the-game-loop"></a>移植遊戲迴圈



**摘要**

-   [第一部分：初始化 Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [第二部分：轉換轉譯架構](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   第三部分：移植遊戲迴圈


說明如何為通用 Windows 平台 (UWP) 遊戲實作視窗，以及如何帶入遊戲迴圈，其中包含如何建置 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 以控制全螢幕的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)。 [將簡單的 Direct3D 9 app 移植到 DirectX 11 和 UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 逐步解說的第三部分。

## <a name="create-a-window"></a>建立視窗


若要使用 Direct3D 9 檢視區來設定桌面視窗，必須針對傳統型應用程式實作傳統視窗架構。 我們過去必須建立 HWND、設定視窗大小、提供視窗處理回呼、讓它變成可見，以及其他動作等等。

UWP 環境現在提供一個更簡單的系統。 使用 DirectX 的 Microsoft Store 遊戲會實作 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)，而不是設定傳統視窗。 為了 DirectX App 與遊戲存在的這個介面，可直接在應用程式容器內的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 中執行。

> **注意：**  Windows 會提供受管理的指標，例如來源應用程式物件和[**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)的資源。 請參閱 [**Handle to Object Operator (^)**]https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx。

 

您的 "main" 類別需要繼承自 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)，並實作五種 **IFrameworkView** 方法：[**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)、[**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)、[**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)、[**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 及 [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)。 除了建立 **IFrameworkView** (這 (基本上) 將是您遊戲所在的位置)，您還需要實作 Factory 類別，以建立 **IFrameworkView** 的執行個體。 您的遊戲仍含有名稱為 **main()** 方法的可執行檔，但是所有的 main 都會使用 Factory 來建立 **IFrameworkView** 執行個體。

Main 函式

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

IFrameworkView Factory

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>移植遊戲迴圈


讓我們看看來自 Direct3D 9 實作的遊戲迴圈。 這個程式碼存在於應用程式的 main 函式。 這個迴圈的每一個反覆項目都會處理視窗訊息或轉譯框架。

Direct3D 9 傳統型遊戲中的遊戲迴圈

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

在遊戲的 UWP 版本中，遊戲迴圈很類似 - 但更簡單：

遊戲迴圈會在 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法中執行 (而非 **main()**)，因為我們的遊戲會在 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 類別中運作。

我們可以呼叫內建於應用程式視窗的 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 的 [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 方法，而不需實作訊息處理架構並呼叫 [**PeekMessage**](https://msdn.microsoft.com/library/windows/desktop/ms644943)。 遊戲迴圈不需要有分支和處理訊息 - 只需呼叫 **ProcessEvents** 並繼續。

Direct3D 11 Microsoft Store 遊戲中的遊戲迴圈

```cpp
// UWP apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

現在，我們的 UWP app 已設定與 DirectX 9 範例相同的基本圖形基礎架構，以及轉譯相同的彩色立方體。

## <a name="where-do-i-go-from-here"></a>接下來該怎麼做


將[移植 DirectX 11 常見問題集](directx-porting-faq.md)設定為書籤。

DirectX UWP 範本包含可靠的 Direct3D 裝置基礎結構，且已準備好與您的遊戲搭配使用。 如需挑選正確範本的指導方針，請參閱[從範本建立 DirectX 遊戲專案](user-interface.md)。

請參閱下列深入探討的 Microsoft Store 遊戲開發文章：

-   [逐步解說：使用 DirectX 建立簡單的 UWP 遊戲](tutorial--create-your-first-uwp-directx-game.md)
-   [遊戲的音訊](working-with-audio-in-your-directx-game.md)
-   [適用於遊戲的移動視角控制項](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 




