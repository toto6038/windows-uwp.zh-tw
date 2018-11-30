---
description: 本文章說明如何在您的傳統型應用程式中裝載 UWP XAML UI。
title: 使用 UWP XAML 中的傳統型應用程式裝載 API
ms.date: 11/27/2018
ms.topic: article
keywords: windows 10、 uwp、 windows forms、 wpf、 win32
ms.localizationpriority: medium
ms.openlocfilehash: df6c47fd93c3f42721fd072d6406a2d32f7889db
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8212579"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>使用 UWP XAML 中的傳統型應用程式裝載 API

> [!NOTE]
> 目前提供做為開發人員預覽 UWP XAML 裝載 API 與 XAML 群島。 雖然我們鼓勵您嘗試它們在您自己的原型程式碼現在，我們不建議您使用它們在實際執行程式碼中這一次。 這些功能將會繼續成熟和穩定在未來的 Windows 版本。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。
>
> 如果您有關於裝載 API 與 XAML 群島 XAML 的意見反應，傳送意見反應給XamlIslandsFeedback@microsoft.com。 您的見解和案例是非常重要給我們。

在 Windows 10 Insider Preview SDK 中開始建置 17709、 非 UWP 傳統型應用程式 （包括 WPF、 Windows Forms 和 c + + Win32 應用程式） 可以使用*UWP XAML 裝載 API*來裝載 UWP 控制項中任何的視窗控制代碼 （相關聯的 UI 元素HWND)。 這個 API 可讓非 UWP 傳統型應用程式使用的最新的 Windows 10 UI 功能，只可透過 UWP 控制項。 例如，非 UWP 傳統型應用程式可以使用此 API 來使用[Fluent 設計系統](../design/fluent-design-system/index.md)和支援[Windows Ink](../design/input/pen-and-stylus-interactions.md)裝載 UWP 控制項。

UWP XAML 裝載 API 提供更為廣泛的控制項，我們提供，讓開發人員將 Fluent UI 移到非 UWP 傳統型應用程式的基礎。 這個案例中是有時也稱為*XAML 群島*。 如需關於此開發人員案例的詳細資訊，請參閱[傳統型應用程式中的 UWP 控制項](xaml-host-controls.md)。

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>UWP XAML 傳統型應用程式的右裝載 API？

UWP XAML 裝載 API 提供低階的基礎結構裝載 UWP 控制項，傳統型應用程式中。 某些類型的傳統型應用程式可以選擇使用替代、 更便利的 Api 來完成此目標。  

* 如果您有 c + + Win32 傳統型應用程式，而且您想在您的應用程式中裝載 UWP 控制項，您必須使用 UWP XAML 裝載 API。 不有任何這類的應用程式的替代方法。

* 對於 WPF 和 Windows Form 應用程式，我們建議在 Windows 社群工具組，而不是 UWP XAML 中使用的[包裝的控制項](xaml-host-controls.md#wrapped-controls)和[主控制項](xaml-host-controls.md#host-controls)裝載 API。 這些控制項使用 UWP XAML 裝載在內部的 API，並提供更簡單的開發體驗。 不過，您可以使用 UWP XAML 裝載直接在這些類型的應用程式中的 API，如果您選擇。

## <a name="related-samples"></a>相關範例

使用 UWP XAML 裝載在程式碼中的 API 的方式取決於您的應用程式類型，在設計您的應用程式，及其他因素。 為了協助說明如何使用此 API 的完整應用程式的內容中，這篇文章指的是程式碼從下列的範例。

### <a name="c-win32"></a>C + + Win32

有數個 GitHub 上的範例，示範如何使用 UWP XAML，c + + Win32 應用程式中裝載 API:

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)。 這個範例示範如何將 UWP [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)， [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)，以及[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)控制項新增至 c + + Win32 應用程式。
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32)。 這個範例示範如何將數個基本 UWP 控制項新增至 c + + Win32 應用程式和處理變更 DPI。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows Form

Windows 社群工具組中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項做為參考範例適用於使用 UWP 裝載在 WPF 和 Windows Form 應用程式中的 API。 在下列位置，可供使用的原始程式碼：

  * 針對 WPF 控制項，[請移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)的版本。 WPF 版本衍生自[**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。
  * Windows Forms 控制項，[請移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)的版本。 Windows Forms 版本衍生自[**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

## <a name="prerequisites"></a>必要條件

UWP XAML 裝載 API 有這些先決條件。

* Windows 10 Insider Preview 組建 17709 （或較新的組建） 和 Windows sdk 中的對應 Insider Preview 組建。 這是不斷變化的功能，所以我們建議使用最新的可用的最佳體驗建置。

* 若要使用 UWP XAML 裝載傳統型應用程式中的 API，您將需要設定您的專案，因此您可以呼叫 UWP Api:

    * **C + + Win32:** 我們建議您設定您的專案使用[C + + /winrt](../cpp-and-winrt-apis/index.md)。 下載並安裝[C + + /winrt Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)從 Visual Studio Marketplace，然後新增```<CppWinRTEnabled>true</CppWinRTEnabled>```屬性設為所述[此處](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)您.vcxproj 檔案。

    * **Windows Form 及 WPF:** 請依照[下列指示](../porting/desktop-to-uwp-enhance.md)。

## <a name="architecture-of-xaml-islands"></a>架構的 XAML 群島

UWP XAML 裝載 API 包含[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)、 [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)及其他相關的類型[**elementcompositionpreview**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting)命名空間中。 傳統型應用程式可以使用此 API 來轉譯 UWP 控制項並路由傳送到和退出元素的鍵盤焦點瀏覽。 傳統型應用程式也可調整大小和位置可視需要予以 UWP 控制項。

當您建立的 XAML 島使用 XAML 的傳統型應用程式中裝載 API 時，您將會有下列物件的階層：

* 在基本層級是您想要用來裝載 XAML 島在您的應用程式的 UI 元素。 這個 UI 元素必須具有視窗控制代碼 (HWND)。 您可以在其中裝載的 XAML 島的 UI 元素的範例包括[**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) WPF 應用程式、 Windows Forms 應用程式， [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)和 c + + Win32 應用程式[視窗](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)。

* 在下一步是**DesktopWindowXamlSource**物件層級。 這個物件會提供適用於裝載 XAML 島基礎結構。 您的程式碼負責建立此物件，並將它附加到父 UI 元素。

* 當您建立**DesktopWindowXamlSource**時，此物件會自動建立原生子視窗來裝載您的 UWP 控制項。 此原生子視窗大部分是抽象的程式碼中，但您可以存取其控制代碼 (HWND) 如有必要。

* 最後，在最上層是您要在傳統型應用程式中裝載 UWP 控制項。 這可以是任何衍生自[**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括 Windows SDK，以及自訂使用者控制項所提供的任何 UWP 控制項的 UWP 物件。

下圖說明 XAML 島中物件的階層。

![DesktopWindowXamlSource 架構](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>如何裝載 UWP XAML 控制項

以下是使用 UWP XAML 裝載 API 來裝載您的應用程式中的 UWP 控制項的主要步驟。

1. 在您的應用程式建立的任何[**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)物件，它將會裝載中[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)之前，請初始化 UWP XAML 架構目前的執行緒。

    * 如果您的應用程式會建立**DesktopWindowXamlSource**物件，它會建立任何**Windows.UI.Xaml.UIElement**物件之前，此架構將會初始化為您時您具現化**DesktopWindowXamlSource**物件. 在此案例中，您不需要新增您自己來初始化架構的任何程式碼。

    * 不過，如果您的應用程式會建立**Windows.UI.Xaml.UIElement**物件，它會建立裝載它們**DesktopWindowXamlSource**物件之前，您的應用程式必須呼叫靜態[**WindowsXamlManager.InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)來明確初始化 UWP XAML 架構之前**Windows.UI.Xaml.UIElement**物件會具現化, 的方法。 具現化裝載**DesktopWindowXamlSource**父 UI 元素時，您的應用程式通常應該呼叫此方法。

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > 這個方法會傳回[**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)物件，其包含 UWP XAML 架構的參考。 您可以建立多個**WindowsXamlManager**物件為您想要針對特定的執行緒上。 不過，因為每個物件保留 UWP XAML 架構的參考，您應該處置的物件，以確保最終發行 XAML 資源。

2. 建立[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)物件，並將它附加到父視窗控制代碼相關聯的應用程式中 UI 元素。

    若要這樣做，您將需要遵循下列步驟執行：

    1. 建立**DesktopWindowXamlSource**物件，並將其轉換為**IDesktopWindowXamlSourceNative** COM 介面。 此介面中宣告```windows.ui.xaml.hosting.desktopwindowxamlsource.h```Windows SDK 中的標頭檔案。 在 c + + Win32 專案中，您可以直接參考此標頭檔案。 在 WPF 或 Windows Forms 專案中，您將需要宣告此介面，在您的應用程式程式碼中使用[**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute)屬性。 請確定您的介面宣告完全符合中的介面宣告```windows.ui.xaml.hosting.desktopwindowxamlsource.h```。

    2. 呼叫**AttachToWindow**方法的**IDesktopWindowXamlSourceNative**介面，並傳入您的應用程式中的父 UI 元素的視窗控制代碼。

    3. 設定初始**DesktopWindowXamlSource**中所包含的內部子視窗的大小。 根據預設，此內部子視窗設為寬度和高度為 0。 如果您未設定視窗的大小，您將新增到**DesktopWindowXamlSource**任何 UWP 控制項將不會顯示。 若要存取內部子視窗**DesktopWindowXamlSource**中的，使用**IDesktopWindowXamlSourceNative**介面的**WindowHandle**屬性。 下列範例會使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函式來設定視窗的大小。

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
    // that will act as the parent UI elemnt for your XAML island. It also assumes
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

3. 設定您想要您**DesktopWindowXamlSource**物件的[**內容**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)屬性來裝載**Windows.UI.Xaml.UIElement** 。 下列範例會設定名為[**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) ```myGrid```的**內容**屬性。

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

如需示範這些工作的工作範例應用程式內容中的完整範例，請參閱下列程式碼檔案：

  * **C + + Win32:** 請參閱[Main.cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp)範例中的檔案[XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)或[Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp)範例中的檔案[XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) 。
  * **WPF:** 請參閱 Windows 社群工具組中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs)檔案。  
  * **Windows Forms:** 請參閱 Windows 社群工具組中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs)檔案。


## <a name="how-to-host-custom-uwp-xaml-controls"></a>如何自訂主機 UWP XAML 控制項

> [!IMPORTANT]
> 目前，從第 3 方自訂 UWP XAML 控制項只支援在 C# WPF 和 Windows Form 應用程式中。 您必須有控制項的原始程式碼，讓您可以在您的應用程式中對它進行編譯。

如果您想要裝載自訂 UWP XAML 控制項 （您可以定義自己的控制項或第 3 方提供的控制項），您必須執行下列的額外工作，除了[上一節](#how-to-host-uwp-xaml-controls)中所述的程序。

1. 定義衍生自[**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application)並也會實作[**IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider)的自訂類型。 這種類型做為根中繼資料提供者載入組件，在目前的目錄中自訂的 UWP XAML 類型，您的應用程式的中繼資料。

    如需範例，示範如何執行此動作，請參閱 Windows 社群工具組中的[XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs)程式碼檔案。 這個檔案是針對 WPF 和 Windows Form，說明如何使用 UWP XAML 裝載在這些類型的應用程式中的 API 的共用**WindowsXamlHost**類別實作的一部分。

2. UWP XAML 控制項的型別名稱指派時，呼叫[**GetXamlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype)方法根中繼資料提供者 （這無法在執行階段，指派給在程式碼中，或您可能會選擇啟用這個選項可在 Visual Studio 的 [屬性] 視窗中指派）。

    如需範例，示範如何執行此動作，請參閱 Windows 社群工具組中的[UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs)程式碼檔案。 這個檔案是針對 WPF 和 Windows Form 的共用**WindowsXamlHost**類別實作的一部分。

3. 將自訂的 UWP XAML 控制項的原始碼整合到您的主機應用程式方案、 建置自訂控制項，並由下列[這些指示](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)應用程式中使用。

## <a name="how-to-handle-keyboard-focus-navigation"></a>如何處理鍵盤焦點瀏覽

當使用者瀏覽您的應用程式 （例如，藉由按下**tab 鍵**或方向鍵方向/金鑰） 使用鍵盤中的 UI 元素時，您將需要以程式設計方式將焦點移至，而不使用**DesktopWindowXamlSource**物件。 當使用者的鍵盤瀏覽到達**DesktopWindowXamlSource**，針對您的 UI，瀏覽順序中的第一個[**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)物件到移動焦點時繼續將焦點移至下列**Windows.UI.Xaml.UIElement**物件在使用者循環期間，透過元素，然後回到退出**DesktopWindowXamlSource**和到父項 UI 元素的移動焦點。  

UWP XAML 裝載 API 提供數個類型和成員，可協助您完成這些工作。

1. 當您**DesktopWindowXamlSource**進入鍵盤瀏覽時，會引發[**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)事件。 處理此事件，並使用[**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)方法，以程式設計方式將焦點移至第一個託管**Windows.UI.Xaml.UIElement** 。

2. 當使用者在您**DesktopWindowXamlSource**中的最後一個可設定焦點項目上，並按下**Tab**鍵或方向鍵時， [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)會引發的事件。 處理此事件，並以程式設計方式將焦點移至主應用程式中的下一個可設定焦點項目。 例如，在 WPF 應用程式中裝載**DesktopWindowXamlSource** [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)中，您可以使用[**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)方法傳輸到主機應用程式中的下一個可設定焦點元素的焦點。

如需範例，示範如何使用範例應用程式的內容中執行此動作，請參閱下列程式碼檔案：
  * **WPF:** 請參閱 Windows 社群工具組中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)檔案。  
  * **Windows Forms:** 請參閱 Windows 社群工具組中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)檔案。

## <a name="how-to-handle-layout-changes"></a>如何處理版面配置變更

當使用者變更父 UI 元素的大小時，您將需要處理任何必要的版面配置變更，以確定您的 UWP 控制項如預期般顯示。 以下是一些要考慮的重要案例。

1. 當父 UI 項目需要取得矩形的區域，以符合您裝載**DesktopWindowXamlSource** **Windows.UI.Xaml.UIElement**所需的大小時，請呼叫**Windows.UI.Xaml.UIElement 的[**度量**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)方法**. 例如：
    * 在 WPF 應用程式就可以這麼從裝載**DesktopWindowXamlSource** [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)方法。
    * 在 Windows Forms 應用程式就可以這麼[**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)方法**DesktopWindowXamlSource**會裝載的[**控制項**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

2. 當父 UI 項目變更，大小呼叫的根**Windows.UI.Xaml.UIElement** [**排列**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)方法，您裝載**DesktopWindowXamlSource**上。 例如：
    * 在 WPF 應用程式就可以這麼從裝載**DesktopWindowXamlSource** [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)物件的[**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)方法。
    * 在 Windows Forms 應用程式就可以這麼的[**控制項**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)在[**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)事件的處理常式從該主機**DesktopWindowXamlSource**。

如需範例，示範如何使用範例應用程式的內容中執行此動作，請參閱下列程式碼檔案：
  * **WPF:** 請參閱 Windows 社群工具組中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。  
  * **Windows Forms:** 請參閱 Windows 社群工具組中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

## <a name="how-to-handle-dpi-changes"></a>如何處理 DPI 變更

如果您想要處理視窗的 DPI 變更的裝載您的 UWP 控制項 （例如，如果使用者使用不同的螢幕 DPI 監視器之間拖曳視窗），您將需要使用轉譯轉換設定 UWP 控制項，接聽 DPI 變更您的應用程式並更新視窗的位置和轉譯以回應 DPI 變更 UWP 控制項的轉換。

下列步驟說明一種方式來處理這個程序的 c + + Win32 應用程式內容中。 如需完整範例，請參閱 GitHub 上的[Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp)和[Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h)程式碼中的檔案[XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32)範例。

1. 維護您的應用程式中的[**ScaleTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform)物件，並將它指派給您的 UWP 控制項的[**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)方法。 下列範例會在 c + + Win32 應用程式中為[**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid)控制項。

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. 在您[**WindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx)函式中，接聽[**WM_DPICHANGED**](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged)訊息。 回應此訊息：
    * 使用[**SetWindowPos**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函式調整大小的視窗中包含 UWP 控制項，以傳遞至訊息的矩形。
    * 更新您的**ScaleTransform**物件，根據新的 DPI 值的 x 軸和 y 軸比例因素。
    * 進行任何必要的調整的外觀與您的 UWP 控制項的版面配置。 下列程式碼範例會調整[**邊框間距**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding)的裝載**Windows.UI.Xaml.Controls.Grid**控制項以回應 DPI 變更。

    ```cppwinrt
    LRESULT HandleDpiChange(HWND hWnd, WPARAM wParam, LPARAM lParam)
    {
        HWND hWndStatic = GetWindow(hWnd, GW_CHILD);
        if (hWndStatic != nullptr)
        {
            UINT uDpi = HIWORD(wParam);

            // Resize the window
            auto lprcNewScale = reinterpret_cast<RECT*>(lParam);

            SetWindowPos(hWnd, nullptr, lprcNewScale->left, lprcNewScale->top,
                lprcNewScale->right - lprcNewScale->left, lprcNewScale->bottom - lprcNewScale->top,
                SWP_NOZORDER | SWP_NOACTIVATE);

            NewScale(uDpi);
          }
          return 0;
    }

    void NewScale(UINT dpi) {

        auto scaleFactor = (float)dpi / 100;

        if (m_scale != nullptr) {
            m_scale.ScaleX(scaleFactor);
            m_scale.ScaleY(scaleFactor);
        }

        ApplyCorrection(scaleFactor);
    }

    void ApplyCorrection(float scaleFactor) {
        float rightCorrection = (m_rootGrid.Width() * scaleFactor - m_rootGrid.Width()) / scaleFactor;
        float bottomCorrection = (m_rootGrid.Height() * scaleFactor - m_rootGrid.Height()) / scaleFactor;

        m_rootGrid.Padding(Windows::UI::Xaml::ThicknessHelper::FromLengths(0, 0, rightCorrection, bottomCorrection));
    }
    ```

2. 若要設定您的應用程式，請留意每個監視器的 DPI，將[資訊清單中以並排組件](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)新增到您的專案，並將設定```<dpiAwareness>```元素```PerMonitorV2```。 如需有關此值的詳細資訊，請參閱[**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)的描述。

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

    完整的範例-並存組件資訊清單，請參閱 GitHub 上的[XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest)範例中的檔案[XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) 。

## <a name="limitations"></a>限制

XAML 裝載 API 可共用相同的 Windows 10 限制為所有其他類型的 XAML 主控制項。 如需詳細的清單，請參閱[XAML 主機控制項限制](xaml-host-controls.md#limitations)。

## <a name="troubleshooting"></a>疑難排解

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>使用 UWP XAML 的 UWP 應用程式中裝載 API 的錯誤

| 問題 | 解決方法 |
|-------|------------|
| 您的應用程式會收到**COMException**包含以下訊息: 「 無法啟用 DesktopWindowXamlSource。 這種類型無法使用 UWP 應用程式中。 」 或者 「 無法啟用 WindowsXamlManager。 這種類型無法使用 UWP 應用程式中。 」 | 這個錯誤表示您嘗試使用 UWP XAML 裝載 API （具體而言，您嘗試具現化[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)或[**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)類型） 中的 UWP 應用程式。 UWP XAML 裝載 API 只適用於非 UWP 傳統型應用程式，例如 WPF、 Windows Forms 和 c + + Win32 應用程式。 |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>錯誤附加到不同的執行緒上的視窗

| 問題 | 解決方法 |
|-------|------------|
| 您的應用程式會收到**COMException**包含以下訊息: 「 AttachToWindow 方法失敗因為指定的 HWND 不同的執行緒上建立 」。 | 這個錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative.AttachToWindow**方法，並傳遞它在不同的執行緒上建立的視窗 HWND。 您必須在呼叫方法的程式碼的相同執行緒上建立的視窗 HWND 傳遞此方法。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>錯誤附加到不同的最上層視窗上的視窗

| 問題 | 解決方法 |
|-------|------------|
| 您的應用程式會收到**COMException**包含以下訊息: 「 AttachToWindow 方法失敗因為指定的 HWND 降移從不同的最上層視窗，比先前的相同執行緒傳遞給 AttachToWindow HWND 」。 | 這個錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative.AttachToWindow**方法，並傳遞它是從不同的最上層視窗，比視窗，您在先前呼叫這個方法中指定的視窗 HWND在相同的執行緒。</p></p>您的應用程式特定的執行緒上呼叫**IDesktopWindowXamlSourceNative.AttachToWindow**之後，相同的執行緒上的所有其他[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)物件只能附加到是相同的最上層視窗的子系的 windows**IDesktopWindowXamlSourceNative.AttachToWindow**第一個呼叫中傳遞。 所有**DesktopWindowXamlSource**物件都會被都關閉特定的執行緒下, 一步**DesktopWindowXamlSource**時再免費重新連接任何視窗。</p></p>若要解決此問題，請關閉所有**DesktopWindowXamlSource**物件，這個執行緒，繫結至其他最上層視窗，或為此**DesktopWindowXamlSource**建立新的執行緒。 |

## <a name="related-topics"></a>相關主題

* [傳統型應用程式中的 UWP 控制項](xaml-host-controls.md)
