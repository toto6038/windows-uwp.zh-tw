---
title: 撰寫陰影
description: 陰影 Api 可讓您加入 UI 內容的動態可自訂的陰影。
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4a47a5f8ffca1d9ca2ddab05fe0baf2f85977d7f
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318186"
---
# <a name="shadows-in-windows-ui"></a>Windows UI 中的陰影

[DropShadow](/uwp/api/Windows.UI.Composition.DropShadow)類別提供建立可套用至一個可設定陰影的方式[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)或是[LayerVisual](/uwp/api/windows.ui.composition.layervisual) （樹狀子目錄的視覺效果）。 由於是慣用的視覺分層中的物件，就可以使用 CompositionAnimations 使用動畫 DropShadow 的所有屬性。

## <a name="basic-drop-shadow"></a>基本的陰影

若要建立基本的陰影，只要建立新的 DropShadow 和其關聯到您的視覺效果。 陰影是預設的矩形。 一組標準屬性的可調整的外觀及操作您的陰影。

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

![使用基本 DropShadow 矩形視覺效果](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>塑造陰影

有幾種方式來定義您 DropShadow 圖形：

- **使用預設**-DropShadow 圖形 CompositionDropShadowSourcePolicy 的 'Default' 模式所定義的預設。 SpriteVisual，預設值為矩形除非提供遮罩。 LayerVisual，預設值為繼承使用視覺效果的筆刷的 alpha 的遮罩。
- **設定遮罩**– 您可以設定[遮罩](/uwp/api/windows.ui.composition.dropshadow.mask)屬性來定義陰影的不透明度遮罩。
- **指定要使用繼承遮罩**– 設定[SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy)屬性，以使用[CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)。 若要使用的視覺效果的筆刷的 alpha 從產生的遮罩 InheritFromVisualContent。

## <a name="masking-to-match-your-content"></a>遮罩，以符合您的內容

如果您想要與視覺效果的內容，您可以使用視覺效果的筆刷陰影遮罩屬性，或是設定為自動繼承內容中的遮罩陰影比您陰影。 如果使用 LayerVisual，陰影將預設繼承的遮罩。

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

![已連接的 web 與遮罩的下拉式陰影的映像](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>使用替代的遮罩

在某些情況下，您可能想要圖形的陰影，使它不符合您的視覺效果內容。 若要達到此效果，您必須明確設定使用 alpha 使用筆刷的遮罩屬性。

在下列範例中，我們會載入兩個介面-一個視覺化內容，陰影遮罩的另一個：

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

![已連接的 web 與遮罩的圓形下拉式陰影的映像](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>建立動畫

為視覺分層中的標準，就可以使用組合動畫使用動畫 DropShadow 屬性。 下面我們修改上面建立動畫之陰影柔化半徑還要範例程式碼。

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>在 XAML 中的陰影

如果您想要為更複雜的架構元素加上陰影，有幾種方式，與 XAML 和組合之間的陰影互通：

1. 使用[DropShadowPanel](https://github.com/windows-toolkit/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs)可用 Windows 社群工具組中。 請參閱[DropShadowPanel 文件](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel)如需有關如何使用它。
1. 建立視覺效果，以做為陰影主應用程式和繫結到 XAML 講義視覺效果。
1. 使用組合範例圖庫[SamplesCommon](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SamplesCommon/SamplesCommon) CompositionShadow 的自訂控制項。 請參閱此處的範例使用方式。

## <a name="performance"></a>效能

雖然視覺分層會有許多最佳化位置進行有效率且可使用的效果，則產生 shadows 可能會相當耗費資源的作業，根據您設定哪些選項。 以下是層級 '的高成本' 不同類型的陰影。 請注意，雖然某些 shadows 可能很昂貴，但它們仍然可能適合在某些情況下盡量不要使用。

陰影特性| 成本
------------- | -------------
矩形    | 低
Shadow.Mask      | 高
CompositionDropShadowSourcePolicy.InheritFromVisualContent | 高
靜態柔化半徑 | 低
以動畫顯示柔化半徑 | 高

## <a name="additional-resources"></a>其他資源

- [撰寫 DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub 存放庫](https://github.com/microsoft/WindowsCompositionSamples)