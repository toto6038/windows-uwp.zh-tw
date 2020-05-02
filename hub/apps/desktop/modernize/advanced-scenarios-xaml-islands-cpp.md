---
description: 此文章討論適用於 C++ Win32 應用程式的進階 XAML Island 裝載案例。
title: C++ Win32 應用程式中適用於 XAML Islands 的進階案例
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, xaml islands, 包裝的控制項, 標準控制項
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80226232"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>C++ Win32 應用程式中適用於 XAML Islands 的進階案例

[裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)和[裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)文章均提供在 C++ Win32 應用程式中裝載 XAML Islands 的指示和範例。 不過，這些文章中的程式碼範例不會處理許多進階案例，而傳統型應用程式可能需要處理這類案例，才能提供順暢的使用者體驗。 此文章提供這其中部分案例的指導方針，以及相關程式碼範例的指標。

## <a name="keyboard-input"></a>鍵盤輸入

若要針對每個 XAML Island 適當地處理鍵盤輸入，您的應用程式必須將所有的 Windows 訊息傳遞到 UWP XAML 架構，才能正確處理特定訊息。 若要這樣做，在應用程式中某個可存取訊息迴圈的位置上，將適用於每個 XAML Island 的 **DesktopWindowXamlSource** 物件轉換為 **IDesktopWindowXamlSourceNative2** COM 介面。 然後，呼叫此介面的 **PreTranslateMessage** 方法，並傳入目前的訊息。

  * **C++ Win32：** 應用程式可以在其主要訊息迴圈中直接呼叫 **PreTranslateMessage**。 如需範例，請參閱 [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) 檔案。

  * **WPF：** 應用程式可以從 [ComponentDispatcher.ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) \(部分機器翻譯\) 事件的事件處理常式中呼叫 **PreTranslateMessage**。 如需範例，請參閱 Windows 社群工具組中的 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) 檔案。

  * **Windows Forms：** 應用程式可以從 [Control.PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage) 方法的覆寫中呼叫 **PreTranslateMessage**。 如需範例，請參閱 Windows 社群工具組中的 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) 檔案。

## <a name="keyboard-focus-navigation"></a>鍵盤焦點瀏覽

當使用者使用鍵盤來瀏覽應用程式中的 UI 元素時 (例如，按 **Tab** 鍵或方向鍵) 時，您必須以程式設計方式將焦點移入和移出 **DesktopWindowXamlSource** 物件。 當使用者的鍵盤瀏覽到達 **DesktopWindowXamlSource** 時，將焦點移至 UI 瀏覽順序中的第一個 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 物件、當使用者循環瀏覽元素時繼續將焦點移至後續的 **Windows.UI.Xaml.UIElement** 物件，接著將焦點移回到 **DesktopWindowXamlSource** 及父 UI 元素。  

UWP XAML 裝載 API 提供數種類型和成員，可協助您完成這些工作。

* 當鍵盤瀏覽進入您的 **DesktopWindowXamlSource** 時，就會引發 [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) \(英文\) 事件。 處理此事件，並使用 [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) \(英文\) 方法，以程式設計方式將焦點移至第一個裝載的 **Windows.UI.Xaml.UIElement**。

* 當使用者位於您的 **DesktopWindowXamlSource** 中最後一個可設定焦點的元素上，並按 **Tab** 鍵或方向鍵時，就會引發 [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) \(英文\) 事件。 處理此事件，並以程式設計方式將焦點移至主控件應用程式中下一個可設定焦點的元素。 例如，在 **DesktopWindowXamlSource** 裝載於 [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) \(部分機器翻譯\) 的 WPF 應用程式中，您可以使用 [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) \(部分機器翻譯\) 方法，以將焦點轉移到主控件應用程式中下一個可設定焦點的元素。

如需示範如何在運作中範例應用程式的內容中執行此操作的範例，請參閱下列程式碼檔案：

  * **C++/Win32：** 請參閱 [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) 檔案。

  * **WPF：** 請參閱 Windows 社群工具組中的 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) 檔案。  

  * **Windows Forms：** 請參閱 Windows 社群工具組中的 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) 檔案。

## <a name="handle-layout-changes"></a>處理版面配置變更

當使用者變更父 UI 元素的大小時，您必須處理任何必要的版面配置變更，以確保您的 UWP 控制項會如預期般顯示。 以下是一些需要考慮的重要案例。

* 在 C++ Win32 應用程式中，當您的應用程式處理 WM_SIZE 訊息時，可以使用 [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) \(英文\) 函式重新置放裝載的 XAML Island。 如需範例，請參閱 [SampleApp.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) 程式碼檔案。

* 當父 UI 元素需要取得所需的矩形區域大小以符合您在 **DesktopWindowXamlSource** 上所裝載的 **Windows.UI.Xaml.UIElement** 時，請呼叫 **Windows.UI.Xaml.UIElement** 的 [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) \(英文\) 方法。 例如：

    * 在 WPF 應用程式中，您可以從裝載 **DesktopWindowXamlSource** 之 [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) \(部分機器翻譯\) 的 [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) \(部分機器翻譯\) 方法中執行此動作。 如需範例，請參閱 Windows 社群工具組中的 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 檔案。

    * 在 Windows Forms 應用程式中，您可以從裝載 **DesktopWindowXamlSource** 之 [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 的 [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) 方法中執行此動作。 如需範例，請參閱 Windows 社群工具組中的 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 檔案。

* 當父 UI 元素的大小變更時，請呼叫您裝載於 **DesktopWindowXamlSource** 上根 **Windows.UI.Xaml.UIElement** 的 [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) \(英文\) 方法。 例如：

    * 在 WPF 應用程式中，您可以從裝載 **DesktopWindowXamlSource** 之 [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) \(部分機器翻譯\) 物件的 [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) \(部分機器翻譯\) 方法中執行此動作。 如需範例，請參閱 Windows 社群工具組中的 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 檔案。

    * 在 Windows Forms 應用程式中，您可以從裝載 **DesktopWindowXamlSource** 之 [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 的 [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) 事件中執行此動作。 如需範例，請參閱 Windows 社群工具組中的 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 檔案。

## <a name="handle-dpi-changes"></a>處理 DPI 變更

UWP XAML 架構會針對裝載的 UWP 控制項自動處理 DPI 變更 (例如，當使用者在具有不同螢幕 DPI 的監視器之間拖曳視窗時)。 為了獲得最佳體驗，我們建議您將 Windows Forms、WPF 或 C++ Win32 應用程式設定為個別監視器 DPI 感知。

若要將應用程式設定為個別監視器 DPI 感知，請將[並存組件資訊清單](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) \(英文\) 新增到您的專案，並將 **\<dpiAwareness\>** 元素設定為 **PerMonitorV2**。 如需此值的詳細資訊，請參閱 [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context) 的描述。

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

* [在傳統型應用程式中裝載 UWP XAML 控制項 (XAML Islands)](xaml-islands.md)
* [在 C++ Win32 應用程式中使用 UWP XAML 裝載 API](using-the-xaml-hosting-api.md)
* [在 C++ Win32 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [在 C++ Win32 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [XAML Islands 程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples) \(英文\)
* [C++ Win32 XAML Islands 範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
