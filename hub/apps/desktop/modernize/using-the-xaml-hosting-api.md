---
description: 本文說明如何在桌面應用程式中裝載 UWP XAML UI。
title: 使用桌面應用程式中裝載 API UWP XAML
ms.date: 04/16/2019
ms.topic: article
keywords: windows 10、 uwp、 windows form、 wpf、 win32、 xaml 群島
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 545e1e1b220de9edf444ca06c3b21140227e8284
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215145"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>使用桌面應用程式中裝載 API UWP XAML

從 Windows 10 版本 1903年、 非 UWP 桌面應用程式 (包括 WPF、 Windows Form 和C++Win32 應用程式) 可以使用*UWP XAML 裝載 API*來裝載任何相關聯的 UI 項目中的 UWP 控制項視窗控制代碼 (HWND)。 此 API 可讓非 UWP 桌面應用程式使用最新的 Windows 10 UI 功能，只可透過 UWP 控制項。 例如，非 UWP 桌面應用程式可以使用此 API 使用的主機 UWP 控制項[Fluent Design System](/windows/uwp/design/fluent-design-system/index)且支援[Windows Ink](/windows/uwp/design/pen-and-stylus-interactions)。

裝載 API UWP XAML 提供廣泛的控制項，我們會提供可讓開發人員將 Fluent UI 帶到非 UWP 桌面應用程式的基礎。 這項功能稱為*XAML 群島*。 如需這項功能的概觀，請參閱 <<c0> [ 桌面應用程式中的 UWP 控制項](xaml-islands.md)。

> [!NOTE]
> 如果您有意見反應的相關 XAML 群島，建立新的問題，在[Microsoft.Toolkit.Win32 存放庫](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)和那里留下您的意見。 如果您想要私下提交您的意見反應，您可以傳送到XamlIslandsFeedback@microsoft.com。 您的深入解析和案例會對我們非常重要。

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>您應該使用裝載 API UWP XAML？

裝載 API UWP XAML UWP 控制項裝載於桌面應用程式提供的低層級的基礎結構。 某些類型的桌面應用程式可以選擇使用替代的更方便的 Api，以達成此目標。  

* 如果您有C++Win32 桌面應用程式，您想要主機 UWP 控制項在您的應用程式中，您必須使用裝載 API UWP XAML。 沒有這類應用程式的替代方案。

* WPF 和 Windows Forms 應用程式，我們強烈建議您使用[包裝控制項](xaml-islands.md#wrapped-controls)並[主控制項](xaml-islands.md#host-controls)Windows 社群工具組，而不是使用直接裝載 API UWP XAML 中。 這些控制項使用 UWP XAML，內部裝載 API，並實作的所有行為，您必須自行處理，如果您使用裝載 API，包括鍵盤導覽和版面配置變更 UWP XAML。 不過，您可以使用裝載 API，直接在這些類型的應用程式中，如果您選擇 UWP XAML。

本文說明如何使用裝載 API，直接在您的應用程式，而不是 Windows 社群工具組所提供之控制項中的 UWP XAML。

## <a name="prerequisites"></a>先決條件

裝載 API UWP XAML 有下列先決條件：

* Windows 10，1903年 （含） 以後的版本和對應組建的 Windows sdk。
* 將專案設定為使用 Windows 執行階段 Api，並依照啟用 XAML 群島[這些指示](xaml-islands.md#requirements)。

## <a name="architecture-of-xaml-islands"></a>架構的 XAML 群島

裝載 API UWP XAML 包含這些主要的 Windows 執行階段型別和 COM 介面：

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). 此類別代表 UWP XAML 架構。 這個類別會提供單一的靜態[ **InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)初始化 UWP XAML 架構的桌面應用程式中目前的執行緒上的方法。

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)。 此類別代表您要裝載您的桌面應用程式中的 UWP XAML 內容的執行個體。 這個類別的最重要的成員是[**內容**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)屬性。 您指派此屬性為[ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)您想要主控。 這個類別也具有路由鍵盤焦點導覽傳入和傳出 XAML 資料島的其他成員。

* **IDesktopWindowXamlSourceNative**並**IDesktopWindowXamlSourceNative2** COM 介面。 **IDesktopWindowXamlSourceNative**提供**AttachToWindow**方法，用來附加至父 UI 項目應用程式中 XAML 島。 **IDesktopWindowXamlSourceNative2**提供**PreTranslateMessage**方法，可讓 UWP XAML framework，才能正確處理特定 Windows 訊息。
    > [!NOTE]
    > 這些介面中宣告**windows.ui.xaml.hosting.desktopwindowxamlsource.h** Windows SDK 中的標頭檔。 根據預設，這個檔案位於 %programfiles (x86) %\Windows Kits\10\Include\\< 組建編號\>\um。 在C++Win32 專案，您可以直接參考此標頭檔。 在 WPF 或 Windows Form 專案中，您必須宣告中使用您建立您的應用程式程式碼的介面[ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute)屬性。 請確定您的介面宣告完全符合中的宣告**windows.ui.xaml.hosting.desktopwindowxamlsource.h**。

下圖說明在桌面應用程式中裝載 XAML 島物件的階層。

![DesktopWindowXamlSource 架構](images/xaml-islands/xaml-hosting-api-rev2.png)

* 在基本層級是您想要用來裝載 XAML 島應用程式中的 UI 項目。 此 UI 項目必須具有的視窗控制代碼 (HWND)。 您可以在其中裝載 XAML 島的 UI 元素的範例包括[ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) WPF 應用程式，如[ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Windows Forms 應用程式，並[視窗](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)的C++的 Win32 應用程式。

* 在下一步是層級**DesktopWindowXamlSource**物件。 這個物件提供基礎結構裝載 XAML 島。 您的程式碼負責建立此物件，並將它附加至父 UI 項目。

* 當您建立**DesktopWindowXamlSource**，這個物件會自動建立 UWP 控制項裝載的原生的子視窗。 此原生的子視窗離開您的程式碼中，大部分抽離，但您可以存取其控制代碼 (HWND) 如有必要。

* 最後，在最上層是您想要裝載您的桌面應用程式中的 UWP 控制項。 這可以是任何 UWP 物件衍生自[ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括任何由 Windows SDK 所提供的 UWP 控制項，以及自訂使用者控制項。

## <a name="related-samples"></a>相關範例

使用裝載 API，在您的程式碼中的 UWP XAML 的方式取決於您的應用程式類型，您的應用程式，以及其他因素的設計。 為了說明如何使用此 API 的完整應用程式內容中，這篇文章主要程式碼從下列的範例。

### <a name="c-win32"></a>C++ Win32

[C++Win32 範例](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)。 這個範例會示範 UWP 使用者控制項裝載中未封裝的完整實作C++Win32 應用程式 （也就是應用程式不會內建於 MSIX 封裝）。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows Form

[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) Windows 社群工具組中的控制項可當做使用 UWP WPF 和 Windows Forms 應用程式中裝載 API 的參考範例。 原始程式碼位於下列位置：

  * 控制項的 WPF 版本[前往此處](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版衍生自[ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。

  * Windows Form 控制項，新版[前往此處](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows Form 版本衍生自[ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

## <a name="host-uwp-xaml-controls"></a>主機 UWP XAML 控制項

以下是使用裝載 API UWP XAML 裝載應用程式中的 UWP 控制項的主要步驟。

1. 初始化目前執行緒的 UWP XAML 架構，您的應用程式建立的任何之前[ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)將會裝載中的物件[ **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)。

    * 如果您的應用程式建立**DesktopWindowXamlSource**物件，然後再建立的任何**Windows.UI.Xaml.UIElement**物件，當您具現化時，此架構會為您初始化**DesktopWindowXamlSource**物件。 在此案例中，您不需要加入您自己來初始化架構的任何程式碼。

    * 不過，如果您的應用程式建立**Windows.UI.Xaml.UIElement**物件，然後再建立**DesktopWindowXamlSource**裝載它們的物件應用程式必須呼叫靜態[**WindowsXamlManager.InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法來明確初始化之前的 UWP XAML framework **Windows.UI.Xaml.UIElement**物件具現化。 您的應用程式通常應該呼叫這個方法時，裝載父系的 UI 項目**DesktopWindowXamlSource**具現化。

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > 這個方法會傳回[ **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)物件，包含 UWP XAML 架構的參考。 您可以建立多個**WindowsXamlManager**物件所指定的執行緒上。 不過，因為每個物件保留的 UWP XAML 架構的參考，您應該處置的物件，以確保最終發行 XAML 資源。

2. 建立[ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)物件，並將它附加到視窗控制代碼相關聯的應用程式中的父代 UI 項目。

    若要這樣做，您必須請遵循下列步驟：

    1. 建立**DesktopWindowXamlSource**物件，並將它轉換成**IDesktopWindowXamlSourceNative**或是**IDesktopWindowXamlSourceNative2** COM 介面。

    2. 呼叫**AttachToWindow**方法**IDesktopWindowXamlSourceNative**或是**IDesktopWindowXamlSourceNative2**介面，並傳入的視窗控制代碼父應用程式中的 UI 項目。

    3. 設定內部的子視窗中所包含的初始大小**DesktopWindowXamlSource**。 根據預設，此內部的子視窗設定寬度和高度都為 0。 如果您未設定 視窗中，您將加入任何 UWP 控制項的大小**DesktopWindowXamlSource**將不會顯示。 若要存取在內部的子視窗**DesktopWindowXamlSource**，使用**WindowHandle**屬性**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**介面。 下列範例會使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函式來設定視窗的大小。

    以下是一些程式碼範例，示範此程序。

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

3. 設定**Windows.UI.Xaml.UIElement**您想要以主控[**內容**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)屬性您**DesktopWindowXamlSource**物件。 下列範例會設定[ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid)名為```myGrid```來**內容**屬性。

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

如需完整範例，示範這些工作的工作範例應用程式內容中，請參閱下列程式碼檔案：

  * **C++Win32:** 請參閱[XamlBridge.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/XamlBridge.cpp)中的檔案[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)。

  * **WPF:** 請參閱[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)並[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) Windows 社群工具組中的檔案。  

  * **Windows Form:** 請參閱[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)並[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) Windows 社群工具組中的檔案。

## <a name="handle-keyboard-input-and-focus-navigation"></a>處理鍵盤輸入和焦點的巡覽

有幾件事，您的應用程式需要時它所主控的 XAML 群島，適當地處理鍵盤輸入和焦點導覽。

### <a name="keyboard-input"></a>鍵盤輸入

為了適當處理鍵盤輸入的每個 XAML 島，您的應用程式必須通過所有的 Windows 訊息給 UWP XAML 架構使其可以正確處理特定訊息。 若要這樣做，請在您的應用程式可存取的訊息迴圈中的某個地方轉換**DesktopWindowXamlSource**物件到每個 XAML 島**IDesktopWindowXamlSourceNative2** COM 介面。 然後，呼叫**PreTranslateMessage**方法，此介面，並傳入目前的訊息。

  * C++ Win32 應用程式可以呼叫**PreTranslateMessage**直接在其主要訊息迴圈中。 如需範例，請參閱[SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L61)中的程式碼檔案[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)。

  * WPF 應用程式可以呼叫**PreTranslateMessage**從事件處理常式，如[ **ComponentDispatcher.ThreadFilterMessage** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2)事件。 如需範例，請參閱[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) Windows 社群工具組中的檔案。

  * 在 Windows Forms 應用程式可以呼叫**PreTranslateMessage**從覆寫[ **Control.PreprocessMessage** ](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2)方法。 如需範例，請參閱[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) Windows 社群工具組中的檔案。

### <a name="keyboard-focus-navigation"></a>鍵盤焦點導覽

當使用者巡覽應用程式中使用鍵盤的 UI 項目 (比方說，是藉由按下 **索引標籤** 或方向/方向鍵)，您必須以程式設計方式將焦點移入及移出 **DesktopWindowXamlSource** 物件。 當使用者的鍵盤瀏覽到達**DesktopWindowXamlSource**，將焦點移到第一個[ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)巡覽順序中的物件針對您的 UI 中，繼續將焦點移至下列**Windows.UI.Xaml.UIElement**物件做為使用者不斷循環的項目，然後再移動焦點設回的**DesktopWindowXamlSource**到父 UI 項目。  

裝載 API UWP XAML 提供幾個型別和成員，可協助您完成這些工作。

* 當輸入鍵盤瀏覽您**DesktopWindowXamlSource**，則[ **GotFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)就會引發事件。 處理此事件，並以程式設計方式將焦點移至第一個裝載**Windows.UI.Xaml.UIElement**利用[ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)方法。

* 當使用者位於最後一個可焦點化項目，在您 **DesktopWindowXamlSource** 且按下 **索引標籤** 索引鍵或方向鍵， [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)就會引發事件。 處理此事件，並以程式設計方式將焦點移至主應用程式中的下一個可焦點化項目。 例如，在 WPF 應用程式所在**DesktopWindowXamlSource**裝載於[ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)，您可以使用[ **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)方法，以將焦點轉移到主應用程式中的下一個可焦點化項目。

如需示範如何執行此動作之工作的範例應用程式內容中的範例，請參閱下列程式碼檔案：

  * **WPF:** 請參閱[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) Windows 社群工具組中的檔案。  

  * **Windows Form:** 請參閱[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) Windows 社群工具組中的檔案。

## <a name="handle-layout-changes"></a>處理版面配置變更

當使用者變更父代 UI 元素的大小時，您必須處理任何所需的版面配置變更，請確定您的 UWP 控制項如預期般顯示。 以下是一些要考慮的重要案例。

* 在C++Win32 應用程式，當您的應用程式處理 WM_SIZE 訊息，它可以使用重新裝載的 XAML 島[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函式。 如需範例，請參閱[SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191)中的程式碼檔案[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)。

* 當父 UI 項目需要取得符合所需的矩形區域的大小**Windows.UI.Xaml.UIElement** ，您會在裝載**DesktopWindowXamlSource**，呼叫[**量值**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)方法**Windows.UI.Xaml.UIElement**。 例如: 

    * 在 WPF 應用程式可能會執行從[ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)方法[ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)裝載**DesktopWindowXamlSource**。 如需範例，請參閱[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 社群工具組中的檔案。

    * 在 Windows Forms 應用程式可能會執行從[ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)方法[**控制**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)裝載**DesktopWindowXamlSource**。 如需範例，請參閱[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 社群工具組中的檔案。

* 當父 UI 項目的大小變更時，呼叫[**排列**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)方法的根**Windows.UI.Xaml.UIElement**您裝載在**DesktopWindowXamlSource**。 例如: 

    * 在 WPF 應用程式可能會執行從[ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)方法[ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)物件裝載**DesktopWindowXamlSource**。 如需範例，請參閱[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 社群工具組中的檔案。

    * 在 Windows Forms 應用程式中您可能會從這樣的處理常式[ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)事件[**控制**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)裝載**DesktopWindowXamlSource**。 如需範例，請參閱[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 社群工具組中的檔案。

## <a name="handle-dpi-changes"></a>處理 DPI 變更

UWP XAML 架構自動處理裝載之 UWP 控制項的 DPI 變更，（例如，當使用者使用不同的螢幕 DPI 的監視器之間拖曳視窗）。 以獲得最佳體驗，我們建議您的 Windows Form、 WPF 中，或C++Win32 應用程式設定為個別監視器 DPI 感知。

若要設定您要個別監視器 DPI 感知的應用程式，新增[並排顯示組件資訊清單](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)為您的專案，將```<dpiAwareness>```項目```PerMonitorV2```。 如需有關此值的詳細資訊，請參閱的描述[ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)。

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

## <a name="host-custom-uwp-xaml-controls"></a>主機自訂 UWP XAML 控制項

> [!IMPORTANT]
> 若要裝載自訂的 UWP XAML 控制項，您必須原始程式碼控制項讓您可以針對它在您的應用程式中編譯。

如果您想要裝載自訂的 UWP XAML 控制項 （控制項自行定義或所提供的控制項，第 3 方），您必須執行下列額外的工作，除了中所述的程序[上一節](#host-uwp-xaml-controls)。

1. 定義自訂類型衍生自[ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application)也會實作[ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider)。 此類型會做為載入目前的目錄中的組件中自訂的 UWP XAML 類型，您的應用程式的中繼資料的根中繼資料提供者。

2. 呼叫[ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype)根中繼資料提供者時指派 UWP XAML 控制項的型別名稱的方法 （這可以在執行階段，指派給程式碼中，或您可能會選擇這麼做是在 Visual Studio 的 [屬性] 視窗中指派）。

    如需範例，請參閱下列程式碼檔案：
      * **C++Win32:** 請參閱[XamlApplication.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup/XamlApplication.cpp)中的程式碼檔案[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)。

      * **WPF 和 Windows Forms**:請參閱[XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs)並[UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) Windows 社群工具組中的檔案的程式碼。 這些檔案會共用實作的一部分**WindowsXamlHost**適用於 WPF 和 Windows Form，說明如何使用裝載 API，這些類型的應用程式中的 UWP XAML 的類別。

3. 將自訂的 UWP XAML 控制項的原始碼整合到應用程式主機方案、 建置自訂控制項，並在您的應用程式中使用它。 針對 WPF 或 Windows Form 應用程式的指示，請參閱[這些指示](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)。 如需範例，如C++Win32 應用程式，請參閱[Microsoft.UI.Xaml.Markup](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup)並[MyApp](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/MyApp)中的專案[ C++ Win32 範例](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)。

## <a name="troubleshooting"></a>疑難排解

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>使用 UWP XAML 裝載 API 的 UWP 應用程式中的錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式接收**COMException**並出現下列訊息：「 無法啟動 DesktopWindowXamlSource。 此類型不能在 UWP 應用程式。 」 或者 「 無法啟動 WindowsXamlManager。 此類型不能在 UWP 應用程式。 」 | 此錯誤表示您嘗試使用裝載 API UWP XAML (具體而言，您嘗試具現化[ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)或是[ **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)類型) 的 UWP 應用程式中。 裝載 API UWP XAML 僅適用於在非 UWP 傳統型應用程式，例如 WPF、 Windows Form 和C++Win32 應用程式。 |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>附加至不同的執行緒上的視窗錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式接收**COMException**並出現下列訊息：「 AttachToWindow 方法失敗，因為指定的 HWND 建立在不同的執行緒上。 」 | 此錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative::AttachToWindow**方法並傳遞它在不同的執行緒建立視窗的 HWND。 您必須傳遞此方法在呼叫方法的程式碼相同的執行緒建立視窗的 HWND。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>附加至不同的最上層視窗的視窗錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式接收**COMException**並出現下列訊息：「 AttachToWindow 方法失敗，因為指定的 HWND，則在相同執行緒先前傳遞至 AttachToWindow HWND 以外的其他最上層視窗的子系。 」 | 此錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative::AttachToWindow**方法並傳遞它的視窗中所指定以外的其他最上層視窗的子系的視窗 HWND先前呼叫這個方法的相同執行緒上。</p></p>在您的應用程式呼叫後**IDesktopWindowXamlSourceNative::AttachToWindow**在特定的執行緒上所有其他[ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)上相同的物件執行緒只能附加至傳入的第一個呼叫相同最上層視窗的下階視窗**IDesktopWindowXamlSourceNative::AttachToWindow**。 當所有**DesktopWindowXamlSource**物件會關閉特定的執行緒，下一步 **DesktopWindowXamlSource**就免費將再次附加至任何視窗。</p></p>若要解決此問題，請關閉所有**DesktopWindowXamlSource**在此執行緒中，繫結至其他最上層的視窗，或建立新的執行緒，這個物件**DesktopWindowXamlSource**。 |

## <a name="related-topics"></a>相關主題

* [桌面應用程式中的 UWP 控制項](xaml-islands.md)
* [C++Win32 XAML 群島範例](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)
