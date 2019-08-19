---
description: 本文說明如何在您的桌面應用程式中裝載 UWP XAML UI。
title: 在桌面應用程式中使用 UWP XAML 裝載 API
ms.date: 07/26/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32, xaml 群島
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 14607dc3e3b32f39a840d623c7a887bb8b3687c5
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601527"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>在桌面應用程式中使用 UWP XAML 裝載 API

從 Windows 10 版本1903開始, 非 UWP 桌面應用程式 (包括 WPF、Windows Forms 和C++ Win32 應用程式) 可以使用*UWP XAML 裝載 API* , 將 uwp 控制項裝載在與視窗控制碼 (HWND) 相關聯的任何 UI 專案中。 此 API 可讓非 UWP 桌面應用程式使用僅透過 UWP 控制項提供的最新 Windows 10 UI 功能。 例如, 非 UWP 桌面應用程式可以使用此 API 來裝載使用[流暢設計系統](/windows/uwp/design/fluent-design-system/index)並支援[WINDOWS Ink](/windows/uwp/design/input/pen-and-stylus-interactions)的 UWP 控制項。

UWP XAML 裝載 API 提供更廣泛的控制項集合的基礎, 讓開發人員能夠將流暢的 UI 帶入非 UWP 桌面應用程式。 這項功能稱為「 *XAML 島*」。 如需這項功能的總覽, 請參閱[桌面應用程式中的 UWP 控制項](xaml-islands.md)。

> [!NOTE]
> 如果您有關于 XAML Islands 的意見反應, 請在[Microsoft 工具](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)組的 Win32 存放庫中建立新的問題, 並在該處留下您的意見。 如果您想要私下提交意見反應, 您可以將它傳送XamlIslandsFeedback@microsoft.com給。 您的深入解析和案例對我們而言非常重要。

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>您應該使用 UWP XAML 裝載 API 嗎？

UWP XAML 裝載 API 提供了在桌面應用程式中裝載 UWP 控制項的低層級基礎結構。 某些類型的桌面應用程式可以選擇使用替代、更方便的 Api 來完成此目標。  

* 如果您有C++ Win32 桌面應用程式, 而且想要在應用程式中裝載 uwp 控制項, 則必須使用 UWP XAML 裝載 API。 這些類型的應用程式沒有任何替代方案。

* 針對 WPF 和 Windows Forms 應用程式, 強烈建議您在 Windows 社區工具組中使用[包裝的控制項](xaml-islands.md#wrapped-controls)和[主控制項](xaml-islands.md#host-controls), 而不是直接使用 UWP XAML 裝載 API。 如果您直接使用 UWP XAML 裝載 API, 包括鍵盤導覽和版面配置變更, 這些控制項就會在內部使用 UWP XAML 裝載 API, 並實作為您必須自行處理的所有行為。 不過, 如果您選擇, 您可以直接在這些類型的應用程式中使用 UWP XAML 裝載 API。

本文說明如何直接在您的應用程式中使用 UWP XAML 裝載 API, 而不是 Windows 社區工具組所提供的控制項。

## <a name="prerequisites"></a>先決條件

UWP XAML 裝載 API 具有下列必要條件:

* Windows 10 1903 版 (或更新版本) 和對應的 Windows SDK 組建。
* 依照[這些指示](xaml-islands.md#requirements), 將您的專案設定為使用 Windows 執行階段 api 並啟用 XAML 孤島。

## <a name="architecture-of-xaml-islands"></a>XAML 群島的架構

UWP XAML 裝載 API 包含下列主要 Windows 執行階段類型和 COM 介面:

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)。 此類別代表 UWP XAML 架構。 這個類別會提供單一靜態[**InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法, 以初始化桌面應用程式中目前線程上的 UWP XAML 架構。

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)。 此類別代表您在桌面應用程式中裝載之 UWP XAML 內容的實例。 這個類別最重要的成員是[**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)屬性。 您會將這個屬性指派給您要裝載的[**Windows.** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) node.js。 這個類別也有其他成員, 可將鍵盤焦點導覽傳入和傳出 XAML 島。

* **IDesktopWindowXamlSourceNative**和**IDesktopWindowXamlSourceNative2** COM 介面。 **IDesktopWindowXamlSourceNative**提供**AttachToWindow**方法, 可讓您將應用程式中的 XAML 島附加至父 UI 元素。 **IDesktopWindowXamlSourceNative2**提供**PreTranslateMessage**方法, 可讓 UWP XAML 架構正確地處理特定的 Windows 訊息。
    > [!NOTE]
    > 這些介面是在 Windows SDK 中的**desktopwindowxamlsource**標頭檔中宣告的。 根據預設, 此檔案位於% programfiles (x86)% \ Windows Kits\10\Include\\< 組建編號 \um.\> 在C++ Win32 專案中, 您可以直接參考此標頭檔。 在 WPF 或 Windows Forms 專案中, 您必須使用[**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute)屬性, 在應用程式程式碼中宣告介面。 請確定您的介面宣告完全符合**desktopwindowxamlsource**中的宣告。

下圖說明裝載于桌面應用程式之 XAML 島中的物件階層。

![DesktopWindowXamlSource 架構](images/xaml-islands/xaml-hosting-api-rev2.png)

* 在基底層級是您的應用程式中, 您想要在其中裝載 XAML 島的 UI 元素。 這個 UI 元素必須有視窗控制碼 (HWND)。 您可以在其中裝載 XAML 島的 UI 元素範例包括[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) for WPF 應用程式、 [system.web 控制項](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)(適用于 Windows Forms 應用程式), 以及適用于C++ Win32 的[視窗](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)應用程式.

* 下一個層級是**DesktopWindowXamlSource**物件。 這個物件會提供裝載 XAML 島的基礎結構。 您的程式碼負責建立此物件, 並將它附加至父 UI 元素。

* 當您建立**DesktopWindowXamlSource**時, 此物件會自動建立原生子視窗來裝載 UWP 控制項。 這個原生子視窗大多會從您的程式碼中抽象化, 但如有必要, 您可以存取其控制碼 (HWND)。

* 最後, 頂層是您想要在桌面應用程式中裝載的 UWP 控制項。 這可以是衍生自[**Windows**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 uwp 物件, 包括 Windows SDK 所提供的任何 uwp 控制項, 以及自訂使用者控制項。

## <a name="related-samples"></a>相關範例

您在程式碼中使用 UWP XAML 裝載 API 的方式, 取決於您的應用程式類型、應用程式的設計和其他因素。 為了協助說明如何在完整應用程式的內容中使用此 API, 本文會參考下列範例中的程式碼。

### <a name="c-win32"></a>C++32

[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)。 這個範例示範在未封裝C++的 Win32 應用程式 (也就是未內建于 MSIX 套件中的應用程式) 中裝載 UWP 使用者控制項的完整執行。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows Forms

Windows 社區工具組中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項, 可做為在 WPF 中使用 UWP 裝載 API 和 Windows Forms 應用程式的參考範例。 原始程式碼可在下列位置取得:

  * 若為 WPF 版本的控制項, 請[移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本衍生自[**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。

  * 如需控制項的 Windows Forms 版本, 請[移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows Forms 版本衍生自[**system.web**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

## <a name="host-uwp-xaml-controls"></a>裝載 UWP XAML 控制項

以下是使用 UWP XAML 裝載 API 在您的應用程式中裝載 UWP 控制項的主要步驟。

1. 為目前的執行緒初始化 UWP XAML 架構, 然後您的應用程式會建立它將裝載于[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)中的任何 node.js 物件[ **。** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)

    * 如果您的應用程式在建立任何**DesktopWindowXamlSource**物件之前先建立它, 則當您將**DesktopWindowXamlSource**物件具現化時, 將會為您初始化此架構. 在此案例中, 您不需要加入自己的任何程式碼來初始化架構。

    * 不過, 如果您的應用程式在建立將裝載這些物件的**DesktopWindowXamlSource**物件之前, 先建立了**UIElement**物件, 則您的應用程式必須呼叫靜態[**WindowsXamlManager. InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法, 以明確初始化 UWP XAML 架構, 然後再具現化 node.js 物件。 當裝載**DesktopWindowXamlSource**的父系 UI 元素具現化時, 您的應用程式通常應該呼叫這個方法。

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > 這個方法會傳回[**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)物件, 其中包含 UWP XAML 架構的參考。 您可以視需要在指定的執行緒上建立多個**WindowsXamlManager**物件。 不過, 因為每個物件都持有 UWP XAML 架構的參考, 所以您應該處置物件, 以確保最終會釋放 XAML 資源。

2. 建立[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)物件, 並將它附加至與視窗控制碼相關聯之應用程式中的父 UI 元素。

    若要這樣做, 您必須遵循下列步驟:

    1. 建立**DesktopWindowXamlSource**物件, 並將它轉換成**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2** COM 介面。

    2. 呼叫**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**介面的**AttachToWindow**方法, 並傳入應用程式中父 UI 元素的視窗控制碼。

    3. 設定**DesktopWindowXamlSource**中所包含之內部子視窗的初始大小。 根據預設, 這個內部子視窗會設定為0的寬度和高度。 如果您未設定視窗的大小, 將不會顯示您新增至**DesktopWindowXamlSource**的任何 UWP 控制項。 若要存取**DesktopWindowXamlSource**中的內部子視窗, 請使用**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**介面的**WindowHandle**屬性。 下列範例會使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函數來設定視窗的大小。

    以下是一些示範此流程的程式碼範例。

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI element for your XAML Island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. 將您想要裝載的**DesktopWindowXamlSource**物件的[**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)屬性設定為。 下列範例會將名[**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) ```myGrid```為的 node.js 設定為**內容**屬性。

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

如需在可運作的範例應用程式內容中示範這些工作的完整範例, 請參閱下列程式碼檔案:

  * **C++32**請參閱[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)檔案。

  * **WPF**請參閱 Windows 社區工具組中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs)檔案。  

  * **Windows Forms:** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs)檔案。

## <a name="handle-keyboard-input-and-focus-navigation"></a>處理鍵盤輸入和焦點導覽

您的應用程式必須執行幾項動作, 才能在裝載 XAML 島時適當地處理鍵盤輸入和焦點導覽。

### <a name="keyboard-input"></a>鍵盤輸入

若要適當地處理每個 XAML 島的鍵盤輸入, 您的應用程式必須將所有的 Windows 訊息傳遞至 UWP XAML 架構, 才能正確處理特定訊息。 若要這樣做, 您可以在應用程式中存取訊息迴圈的某個位置, 將每個 XAML 島的**DesktopWindowXamlSource**物件轉換成**IDesktopWindowXamlSourceNative2** COM 介面。 然後, 呼叫此介面的**PreTranslateMessage**方法, 並傳入目前的訊息。

  * C++ Win32 應用程式可以直接在其主要訊息迴圈中呼叫**PreTranslateMessage** 。 如需範例, 請參閱[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6)檔案。

  * WPF 應用程式可以從[**ComponentDispatcher. ThreadFilterMessage**](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2)事件的事件處理常式呼叫**PreTranslateMessage** 。 如需範例, 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177)檔案。

  * Windows Forms 應用程式可以從[**system.windows.forms.control.preprocessmessage**](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2)方法的覆寫呼叫**PreTranslateMessage** 。 如需範例, 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100)檔案。

### <a name="keyboard-focus-navigation"></a>鍵盤焦點導覽

當使用者巡覽應用程式中使用鍵盤的 UI 項目 (比方說，是藉由按下 **索引標籤** 或方向/方向鍵)，您必須以程式設計方式將焦點移入及移出 **DesktopWindowXamlSource** 物件。 當使用者的鍵盤導覽到達**DesktopWindowXamlSource**時, 請將焦點移至 ui 的導覽順序中的第一個 node.js 物件, 然後繼續將焦點移至下列[**視窗**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)當使用者逐一查看元素時, 會將焦點移回**DesktopWindowXamlSource** , 並將焦點移至父 ui 元素中。  

UWP XAML 裝載 API 提供數種類型和成員, 可協助您完成這些工作。

* 當鍵盤導覽進入您的**DesktopWindowXamlSource**時, 會引發[**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)事件。 處理這個事件, 並使用[**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)方法, 以程式設計方式將焦點移至第一個裝載的**Windows.** node.js。

* 當使用者位於最後一個可焦點化項目，在您 **DesktopWindowXamlSource** 且按下 **索引標籤** 索引鍵或方向鍵， [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)就會引發事件。 處理這個事件, 並以程式設計方式將焦點移至主應用程式中的下一個可設定元素。 例如, 在**DesktopWindowXamlSource**裝載于[**HWNDHOST**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)中的 WPF 應用程式中, 您可以使用[**system.windows.frameworkelement.movefocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)方法, 將焦點轉移到主應用程式中的下一個可設定元素。

如需示範如何在運作中範例應用程式的內容中執行這項操作的範例, 請參閱下列程式碼檔案:

  * **WPF**請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)檔案。  

  * **Windows Forms:** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)檔案。

  * **C++/Win32**:請參閱[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge .cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)檔案。

## <a name="handle-layout-changes"></a>處理版面配置變更

當使用者變更父系 UI 元素的大小時, 您必須處理任何必要的版面配置變更, 以確保您的 UWP 控制項如預期般顯示。 以下是一些需要考慮的重要案例。

* 在C++ Win32 應用程式中, 當您的應用程式處理 WM_SIZE 訊息時, 它可以使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函式重新置放裝載的 XAML 島。 如需範例, 請參閱[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)中的[SampleApp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191)程式碼檔案。

* 當父 UI 元素需要取得所需的矩形區域大小, 使其符合您在 **DesktopWindowXamlSource**上裝載的 node.js 時, 請呼叫 Windows. ui 的[**Measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)方法. 例如:

    * 在 WPF 應用程式中, 您可以從裝載**DesktopWindowXamlSource**之[**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)的[**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)方法來執行此動作。 如需範例, 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

    * 在 Windows Forms 應用程式中, 您可以從裝載**DesktopWindowXamlSource**之[**控制項**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[**getpreferredsize 所**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)方法來執行此動作。 如需範例, 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

* 當父 UI 元素的大小變更時, 請呼叫您在**DesktopWindowXamlSource**上裝載之根**Windows.** node.js 的[**排列**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)方法。 例如:

    * 在 WPF 應用程式中, 您可以從裝載**DesktopWindowXamlSource**之[**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)物件的[**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)方法來執行此動作。 如需範例, 請參閱 Windows 社區工具組中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

    * 在 Windows Forms 應用程式中, 您可以從裝載**DesktopWindowXamlSource**之[**控制項**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)事件處理常式來執行此動作。 如需範例, 請參閱 Windows 社區工具組中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

## <a name="handle-dpi-changes"></a>處理 DPI 變更

UWP XAML 架構會自動處理所裝載 UWP 控制項的 DPI 變更 (例如, 當使用者在具有不同螢幕 DPI 的監視器之間拖曳視窗時)。 為了獲得最佳體驗, 我們建議您將 Windows Forms、WPF 或C++ Win32 應用程式設定為每個監視器 DPI 感知。

若要將應用程式設定為每個監視器 DPI 感知, 請將[並存組件資訊清單](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)新增至您的專案, 並```<dpiAwareness>```將元素```PerMonitorV2```設定為。 如需此值的詳細資訊, 請參閱[**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)的描述。

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

## <a name="host-custom-uwp-xaml-controls"></a>裝載自訂 UWP XAML 控制項

請遵循下列一般步驟, 在 XAML 島中裝載自訂 UWP XAML 控制項 (您自行定義的控制項或由協力廠商所提供的控制項)。 您必須擁有自訂控制項的原始程式碼, 才能使用您的應用程式進行編譯。

1. 在您的主應用程式方案中, 整合自訂 UWP XAML 控制項的原始程式碼, 並建立自訂控制項。

2. 將 UWP 應用程式專案新增至您的主應用程式方案, 並將[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 套件新增至這個專案。

3. 在 UWP 應用程式專案中, 修改`App`您的類別, 使其從[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 封裝所公開的**XamlApplication**類別中加以移除,。 這個型別會作為根中繼資料提供者, 以便在應用程式目前目錄的元件中載入自訂 UWP XAML 類型的中繼資料。

4. 在`App`類別的函式中, 呼叫基底**XamlApplication**類別的**Initialize**方法。

5. 在您的主應用程式專案中, 遵循上一[節](#host-uwp-xaml-controls)所述的流程, 將自訂控制項裝載在 XAML 島中。

如需C++ win32 應用程式的範例, 請參閱[ C++ win32 範例](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App)中的下列專案:

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl):此專案會執行名為`MyUserControl`的自訂 UWP XAML 控制項, 其中包含文字方塊、數個按鈕和一個下拉式方塊。
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp):這是一個 UWP 應用程式專案, 其中包含先前步驟中所述的變更。
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp):這是在C++ XAML 島中裝載自訂 UWP XAML 控制項的 Win32 應用程式專案。

如需 WPF 或 Windows Forms 應用程式的詳細指示和範例, 請參閱[這些指示](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)。

## <a name="troubleshooting"></a>疑難排解

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>在 UWP 應用程式中使用 UWP XAML 裝載 API 時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到包含下列訊息的**COMException** :「無法啟用 DesktopWindowXamlSource。 此類型不能用在 UWP 應用程式中。」 或「無法啟用 WindowsXamlManager。 此類型不能用在 UWP 應用程式中。」 | 此錯誤表示您正嘗試使用 uwp XAML 裝載 API (具體而言, 您嘗試在 UWP 應用程式中具現化[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)或[**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)類型)。 UWP XAML 裝載 API 僅適用于非 UWP 桌面應用程式, 例如 WPF、Windows Forms 和C++ Win32 應用程式。 |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>附加至不同執行緒上的視窗時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到包含下列訊息的**COMException** :「AttachToWindow 方法失敗, 因為指定的 HWND 是在不同的執行緒上建立的。」 | 此錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative:: AttachToWindow**方法, 並將在不同執行緒上建立之視窗的 HWND 傳遞給它。 您必須將在與您呼叫方法的程式碼相同的執行緒上建立的視窗 HWND, 傳遞給這個方法。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>附加至另一個最上層視窗的視窗時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到包含下列訊息的**COMException** :「AttachToWindow 方法失敗, 因為指定的 HWND 是從另一個最上層視窗遞減, 而不是先前在同一個執行緒上傳遞給 AttachToWindow 的 HWND。」 | 此錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative:: AttachToWindow**方法, 並將視窗的 HWND 傳遞至與您在先前呼叫此方法時所指定的視窗不同的最上層視窗在同一個執行緒上。</p></p>當您的應用程式在特定執行緒上呼叫**IDesktopWindowXamlSourceNative:: AttachToWindow**之後, 相同執行緒上的所有其他[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)物件都只能附加至屬於相同頂層的子系的視窗第一次呼叫**IDesktopWindowXamlSourceNative:: AttachToWindow**時所傳遞的視窗。 當特定執行緒的所有**DesktopWindowXamlSource**物件都關閉時, 下一個**DesktopWindowXamlSource**就可以自由附加到任何視窗。</p></p>若要解決此問題, 請關閉系結至這個執行緒上其他最上層視窗的所有**DesktopWindowXamlSource**物件, 或為此**DesktopWindowXamlSource**建立新的執行緒。 |

## <a name="related-topics"></a>相關主題

* [桌面應用程式中的 UWP 控制項](xaml-islands.md)
* [C++Win32 XAML 群島範例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
