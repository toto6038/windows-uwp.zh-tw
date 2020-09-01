---
title: 組合陰影
description: 影子 Api 可讓您將動態可自訂陰影新增至 UI 內容。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29ca04ac3faf3a2884bcc2346177f49222cf786e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166422"
---
# <a name="shadows-in-windows-ui"></a>Windows UI 中的陰影

[DropShadow](/uwp/api/Windows.UI.Composition.DropShadow)類別提供建立可設定的陰影，以套用至視覺效果) 之[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)或[LayerVisual](/uwp/api/windows.ui.composition.layervisual) (子樹的方法。 如同視覺分層中物件的慣例，DropShadow 的所有屬性都可以使用 CompositionAnimations 來進行動畫。

## <a name="basic-drop-shadow"></a>基本陰影

若要建立基本陰影，只要建立新的 DropShadow，並將它與您的視覺效果建立關聯。 根據預設，陰影是矩形。 您可以使用一組標準的屬性來調整陰影的外觀和風格。

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![具有基本 DropShadow 的矩形視覺效果](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>塑造陰影

有幾種方式可以為您的 DropShadow 定義圖形：

- **使用預設值** -根據預設，DropShadow 圖形是在 CompositionDropShadowSourcePolicy 上以「預設」模式定義。 若為 SpriteVisual，除非提供遮罩，否則預設值為矩形。 針對 LayerVisual，預設值是使用視覺效果筆刷的 Alpha 來繼承遮罩。
- **設定遮罩** –您可以設定 [mask](/uwp/api/windows.ui.composition.dropshadow.mask) 屬性來定義陰影的不透明度遮罩。
- **指定使用繼承的遮罩** –將 [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) 屬性設定為使用 [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)。 InheritFromVisualContent，以使用從視覺效果之筆刷的 Alpha 產生的遮罩。

## <a name="masking-to-match-your-content"></a>遮罩以符合您的內容

如果您想要讓您的陰影符合視覺效果的內容，您可以針對陰影遮罩屬性使用視覺效果的筆刷，或將陰影設定為自動繼承內容中的遮罩。 如果使用 LayerVisual，陰影預設會繼承遮罩。

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![具有遮罩陰影的連線 web 影像](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>使用替代遮罩

在某些情況下，您可能會想要塑造陰影，使其不符合視覺效果的內容。 若要達成這個效果，您必須使用具有 Alpha 的筆刷來明確設定 Mask 屬性。

在下列範例中，我們會載入兩個介面：一個適用于視覺內容，另一個用於陰影遮罩：

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![具有圓形遮罩投影的已連接網路影像](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>動畫

如同視覺分層中的標準，DropShadow 屬性可以使用複合動畫來進行動畫。 在下面，我們會修改上述還要範例中的程式碼，以動畫顯示陰影的模糊半徑。

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>XAML 中的陰影

如果您想要將陰影新增至更複雜的 framework 元素，有幾種方式可在 XAML 和組合之間與陰影進行 interop：

1. 使用 Windows 社群工具組中提供的 [DropShadowPanel](https://github.com/windows-toolkit/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) 。 如需如何使用的詳細資訊，請參閱 [DropShadowPanel 檔](/windows/uwpcommunitytoolkit/controls/DropShadowPanel) 。
1. 建立要做為陰影主控制項的視覺效果，& 將其系結至 XAML 講義視覺效果。
1. 使用組合範例庫的 [SamplesCommon](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SamplesCommon/SamplesCommon) 自訂 CompositionShadow 控制項。 請參閱此處的範例以瞭解用法。

## <a name="performance"></a>效能

雖然視覺分層具有許多優化功能，可讓效果有效率且可供使用，但根據您設定的選項而定，產生陰影可能是相當昂貴的作業。 以下是不同類型陰影的高階「成本」。 請注意，雖然某些陰影的成本可能很高，但在某些情況下，可能仍適合謹慎使用。

陰影特性| 成本
------------- | -------------
矩形    | 低
遮蔽      | 高
CompositionDropShadowSourcePolicy.InheritFromVisualContent | 高
靜態模糊半徑 | 低
製作模糊半徑的動畫 | 高

## <a name="additional-resources"></a>其他資源

- [撰寫 DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub 存放庫](https://github.com/microsoft/WindowsCompositionSamples)