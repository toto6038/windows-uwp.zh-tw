---
title: WinUI 3 預覽版 2 (2020 年 7 月)
description: WinUI 3 預覽版 2 的概觀。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: b7336aa054bac4c59cd535951cc3fc92d4a3486a
ms.sourcegitcommit: aa88679989ef3c8b726e1bf5a0ed17c1206a414f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687774"
---
# <a name="windows-ui-library-3-preview-2-july-2020"></a>Windows UI 程式庫 3 預覽版 2 (2020 年 7 月)

Windows UI 程式庫 (WinUI) 3 是同時適用於 Windows 桌面和 UWP 應用程式的原生使用者體驗 (UX)。

**WinUI 3 預覽版 2** 是品質和穩定性驅動的版本，著重於修正預覽版 1 的錯誤和已知問題。

**請參閱 [預覽版 2 的限制和已知問題](#preview-2-limitations-and-known-issues)** 。

> [!Important]
> WinUI 3 預覽版的目的是進行早期評估，並從開發人員社群收集意見反應。 **不會** 用於生產應用程式。
>
> 我們會繼續從 2020 年到 2021 年初提供 WinUI 3 的預覽版本，在這個日期之後，第一個正式版本就會推出。
>
> 請使用 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml)以提供意見反應及記錄建議和問題。

## <a name="install-winui-3-preview-2"></a>安裝 WinUI 3 預覽版 2

WinUI 3 預覽版 2 包含 Visual Studio 專案範本，可協助您開始使用以 WinUI 為基礎的使用者介面以及包含 WinUI 程式庫的 NuGet 套件，來建置應用程式。 若要安裝 WinUI 3 預覽版 2，請遵循下列步驟。

> [!NOTE]
> 您也可以複製 WinUI 3 預覽版 2 版本的 [XAML 控制項資源庫](#xaml-controls-gallery-winui-3-preview-2-branch)，並加以建置。

1. 確定您的開發電腦已安裝 Windows 10 1803 (組建 17134) 或較新版本。

2. 安裝 [Visual Studio 2019 (16.7.2 版本)](https://visualstudio.microsoft.com/vs/)

    安裝 Visual Studio 時，您必須包含下列工作負載：
    - .NET 桌面開發
    - 通用 Windows 平台開發

    若要建立 C++ 應用程式，您也必須包含下列工作負載：
    - 使用 C++ 的傳統型開發
    - *C++ (v142) 通用 Windows 平台工具* ，通用 Windows 平台工作負載的選用元件 (請參閱右側窗格中 [通用 Windows 平台開發] 區段底下的 [安裝詳細資料])

    安裝 Visual Studio 之後，請確保在程式中啟用 .NET 預覽： 
    - 移至 [工具] > [選項] > [預覽功能] > 選取 [使用 .NET Core SDK 的預覽 (需要重新開機)]。 
    
3. 請確定您的系統已針對 **nuget.org** 啟用 NuGet 套件來源。如需詳細資訊，請參閱[常見的 NuGet 設定](/nuget/consume-packages/configuring-nuget-behavior)。

4. 如果想要建立適用於 C#/.NET 5 和 C++/Win32 應用程式的桌面 WinUI 專案，則必須同時安裝 x64 和 x86 版本的 .NET 5 預覽版 5。 **請注意，.NET 5 預覽版 5 目前僅支援 WinUI 3 的 .NET 5 預覽** ：

    - [適用於 .NET 5 Preview 5 的 x64 安裝程式](https://dotnet.microsoft.com/download/dotnet/thank-you/sdk-5.0.100-preview.5-windows-x64-installer)
    - [適用於 .NET 5 Preview 5 的 x86 安裝程式](https://dotnet.microsoft.com/download/dotnet/thank-you/sdk-5.0.100-preview.5-windows-x86-installer)

5. 下載及安裝 [WinUI 3 預覽版 2 VSIX 套件](https://aka.ms/winui3/previewdownload)。 此 VSIX 套件會將 WinUI 3 專案範本和 NuGet 套件 (包含 WinUI 3 程式庫) 新增至 Visual Studio 2019。

    如需如何將 VSIX 套件新增至 Visual Studio 的指示，請參閱[尋找和使用 Visual Studio 擴充功能](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)。


## <a name="create-winui-projects"></a>建立 WinUI 專案

安裝 WinUI 3 預覽版 2 VSIX 套件之後，您就可以在 Visual Studio 中使用其中一個 WinUI 專案範本來建立新的專案。 若要在 [建立新專案] 對話方塊中存取 WinUI 專案範本，請將語言篩選為 **C++** 或 **C#** ，將平台篩選為 **Windows** ，以及將專案類型篩選為 **WinUI** 。 或者，您可以搜尋「WinUI」並選取其中一個可用的 C# 或 C++ 範本。

![WinUI 專案範本](images/winui-projects-csharp.png)

如需開始使用 WinUI 專案範本的詳細資訊，請參閱下列文章：

- [開始使用適用於桌面應用程式的 WinUI 3](get-started-winui3-for-desktop.md)
- [開始使用適用於 UWP 應用程式的 WinUI 3](get-started-winui3-for-uwp.md)

除了[限制和已知問題](#preview-2-limitations-and-known-issues)之外，使用 WinUI 專案建置應用程式類似於使用 XAML 和 WinUI 2.x 建置 UWP 應用程式。 因此，適用於 UWP 應用程式的指引和文件，以及 Windows SDK 中的 **Windows.UI** WinRT 命名空間都適用。

如果您使用 WinUI 3 預覽版 1 建立專案，您可以將專案升級為使用預覽版 2。 在 [GitHub 存放庫](https://aka.ms/winui3/upgrade-instructions)上參閱詳細指示。

### <a name="project-templates-for-winui-3"></a>WinUI 3 的專案範本

您可以使用這些 WinUI 專案範本來建立應用程式。

| 範本 | Language | 說明 |
|----------|----------|-------------|
| 已封裝的空白應用程式 (WinUI in Desktop) | C# 和 C++ | 使用以 WinUI 為基礎的使用者介面，建立桌面 .NET 5 (C#) 或原生 Win32 (C++) 應用程式。 產生的專案包含一個基本視窗，該視窗是從 WinUI 程式庫中的 **Microsoft.UI.Xaml.Window** 類別衍生，您可以用來開始建置您的 UI。 如需此專案類型的詳細資訊，請參閱[開始使用適用於桌面應用程式的 WinUI 3](get-started-winui3-for-desktop.md)。<p></p>解決方案也包含 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建置到 [MSIX 套件](/windows/msix/overview)。 這可提供新式部署體驗，透過套件擴充功能與 Windows 10 功能整合的能力，還有更多功能。  |
| 空白應用程式 (WinUI in UWP)  | C# 和 C++ | 建立具有以 WinUI 為基礎的使用者介面的 UWP 應用程式。 產生的專案包含一個基本頁面，該頁面是從 WinUI 程式庫中的 **Microsoft.UI.Xaml.Controls.Page** 類別衍生，您可以用來開始建置您的 UI。 如需此專案類型的詳細資訊，請參閱[開始使用適用於 UWP 應用程式的 WinUI 3](get-started-winui3-for-uwp.md)。 |

您可以使用這些 WinUI 專案範本來建置可供以 WinUI 為基礎的應用程式載入及使用的元件。

| 範本 | Language | 說明 |
|----------|----------|-------------|
| 類別庫 (WinUI in Desktop) | 僅限 C# | 在 C# 中建立 .NET 5 受控類別庫 (DLL)，可供具有以 WinUI 為基礎的使用者介面的其他 .NET 5 桌面應用程式使用。  |
| 類別庫 (WinUI in UWP)  | 僅限 C# | 在 C# 中建立受控類別程式庫 (DLL)，可供具有以 WinUI 為基礎的使用者介面的其他 UWP 應用程式使用。 |
| Windows 執行階段元件 (WinUI in UWP) | C# 和 C++ | 建立以 C# 或 C++/WinRT 撰寫的 [Windows 執行階段元件](/windows/uwp/winrt-components/)，可以由具有以 WinUI 為基礎的使者介面的任何 UWP 應用程式取用，不論該應用程式是以何種程式設計語言撰寫。 |

### <a name="item-templates-for-winui-3"></a>WinUI 3 的項目範本

您可以在 WinUI 專案中使用下列項目範本。 若要存取這些 WinUI 專案範本，請以滑鼠右鍵按一下 [方案總管] 中的專案節點，選取 [新增]  ->  [新項目]，然後按一下 [新增新項目] 對話方塊中的 [WinUI]。

![WinUI 項目範本](images/winui-items-csharp.png)

| 範本 | Language | 說明 |
|----------|----------|-------------|
| 空白頁面 (WinUI) | C# 和 C++ | 新增 XAML 檔案和程式碼檔案，該檔案會定義衍生自 WinUI 程式庫中 **Microsoft.UI.Xaml.Controls.Page** 類別的新頁面。 |
| 空白視窗 (WinUI in Desktop) | C# 和 C++ | 新增 XAML 檔案和程式碼檔案，該檔案會定義衍生自 WinUI 程式庫中 **Microsoft.UI.Xaml.Window** 類別的新視窗。 |
| 自訂控制項 (WinUI) | C# 和 C++ | 新增程式碼檔案，以使用預設樣式建立樣板化控制項。 樣板化控制項衍生自 WinUI 程式庫中的 **Microsoft.UI.Xaml.Controls.Control** 類別。<p></p>如需示範如何使用此項目範本的逐步解說，請參閱 [UWP 和 WinUI 3 應用程式的樣板化 XAML 控制項 (使用 C++/WinRT)](xaml-templated-controls-cppwinrt-winui-3.md) 和 [UWP 和 WinUI 3 應用程式的樣板化 XAML 控制項 (使用 C#)](xaml-templated-controls-csharp-winui-3.md)。 如需樣板化控制項的詳細資訊，請參閱[自訂 XAML 控制項](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |
| 資源字典 (WinUI) | C# 和 C++ | 新增 XAML 資源的空白索引鍵集合。 如需詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)。 |
| 資源檔 (WinUI) | C# 和 C++ | 新增檔案，用於儲存應用程式的字串和條件式資源。 您可以使用此項目來協助將您的應用程式當地語系化。 如需詳細資料，請參閱[將您 UI 和應用程式套件資訊清單中的字串當地語系化](/windows/uwp/app-resources/localize-strings-ui-manifest)。 |
| 使用者控制項 (WinUI) | C# 和 C++ | 新增 XAML 檔案和程式碼檔案，以建立衍生自 WinUI 程式庫中 **Microsoft.UI.Xaml.Controls.UserControl** 類別的使用者控制項。 一般而言，使用者控制項會封裝相關的現有控制項，並提供自己的邏輯。<p></p>如需使用者控制項的詳細資訊，請參閱[自訂 XAML 控制項](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |

## <a name="bug-fixes-and-other-improvements-in-winui-3-preview-2"></a>WinUI 3 預覽版 2 中的錯誤修正和其他改良功能

這是適用於預覽版 2 的錯誤修正和其他更新完整清單。 如需此版本中已解決的最重要錯誤修正清單，請參閱我們的[版本公告](https://aka.ms/winui3/preview2-announcement)。

> [!NOTE]
> WinUI 3 預覽版 2 使用 WinUI 2 程式庫的 2.4.2 版。 

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) 和 [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged) 現在會如預期般的在 C# 桌面應用程式中運作
  - 這會清除在後端中更新時未在 UI 中更新的集合控制項相關其他問題。
  - *感謝 @hshristov 在 GitHub 上提出 [類似的問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2490)！*
- 預覽版 2 現在與適用於桌面應用程式的 [.NET 5 預覽版 5](/dotnet/api/?view=net-5.0&preserve-view=true) 相容
- WinUI 3 現在與 [WinUI 2.4](../winui2/release-notes/winui-2.4.md) 同位，其中包含新的控制項和功能，例如[階層式 NavigationView](../winui2/release-notes/winui-2.4.md#hierarchical-navigation) 和 [ProgressRing](../winui2/release-notes/winui-2.4.md#progressring)。
- 損毀已修正：搭配觸控使用 [TabView](/windows/uwp/design/controls-and-patterns/tab-view)
- [XAML 控制項庫範例](#xaml-controls-gallery-winui-3-preview-2-branch)中的 [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) 現在使用左側模式，而不是左側精簡模式
- 損毀已修正：在輸入驗證和 [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) 中輸入太快
  - *感謝 @paulovilla 在 GitHub 上提出 [這個問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2563)！*
- 損毀已修正：當 [TextBox](/windows/uwp/design/controls-and-patterns/text-box) 功能表開啟時，與 XAML UI 互動
- 在瀏覽至多個頁面之後，[XAML 控制項庫範例](#xaml-controls-gallery-winui-3-preview-2-branch)標題文字已不再變得混亂
- 搭配觸控使用 [WebView2](/microsoft-edge/webview2/) 不再讓您在位置上有些微位移
- WinUIEdit.dll 中的類別已從 Windows.UI.Text 命名空間移至 Microsoft.UI.Text 命名空間
- 損毀已修正：在多重選取模式 (Windows 10 版本 1803) 中選取 [TreeView](/windows/uwp/design/controls-and-patterns/tree-view) 中的項目
- 點、矩形和大小成員現在會在適用於桌面應用程式的 API C# 投影中進行雙重輸入。
  - *感謝 @dotMorten 在 GitHub 上提出 [這個問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2474)！*
- 損毀已修正：搭配 .rtf 檔案使用 [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box)
- [TabView](/windows/uwp/design/controls-and-patterns/tab-view) 關閉按鈕不會再有空白的工具提示
- [影像](/windows/uwp/design/controls-and-patterns/images-imagebrushes)控制項現在會正確呈現 SVG 檔案
  - *感謝 @mqudsi 在 GitHub 上提出 [這個問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2565)！*
- 損毀已修正：使用/瀏覽至 Page 元素
- 使用觸控來選取 [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) 中的項目，現在會取消選取所有其他項目 (在單一選取模式中)
- 損毀已修正：不會再發生由於明確設定大小[滑桿](/windows/uwp/design/controls-and-patterns/slider)控制項上設定的值所造成的 LayoutSliderException 
  - *感謝 @hig-dev 在 GitHub 上提出 [這個問題](https://github.com/microsoft/microsoft-ui-xaml/issues/477)！*
- 損毀已修正：使用 [ColorPicker](/windows/uwp/design/controls-and-patterns/color-picker) 會在關機時造成損毀
- 損毀已修正：使用 [Pivot](/windows/uwp/design/controls-and-patterns/pivot) 會在關機時造成損毀
- 損毀已修正：[NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) 因為 Windows 10 版本 1803 中遺失資源而造成損毀
- 損毀已修正：專注於 [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) 自訂編輯器 
- 損毀已修正：[SemanticZoom](/windows/uwp/design/controls-and-patterns/semantic-zoom) 
- 繫結現在會如預期般在具有隱含 Mode=OneWay 的標記中運作
  - *感謝 @tomasfabian 在 GitHub 上提出 [這個問題](https://github.com/microsoft/microsoft-ui-xaml/issues/2630)！*
- 已修正動畫：[XAML 控制項庫範例](#xaml-controls-gallery-winui-3-preview-2-branch)中的新功能

## <a name="new-features-and-capabilities-introduced-in-winui-3-preview-1"></a>WinUI 3 預覽版 1 中引進的新特性和功能

下列功能是在 WinUI 3 預覽版 1 中引進，並繼續在 WinUI 3 預覽版 2 中受支援。

- 能夠使用 WinUI 建立桌面應用程式，包括適用於 Win32 應用程式的 [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0)
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView 更新](/windows/uwp/design/controls-and-patterns/tab-view)
- 深色主題更新
- [WebView2](/microsoft-edge/hosting/webview2) 的改良和更新
  - 高 DPI 的支援
  - 支援視窗的大小調整和移動
  - 已更新為以較新版本的 Edge 為目標
  - 不再需要參考 WebView2 專屬的 Nuget 套件
- SwapChainPanel
- 遷移開放原始碼所需的改良功能

如需有關 WinUI 3 和 WinUI 藍圖優點的詳細資訊，請參閱 GitHub 上的 [Windows UI 程式庫藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

### <a name="provide-feedback-and-suggestions"></a>提供意見反應和建議

我們歡迎您在 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) 中提供意見反應。

## <a name="preview-2-limitations-and-known-issues"></a>預覽版 2 的限制和已知問題

預覽版 2 版本僅供預覽。 特別是與桌面版 Win32 應用程式相關的案例都是新項目。 請預期會有錯誤、限制和其他問題。

下列項目是 WinUI 3 預覽版 2 的一些已知問題。 如果您發現下面未列出的問題，請在 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中參與現有問題或提出新問題，來讓我們知道。

### <a name="platform-and-os-support"></a>平台和 OS 支援

WinUI 3 預覽版 2 可與執行 Windows 10 2018 年 4 月更新 (版本 1803 - 組建 17134) 和更新版本的電腦相容。

### <a name="developer-tools"></a>開發人員工具

- 目前僅支援 C# 和 C++/WinRT 應用程式
- 桌面應用程式支援 .NET 5 和 C# 8，而且必須加以封裝
- UWP 應用程式支援 .NET Native 和 C# 7.3
- Intellisense 不完整
- 沒有視覺化設計工具
- 沒有熱重載
- 沒有即時視覺化樹狀結構
- 尚不支援使用 VS Code 進行開發
- 不支援新的 C++/CX 應用程式，不過，您現有的應用程式仍可繼續運作 (請盡速移至 C++/WinRT)
- WinUI 3 內容只能出現在每個程序的一個視窗中，或每個應用程式的一個 ApplicationView 中
- 不支援未封裝的桌面部署
- 沒有 ARM64 支援
- UWP 應用程式中的 C# 自訂控制項：`Themes/Generic.xaml` 不會自動產生。 若要解決這個問題，您可以在類別中手動建立 [佈景主題] 資料夾，並且將 XAML 檔案放在該資料夾內部，將檔案稱為 `Generic.xaml`。
- 將 WinUI 自訂控制項新增至您的專案之後，您的檔案可能會遺失 "CustomControl" 標頭。 若要解決這個問題，您可以手動將該標頭新增至您的 `pch.h` 檔案。
- 新增 DataGrid、其他 Windows 社群工具組控制項和第三方程式庫控制項，可能會導致組建失敗。 若要解決這個問題，請將這個合併的字典新增至您的 `App.xaml` 檔案：
  ```xaml
  <ResourceDictionary Source="ms-appx:///<library_name>/Themes/Generic.xaml"/>
  ```

### <a name="missing-platform-features"></a>缺少平台功能

- Xbox 支援
- HoloLens 支援
- 視窗型快顯
- 筆跡支援
- 背景壓克力
- MediaElement 和 MediaPlayerElement
- RenderTargetBitmap
- MapControl
- SwapChainPanel 不支援透明度
- 全域顯示 (Global Reveal) 會使用回溯行為 (實心筆刷)
- 此版本不支援 XAML Islands
- 第三方生態系統程式庫的功能不完整
- IME 無法運作

### <a name="known-issues"></a>已知問題


- C# UWP 應用程式：

  WinUI 3 架構是一組 WinRT 元件，然而即使 WinRT 具有與 .NET 中類似的類型和物件，它們原本也不是相容的。  C#/WinRT 投影會處理.NET 5 中 .NET 與 WinRT 之間的互通性，讓您可以立即在 .NET 5 應用程式中自由使用 .NET 介面。 
  
  不過，C#/WinRT 無法處理 .NET Native 應用程式中的互通性，因此 WinUI 3 API 會直接在 UWP 應用程式中投影。 因此，您無法再使用這些相同的 .NET 介面。 **一旦 UWP 應用程式不再使用 .NET Native，則這項限制將不再存在** 。

  例如，`INotifyPropertyChanged`API 會投影在桌面應用程式中 WinUI3 的 `System.ComponentModel` 命名空間中，但其會出現在 UWP 應用程式 (和所有 C++ 應用程式) 中 WinUI3 的 `Microsoft.UI.Xaml.Data` 命名空間中。 
  
  此問題適用於：
    - `INotifyPropertyChanged` (和相關的類型)
    - `INotifyCollectionChanged`
    - `ICommand`

> [!Note] 
> 在 `INotifyPropertyChanged` 和 `INotifyCollectionChanged` 無法正常運作時，`ObservableCollection<T>` 類別也會受到影響。 如需實作自己的 `ObservableCollection<T>` 版本範例，請參閱[此範例](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)。 


## <a name="xaml-controls-gallery-winui-3-preview-2-branch"></a>XAML 控制項庫 (WinUI 3 預覽版 2 分支)

如需包含所有 WinUI 3 預覽版 2 控制項和功能的範例應用程式，請參閱 [XAML 控制項資源庫的 WinUI 3 預覽版 2 分支](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)。

![WinUI 3 預覽版 2 XAML 控制項庫應用程式](images/WinUI3XamlControlsGalleryP2.png)<br/>
WinUI 3 預覽版 2 XAML 控制項庫應用程式的範例

若要下載範例，請使用下列命令來複製 **winui3preview** 分支：

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

複製之後，請確定您已切換至本機 Git 環境中的 **winui3preview** 分支：

```
git checkout winui3preview
```
