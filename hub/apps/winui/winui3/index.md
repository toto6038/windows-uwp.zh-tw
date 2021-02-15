---
title: 'WinUI 3 Preview 4 (2021 年2月) '
description: WinUI 3 Preview 4 版本的總覽。
ms.date: 02/09/2021
ms.topic: article
ms.openlocfilehash: 7bbc5c4983f77080366942ecaf702e7e1f844886
ms.sourcegitcommit: 884318ec5118cade85a31f4d5644436614e9f272
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/15/2021
ms.locfileid: "100524994"
---
# <a name="windows-ui-library-3-preview-4-february-2021"></a>Windows UI 程式庫 3 Preview 4 (2021 年2月) 

Windows UI 程式庫 (WinUI) 3 是同時適用於 Windows 桌面和 UWP 應用程式的原生使用者體驗 (UX)。

**WinUI 3 preview 4** 是一個穩定性預覽版本，其中包含重大 bug 修正和其他一般改進 (請參閱 **[Preview 4) 所引進的功能](#capabilities-introduced-in-preview-4)** 。

> [!Important]
> WinUI 3 預覽版的目的是進行早期評估，並從開發人員社群收集意見反應。 **不會** 用於生產應用程式。
>
> 我們會繼續將 WinUI 3 的預覽版本寄送至2021，後面接著第一個官方支援的版本（2021年3月）。
>
> 請使用 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml)以提供意見反應及記錄建議和問題。

## <a name="install-winui-3-preview-4"></a>安裝 WinUI 3 Preview 4

WinUI 3 Preview 4 包含 Visual Studio 的專案範本，可協助您開始使用以 WinUI 為基礎的使用者介面來建立應用程式，以及包含 WinUI 程式庫的 NuGet 套件。 若要安裝 WinUI 3 Preview 4，請遵循下列步驟。

> [!NOTE]
> 您也可以複製和建立 WinUI 3 Preview 4 版的 [XAML 控制項庫](#xaml-controls-gallery-winui-3-preview-4-branch)。

1. 確定您的開發電腦已安裝 Windows 10 1803 (組建 17134) 或較新版本。

2. 安裝 [Visual Studio 2019 16.9 版 Preview](https://visualstudio.microsoft.com/vs/preview/)。 下載 **最新的預覽版本** ，以確保您能取得工作負載的所有必要更新 (例如 .net 5) 。

    安裝 Visual Studio 時，您必須包含下列工作負載：
    - 通用 Windows 平台開發

    若要建立 .NET 應用程式，您也必須包含下列工作負載：
    - .NET 桌面開發 (這也會安裝最新版本的 .NET 5，您將需要) 

    若要建立 C++ 應用程式，您也必須包含下列工作負載：
    - 使用 C++ 的傳統型開發
    - *C++ (v142) 通用 Windows 平台工具*，通用 Windows 平台工作負載的選用元件 (請參閱右側窗格中 [通用 Windows 平台開發] 區段底下的 [安裝詳細資料])

3. 請確定您的系統已針對 **nuget.org** 啟用 NuGet 套件來源。如需詳細資訊，請參閱 [常見的 NuGet 設定](/nuget/consume-packages/configuring-nuget-behavior)。

4. 下載並安裝 [WinUI 3 Preview 4 VSIX 套件](https://aka.ms/winui3/preview4-download)。 這會將 WinUI 3 專案範本和 NuGet 套件 (包含 WinUI 3 程式庫) 新增至 Visual Studio 2019。

    如需如何將 VSIX 套件新增至 Visual Studio 的指示，請參閱[尋找和使用 Visual Studio 擴充功能](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)。

5. 若要使用 [即時視覺化樹狀結構]、[熱重新載入] 和 [即時屬性瀏覽器] 這類 WinUI 的工具，您必須啟用具有 Visual Studio 預覽功能的 WinUI 3 工具（如 [這裡的指示](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述）。

#### <a name="webview2"></a>WebView2
若要使用 WebView2 搭配 WinUI 3 Preview 4，請下載在 [此頁面](https://developer.microsoft.com/microsoft-edge/webview2/)上找到的最長的啟動載入器或 Alwayson 獨立安裝程式。 

#### <a name="windows-community-toolkit"></a>Windows 社群工具組

如果您使用的是 Windows 社群工具組，請[下載最新版本](https://aka.ms/wct-winui3)。

## <a name="create-winui-projects"></a>建立 WinUI 專案

安裝 WinUI 3 Preview 4 VSIX 套件之後，您就可以使用 Visual Studio 中的其中一個 WinUI 專案範本來建立新專案。 若要在 [建立新專案] 對話方塊中存取 WinUI 專案範本，請將語言篩選為 **C++** 或 **C#** ，將平台篩選為 **Windows**，以及將專案類型篩選為 **WinUI**。 或者，您可以搜尋「WinUI」並選取其中一個可用的 C# 或 C++ 範本。

![WinUI 專案範本](images/winui-projects-csharp.png)

如需開始使用 WinUI 專案範本的詳細資訊，請參閱下列文章：

- [開始使用適用於桌面應用程式的 WinUI 3](get-started-winui3-for-desktop.md)
- [開始使用適用於 UWP 應用程式的 WinUI 3](get-started-winui3-for-uwp.md)

除了[限制和已知問題](#limitations-and-known-issues)之外，使用 WinUI 專案建置應用程式類似於使用 XAML 和 WinUI 2.x 建置 UWP 應用程式。 因此，大部分適用於 UWP 應用程式的 [指引文件](/windows/uwp/design/)，以及 Windows SDK 中的 **Windows.UI** WinRT 命名空間都適用。

即將推出此版本的 API 參考檔。 此連結會在有提供時提供。 在此同時，歡迎您查看 [Preview 3 的 WinUI 3 API 參考檔](/windows/winui/api/)。

如果您建立了使用 WinUI 3 Preview 3 的專案，您可以將專案升級為使用 Preview 4。 如需詳細指示，請參閱 [WinUI GitHub 存放庫](https://aka.ms/winui3/upgrade-instructions)。

### <a name="project-templates-for-winui-3"></a>WinUI 3 的專案範本

您可以使用這些 WinUI 專案範本來建立應用程式。

| 範本 | Language | 描述 |
|----------|----------|-------------|
| 已封裝的空白應用程式 (WinUI in Desktop) | C# 和 C++ | 使用以 WinUI 為基礎的使用者介面，建立桌面 .NET 5 (C#) 或原生 Win32 (C++) 應用程式。 產生的專案包含一個基本視窗，該視窗是從 WinUI 程式庫中的 **Microsoft.UI.Xaml.Window** 類別衍生，您可以用來開始建置您的 UI。 如需此專案類型的詳細資訊，請參閱[開始使用適用於桌面應用程式的 WinUI 3](get-started-winui3-for-desktop.md)。<p></p>解決方案也包含 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建置到 [MSIX 套件](/windows/msix/overview)。 這可提供新式部署體驗，透過套件擴充功能與 Windows 10 功能整合的能力，還有更多功能。  |
| 空白應用程式 (WinUI in UWP)  | C# 和 C++ | 建立具有以 WinUI 為基礎的使用者介面的 UWP 應用程式。 產生的專案包含一個基本頁面，該頁面是從 WinUI 程式庫中的 **Microsoft.UI.Xaml.Controls.Page** 類別衍生，您可以用來開始建置您的 UI。 如需此專案類型的詳細資訊，請參閱[開始使用適用於 UWP 應用程式的 WinUI 3](get-started-winui3-for-uwp.md)。 |

您可以使用這些 WinUI 專案範本來建置可供以 WinUI 為基礎的應用程式載入及使用的元件。

| 範本 | Language | 描述 |
|----------|----------|-------------|
| 類別庫 (WinUI in Desktop) | 僅限 C# | 在 C# 中建立 .NET 5 受控類別庫 (DLL)，可供具有以 WinUI 為基礎的使用者介面的其他 .NET 5 桌面應用程式使用。  |
| 類別庫 (WinUI in UWP)  | 僅限 C# | 在 C# 中建立受控類別程式庫 (DLL)，可供具有以 WinUI 為基礎的使用者介面的其他 UWP 應用程式使用。 |
| Windows 執行階段元件 (WinUI) | C++ | 建立以 C++/WinRT 撰寫的 [Windows 執行階段元件](/windows/uwp/winrt-components/)，可以由具有 WinUI 架構使者介面的任何 UWP 或桌面應用程式取用，不論該應用程式是以何種程式設計語言撰寫。 |
| Windows 執行階段元件 (UWP) | C# | 建立以 C# 撰寫的 [Windows 執行階段元件](/windows/uwp/winrt-components/)，可以由具有 WinUI 架構使者介面的任何 UWP 應用程式取用，不論該應用程式是以何種程式設計語言撰寫。 |

### <a name="item-templates-for-winui-3"></a>WinUI 3 的項目範本

您可以在 WinUI 專案中使用下列項目範本。 若要存取這些 WinUI 項目範本，請以滑鼠右鍵按一下 [方案總管] 中的專案節點，選取 [新增]  ->  [新項目]，然後按一下 [新增新項目] 對話方塊中的 [WinUI]。

![WinUI 項目範本](images/winui-items-csharp.png)

| 範本 | Language | 說明 |
|----------|----------|-------------|
| 空白頁面 (WinUI) | C# 和 C++ | 新增 XAML 檔案和程式碼檔案，該檔案會定義衍生自 WinUI 程式庫中 **Microsoft.UI.Xaml.Controls.Page** 類別的新頁面。 |
| 空白視窗 (WinUI in Desktop) | C# 和 C++ | 新增 XAML 檔案和程式碼檔案，該檔案會定義衍生自 WinUI 程式庫中 **Microsoft.UI.Xaml.Window** 類別的新視窗。 |
| 自訂控制項 (WinUI) | C# 和 C++ | 新增程式碼檔案，以使用預設樣式建立樣板化控制項。 樣板化控制項衍生自 WinUI 程式庫中的 **Microsoft.UI.Xaml.Controls.Control** 類別。<p></p>如需示範如何使用此項目範本的逐步解說，請參閱 [UWP 和 WinUI 3 應用程式的樣板化 XAML 控制項 (使用 C++/WinRT)](xaml-templated-controls-cppwinrt-winui-3.md) 和 [UWP 和 WinUI 3 應用程式的樣板化 XAML 控制項 (使用 C#)](xaml-templated-controls-csharp-winui-3.md)。 如需樣板化控制項的詳細資訊，請參閱[自訂 XAML 控制項](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |
| 資源字典 (WinUI) | C# 和 C++ | 新增 XAML 資源的空白索引鍵集合。 如需詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)。 |
| 資源檔 (WinUI) | C# 和 C++ | 新增檔案，用於儲存應用程式的字串和條件式資源。 您可以使用此項目來協助將您的應用程式當地語系化。 如需詳細資料，請參閱[將您 UI 和應用程式套件資訊清單中的字串當地語系化](/windows/uwp/app-resources/localize-strings-ui-manifest)。 |
| 使用者控制項 (WinUI) | C# 和 C++ | 新增 XAML 檔案和程式碼檔案，以建立衍生自 WinUI 程式庫中 **Microsoft.UI.Xaml.Controls.UserControl** 類別的使用者控制項。 一般而言，使用者控制項會封裝相關的現有控制項，並提供自己的邏輯。<p></p>如需使用者控制項的詳細資訊，請參閱[自訂 XAML 控制項](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |

### <a name="visual-studio-support"></a>Visual Studio 支援

為了充分利用新增至 WinUI 3 Preview 4 （例如熱重新載入、即時視覺化樹狀結構和即時屬性瀏覽器）的最新工具功能，您必須使用最新的 Visual Studio preview 版本搭配最新的 WinUI 3 preview，並務必在 Visual Studio 預覽功能中啟用 WinUI 工具，如 [以下指示](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述。 下表顯示 WinUI 3 Preview 4 未來版本的相容性：

| VS 版本  | WinUI 3 Preview 4  |
|---|---|
| 16.8 RTM  | 否   |
| 16.9 預覽版  | 是  | 
| 16.9 RTM  | 否   |
| 16.10 預覽版  | 是   |

## <a name="capabilities-introduced-in-preview-4"></a>Preview 4 中引進的功能

- WinUI (2.5 的同位檢查包括資訊列控制項、ProgressRing 和 NavigationView 的新功能，以及 bug 修正) 
- 自訂標題列功能：新的 ExtendsContentIntoTitleBar 和 SetTitleBar Api，可讓開發人員在桌面應用程式中建立自訂標題列。
- Virtualsurfaceimagesource 這是支援

## <a name="list-of-bugs-fixed-in-preview-4"></a>Preview 4 中已修正的 bug 清單

以下是小組自 Preview 3 以來修正的使用者對應 bug 清單。 另外還有許多關於穩定的工作，並改善我們的測試。

- 此版本已在新版的 CS/WinRT 和 Windows SDK 上進行，其修正了下列 bug：
  - 使用 {Binding} 系結至 URI 屬性時發生損毀
  - C #/WinRT 封送處理函式無法與 .NET 5 正確地交互操作

- 在 Windows 測試人員組建上執行時，WinUI 3 損毀
  - 感謝多個可在 GitHub 上報告此錯誤的社區參與者！ 
- WebView2 不會將主機應用程式的語言/地區設定套用至 CoreWebView2Environment
- Windows 社群工具組 DataGrid 控制項在啟動時/當機時，會控制損毀應用程式
  - 感謝多個可在 GitHub 上報告此錯誤的社區參與者！
- 顯示模式變更時，頁面轉譯處於不良狀態
- 在 CalendarView 中使用語言下拉式方塊時發生損毀
- WinUI 3 Desktop：無法移出 WebView2 的索引標籤
- WinUI 3 Desktop：具有衍生 TreeViewNodes 損毀的 TreeView 
  - 感謝您 @eleanorleffler [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/2699)提出此問題！
- WinUI 3 Desktop：無法在 ContentDialog 內的文字方塊中輸入文字 
  - 感謝您 @eleanorleffler [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/2704)提出此問題！
- WinUI 3 Desktop： ALT 和 F6 無法運作
- 舊版移除的 SwapChainPanel 會在新的 SwapChain 上轉譯
  - 感謝您 @dotMorten [在 Github 上](https://github.com/microsoft/microsoft-ui-xaml/issues/2942)提出此問題！
- WinUI 3 Desktop：無法以軌跡板滾動
- 在相同執行緒上搭配多個視窗使用 NavigationView 控制項時發生損毀
- 協助工具問題：在 WinUI 桌面應用程式啟動時顯示焦點矩形
- 滾動至 DataGrid 時發生存取違規
  - 感謝您 @TroelsL [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/2946)提出此問題！
- WinUI 3 Desktop：無法使用 Tab 鍵迴圈 
- 在 WinUI Xaml 孤島的桌面應用程式中，將 GridView 拖放至 GridView 失敗
  - 感謝您 @smk2007 [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3871)提出此問題！
- 協助工具問題：無法在 WinUI 3 桌面上使用 PageUp/PageDown 鍵進行滾動
- WebView2 具有錯誤的區大小
- 開啟 MenuFlyout 後按一下 WebView2 損毀
  - 感謝您 @sudongg [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3729)提出此問題！
- WinUI 3 Desktop：嘗試關閉 DropDownButton 或 SplitButton 的飛出視窗會導致應用程式損毀
- WebView2：以滑鼠右鍵按一下滑鼠會造成損毀
- 按一下 ToggleSplitButton 會導致應用程式損毀
  - 感謝您 @lhak [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3641)提出此問題！
- WinUI 3 Desktop：工作列上顯示空白的 DesktopWindowXamlSource 視窗
  - 感謝您 @bridgesquared [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3698)提出此問題！
- WinUI 3 Desktop： DataGrid 未顯示
  - 感謝您 @eleanorleffler [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/2703)提出此問題！
- WinUI 3 Desktop：無法將檔案放在方格上 
  - 感謝您 @eleanorleffler [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/2715)提出此問題！
- WinUI 3 Desktop： WinUI 3 Preview 2 中的 ItemsRepeater 損毀 
  - 感謝您 @hshristov [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3007)提出此問題！
- 更新系結時擲回的了 [accessviolationexception
  - 感謝您 @WamWooWam [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3680)提出此問題！
- WinUI 3 Desktop：在滾動 NavigationView 上應用程式當機
  - 感謝您 @Berkunath [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3598)提出此問題！
- 當動態新增或移除其 ItemsSource 集合中的專案時，不會更新 ItemsControl。 
  - 感謝您 @VigneshRameshh [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3517)提出此問題！
- 如果已啟用 c + + 符合模式，則 C2760 中的編譯錯誤 
  - 感謝您 @boostafazoo [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3716)提出此問題！


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>過去 WinUI 3 預覽版 3 中引進的新特性和功能

下列是 WinUI 3 Preview 1-3 中引進的功能，並在 WinUI 3 Preview 4 中繼續支援。

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
- MRT Core 支援
  - 這可讓應用程式在啟動時更快速且負擔更輕，並提供更快的資源查閱。
- ARM64 支援
- 在應用程式內部和外部拖放
- RenderTargetBitmap (目前僅限 XAML 內容 - 無 SwapChainPanel 內容)
- 自訂游標支援
- 無執行緒輸入
- 工具/開發人員體驗的改良：
  - 即時視覺化樹狀結構、熱重新載入、即時屬性總管和類似工具
  - 適用于 WinUI 3 的 Intellisense
- 遷移開放原始碼所需的改良功能

如需有關 WinUI 3 和 WinUI 藍圖優點的詳細資訊，請參閱 GitHub 上的 [Windows UI 程式庫藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

### <a name="provide-feedback-and-suggestions"></a>提供意見反應和建議

我們歡迎您在 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) 中提供意見反應。

### <a name="whats-coming-next"></a>未來將推出哪些新功能？

請查看我們的詳細[功能藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)，了解將特定功能引進 WinUI 3 的時間。 

## <a name="limitations-and-known-issues"></a>限制與已知問題

Preview 4 版本只是預覽版本。 特別是與桌面版應用程式相關的案例都是新項目。 請預期會有錯誤、限制和其他問題。

下列專案是 WinUI 3 Preview 4 的一些已知問題。 如果您發現下面未列出的問題，請透過 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)參與現有問題或提出新問題，來讓我們知道。

### <a name="platform-and-os-support"></a>平台和 OS 支援

WinUI 3 Preview 4 與執行 Windows 10 2018 年4月更新 (版本 1803-組建 17134) 和更新版本的電腦相容。

### <a name="developer-tools"></a>開發人員工具

- 僅支援 C# 和 C++/WinRT 應用程式
- 桌面應用程式支援 .NET 5 和 C# 9，而且必須在 MSIX 應用程式中加以封裝
- UWP 應用程式支援 .NET Native 和 C# 7.3
- 開發人員工具和 Intellisense 在 Visual Studio 16.8 版中可能無法正常運作。
- 沒有 XAML 設計工具支援
- 不支援新的 C++/CX 應用程式，不過，您現有的應用程式仍可繼續運作 (請盡速移至 C++/WinRT)
- 桌面應用程式中的多重視窗支援仍在準備中，尚未完成也未穩定。
  - 如果您發現多重視窗行為上出現新問題或功能衰退，請在我們的存放庫中提出該問題。
- 不支援未封裝的桌面部署
- 使用 F5 執行桌面應用程式時，請確定您執行的是封裝專案。 在應用程式專案上按 F5，將會執行未封裝的應用程式，而 WinUI 3 尚不支援此動作。

### <a name="missing-platform-features"></a>缺少平台功能

- Xbox 支援
- HoloLens 支援
- 視窗型快顯
  - 更明確地說，`ShouldConstrainToRootBounds` 屬性的行為一律會如同設定為 `true` 時的行為 (不論屬性值為何)。
- 筆跡支援
- 壓克力
- MediaElement 和 MediaPlayerElement
- MapControl
- SwapChainPanel 和非 XAML 內容的 RenderTargetBitmap
- SwapChainPanel 不支援透明度
- 全域顯示 (Global Reveal) 會使用回溯行為 (實心筆刷)
- 此版本不支援 XAML Islands
- 第三方生態系統程式庫的功能不完整
- IME 無法運作
- 桌面應用程式中不支援 CoreWindow、ApplicationView、CoreApplicationView、CoreDispatcher 及其相依性 (請參閱下文) 

#### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>桌面應用程式中的 CoreWindow、ApplicationView、CoreApplicationView 和 CoreDispatcher

Preview4、 [CoreWindow](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)、 [ApplicationView](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationView)、 [CoreApplicationView](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)CoreDispatcher 及其相依性的新功能， 
 [](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)無法在桌面應用程式中使用。

例如，[ [發送器]](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Dispatcher) 屬性一律是 null，但 DispatcherQueue 屬性可以用來做為替代方案。

這些 Api 只能在 UWP 應用程式中運作。
在過去的預覽中，它們也已在桌面應用程式中部分運作，但在 Preview4 中，它們已完全停用。
這些 Api 是針對 UWP 案例所設計，其中每個執行緒只有一個視窗，而 WinUI3 的其中一項功能是啟用多個。

這些 Api 會在內部相依于並存這些 Api，因此桌面應用程式不支援這些 api。 這些 Api 通常有靜態 `GetForCurrentView` 方法。 例如 [UIViewSettings. GetForCurrentView](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView)。


### <a name="known-issues"></a>已知問題

- Alt + F4 不會關閉桌面應用程式視窗。

- 由於 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)的變更，下列 WinRT api 可能無法再如預期般使用 **桌面** 應用程式：
  - [`ApplicationView`](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) 而且所有相關的 Api 都將無法再運作。
  - [`CoreApplicationView`](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview) 而且所有相關的 Api 都將無法再運作。
  - `GetForCurrentView`例如，可能不支援所有 api [`CoreInputView.GetForCurrentView`](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.Core.CoreInputView.GetForCurrentView) 。
  - [`CoreWindow.GetForCurrentThread`](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow.GetForCurrentThread) 現在會傳回 null。

  如需在 WinUI 3 傳統型應用程式中使用 WinRT Api 的詳細資訊，請參閱 [Windows 執行階段適用于桌面應用程式的 api](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-supported-api
)。

- 桌面應用程式已不再支援 [UISettings. ColorValuesChanged 事件](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) 和 [AccessibilitySettings. HighContrastChanged 事件](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged) 。 如果您使用它來偵測 Windows 主題的變更，這可能會造成問題。 

- 此版本包含一些實驗性 Api。 小組尚未徹底測試這些專案，而且可能有未知的問題。 如果您遇到任何問題，請在存放庫中提出 [錯誤](https://github.com/microsoft/microsoft-ui-xaml/issues/new?assignees=&labels=&template=bug_report.md&title=) 。 

- 在過去，若要取得 CompositionCapabilities 實例，您可以呼叫 [CompositionCapabilites GetForCurrentView () ](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview)。 不過，此呼叫所傳回的功能並 *不* 相依于視圖。 為了解決並反映這一點，我們已在此版本中刪除 GetForCurrentView () 靜態，因此您現在可以直接建立 [CompositionCapabilties](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities) 物件。

- 針對 C# UWP 應用程式：

  WinUI 3 架構是一組 WinRT 元件，可以從 C++ (使用 C++/WinRT) 或 C# 中使用。 使用時 C#，.NET 會有兩個版本 (視應用程式模型而定)：在 UWP 應用程式中使用 WinUI 3 時，您使用的是使用 .NET Native；而在桌面應用程式中使用時，則是使用 .NET 5 (和 C#/WinRT)。

  在 UWP 中針對 WinUI 3 應用程式使用 C# 時，API 命名空間會與在 WinUI 3 桌面應用程式或 C# WinUI 2 應用程式中使用 C# 時有一些差異：某些類型會位於 `Microsoft` 命名空間，而不是 `System` 命名空間中。 例如，不是在 `System.ComponentModel` 命名空間中的 `INotifyPropertyChanged` 介面，而是在 `Microsoft.UI.Xaml.Data` 命名空間中。 

  這適用於：
    - `INotifyPropertyChanged` (和相關的類型)
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 命名空間版本仍然存在，但不能與 WinUI 3 搭配使用。 這表示 `ObservableCollection` 在 WinUI 3 C# UWP 應用程式中不會如預期般運作。 如需因應措施，請參閱 [XAML 控制項庫範例](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)中的 [CollectionsInterop 範例](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)。

## <a name="xaml-controls-gallery-winui-3-preview-4-branch"></a>XAML 控制項資源庫 (WinUI 3 Preview 4 分支) 

如需包含所有 WinUI 3 Preview 4 控制項和功能的範例應用程式，請參閱 [XAML 控制項庫的 WinUI 3 preview 4 分支](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview) 。

![WinUI 3 Preview 4 XAML 控制項資源庫應用程式](images/WinUI3XamlControlsGallery.PNG)<br/>
*WinUI 3 Preview 4 XAML 控制項資源庫應用程式範例*

若要下載範例，請使用下列命令來複製 **winui3preview** 分支：

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

複製之後，請確定您已切換至本機 Git 環境中的 **winui3preview** 分支： 

```
git checkout winui3preview
```
