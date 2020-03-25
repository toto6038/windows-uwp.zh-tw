---
description: 本文示範如何使用 XAML 裝載 API，在C++ Win32 應用程式中裝載標準 UWP 控制項。
title: 使用 XAML 群島在C++ Win32 應用程式中裝載標準 UWP 控制項
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10，uwp，cpp，win32，xaml islands，包裝的控制項，標準控制項
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 08308c7bca3cd7f39b08c836e43d791a3fda048f
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226272"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>在C++ Win32 應用程式中裝載標準 UWP 控制項

本文示範如何使用[UWP XAML 裝載 API](using-the-xaml-hosting-api.md) ，在新C++的 Win32 應用程式中裝載標準 UWP 控制項（也就是 Windows SDK 所如果的控制項）。 程式碼是以簡單的[XAML 島範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)為基礎，本節將討論一些最重要的程式碼部分。 如果您有現有C++的 Win32 應用程式專案，您可以針對您的專案調整這些步驟和程式碼範例。

> [!NOTE]
> 本文所示範的案例不支援直接編輯應用程式中所裝載 UWP 控制項的 XAML 標記。 此案例會限制您透過程式碼修改託管 UWP 控制項的外觀和行為。 如需可讓您在裝載 UWP 控制項時直接編輯 XAML 標記的指示，請參閱[在C++ Win32 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)。

## <a name="create-a-desktop-application-project"></a>建立桌面應用程式專案

1. 在已安裝 Windows 10 版本 1903 SDK （版本10.0.18362）或更新版本的 Visual Studio 2019 中，建立新的**Windows 桌面應用程式**專案，並將其命名為**MyDesktopWin32App**。 []、[ **C++** **Windows**] 和 [**桌面**] 專案篩選器下提供此專案類型。

2. 在**方案總管**中，以滑鼠右鍵按一下方案節點，然後按一下 [重定**目標方案**]，選取**10.0.18362.0**或更新版本的 SDK，然後按一下 **[確定]** 。

3. 安裝[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 套件，以在您的專案中啟用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)的支援：

    1. 在**方案總管**中，以滑鼠右鍵按一下您的專案，然後選擇 [**管理 NuGet 封裝**]。
    2. 選取 [**流覽**] 索引標籤，搜尋[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)套件，然後安裝此套件的最新版本。

    > [!NOTE]
    > 針對新的專案，您也可以安裝[ C++/WinRT Visual Studio 擴充功能（VSIX）](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) ，並使用該C++延伸模組中包含的其中一個/WinRT 專案範本。 如需詳細資料，請參閱[這篇文章](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

4. 安裝[Microsoft 工具](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK)組. SDK NuGet 套件：

    1. 在 [ **NuGet 套件管理員**] 視窗中，確認已選取 [**包含發行**前版本]。
    2. 選取 [**流覽**] 索引標籤，**搜尋 [6.0.0** ] （或更新版本）此套件的套件。 此套件提供數個組建和執行時間資產，讓 XAML 孤島可在您的應用程式中運作。

5. 在[應用程式資訊清單](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)中設定 `maxVersionTested` 值，指定您的應用程式與 Windows 10 1903 版或更新版本相容。

    1. 如果您的專案中還沒有應用程式資訊清單，請將新的 XML 檔案加入您的專案中，並將它命名為**app.config**。
    2. 在您的應用程式資訊清單中，包含**相容性**元素和下列範例中所示的子項目。 將**maxVersionTested**元素的**Id**屬性取代為您的目標 windows 10 版本號碼（必須是 windows 10 1903 版或更新版本）。

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
            <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
                <application>
                    <!-- Windows 10 -->
                    <maxversiontested Id="10.0.18362.0"/>
                    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
                </application>
            </compatibility>
        </assembly>
        ```

## <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>使用 XAML 裝載 API 來裝載 UWP 控制項

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

1. 在專案的 [**來源**檔案] 資料夾中，開啟預設的**MyDesktopWin32App .cpp**檔案。 刪除檔案的整個內容，並加入下列 `include` 和 `using` 語句。 除了標準C++和 UWP 標頭和命名空間之外，這些語句還包含 XAML 島特定的數個專案。

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
  * 請參閱[HelloWindowsDesktop .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp)檔案。
  * 請參閱[XamlBridge .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp)檔案。
* **WPF：** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs)檔案。  
* **Windows Forms：** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs)檔案。

## <a name="package-the-app"></a>封裝應用程式

您可以選擇性地將應用程式封裝在[MSIX 套件](https://docs.microsoft.com/windows/msix)中進行部署。 MSIX 是適用于 Windows 的現代化應用程式封裝技術，它是以 MSI、.appx、App-v 和 ClickOnce 安裝技術的組合為基礎。

下列指示說明如何使用 Visual Studio 2019 中的[Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，在 MSIX 套件中封裝解決方案中的所有元件。 只有當您想要在 MSIX 套件中封裝應用程式時，才需要執行這些步驟。

> [!NOTE]
> 如果您選擇不要在[MSIX 套件](https://docs.microsoft.com/windows/msix)中封裝應用程式以進行部署，執行應用程式的電腦必須安裝[Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) 。

1. 將新的[Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)新增至您的方案。 當您建立專案時，請選取 [ **Windows 10，版本1903（10.0;）]。組建18362）** ，適用于**目標版本**和**最低版本**。

2. 在封裝專案中，以滑鼠右鍵按一下 [**應用程式**] 節點，然後選擇 [**加入參考**]。 在專案清單中，選取方案中C++的/Win32 桌面應用程式專案，然後按一下 **[確定]** 。

3. 建立並執行封裝專案。 確認應用程式會執行，並如預期般顯示 UWP 控制項。

## <a name="next-steps"></a>後續步驟

本文中的程式碼範例可讓您開始使用在C++ Win32 應用程式中裝載標準 UWP 控制項的基本案例。 下列各節將介紹您的應用程式可能需要支援的其他案例。

### <a name="host-a-custom-uwp-control"></a>裝載自訂 UWP 控制項

在許多情況下，您可能需要裝載自訂 UWP XAML 控制項，其中包含數個共同作業的個別控制項。 在C++ Win32 應用程式中裝載自訂 UWP 控制項的程式（您自行定義的控制項或協力廠商所提供的控制項），比裝載標準控制項更為複雜，而且需要額外的程式碼。

如需完整的逐步解說，請參閱[使用 XAML 裝載 API C++在 Win32 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)。

### <a name="advanced-scenarios"></a>進階案例

許多裝載 XAML 孤島的桌面應用程式都必須處理其他案例，才能提供順暢的使用者體驗。 例如，桌面應用程式可能需要處理 XAML 島中的鍵盤輸入、XAML 島和其他 UI 專案之間的焦點導覽，以及版面配置變更。

如需有關處理這些案例和相關程式碼範例指標的詳細資訊，請參閱[Win32 應用程式C++中 XAML Islands 的 Advanced 案例](advanced-scenarios-xaml-islands-cpp.md)。

## <a name="related-topics"></a>相關主題

* [在桌面應用程式中裝載 UWP XAML 控制項（XAML 島）](xaml-islands.md)
* [在C++ Win32 應用程式中使用 UWP XAML 裝載 API](using-the-xaml-hosting-api.md)
* [在C++ Win32 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [Win32 應用程式中C++ XAML 孤島的 Advanced 案例](advanced-scenarios-xaml-islands-cpp.md)
* [XAML 島程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples)
