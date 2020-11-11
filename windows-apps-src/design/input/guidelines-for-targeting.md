---
description: 本主題描述在 Windows 執行階段應用程式中如何使用接觸幾何來預測觸控目標，並提供觸控目標的最佳做法。
title: 目標
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c0f18feed166ea775f540a28fd873d7e81083cf0
ms.sourcegitcommit: 4fffc66fac18fc4c80281e2a4afa9c4f2e1f7551
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2020
ms.locfileid: "94513687"
---
# <a name="guidelines-for-touch-targets"></a>觸控目標的指導方針

Windows 應用程式中的所有互動式 UI 元素必須夠大，才能讓使用者精確地存取和使用，不論裝置類型或輸入方法為何。

支援觸控輸入 (和觸控連絡人區域相對不精確的本質) 需要針對目標大小和控制項版面配置進行進一步的優化，因為觸控數位板所報告的較大、更複雜的輸入資料集是用來判斷使用者的預期 (或最可能) 的目標。

所有 UWP 控制項都是使用預設的觸控目標大小和版面配置所設計，可讓您建立視覺效果平衡且吸引人的應用程式，這些應用程式很熟悉、便於使用，並可激發信心。

在本主題中，我們將說明這些預設行為，讓您可以使用平臺控制項和自訂控制項來設計應用程式，以達到最大的可用性， (您的應用程式需要) 。

> **重要 API** ： [**Windows.UI.Core**](/uwp/api/Windows.UI.Core)、 [**Windows.UI.Input**](/uwp/api/Windows.UI.Input)、 [**Windows.UI.Xaml.Input**](/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Fluent 標準大小調整

「Fluent 標準大小調整」旨在提供資訊密度和使用者舒適度之間平衡。 實際上，畫面上所有的項目都會對齊 40x40 有效像素 (epx) 目標，其可讓 UI 元素貼齊格線並根據系統層級縮放適度調整。

> [!NOTE]
> 如需有效像素與縮放規模的詳細資訊，請參閱 [Windows 應用程式設計簡介](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 如需系統層級縮放的詳細資訊，請參閱[對齊、邊界、邊框距離](../layout/alignment-margin-padding.md)。

## <a name="fluent-compact-sizing"></a>Fluent 精簡大小調整

應用程式可以使用 *流暢的精簡大小* 來顯示較高層級的資訊密度。 Compact 調整大小會將 UI 元素對齊至 32x32 window.epx.codesnippet.copycode 目標，讓 UI 元素對齊更緊密的方格，並根據系統層級調整適當地調整規模。

### <a name="examples"></a>範例

您可以在頁面或格線層級套用精簡大小調整。

### <a name="page-level"></a>頁面層級

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>格線層級

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>目標大小

一般情況下，請將您的觸控目標大小設定為 7.5 mm 平方範圍 (在 135 PPI 顯示器上以 1.0 x 縮放高原) 40x40 圖元。 UWP 控制項通常會對齊 7.5 mm 觸控目標 (這會根據特定的控制項和任何常見的使用模式) 而有所不同。 如需詳細資訊，請參閱 [控制項大小和密度](../style/spacing.md) 。

這些建議目標大小可以根據自己的特殊情況而加以調整。 以下是要考量的一些事項：

- 觸碰的頻率-考慮將重複或經常按下的目標設為大於最小大小。
- 錯誤結果-如果發生錯誤，則會產生嚴重後果的目標：應該要有更大的填補，並從內容區域的邊緣進一步放置。 如果目標經常被點選，更是如此。
- 內容區域中的位置。
- 板型規格和螢幕大小。
- 手指狀況。
- 觸控視覺效果。

## <a name="related-articles"></a>相關文章

- [Windows 應用程式設計簡介](../basics/design-and-ui-intro.md)
- [控制項大小和密度](../style/spacing.md)
- [對齊、邊界、邊框間距](../layout/alignment-margin-padding.md)

### <a name="samples"></a>範例

- [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

### <a name="archive-samples"></a>封存範例

- [輸入：XAML 使用者輸入事件範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [輸入：裝置功能範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [輸入：觸控點擊測試範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 滾動、移動流覽和縮放範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [輸入：簡化的筆跡範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
* [輸入：操作和手勢範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX 觸控輸入範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
