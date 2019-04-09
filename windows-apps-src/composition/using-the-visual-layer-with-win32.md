---
title: 使用 Win32 的視覺分層
description: 您可以使用視覺圖層來增強的 Win32 桌面應用程式 UI。
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP、 轉譯、 組合，win32
ms.localizationpriority: medium
ms.openlocfilehash: cfaa0d19b7a7361c5d604636c30beda0f416d063
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "58174068"
---
# <a name="using-the-visual-layer-with-win32"></a>使用 Win32 的視覺分層

您可以使用 Windows 執行階段撰寫 Api (也稱為[視覺分層](visual-layer.md)) 在您的 Win32 應用程式，以建立新式體驗也淺註冊 Windows 10 使用者。

本教學課程的完整程式碼是可在 GitHub 上：[Win32 HelloComposition 範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)。

需要精確控制其 UI 複合應用程式的通用 Windows 應用程式可以存取[Windows.UI.Composition](/uwp/api/windows.ui.composition)執行精細的控制如何撰寫和轉譯其 UI 的命名空間。 這個撰寫 API 並不限於 UWP 應用程式，不過。 Win32 桌面應用程式可以利用新式的組合中的系統 UWP 和 Windows 10。

## <a name="prerequisites"></a>先決條件

裝載 API UWP 有這些必要條件。

- 我們假設您已熟悉使用 Win32 與 UWP 應用程式開發。 如需詳細資訊，請參閱：
  - [開始使用 Win32 和C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)
  - [加強適用於 Windows 10 桌面應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 1803 （含） 以後版本
- 17134 或更新版本的 Windows 10 SDK

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>如何使用撰寫 Api，從 Win32 桌面應用程式

在本教學課程中，您會建立簡單的 Win32C++應用程式並為其新增 UWP 撰寫項目。 重點是在正確設定專案、 建立 interop 的程式碼，以及繪製簡單使用 Windows 撰寫 Api。 完成的應用程式看起來像這樣。

![執行的應用程式 UI](images/interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>建立C++Visual Studio 中的 Win32 專案

第一個步驟是在 Visual Studio 中建立 Win32 應用程式專案。

若要建立新的 Win32 應用程式專案中C++名為_HelloComposition_:

1. 開啟 Visual Studio，然後選取**檔案** > **新增** > **專案**。

    **新的專案** 對話方塊隨即開啟。
1. 底下**已安裝**分類中，展開**Visual C++** 節點，然後再選取**Windows Desktop**。
1. 選取  **Windows 桌面應用程式**範本。
1. 輸入名稱_HelloComposition_，然後按一下**確定**。

    Visual Studio 會建立專案，並開啟主要應用程式檔案的編輯器。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>將專案設定為使用 Windows 執行階段 Api

若要使用 Windows 執行階段 (WinRT) Api 在 Win32 應用程式中，我們使用C++/WinRT。 您需要設定 Visual Studio 專案，以新增C++/WinRT 支援。

(如需詳細資訊，請參閱 <<c0> [ 開始使用C++/WinRT-修改 Windows 桌面應用程式專案，以加入C++/WinRT 支援](../cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support))。</c0>

1. 從**專案**功能表上，開啟專案屬性 (_HelloComposition 屬性_)，並確定下列設定設為指定的值：

    - 針對**組態**，選取_所有組態_。 針對**平台**，選取_所有平台_。
    - **組態屬性** > **一般** > **Windows SDK 版本** = _10.0.17763.0_或更新版本

    ![設定 SDK 版本](images/interop/sdk-version.png)

    - **C /C++** > **語言** >   **C++語言標準** = _ISO C++ c++17 標準 (/ stf:c + + 17)_

    ![設定標準的語言](images/interop/language-standard.png)

    - **連結器** > **輸入** > **其他相依性**必須包含 「_windowsapp.lib_"。 如果它不包含在清單中，請將它加入。

    ![加入連結器相依性](images/interop/linker-dependencies.png)

1. 更新先行編譯標頭

    - 重新命名`stdafx.h`並`stdafx.cpp`要`pch.h`和`pch.cpp`分別。
    - 設定專案屬性**C /C++** > **先行編譯標頭** > **先行編譯標頭檔**到*pch.h*.
    - 尋找和取代`#include "stdafx.h"`與`#include "pch.h"`中的所有檔案。

        (**編輯** > **尋找和取代** > **檔案中尋找**)

        ![尋找和取代 stdafx.h](images/interop/replace-stdafx.png)

    - 在  `pch.h`，包括`winrt/base.h`和`unknwn.h`。

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

它是個不錯的主意，若要建置此專案目前以確定沒有任何錯誤，再繼續。

## <a name="create-a-class-to-host-composition-elements"></a>建立類別以主應用程式組合項目

主機內容來建立含視覺化的圖層，建立類別 (_CompositionHost_) 來管理 interop 及建立複合項目。 這是設定的您用來大部分裝載的 Api 組合，包括：

- 取得[複合](/uwp/api/windows.ui.composition.compositor)，這會建立並管理中的物件[Windows.UI.Composition](/uwp/api/windows.ui.composition)命名空間。
- 建立[DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) WinRT api 管理工作。
- 建立[DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget)和顯示的組合物件的組合容器。

我們讓此類別為單一子句，以避免執行緒問題。 比方說，您可以只建立一個發送器佇列，每個執行緒，因此 CompositionHost 的相同執行緒上的第二個執行個體具現化會產生錯誤。

> [!TIP]
> 如果您要檢查的完整程式碼的教學課程，以確定所有的程式碼是在正確的位置，當您完成本教學課程結尾處。

1. 將新的類別檔案新增到您的專案。
    - 在 **方案總管**，以滑鼠右鍵按一下_HelloComposition_專案。
    - 在操作功能表中，選取**新增** > **類別...**.
    - 在 [**加入類別**] 對話方塊中，將類別命名為_CompositionHost.cs_，然後按一下**新增**。

1. 包含標頭和_using_撰寫 interop 所需。
    - 在 CompositionHost.h，加入下列_包含_在檔案頂端。

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - 在 CompositionHost.cpp，加入下列_using_頂端的檔案之後,_包含_。

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. 編輯要使用單一子句模式的類別。
    - 在 CompositionHost.h，請建構函式私用。
    - 宣告的公用靜態_GetInstance_方法。

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - 在 CompositionHost.cpp，定義加入_GetInstance_方法。

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. 在 CompositionHost.h，複合、 DispatcherQueueController，和 DesktopWindowTarget 變數宣告私用成員。

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. 新增公用的方法，以初始化撰寫 interop 物件。
    > [!NOTE]
    > 在 _初始化_，您呼叫_EnsureDispatcherQueue_， _CreateDesktopWindowTarget_，以及_CreateCompositionRoot_方法。 您在後續步驟中建立這些方法。

    - 在 CompositionHost.h，宣告名為公用方法_初始化_會做為引數的 HWND。

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - 在 CompositionHost.cpp，定義加入_初始化_方法。

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. 將使用 Windows 撰寫的執行緒上建立發送器佇列。

    必須有發送器佇列，因此這個方法在初始化期間呼叫第一個執行緒上建立複合。

    - 在 CompositionHost.h，宣告名為私用方法_EnsureDispatcherQueue_。

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - 在 CompositionHost.cpp，定義加入_EnsureDispatcherQueue_方法。

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. 註冊您的應用程式視窗做為撰寫目標。
    - 在 CompositionHost.h，宣告名為私用方法_CreateDesktopWindowTarget_會做為引數的 HWND。

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - 在 CompositionHost.cpp，定義加入_CreateDesktopWindowTarget_方法。

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. 建立來保存視覺物件的根視覺容器。
    - 在 CompositionHost.h，宣告名為私用方法_CreateCompositionRoot_。

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - 在 CompositionHost.cpp，定義加入_CreateCompositionRoot_方法。

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 24, 24, 0 });
        m_target.Root(root);
    }
    ```

建置專案，現在先確定沒有任何錯誤。

這些方法會設定 UWP 視覺分層和 Win32 Api 之間的互通性所需的元件。 現在您可以將內容加入您的應用程式。

### <a name="add-composition-elements"></a>將複合項目

就地基礎結構，您現在可以產生您想要顯示的組合內容。

針對此範例中，您新增程式碼，建立一個簡單的正方形[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)。

1. 新增撰寫項目。
    - 在 CompositionHost.h，宣告名為公用方法_AddElement_採用 3 **float**做為引數的值。

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - 在 CompositionHost.cpp，定義加入_AddElement_方法。

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
            visual.Size({ size, size });
            visual.Offset({ x, y, 0.0f, });

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>建立並顯示視窗

現在，您可以將 UWP 組合內容加入 Win32 UI。 使用您的應用程式_InitInstance_方法來初始化 Windows 撰寫和產生的內容。

1. 在 HelloComposition.cpp，包括_CompositionHost.h_在檔案頂端。

    ```cppwinrt
    #include "CompositionHost.h"
    ```

1. 在 `InitInstance`方法，加入程式碼來初始化，並使用 CompositionHost 類別。

    新增下列程式碼，建立 HWND 之後，然後再呼叫`ShowWindow`。

    ```cppwinrt
    CompositionHost* compHost = CompositionHost::GetInstance();
    compHost->Initialize(hWnd);
    compHost->AddElement(150, 10, 10);
    ```

您現在可以建置並執行您的應用程式。 如果您要檢查的完整程式碼結尾的教學課程，以確定所有的程式碼是在正確的位置。

當您執行應用程式時，您應該會看到藍色方框，加入至 UI。

## <a name="additional-resources"></a>其他資源

- [Win32 HelloComposition 範例 (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [開始使用 Win32 和C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)(UWP)
- [增強您的桌面應用程式適用於 Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 命名空間](/uwp/api/windows.ui.composition)(UWP)

## <a name="complete-code"></a>完整程式碼

以下是 CompositionHost 類別和 InitInstance 方法的完整程式碼。

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 24, 24, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
        visual.Size({ size, size });
        visual.Offset({ x, y, 0.0f, });

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp---initinstance"></a>HelloComposition.cpp - InitInstance

```cppwinrt
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

   if (!hWnd)
   {
      return FALSE;
   }

   CompositionHost* compHost = CompositionHost::GetInstance();
   compHost->Initialize(hWnd);
   compHost->AddElement(150, 10, 10);

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}
```