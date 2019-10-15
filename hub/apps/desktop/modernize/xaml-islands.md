---
description: 本指南可協助您直接在 WPF 和 Windows Forms 應用程式中建立 Fluent 型 UWP UI
title: 桌面應用程式中的 UWP 控制項
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10，uwp，windows forms，wpf，xaml 群島
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 52287576dbc395af60e15b5f4b4a403db7e92900
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72313447"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>在桌面應用程式中裝載 UWP XAML 控制項（XAML 島）

從 Windows 10 版本1903開始，您可以使用稱為*XAML Islands*的功能，將 uwp 控制項裝載在非 UWP 桌面應用程式中。 這項功能可讓您使用僅透過 UWP 控制項提供的最新 Windows 10 UI 功能，來C++增強現有 WPF、Windows Forms 和 Win32 應用程式的外觀、風格和功能。 這表示您可以在現有的 WPF、Windows Forms 和C++ Win32 應用程式中使用 UWP 功能（例如[Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)和控制項），以支援流暢的[設計系統](/windows/uwp/design/fluent-design-system/index)。

您可以裝載衍生自[Windows. UI](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 UWP 控制項，包括：

* Windows SDK 或 WinUI 程式庫所提供的任何第一方 UWP 控制項。
* 任何自訂 UWP 控制項（例如，由數個可搭配使用的 UWP 控制群組成的使用者控制項）。 您必須擁有自訂控制項的原始程式碼，才能使用您的應用程式進行編譯。

基本上，會使用*UWP xaml 裝載 API*來建立 XAML 島。 這個 API 是由 Windows 10 版本 1903 SDK 中引進的數個 Windows 執行階段類別和 COM 介面所組成。 我們也在[Windows 社區工具](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)組中提供一組 xaml 島 .net 控制項，在內部使用 UWP XAML 裝載 API，並為 WPF 和 Windows Forms 應用程式提供更方便的開發體驗。

您使用 XAML 島的方式取決於您的應用程式類型，以及您想要裝載的 UWP 控制項類型。

> [!NOTE]
> 如果您有關于 XAML Islands 的意見反應，請在[Microsoft 工具](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)組的 Win32 存放庫中建立新的問題，並在該處留下您的意見。 如果您想要私下提交意見反應，可以將它傳送給 XamlIslandsFeedback@microsoft.com。 您的深入解析和案例對我們而言非常重要。

## <a name="wpf-and-windows-forms-applications"></a>WPF 和 Windows Forms 應用程式

我們建議 WPF 和 Windows Forms 應用程式使用 Windows 社區工具組中提供的 XAML 島 .NET 控制項。 這些控制項會提供物件模型，以模擬（或提供）對應 UWP 控制項的屬性、方法和事件的存取權。 它們也會處理鍵盤導覽和版面配置變更之類的行為。

WPF 和 Windows Forms 應用程式有兩組 XAML 島控制項：已*包裝的控制項*和*主控制項*。 從 Windows 10 1903 版，這些控制項是以[開發人員預覽](#feature-roadmap)的形式提供。

### <a name="wrapped-controls"></a>包裝的控制項

WPF 和 Windows Forms 應用程式可以使用 XAML 島控制項的選取專案，以包裝特定 UWP 控制項的介面和功能。 您可以將這些控制項直接加入至 WPF 或 Windows Forms 專案的設計介面，然後像設計工具中的任何其他 WPF 或 Windows Forms 控制項一樣使用它們。

下列已包裝的 UWP 控制項目前可在 Windows 社區工具組中使用。 

| 控制項 | 支援的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 (版本 1903) | 在您的 Windows Forms 或 WPF 桌面應用程式中，為 Windows 筆墨型使用者互動提供介面和相關的工具列。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 (版本 1903) | 內嵌在 Windows Forms 或 WPF 桌面應用程式中串流和轉譯媒體內容（例如影片）的視圖。 |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 (版本 1903) | 可讓您在 Windows Forms 或 WPF 桌面應用程式中顯示符號或照片的地圖。 |

如需示範如何使用已包裝 UWP 控制項的逐步解說，請參閱[在 WPF 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands.md)。

### <a name="host-controls"></a>主控制項

除了可用包裝控制項所涵蓋的案例之外，WPF 和 Windows Forms 應用程式也可以使用 Windows 社區工具組中提供的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項。

| 控制項 | 支援的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10 (版本 1903) | 可以裝載衍生自[Windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 UWP 控制項，包括 Windows SDK 所提供的任何第一方 uwp 控制項，以及自訂控制項。 |

如需示範如何使用**WindowsXamlHost**控制項的逐步解說，請參閱[在 wpf 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands.md)和[使用 XAML 島在 wpf 應用程式中裝載自訂 uwp 控制項](host-custom-control-with-xaml-islands.md)。

> [!NOTE]
> 只有在 WPF 和以 .NET Core 3 為目標的 Windows Forms 應用程式中，才支援使用**WindowsXamlHost**控制項來裝載自訂 UWP 控制項。 以 .NET Framework 或 .NET Core 3 為目標的應用程式支援裝載 Windows SDK 所提供的第一方 UWP 控制項。

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>將您的專案設定為使用 XAML 島 .NET 控制項

XAML 島 .NET 控制項需要 Windows 10 版本1903或更新版本。 若要使用這些控制項，請安裝以下所列的其中一個 NuGet 套件。 這些套件提供您使用 XAML 島包裝控制項和主控制項所需的所有專案，並包含其他也需要的相關 NuGet 套件。

| 控制項類型 | NuGet 套件  | 相關文章 |
|-----------------|----------------|---------------------|
| [包裝的控制項](#wrapped-controls) | Version 6.0.0-preview7 或更新版本的這些套件： <ul><li>WPF[Microsoft 工具組. UI. 控制項](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms：[Microsoft 工具組. UI. 控制項](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [在 WPF 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands.md)  |
| [主控制項](#host-controls) | Version 6.0.0-preview7 或更新版本的這些套件： <ul><li>WPF[XamlHost 的工具。](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms：[XamlHost 的工具。](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [在 WPF 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands.md)<br/>[在 WPF 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands.md)  |

請注意下列詳細資料：

* 主控控制封裝也會包含在包裝的控制項封裝中。 如果您想要同時使用這兩組控制項，您可以安裝包裝的控制項套件。

* 如果您裝載的是自訂 UWP 控制項，則您的 WPF 或 Windows Forms 專案必須以 .NET Core 3 為目標。 以 .NET Framework 為目標的應用程式不支援裝載自訂 UWP 控制項。 您也必須執行一些額外的步驟來參考自訂控制項。 如需詳細資訊，請參閱[使用 XAML 島在 WPF 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands.md)。

* 這些指示的舊版中，您會將 `maxversiontested` 元素新增至 WPF 或 Windows Forms 專案中的應用程式資訊清單。 只要您使用以上所列的最新預覽版本的 NuGet 套件，就不再需要將此元素新增至資訊清單。

### <a name="architecture-of-xaml-island-net-controls"></a>XAML 島 .NET 控制項的架構

以下將快速瞭解不同類型的 XAML 島控制項在 UWP XAML 裝載 API 之上的組織結構。

![裝載控制項架構](images/xaml-islands/host-controls.png)

隨附於 Windows SDK 的此圖表底部會出現 API。 已包裝的控制項和主控制項可透過 Windows 社區工具組中的 NuGet 套件來取得。

### <a name="web-view-controls"></a>Web view 控制項

Windows 社區工具組也提供下列 .NET 控制項，以便在 WPF 和 Windows Forms 應用程式中裝載 web 內容。 這些控制項通常用於類似的桌面應用程式現代化案例，如同 XAML 島控制項，而且它們會在與 XAML 島控制項相同的[Microsoft 工具](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32)組存放庫中進行維護。

| 控制項 | 支援的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 會使用 Microsoft Edge 轉譯引擎來顯示 web 內容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供與更多作業系統版本相容的**web**程式版本。 此控制項使用 Microsoft Edge 轉譯引擎來顯示 Windows 10 1803 版和更新版本上的 web 內容，以及 Internet Explorer 轉譯引擎，以顯示舊版 Windows 10、Windows 8.x 和 Windows 7 上的 web 內容。 |

若要使用這些控制項，請安裝下列其中一個 NuGet 套件：

* WPF[Microsoft 工具組. UI. 控制項](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms：[Microsoft 工具組. UI. 控制項](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++Win32 應用程式

C++ Win32 應用程式不支援 XAML 島 .net 控制項。 這些應用程式必須改為使用 Windows 10 SDK （版本1903和更新版本）所提供的*UWP XAML 裝載 API* 。

UWP XAML 裝載 API 是由數個 Windows 執行階段類別和 COM 介面所組成C++ ，您的 Win32 應用程式可以用來裝載衍生自[WINDOWS](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 UWP 控制項。 您可以在具有相關聯視窗控制碼（HWND）的應用程式中，將 UWP 控制項裝載于任何 UI 元素中。 如需此 API 的詳細資訊（包括必要條件），請參閱[在C++ Win32 應用程式中使用 UWP XAML 裝載 API](using-the-xaml-hosting-api.md)。

> [!NOTE]
> Windows 社區工具組中的已包裝控制項和主控制項，會在內部使用 UWP XAML 裝載 API，並在您直接使用 UWP XAML 裝載 API （包括鍵盤導覽）的情況下，執行您需要自行處理的所有行為。和版面配置變更。 對於 WPF 和 Windows Forms 應用程式，我們強烈建議您直接使用這些控制項，而不是 UWP XAML 裝載 API，因為它們會抽象化許多使用 API 的執行詳細資料。

## <a name="feature-roadmap"></a>功能藍圖

隨著 Windows 10 版本1903的發行，Windows 社區工具組中的 XAML 島 .NET 控制項仍處於開發人員預覽階段，直到有版本1.0 版的控制項可用為止。

* .NET Framework 4.6.2 和更新版本之控制項的1.0 版已計畫在[6.0 版的工具](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)組中發行。
* .NET Core 3 的控制項版本1.0 已規劃為較新版本的工具組。
* 如果您想要針對 .NET Framework 和 .NET Core 3 嘗試這些控制項1.0 版的最新預覽，請參閱[UWP 社區工具](https://dotnet.myget.org/gallery/uwpcommunitytoolkit)組資源庫中的 6.0.0-preview7 （或更新版本） NuGet 套件。

如需詳細資訊，請參閱 <<c0> [ 此部落格文章](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)。

## <a name="additional-resources"></a>其他資源

如需更多有關使用 XAML Islands 的背景資訊和教學課程，請參閱下列文章和資源：

* [將 WPF 應用程式的現代化教學](modernize-wpf-tutorial.md)課程：本教學課程提供逐步指示，說明如何使用 Windows 社區工具組中的包裝控制項和主控制項，將 UWP 控制項新增至現有的 WPF 企業營運應用程式。 本教學課程包含 WPF 應用程式的完整程式碼，以及處理常式中每個步驟的詳細指示。
* [XAML Islands v1-更新與藍圖](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)：此 blog 文章討論許多有關 XAML Islands 的常見問題，並提供詳細的開發藍圖。
