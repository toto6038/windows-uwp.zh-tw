---
description: 本指南可協助您直接在 WPF 和 Windows Forms 應用程式中建立 Fluent 型 UWP UI
title: 桌面應用程式中的 UWP 控制項
ms.date: 07/26/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml 群島
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 765fefa0b489e1620d7a37fe75acd02acb8d5ae8
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729471"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>在桌面應用程式中裝載 UWP XAML 控制項 (XAML 島)

從 Windows 10 版本1903開始, 您可以使用稱為*XAML Islands*的功能, 將 uwp 控制項裝載在非 UWP 桌面應用程式中。 這項功能可讓您使用僅透過 UWP 控制項提供的最新 Windows 10 UI 功能, 來增強現有桌面應用程式的外觀、風格和功能。 這表示您可以在現有的 WPF、Windows Forms 和C++ Win32 應用程式中使用 UWP 功能 (例如[Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)和控制項), 以支援流暢的[設計系統](/windows/uwp/design/fluent-design-system/index)。

我們會根據您所使用的技術或架構, 提供數種方式C++來使用您 WPF、Windows Forms 和 Win32 應用程式中的 XAML 島。 

> [!NOTE]
> 如果您有關于 XAML Islands 的意見反應, 請在[Microsoft 工具](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)組的 Win32 存放庫中建立新的問題, 並在該處留下您的意見。 如果您想要私下提交意見反應, 您可以將它傳送XamlIslandsFeedback@microsoft.com給。 您的深入解析和案例對我們而言非常重要。

## <a name="how-do-xaml-islands-work"></a>XAML 孤島如何正常執行？

從 Windows 10 版本1903開始, 我們提供兩種方式, 可讓您在 WPF、Windows Forms 和C++ Win32 應用程式中使用 XAML 孤島:

* Windows SDK 提供數個 Windows 執行階段類別和 COM 介面, 讓您的應用程式可以用來裝載衍生自[**Windows**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 UWP 控制項。 這些類別和介面統稱為*UWP XAML 裝載 API*, 可讓您在應用程式中具有相關聯的視窗控制碼 (HWND) 的任何 UI 元素中裝載 UWP 控制項。 如需此 API 的詳細資訊, 請參閱[使用 XAML 裝載 API](using-the-xaml-hosting-api.md)。

* [Windows 社區工具](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)組也提供適用于 WPF 和 Windows Forms 的其他 XAML 島控制項。 如果您直接使用 UWP XAML 裝載 API, 包括鍵盤導覽和版面配置變更, 這些控制項就會在內部使用 UWP XAML 裝載 API, 並實作為您必須自行處理的所有行為。 對於 WPF 和 Windows Forms 應用程式, 我們強烈建議您直接使用這些控制項, 而不是 UWP XAML 裝載 API, 因為它們會抽象化許多使用 API 的執行詳細資料。 請注意, 從 Windows 10 1903 版, 這些控制項是以[開發人員預覽](#feature-roadmap)的形式提供。

> [!NOTE]
> C++Win32 桌面應用程式必須使用 UWP XAML 裝載 API 來裝載 UWP 控制項。 Windows 社區工具組中的 XAML 島控制項無法用於C++ Win32 桌面應用程式。

WPF 和 Windows Forms 應用程式的 Windows 社區工具組提供兩種類型的 XAML 島控制項: 已*包裝的控制項*和*主控制項*。 

### <a name="wrapped-controls"></a>包裝的控制項

WPF 和 Windows Forms 應用程式可以在[Windows 社區工具](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)組中使用已包裝 UWP 控制項的選取專案。 這些稱為*包裝的控制項*, 因為它們會包裝特定 UWP 控制項的介面和功能。 您可以將這些控制項直接加入至 WPF 或 Windows Forms 專案的設計介面, 然後在設計工具中使用它們, 就像任何其他 WPF 或 Windows Forms 控制項一樣。

下列用於實現 XAML 島的已包裝 UWP 控制項目前適用于 WPF (請參閱[microsoft. 工具組. ui. 控制項](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)套件) 和 Windows Forms 應用程式 (請參閱[microsoft 工具組. ui. 控制項](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)套件)).

| 控制項 | 支援的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 (版本 1903) | 在您的 Windows Forms 或 WPF 桌面應用程式中, 為 Windows 筆墨型使用者互動提供介面和相關的工具列。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 (版本 1903) | 內嵌在 Windows Forms 或 WPF 桌面應用程式中串流和轉譯媒體內容 (例如影片) 的視圖。 |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 (版本 1903) | 可讓您在 Windows Forms 或 WPF 桌面應用程式中顯示符號或照片的地圖。 |

除了 XAML 島的已包裝控制項之外, Windows 社區工具組也提供下列控制項, 以便在 WPF 中裝載 web 內容 (請參閱[Microsoft 工具組.](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView) ) 和 Windows Forms 應用程式 (請參閱[Microsoft. 工具組. UI. Controls. web](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)工作套件)。

| 控制項 | 支援的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 會使用 Microsoft Edge 轉譯引擎來顯示 web 內容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供與更多作業系統版本相容的**web**程式版本。 此控制項使用 Microsoft Edge 轉譯引擎來顯示 Windows 10 1803 版和更新版本上的 web 內容, 以及 Internet Explorer 轉譯引擎, 以顯示舊版 Windows 10、Windows 8.x 和 Windows 7 上的 web 內容。 |

### <a name="host-controls"></a>主控制項

除了可用包裝控制項所涵蓋的案例之外, WPF 和 Windows Forms 應用程式也可以使用[Windows 社區工具](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)組中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項。 這個控制項可以裝載衍生自[**Windows**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 uwp 控制項, 包括 Windows SDK 所提供的任何 uwp 控制項, 以及自訂使用者控制項。 此控制項支援 Windows 10 Insider Preview SDK 組建17709和更新版本。

這些控制項可在[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (適用于 Wpf) 和[XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (適用于 Windows Forms) 套件中取得。 (針對)。 這些套件包含在包含已包裝控制項的 [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)和[Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)。

### <a name="architecture-overview"></a>架構概觀

以下可快速查看這些控制項在架構上的組織方式。

![裝載控制項架構](images/xaml-islands/host-controls.png)

隨附於 Windows SDK 的此圖表底部會出現 API。 已包裝的控制項和主控制項可透過 Windows 社區工具組中的 Nuget 套件來取得。

<span id="requirements" />

## <a name="configure-your-project-to-use-xaml-islands"></a>將您的專案設定為使用 XAML 群島

XAML Islands 需要 Windows 10, 版本1903和更新版本。 若要在您的應用程式中使用 XAML Islands, 您必須先設定您的專案:

1. 修改您的專案, 以使用 Windows 執行階段 Api。 如需指示, 請參閱[這篇文章](desktop-to-uwp-enhance.md#set-up-your-project)。
2. 在您的專案中安裝其中一個 NuGet 套件。 請確定您安裝的是版本 6.0.0-preview7 或更新版本的套件。
    * WPF安裝[Microsoft 工具組. UI. 控制項](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)
    * Windows Forms:[Microsoft 工具組. UI. 控制項](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)
    * C++32[XamlApplication 的使用者。](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)

> [!NOTE]
> 這些指示的舊版中, 您會將**maxversiontested**元素新增至專案中的應用程式資訊清單。 在最新的 NuGet 套件預覽版本中, 您不再需要將此元素新增至資訊清單。

## <a name="feature-roadmap"></a>功能藍圖

隨著 Windows 10 版本1903的發行, Windows 社區工具組中的已包裝控制項和主控制項仍處於開發人員預覽狀態, 直到有版本1.0 版本的控制項可用為止。

* .NET Framework 4.6.2 和更新版本之控制項的1.0 版已計畫在[6.0 版的工具](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)組中發行。
* .NET Core 3 的控制項版本1.0 已規劃為較新版本的工具組。
* 如果您想要針對 .NET Framework 和 .NET Core 3 嘗試這些控制項1.0 版的最新預覽, 請參閱[UWP 社區工具](https://dotnet.myget.org/gallery/uwpcommunitytoolkit)組資源庫中的**6.0.0-preview7** NuGet 套件。

如需詳細資訊，請參閱 <<c0> [ 此部落格文章](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)。

## <a name="additional-resources"></a>其他資源

如需更多有關使用 XAML Islands 的背景資訊和教學課程, 請參閱下列文章和資源:

* [將 WPF 應用程式的現代化教學](modernize-wpf-tutorial.md)課程:本教學課程提供逐步指示, 說明如何使用 Windows 社區工具組中的包裝控制項和主控制項, 將 UWP 控制項新增至現有的 WPF 企業營運應用程式。 本教學課程包含 WPF 應用程式的完整程式碼, 以及處理常式中每個步驟的詳細指示。
* [XAML Islands v1-更新與藍圖](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap):此 blog 文章討論許多有關 XAML Islands 的常見問題, 並提供詳細的開發藍圖。
