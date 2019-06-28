---
description: 本指南可協助您直接在 WPF 和 Windows Forms 應用程式中建立 Fluent 型 UWP UI
title: 桌面應用程式中的 UWP 控制項
ms.date: 04/16/2019
ms.topic: article
keywords: windows 10、 uwp、 windows form、 wpf、 xaml 群島
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: e6074202a05c80a9dc759cdf81b2c20c7cc17d07
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414095"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>主機 UWP XAML 控制項在傳統型應用程式 （XAML 群島）

從 Windows 10 版本 1903，您可以裝載在非 UWP 桌面應用程式使用稱為 UWP 控制項*XAML 群島*。 這項功能可讓您加強的外觀、 風格和您現有的傳統型應用程式最新的 Windows 10 UI 功能才可透過 UWP 控制項的功能。 這表示您可以使用 UWP 功能這類[Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)並控制該支援[Fluent Design System](/windows/uwp/design/fluent-design-system/index)在您現有的 WPF、 Windows Form 和C++Win32 應用程式。

我們提供數種方式使用您的 WPF、 Windows Form 中的 XAML 群島和C++Win32 應用程式的技術或您使用的架構而定。 

> [!NOTE]
> 如果您有意見反應的相關 XAML 群島，建立新的問題，在[Microsoft.Toolkit.Win32 存放庫](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)和那里留下您的意見。 如果您想要私下提交您的意見反應，您可以傳送到XamlIslandsFeedback@microsoft.com。 您的深入解析和案例會對我們非常重要。

## <a name="how-do-xaml-islands-work"></a>XAML 群島如何運作？

從 Windows 10 版本 1903，我們提供兩種方式可使用您的 WPF、 Windows Form 中的 XAML 群島和C++Win32 應用程式：

* Windows SDK 提供數個 Windows 執行階段類別和 COM 介面，您的應用程式可以用來裝載任何 UWP 控制項衍生自[ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)。 整體而言，這些類別和介面統稱*UWP XAML 裝載 API*，可讓您在有相關聯的視窗控制代碼 (HWND) 的應用程式中的任何 UI 項目中的主機 UWP 控制項。 如需有關此 API 的詳細資訊，請參閱 <<c0> [ 使用裝載 API XAML](using-the-xaml-hosting-api.md)。

* [Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)也提供適用於 WPF 和 Windows Forms 的 其他 XAML 島控制項。 這些控制項使用 UWP XAML，內部裝載 API，並實作的所有行為，您必須自行處理，如果您使用裝載 API，包括鍵盤導覽和版面配置變更 UWP XAML。 WPF 和 Windows Forms 應用程式，我們強烈建議您使用這些控制項，而不是 UWP XAML 直接裝載 API，因為其抽象許多使用 API 的實作詳細資料。 請注意，從 Windows 10 版本 1903，這些控制項可[可供開發人員預覽](#feature-roadmap)。

> [!NOTE]
> C++Win32 桌面應用程式必須使用裝載 API 來裝載 UWP 控制項 UWP XAML。 Windows 社群工具組中的 XAML 島控制項沒有可用的C++Win32 桌面應用程式。

有兩種類型的 WPF 和 Windows Form 應用程式針對 Windows 社群工具組所提供的 XAML 島控制項：*包裝控制項*並*主控制項*。 

### <a name="wrapped-controls"></a>已包裝的控制項

WPF 和 Windows Forms 應用程式可以使用選取的已包裝的 UWP 控制項中[Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 這些稱為*包裝控制項*因為它們所包裝的介面和特定的 UWP 控制項的功能。 您可以將這些控制項直接加入 WPF 或 Windows Form 專案的設計介面，然後依照像任何其他 WPF 或 Windows Form 控制項設計工具中使用它們。

下列實作 XAML 島已包裝的 UWP 控制項是目前適用於 WPF 和 Windows Form 應用程式。

| 控制項 | 支援的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 (版本 1903) | 提供在 Windows Form 或 WPF 桌面應用程式中的 Windows Ink 為基礎的使用者互動的介面和相關的工具列。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 (版本 1903) | 將內嵌資料流，並呈現媒體內容，例如視訊在 Windows Form 或 WPF 桌面應用程式中的檢視。 |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 (版本 1903) | 可讓您在 Windows Form 或 WPF 桌面應用程式中顯示的符號或逼真的對應。 |

XAML 島已包裝控制項，除了 Windows 社群工具組也提供下列控制項來裝載 web 內容。

| 控制項 | 支援的最低 OS | 描述 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 您可以使用 Microsoft Edge 轉譯引擎來顯示 web 內容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供的版本**WebView**與更多的 OS 版本相容。 此控制項會使用 Microsoft Edge 轉譯引擎，以顯示在 Windows 10 在版本 1803年和更新版本上的 web 內容和 Internet Explorer 轉譯引擎，以顯示 web 內容在較早版本的 Windows 10，Windows 8.x 和 Windows 7。 |

### <a name="host-controls"></a>主控制項

如需可用的已包裝控制項所未包含的案例，WPF 和 Windows Form 應用程式也可以使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制中[Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)。 這個控制項可以裝載任何 UWP 控制項衍生自[ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括任何由 Windows SDK 所提供的 UWP 控制項，以及自訂使用者控制項。 此控制項支援 Windows 10 Insider Preview SDK 建置 17709 和更新版本。

### <a name="architecture-overview"></a>架構概觀

以下可快速查看這些控制項在架構上的組織方式。

![裝載控制項架構](images/xaml-islands/host-controls.png)

隨附於 Windows SDK 的此圖表底部會出現 API。 已包裝的控制項和主控制項可透過 Windows 社群工具組中的 Nuget 套件。

## <a name="requirements"></a>需求

XAML 群島需要 Windows 10 版本 1903，及更新版本。 若要在您的應用程式中使用 XAML 群島，您必須先設定您的專案。

### <a name="step-1-modify-your-project-to-use-windows-runtime-apis"></a>步驟 1：將專案修改為使用 Windows 執行階段 Api

如需相關指示，請參閱 <<c0> [ 這篇文章](desktop-to-uwp-enhance.md#set-up-your-project)。

### <a name="step-2-enable-xaml-island-support-in-your-project"></a>步驟 2：在您的專案中啟用 XAML 島支援

進行下列變更的其中一個專案，讓 XAML 島支援。 如需詳細資訊，請參閱 <<c0> [ 此部落格文章](https://techcommunity.microsoft.com/t5/Windows-Dev-AppConsult/Using-XAML-Islands-on-Windows-10-19H1-fixing-the-quot/ba-p/376330#M117)。

#### <a name="option-1-package-your-application-in-an-msix-package"></a>選項 1：MSIX 封裝中的應用程式封裝  

安裝 Windows 10，版本 1903 SDK （或更新版本）。 然後，藉由新增封裝 MSIX 封裝中的應用程式[Windows 應用程式封裝專案](https:/docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)到您的解決方案，並將參考加入您的 WPF 或 Windows Form 專案。

#### <a name="option-2-set-the-maxversiontested-value-in-your-assembly-manifest"></a>選項 2：設定您的組件資訊清單中 maxVersionTested 值

如果您不想在 MSIX 封裝中的應用程式封裝，您可以新增[並排顯示組件資訊清單](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)至您的專案，並新增**maxVersionTested**值來指定資訊清單程式應用程式是與 Windows 10 版本 1903年或更新版本相容。

1. 如果您還沒有組件資訊清單中您的專案，專案中加入新的 XML 檔案並將它命名**app.manifest**。 針對 WPF 或 Windows Form 應用程式，請確定您也將指派**Manifest**屬性設 **。 app.manifest**中**應用程式**頁面您[專案屬性](https://docs.microsoft.com/visualstudio/ide/reference/application-page-project-designer-csharp?view=vs-2019#resources)。
2. 在您的組件資訊清單，包括**相容性**項目和子項目，在下列範例所示。 取代**識別碼**屬性**maxVersionTested**與您設為目標的 Windows 10 的版本號碼的項目 （必須是 Windows 10 版本 1903 或更新版本）。 

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

## <a name="feature-roadmap"></a>功能藍圖

在版本的 Windows 10 版本 1903，包裝的控制項和 Windows 社群工具組中的主控制項是仍在開發人員指南預覽中有可用的控制項的 1.0 版發行為止。

* 1\.0 版的.NET Framework 4.6.2 中的控制項和更新版本已計劃將於發行[6.0 版的工具組](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones)。
* 以較新版的工具組 1.0 版的.NET Core 3 的控制項已計劃。
* 如果您想要嘗試最新預覽版本 1.0 發行版本，這些控制項的.NET Framework 和.NET Core 3，請參閱**6.0.0-preview3** NuGet 套件中[UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit)資源庫。

如需詳細資訊，請參閱 <<c0> [ 此部落格文章](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)。

## <a name="additional-resources"></a>其他資源

如需詳細背景資訊和教學課程，需使用 XAML 群島，請參閱下列文章和資源：

* [現代化的 WPF 應用程式教學課程](modernize-wpf-tutorial.md):本教學課程提供在 Windows 社群工具組中使用的已包裝的控制項和主控制項，將 UWP 控制項新增至現有的 WPF 特定業務應用程式的逐步指示。 本教學課程包含程序中的 WPF 應用程式的完整程式碼，以及每個步驟的詳細的指示。
* [XAML 群島 v1-更新和藍圖](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap):此部落格文章會討論有關 XAML 群島的許多常見問題，並提供詳細的開發藍圖。
