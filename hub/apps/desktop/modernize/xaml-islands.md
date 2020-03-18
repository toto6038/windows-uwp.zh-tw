---
description: 本指南可協助您直接在 WPF 和 Windows Forms 應用程式中建立 Fluent 型 UWP UI
title: 傳統型應用程式中的 UWP 控制項
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 061ad7a3f63fc92dd2f865f8870c7de5edf862af
ms.sourcegitcommit: 756217c559155e172087dee4d762d328c6529db6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/09/2020
ms.locfileid: "78935351"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>在傳統型應用程式中裝載 UWP XAML 控制項 (XAML Islands)

從 Windows 10 版本 1903 開始，您可以使用稱為「XAML Islands」  的功能，將 UWP 控制項裝載在非 UWP 傳統型應用程式中。 這項功能可讓您使用僅透過 UWP 控制項提供的最新 Windows 10 UI 功能，來增強現有 WPF、Windows Forms 和 C++ Win32 應用程式的外觀、風格和功能。 這表示您可以使用 UWP 功能，例如 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) 和控制項，這些功能支援現有 WPF、Windows Forms 和 C++ Win32 應用程式中的 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/index)。

您可以裝載衍生自 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的任何 UWP 控制項，包括：

* Windows SDK 或 WinUI 程式庫所提供的任何第一方 UWP 控制項。
* 任何自訂 UWP 控制項 (例如，由數個可搭配使用的 UWP 控制群組成的使用者控制項)。 您必須擁有自訂控制項的原始程式碼，才能使用您的應用程式進行編譯。

基本上，會使用「UWP XAML 裝載 API」  來建立 XAML Islands。 這個 API 是由 Windows 10 版本 1903 SDK 中引進的數個 Windows 執行階段類別和 COM 介面所組成。 我們也在 [Windows 社區工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中提供一組 XAML Island .NET 控制項，在內部使用 UWP XAML 裝載 API，並為 WPF 和 Windows Forms 應用程式提供更方便的開發體驗。

您使用 XAML Islands 的方式取決於您的應用程式類型，以及您想要裝載的 UWP 控制項類型。

> [!NOTE]
> 如果您有關於 XAML Islands 的意見反應，請在 [Microsoft.Toolkit.Win32 存放庫](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)中建立新問題，並留下您的意見。 如果您想要私下提交意見反應，可以將意見反應傳送到 XamlIslandsFeedback@microsoft.com。 您的深入解析和案例對我們而言非常重要。

## <a name="wpf-and-windows-forms-applications"></a>WPF 和 Windows Form 應用程式

我們建議 WPF 和 Windows Forms 應用程式使用「Windows 社區工具組」中提供的 XAML Island .NET 控制項。 這些控制項會提供物件模型，模擬 (或提供存取) 對應 UWP 控制項的屬性、方法和事件。 它們也會處理鍵盤導覽和版面配置變更之類的行為。

WPF 和 Windows Forms 應用程式有兩組 XAML Island 控制項：「包裝的控制項」  和「主控制項」  。 

### <a name="wrapped-controls"></a>包裝的控制項

WPF 和 Windows Forms 應用程式可以使用 XAML Island 控制項的選項，以包裝特定 UWP 控制項的介面和功能。 您可以直接將控制項新增至 WPF 或 Windows Forms 專案的設計介面，然後像設計工具中的任何其他 WPF 或 Windows Forms 控制項一樣來使用它們。

下列包裝的 UWP 控制項目前可在「Windows 社區工具組」中使用。 

| 控制 | 最低支援的作業系統 | 說明 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 (版本 1903) | 在您的 Windows Forms 或 WPF 傳統型應用程式中，為 Windows Ink 型使用者互動提供介面和相關的工具列。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 (版本 1903) | 內嵌檢視，該檢視會在 Windows Forms 或 WPF 傳統型應用程式中串流和轉譯媒體內容 (例如影片)。 |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 (版本 1903) | 可讓您在 Windows Forms 或 WPF 傳統型應用程式中顯示符號或真實感地圖。 |

如需示範如何使用包裝的 UWP 控制項的逐步解說，請參閱[在 WPF 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands.md)。

### <a name="host-controls"></a>主控制項

對於可用包裝的控制項未涵蓋的自訂控制項和其他案例，WPF 和 Windows Forms 應用程式也可以使用「Windows 社群工具組」中提供的 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項。

| 控制 | 最低支援的作業系統 | 說明 |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10 (版本 1903) | 可以裝載衍生自 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的任何 UWP 控制項，包括 Windows SDK 所提供的任何第一方 UWP 控制項，以及自訂控制項。 |

如需示範如何使用 **WindowsXamlHost** 控制項的逐步解說，請參閱[在 WPF 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands.md)和[使用 XAML Islands 在 WPF 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands.md)。

> [!NOTE]
> 只有在以 .NET Core 3 為目標的 WPF 和 Windows Forms 應用程式中，才支援使用 **WindowsXamlHost** 控制項來裝載自訂 UWP 控制項。 以 .NET Framework 或 .NET Core 3 為目標的應用程式支援裝載 Windows SDK 所提供的第一方 UWP 控制項。

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>將您的專案設定為使用 XAML Island .NET 控制項

XAML Island .NET 控制項需要 Windows 10 版本 1903 或更新版本。 若要使用這些控制項，請安裝以下所列的其中一個 NuGet 套件。 這些套件提供您使用 XAML Island 包裝的控制項和主控制項所需的所有項目，並且包含其他也需要的相關 NuGet 套件。

| 控制項的類型 | NuGet 套件  | 相關文章 |
|-----------------|----------------|---------------------|
| [包裝的控制項](#wrapped-controls) | 這些套件的 6.0.0 版或更新版本： <ul><li>WPF：[Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms：[Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [在 WPF 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands.md)  |
| [主控制項](#host-controls) | 這些套件的 6.0.0 版或更新版本： <ul><li>WPF：[Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms：[Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [在 WPF 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands.md)<br/>[在 WPF 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands.md)  |

請注意下列詳細資料：

* 主控制項套件也會包含在包裝的控制項套件中。 如果您想要同時使用這兩組控制項，您可以安裝包裝的控制項套件。

* 如果您裝載的是自訂 UWP 控制項，則您的 WPF 或 Windows Forms 專案必須以 .NET Core 3 為目標。 以 .NET Framework 為目標的應用程式不支援裝載自訂 UWP 控制項。 您也必須執行一些額外的步驟來參考自訂控制項。 如需詳細資訊，請參閱[使用 XAML Islands 在 WPF 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands.md)。

### <a name="web-view-controls"></a>Web 檢視控制項

「Windows 社區工具組」也提供下列 .NET 控制項，以便在 WPF 和 Windows Forms 應用程式中裝載 Web 內容。 這些控制項通常用於類似的傳統型應用程式現代化案例，作為 XAML Island 控制項，而且它們會在與 XAML Island 控制項相同的 [Microsoft.Toolkit.Win32 存放庫](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32)中進行維護。

| 控制 | 最低支援的作業系統 | 說明 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 (版本 1803) | 使用 Microsoft Edge 轉譯引擎來顯示 Web 內容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供與更多作業系統版本相容的 **WebView** 版本。 此控制項使用 Microsoft Edge 轉譯引擎來顯示 Windows 10 1803 版和更新版本上的 Web 內容，以及使用 Internet Explorer 轉譯引擎來顯示舊版 Windows 10、Windows 8.x 和 Windows 7 上的 Web 內容。 |

若要使用這些控制項，請安裝其中一個 NuGet 套件：

* WPF：[Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms：[Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++ Win32 應用程式

C++ Win32 應用程式不支援 XAML Island .NET 控制項。 這些應用程式必須改為使用 Windows 10 SDK (版本 1903 和更新版本) 所提供的「UWP XAML 裝載 API」  。

UWP XAML 裝載 API 是由數個 Windows 執行階段類別和 COM 介面所組成，您的 C++ Win32 應用程式可以用來裝載任何衍生自 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的 UWP 控制項。 您可以在具有相關聯視窗控制碼 (HWND) 的應用程式中，將 UWP 控制項裝載於任何 UI 元素中。 如需此 API 的詳細資訊，包括必要條件，請參閱[在 C++ Win32 應用程式中使用 UWP XAML 裝載 API](using-the-xaml-hosting-api.md)。

> [!NOTE]
> 「Windows 社區工具組」中包裝的控制項和主控制項，會在內部使用 UWP XAML 裝載 API，並在您直接使用 UWP XAML 裝載 API (包括鍵盤導覽和版面配置變更) 的情況下，實作您需要自行處理的所有行為。 對於 WPF 和 Windows Forms 應用程式，我們強烈建議您使用這些控制項，而不是直接使用 UWP XAML 裝載 API，因為它們會抽象化許多使用 API 的實作詳細資料。

## <a name="architecture-of-xaml-islands"></a>XAML Island 的架構

您可以在這裡快速檢視不同類型的 XAML Island 控制項如何在 UWP XAML 裝載 API 上方在架構上進行組織。

![裝載控制項架構](images/xaml-islands/host-controls.png)

隨附於 Windows SDK 的此圖表底部會出現 API。 包裝的控制項和主控制項可透過「Windows 社區工具組」中的 NuGet 套件來取得。

## <a name="window-host-context-for-xaml-islands"></a>XAML Island 的視窗裝載內容

當您在桌面應用程式中裝載 XAML Island 時，您可以在相同的執行緒上同時執行 XAML 內容的多個樹狀結構。 若要在 XAML Island 中存取 XAML 內容樹狀結構的根元素，並取得其裝載所在內容的相關資訊，請使用 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 類別。 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)、[ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) 和 [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) 類別不會提供 XAML Island 的正確資訊。 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) 和 [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) 物件存在於執行緒上，且可供您的應用程式存取，但不會傳回有意義的界限或可見度 (這些物件一律不可見，且大小為 1x1)。 如需詳細資訊，請參閱[視窗裝載](/windows/uwp/design/layout/show-multiple-views#windowing-hosts)。

例如，若要取得裝載在 XAML Island 中的 UWP 控制項所在視窗的周框矩形，請使用控制項的 [XamlRoot.Size](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot.size) 屬性。 可裝載於 XAML Island 中的每個 UWP 控制項都衍生自 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，因此您可以使用控制項的 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.xamlroot) 屬性來存取 **XamlRoot** 物件。

```csharp
Size windowSize = myUWPControl.XamlRoot.Size;
```

請不要使用 [CoreWindows.Bounds](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.bounds) 屬性來取得周框矩形。

```csharp
// This will return incorrect information for a UWP control that is hosted in a XAML Island.
Rect windowSize = CoreWindow.GetForCurrentThread().Bounds;
```

如需您在 XAML Island 內容中應避免使用的一般視窗相關 API 的表格，以及建議的 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 取代項目，請參閱[本節](/windows/uwp/design/layout/show-multiple-views#make-code-portable-across-windowing-hosts)的表格。

## <a name="feature-roadmap"></a>功能藍圖

以下是自從 Windows 10 版本 1903 和 6.0 版「Windows 社區工具組」起 XAML Islands 相關功能的目前狀態：

* **C++ Win32 應用程式：** UWP XAML 裝載 API 會被視為從 Windows 10 版本 1903 起的 1.0 版。
* **以 .NET Framework 4.6.2 和更新版本為目標的受控應用程式：** [6.0.0 版 NuGet 套件](#configure-your-project-to-use-the-xaml-island-net-controls)中所提供的 XAML Island 控制項，會被視為以 .NET Framework 4.6.2 和更新版本為目標的應用程式 1.0 版。
* **以 .NET Core 3.0 和更新版本為目標的受控應用程式：** [6.0.0 版 NuGet 套件](#configure-your-project-to-use-the-xaml-island-net-controls)中所提供的控制項，對於以 .NET Core 3.0 和更新版本為目標的應用程式，仍然處於開發人員預覽狀態。 這些適用於 .NET Core 3.0 和更新版本的控制項 1.0 版已規劃較新版本。

## <a name="additional-resources"></a>其他資源

如需有關使用 XAML Islands 的更多背景資訊和教學課程，請參閱下列文章和資源：

* [將 WPF 應用程式現代化教學課程](modernize-wpf-tutorial.md)：本教學課程提供逐步指示，說明如何使用「Windows 社區工具組」中包裝的控制項和主控制項，將 UWP 控制項新增至現有的 WPF 企業營運應用程式。 本教學課程包含 WPF 應用程式的完整程式碼，以及程序中每個步驟的詳細指示。
* [XAML Islands 程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples)：此報告包含 Windows Forms、WPF 和 C++/Win32 範例，以示範如何使用 XAML Islands。
* [XAML Islands v1 - 更新與藍圖](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)：此部落格文章討論許多有關 XAML Islands 的常見問題，並提供詳細的開發藍圖。
