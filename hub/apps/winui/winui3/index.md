---
title: 'WinUI 3 Project 留尼旺島 0.5 (三月 2021) '
description: WinUI 3 Project 留尼旺島0.5 的總覽。
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: 92437934b7a07c6409d5d44325094f8dd024f443
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730461"
---
# <a name="windows-ui-library-3---project-reunion-05-march-2021"></a>Windows UI 程式庫 3-Project 留尼旺島 0.5 (三月 2021) 

Windows UI 程式庫 (WinUI) 3 是一種原生使用者體驗， (UX) 架構來建立新式 Windows 應用程式。  它會獨立于 Windows 作業系統中，做為 [專案留尼旺島](../../project-reunion/index.md)的一部分。  Project 留尼旺島0.5 版提供 [Visual Studio 的專案範本](https://aka.ms/projectreunion/vsixdownload) ，可協助您開始使用 WinUI 3 型使用者介面來建立應用程式。

**WinUI 3-Project 留尼旺島 0.5** 是第一個穩定、支援的 WinUI 3 版本，可用來建立可發佈至 Microsoft Store 的生產環境應用程式。 此版本包含穩定性更新和一般改良功能，可讓 WinUI 3 向前相容和生產環境就緒。


## <a name="install-winui-3---project-reunion-05"></a>安裝 WinUI 3-Project 留尼旺島0。5

這個新版本的 WinUI 3 可作為 Project 留尼旺島0.5 的一部分。 若要安裝，請參閱：

**[Project 留尼旺島0.5 的安裝指示](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)**

既然 WinUI 是做為專案留尼旺島的一部分，您將 (VSIX) 開始下載專案，Visual Studio 擴充功能，包括一組開發人員工具與元件。 如需專案留尼旺島套件的詳細資訊，請參閱 [部署使用 Project 留尼旺島的應用程式](../../project-reunion/deploy-apps-that-use-project-reunion.md)。 Project 留尼旺島 VSIX 包含 [WinUI 專案範本](winui-project-templates-in-visual-studio.md) ，您將使用這些範本來建立 WinUI 3 應用程式。 

> [!NOTE]
> 若要查看作用中的 WinUI 3 控制項和功能，您可以從 GitHub 複製並建立 WinUI 3 版的 [XAML 控制項庫](#winui-3-controls-gallery) 。

> [!NOTE]
> 若要使用 [即時視覺化樹狀結構]、[熱重新載入] 和 [即時屬性瀏覽器] 這類 WinUI 的工具，您必須啟用具有 Visual Studio 預覽功能的 WinUI 3 工具（如 [這裡的指示](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述）。

設定您的開發環境之後，請參閱 [Visual Studio 中的 WinUI 3 專案範本](winui-project-templates-in-visual-studio.md) ，以熟悉可用的 Visual Studio 專案和專案範本。 

如需開始建立 WinUI 3 應用程式的詳細資訊，請參閱下列文章：

- [開始使用適用於桌面應用程式的 WinUI 3](get-started-winui3-for-desktop.md)
- [建立適用于桌面的基本 WinUI 3 應用程式](desktop-build-basic-winui3-app.md)

除了[限制和已知問題](#limitations-and-known-issues)之外，使用 WinUI 專案建置應用程式類似於使用 XAML 和 WinUI 2.x 建置 UWP 應用程式。 因此，大部分適用於 UWP 應用程式的 [指引文件](/windows/uwp/design/)，以及 Windows SDK 中的 **Windows.UI** WinRT 命名空間都適用。

WinUI 3 API 參考檔可在此處取得： [WinUI 3 Api 參考](/windows/winui/api)

### <a name="webview2"></a>WebView2

若要在此 WinUI 3 版本中使用 WebView2，請下載 [此頁面](https://developer.microsoft.com/microsoft-edge/webview2/) 上所找到的最長時間啟動載入器或 Alwayson 獨立安裝程式（如果您尚未安裝 WebView2 Runtime）。 

### <a name="windows-community-toolkit"></a>Windows 社群工具組

如果您使用的是 Windows 社群工具組，請[下載最新版本](https://aka.ms/wct-winui3)。

### <a name="visual-studio-support"></a>Visual Studio 支援

為了充分利用 WinUI 3 （例如熱重新載入、即時視覺化樹狀結構和即時屬性瀏覽器）所新增的最新工具功能，您必須使用 Visual Studio 的最新預覽版本，並務必在 Visual Studio 預覽功能中啟用 WinUI 工具，如這裡的 [指示](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述。 下表顯示與 WinUI 3-Project 留尼旺島 0.5 Visual Studio 2019 版本的相容性：

| VS 版本  | WinUI 3-Project 留尼旺島0。5  |
|---|---|
| 16.8  | 否   |
| 16.9  | 是，但是沒有熱重新載入、即時視覺化樹狀結構或即時屬性瀏覽器  |
| 16.10 預覽版  | 是，所有 WinUI 3 工具   |

## <a name="updating-your-existing-winui-3-app"></a>更新現有的 WinUI 3 應用程式

您可以更新使用 WinUI 3 預覽版本的應用程式，以使用這個新支援的 WinUI 3 版本。 根據您的應用程式類型，請參閱下列指示。

> [!NOTE] 
> 這些指示可能會因為每個應用程式個別案例的唯一性而發生問題。 請仔細關注它們，如果您發現問題，請 [在我們的 GitHub 存放](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)庫中提出錯誤。 

### <a name="updating-a-winui-3-preview-4-app-to-use-winui-3---project-reunion-05"></a>更新 WinUI 3 Preview 4 應用程式以使用 WinUI 3-Project 留尼旺島0。5

開始之前，請確定您已安裝所有 WinUI 3-Project 留尼旺島0.5 必要條件，包括 Project 留尼旺島 VSIX 和 NuGet 套件。 請參閱 [此處的安裝指示](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)。

首先，請務必完成下列步驟： 
- 在 wapproj 檔案中，如果您的 TargetPlatformMinVersion 早于10.0.17763.0，請將其變更為10.0.17763。0 

- 如果您的應用程式使用該 `Application.Suspending` 事件，請務必移除或變更該行，因為不再 `Application.Suspending` 針對桌面應用程式呼叫該程式程式碼。 如需詳細資訊，請參閱 [API 參考檔](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true) 。

  請注意，c + + 和 c # 應用程式的預設專案範本包含下列幾行。 如果您的程式碼中仍有這些行，請務必移除這些行：

    C#: `this.Suspending += OnSuspending;`
  
    C++：`Suspending({ this, &App::OnSuspending });` 

現在，對您的專案進行一些變更： 
  
1. 在 Visual Studio 中，移至 [**工具**  ->  **Nuget 封裝管理員**]  ->  **封裝管理員主控台**。
2. 輸入 ```uninstall-package Microsoft.WinUI -ProjectName {yourProject}```
3. 輸入 ```install-package Microsoft.ProjectReunion -Version 0.5.0 -ProjectName {yourProjectName}```
4. 在您的應用程式中進行下列變更 (封裝) wapproj：

    新增此區段：

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0]">
        <IncludeAssets>build</IncludeAssets>
      </PackageReference>
    </ItemGroup>
    ```

    然後移除下列幾行：

    ```xml
    <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
    ```

    ```xml
    <Import Project="$(AppxTargetsLocation)Microsoft.WinUI.AppX.targets" />
    ```

5. 刪除專案的 `Microsoft.WinUI.AppX.targets` {yourproject。} (套件) /build/資料夾下的現有檔案。

### <a name="updating-a-winui-3---project-reunion-05-preview-app-to-use-winui-3---project-reunion-05-stable"></a>更新 WinUI 3-Project 留尼旺島 0.5 Preview 應用程式以使用 WinUI 3-Project 留尼旺島 0.5 (穩定) 

開始之前，請確定您已安裝所有 WinUI 3-Project 留尼旺島0.5 必要條件，包括 Project 留尼旺島 VSIX 和 NuGet 套件。 請參閱 [此處的安裝指示](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)。

首先，請務必完成下列步驟：
- 在 wapproj 檔案中，如果您的 TargetPlatformMinVersion 早于10.0.17763.0，請將其變更為10.0.17763。0 

- 如果您的應用程式使用該 `Application.Suspending` 事件，請務必移除或變更該行，因為不再 `Application.Suspending` 針對桌面應用程式呼叫該程式程式碼。 如需詳細資訊，請參閱 [API 參考檔](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true) 。

  請注意，c + + 和 c # 應用程式的預設專案範本包含下列幾行。 如果您的程式碼中仍有這些行，請務必移除這些行：

    C#: `this.Suspending += OnSuspending;`
  
    C++：`Suspending({ this, &App::OnSuspending });` 

現在，對您的專案進行一些變更： 

1. 在 Visual Studio 中，移至 [**工具**  ->  **Nuget 封裝管理員**]  ->  **封裝管理員主控台**。
2. 輸入 ```uninstall-package Microsoft.ProjectReunion -ProjectName {yourProject}```
3. 輸入 ```uninstall-package Microsoft.ProjectReunion.Foundation -ProjectName {yourProject}```
4. 輸入 ```uninstall-package Microsoft.ProjectReunion.WinUI -ProjectName {yourProject}```
5. 輸入 ```install-package Microsoft.ProjectReunion -Version 0.5.0 -ProjectName {yourProjectName}```
6. 在您的應用程式中進行下列變更 (封裝) wapproj：
  
    新增此區段：

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0]">
        <IncludeAssets>build</IncludeAssets>
      </PackageReference>
    </ItemGroup>
    ```
    然後移除下列幾行 (如果存在) ：
    ```xml
    <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
    ```

    ```xml
    <Import Project="$(Microsoft_ProjectReunion_AppXReference_props)" />
    <Import Project="$(Microsoft_WinUI_AppX_targets)" />
    ```

    然後移除這個專案群組：

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
        <ExcludeAssets>all</ExcludeAssets>
      </PackageReference>
      <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
        <ExcludeAssets>all</ExcludeAssets>
      </PackageReference>
    </ItemGroup>
    ```

5. 刪除專案的 `Microsoft.WinUI.AppX.targets` {yourproject。} (套件) /build/資料夾下的現有檔案。

## <a name="major-changes-introduced-in-this-release"></a>此版本中引進的重大變更

### <a name="stable-features"></a>穩定功能

此版本提供穩定性和支援，讓 WinUI 3 適用于可交付給 Microsoft Store 的生產應用程式。 它包含過去預覽中引進的大部分功能的支援和向前相容性：

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
- 應用程式內壓克力

### <a name="preview-features"></a>預覽功能

因為這是穩定的版本，所以已從這個版本的 WinUI 3 中移除預覽功能。 您仍然可以使用 [WinUI 3 的目前預覽版本](release-notes/winui3-project-reunion-0.5-preview.md)來存取這些功能。 請注意，下列主要功能仍處於預覽狀態，而為了穩定它們正在進行穩定：

- UWP 支援
  - 這表示您無法使用 WinUI 3-Project 留尼旺島 0.5 VSIX 來建立或執行 UWP 應用程式。 您必須使用 [WinUI 3-Project 留尼旺島 0.5 PREVIEW VSIX](https://aka.ms/projectreunion/previewdownload)，並遵循在 [開始使用 Project 留尼旺島](../../project-reunion/get-started-with-project-reunion.md)中設定開發環境的其他指示。 如需詳細資訊，請參閱  [WINDOWS UI 程式庫 3-Project 留尼旺島 0.5 Preview (3 月 2021) 版本](release-notes/winui3-project-reunion-0.5-preview.md) 資訊。

- 桌面應用程式中的多視窗支援

- 輸入驗證

### <a name="provide-feedback-and-suggestions"></a>提供意見反應和建議

我們歡迎您在 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) 中提供意見反應。

### <a name="whats-coming-next"></a>未來將推出哪些新功能？

如需規劃特定功能的詳細資訊，請參閱 GitHub 上的 [功能藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap) 。

## <a name="limitations-and-known-issues"></a>限制與已知問題

下列專案是 WinUI 3-Project 留尼旺島0.5 的一些已知問題。 如果您發現下面未列出的問題，請透過 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)參與現有問題或提出新問題，來讓我們知道。

### <a name="platform-and-os-support"></a>平台和 OS 支援

WinUI 3-Project 留尼旺島0.5 與執行 Windows 10 2018 年10月更新 (1809 版-組建 17763) 和更新版本的電腦相容。

### <a name="developer-tools"></a>開發人員工具

- 僅支援 C# 和 C++/WinRT 應用程式
- 桌面應用程式支援 .NET 5 和 C# 9，而且必須在 MSIX 應用程式中加以封裝
- 沒有 XAML 設計工具支援
- 不支援新的 C++/CX 應用程式，不過，您現有的應用程式仍可繼續運作 (請盡速移至 C++/WinRT)
- 不支援未封裝的桌面部署
- 使用 F5 執行傳統型應用程式時，請確定您正在執行封裝專案。 在應用程式專案上按 F5，將會執行未封裝的應用程式，而 WinUI 3 尚不支援此動作。

### <a name="missing-platform-features"></a>缺少平台功能

- Xbox 支援
- HoloLens 支援
- 視窗型快顯
  - 更明確地說，`ShouldConstrainToRootBounds` 屬性的行為一律會如同設定為 `true` 時的行為 (不論屬性值為何)。
- 筆跡支援，包括：
  - [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)
  - [HandwritingView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HandwritingView)
  - [InkPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)
- 背景壓克力
- MediaElement 和 MediaPlayerElement
- MapControl
- SwapChainPanel 不支援透明度
- 全域顯示 (Global Reveal) 會使用回溯行為 (實心筆刷)
- 此版本不支援 XAML Islands
- 直接在現有的非 WinUI 傳統型應用程式中使用 WinUI 3 有下列限制：目前可用於遷移現有應用程式的路徑，是將新的 WinUI 3 專案 **新增** 至您的解決方案，並視需要調整或重構邏輯。

- 在桌面應用程式中，不會呼叫應用程式。 請參閱應用程式上的 API 參考檔。如需詳細資料，請 [暫停事件](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true ) 。 

- 桌面應用程式中不支援 CoreWindow、ApplicationView、CoreApplicationView、CoreDispatcher 及其相依性 (請參閱下文) 

### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>桌面應用程式中的 CoreWindow、ApplicationView、CoreApplicationView 和 CoreDispatcher

WinUI 3 Preview 4 的新功能和未來版本中的 [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow)、 [ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView)、 [CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher)及其相依性，不適用於桌面應用程式。 例如，[ [發送器]](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) 屬性一律是 **Null**，但 **DispatcherQueue** 屬性可以用來做為替代方案。

這些 Api 只能在 UWP 應用程式中運作。 在過去的預覽中，它們也已在桌面應用程式中部分運作，但自 Preview 4 起已完全停用。 這些 Api 是針對 UWP 案例所設計，其中每個執行緒只有一個視窗，而 WinUI 3 的其中一項功能是在未來啟用多個。

有 Api 會在內部相依于存在這些 Api，因此桌面應用程式不支援這些 api。 這些 Api 通常有靜態 `GetForCurrentView` 方法。 例如 [UIViewSettings. GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView)。

如需有關受影響 Api 以及這些 Api 之因應措施和取代的詳細資訊，請參閱 [適用于桌面應用程式的 WINRT api 變更](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/winrt-apis-for-desktop.md)

### <a name="known-issues"></a>已知問題

- Alt + F4 不會關閉桌面應用程式視窗。

- 桌面應用程式已不再支援 [UISettings. ColorValuesChanged 事件](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) 和 [AccessibilitySettings. HighContrastChanged 事件](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged) 。 如果您使用它來偵測 Windows 主題的變更，這可能會造成問題。 

- 在過去，若要取得 CompositionCapabilities 實例，您可以呼叫 [CompositionCapabilites GetForCurrentView () ](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview)。 不過，此呼叫所傳回的功能並 *不* 相依于視圖。 為了解決並反映這一點，我們已在此版本中刪除 GetForCurrentView () 靜態，因此您現在可以直接建立 [CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities) 物件。

- Acrylic 筆刷呈現透明。 

- 由於 c #/WinRT 問題，訂閱某些 framework 元素事件和頁面流覽可能會造成記憶體流失。 

- 您可能會在此版本中遇到其他 c #/WinRT 問題，例如 GC/ObjectDisposedExceptions、封送處理值以及可為 null 的類型， (TimeSpan、IReference 等 <Vector3>) 。 
  - 這些將在四月的即將推出的 .NET 5 SDK 服務版本中修正。 您可以直接下載並安裝此更新 (例如，在組建管線中) 或以隱含方式透過 Visual Studio 更新來挑選。

## <a name="winui-3-controls-gallery"></a>WinUI 3 控制項資源庫

查看 WinUI 3 控制項資源庫 (先前稱為 _XAML 控制項庫-WinUI 3 版本_) 適用于範例應用程式，其中包含 WinUI 3-Project 留尼旺島0.5 的一部分的所有控制項和功能。

![WinUI 3 控制項資源庫應用程式](images/WinUI3XamlControlsGallery.png)<br/>
*WinUI 3 控制項資源庫應用程式的範例*

您可以藉由複製 GitHub 存放庫來下載範例。 若要這樣做，請使用下列命令來複製 **winui3** 分支：

```
git clone --single-branch --branch winui3 https://github.com/microsoft/Xaml-Controls-Gallery.git
```

在複製之後，請確定您已切換至本機 Git 環境中的 **winui3** 分支： 

```
git checkout winui3
```
