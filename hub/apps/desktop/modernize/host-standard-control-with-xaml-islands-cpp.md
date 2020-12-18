---
description: 此文章示範如何使用 XAML 裝載 API，在 C++ Win32 應用程式中裝載標準 WinRT XAML 控制項。
title: 使用 XAML Islands 在 C++ Win32 應用程式中裝載標準 WinRT XAML 控制項
ms.date: 10/02/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, xaml islands, 包裝的控制項, 標準控制項
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ccd5efd5270ed12d17992f53b3c9ee50feddec4b
ms.sourcegitcommit: 6b64741cba279ac17f23f07baaf4a92a2696e8e1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97502878"
---
# <a name="host-a-standard-winrt-xaml-control-in-a-c-win32-app"></a>在 C++ Win32 應用程式中裝載標準 WinRT XAML 控制項

此文章示範如何使用 [UWP XAML 裝載 API](using-the-xaml-hosting-api.md)，在新的 C++ Win32 應用程式中裝載標準 WinRT XAML 控制項 (也就是 Windows SDK 所提供的控制項)。 此程式碼會以[簡單的 XAML Island 範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)為基礎，而此節將討論程式碼中一些最重要的部分。 如果有現有的 C++ Win32 應用程式專案，則可針對您的專案調整這些步驟和程式碼範例。

> [!NOTE]
> 此文章示範的案例不支援直接編輯應用程式中裝載之 WinRT XAML 控制項的 XAML 標記。 此案例限制您只能透過程式碼，修改裝載控制項的外觀和行為。 如需可讓您在裝載 WinRT XAML 控制項時直接編輯 XAML 標記的指示，請參閱[在 C++ Win32 應用程式中裝載自訂 WinRT XAML 控制項](host-custom-control-with-xaml-islands-cpp.md)。

## <a name="create-a-desktop-application-project"></a>建立傳統型應用程式專案

1. 在已安裝 Windows 10 1903 版 SDK (10.0.18362 版) 或更新版本的 Visual Studio 2019 中，建立新的 **Windows 傳統型應用程式** 專案，並將其命名為 **MyDesktopWin32App**。 此專案類型可在 **C++** 、**Windows** 和 **傳統型** 專案篩選中取得。

2. 在 [方案總管]  中，以滑鼠右鍵按一下解決方案節點、按一下 [重定解決方案目標]  、選取 [10.0.18362.0]  或更新的 SDK 版本，然後按一下 [確定]  。

3. 安裝 [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) \(英文\) NuGet 套件，以在您的專案中啟用對 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) 的支援：

    1. 以滑鼠右鍵按一下 [方案總管]  中的專案，然後選擇 [管理 NuGet 套件]  。
    2. 選取 [瀏覽]  索引標籤、搜尋 [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) \(英文\) 套件，然後安裝此套件的最新版本。

    > [!NOTE]
    > 針對新專案，您也可以安裝 [C++/WinRT Visual Studio 延伸模組 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) \(英文\)，並使用該延伸模組隨附的其中一個 C++/WinRT 專案範本。 如需詳細資料，請參閱[這篇文章](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

4. 在 [NuGet 套件管理員] 的 [瀏覽] 索引標籤上，搜尋 [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 套件，然後安裝此套件的最新穩定版本。 此套件提供數個組建和執行階段資產，讓 XAML Islands 可在您的應用程式中運作。

5. 在您的[應用程式資訊清單](/windows/desktop/SbsCs/application-manifests) \(英文\) 中設定 `maxVersionTested` 值，以指定您的應用程式與 Windows 10 1903 版或更新版本相容。

    1. 如果您的專案中還沒有應用程式資訊清單，請將新的 XML 檔案新增至您的專案並命名為 **app.manifest**。
    2. 在您的應用程式資訊清單中，包含下列範例所示的 **compatibility** 元素及子元素。 以您要設為目標的 Windows 10 版本號碼 (必須是 10.0.18362 或更新版本) 來取代 **maxVersionTested** 元素的 **Id** 屬性。

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

6. 新增 Windows 執行階段中繼資料的參考：
   1. 在 [方案總管] 中，以滑鼠右鍵按一下專案的 [參考] 節點，然後選取 [新增參考]。
   2. 按一下頁面底部的 [瀏覽] 按鈕，然後瀏覽至 SDK 安裝路徑中的 UnionMetadata 資料夾。 SDK 預設會安裝到 `C:\Program Files (x86)\Windows Kits\10\UnionMetadata`。 
   3. 然後，選取以您要瞄準的 Windows 版本 (例如 10.0.18362.0) 所命名的資料夾，然後在該資料夾內挑選 `Windows.winmd` 檔案。
   4. 按一下 [確定] 以關閉 [新增參考] 對話方塊。

## <a name="use-the-xaml-hosting-api-to-host-a-winrt-xaml-control"></a>使用 XAML 裝載 API 來裝載 WinRT XAML 控制項

使用 XAML 裝載 API 來裝載 WinRT XAML 控制項的基本程序會遵循下列一般步驟：

1. 針對目前執行緒來將 UWP XAML 架構初始化，然後您的應用程式才會建立任何其將裝載的 [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) \(英文\) 物件。 根據您打算建立將裝載控制項之 [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) \(英文\) 物件的時機而定，有數種方式可執行此操作。

    * 如果您的應用程式在建立任何其將裝載的 **Windows.UI.Xaml.UIElement** 物件之前先建立 **DesktopWindowXamlSource** 物件，則會在您將 **DesktopWindowXamlSource** 物件具現化時，將此架構初始化。 在此案例中，您不需要新增自己的任何程式碼來將架構初始化。

    * 不過，如果您的應用程式在建立將裝載 **Windows.UI.Xaml.UIElement** 物件的 **DesktopWindowXamlSource** 物件之前先建立了前者物件，則您的應用程式必須先呼叫靜態的 [WindowsXamlManager.InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 方法，明確地將 UWP XAML 架構初始化，然後再將 **Windows.UI.Xaml.UIElement** 物件具現化。 將裝載 **DesktopWindowXamlSource** 的父 UI 元素具現化時，您的應用程式通常應該會呼叫此方法。

    > [!NOTE]
    > 此方法會傳回包含 UWP XAML 架構參考的 [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) \(英文\) 物件。 您可以視需要在指定的執行緒上建立多個 **WindowsXamlManager** 物件。 不過，由於每個物件都持有 UWP XAML 架構的參考，因此，您應該處置物件，以確保最終會釋放 XAML 資源。

2. 建立 [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) \(英文\) 物件，並將其附加至與視窗控制碼相關聯之應用程式中的父 UI 元素。

    若要這樣做，您必須遵循下列步驟：

    1. 建立 **DesktopWindowXamlSource** 物件，並將其轉換成 **IDesktopWindowXamlSourceNative** 或 **IDesktopWindowXamlSourceNative2** COM 介面。
        > [!NOTE]
        > 這些介面均宣告於 Windows SDK 的 **windows.ui.xaml.hosting.desktopwindowxamlsource.h** 標頭檔中。 根據預設，此檔案位於 %programfiles(x86)%\Windows Kits\10\Include\\<組建編號\>\um。

    2. 呼叫 **IDesktopWindowXamlSourceNative** 或 **IDesktopWindowXamlSourceNative2** 介面的 **AttachToWindow** 方法，並傳入應用程式中父 UI 元素的視窗控制碼。

    3. 設定 **DesktopWindowXamlSource** 中所含之內部子視窗的初始大小。 根據預設，這個內部子視窗的寬度和高度均設為 0。 如果您未設定視窗的大小，則您新增至 **DesktopWindowXamlSource** 的任何 WinRT XAML 控制項都不會顯示。 若要存取 **DesktopWindowXamlSource** 中的內部子視窗，請使用 **IDesktopWindowXamlSourceNative** 或 **IDesktopWindowXamlSourceNative2** 介面的 **WindowHandle** 屬性。

3. 最後，將您想要裝載的 **Windows.UI.Xaml.UIElement** 指派給 **DesktopWindowXamlSource** 物件的 [Content](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) \(英文\) 屬性。

下列步驟和程式碼範例示範如何實作上述程序：

1. 在專案的 [來源檔案]  資料夾中，開啟預設的 **MyDesktopWin32App.cpp** 檔案。 刪除檔案的整個內容，並新增下列 `include` 和 `using` 陳述式。 除了標準 C++ 和 UWP 標頭及命名空間之外，這些陳述式還包含數個 XAML Islands 特定的項目。

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

3. 將下列程式碼複製到上一個區段之後。 此程式碼會定義應用程式的 **WinMain** 函式。 此函式會將基本視窗初始化，並使用 XAML 裝載 API，在視窗中裝載簡單的 UWP **TextBlock** 控制項。

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
        // to host WinRT XAML controls in any UI element that is associated with a window handle (HWND).
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

4. 將下列程式碼複製到上一個區段之後。 這段程式碼會定義視窗的[視窗程序](/windows/win32/learnwin32/writing-the-window-procedure) \(英文\)。

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

5. 儲存程式碼檔案，然後建置並執行應用程式。 確認您會在應用程式視窗中看到 UWP **TextBlock** 控制項。
    > [!NOTE]
    > 您可能會看到數個建置警告，包括 `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` 和 `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`。 這些警告是目前工具和 NuGet 套件的已知問題，可加以忽略。

如需示範如何使用 XAML 裝載 API 來裝載 WinRT XAML 控制項的完整範例，請參閱下列程式碼檔案：

* **C++ Win32：** 請參閱 [XAML Islands 程式碼範例存放庫](https://github.com/microsoft/Xaml-Islands-Samples)中的 [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/Contoso/App/XamlBridge.cpp) 檔案。
* **WPF：** 請參閱 Windows 社群工具組中的 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) 和 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) 檔案。  
* **Windows Forms：** 請參閱 Windows 社群工具組中的 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) 和 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) 檔案。

## <a name="package-the-app"></a>封裝應用程式

您可以選擇性地在 [MSIX 套件](/windows/msix)中封裝應用程式以供部署。 MSIX 是 Windows 的新式應用程式封裝技術，以 MSI、.appx、App-V 和 ClickOnce 安裝技術的組合為基礎。

下列指示說明如何使用 Visual Studio 2019 中的 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，將解決方案中的所有元件封裝在 MSIX 套件中。 只有當您想要在 MSIX 套件中封裝應用程式時，才需要執行這些步驟。

> [!NOTE]
> 如果您選擇不要在 [MSIX 套件](/windows/msix)中封裝應用程式以供部署，則執行您應用程式的電腦必須安裝 [Visual C++ 執行階段](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)。

1. 將新的 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)新增到您的方案。 當您建立專案時，同時對 [目標版本]  和 [最低版本]  選取 [Windows 10 版本 1903 (10.0；組建 18362)]  。

2. 在封裝專案中，以滑鼠右鍵按一下 [應用程式]  節點，然後選擇 [新增參考]  。 在專案清單中，選取您解決方案中的 C++/Win32 傳統型應用程式專案，然後按一下 [確定]  。

3. 建置和執行封裝專案。 確認應用程式會如預期般執行並顯示 WinRT XAML 控制項。

## <a name="next-steps"></a>接下來的步驟

此文章中的程式碼範例可讓您開始使用在 C++ Win32 應用程式中裝載標準 WinRT XAML 控制項的基本案例。 下列各節將介紹您的應用程式可能需要支援的其他案例。

### <a name="host-a-custom-winrt-xaml-control"></a>裝載自訂 WinRT XAML 控制項

針對許多案例，您可能需要裝載自訂 UWP XAML 控制項，其中包含數個要共同作業的個別控制項。 在 C++ Win32 應用程式中裝載自訂控制項的程序 (您自行定義的控制項或協力廠商所提供的控制項)，比裝載標準控制項更複雜，而且需要額外的程式碼。

如需完整的逐步解說，請參閱[使用 XAML 裝載 API 在 C++ Win32 應用程式中裝載自訂 WinRT XAML 控制項](host-custom-control-with-xaml-islands-cpp.md)。

### <a name="advanced-scenarios"></a>進階案例

許多裝載 XAML Islands 的傳統型應用程式都必須處理其他案例，才能提供順暢的使用者體驗。 例如，傳統型應用程式可能需要處理 XAML Islands 中的鍵盤輸入、XAML Islands 和其他 UI 元素之間的焦點瀏覽，以及版面配置變更。

如需處理這些案例和相關程式碼範例指標的詳細資訊，請參閱 [C++ Win32 應用程式中適用於 XAML Islands 的進階案例](advanced-scenarios-xaml-islands-cpp.md)。

## <a name="related-topics"></a>相關主題

* [在傳統型應用程式中裝載 UWP XAML 控制項 (XAML Islands)](xaml-islands.md)
* [在 C++ Win32 應用程式中使用 UWP XAML 裝載 API](using-the-xaml-hosting-api.md)
* [在 C++ Win32 應用程式中裝載自訂 WinRT XAML 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [C++ Win32 應用程式中適用於 XAML Islands 的進階案例](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands 程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples) \(英文\)
