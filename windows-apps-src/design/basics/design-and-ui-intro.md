---
Description: 每個 UWP 應用程式都具有通用設計功能，可協助組建出能在各種裝置上美觀縮放的應用程式。
title: Windows 應用程式設計簡介 (Windows 應用程式)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 75d8dfe44c9296fbaf1d8caf5127db0244fc1d8d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216511"
---
# <a name="introduction-to-windows-app-design"></a>Windows 應用程式設計簡介

![範例光源應用程式](images/introUWP-header.jpg)

Windows 應用程式設計指引可以協助您設計和建立美觀、優雅的應用程式。

這不是一份詳盡規則的清單，這份文件會隨著不斷演進的 [Fluent Design 系統](/windows/apps/fluent-design-system)以及我們應用程式建置社群的需求改變。

本簡介提供每個 UWP 應用程式中包含的通用設計功能概觀，幫助您建立可在各種裝置上完美縮放的使用者介面 (UI)。

## <a name="effective-pixels-and-scaling"></a>有效像素與縮放

UWP 應用程式可在所有 [Windows 10 裝置](../devices/index.md) 上執行，從您的電視到您的平板電腦或個人電腦。 因此，要如何設計出在各種裝置和螢幕大小上，看起來都很棒的 UI？

![各種裝置上的同一個應用程式](images/universal-image-1.jpg)

UWP 應用程式會自動調整控 UI 元素的大小，因此在所有裝置和螢幕大小清晰可讀，而且可輕鬆地與其互動。

當您在裝置上執行應用程式 時，系統會使用演算法將螢幕上 UI 元素的顯示方式標準化。 這個縮放演算法會考量檢視距離和畫面密度 (每英吋像素) 來最佳化認知大小 (而不是實體大小)。 此縮放演算法可確保使用者在 10 英呎遠的 Surface Hub 上看到的 24px 字型，就和在只有幾英吋遠的 5 吋手機上看到的 24px 字型一樣清晰。

![不同裝置的檢視距離](images/scaling-chart.png)

因為此縮放系統的運作方式，所以您設計 UWP 應用程式時是使用有效像素，而不是實體像素。 有效像素 (epx) 是虛擬測量單位，用來快速表達配置的尺寸和間距，與畫面密度無關。 (在指導方針中，epx、ep 及 px 會交替使用。)

在設計的時候，您可以忽略像素密度和實際螢幕解析度。 相反地，針對大小類別設計實際解析度 (有效像素解析度) (如需詳細資訊，請參閱[螢幕大小與中斷點文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md))。

> [!TIP]
> 在影像編輯程式中建立螢幕圖樣時，請將 DPI 設定為 72，並針對您的目標大小類別，將影像尺寸設定為有效解析度。 如需大小類別和有效解析度的清單，請參閱[螢幕的大小與中斷點文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

### <a name="multiples-of-four"></a>四的倍數

:::row:::
    :::column span:::
在 UWP 應用程式中，UI 元素的大小、邊界及位置應一律為 **4 epx 的倍數**。

UWP 可透過 100%、125%、150%、175%、200%、225%、250%、300%、350% 和 400% 的縮放水準調整各種裝置。 基礎單位為 4，因為它是唯一可依非整數調整的整數 (例如 4*1.5 = 6)。 使用四的倍數將所有 UI 元素與整個像素對齊，並確保 UI 元素具有清晰、銳利的邊緣。 (請注意，文字並不具有此需求；文字可以有任何大小和位置。)
    :::column-end:::
    :::column:::
![方格](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>版面配置

由於 UWP 應用程式會對於所有裝置自動調整大小，所以設計適用於任何裝置的 UWP 應用程式依照相同的結構。 讓我們從您的 UWP 應用程式 UI 一開頭開始。

### <a name="windows-frames-and-pages"></a>Windows、框架和頁面

:::row:::
    :::column:::
當 UWP 應用程式在任何 Windows 10 裝置上啟動時，它會在 [Window](/uwp/api/windows.ui.xaml.window) 中啟動，其含有[框架](/uwp/api/windows.ui.xaml.controls.frame)，其可在[頁面](/uwp/api/windows.ui.xaml.controls.page)執行個體間巡覽。
    :::column-end:::
    :::column:::
![Frame](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
您可以將您應用程式的 UI 視為頁面的集合。 這取決於您決定每個頁面上應顯示內容以及在頁面之間的關係。

若要了解如何組織頁面，請參閱[瀏覽基本知識](navigation-basics.md)。
    :::column-end:::
    :::column:::
![Frame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>頁面配置

那些頁面看起來應該像什麼？ 大多數的頁面遵循相同的結構以提供一致性，以便使用者可以輕鬆地在應用程式的頁面之間巡覽。 頁面通常會包含三種類型的 UI 元素：

- [瀏覽](navigation-basics.md)元素協助使用者選擇他們想要顯示的內容。
- [命令](commanding-basics.md)元素會起始動作，例如操作、儲存或分享內容。
- [內容](content-basics.md)元素顯示應用程式的內容。

![常見的配置模式](../layout/images/page-components.svg)

若要深入了解如何實作常見 UWP 應用程式模式，請參閱[頁面配置](../layout/page-layout.md)文章。

您也可以使用 Visual Studio 中的 [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) 開始使用您的應用程式版面配置。

## <a name="controls"></a>控制項

UWP 的設計平台提供一組通用控制項，確保可在所有執行 Windows 的裝置上運作良好，並遵循我們的 [Fluent Design 系統](/windows/apps/fluent-design-system)原則。 這些控制項包括從按鈕和文字元素這類簡單的控制項，到可從一組資料和範本產生清單的複雜控制項。

![UWP 控制項](../style/images/color/windows-controls.svg)

如需 UWP 控制項的完整清單，以及可使用它們建立的模式，請參閱[控制項和模式一節](../controls-and-patterns/index.md)。

## <a name="style"></a>樣式

通用控制項會自動反映系統佈景主題和輔色、與所有輸入類型搭配運作，以及根據所有裝置進行調整。 如此一來，它們反映 Fluent Design 系統的彈性、共鳴和美觀。 通用控制項用於預設樣式中的燈號、動作和深度，因此透過使用它們，您即可在您的應用程式中結合我們的 Fluent Design 系統。

通用控制項還可高度自訂，您可以變更其前景色彩，或完全自訂其外觀。 若要覆寫控制項中的預設樣式，請在 XAML 中使用[輕量型樣式設定](../controls-and-patterns/xaml-styles.md#lightweight-styling)，或建立[自訂控制項](../controls-and-patterns/control-templates.md)。

![輔色 gif](images/intro-style.gif)

## <a name="shell"></a>殼層

:::row:::
    :::column:::
您的 UWP 應用程式會利用 Windows [Shell](../shell/tiles-and-notifications/creating-tiles.md) 中的磚和通知更廣泛的 Windows 體驗互動。

磚會顯示在 [開始] 功能表中，而當您的應用程式啟動時，其可讓您一窺該應用程式的狀態。 磚的支援來自後端的內容，以及情報，並利用貢獻的內容精心製作。

UWP 應用程式有四種磚大小 (小型、中型、寬形及大型)，可利用應用程式的圖示和身分識別自訂。 如需設計 UWP 應用程式磚的指導方針，請參閱[磚和圖示資產的指導方針](../style/app-icons-and-logos.md)。
    :::column-end:::
    :::column:::
![開始功能表上的磚](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>輸入

:::row:::
    :::column:::
UWP 應用程式需依賴智慧型互動。 您可以針對按一下的互動來設計，無需知道或定義按一下是來自滑鼠、手寫筆或手指的點選。 不過，您也可以針對[特定的輸入模式](../input/input-primer.md)設計您的應用程式。
    :::column-end:::
    :::column:::
![輸入](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>裝置

![裝置](../layout/images/size-classes.svg)

同樣地，UWP 會自動調整您的應用程式以符合不同的裝置，您也可以[針對特定裝置最佳化您的 UWP 應用程式](../devices/index.md)。

## <a name="usability"></a>可用性

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

最後同樣重要的是可用性，可讓您將應用程式開放給所有使用者體驗。 每個人都可受惠於真正的全人使用者體驗 - 請參閱 [UWP 應用程式的可用性](../usability/index.md)，了解如何讓每個人輕鬆使用您的應用程式。

如果您正在為國際對象進行設計，可能會想要查看[全球化和當地語系化](../globalizing/globalizing-portal.md)。

您也可以為視障、聽障和身障人士考慮[協助工具功能](../accessibility/accessibility-overview.md)。 如果協助工具一開始便深植在您的設計中，那麼不需花太多時間和精力，就能[讓您的應用程式提供無障礙功能](../accessibility/accessibility-in-the-store.md)。

## <a name="tools-and-design-toolkits"></a>工具與設計工具組

現在您已了解基本設計功能，想要開始設計您的 UWP 應用程式嗎？

我們提供各式各樣的工具，在設計過程中幫助您：

- 請參閱我們的[設計工具組頁面](../downloads/index.md)，以取得 XD、Illustrator、Photoshop、Framer 和 Sketch 工具組，及其他的設計工具與字型下載。

- 若要將您的電腦設定好，以便編寫 UWP 應用程式程式碼，請參閱我們的[開始使用&gt;開始設定](../../get-started/get-set-up.md)文章。

- 若想獲得一些如何實作 UWP UI 的靈感，請看看我們的完整[範例 UWP 應用程式](https://developer.microsoft.com/windows/samples)。

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>下一步：Fluent Design 系統

如果您想要深入了解 Fluent Design (Microsoft 的設計系統) 背後的原理，並查看更多可納入 UWP 應用程式中的功能，請繼續前往 [Fluent Design 系統](/windows/apps/fluent-design-system)。

## <a name="related-articles"></a>相關文章

- [什麼是 UWP 應用程式？](../../get-started/universal-application-platform-guide.md)
- [Fluent Design 系統](/windows/apps/fluent-design-system)
- [XAML 平台概觀](../../xaml-platform/index.md)
