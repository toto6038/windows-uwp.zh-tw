---
title: WinUI 3.0 預覽版 1 (2020 年 5月)
description: WinUI 3.0 預覽版的概觀。
ms.date: 05/14/2020
ms.topic: article
ms.openlocfilehash: 3aac14807f8455eb9a9294c40ddc76ddfa224659
ms.sourcegitcommit: 7e8c7f89212c88dcc0274c69d2c3365194c0954a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/20/2020
ms.locfileid: "83688484"
---
# <a name="windows-ui-library-30-preview-1-may-2020"></a>Windows UI 程式庫 3.0 預覽版 1 (2020 年 5月)

Windows UI 程式庫 (WinUI) 3.0 是一項重大更新，WinUI 將轉換為所有 Windows 應用程式類型 (從 Win32 到 UWP) 適用的完整 UX 架構。

> [!Important]
> WinUI 3.0 預覽版的目的是進行早期評估，並從開發人員社群收集意見反應。 **不會**用於生產應用程式。
>
> **請參閱[預覽版 1 的限制和已知問題](#preview-1-limitations-and-known-issues)** 。
## <a name="new-features-in-winui-30-preview-1"></a>WinUI 3.0 預覽版 1 中的新功能

- 能夠使用 WinUI 建立桌面應用程式，包括適用於 Win32 應用程式的 [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0)
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView 更新](/windows/uwp/design/controls-and-patterns/tab-view)
- 深色主題更新
- [WebView2](https://docs.microsoft.com/microsoft-edge/hosting/webview2) 的改良和更新
  - 高 DPI 的支援
  - 支援視窗的大小調整和移動
  - 已更新為以較新版本的 Edge 為目標
  - 不再需要參考 WebView2 專屬的 Nuget 套件
- SwapChainPanel
- 遷移開放原始碼所需的改良功能

如需有關 WinUI 3.0 和 WinUI 藍圖優點的詳細資訊，請參閱 GitHub 上的 [Windows UI 程式庫藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

### <a name="provide-feedback-and-suggestions"></a>提供意見反應和建議

我們歡迎您在 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) 中提供意見反應。

## <a name="try-winui-30-preview-1"></a>試用 WinUI 3.0 預覽版 1

設定您的開發環境 (如需詳細指示，請參閱[設定您的開發環境](#configure-your-dev-environment))、從下列連結安裝 WinUI 3.0 預覽版 1 VSIX，然後試用 WinUI 3.0 專案範本。

<table>
<tr>
<td align="center">
<a href="https://aka.ms/winui3/previewdownload"><img src="images/downloadbuttontx.png" alt="Download the WinUI 3.0 Preview 1 VSIX"/></a>
<!--
<br/>
<a href="https://aka.ms/winui3/previewdownload">Download the WinUI 3.0 Preview 1 VSIX</a>
-->
</td>
</tr>
</table>

您也可以複製 WinUI 3.0 預覽版 1 版本的 [Xaml 控制項資源庫](#xaml-controls-gallery-winui-30-preview-1-branch)，並加以建置。

### <a name="configure-your-dev-environment"></a>設定您的開發環境

確定您的開發電腦已安裝 Windows 10 2018 年 4 月更新 (1803 版 - 組建 17134) 或較新版本。

安裝 Visual Studio 2019 (16.7 預覽版 1 版本)。 您可以從 [Visual Studio Preview](https://visualstudio.microsoft.com/vs/preview) 下載此版本。

安裝 Visual Studio Preview 時，您必須包含下列工作負載：

- .NET 桌面開發
- 通用 Windows 平台開發

若要建立 C++ 應用程式，您也必須包含下列工作負載：

- 使用 C++ 的傳統型開發
- 適用於通用 Windows 平台工作負載的「C++ (v142) 通用 Windows 平台工具」選用元件

### <a name="visual-studio-project-templates"></a>Visual Studio 專案範本

若要存取 WinUI 3.0 預覽版 1 和專案範本，請移至 **https://aka.ms/winui3/previewdownload**

下載 Visual Studio 擴充功能 (`.vsix`)，將 WinUI 專案範本和 NuGget 套件新增至 Visual Studio 2019。

如需如何將 `.vsix` 新增至 Visual Studio 的指示，請參閱[尋找和使用 Visual Studio 擴充功能](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box)。

安裝 `.vsix` 擴充功能之後，您可以藉由搜尋 "WinUI" 並選取其中一個可用的 C# 或 C++ 範本來建立新的 WinUI 3.0 專案。

![WinUI 3.0 Visual Studio 範本](images/WinUI3Templates.png)<br/>
WinUI 3.0 Visual Studio 範本的範例

### <a name="visual-studio-project-configuration"></a>Visual Studio 專案組態

當您使用其中一個 WinUI 3.0 預覽版 1 範本建立專案時，請將**目標版本**設定為 Windows 10 1903 版 (組建 18362)，並將**最低版本**設定為 Windows 10 1803 版 (組建17134)。

若要在建立專案之後變更這些值，請以滑鼠右鍵按一下**方案總管**中的專案，然後選取 [屬性]。

### <a name="creating-a-desktop-win32-app-with-winui-30-preview-1"></a>使用 WinUI 3.0 預覽版 1 建立桌面版的 Win32 應用程式

請參閱[開始使用適用於桌面應用程式的 WinUI 3.0](get-started-winui3-for-desktop.md)。

除了以下所述的限制和已知問題之外，使用 WinUI 3.0 預覽版 1 建立應用程式非常類似於使用 Xaml 和 WinUI 2.x 建立 UWP 應用程式，因此，適用大部分 UWP 應用程式和 `Windows.UI` API 的指引和文件。

## <a name="preview-1-limitations-and-known-issues"></a>預覽版 1 的限制和已知問題

預覽版 1 版本僅供預覽。 特別是與桌面版 Win32 應用程式相關的案例都是新項目。 請預期會有錯誤、限制和問題。

下列項目是 WinUI 3.0 預覽版 1 的一些已知問題。 如果您發現下面未列出的問題，請在 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中參與現有問題或提出新問題，來讓我們知道。

### <a name="platform-and-os-support"></a>平台和 OS 支援

WinUI 3.0 預覽版 1 可與執行 Windows 10 2018 年 4 月更新 (1803 版 - 組建 17134) 和更新版本的電腦相容。

### <a name="developer-tools"></a>開發人員工具

- 桌面應用程式支援 .NET 5 和 C# 8，而且必須加以封裝。
- UWP 應用程式支援 .NET Native 和 C# 7.3
- Intellisense 不完整
- 沒有視覺化設計工具
- 沒有熱重載
- 沒有即時視覺化樹狀結構
- 尚不支援使用 VS Code 進行開發
- 不支援新的 C++/CX 應用程式，不過，您現有的應用程式仍可繼續運作 (請盡速移至 C++/WinRT)
- WinUI 3.0 內容只能出現在每個程序的一個視窗中，或每個應用程式的一個 ApplicationView 中
- 不支援未封裝的桌面部署
- 沒有 ARM64 支援

### <a name="missing-platform-features"></a>缺少平台功能

- 不支援 Xbox
- 不支援 HoloLens
- 不支援視窗型快顯視窗
- 沒有筆跡支援
- 背景壓克力
- MediaElement 和 MediaPlayerElement
- RenderTargetBitmap
- MapControl
- 具有 NavigationView 的階層式導覽
- SwapChainPanel 不支援透明度
- 全域顯示 (Global Reveal) 會使用回溯行為 (實心筆刷)
- 此版本不支援 XAML Islands
- 第三方生態系統程式庫的功能不完整
- IME 無法運作
- 無法呼叫 Windows.UI.Text 命名空間的方法

### <a name="known-issues"></a>已知問題

- 在 C# 傳統型應用程式中：
   - 對於 Windows 物件的弱式參考 (包含 Xaml 物件)，您必須使用 `WinRT.WeakReference<T>`，而不是 `System.WeakReference<T>`。
   - [Point](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)、[Rect](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect)和 [Size](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size) 結構的成員類型是 Float 而不是 Double。


## <a name="xaml-controls-gallery-winui-30-preview-1-branch"></a>Xaml 控制項資源庫 (WinUI 3.0 預覽版 1 分支)

如需包含所有 WinUI 3.0 預覽版 1 控制項和功能的範例應用程式，請參閱 [Xaml 控制項資源庫的 WinUI 3.0 預覽版 1 分支](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)。

![WinUI 3.0 預覽版 1 Xaml 控制項資源庫應用程式](images/WinUI3XamlControlsGallery.png)<br/>
WinUI 3.0 預覽版 1 Xaml 控制項資源庫應用程式的範例

若要下載範例，請使用下列命令來複製 **winui3preview** 分支：

> `git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git`

複製之後，請確定您已切換至本機 Git 環境中的 **winui3preview**分支：

> `git checkout winui3preview`
