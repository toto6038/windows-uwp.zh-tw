---
title: WinUI 3 預覽版 3 (2020 年 11 月)
description: WinUI 3 預覽版 3 的概觀。
ms.date: 11/17/2020
ms.topic: article
ms.openlocfilehash: ac641036af8505b1e51fb81385f5206a9aa44f40
ms.sourcegitcommit: 29c8999fb7a941fc6e26b49cf10f4cc1fcb69641
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "95002913"
---
# <a name="windows-ui-library-3-preview-3-november-2020"></a>Windows UI 程式庫 3 預覽版 3 (2020 年 11 月)

Windows UI 程式庫 (WinUI) 3 是同時適用於 Windows 桌面和 UWP 應用程式的原生使用者體驗 (UX)。

**WinUI 3 預覽版 3** 包含全新和改良的功能，以及重大的錯誤修正。

**請參閱 [預覽版 3 的限制和已知問題](#preview-3-limitations-and-known-issues)** 。

> [!Important]
> WinUI 3 預覽版的目的是進行早期評估，並從開發人員社群收集意見反應。 **不會** 用於生產應用程式。
>
> 我們會繼續將 WinUI 3 的預覽版本延續到 2021，然後推出第一個正式版本。
>
> 請使用 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml)以提供意見反應及記錄建議和問題。

## <a name="install-winui-3-preview-3"></a>安裝 WinUI 3 預覽版 3

WinUI 3 預覽版 3 包含 Visual Studio 專案範本，可協助您開始使用以 WinUI 為基礎的使用者介面以及包含 WinUI 程式庫的 NuGet 套件，來建置應用程式。 若要安裝 WinUI 3 預覽版 3，請遵循下列步驟。

> [!NOTE]
> 您也可以複製 WinUI 3 預覽版 3 版本的 [XAML 控制項資源庫](#xaml-controls-gallery-winui-3-preview-3-branch)，並加以建置。

1. 確定您的開發電腦已安裝 Windows 10 1803 (組建 17134) 或較新版本。

2. 安裝 [Visual Studio 2019 (16.9 預覽版)](https://visualstudio.microsoft.com/vs/preview/)

    安裝 Visual Studio 時，您必須包含下列工作負載：
    - .NET 桌面開發 (這也會安裝 .NET 5)
    - 通用 Windows 平台開發

    若要建立 C++ 應用程式，您也必須包含下列工作負載：
    - 使用 C++ 的傳統型開發
    - *C++ (v142) 通用 Windows 平台工具*，通用 Windows 平台工作負載的選用元件 (請參閱右側窗格中 [通用 Windows 平台開發] 區段底下的 [安裝詳細資料])
3. 請確定您的系統已針對 **nuget.org** 啟用 NuGet 套件來源。如需詳細資訊，請參閱 [常見的 NuGet 設定](/nuget/consume-packages/configuring-nuget-behavior).[Windows 社群工具組](#windows-community-toolkit)

4. 下載及安裝 [WinUI 3 預覽版 3 VSIX 套件](https://aka.ms/winui3/preview3-download)。 這會將 WinUI 3 專案範本和 NuGet 套件 (包含 WinUI 3 程式庫) 新增至 Visual Studio 2019。

    如需如何將 VSIX 套件新增至 Visual Studio 的指示，請參閱[尋找和使用 Visual Studio 擴充功能](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)。

#### <a name="webview2"></a>WebView2

如果您在應用程式中使用 WebView2 控制項，請從 [Microsoft Edge Insider Channels](https://www.microsoftedgeinsider.com/en-us/download)安裝 **Microsoft Edge 瀏覽器的 Dev 通道版本**。 請務必將 Microsoft Edge Beta、Microsoft Edge Dev 和 Microsoft Edge WebView2 執行階段的任何現有執行個體解除安裝。

#### <a name="windows-community-toolkit"></a>Windows 社群工具組

如果您使用的是 Windows 社群工具組，請[下載最新版本](https://aka.ms/wct-winui3)。

## <a name="create-winui-projects"></a>建立 WinUI 專案

安裝 WinUI 3 預覽版 3 VSIX 套件之後，您就可以在 Visual Studio 中使用其中一個 WinUI 專案範本來建立新的專案。 若要在 [建立新專案] 對話方塊中存取 WinUI 專案範本，請將語言篩選為 **C++** 或 **C#** ，將平台篩選為 **Windows**，以及將專案類型篩選為 **WinUI**。 或者，您可以搜尋「WinUI」並選取其中一個可用的 C# 或 C++ 範本。

![WinUI 專案範本](images/winui-projects-csharp.png)

如需開始使用 WinUI 專案範本的詳細資訊，請參閱下列文章：

- [開始使用適用於桌面應用程式的 WinUI 3](get-started-winui3-for-desktop.md)
- [開始使用適用於 UWP 應用程式的 WinUI 3](get-started-winui3-for-uwp.md)

除了[限制和已知問題](#preview-3-limitations-and-known-issues)之外，使用 WinUI 專案建置應用程式類似於使用 XAML 和 WinUI 2.x 建置 UWP 應用程式。 因此，大部分適用於 UWP 應用程式的 [指引文件](/windows/uwp/design/)，以及 Windows SDK 中的 **Windows.UI** WinRT 命名空間都適用。

在此版本中，我們也新增了 [WinUI 3 API 參考文件](/windows/winui/api/)，適用於所有已移植到 WinUI 3 的 WinRT API。

如果您使用 WinUI 3 預覽版 2 建立專案，您可以將專案升級為使用預覽版 3。 如需詳細指示，請參閱 [WinUI GitHub 存放庫](https://aka.ms/winui3/upgrade-instructions)。

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

若要使用新增至 WinUI 3 預覽版 3 的最新工具功能 (如熱重新載入、即時視覺化樹狀結構和即時屬性總管)，您必須使用最新的 Visual Studio 預覽版與最新的 WinUI 3 預覽版。 下表顯示未來版本與 WinUI 3 預覽版 3 的相容性：

| VS 版本  | WinUI 3 預覽版 3  |
|---|---|
| 16.8 RTM  | 否   |
| 16.9 預覽版  | 是  | 
| 16.9 RTM  | 否   |
| 16.10 預覽版  | 是   |


## <a name="new-features-and-capabilities-in-preview-3"></a>預覽版 3 的新特性與功能

- ARM64 支援
- 在應用程式內部和外部拖放
- RenderTargetBitmap (目前僅限 XAML 內容 - 無 SwapChainPanel 內容)
- 工具/開發人員體驗的改良：
  - 即時視覺化樹狀結構、熱重新載入、即時屬性總管和類似工具
  - Intellisense 現在可用於 WinUI 3 
- MRT Core 支援
  - 這可讓應用程式在啟動時更快速且負擔更輕，並提供更快的資源查閱。
- 自訂游標支援
- 無執行緒輸入
- 自預覽版 2 以來的效能改進
- 桌面應用程式中的多重視窗 - 預覽版 2 之後的改良支援


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>過去 WinUI 3 預覽版 3 中引進的新特性和功能

下列功能是在 WinUI 3 預覽版 1 與 2 中引進，並繼續在 WinUI 3 預覽版 3 中受支援。

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

### <a name="whats-coming-next"></a>未來將推出哪些新功能？

請查看我們的詳細[功能藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)，了解將特定功能引進 WinUI 3 的時間。 

## <a name="preview-3-limitations-and-known-issues"></a>預覽版 3 的限制和已知問題

預覽版 3 版本僅供預覽。 特別是與桌面版應用程式相關的案例都是新項目。 請預期會有錯誤、限制和其他問題。

下列項目是 WinUI 3 預覽版 3 的一些已知問題。 如果您發現下面未列出的問題，請透過 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)參與現有問題或提出新問題，來讓我們知道。

### <a name="platform-and-os-support"></a>平台和 OS 支援

WinUI 3 預覽版 3 可與執行 Windows 10 2018 年 4 月更新 (版本 1803 - 組建 17134) 和更新版本的電腦相容。

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

### <a name="known-issues"></a>已知問題

- Alt + F4 不會關閉桌面應用程式視窗。

-   如果您在應用程式中使用 WebView2，但其並未呈現或載入，則您可能執行了不相容的 Edge 瀏覽器版本。 [請下載 Microsoft Edge 的 Dev 通道版本](https://www.microsoftedgeinsider.com/en-us/download)，並且務必將 Microsoft Edge Beta、Microsoft Edge Dev 和 Microsoft Edge WebView2 執行階段的任何現有執行個體解除安裝。

- 您不應在 .NET 5 WinUI 應用程式中使用封送函式，因為這些函式無法與 C#/WinRT 正確地交互操作。 如需詳細資訊，請參閱[本頁面](https://github.com/microsoft/CsWinRT/blob/master/docs/interop.md)。

- 如果您在設定 URI 屬性時發生損毀情況 (例如在 Xaml 標記中使用 `{Binding}` 時)，您可以使用 `{x:Bind}` 或使用 C#/WinRT 的預覽版本來解決此問題。 若要這麼做，請將下列幾行新增至您的 .csproj 檔案：

  ```xml
  <ItemGroup>
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        RuntimeFrameworkVersion="10.0.18362.11-preview" />
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        TargetingPackVersion="10.0.18362.11-preview" />
  </ItemGroup>
  ```

- 針對 C# UWP 應用程式：

  WinUI 3 架構是一組 WinRT 元件，可以從 C++ (使用 C++/WinRT) 或 C# 中使用。 使用時 C#，.NET 會有兩個版本 (視應用程式模型而定)：在 UWP 應用程式中使用 WinUI 3 時，您使用的是使用 .NET Native；而在桌面應用程式中使用時，則是使用 .NET 5 (和 C#/WinRT)。

  在 UWP 中針對 WinUI 3 應用程式使用 C# 時，API 命名空間會與在 WinUI 3 桌面應用程式或 C# WinUI 2 應用程式中使用 C# 時有一些差異：某些類型會位於 `Microsoft` 命名空間，而不是 `System` 命名空間中。 例如，不是在 `System.ComponentModel` 命名空間中的 `INotifyPropertyChanged` 介面，而是在 `Microsoft.UI.Xaml.Data` 命名空間中。 

  這適用於：
    - `INotifyPropertyChanged` (和相關的類型)
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 命名空間版本仍然存在，但不能與 WinUI 3 搭配使用。 這表示 `ObservableCollection` 在 WinUI 3 C# UWP 應用程式中不會如預期般運作。 如需因應措施，請參閱 [XAML 控制項庫範例](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)中的 [CollectionsInterop 範例](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)。

## <a name="xaml-controls-gallery-winui-3-preview-3-branch"></a>XAML 控制項庫 (WinUI 3 預覽版 3 分支)

如需包含所有 WinUI 3 預覽版 3 控制項和功能的範例應用程式，請參閱 [XAML 控制項資源庫的 WinUI 3 預覽版 3 分支](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)。

![WinUI 3 預覽版 3 XAML 控制項庫應用程式](images/WinUI3XamlControlsGalleryP3.PNG)<br/>
WinUI 3 預覽版 3 XAML 控制項庫應用程式的範例

若要下載範例，請使用下列命令來複製 **winui3preview** 分支：

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

複製之後，請確定您已切換至本機 Git 環境中的 **winui3preview** 分支： 

```
git checkout winui3preview
```
