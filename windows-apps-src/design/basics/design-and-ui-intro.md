---
author: serenaz
Description: An overview of the universal design features that are included in every UWP app to help you build apps that scale beautifully across a range of devices.
title: 通用 Windows 平台 (UWP) 應用程式設計簡介 (Windows 應用程式)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: sezhen
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d0527866777c7b5dbbc10697bb313d664f4555fa
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817276"
---
#  <a name="introduction-to-uwp-app-design"></a>UWP 應用程式設計簡介

通用 Windows 平台 (UWP) 設計指導方針可以協助您設計和建立美觀、優雅的應用程式。

這不是一份詳盡規則的清單，這份文件會隨著不斷演進的 [Fluent Design 系統](../fluent-design-system/index.md)以及我們 app 建置社群的需求改變。

本簡介提供每個 UWP app 中包含的通用設計功能概觀，幫助您建立可在各種裝置上精美縮放的使用者介面 (UI)。

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="effective-pixels-and-scaling"></a>有效像素與縮放

UWP 應用程式會自動調整控制項的大小、字型和其他 UI 元素，使其可在[支援 UWP app 的所有裝置](../devices/index.md)上清晰可讀並可輕鬆地與其互動。

當您在裝置上執行應用程式時，系統會使用演算法將螢幕上 UI 元素的顯示方式標準化。 這個縮放演算法會考量檢視距離和畫面密度 (每英吋像素) 來最佳化認知大小 (而不是實體大小)。 此縮放演算法可確保使用者在 10 英呎遠的 Surface Hub 上看到的 24 px 字型，就和在只有幾英吋遠的 5 吋手機上看到的 24 px 字型一樣清晰。

![不同裝置的檢視距離](images/1910808-hig-uap-toolkit-03.png)

因為此縮放系統的運作方式，所以您設計 UWP app 時是使用有效像素，而不是實體像素。 有效像素 (epx) 是虛擬度量單位，用來快速配置尺寸和間距，與畫面密度無關。 (在我們的指導方針中，epx、ep 及 px 會交換使用。)

在設計的時候，您可以忽略像素密度和實際螢幕解析度。 相反地，針對大小類別設計實際解析度 (有效像素解析度) (如需詳細資訊，請參閱[螢幕大小與中斷點文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md))。

> [!TIP]
> 在影像編輯程式中建立螢幕圖樣時，請將 DPI 設定為 72，並針對您的目標大小類別，將影像尺寸設定為有效解析度。 如需大小類別和有效解析度的清單，請參閱[螢幕的大小與中斷點文章](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。


## <a name="layout"></a>版面配置

### <a name="margins-spacing-and-positioning"></a>邊界、間距與位置 
![網格](images/4epx.png)

系統調整您 app 的 UI 時，會以 4 的倍數進行。

因此，**使用者介面元素的大小、邊界及位置應一律為 4 epx 的倍數**。 這樣可符合整數像素，產生最佳轉譯效果。 此外也能確保 UI 元素邊緣清晰、銳利。 

請注意，文字並不具有此需求；文字可以有任何大小和位置。 如需如何將文字對齊其他 UI 元素的指導方針，請參閱 [UWP 印刷樣式指南](../style/typography.md)。

![縮放格線](images/epx-4pixelgood.png)

### <a name="layout-patterns"></a>配置模式

![常見的配置模式](images/page-components.png)

使用者介面由三種類型元素組成： 
1. **瀏覽元素**協助使用者選擇他們想要顯示的內容。 請參閱[瀏覽基本知識](navigation-basics.md)。
2. **命令元素**會起始動作，例如操作、儲存或分享內容。 請參閱[命令基本知識](commanding-basics.md)。
3. **內容元素**顯示應用程式的內容。 請參閱[內容基本知識](content-basics.md)。

### <a name="adaptive-behavior"></a>調適性行為
![電話和桌上型電腦的調適性行為](../controls-and-patterns/images/patterns_masterdetail.png)

雖然您的 App 將可在所有 Windows 支援的裝置上辨識和使用，您可能想要針對特定裝置和螢幕大小自訂您的 App UI。 如需其他特定指導方針，請參閱[螢幕大小和中斷點](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)和[回應式設計技術](../layout/responsive-design.md)。

## <a name="type"></a>字體

根據預設，UWP 應用程式使用 **Segoe UI** 字型且 UWP 字體坡形包括七種類別的印刷樣式，能針對所有顯示器大小提供最有效率的方式。 

![字體坡形](images/type-ramp.png)

如需字體坡形的詳細資訊，請參閱 [UWP 印刷樣式指南](../style/typography.md)。 若要了解如何在您的 App 中使用不同層級的 UWP 字體坡形，請參閱[佈景主題資源](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)。

## <a name="color"></a>色彩

色彩提供與使用者溝通資訊的直覺方式。 它可用來發出互動訊號、對使用者動作提供反饋、傳達狀態資訊，以及帶給您的介面視覺上的持續感。 

Windows 10 提供 48 色的共用通用調色盤，適用於殼層和 UWP App。 

![通用 windows 調色盤](images/colors.png)

系統會自動套用色彩至您的 UWP app，並使用系統輔色。 當使用者在 [設定] 中從調色盤選擇輔色時，色彩會顯示為系統佈景主題的一部分。 根據使用者喜好設定，系統輔色也可以在 [開始]、[開始] 磚、工作列和標題列上顯示。 

![在設定中選取輔色](images/selectcolor.png)

在您的 UWP app 中，通用控制項內的超連結和選取的狀態將會反映出系統輔色。

![控制項上的系統輔色](images/accentcolor.png)

UWP app 可以覆寫控制項中顯示的系統輔色，方法是使用[輕量型樣式設定](../controls-and-patterns/xaml-styles.md#lightweight-styling)或建立[自訂控制項](../controls-and-patterns/control-templates.md)。

有關在 UWP app 中使用色彩的其他指導方針，請參閱[色彩](../style/color.md)文章。

### <a name="themes"></a>佈景主題

使用者可以選擇淺色、深色或高對比佈景主題。 您可以選擇修改 app 的外觀，根據使用者佈景主題喜好設定，或選擇退出。

淺色主題最適合生產力工作和朗讀。

深色佈景主題適合在包含大量視訊或影像的媒體為主 app 和場景中讓對比更顯著。 在這些案例中，主要工作有可能是電影觀賞而非閱讀，也可能是在周遭環境光線不足的情況下進行。

![小算盤會以淺色和深色佈景主題顯示](images/light-dark.png)

高對比佈景主題使用色彩種類少的對比色，讓介面更容易分辨。
![淺色佈景主題和「黑底白字」佈景主題中的「小算盤」。](../accessibility/images/high-contrast-calculators.png)


如需在應用程式中使用佈景主題和 UWP 色彩坡形的詳細資訊，請參閱 [佈景主題資源](../controls-and-patterns/xaml-theme-resources.md)。

## <a name="icons"></a>圖示
![圖示](images/icons.png)

圖示是一種視覺化語言類型，已深入我們的日常生活當中。 圖示可讓我們以視覺上引人注目的方式表達概念和動作、節省螢幕空間，並且做為瀏覽數位生活的方式。 

所有 UWP app 都可在 [Segoe MDL2 字型](../style/segoe-ui-symbol-font.md)中存取圖示。 這些圖示依賴每個人熟悉且容易辨識的既定形式，但同時也經過現代化，看起來就像單手繪製而成。

如果您想要建立自己的圖示，請參閱 [UWP app 的圖示](../style/icons.md)。

## <a name="tiles"></a>磚
![開始功能表上的磚](images/tiles.png)

磚會顯示在 [開始] 功能表中，並可讓您一窺 app 的狀態。 磚的支援來自後端的內容，以及情報，並利用貢獻的內容精心製作。 

UWP app 有四種磚大小 (小型、中型、寬形及大型)，可利用 app 的圖示和身分識別自訂。 如需設計 UWP app 磚的指導方針，請參閱[磚和圖示資產的指導方針](../shell/tiles-and-notifications/app-assets.md)。

## <a name="controls"></a>控制項
![按鈕控制項](images/1910808-hig-uap-toolkit-01.png)

UWP 提供一組保證在所有運作 Windows 的裝置上都能運作良好的通用控制項。 這些控制項包括從按鈕和文字元素這類簡單的控制項，到可從一組資料和範本產生清單的複雜控制項。

UWP 控制項會自動反映系統佈景主題和輔色、與所有輸入類型搭配運作，以及根據所有裝置調整。 它們還可高度自訂，您可以變更控制項的前景色彩，或完全自訂其外觀。 

如需 UWP 控制項的完整清單，以及可使用它們建立的模式，請參閱[控制項和模式一節](../controls-and-patterns/index.md)。

## <a name="input"></a>輸入
![輸入](../input/images/input-interactions/icons-inputdevices03.png)

UWP app 需依賴智慧型互動。 您可以針對按一下的互動來設計，而不需知道或定義按一下是來自滑鼠、手寫筆或手指的點選。 不過，您也可以針對[特定的輸入模式和裝置](../input/input-primer.md)設計您的應用程式。

## <a name="accessibility"></a>協助工具
![全人設計的人員](images/inclusive.png)

最後同樣重要的是協助工具，可讓您將 app 開放給所有使用者體驗。 它與所有人相關，而不只是身心障礙人士。 每個人都可受惠於真正的全人使用者體驗 - 請參閱 [UWP app 的可用性](../usability/index.md)，了解如何讓每個人輕鬆使用您的 app。 您也可以為視障、聽障和身障人士考慮[協助工具功能](../accessibility/accessibility-overview.md)。 

如果協助工具一開始便深植在您的設計中，那麼不需花太多時間和精力，就能[讓您的 app 提供無障礙功能](../accessibility/accessibility-in-the-store.md)。

## <a name="tools-and-design-toolkits"></a>工具與設計工具組
現在您已了解基本設計功能，想要開始設計您的 UWP app 嗎？

我們提供各式各樣的工具，在設計過程中幫助您：

* 請參閱我們的[設計工具組頁面](../downloads/index.md)，以取得 XD、Illustrator、Photoshop、Framer 和 Sketch 工具組，及其他的設計工具與字型下載。 

* 若要將您的電腦設定好，以便編寫 UWP app 程式碼，請參閱我們的[開始 &gt; 設定](../../get-started/get-set-up.md)文章。 

* 若想獲得一些如何實作 UWP UI 的靈感，請看看我們的完整[範例 UWP app](https://developer.microsoft.com/windows/samples)。

## <a name="next-fluent-design-system"></a>下一節：Fluent Design 系統
如果您想要深入了解 Fluent Design (Microsoft 的設計系統) 背後的原理，並查看更多可納入 UWP app 中的功能，請繼續前往 [Fluent Design 系統](../fluent-design-system/index.md)。