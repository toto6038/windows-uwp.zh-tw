---
title: Windows UI 程式庫 3-Project 留尼旺島 0.5 Preview (3 月 2021) 版本資訊
description: Windows UI 程式庫 3-Project 留尼旺島 0.5 Preview (3 月 2021) 版本的版本資訊。
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: e6d6f1a2007ab09b0295de24b1761d3dbb0c9094
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730857"
---
# <a name="windows-ui-library-3---project-reunion-05-preview-march-2021-release-notes"></a>Windows UI 程式庫 3-Project 留尼旺島 0.5 Preview (3 月 2021) 版本資訊

Windows UI 程式庫 (WinUI) 3 是一種原生使用者體驗 (UX) 平臺，可用於建立新式 Windows 應用程式。 這 WinUI 3 的預覽版本適用于 desktop/Win32 和 UWP 應用程式，並包含 Visual Studio 的專案範本，可協助您開始使用以 WinUI 為基礎的使用者介面來建立應用程式，以及包含 WinUI 程式庫的 NuGet 套件。

**WinUI 3-Project 留尼旺島 0.5 Preview** 是 WinUI 3 的第一版，其隨附于專案留尼旺島套件中。 除了這項變更之外，此預覽版本還包含重大的 bug 修正、更高的穩定性，以及一些其他一般改進 (查看 **[WinUI 3-Project 留尼旺島 0.5 preview) 中引進的功能](#major-changes-introduced-in-this-release)** 。

> [!Important]
> 此 WinUI 3 preview 旨在搶先評估並從開發人員群體收集意見反應。 **不會** 用於生產應用程式。
>
> 我們預計會在三月月底推出專案的0.5，其中包含 WinUI 3 的第一個穩定、支援的版本。
>
> 請使用 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml)以提供意見反應及記錄建議和問題。

## <a name="install-winui-3---project-reunion-05-preview"></a>安裝 WinUI 3-Project 留尼旺島 0.5 Preview

此版本的 WinUI 3 可作為 Project 留尼旺島 0.5 Preview 的一部分。 

若要安裝，請遵循《 [設定您的開發環境](../../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment) 指南》中的「專案留尼旺島」中找到的指示。 

相較于過去的 WinUI 3 預覽版本，您將下載 Project 留尼旺島 VSIX 套件，而不是 WinUI VSIX 套件。 Project 留尼旺島 VSIX 包含 [WinUI 專案範本](../winui-project-templates-in-visual-studio.md) ，您將使用這些範本來建立 WinUI 3 應用程式。 完成安裝之後，開發 WinUI 3 應用程式的體驗不應變更。

> [!NOTE]
> 您也可以複製和建立 WinUI 3 Preview 版本的 [XAML 控制項庫](#xaml-controls-gallery-winui-3-preview-branch)。

> [!NOTE]
> 若要使用 [即時視覺化樹狀結構]、[熱重新載入] 和 [即時屬性瀏覽器] 這類 WinUI 的工具，您必須啟用具有 Visual Studio 預覽功能的 WinUI 3 工具（如 [這裡的指示](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述）。

設定您的開發環境之後，請參閱 [建立 WinUI 3 專案](../winui-project-templates-in-visual-studio.md) ，以熟悉可用的 Visual Studio 專案和專案範本。 

如需開始使用 WinUI 專案範本的詳細資訊，請參閱下列文章：

- [開始使用適用於桌面應用程式的 WinUI 3](../get-started-winui3-for-desktop.md)
- [開始使用適用于 UWP 應用程式的 WinUI 3 (Preview) ](../get-started-winui3-for-uwp.md)

除了[限制和已知問題](#limitations-and-known-issues)之外，使用 WinUI 專案建置應用程式類似於使用 XAML 和 WinUI 2.x 建置 UWP 應用程式。 因此，大部分適用於 UWP 應用程式的 [指引文件](/windows/uwp/design/)，以及 Windows SDK 中的 **Windows.UI** WinRT 命名空間都適用。

WinUI 3 API 參考檔可在此處取得： [WinUI 3 Api 參考](/windows/winui/api)

如果您使用 WinUI 3 Preview 4 建立專案，您可以將專案升級為使用 Project 留尼旺島 0.5 Preview。 如需詳細指示，請參閱 [WinUI GitHub 存放庫](https://aka.ms/winui3/upgrade-instructions)。

### <a name="webview2"></a>WebView2
若要搭配此 WinUI 3 preview 使用 WebView2，請下載 [此頁面](https://developer.microsoft.com/microsoft-edge/webview2/) 上所找到的最長時間啟動載入器或 Alwayson 獨立安裝程式（如果您尚未安裝 WebView2 執行時間）。 

### <a name="windows-community-toolkit"></a>Windows 社群工具組

如果您使用的是 Windows 社群工具組，請[下載最新版本](https://aka.ms/wct-winui3)。

### <a name="visual-studio-support"></a>Visual Studio 支援

為了充分利用 WinUI 3 （例如熱重新載入、即時視覺化樹狀結構和即時屬性瀏覽器）所新增的最新工具功能，您必須使用最新的 WinUI 3 preview 版 Visual Studio 的最新 **預覽** 版本，並務必在 Visual Studio 預覽功能中啟用 WinUI 工具，如 [這裡的指示](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述。 下表顯示未來版本與 WinUI 3-Project 留尼旺島 0.5 Preview 的相容性：

| VS 版本  | WinUI 3-Project 留尼旺島 0.5 Preview  |
|---|---|
| 16.8 RTM  | 否   |
| 16.9 預覽版  | 是，使用工具  | 
| 16.9 RTM  | 是，但是沒有熱重新載入、即時視覺化樹狀結構或即時屬性瀏覽器   |
| 16.10 預覽版  | 是，使用工具   |

## <a name="major-changes-introduced-in-this-release"></a>此版本中引進的重大變更

- WinUI 3 現在隨附于 Project 留尼旺島套件中，這也是我們未來支援的發行版本將會提供的機制。

- 現在支援應用程式內 acrylic。

- 不再支援 Pivot 控制項，而且已在 WinUI 3 中淘汰。 建議您在應用程式內流覽案例中使用 [NavigationView 控制項](/windows/uwp/design/controls-and-patterns/navigationview) 。

- WinUI 3 與 Project 留尼旺島的 Windows 10 版本將僅支援舊版 1809-需要組建17763或更新版本。

- 預覽功能現在會標示為實驗。 
  - 預覽功能是將繼續成為 WinUI 3 preview 一部分的任何功能，但不是下一個 WinUI 3 支援版本的一部分。 
  - 預覽功能也包含任何屬於 WinUI 2.6 preview 的實驗性 Api。
  - 建立使用預覽功能的應用程式時，您的應用程式會擲回警告。 


## <a name="list-of-bugs-fixed-in-winui-3---project-reunion-05-preview"></a>WinUI 3 中已修正的 bug 清單-Project 留尼旺島 0.5 Preview

以下是小組自 Preview 3 以來修正的使用者對應 bug 清單。 另外還有許多關於穩定的工作，並改善我們的測試。

- WinUI 3 錯誤訊息需要改寫：「無法解析 ' Windows. metadata '。  請安裝 Windows 軟體開發套件。 Windows SDK 與 Visual Studio 一起安裝。」
- 在重新開機之前選取 Windows 預設主題時，應用程式不會回應 Windows 中的主題變更
- XamlDirect 上的例外狀況呼叫
  - 感謝您 @BorzillaR [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3509)提出此問題！
- ProgressBar 不會顯示已暫停和錯誤選項之間的差異
- StandardUICommand 清單視圖專案飛出視窗顯示在錯誤的位置。
- 嘗試以觸控方式重新排序 ListView 專案時，在桌面 XamlControlsGallery 中損毀
  - 感謝您 @j0shuams [在 GitHub 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3694)提出此問題！
- 按住 k 會將飛出視窗放在錯誤的位置
- 向左/向右箭號不會透過選項按鈕移動焦點
  - 感謝您 @vmadurga [在 Github 上](https://github.com/microsoft/microsoft-ui-xaml/issues/3385)提出此問題！
- 當使用者按下/向上鍵以選取 DatePicker 中的下一個/上一個月/年/日時，朗讀程式會保持無提示

- NavigationView light-關閉在 WinUI 3 中無法運作


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>過去 WinUI 3 預覽版 3 中引進的新特性和功能

下列是 WinUI 3 Preview 1-4 中引進的功能，並在 WinUI 3-Project 留尼旺島 0.5 Preview 中繼續支援此功能。

- 使用 WinUI 建立桌面應用程式的能力，包括適用于 Win32 應用程式的[.net 5](https://github.com/dotnet/core/tree/master/release-notes/5.0)
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
- 自訂標題列功能：新的 [ExtendsContentIntoTitleBar](/windows/winui/api/microsoft.ui.xaml.window.extendscontentintotitlebar) 和 [SetTitleBar](/windows/winui/api/microsoft.ui.xaml.window.settitlebar) api，可讓開發人員在桌面應用程式中建立自訂標題列。
- Virtualsurfaceimagesource 這是支援

如需有關 WinUI 3 和 WinUI 藍圖優點的詳細資訊，請參閱 GitHub 上的 [Windows UI 程式庫藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

### <a name="provide-feedback-and-suggestions"></a>提供意見反應和建議

我們歡迎您在 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) 中提供意見反應。

### <a name="whats-coming-next"></a>未來將推出哪些新功能？

請查看我們的詳細[功能藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)，了解將特定功能引進 WinUI 3 的時間。 

## <a name="limitations-and-known-issues"></a>限制與已知問題

WinUI 3-Project 留尼旺島 0.5 Preview 只是預覽。 桌面應用程式的相關案例特別新。 請預期會有錯誤、限制和其他問題。

下列專案是 WinUI 3-Project 留尼旺島 0.5 Preview 的一些已知問題。 如果您發現下面未列出的問題，請透過 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)參與現有問題或提出新問題，來讓我們知道。

### <a name="platform-and-os-support"></a>平台和 OS 支援

WinUI 3-Project 留尼旺島 0.5 Preview 與執行 Windows 10 2018 年10月更新 (1809 版-組建 17763) 和更新版本的電腦相容。

### <a name="developer-tools"></a>開發人員工具

- 僅支援 C# 和 C++/WinRT 應用程式
- 桌面應用程式支援 .NET 5 和 C# 9，而且必須在 MSIX 應用程式中加以封裝
- UWP 應用程式支援 .NET Native 和 C# 7.3
- 在 Visual Studio 中，開發人員工具和 Intellisense 可能無法正常運作。
- 沒有 XAML 設計工具支援
- 不支援新的 C++/CX 應用程式，不過，您現有的應用程式仍可繼續運作 (請盡速移至 C++/WinRT)
- 桌面應用程式中的多個 windows 支援正在進行中，但尚未完成且穩定。
  - 如果您發現多重視窗行為上出現新問題或功能衰退，請在我們的存放庫中提出該問題。
- 不支援未封裝的桌面部署
- 使用 F5 執行傳統型應用程式時，請確定您正在執行封裝專案。 在應用程式專案上按 F5，將會執行未封裝的應用程式，而 WinUI 3 尚不支援此動作。

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

### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>桌面應用程式中的 CoreWindow、ApplicationView、CoreApplicationView 和 CoreDispatcher

Preview 4 的新功能、 [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow)、 [ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView)、 [CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 
 [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher)及其相依性無法在桌面應用程式中使用。 例如，[ [發送器]](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) 屬性一律是 null，但 DispatcherQueue 屬性可以用來做為替代方案。

這些 Api 只能在 UWP 應用程式中運作。 在過去的預覽中，它們也已在桌面應用程式中部分運作，但在 Preview4 中，它們已完全停用。 這些 Api 是針對 UWP 案例所設計，其中每個執行緒只有一個視窗，而 WinUI3 的其中一項功能是啟用多個。

有 Api 會在內部相依于存在這些 Api，因此桌面應用程式不支援這些 api。 這些 Api 通常有靜態 `GetForCurrentView` 方法。 例如 [UIViewSettings. GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView)。

如需有關受影響 Api 以及這些 Api 之因應措施和取代的詳細資訊，請參閱 [適用于桌面應用程式的 WINRT api 變更](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/winrt-apis-for-desktop.md)

### <a name="known-issues"></a>已知問題

- 發生問題，導致 UWP 應用程式無法在 Windows 10 版本1809上啟動。 小組正在努力針對下一個預覽版本修正這個錯誤。

- Alt + F4 不會關閉桌面應用程式視窗。

- 桌面應用程式已不再支援 [UISettings. ColorValuesChanged 事件](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) 和 [AccessibilitySettings. HighContrastChanged 事件](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged) 。 如果您使用它來偵測 Windows 主題的變更，這可能會造成問題。 

- 此版本包含一些實驗性 Api，其會在使用時擲回組建警告。 小組尚未徹底測試這些專案，而且可能有未知的問題。 如果您遇到任何問題，請在存放庫中提出 [錯誤](https://github.com/microsoft/microsoft-ui-xaml/issues/new?assignees=&labels=&template=bug_report.md&title=) 。 

- 在過去，若要取得 CompositionCapabilities 實例，您可以呼叫 [CompositionCapabilites GetForCurrentView () ](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview)。 不過，此呼叫所傳回的功能並 *不* 相依于視圖。 為了解決並反映這一點，我們已在此版本中刪除 GetForCurrentView () 靜態，因此您現在可以直接建立 [CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities) 物件。

- 針對 C# UWP 應用程式：

  WinUI 3 架構是一組 WinRT 元件，可以從 C++ (使用 C++/WinRT) 或 C# 中使用。 使用 c # 時，有兩個版本的 .NET，視應用程式模型而定：當您在 UWP 應用程式中使用 WinUI 3 時 .NET Native;在桌面應用程式中使用時，您使用的是 .NET 5 (和 c #/WinRT) 。

  針對 UWP 中的 WinUI 3 應用程式使用 c # 時，相較于 WinUI 3 傳統型應用程式或 c # WinUI 2 應用程式中的 c #，有幾個 API 命名空間差異：某些類型位於 `Microsoft` 命名空間中，而不是 `System` 命名空間中。 例如，不是在 `System.ComponentModel` 命名空間中的 `INotifyPropertyChanged` 介面，而是在 `Microsoft.UI.Xaml.Data` 命名空間中。 

  這適用於：
    - `INotifyPropertyChanged` (和相關的類型)
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 命名空間版本仍然存在，但不能與 WinUI 3 搭配使用。 這表示 `ObservableCollection` 在 WinUI 3 C# UWP 應用程式中不會如預期般運作。 如需因應措施，請參閱 [XAML 控制項庫範例](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)中的 [CollectionsInterop 範例](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)。

## <a name="xaml-controls-gallery-winui-3-preview-branch"></a>XAML 控制項資源庫 (WinUI 3 Preview 分支) 

請參閱 [XAML 控制項庫中的 WinUI 3 Preview 分支](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview) ，以取得範例應用程式，其中包含 WinUI 3-Project 留尼旺島 0.5 Preview 中的所有控制項和功能。

:::image type="content" source="../images/WinUI3XamlControlsGallery.png" alt-text="WinUI 3 預覽 XAML 控制項資源庫應用程式":::<br/>
*WinUI 3 Preview XAML 控制項資源庫應用程式範例*

若要下載範例，請使用下列命令來複製 **winui3preview** 分支：

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

複製之後，請確定您已切換至本機 Git 環境中的 **winui3preview** 分支： 

```
git checkout winui3preview
```
