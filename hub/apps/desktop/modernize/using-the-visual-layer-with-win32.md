---
title: 使用視覺層搭配 Win32
description: 使用視覺層來增強 Win32 傳統型應用程式的 UI。
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, 轉譯, 組合, win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: c9b4ec38b0dd1f6eca3f43cfded74c6292c08100
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215193"
---
# <a name="using-the-visual-layer-with-win32"></a>使用視覺層搭配 Win32

您可以在 Win32 應用程式中使用 Windows 執行階段 Composition API (又稱為[視覺層](/windows/uwp/composition/visual-layer))，建立適用於 Windows 10 使用者的現代化體驗。

您可在 GitHub 上取得本教學課程的完整程式碼：[Win32 HelloComposition 範例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)。

需要精確控制其 UI 組合的通用 Windows 應用程式可以存取 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 命名空間，更精細地控制其 UI 的組成與轉譯方式。 不過，此 Composition API 並不限於 UWP 應用程式。 Win32 傳統型應用程式可利用 UWP 和 Windows 10 中的新式組合系統。

## <a name="prerequisites"></a>必要條件

UWP 主控 API 具備下列先決條件。

- 假設您對使用 Win32 和 UWP 進行應用程式開發已有一些熟悉。 如需詳細資訊，請參閱：
  - [開始使用 Win32 和 C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [開始使用 Windows 10 應用程式](/windows/uwp/get-started/)
  - [增強您的 Windows 10 傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 版本 1803 或更新版本
- Windows 10 SDK 17134 或更新版本

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>如何從 Win32 傳統型應用程式使用 Composition API

在本教學課程中，您會建立簡單的 Win32 C++ 應用程式，並在其中新增 UWP 組合 元素。 重點在於正確地設定專案、建立 Interop 程式碼，以及使用 Windows Composition API 繪製簡單的東西。 完成的應用程式看起來像這樣。

![執行中的應用程式 UI](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>在 Visual Studio 中建立 C++ Win32 專案

第一個步驟是在 Visual Studio 中建立 Win32 應用程式專案。

若要使用 C++ 建立名為 _HelloComposition_ 的 Win32 應用程式專案：

1. 開啟 Visual Studio，然後選取 [檔案]   > [新增]   > [專案]  。

    [新增專案]  對話方塊隨即開啟。
1. 在 [已安裝]  類別下，展開 [Visual C++]  節點，然後選取 [Windows Desktop]  。
1. 選取 [Windows 傳統型應用程式]  範本。
1. 輸入名稱 _HelloComposition_，然後按一下 [確定]  。

    Visual Studio 會建立專案，並開啟主要應用程式檔案的編輯器。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>設定專案以使用 Windows 執行階段 API

若要在 Win32 應用程式中使用 Windows 執行階段 (WinRT) API，我們會使用 C++/WinRT。 您必須設定您的 Visual Studio 專案，以新增 C++/WinRT 支援。

(如需詳細資訊，請參閱[開始使用 C++/WinRT - 修改 Windows 傳統型應用程式專案來新增 C++/WinRT 支援](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。)

1. 從 [專案]  功能表，開啟專案屬性 (_HelloComposition 屬性_)，並確定下列設定已設定為指定的值：

    - 針對 [設定]  ，選取 [所有設定]  。 針對 [平台]  ，選取 [所有平台]  。
    - [設定屬性]   > [一般]   > [Windows SDK 版本]   = _10.0.17763.0_ 或更高版本

    ![設定 SDK 版本](images/visual-layer-interop/sdk-version.png)

    - [C/C++]   > [語言]   > [C++ 語言標準]   = _ISO C++ 17 標準 (/stf:c++17)_

    ![設定語言標準](images/visual-layer-interop/language-standard.png)

    - [連結器]   > [輸入]   > [其他相依性]  必須包含 "_windowsapp.lib_"。 如果未包含在清單中，請加以新增。

    ![新增連結器相依性](images/visual-layer-interop/linker-dependencies.png)

1. 更新預先編譯的標頭

    - 將 `stdafx.h` 和 `stdafx.cpp` 分別重新命名為 `pch.h` 和 `pch.cpp`。
    - 將專案屬性 [C/C++]   > [預先編譯的標頭]   > [預先編譯的標頭]  設為 [pch.h]  。
    - 在所有檔案中以 `#include "pch.h"` 尋找並取代 `#include "stdafx.h"`。

        ([編輯]   > [尋找和取代]   > [在檔案中尋找]  )

        ![尋找並取代 stdafx.h](images/visual-layer-interop/replace-stdafx.png)

    - 在 `pch.h` 中包含 `winrt/base.h` 和 `unknwn.h`。

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

在此時建立專案是個不錯的主意，可確定沒有任何錯誤，再繼續進行。

## <a name="create-a-class-to-host-composition-elements"></a>建立類別來裝載組合元素

若要裝載使用視覺層建立的內容，請建立類別 (_CompositionHost_) 來管理 Interop 並建立組合元素。 您可以在這裡進行用於託管 Composition API 的大部分組態，包括：

- 取得 [Compositor](/uwp/api/windows.ui.composition.compositor)，其可在 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 命名空間中會建立和管理物件。
- 建立 [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) 來管理 WinRT API 的工作。
- 建立 [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) 和組合容器，以顯示組合物件。

我們將此類別設為單一實體，以避免執行緒問題。 例如，您只能為每個執行緒建立一個發送器佇列，因此在相同執行緒上具現化 CompositionHost 的第二個執行個體會造成錯誤。

> [!TIP]
> 如有需要，請在本教學課程最後檢查完整的程式碼，以確保當您逐步進行教學課程時，所有程式碼皆位於正確的位置。

1. 將新的類別檔案新增到您的專案。
    - 在 [方案總管]  中，以滑鼠右鍵按一下 [HelloComposition]  專案。
    - 在內容功能表中，選取 [新增]   > [類別...]  。
    - 在 [新增類別]  對話方塊中，將類別命名為 _CompositionHost.cs_，然後按一下 [新增]  。

1. 包含組合 Interop 所需的標頭和 _using_。
    - 在 CompositionHost.h 中，在檔案頂端新增下列 _include_。

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - 在 CompositionHost.cpp 中，在檔案頂端的任何 _include_ 之後，新增下列 _using_。

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. 編輯類別，以使用單一模式。
    - 在 CompositionHost.h 中，將建構函式設為私人的。
    - 宣告公用靜態 _GetInstance_ 方法。

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

    - 在 CompositionHost.cpp 中，新增 _GetInstance_ 方法的定義。

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. 在 CompositionHost.h 中，宣告 Compositor、DispatcherQueueController 和 DesktopWindowTarget 的私人成員變數。

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. 新增公用方法，以初始化組合 Interop 物件。
    > [!NOTE]
    > 在 _Initialize_中，您可呼叫 _EnsureDispatcherQueue_、_CreateDesktopWindowTarget_ 和 _CreateCompositionRoot_ 方法。 您會在後續步驟中建立這些方法。

    - 在 CompositionHost.h 中，宣告名為 _Initialize_ 的公用方法，其採用 HWND 作為引數。

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - 在 CompositionHost.cpp 中，新增 _Initialize_ 方法的定義。

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. 在即將使用 Windows Composition 的執行緒上建立發送器佇列。

    必須在具有發送器佇列的執行緒上建立 Compositor，因此會在初始化期間先呼叫這個方法。

    - 在 CompositionHost.h 中，宣告名為 _EnsureDispatcherQueue_ 的私人方法。

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - 在 CompositionHost.cpp 中，新增 _EnsureDispatcherQueue_ 方法的定義。

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

1. 將應用程式的視窗註冊為組合目標。
    - 在 CompositionHost.h 中，宣告名為 _CreateDesktopWindowTarget_ 的私人方法，其會採用 HWND 作為引數。

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - 在 CompositionHost.cpp 中，新增 _CreateDesktopWindowTarget_ 方法的定義。

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

1. 建立根視覺容器來保存視覺物件。
    - 在 CompositionHost.h 中，宣告名為 _CreateCompositionRoot_ 的私人方法。

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - 在 CompositionHost.cpp 中，新增 _CreateCompositionRoot_ 方法的定義。

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

立即建立專案，以確保沒有任何錯誤。

這些方法會設定在 UWP 視覺層與 Win32 API 之間進行 Interop 所需的元件。 您現在可以將內容新增至您的應用程式。

### <a name="add-composition-elements"></a>新增組合元素

基礎結構已就緒之後，即可產生要顯示的 Compositio 內容。

在此範例中，您會新增程式碼來建立隨機彩色方塊 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)，其中包含的動畫可使其在短暫延遲之後卸除。

1. 新增組合元素。
    - 在 CompositionHost.h 中，宣告名為 _AddElement_ 的公用方法，其採用 3 個 **float** 值作為引數。

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - 在 CompositionHost.cpp 中，新增 _AddElement_ 方法的定義。

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>建立並顯示視窗

現在，您可以將按鈕和 UWP 組合內容新增至您的 Win32 UI。

1. 在 HelloComposition.cpp 的檔案頂端，包含 _CompositionHost.h_、定義 BTN_ADD，以及取得 CompositionHost 的執行個體。

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. 在 `InitInstance` 方法中，變更所建立的視窗大小。 (在這一行中，將 `CW_USEDEFAULT, 0` 變更為 `900, 672`。)

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. 在 WndProc 函式中，將 `case WM_CREATE` 新增至 _message_ switch 區塊。 在此情況下，您會初始化 CompositionHost 並建立按鈕。

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. 此外，在 WndProc 函式中，處理 button click 以將組合元素新增至 UI。 

    將 `case BTN_ADD` 新增至 WM_COMMAND 區塊內的 _wmId_ switch 區塊。

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

您現在可以建置及執行您的應用程式。 如有需要，請在本教學課程最後檢查完整的程式碼，以確保所有程式碼皆位於正確的位置。

當您執行應用程式，並按一下按鈕時，應該會看見新增到 UI 的動畫方塊。

## <a name="additional-resources"></a>其他資源

- [Win32 HelloComposition 範例 (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [開始使用 Win32 和 C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [開始使用 Windows 10 應用程式](/windows/uwp/get-started/) (UWP)
- [增強您的 Windows 10 傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 命名空間](/uwp/api/windows.ui.composition) (UWP) (英文)

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
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp (部分)

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```