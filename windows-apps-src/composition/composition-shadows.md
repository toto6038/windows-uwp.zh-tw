---
author: daneuber
title: 組合陰影
description: 陰影 Api 可讓您將動態可自訂的陰影新增至 UI 內容。
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 84e12d6c3e25a18902aaa55011949dd5b5ff97ca
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2018
ms.locfileid: "3373734"
---
# <a name="shadows-in-windows-ui"></a>Windows UI 中的陰影

[DropShadow](/uwp/api/Windows.UI.Composition.DropShadow)類別提供方法來建立可設定的陰影可套用至磁碟[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)或[LayerVisual](/uwp/api/windows.ui.composition.layervisual) （樹狀子目錄的視覺效果）。 因為是自訂的視覺層中的物件，就可以使用 CompositionAnimations 使用動畫 DropShadow 的所有屬性。

## <a name="basic-drop-shadow"></a>基本的陰影

若要建立基本的陰影，只要建立新的 DropShadow，並將它以您的 visual 產生關聯。 陰影預設是矩形。 調整您陰影外觀和操作方式提供一組標準的屬性。

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

![矩形視覺基本 DropShadow](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>罰陰影

有幾個您 DropShadow 為定義圖形的方式：

- **使用預設值**-預設 DropShadow 圖形是由定義 CompositionDropShadowSourcePolicy 上的 「 預設 」 模式。 SpriteVisual 的預設值是矩形除非遮罩會提供。 LayerVisual，預設值是要繼承使用的 「 視覺效果的筆刷的 alpha 遮罩。
- **設定遮罩**– 您可以設定來定義不透明度遮罩，陰影的[遮罩](/uwp/api/windows.ui.composition.dropshadow.mask)屬性。
- **指定要使用繼承的遮罩**– 設定要使用[CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy) [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy)屬性。 若要使用的 「 視覺效果的筆刷 alpha，從產生的遮罩 InheritFromVisualContent。

## <a name="masking-to-match-your-content"></a>遮罩，以符合您的內容

如果您想要以符合 「 視覺效果的內容，您可以使用 「 視覺效果的筆刷適用於您陰影遮罩的屬性，或設定要自動從內容繼承遮罩，陰影您陰影。 如果使用 LayerVisual，陰影將預設會繼承遮罩。

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

![已連線的網頁影像遮罩的陰影](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>使用替代遮罩

在某些情況下，您可能會想要打造陰影，如此它不符合視覺效果的內容。 若要達到這個效果，您將需要明確地將使用筆刷與 alpha 遮罩屬性。

在下列範例中，我們載入兩個表面的另一個用於視覺化內容，一個用於陰影遮罩：

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

![已連線的網頁影像已被遮蔽的圓形陰影](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>產生動畫效果

因為是標準視覺層中，就可以使用組合動畫皆使用動畫 DropShadow 屬性。 以下，我們可以修改噴灑上面的範例以動畫顯示模糊半徑陰影中的程式碼。

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

如果您想要新增陰影更複雜的架構項目時，有幾種方式與陰影 XAML 與組合之間的互通性：

1. 使用[DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs)於 Windows 社群工具組。 如何使用它，請參閱[DropShadowPanel 文件](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel)，如需詳細資訊。
1. 建立以使用與陰影主機與將它繫結至 XAML 交出 Visual 的視覺效果。
1. 使用組合範例資源庫的[SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon)自訂 CompositionShadow 控制項。 請參閱下列範例使用方式。

## <a name="performance"></a>效能

視覺層中的位置，讓效果有效率及使用有許多最佳化，雖然產生陰影可以是相當耗費資源的作業取決於您設定哪些選項。 以下是層級 」 的高成本' 不同類型的陰影。 請注意，雖然特定陰影可能高度耗費資源他們仍然可能適合在某些情況下謹慎使用。

陰影特性| 費用
------------- | -------------
矩形    | 低
Shadow.Mask      | 高
CompositionDropShadowSourcePolicy.InheritFromVisualContent | 高
靜態模糊 Radius | 低
模糊半徑產生動畫效果 | 高

## <a name="additional-resources"></a>其他資源

- [組合 DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub 存放庫](https://github.com/Microsoft/WindowsUIDevLabs)