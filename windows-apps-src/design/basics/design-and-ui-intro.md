---
author: serenaz
Description: An overview of the universal design features that are included in every UWP app to help you build apps that scale beautifully across a range of devices.
title: 通用 Windows 平台 (UWP) 應用程式設計簡介 (Windows 應用程式)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: sezhen
ms.date: 05/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e6f5d17a9fbdc044c3e1c0fcd152e6bafe2705a7
ms.sourcegitcommit: 4b6c197e1567d86e19af3ab5da516c022f1b6dfb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/11/2018
ms.locfileid: "1877270"
---
# <a name="introduction-to-uwp-app-design"></a>UWP app 設計簡介

![範例光源 App](images/introUWP-header.jpg)

通用 Windows 平台 (UWP) 設計指導方針是一項資源，可以協助您設計和建立美觀、優雅的應用程式。

這不是一份詳盡規則的清單，這份文件會隨著不斷演進的 [Fluent Design 系統](../fluent-design-system/index.md)以及我們 app 建置社群的需求改變。

本簡介提供每個 UWP app 中包含的通用設計功能概觀，幫助您建立可在各種裝置上精美縮放的使用者介面 (UI)。

## <a name="effective-pixels-and-scaling"></a>有效像素與縮放

首先，UWP app 在所有 [Windows 10 裝置](../devices/index.md) 上執行，從您的電視到您的平板電腦或個人電腦。 那會如何影響您的 App 的 UI？

![各種裝置上的相同 App](images/universal-image-1.jpg)

您很幸運，UWP app 會自動調整控 UI 元素的大小，使其可在所有裝置和螢幕大小上清晰可讀並可輕鬆地與其互動。

當您在裝置上執行 App 時，系統會使用演算法將螢幕上 UI 元素的顯示方式標準化。 這個縮放演算法會考量檢視距離和畫面密度 (每英吋像素) 來最佳化認知大小 (而不是實體大小)。 此縮放演算法可確保使用者在 10 英呎遠的 Surface Hub 上看到的 24 px 字型，就和在只有幾英吋遠的 5 吋手機上看到的 24 px 字型一樣清晰。

![不同裝置的檢視距離](images/scaling-chart.png)

因為此縮放系統的運作方式，所以您設計 UWP app 時是使用有效像素，而不是實體像素。 有效像素 (epx) 是虛擬度量單位，用來快速配置尺寸和間距，與畫面密度無關。 (在我們的指導方針中，epx、ep 及 px 會交換使用。)

在設計的時候，您可以忽略像素密度和實際螢幕解析度。 相反地，針對大小類別設計實際解析度 (有效像素解析度) (如需詳細資訊，請參閱[螢幕大小與中斷點文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md))。

> [!TIP]
> 在影像編輯程式中建立螢幕圖樣時，請將 DPI 設定為 72，並針對您的目標大小類別，將影像尺寸設定為有效解析度。 如需大小類別和有效解析度的清單，請參閱[螢幕的大小與中斷點文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

### <a name="multiples-of-four"></a>四的倍數

:::列::: :::欄範圍::: 系統調整您 app 的 UI 時，會以 4 的倍數進行。

        As a result, the sizes, margins, and positions of **UI elements should always be in multiples of 4 epx**. This results in the best rendering by aligning with whole pixels. It also ensures that UI elements have crisp, sharp edges. (Note that text doesn't have this requirement; text can have any size and position.)
    :::column-end:::
    :::column:::
        ![grid](images/4epx.svg)
    :::column-end:::
:::列結束:::

## <a name="layout"></a>版面配置

由於 UWP app 會對於所有裝置自動調整大小，所以設計適用於任何裝置的 UWP app 依照相同的結構。 讓我們從您的 UWP app 的 UI 的一開頭開始。

### <a name="windows-frames-and-pages"></a>Windows、框架和頁面

::: 列:::::: 欄::: 當 UWP app 在任何 Windows 10 裝置上啟動時，它會在 [Window](/uwp/api/Windows.UI.Xaml.Controls.Window) 中啟動，其含有[框架](/uwp/api/Windows.UI.Xaml.Controls.Frame)，其可在[頁面](/uwp/api/Windows.UI.Xaml.Controls.Page)執行個體間巡覽。
:::欄結束::: :::欄::: ![框架](images/frame.svg) :::欄結束::: :::列結束:::

::: 列:::::: 欄::: 您可以將您的 App 的 UI 視為頁面的集合。 這取決於您決定每個頁面上應顯示內容以及在頁面之間的關係。

        To learn how you can organize your pages, see [Navigation basics](navigation-basics.md).
    :::column-end:::
    :::column:::
        ![Frame](images/collection-pages.svg)
    :::column-end:::
:::列結束:::

### <a name="page-layout"></a>頁面配置

那些頁面看起來應該像什麼？ 大多數的頁面遵循相同的結構以提供一致性，以便使用者可以輕鬆地在 App 的頁面之間巡覽。 頁面通常會包含三種類型的 UI 元素：

- [瀏覽](navigation-basics.md)元素協助使用者選擇他們想要顯示的內容。
- [命令](commanding-basics.md)元素會起始動作，例如操作、儲存或分享內容。
- [內容](content-basics.md)元素顯示 App 的內容。

![常見的配置模式](../layout/images/page-components.svg)

若要深入了解如何實作常見 UWP app 模式，請參閱[頁面配置](../layout/page-layout.md)文章。

您也可以使用 Visual Studio 中的 [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) 開始使用您的 App 的版面配置。

## <a name="controls"></a>控制項

UWP 的設計平台提供一組通用控制項，確保可在所有執行 Windows 的裝置上運作良好，並遵循我們的 [Fluent Design 系統](../fluent-design-system/index.md)原則。 這些控制項包括從按鈕和文字元素這類簡單的控制項，到可從一組資料和範本產生清單的複雜控制項。

![UWP 控制項](../style/images/color/windows-controls.svg)

如需 UWP 控制項的完整清單，以及可使用它們建立的模式，請參閱[控制項和模式一節](../controls-and-patterns/index.md)。

## <a name="style"></a>樣式

通用控制項會自動反映系統佈景主題和輔色、與所有輸入類型搭配運作，以及根據所有裝置進行調整。 如此一來，它們反映 Fluent Design 系統的彈性、共鳴和美觀。 通用控制項用於預設樣式中的燈號、動作和深度，因此透過使用它們，您即在您的 App 中結合我們的 Fluent Design 系統。

通用控制項還可高度自訂，您可以變更其前景色彩，或完全自訂其外觀。 若要覆寫控制項中的預設樣式，請在 XAML. 中使用[輕量型樣式設定](../controls-and-patterns/xaml-styles.md#lightweight-styling)，或建立[自訂控制項](../controls-and-patterns/control-templates.md)。

![輔色 gif](images/intro-style.gif)

## <a name="shell"></a>命令介面

::: 列:::::: 欄::: 您的 UWP app 會利用 Windows [命令介面](../shell/tiles-and-notifications/creating-tiles.md)中的磚和通知更廣泛的 Windows 體驗互動。

        Tiles are displayed in the Start menu and when your app launches, and they provide a glimpse of what's going on in your app. Their power comes from the content behind them, and the intelligence and craft with which they're offered up.

        UWP apps have four tile sizes (small, medium, wide, and large) that can be customized with the app's icon and identity. For guidance on designing tiles for your UWP app, see [Guidelines for tile and icon assets](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
        ![tiles on start menu](images/shell.svg)
    :::column-end:::
:::列結束:::

## <a name="inputs"></a>輸入

:::列::: :::欄::: UWP app 仰賴智慧型互動。 您可以針對按一下的互動來設計，無需知道或定義按一下是來自滑鼠、手寫筆或手指的點選。 不過，您也可以針對[特定的輸入模式](../input/input-primer.md)設計您的應用程式。
:::欄結束::: :::欄::: ![輸入](images/inputs.svg) :::欄結束::: :::列結束:::

## <a name="devices"></a>裝置

![裝置](../layout/images/size-classes.svg)

同樣地，UWP 會自動調整您的 App 以符合不同的裝置，您也可以[針對特定裝置最佳化您的 UWP app](../devices/index.md)。

## <a name="usability"></a>可用性

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

最後同樣重要的是可用性，可讓您將 App 開放給所有使用者體驗。 每個人都可受惠於真正的全人使用者體驗 - 請參閱 [UWP app 的可用性](../usability/index.md)，了解如何讓每個人輕鬆使用您的 App。

如果您正在為國際對象進行設計，可能會想要查看[全球化和當地語系化](../globalizing/globalizing-portal.md)。

您也可以為視障、聽障和身障人士考慮[協助工具功能](../accessibility/accessibility-overview.md)。 如果協助工具一開始便深植在您的設計中，那麼不需花太多時間和精力，就能[讓您的 app 提供無障礙功能](../accessibility/accessibility-in-the-store.md)。

## <a name="tools-and-design-toolkits"></a>工具與設計工具組

現在您已了解基本設計功能，想要開始設計您的 UWP app 嗎？

我們提供各式各樣的工具，在設計過程中幫助您：

- 請參閱我們的[設計工具組頁面](../downloads/index.md)，以取得 XD、Illustrator、Photoshop、Framer 和 Sketch 工具組，及其他的設計工具與字型下載。

- 若要將您的電腦設定好，以便編寫 UWP app 程式碼，請參閱我們的[開始 &gt; 設定](../../get-started/get-set-up.md)文章。

- 若想獲得一些如何實作 UWP UI 的靈感，請看看我們的完整[範例 UWP app](https://developer.microsoft.com/windows/samples)。

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>下一節：Fluent Design 系統

如果您想要深入了解 Fluent Design (Microsoft 的設計系統) 背後的原理，並查看更多可納入 UWP app 中的功能，請繼續前往 [Fluent Design 系統](../fluent-design-system/index.md)。

## <a name="related-articles"></a>相關文章

- [什麼是 UWP app？](../../get-started/universal-application-platform-guide.md)
- [Fluent Design 系統](../fluent-design-system/index.md)
- [XAML 平台概觀](../../xaml-platform/index.md)