---
author: mijacobs
description: "顯色是一種新的互動模型，有助於讓更多人注意與喜好您的應用程式。"
title: "顯色"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.openlocfilehash: d50e3f47faad5fff0ef461a4b5312127a0b9ec9c
ms.sourcegitcommit: 0d5b3daddb3ae74f91178c58e35cbab33854cb7f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2017
---
# <a name="reveal"></a>顯色

> [!IMPORTANT]
> 本文說明尚未發佈但可能在正式發行前大幅度修改的功能。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

顯色是一種光源效果，有助於讓應用程式的互動元素更具深度感並吸引注意力。

> **重要的 API**：[RevealBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)、[RevealBackgroundBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush)、[RevealBorderBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush)、[RevealBrushHelper 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper)、[VisualState 類別](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

顯色行為的運作方式是當滑鼠或焦點矩形停留在所需的區域上時，會顯示主角 (或焦點) 內容附近的一些元素。

![顯色視覺效果](images/Nav_Reveal_Animation.gif)

透過公開物件四周的隱藏邊框，顯色讓使用者能更進一步了解他們正在互動的空間，並幫助他們了解可用的動作。 這對清單控制項以及具有背板的控制項尤其重要。

## <a name="reveal-and-the-fluent-design-system"></a>顯色和 Fluent 設計系統

 Fluent 設計系統協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 顯色是將光源加入應用程式中的 Fluent 設計系統元件。 

## <a name="what-is-reveal"></a>什麼是顯色？

顯色有兩個主要的視覺元件：**暫留顯色**行為與**邊框顯色**行為。

![顯色層級](images/RevealLayers.png)

暫留顯色直接與正在暫留 (透過指標或焦點輸入) 的內容繫結，並會在暫留或聚焦的項目周圍套用柔和的光暈形狀，讓您知道您可以與該項目互動。

邊框顯色會套用到焦點項目與附近的項目。 這會讓您看到這些附近物件會採取與目前所聚焦物件類似的動作。

顯色配方明細為：

- 邊框顯色將會在所有內容的頂端，但在指定的邊緣上
- 文字與內容將直接顯示在邊框顯色下方
- 暫留顯色將位在內容與文字下方
- 背板 (開啟與啟用暫留顯色)
- 背景 (控制項背景)

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->

## <a name="how-to-use-it"></a>如何使用

當您需要游標時，顯色會在其周圍顯示幾何形狀，並會在您移動後不著痕跡地淡出。

顯色最適用於當在已暗示有框線與背板的應用程式主要內容 (主角內容) 上啟用時。 此外，顯色也應用於集合或類似清單的控制項。

## <a name="controls-that-automatically-use-reveal"></a>自動使用顯色的控制項

- [**ListView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)

## <a name="enabling-reveal-on-other-common-controls"></a>在其他常見控制項上啟用顯色

如果您有一個應該套用顯色的案例 (這些控制項為主要內容和/或用於清單或集合方向中)，我們提供了選擇加入資源樣式，讓您能夠針對這幾類的狀況啟用顯色。

這些控制項預設沒有顯色，因為它們是較小的控制項，且通常是您應用程式主要焦點的協助程式控制項；但每個應用程式都不相同，如果在您的應用程式中最常使用這些控制項，我們已提供一些樣式加以協助：

| 控制項名稱   | 資源名稱 |
|----------|:-------------:|
| Button |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |
| ComboBoxItem | ComboxBoxItemRevealStyle |

若要套用這些樣式，只需更新 Style 屬性，如下所示：

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

> [!NOTE]
> 16190 版本的 SDK 不會自動提供這些樣式。 若要取得這些樣式，您必須手動從 generic.xaml 複製到應用程式中。 Generic.xaml 通常位於 C:\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\10.0.16190.0\Generic。 日後的組建將會修正此問題。 

## <a name="enabling-reveal-on-custom-controls"></a>在自訂控制項上啟用顯色

若要在自訂控項或重新樣板化控制項上啟用顯色，您可以在該控制項範本的 \[視覺狀態\] 中移至該控制項的樣式，然後在根格線上指定顯色：

```xaml
<VisualState x:Name="PointerOver">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="PointerOver" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPointerOver}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}"/>
  </VisualState.Setters>
</VisualState>
```

若要得到顯色的拉提效果，您必須也將相同的 RevealBrushHelper 新增至 PressedState：

```xaml
<VisualState x:Name="Pressed">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="Pressed" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPressed}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}"/>
  </VisualState.Setters>
</VisualState>
```


使用我們系統資源顯色，表示我們將為您處理所有的佈景主題變更。

## <a name="dos-and-donts"></a>可行與禁止事項
- 讓使用者可以採取動作的元素上有顯色 - 顯色不應是靜態內容
- 在資料清單或集合中顯示顯色
- 將顯色套用到以背板清楚包含的內容
- 不要在您無法互動的靜態背景、文字或影像上顯示顯色
- 不要將顯色套用到不相關的浮動內容
- 不要在一次性、獨立的情況中 (如內容對話方塊或通知) 使用顯色
- 不要在安全決策中使用顯色，因為可能會轉移對您必須提供給使用者之訊息的注意力

## <a name="related-articles"></a>相關文章

- [RevealBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [**壓克力**](acrylic.md)
- [**組合效果**](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
