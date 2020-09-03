---
title: 間距和大小
description: 不論裝置和輸入方法為何，新的 Fluent 標準和精簡控制項樣式都可確保舒適的使用者體驗。
keywords: UWP, Windows 10, 控制項, 大小, 密度, 標準, 精簡
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 37bc5688321249428405e730d366eb987a81bd66
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159842"
---
# <a name="control-size-and-density"></a>控制項大小和密度

使用控制項大小和密度的組合對您的 Windows 應用程式進行最佳化，並提供最適合您應用程式功能及互動需求的使用者體驗。

根據預設，UWP 應用程式會以低密度 (或 `Standard`) 配置。 不過，從 WinUI 2.1 開始，也支援高密度 (或 `Compact`) 版面配置選項 (適用於資訊豐富的 UI 和類似的特殊情況)。 這可透過基本樣式資源指定 (請參閱以下範例)。

雖然功能和行為尚未變更，並且在兩個大小和密度選項之間保持一致，但所有控制項的預設主體字型大小已更新為 14px，以支援這兩個密度選項。 這個字型大小適用於各個區域和裝置，確保您的應用程式保持平衡且方便使用者使用。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/Compact Sizing">開啟應用程式，並查看精簡大小調整的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-standard-sizing"></a>Fluent 標準大小調整

「Fluent 標準大小調整」旨在提供資訊密度和使用者舒適度之間平衡。 實際上，畫面上所有的項目都會對齊 40x40 有效像素 (epx) 目標，其可讓 UI 元素貼齊格線並根據系統層級縮放適度調整。

**大小調整的設計訴求是要適應觸控和指標輸入。**

> [!NOTE]
>如需有效像素與縮放規模的詳細資訊，請參閱 [Windows 應用程式設計簡介](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 如需系統層級縮放的詳細資訊，請參閱[對齊、邊界、邊框距離](../layout/alignment-margin-padding.md)。

對於 Windows 10 2018 年 10 月更新 (1809 版)，所有 UWP 控制項的標準、預設大小已縮小，以提高所有使用案例的可用性。

下圖顯示 Windows 10 2018 年 10 月更新所引進的一些控制項版面配置變更。 具體而言，控制項的標頭與頂端之間的邊界從 8epx 縮小為 4epx，且 44epx 格線已變更為 40epx 格線。

![標準控制項版面配置範例](images/standarddensity.png)

*標準控制項版面配置範例*

下圖顯示對 Windows 10 2018 年 10 月更新的控制項大小所做的變更。 具體來說，對齊 40epx 格線。

![標準命令範例](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Fluent 精簡大小調整

精簡大小調整讓密集、資訊豐富的控制項群組對下列作業有所幫助：

- 瀏覽大量內容。
- 將頁面上可見的內容最大化。
- 瀏覽控制項和內容並與其互動

**大小調整的主要設計訴求是要適應指標輸入。**

### <a name="examples"></a>範例

精簡大小調整是透過特殊資源字典實作，您可以在應用程式中的頁面層級或特定版面配置上指定該字典。 在 [WinUI](/uwp/toolkits/winui/) Nuget 套件中可取得資源字典。

下列範例顯示頁面和個別格線控制項如何才能套用 `Compact` 樣式。

#### <a name="page-level"></a>頁面層級

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>格線層級

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [觸控目標的指導方針](../input/guidelines-for-targeting.md)
- [ResourceDictionary 與 XAML 資源參考](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
- [資源字典](/uwp/api/windows.ui.xaml.resourcedictionary)
- [XAML 樣式](../controls-and-patterns/xaml-styles.md)