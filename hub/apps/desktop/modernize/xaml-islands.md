---
description: 本指南可協助您直接在 WPF 和 Windows Forms 應用程式中建立 Fluent 型 UWP UI
title: 在傳統型應用程式中裝載 WinRT XAML 控制項
ms.date: 10/20/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 6e881fbb883d35d35b70eb8984b9264acd78191d
ms.sourcegitcommit: cc871be2508f52509b6a947fe879aeec360d0fd2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/02/2021
ms.locfileid: "106270175"
---
# <a name="host-winrt-xaml-controls-in-desktop-apps-xaml-islands"></a>在傳統型應用程式中裝載 WinRT XAML 控制項 (XAML Islands)

從 Windows 10 版本 1903 開始，您可以使用稱為「XAML Islands」的功能，將 WinRT XAML 控制項裝載在非 UWP 傳統型應用程式中。 這項功能可讓您使用僅透過 WinRT XAML 控制項提供的最新 Windows 10 UI 功能，來增強現有 WPF、Windows Forms 和 C++ Win32 應用程式的外觀、風格和功能。 這表示您可以使用 UWP 功能，例如 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) 和控制項，這些功能支援現有 WPF、Windows Forms 和 C++ Win32 應用程式中的 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/index)。

您可以裝載衍生自 [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) 的任何 WinRT XAML 控制項，包括：

* Windows SDK 或 WinUI 2.x 程式庫所提供的任何第一方 WinRT XAML 控制項。
* 任何自訂 WinRT XAML 控制項 (例如，由數個可搭配使用的 WinRT XAML 控制群組成的使用者控制項)。 您必須擁有自訂控制項的原始程式碼，才能使用您的應用程式進行編譯。

基本上，會使用「UWP XAML 裝載 API」來建立 XAML Islands。 這個 API 是由 Windows 10 版本 1903 SDK 中引進的數個 Windows 執行階段類別和 COM 介面所組成。 我們也在 [Windows 社區工具組](/windows/uwpcommunitytoolkit/)中提供一組 XAML Island .NET 控制項，在內部使用 UWP XAML 裝載 API，並為 WPF 和 Windows Forms 應用程式提供更方便的開發體驗。

您使用 XAML Islands 的方式取決於您的應用程式類型，以及您想要裝載的 WinRT XAML 控制項類型。

> [!NOTE]
> 如果您有關於 XAML Islands 的意見反應，請在 [Microsoft.Toolkit.Win32 存放庫](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)中建立新問題，並留下您的意見。

## <a name="requirements"></a>需求

XAML Islands 具有下列執行階段需求：

* Windows 10 版本 1903 或更新版本。
* 如果您的應用程式沒有封裝在 [MSIX 套件](/windows/msix)中以供部署，則電腦必須安裝 [C++ Visual Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)。

## <a name="wpf-and-windows-forms-applications"></a>WPF 和 Windows Form 應用程式

> [!NOTE]
> 目前只有以 .NET Core 3.x 為目標的應用程式才支援使用 XAML 孤島來裝載 WPF 中的 WinRT XAML 控制項和 Windows Forms 應用程式。 以 .NET 5 為目標的應用程式，或任何 .NET Framework 版本的應用程式中，尚不支援 XAML 孤島。

我們建議 WPF 和 Windows Forms 應用程式使用「Windows 社區工具組」中提供的 XAML Island .NET 控制項。 這些控制項會提供物件模型，模擬 (或提供存取) 對應 WinRT XAML 控制項的屬性、方法和事件。 它們也會處理鍵盤導覽和版面配置變更之類的行為。

WPF 和 Windows Forms 應用程式有兩組 XAML Island 控制項：「包裝的控制項」和「主控制項」。 

### <a name="wrapped-controls"></a>包裝的控制項

WPF 和 Windows Forms 應用程式可以使用 XAML Island 控制項的選項，以包裝特定 WinRT XAML 控制項的介面和功能。 您可以直接將控制項新增至 WPF 或 Windows Forms 專案的設計介面，然後像設計工具中的任何其他 WPF 或 Windows Forms 控制項一樣來使用它們。

下列包裝的 WinRT XAML 控制項目前可在「Windows 社區工具組」中使用。 

| 控制 | 最低支援的作業系統 | 說明 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 (版本 1903) | 在您的 Windows Forms 或 WPF 傳統型應用程式中，為 Windows Ink 型使用者互動提供介面和相關的工具列。 |
| [MediaPlayerElement](/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 (版本 1903) | 內嵌檢視，該檢視會在 Windows Forms 或 WPF 傳統型應用程式中串流和轉譯媒體內容 (例如影片)。 |
| [MapControl](/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 (版本 1903) | 可讓您在 Windows Forms 或 WPF 傳統型應用程式中顯示符號或真實感地圖。 |

如需示範如何使用包裝的 WinRT XAML 控制項的逐步解說，請參閱[在 WPF 應用程式中裝載標準 WinRT XAML 控制項](host-standard-control-with-xaml-islands.md)。

### <a name="host-controls"></a>主控制項

對於可用包裝的控制項未涵蓋的自訂控制項和其他案例，WPF 和 Windows Forms 應用程式也可以使用「Windows 社群工具組」中提供的 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項。

| 控制 | 最低支援的作業系統 | 說明 |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10 (版本 1903) | 可以裝載衍生自 [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) 的任何 WinRT XAML 控制項，包括 Windows SDK 所提供的任何第一方 WinRT XAML 控制項，以及自訂控制項。 |

如需示範如何使用 **WindowsXamlHost** 控制項的逐步解說，請參閱 [在 WPF 應用程式中裝載標準 WinRT XAML 控制項](host-standard-control-with-xaml-islands.md)和 [使用 XAML Islands 在 WPF 應用程式中裝載自訂 WinRT XAML 控制項](host-custom-control-with-xaml-islands.md)。

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>將您的專案設定為使用 XAML Island .NET 控制項

XAML Island .NET 控制項需要 Windows 10 版本 1903 或更新版本。 若要使用這些控制項，請安裝以下所列的其中一個 NuGet 套件。 這些套件提供您使用 XAML Island 包裝的控制項和主控制項所需的所有項目，並且包含其他也需要的相關 NuGet 套件。

| 控制項的類型 | NuGet 套件  | 相關文章 |
|-----------------|----------------|---------------------|
| [包裝的控制項](#wrapped-controls) | 這些套件的 6.0.0 版或更新版本： <ul><li>WPF：[Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms：[Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [在 WPF 應用程式中裝載標準 WinRT XAML 控制項](host-standard-control-with-xaml-islands.md)  |
| [主控制項](#host-controls) | 這些套件的 6.0.0 版或更新版本： <ul><li>WPF：[Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms：[Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [在 WPF 應用程式中裝載標準 WinRT XAML 控制項](host-standard-control-with-xaml-islands.md)<br/>[在 WPF 應用程式中裝載自訂 WinRT XAML 控制項](host-custom-control-with-xaml-islands.md)  |

請注意下列詳細資料：

* 主控制項套件也會包含在包裝的控制項套件中。 如果您想要同時使用這兩組控制項，您可以安裝包裝的控制項套件。

* 如果您裝載的是自訂 WinRT XAML 控制項，您也必須執行一些額外的步驟來參考自訂控制項。 如需詳細資訊，請參閱[使用 XAML Islands 在 WPF 應用程式中裝載自訂 WinRT XAML 控制項](host-custom-control-with-xaml-islands.md)。

### <a name="web-view-controls"></a>Web 檢視控制項

「Windows 社區工具組」也提供下列 .NET 控制項，以便在 WPF 和 Windows Forms 應用程式中裝載 Web 內容。 這些控制項通常用於類似的傳統型應用程式現代化案例，作為 XAML Island 控制項，而且它們會在與 XAML Island 控制項相同的 [Microsoft.Toolkit.Win32 存放庫](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32)中進行維護。

| 控制 | 最低支援的作業系統 | 說明 |
|-----------------|-------------------------------|-------------|
| [WebView](/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 (版本 1803) | 使用 Microsoft Edge 轉譯引擎來顯示 Web 內容。 |
| [WebViewCompatible](/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供與更多作業系統版本相容的 **WebView** 版本。 此控制項使用 Microsoft Edge 轉譯引擎來顯示 Windows 10 1803 版和更新版本上的 Web 內容，以及使用 Internet Explorer 轉譯引擎來顯示舊版 Windows 10、Windows 8.x 和 Windows 7 上的 Web 內容。 |

若要使用這些控制項，請安裝其中一個 NuGet 套件：

* WPF：[Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms：[Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++ Win32 應用程式

C++ Win32 應用程式不支援 XAML Island .NET 控制項。 這些應用程式必須改為使用 Windows 10 SDK (版本 1903 和更新版本) 所提供的「UWP XAML 裝載 API」。

UWP XAML 裝載 API 是由數個 Windows 執行階段類別和 COM 介面所組成，您的 C++ Win32 應用程式可以用來裝載任何衍生自 [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) 的 WinRT XAML 控制項。 您可以在具有相關聯視窗控制碼 (HWND) 的應用程式中，將 WinRT XAML 控制項裝載於任何 UI 元素中。 如需此 API 的詳細資訊，請參閱下列文章：

* [在 C++ Win32 應用程式中使用 UWP XAML 裝載 API](using-the-xaml-hosting-api.md)
* [在 C++ Win32 應用程式中裝載標準 WinRT XAML 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [在 C++ Win32 應用程式中裝載自訂 WinRT XAML 控制項](host-custom-control-with-xaml-islands-cpp.md)

> [!NOTE]
> 「Windows 社區工具組」中包裝的控制項和主控制項，會在內部使用 UWP XAML 裝載 API，並在您直接使用 UWP XAML 裝載 API (包括鍵盤導覽和版面配置變更) 的情況下，實作您需要自行處理的所有行為。 對於 WPF 和 Windows Forms 應用程式，我們強烈建議您使用這些控制項，而不是直接使用 UWP XAML 裝載 API，因為它們會抽象化許多使用 API 的實作詳細資料。

## <a name="architecture-of-xaml-islands"></a>XAML Island 的架構

您可以在這裡快速檢視不同類型的 XAML Island 控制項如何在 UWP XAML 裝載 API 上方在架構上進行組織。

![裝載控制項架構](images/xaml-islands/host-controls.png)

隨附於 Windows SDK 的此圖表底部會出現 API。 包裝的控制項和主控制項可透過「Windows 社區工具組」中的 NuGet 套件來取得。

## <a name="limitations-and-workarounds"></a>限制和因應措施

下列各節將討論使用 XAML Islands 的傳統型應用程式中某些 UWP 開發案例的限制和因應措施。 

### <a name="supported-only-with-workarounds"></a>有因應措施才能使用

:heavy_check_mark:在目前的 XAML Islands 版本中，有條件地支援 XAML Island 中 [WinUI 2.x 程式庫](../../winui/index.md) 的裝載控制項。 如果您的桌面應用程式使用 [MSIX 套件](/windows/msix)進行部署，則可以從 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)NugGet 套件的搶鮮版或發行版本裝載 WinUI 控制項。 如果您的傳統型應用程式未使用 MSIX 進行封裝，您必須先安裝 [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) NuGet 套件搶鮮版才能裝載 WinUI 控制項。 [WinUI 3.0 程式庫](../../winui/winui3/index.md)中的裝載控制項支援會在之後的版本中推出。

:heavy_check_mark:若要在 XAML Island 中存取 XAML 內容樹狀結構的根元素，並取得其裝載所在內容的相關資訊，請勿使用 [CoreWindow](/uwp/api/windows.ui.core.corewindow)、[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)、[Window](/uwp/api/windows.ui.xaml.window) 類別。 而是改成使用 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 類別。 如需詳細資訊，請參閱[本節](#window-host-context-for-xaml-islands)。

:heavy_check_mark:若要從 WPF、Windows Forms 或C++ Win32 應用程式支援[分享協定](/windows/uwp/app-to-app/share-data)，您的應用程式必須使用 [IDataTransferManagerInterop](/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) 介面來取得 [DataTransferManager](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) 物件，以起始特定視窗的共用作業。 如需示範如何在 WPF 應用程式中使用此介面的範例，請參閱 [ShareSource 範例](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource)。

:heavy_check_mark:不支援在 XAML Islands 中搭配使用 `x:Bind` 與裝載控制項。 您將必須在 .NET Standard 程式庫中宣告資料模型。

### <a name="not-supported"></a>不受支援

:no_entry_sign:在以 .NET Framework 為目標的 WPF 和 Windows Forms 應用程式中使用 XAML Islands。 只有以 .NET Core 3.x 為目標的應用程式才支援 XAML Islands。

:no_entry_sign:在執行階段，XAML Islands 中的 UWP XAML 內容不會回應從深色變淺色的 Windows 主題變更，反之亦然。 內容會在執行階段回應高對比變更。

:no_entry_sign:新增 **WebView** 控制項到自訂使用者控制項 (開啟執行緒、關閉執行緒或退出程序)。

:no_entry_sign:在全螢幕模式中不支援 [MediaPlayer](/uwp/api/Windows.Media.Playback.MediaPlayer) 控制項和 [MediaPlayerElement](/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) 主控制項。

:no_entry_sign:含手寫檢視的文字輸入。 如需這項功能的詳細資訊，請參閱[本文](/windows/uwp/design/controls-and-patterns/text-handwriting-view)。

:no_entry_sign:使用 `@Places` 和 `@People` 內容連結的文字控制項。 如需這項功能的詳細資訊，請參閱[本文](/windows/uwp/design/controls-and-patterns/content-links)。

:no_entry_sign:XAML Islands 不支援裝載 [ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)，其中包含接受文字輸入的控制項，例如 [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)、[RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox)或 [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)。 如果這樣做，輸入控制項將不會正確地回應按鍵功能。 若要使用 XAML Island 來達到類似的功能，建議您裝載包含輸入控制項的[快顯視窗](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup)。

:no_entry_sign:XAML Islands 目前不支援在裝載的 [Windows.UI.Xaml.Controls.Image](/uwp/api/Windows.UI.Xaml.Controls.Image) 控制項中，或透過使用 [Windows.UI.Xaml.Media.Imaging.SvgImageSource](/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)物件來顯示 SVG 檔案。 因應措施是將您想要顯示的影像檔案轉換成點陣式的格式，例如 JPG 或 PNG。

### <a name="window-host-context-for-xaml-islands"></a>XAML Island 的視窗裝載內容

當您在桌面應用程式中裝載 XAML Island 時，您可以在相同的執行緒上同時執行 XAML 內容的多個樹狀結構。 若要在 XAML Island 中存取 XAML 內容樹狀結構的根元素，並取得其裝載所在內容的相關資訊，請使用 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 類別。 [CoreWindow](/uwp/api/windows.ui.core.corewindow)、[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) 和 [Window](/uwp/api/windows.ui.xaml.window) 類別不會提供 XAML Island 的正確資訊。 [CoreWindow](/uwp/api/windows.ui.core.corewindow) 和 [Window](/uwp/api/windows.ui.xaml.window) 物件存在於執行緒上，且可供您的應用程式存取，但不會傳回有意義的界限或可見度 (這些物件一律不可見，且大小為 1x1)。 如需詳細資訊，請參閱[視窗裝載](/windows/uwp/design/layout/show-multiple-views#windowing-hosts)。

例如，若要取得裝載在 XAML Island 中的 WinRT XAML 控制項所在視窗的周框矩形，請使用控制項的 [XamlRoot.Size](/uwp/api/windows.ui.xaml.xamlroot.size) 屬性。 可裝載於 XAML Island 中的每個 WinRT XAML 控制項都衍生自 [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement)，因此您可以使用控制項的 [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) 屬性來存取 **XamlRoot** 物件。

```csharp
Size windowSize = myUWPControl.XamlRoot.Size;
```

請不要使用 [CoreWindows.Bounds](/uwp/api/windows.ui.core.corewindow.bounds) 屬性來取得周框矩形。

```csharp
// This will return incorrect information for a WinRT XAML control that is hosted in a XAML Island.
Rect windowSize = CoreWindow.GetForCurrentThread().Bounds;
```

如需您在 XAML Island 內容中應避免使用的一般視窗相關 API 的表格，以及建議的 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 取代項目，請參閱[本節](/windows/uwp/design/layout/show-multiple-views#make-code-portable-across-windowing-hosts)的表格。

如需示範如何在 WPF 應用程式中使用此介面的範例，請參閱 [ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource) 範例。

## <a name="additional-resources"></a>其他資源

如需有關使用 XAML Islands 的更多背景資訊和教學課程，請參閱下列文章和資源：

* [將 WPF 應用程式現代化教學課程](modernize-wpf-tutorial.md)：本教學課程提供逐步指示，說明如何使用「Windows 社區工具組」中包裝的控制項和主控制項，將 WinRT XAML 控制項新增至現有的 WPF 企業營運應用程式。 本教學課程包含 WPF 應用程式的完整程式碼，以及程序中每個步驟的詳細指示。
* [XAML Islands 程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples)：此報告包含 Windows Forms、WPF 和 C++/Win32 範例，以示範如何使用 XAML Islands。
* [XAML Islands v1 - 更新與藍圖](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)：此部落格文章討論許多有關 XAML Islands 的常見問題，並提供詳細的開發藍圖。