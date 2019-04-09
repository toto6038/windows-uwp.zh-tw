---
title: 間距和大小
description: 新的 Fluent 標準和 Compact 控制項樣式確保舒適的使用者體驗，不論他們的裝置和輸入的方法。
keywords: UWP、 Windows 10、 控制項、 大小、 高密度、 標準、 compact
ms.date: 4/4/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 7b74e3dc2ad047d9e52509b71ef00b829ad63a0d
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249450"
---
# <a name="control-size-and-density"></a>控制項大小和密度

您可以使用控制項大小和密度的組合來最佳化您的通用 Windows 平台 (UWP) 應用程式，並提供最適合您的應用程式功能和互動需求的使用者體驗。

根據預設，UWP 應用程式要用來呈現之低密度 (或`Standard`) 配置。 不過，開頭為 WinUI 2.1，高密度 (或`Compact`) 版面配置選項資訊豐富的 UI 和類似的特殊情況下，也支援。 這可以指定透過基本的樣式資源 （請參閱以下範例）。

在功能和行為尚未變更，且兩者間保持一致的大小和密度的選項，預設主體字型大小已更新為 14px 所有控制項，以支援這些兩個密度選項。 這個字型大小跨區域和裝置的運作方式，並可確保您的應用程式保持平衡且方便的使用者。

## <a name="fluent-standard-sizing"></a>Fluent 標準的調整大小

*Fluent 的標準規模*建立提供資訊密度和使用者緩和之間取得平衡。 實際上，在螢幕上的所有項目會對齊 40 x 40 有效像素 (epx) 目標，這可讓 UI 項目貼齊格線和調整適當地根據系統層級調整。

**標準的調整大小被設計來容納觸控和輸入的指標。**

> [!NOTE]
>如需有效的像素與調整規模的詳細資訊，請參閱[UWP 應用程式的設計簡介](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 如需系統層級調整的詳細資訊，請參閱[對齊、 邊界、 邊框距離](../layout/alignment-margin-padding.md)。

適用於 Windows 10 年 10 月 2018 Update （版本 1809年） 標準，所有 UWP 控制項的預設大小已減少跨所有使用案例提高使用性。

下圖顯示一些控制項的版面配置變更後使用 Windows 10 年 10 月 2018年的更新。 具體而言，標頭和控制項的頂端之間的邊界從 8epx 減少 4epx，，和 44epx 格線已變更為 40epx 方格。

![標準控制項的版面配置範例](images/standarddensity.png)

*標準控制項的版面配置範例*

下一步 下圖顯示變更控制項大小適用於 Windows 10 年 10 月 2018年的更新。 具體來說，40epx 格線對齊。

![標準的命令範例](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Fluent Compact 的調整大小

Compact 的調整大小啟用密集、 資訊豐富的控制項群組，並可使用下列協助：

- 瀏覽大量的內容。
- 最大化在網頁上可見的內容。
- 瀏覽並與其互動的控制項和內容

**Compact 的調整大小主要被設計來容納指標輸入。**

### <a name="examples"></a>範例

Compact 縮放是透過特殊的資源字典可指定您在頁面層級，或在特定的版面配置上的應用程式中實作。 資源字典可用於[WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/) Nuget 套件。

下列範例會顯示如何`Compact`頁面和個別的方格控制項，可以套用樣式。

#### <a name="page-level"></a>頁面層級

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>方格層級

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="related-articles"></a>相關文章

- [觸控目標的指導方針](../input/guidelines-for-targeting.md)
- [ResourceDictionary 與 XAML 資源參考](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [資源字典](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [XAML 樣式](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
