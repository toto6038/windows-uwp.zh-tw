---
description: 本文討論 Win32 應用程式的C++ Advanced XAML 島裝載案例。
title: Win32 應用程式中C++ XAML 孤島的 Advanced 案例
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10，uwp，cpp，win32，xaml islands，包裝的控制項，標準控制項
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226232"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Win32 應用程式中C++ XAML 孤島的 Advanced 案例

[裝載標準 uwp 控制項](host-standard-control-with-xaml-islands-cpp.md)和[裝載自訂 uwp 控制項](host-custom-control-with-xaml-islands-cpp.md)文章提供在C++ Win32 應用程式中裝載 XAML 孤島的指示和範例。 不過，這些文章中的程式碼範例並不會處理許多先進的案例，桌面應用程式可能需要進行處理，以提供順暢的使用者體驗。 本文提供部分案例的指引，以及相關程式碼範例的指標。

## <a name="keyboard-input"></a>鍵盤輸入

若要適當地處理每個 XAML 島的鍵盤輸入，您的應用程式必須將所有的 Windows 訊息傳遞至 UWP XAML 架構，才能正確處理特定訊息。 若要這樣做，您可以在應用程式中存取訊息迴圈的某個位置，將每個 XAML 島的**DesktopWindowXamlSource**物件轉換成**IDesktopWindowXamlSourceNative2** COM 介面。 然後，呼叫此介面的**PreTranslateMessage**方法，並傳入目前的訊息。

  * Win32：：應用程式可以直接在其主要訊息迴圈中呼叫**PreTranslateMessage** 。 **C++** 如需範例，請參閱[XamlBridge .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16)檔案。

  * **WPF：** 應用程式可以從[ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage)事件的事件處理常式呼叫**PreTranslateMessage** 。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177)檔案。

  * **Windows Forms：** 應用程式可以從[system.windows.forms.control.preprocessmessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage)方法的覆寫呼叫**PreTranslateMessage** 。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100)檔案。

## <a name="keyboard-focus-navigation"></a>鍵盤焦點導覽

當使用者使用鍵盤導覽應用程式中的 UI 專案時（例如，按**tab**或方向/方向鍵），您將需要以程式設計方式將焦點移入和移出**DesktopWindowXamlSource**物件。 當使用者的鍵盤導覽到達**DesktopWindowXamlSource**時，請將焦點移至 ui 導覽順序中的第一個 node.js 物件，然後在使用者逐一查看專案時繼續將焦點移**至下列的** [node.js 物件，](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)然後將焦點移出**DesktopWindowXamlSource**和父 UI 元素。）。  

UWP XAML 裝載 API 提供數種類型和成員，可協助您完成這些工作。

* 當鍵盤導覽進入您的**DesktopWindowXamlSource**時，會引發[GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)事件。 處理這個事件，並使用[NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)方法，以程式設計方式將焦點移至第一個裝載的**Windows.** node.js。

* 當使用者在**DesktopWindowXamlSource**中的最後一個可設定焦點元素上，並按下**Tab**鍵或方向鍵時，就會引發[TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)事件。 處理這個事件，並以程式設計方式將焦點移至主應用程式中的下一個可設定元素。 例如，在**DesktopWindowXamlSource**裝載于[HWNDHOST](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)中的 WPF 應用程式中，您可以使用[system.windows.frameworkelement.movefocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)方法，將焦點轉移到主應用程式中的下一個可設定元素。

如需示範如何在運作中範例應用程式的內容中執行這項操作的範例，請參閱下列程式碼檔案：

  * /Win32：請參閱[XamlBridge .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp)檔案。 **C++**

  * **WPF：** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)檔案。  

  * **Windows Forms：** 請參閱 Windows 社區工具組中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)檔案。

## <a name="handle-layout-changes"></a>處理版面配置變更

當使用者變更父系 UI 元素的大小時，您必須處理任何必要的版面配置變更，以確保您的 UWP 控制項如預期般顯示。 以下是一些需要考慮的重要案例。

* 在C++ Win32 應用程式中，當您的應用程式處理 WM_SIZE 訊息時，它可以使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函式重新置放裝載的 XAML 島。 如需範例，請參閱[SampleApp .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170)程式碼檔。

* 當父 UI 元素需要取得所需之矩形區域的大小，以符合您在**DesktopWindowXamlSource**上**裝載的 node.js**時，請呼叫**windows. ui**的[Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)方法。 例如，

    * 在 WPF 應用程式中，您可以從裝載**DesktopWindowXamlSource**之[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)的[MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)方法來執行此動作。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

    * 在 Windows Forms 應用程式中，您可以從裝載**DesktopWindowXamlSource**之[控制項](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[getpreferredsize 所](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)方法來執行此動作。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

* 當父 UI 元素的大小變更時，請呼叫您在**DesktopWindowXamlSource**上裝載之根**Windows.** node.js 的[排列](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)方法。 例如，

    * 在 WPF 應用程式中，您可以從裝載**DesktopWindowXamlSource**之[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)物件的[ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)方法來執行此動作。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

    * 在 Windows Forms 應用程式中，您可以從裝載**DesktopWindowXamlSource**之[控制項](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)事件處理常式來執行此動作。 如需範例，請參閱 Windows 社區工具組中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)檔案。

## <a name="handle-dpi-changes"></a>處理 DPI 變更

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

## <a name="related-topics"></a>相關主題

* [在桌面應用程式中裝載 UWP XAML 控制項（XAML 島）](xaml-islands.md)
* [在C++ Win32 應用程式中使用 UWP XAML 裝載 API](using-the-xaml-hosting-api.md)
* [在C++ Win32 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [在C++ Win32 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [XAML 島程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML 群島範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
