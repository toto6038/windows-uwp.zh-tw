---
Description: 本主題描述在 Windows 執行階段應用程式中如何使用接觸幾何來預測觸控目標，並提供觸控目標的最佳做法。
title: 目標預測
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 34f8d15b971cc9ed286471010a21d1b44b84af13
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363464"
---
# <a name="guidelines-for-touch-targets"></a>觸控目標的指導方針

所有互動的 UI 項目，在通用 Windows 平台 (UWP) 應用程式中必須要夠大，精確地存取及使用，無論裝置類型，或是輸入方法的使用者。

支援觸控的輸入 （和觸控連絡資訊區域的相對較不精確的性質） 在較大且更複雜的輸入資料集報告觸控數位板用來判斷，需要進一步的最佳化，相對於目標的大小和控制項的版面配置使用者的預期 （或最可能） 目標。

所有 UWP 控制項設計預設觸控目標大小和可讓您建置的舒適、 容易使用，以視覺化方式平衡且吸引人的應用程式的版面配置，並激發信心。

本主題中，我們會說明這些預設行為，讓您可以設計您的應用程式使用平台控制項和自訂控制項 （應您的應用程式需要它們） 的最大可用性。

> **重要的 Api**:[**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)， [ **Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)， [ **Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Fluent 標準的調整大小

*Fluent 的標準規模*建立提供資訊密度和使用者緩和之間取得平衡。 實際上，在螢幕上的所有項目會對齊 40 x 40 有效像素 (epx) 目標，這可讓 UI 項目貼齊格線和調整適當地根據系統層級調整。

> [!NOTE]
>如需有效的像素與調整規模的詳細資訊，請參閱[UWP 應用程式的設計簡介](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 如需系統層級調整的詳細資訊，請參閱[對齊、 邊界、 邊框距離](../layout/alignment-margin-padding.md)。

## <a name="fluent-compact-sizing"></a>Fluent Compact 的調整大小

應用程式可以顯示較高層級的使用資訊密度*Fluent Compact 的調整大小*。 Compact 的調整大小會對齊 32 x 32 epx 目標，這可讓 UI 項目將對齊更緊密的方格和規模適當地取決於系統層級調整 UI 項目。

### <a name="examples"></a>範例

Compact 的調整大小可以套用在頁面或方格層級。

### <a name="page-level"></a>頁面層級

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>方格層級

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>目標大小

一般情況下，將觸控目標大小設定 7.5 mm 方形範圍 （在 x 調整高原 1.0 135 PPI 顯示器上 40 x 40 像素）。 一般而言，UWP 控制項配合 7.5 mm 觸碰目標 （這可以不同根據特定的控制項和任何一般使用模式）。 請參閱[控制大小和密度](../style/spacing.md)如需詳細資訊。

這些建議目標大小可以根據自己的特殊情況而加以調整。 以下是一些考量事項：

- 頻率修飾-請考慮將重複或經常按下大於最小大小的目標。
- 錯誤結果-有嚴重的後果，如果錯誤接觸到的目標應該有更大的填補，但更內容區域的邊緣。 如果目標經常被點選，更是如此。
- 在 [內容] 區域的位置。
- 構成要素和螢幕大小。
- 指的狀態。
- 觸碰視覺效果。

## <a name="related-articles"></a>相關文章

- [UWP app 設計](../basics/design-and-ui-intro.md)
- [控制項大小和密度](../style/spacing.md)
- [對齊、 邊界、 邊框間距](../layout/alignment-margin-padding.md)

### <a name="samples"></a>範例

- [基本的輸入的範例](https://go.microsoft.com/fwlink/p/?LinkID=620302)
- [低延遲的輸入的範例](https://go.microsoft.com/fwlink/p/?LinkID=620304)
- [使用者互動模式範例](https://go.microsoft.com/fwlink/p/?LinkID=619894)
- [焦點視覺效果範例](https://go.microsoft.com/fwlink/p/?LinkID=619895)

### <a name="archive-samples"></a>封存範例

- [輸入：XAML 使用者輸入的事件範例](https://go.microsoft.com/fwlink/p/?linkid=226855)
- [輸入：裝置功能的範例](https://go.microsoft.com/fwlink/p/?linkid=231530)
- [輸入：觸控的點擊測試範例](https://go.microsoft.com/fwlink/p/?linkid=231590)
- [捲動、 移動和縮放範例的 XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
- [輸入：簡化的手寫範例](https://go.microsoft.com/fwlink/p/?linkid=246570)
- [輸入：Windows 8 筆勢範例](https://go.microsoft.com/fwlink/p/?LinkId=264995)
- [輸入：操作和手勢 (C++) 範例](https://go.microsoft.com/fwlink/p/?linkid=231605)
- [DirectX 觸控的輸入的範例](https://go.microsoft.com/fwlink/p/?LinkID=231627)
