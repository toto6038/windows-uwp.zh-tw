---
description: 本文說明如何在您的桌面C++ Win32 應用程式中裝載 UWP XAML UI。
title: 在 C++ Win32 應用程式中使用 UWP XAML 裝載 API
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10，uwp，windows forms，wpf，win32，xaml 群島
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 9e4fdc8366e26bcd7e106bf070cb42ed2cd1a49f
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683681"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>在 C++ Win32 應用程式中使用 UWP XAML 裝載 API

從 Windows 10 版本1903開始，非 UWP 桌面應用程式（包括C++ WIN32、WPF 和 Windows Forms 應用程式）可以使用*UWP XAML 裝載 API* ，將 uwp 控制項裝載于與視窗控制碼（HWND）相關聯的任何 UI 專案中。 此 API 可讓非 UWP 桌面應用程式使用僅透過 UWP 控制項提供的最新 Windows 10 UI 功能。 例如，非 UWP 桌面應用程式可以使用此 API 來裝載使用[流暢設計系統](/windows/uwp/design/fluent-design-system/index)並支援[WINDOWS Ink](/windows/uwp/design/input/pen-and-stylus-interactions)的 UWP 控制項。

UWP XAML 裝載 API 提供更廣泛的控制項集合的基礎，讓開發人員能夠將流暢的 UI 帶入非 UWP 桌面應用程式。 這項功能稱為「 *XAML 島*」。 如需這項功能的總覽，請參閱[在桌面應用程式中裝載 UWP XAML 控制項（Xaml 島）](xaml-islands.md)。

> [!NOTE]
> 如果您有關于 XAML Islands 的意見反應，請在[Microsoft 工具](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)組的 Win32 存放庫中建立新的問題，並在該處留下您的意見。 如果您想要私下提交意見反應，可以將它傳送給 XamlIslandsFeedback@microsoft.com。 您的深入解析和案例對我們而言非常重要。

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>您應該使用 UWP XAML 裝載 API 嗎？

UWP XAML 裝載 API 提供了在桌面應用程式中裝載 UWP 控制項的低層級基礎結構。 某些類型的桌面應用程式可以選擇使用替代、更方便的 Api 來完成此目標。  

* 如果您有C++ Win32 桌面應用程式，而且想要在應用程式中裝載 uwp 控制項，則必須使用 UWP XAML 裝載 API。 這些類型的應用程式沒有任何替代方案。

* 針對 WPF 和 Windows Forms 應用程式，強烈建議您在 Windows 社區工具組中使用[XAML 島 .net 控制項](xaml-islands.md#wpf-and-windows-forms-applications)，而不是直接使用 UWP XAML 裝載 API。 如果您直接使用 UWP XAML 裝載 API，包括鍵盤導覽和版面配置變更，這些控制項就會在內部使用 UWP XAML 裝載 API，並實作為您必須自行處理的所有行為。

因為我們建議只有C++ win32 應用程式使用 UWP XAML 裝載 API，本文主要會提供C++ Win32 應用程式的指示和範例。 不過，如果您選擇，您可以在 WPF 和 Windows Forms 應用程式中使用 UWP XAML 裝載 API。 本文指向適用于 WPF 的[主控制項](xaml-islands.md#host-controls)和 Windows Forms 在 Windows 社區工具組中的相關原始程式碼，讓您可以看到 UWP XAML 裝載 API 如何由這些控制項使用。

## <a name="prerequisites"></a>先決條件

XAML Islands 需要 Windows 10 1903 版（或更新版本）和對應的 Windows SDK 組建。 若要在C++ Win32 應用程式中使用 XAML Islands，您必須先設定您的專案。

### <a name="support-for-cwinrt"></a>支援C++/WinRT

請確定您的專案支援[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)：

* 針對新的專案，您可以安裝[ C++/WinRT Visual Studio 擴充功能（VSIX）](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) ，並使用該C++延伸模組中包含的其中一個/WinRT 專案範本。
* 針對現有的專案，您可以在專案中安裝[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 套件。

如需這些選項的詳細資訊，請參閱[這篇文章](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

### <a name="configure-your-project-for-app-deployment"></a>設定專案以進行應用程式部署

選擇下列其中一個選項，準備您的專案以進行部署：

* **在 MSIX 套件中封裝您的應用程式**。 將您的應用程式封裝在[MSIX 套件](https://docs.microsoft.com/windows/msix/)中，可提供許多部署和執行時間的優點。
    1. 安裝 Windows 10 版本 1903 SDK （版本10.0.18362）或更新版本。
    2. 將[Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)新增至您的方案，並將參考新增至您C++的/Win32 專案，以將您的應用程式封裝在 MSIX 套件中。

* **安裝 Microsoft 工具組. SDK 套件**。 如果您不想要在 MSIX 套件中封裝您的應用程式，您可以安裝 6.0.0-preview7 或更新[版本。](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) 此套件提供數個組建和執行時間資產，讓 XAML 孤島可在您的應用程式中運作。 請確定已選取 [**包含發行**前版本] 選項，讓您可以查看此套件的最新預覽版。

> [!NOTE]
> 這些指示的舊版中，您會將 `maxversiontested` 元素加入至專案中的應用程式資訊清單。 只要您使用上述其中一個選項，就不再需要將此元素新增至資訊清單。

### <a name="additional-requirements-for-custom-uwp-controls"></a>自訂 UWP 控制項的其他需求

如果您裝載的是自訂 UWP 控制項（例如，由數個可搭配使用的 UWP 控制群組成的使用者控制項），您必須遵循[本節中的](#host-a-custom-uwp-control)其他指示。 您也必須擁有自訂控制項的原始程式碼，才能使用您的應用程式進行編譯。

## <a name="architecture-of-the-api"></a>API 的架構

UWP XAML 裝載 API 包含這些主要的 Windows 執行階段類型和 COM 介面。

|  類型或介面 | 說明 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 此類別代表 UWP XAML 架構。 此類別提供單一靜態[InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法，可在桌面應用程式中的目前線程上初始化 UWP XAML 架構。 |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 此類別代表您在桌面應用程式中裝載之 UWP XAML 內容的實例。 這個類別最重要的成員是[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)屬性。 您會將這個屬性指派給您要裝載的[Windows.](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) node.js。 這個類別也有其他成員，可將鍵盤焦點導覽傳入和傳出 XAML 島。 |
| IDesktopWindowXamlSourceNative | 這個 COM 介面提供**AttachToWindow**方法，您可以用它將應用程式中的 XAML 島附加至父 UI 元素。 每個**DesktopWindowXamlSource**物件都會執行此介面。 |
| IDesktopWindowXamlSourceNative2 | 這個 COM 介面提供**PreTranslateMessage**方法，可讓 UWP XAML 架構正確地處理特定的 Windows 訊息。 每個**DesktopWindowXamlSource**物件都會執行此介面。 |

下圖說明在傳統型應用程式中裝載的 XAML 島中，物件的階層結構。

* 在基底層級是您的應用程式中，您想要在其中裝載 XAML 島的 UI 元素。 這個 UI 元素必須有視窗控制碼（HWND）。 您可以在其中裝載 XAML 島的 UI 元素範例C++包括 Win32 應用程式的[視窗](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)、 [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) for WPF 應用程式，以及適用于 Windows Forms 應用程式的[system.web 控制項](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

* 下一個層級是**DesktopWindowXamlSource**物件。 這個物件會提供裝載 XAML 島的基礎結構。 您的程式碼負責建立此物件，並將它附加至父 UI 元素。

* 當您建立**DesktopWindowXamlSource**時，此物件會自動建立原生子視窗來裝載 UWP 控制項。 這個原生子視窗大多會從您的程式碼中抽象化，但如有必要，您可以存取其控制碼（HWND）。

* 最後，最上層是您想要在桌面應用程式中裝載的 UWP 控制項。 這可以是衍生自[Windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 uwp 物件，包括 Windows SDK 所提供的任何 uwp 控制項，以及自訂使用者控制項。

![DesktopWindowXamlSource 架構](images/xaml-islands/xaml-hosting-api-rev2.png)

## <a name="related-samples"></a>相關範例

您在程式碼中使用 UWP XAML 裝載 API 的方式，取決於您的應用程式類型、應用程式的設計和其他因素。 為了協助說明如何在完整應用程式的內容中使用此 API，本文會參考下列範例中的程式碼。

### <a name="c-win32"></a>C++32

下列範例示範如何在C++ Win32 應用程式中使用 UWP XAML 裝載 API：

* [簡單的 XAML 島範例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)。 這個範例示範在未封裝C++的 Win32 應用程式（也就是未內建于 MSIX 套件中的應用程式）中裝載 UWP 控制項的基本部署。

* [具有自訂控制項範例的 XAML 島](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)。 這個範例示範在未封裝C++的 Win32 應用程式中裝載自訂 UWP 控制項的完整執行，以及處理其他行為（例如鍵盤輸入和焦點導覽）。 

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows Form

Windows 社區工具組中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項，做為在 WPF 和 Windows Forms 應用程式中使用 UWP 裝載 API 的參考範例。 原始程式碼可在下列位置取得：

* 若為 WPF 版本的控制項，請[移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本衍生自[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。

* 如需控制項的 Windows Forms 版本，請[移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows Forms 版本衍生自[system.web](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

## <a name="host-a-standard-uwp-control"></a>裝載標準 UWP 控制項

本節將逐步引導您使用 UWP XAML 裝載 API，在新C++的 Win32 應用程式中裝載標準 uwp 控制項（也就是由 Windows SDK 或 WinUI 程式庫所提供的控制項）。 程式碼是以簡單的[XAML 島範例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)為基礎，本節將討論一些最重要的程式碼部分。 如果您有現有C++的 Win32 應用程式專案，您可以針對您的專案調整這些步驟和程式碼範例。

### <a name="configure-the-project"></a>設定專案

1. 在已安裝 Windows 10 版本 1903 SDK （版本10.0.18362）或更新版本的 Visual Studio 2019 中，建立新的**Windows 桌面應用程式**專案。 此專案可在**C++** 、 **Windows**和**桌面**專案篩選下取得。

2. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後按一下 [重定**目標方案**]，選取**10.0.18362.0**或更新版本的 SDK，然後按一下 **[確定]** 。

3. 安裝[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 套件：

    1. 在 [方案總管] 中以滑鼠右鍵按一下專案，然後選擇 [管理 NuGet 封裝]。
    2. 選取 [**流覽**] 索引標籤，搜尋[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)套件，然後安裝此套件的最新版本。

4. 安裝[Microsoft 工具](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK)組. SDK NuGet 套件：

    1. 在 [ **NuGet 套件管理員**] 視窗中，確認已選取 [**包含發行**前版本]。
    2. 選取 [**流覽**] 索引標籤，[搜尋 [6.0.0](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) -preview7 （或更新版本）] 此套件的 [app-v] 套件。

### <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>使用 XAML 裝載 API 來裝載 UWP 控制項

使用 XAML 裝載 API 來裝載 UWP 控制項的基本程式，會遵循下列一般步驟：

1. 為目前的執行緒初始化 UWP XAML 架構，然後您的應用程式才會建立它將裝載的任何[Windows.](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) node.js 物件。 有幾種方式可以執行這項操作，視您計畫建立將裝載控制項的[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)物件而定。

    * 如果您的應用程式在建立**DesktopWindowXamlSource**物件之前先建立它，則會在您將**DesktopWindowXamlSource**物件具現化時，為您初始化此**架構。** 在此案例中，您不需要加入自己的任何程式碼來初始化架構。

    * 不過，如果**您的應用**程式在建立將裝載這些物件的**DesktopWindowXamlSource**物件之前，會先建立它，則您的應用程式必須呼叫靜態[WindowsXamlManager. InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法，以明確初始化 UWP Xaml 架構，然後才會具現化**windows. UI. uielement**物件。 當裝載**DesktopWindowXamlSource**的父系 UI 元素具現化時，您的應用程式通常應該呼叫這個方法。

    > [!NOTE]
    > 這個方法會傳回[WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)物件，其中包含 UWP XAML 架構的參考。 您可以視需要在指定的執行緒上建立多個**WindowsXamlManager**物件。 不過，因為每個物件都持有 UWP XAML 架構的參考，所以您應該處置物件，以確保最終會釋放 XAML 資源。

2. 建立[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)物件，並將它附加至與視窗控制碼相關聯之應用程式中的父 UI 元素。

    若要這樣做，您必須遵循下列步驟：

    1. 建立**DesktopWindowXamlSource**物件，並將它轉換成**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2** COM 介面。
        > [!NOTE]
        > 這些介面是在 Windows SDK 中的**desktopwindowxamlsource**標頭檔中宣告的。 根據預設，此檔案位於% programfiles （x86）% \ Windows Kits\10\Include\\< 組建編號\>\um。

    2. 呼叫**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**介面的**AttachToWindow**方法，並傳入應用程式中父 UI 元素的視窗控制碼。

    3. 設定**DesktopWindowXamlSource**中所包含之內部子視窗的初始大小。 根據預設，這個內部子視窗會設定為0的寬度和高度。 如果您未設定視窗的大小，將不會顯示您新增至**DesktopWindowXamlSource**的任何 UWP 控制項。 若要存取**DesktopWindowXamlSource**中的內部子視窗，請使用**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**介面的**WindowHandle**屬性。

3. 最後，**將您要裝載的 node.js**指派給**DesktopWindowXamlSource**物件的[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)屬性。

下列步驟和程式碼範例示範如何執行上述程式：

1. 在專案的 [**來源**檔案] 資料夾中，開啟預設的**WindowsProject .cpp**檔案。 刪除檔案的整個內容，並加入下列 `include` 和 `using` 語句。 除了標準C++和 UWP 標頭和命名空間之外，這些語句還包含 XAML 島特定的數個專案。

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. 複製上一節後面的下列程式碼。 此程式碼會定義應用程式的**WinMain**函式。 此函式會初始化基本的視窗，並使用 XAML 裝載 API 在視窗中裝載簡單的 UWP **TextBlock**控制項。

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. 複製上一節後面的下列程式碼。 這段程式碼會定義視窗的[視窗進程](https://docs.microsoft.com/windows/win32/learnwin32/writing-the-window-procedure)。

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. 儲存程式碼檔案，並建立並執行應用程式。 確認您在應用程式視窗中看到 UWP **TextBlock**控制項。
    > [!NOTE]
    > 您可能會看到數個組建警告，包括 `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` 和 `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`。 這些警告是目前工具和 NuGet 套件的已知問題，而且可以忽略。

如需示範這些工作的完整範例，請參閱下列程式碼檔案：

* **C++32**
  * 請參閱[SIMPLE XAML 孤島範例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)中的[HelloWindowsDesktop。](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_SimpleApp/Win32DesktopApp/HelloWindowsDesktop.cpp)
  * 請參閱[具有自訂控制項範例的 XAML 島](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge。](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)

* **WPF：** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs)檔案。  

* **Windows Forms：** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs)檔案。

## <a name="host-a-custom-uwp-control"></a>裝載自訂 UWP 控制項

在C++ Win32 應用程式中裝載自訂 UWP 控制項的程式（您自行定義的控制項或協力廠商所提供的控制項），會從[裝載標準控制項](#host-a-standard-uwp-control)的相同步驟開始。 不過，裝載自訂 UWP 控制項需要額外的專案和步驟。

若要裝載自訂 UWP 控制項，您需要下列專案和元件：

* **應用程式的專案和原始程式碼**。 請確定您已設定您的專案，以符合裝載 XAML 孤島的[必要條件](#prerequisites)。

* **自訂 UWP 控制項**。 您需要裝載的自訂 UWP 控制項的原始程式碼，才能使用您的應用程式進行編譯。 自訂控制項通常會定義在 UWP 類別庫專案中，而您會在與C++ Win32 專案相同的方案中參考它。

* **定義 XamlApplication 物件的 UWP 應用程式專案**。 您C++的 Win32 專案必須能夠存取 Windows 社區工具組所提供之 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 類別的實例。 這個型別會作為根中繼資料提供者，以便在應用程式目前目錄的元件中載入自訂 UWP XAML 類型的中繼資料。 建議的做法是將**空白應用程式（通用 Windows）** 專案新增至與您C++的 Win32 專案相同的方案，並在此專案中修改預設的 `App` 類別。
  > [!NOTE]
  > 您的方案只能包含一個定義 `XamlApplication` 物件的專案。 應用程式中的所有自訂 UWP 控制項都會共用相同的 `XamlApplication` 物件。 定義 `XamlApplication` 物件的專案必須包含所有其他 UWP 程式庫的參考，以及在 XAML 島中主控 UWP 控制項的專案。

若要在C++ Win32 應用程式中裝載自訂 UWP 控制項，請遵循下列一般步驟。

1. 在包含C++ Win32 桌面應用程式專案的方案中，新增**空白應用程式（通用 Windows）** 專案，並依照[本節](host-custom-control-with-xaml-islands.md#create-a-xamlapplication-object-in-a-uwp-app-project)中相關 WPF 逐步解說中的詳細指示來設定它。

2. 在相同的方案中，加入包含自訂 UWP XAML 控制項原始程式碼的專案（這通常是 UWP 類別庫專案），並建立專案。

3. 在 UWP 應用程式專案中，加入 UWP 類別庫專案的參考。

4. 在您C++的 Win32 專案中，加入 uwp 應用程式專案和方案中 uwp 類別庫專案的參考。

5. 請遵循[使用 xaml 裝載 API 來裝載 UWP 控制項](#use-the-xaml-hosting-api-to-host-a-uwp-control)一節中所述的程式，將自訂控制項裝載于應用程式中的 XAML 島。 指派自訂控制項的實例，以裝載至程式碼中**DesktopWindowXamlSource**物件的[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)屬性。

如需C++ Win32 應用程式的完整範例，請參閱[使用自訂控制項的 XAML 島](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的下列專案範例：

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl)：此專案會執行名為 `MyUserControl` 的自訂 UWP XAML 控制項，其中包含文字方塊、數個按鈕和一個下拉式方塊。
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp)：這是具有上述變更的 UWP 應用程式專案。
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp)：這是在C++ XAML 島中裝載自訂 UWP XAML 控制項的 Win32 應用程式專案。

## <a name="handle-keyboard-layout-and-dpi"></a>處理鍵盤、版面配置和 DPI

下列各節提供有關如何處理鍵盤和版面配置案例之程式碼範例的指引和連結。

* [鍵盤輸入](#keyboard-input)
* [鍵盤焦點導覽](#keyboard-focus-navigation)
* [處理版面配置變更](#handle-layout-changes)
* [處理 DPI 變更](#handle-dpi-changes)

### <a name="keyboard-input"></a>鍵盤輸入

若要適當地處理每個 XAML 島的鍵盤輸入，您的應用程式必須將所有的 Windows 訊息傳遞至 UWP XAML 架構，才能正確處理特定訊息。 若要這樣做，您可以在應用程式中存取訊息迴圈的某個位置，將每個 XAML 島的**DesktopWindowXamlSource**物件轉換成**IDesktopWindowXamlSourceNative2** COM 介面。 然後，呼叫此介面的**PreTranslateMessage**方法，並傳入目前的訊息。

  * Win32：：應用程式可以直接在其主要訊息迴圈中呼叫**PreTranslateMessage** 。 **C++** 如需範例，請參閱[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6)檔案。

  * **WPF：** 應用程式可以從[ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage)事件的事件處理常式呼叫**PreTranslateMessage** 。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177)檔案。

  * **Windows Forms：** 應用程式可以從[system.windows.forms.control.preprocessmessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage)方法的覆寫呼叫**PreTranslateMessage** 。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100)檔案。

### <a name="keyboard-focus-navigation"></a>鍵盤焦點導覽

當使用者使用鍵盤導覽應用程式中的 UI 專案時（例如，按**tab**或方向/方向鍵），您將需要以程式設計方式將焦點移入和移出**DesktopWindowXamlSource**物件。 當使用者的鍵盤導覽到達**DesktopWindowXamlSource**時，請將焦點移至 ui 導覽順序中的第一個 node.js 物件，然後在使用者逐一查看專案時繼續將焦點移**至下列的** [node.js 物件，](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)然後將焦點移出**DesktopWindowXamlSource**和父 UI 元素。）。  

UWP XAML 裝載 API 提供數種類型和成員，可協助您完成這些工作。

* 當鍵盤導覽進入您的**DesktopWindowXamlSource**時，會引發[GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)事件。 處理這個事件，並使用[NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)方法，以程式設計方式將焦點移至第一個裝載的**Windows.** node.js。

* 當使用者在**DesktopWindowXamlSource**中的最後一個可設定焦點元素上，並按下**Tab**鍵或方向鍵時，就會引發[TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)事件。 處理這個事件，並以程式設計方式將焦點移至主應用程式中的下一個可設定元素。 例如，在**DesktopWindowXamlSource**裝載于[HWNDHOST](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)中的 WPF 應用程式中，您可以使用[system.windows.frameworkelement.movefocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)方法，將焦點轉移到主應用程式中的下一個可設定元素。

如需示範如何在運作中範例應用程式的內容中執行這項操作的範例，請參閱下列程式碼檔案：

  * /Win32：請參閱[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)檔案。 **C++**

  * **WPF：** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)檔案。  

  * **Windows Forms：** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)檔案。

### <a name="handle-layout-changes"></a>處理版面配置變更

當使用者變更父系 UI 元素的大小時，您必須處理任何必要的版面配置變更，以確保您的 UWP 控制項如預期般顯示。 以下是一些需要考慮的重要案例。

* 在C++ Win32 應用程式中，當您的應用程式處理 WM_SIZE 訊息時，它可以使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函式重新置放裝載的 XAML 島。 如需範例，請參閱[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)中的[SampleApp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191)程式碼檔案。

* 當父 UI 元素需要取得所需之矩形區域的大小，以符合您在**DesktopWindowXamlSource**上**裝載的 node.js**時，請呼叫**windows. ui**的[Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)方法。 例如：

    * 在 WPF 應用程式中，您可以從裝載**DesktopWindowXamlSource**之[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)的[MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)方法來執行此動作。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

    * 在 Windows Forms 應用程式中，您可以從裝載**DesktopWindowXamlSource**之[控制項](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[getpreferredsize 所](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)方法來執行此動作。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

* 當父 UI 元素的大小變更時，請呼叫您在**DesktopWindowXamlSource**上裝載之根**Windows.** node.js 的[排列](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)方法。 例如：

    * 在 WPF 應用程式中，您可以從裝載**DesktopWindowXamlSource**之[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)物件的[ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)方法來執行此動作。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

    * 在 Windows Forms 應用程式中，您可以從裝載**DesktopWindowXamlSource**之[控制項](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)事件處理常式來執行此動作。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

### <a name="handle-dpi-changes"></a>處理 DPI 變更

UWP XAML 架構會自動處理所裝載 UWP 控制項的 DPI 變更（例如，當使用者在具有不同螢幕 DPI 的監視器之間拖曳視窗時）。 為了獲得最佳體驗，我們建議您將 Windows Forms、WPF 或C++ Win32 應用程式設定為每個監視器 DPI 感知。

若要將應用程式設定為每個監視器 DPI 感知，請將[並存組件資訊清單](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)新增至您的專案，並將 **\<DPIAwareness\>** 元素設定為**PerMonitorV2**。 如需此值的詳細資訊，請參閱[DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)的描述。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="troubleshooting"></a>[疑難排解]

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>在 UWP 應用程式中使用 UWP XAML 裝載 API 時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到**COMException** ，並顯示下列訊息：「無法啟動 DesktopWindowXamlSource。 此類型不能用在 UWP 應用程式中。」 或「無法啟用 WindowsXamlManager。 此類型不能用在 UWP 應用程式中。」 | 此錯誤表示您正嘗試使用 uwp XAML 裝載 API （具體而言，您嘗試在 UWP 應用程式中具現化[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)或[WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)類型）。 UWP XAML 裝載 API 僅適用于非 UWP 桌面應用程式，例如 WPF、Windows Forms 和C++ Win32 應用程式。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>嘗試使用 WindowsXamlManager 或 DesktopWindowXamlSource 類型時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到例外狀況，並顯示下列訊息：「WindowsXamlManager 和 DesktopWindowXamlSource 支援以 Windows 版本10.0.18226.0 和更新版本為目標的應用程式。 請檢查應用程式資訊清單或套件資訊清單，並確定 MaxTestedVersion 屬性已更新。」 | 此錯誤表示您的應用程式嘗試在 UWP XAML 裝載 API 中使用**WindowsXamlManager**或**DesktopWindowXamlSource**類型，但 OS 無法判斷應用程式是否已建立成以 Windows 10 1903 版或更新版本為目標。 UWP XAML 裝載 API 最初是在舊版 Windows 10 中引進為預覽版本，但只有從 Windows 10 版本1903開始才支援。</p></p>若要解決此問題，請建立應用程式的 MSIX 套件，並從封裝執行它，或在您的專案中安裝[Microsoft. 工具](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK)組. SDK NuGet 套件。 如需詳細資訊，請參閱[本節](#configure-your-project-for-app-deployment)。 |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>附加至不同執行緒上的視窗時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到**COMException** ，並顯示下列訊息：「AttachToWindow 方法失敗，因為指定的 HWND 是在不同的執行緒上建立的。」 | 此錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative：： AttachToWindow**方法，並將在不同執行緒上建立之視窗的 HWND 傳遞給它。 您必須將在與您呼叫方法的程式碼相同的執行緒上建立的視窗 HWND，傳遞給這個方法。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>附加至另一個最上層視窗的視窗時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到**COMException** ，並顯示下列訊息：「AttachToWindow 方法失敗，因為指定的 hwnd 是從另一個最上層視窗遞減，而不是先前在同一個執行緒上傳遞至 ATTACHTOWINDOW 的 hwnd。」 | 此錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative：： AttachToWindow**方法，並將視窗的 HWND 傳遞至與您在同一個執行緒上先前呼叫這個方法所指定的視窗相比，從另一個最上層視窗遞減。</p></p>當您的應用程式在特定執行緒上呼叫**AttachToWindow**之後，相同執行緒上的所有其他[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)物件都只能附加至 windows，而這些子系是在第一次呼叫**AttachToWindow**時所傳遞的同一個最上層視窗的子系。 當特定執行緒的所有**DesktopWindowXamlSource**物件都關閉時，下一個**DesktopWindowXamlSource**就可以自由附加到任何視窗。</p></p>若要解決此問題，請關閉系結至這個執行緒上其他最上層視窗的所有**DesktopWindowXamlSource**物件，或為此**DesktopWindowXamlSource**建立新的執行緒。 |

## <a name="related-topics"></a>相關主題

* [桌面應用程式中的 UWP 控制項](xaml-islands.md)
* [C++Win32 XAML 群島範例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
