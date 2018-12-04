---
ms.assetid: 6e9b9ff2-234b-6f63-0975-1afb2d86ba1a
title: 組合效果
description: 效果 API 可讓開發人員自訂其 UI 的轉譯方式。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 75af433d80364485b0c12a9540c0d7bb471c4e28
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8477936"
---
# <a name="composition-effects"></a>組合效果

[**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) API 能夠套用即時效果至影像以及有可動畫效果屬性的 UI。 在本概觀中，我們會逐步說明允許套用效果至視覺化組合的可用功能。

為支援[通用 Windows 平台 (UWP)](https://msdn.microsoft.com/library/windows/apps/dn726767.aspx) 一致性以供開發人員在其應用程式中描述效果， 組合效果會利用 Win2D 的 IGraphicsEffect 介面 透過 [Microsoft.Graphics.Canvas.Effects](http://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) 命名空間來使用效果描述。

筆刷效果可用來將效果套用到一組現有的影像，以繪製應用程式的部分區域。 Windows 10 組合效果 API 著重在 SpriteVisual 上。 SpriteVisual 在色彩、影像和效果建立上提供彈性和相互作用。 SpriteVisual 是一種可利用筆刷填滿 2D 矩形的視覺化組合類型。 視覺化定義矩形的界線，筆刷則定義用來繪製矩形的像素。

筆刷效果用於視覺化組合樹狀結構，這種效果的內容來自效果圖形的輸出。 效果可以參考現有表面/紋理，但沒有其他組合樹狀結構的輸出。

搭配使用效果筆刷與 [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)，也可以將效果套用到 XAML UIElement。

## <a name="effect-features"></a>效果功能

- [效果程式庫](./composition-effects.md#effect-library)
- [鏈結效果](./composition-effects.md#chaining-effects)
- [動畫支援](./composition-effects.md#animation-support)
- [常數與動畫效果屬性](./composition-effects.md#constant-vs-animated-effect-properties)
- [使用獨立屬性的多個效果執行個體](./composition-effects.md#multiple-effect-instances-with-independent-properties)

### <a name="effect-library"></a>效果程式庫

目前組合支援下列效果：

| 效果               | 說明                                                                                                                                                                                                                |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2D 仿射轉換  | 套用 2D 仿射轉換矩陣至影像。 我們使用這種效果讓我們的效果[範例](http://go.microsoft.com/fwlink/?LinkId=785341)中的 Alpha 遮罩產生動畫效果。       |
| 算術複合 | 使用彈性的方程式結合兩個影像。 我們使用算術複合在我們的[範例](http://go.microsoft.com/fwlink/?LinkId=785341)中建立淡入與淡出效果。 |
| 混合效果         | 建立結合兩個影像的混合效果。 組合提供了 Win2D 中支援之 26 種[混合模式](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_Effects_BlendEffectMode.htm)的 21 種。        |
| 色彩來源         | 產生包含單色的影像。                                                                                                                                                                               |
| 複合            | 結合兩個影像。 組合提供了 Win2D 中支援之所有 13 種[複合模式](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasComposite.htm)。                                              |
| 對比             | 增加或減少影像的對比。                                                                                                                                                                           |
| 曝光             | 增加或減少影像的曝光。                                                                                                                                                                           |
| 灰階            | 將影像轉換為灰色。                                                                                                                                                                                   |
| 色差補正移轉       | 藉由套用各頻道色差補正移轉功能改變影像的色彩。                                                                                                                                           |
| 色相旋轉           | 旋轉影像的色相值改變影像的色彩。                                                                                                                                                                   |
| 負片               | 反轉影像的色彩。                                                                                                                                                                                            |
| 飽和度             | 改變影像的飽和度。                                                                                                                                                                                         |
| 懷舊                | 將影像轉換成懷舊色調。                                                                                                                                                                                          |
| 色溫和色調 | 調整影像的色溫和/或色調。                                                                                                                                                                           |

如需詳細資訊，請參閱 Win2D 的 [Microsoft.Graphics.Canvas.Effects](http://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm) 命名空間。 組合中不支援的效果會標示為 \[NoComposition\]。

### <a name="chaining-effects"></a>鏈結效果

效果可以鏈結，允許應用程式同時在影像上使用多個效果。 效果圖形可以支援可互相參照的多個效果。 當描述您的效果時，只需新增效果做為您效果的輸入。

```cs
IGraphicsEffect graphicsEffect =
new Microsoft.Graphics.Canvas.Effects.ArithmeticCompositeEffect
{
  Source1 = new CompositionEffectSourceParameter("source1"),
  Source2 = new SaturationEffect
  {
    Saturation = 0,
    Source = new CompositionEffectSourceParameter("source2")
  },
  MultiplyAmount = 0,
  Source1Amount = 0.5f,
  Source2Amount = 0.5f,
  Offset = 0
}
```

上述範例說明一個有二項輸入的算數複合效果。 第二個輸入包含飽和度屬性為 0.5 的效果。

### <a name="animation-support"></a>動畫支援

效果屬性支援動畫，您可以在效果編譯期間指定用動畫顯示，且「一律」做為常數的效果屬性。 可展示動畫的屬性是透過表單 "effect name.property name" 的字串來指定。 這些屬性可以透過多個效果的具現化，獨立產生動畫效果。

### <a name="constant-vs-animated-effect-properties"></a>常數與動畫效果屬性

您可以在效果編譯期間，將效果屬性指定為動態或「一律」做為常數的屬性。 動態屬性是透過「<effect name>.<property name>」格式的字串來指定。 動態屬性可以設為特定值，或者也可以使用組合動畫系統來以動畫效果顯示。

在編譯上述效果時，您有彈性可以選擇飽和度一律等於 0.5，或將它設為動態值並以動態或動畫效果顯示。

編譯固定飽和度的效果：

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
```

編譯動態飽和度的效果：

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect, new[]{"SaturationEffect.Saturation"});
_catEffect = effectFactory.CreateBrush();
_catEffect.SetSourceParameter("mySource", surfaceBrush);
_catEffect.Properties.InsertScalar("saturationEffect.Saturation", 0f);
```

上述效果的飽和度屬性可接著設為靜態值，或者使用 Expression 或 ScalarKeyFrame 動畫來以動畫效果顯示。

您可以建立如下的 ScalarKeyFrame，來以動畫顯示 Saturation 屬性：

```cs
ScalarKeyFrameAnimation effectAnimation = _compositor.CreateScalarKeyFrameAnimation();
            effectAnimation.InsertKeyFrame(0f, 0f);
            effectAnimation.InsertKeyFrame(0.50f, 1f);
            effectAnimation.InsertKeyFrame(1.0f, 0f);
            effectAnimation.Duration = TimeSpan.FromMilliseconds(2500);
            effectAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
```

啟動效果的 Saturation 屬性動畫的方法：

```cs
catEffect.Properties.StartAnimation("saturationEffect.Saturation", effectAnimation);
```

請參閱[去飽和度 - 動畫範例](http://go.microsoft.com/fwlink/?LinkId=785342)來了解使用主要畫面格以動畫顯示的效果屬性，以及參閱 [AlphaMask 範例](http://go.microsoft.com/fwlink/?LinkId=785343)來了解效果和運算式的使用方式。

### <a name="multiple-effect-instances-with-independent-properties"></a>使用獨立屬性的多個效果執行個體

藉由在效果編譯期間將參數指定為動態，該參數則可以在各效果執行個體的基礎上進行變更。 這可讓兩個視覺效果使用相同的效果，但是以不同的效果屬性呈現。 如需詳細資訊，請參閱 ColorSource 和 Blend [範例](http://go.microsoft.com/fwlink/?LinkId=785344)。

## <a name="getting-started-with-composition-effects"></a>開始使用組合效果

這個快速入門教學課程會示範如何使用效果的一些基本功能。

- [安裝 Visual Studio](./composition-effects.md#installing-visual-studio)
- [建立新的專案](./composition-effects.md#creating-a-new-project)
- [安裝 Win2D](./composition-effects.md#installing-win2d)
- [設定您的組合基本知識](./composition-effects.md#setting-your-composition-basics)
- [建立 CompositionSurface 筆刷](./composition-effects.md#creating-a-compositionsurface-brush)
- [建立、編譯以及套用效果](./composition-effects.md#creating-compiling-and-applying-effects)

### <a name="installing-visual-studio"></a>安裝 Visual Studio

- 如果您沒有安裝支援的 Visual Studio 版本，請移至[這裡](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)的 Visual Studio 下載頁面。

### <a name="creating-a-new-project"></a>建立新的專案

- 移至 [檔案] -&gt; [新增] -&gt; [專案]...
- 選取 [Visual C#]
- 建立 [空白的應用程式 \(Windows 通用\)] \(Visual Studio 2015\)
- 輸入您選擇的專案名稱
- 按一下 [確定]

### <a name="installing-win2d"></a>安裝 Win2D

Win2D 是以 Nuget.org 套件發行，且必須安裝後才可以使用效果。

有兩種版本的套件，其中一個適用於 Windows 10，另一個適用於 Windows 8.1。 若是組合效果，則您必須使用 Windows 10 版本。

- 移至 [工具] → [NuGet 套件管理員] → [管理方案的 NuGet 套件] 啟動 NuGet 套件管理員。
- 搜尋 "Win2D"，然後針對您的 Windows 目標版本選取適當的套件。 因為 Windows.UI。 組合支援 Windows 10 (不支援 8.1)，選取 Win2D.uwp。
- 接受授權合約
- 按一下 [關閉]

在接下來的幾個步驟中，我們會使用組合 API，把移除所有飽和度的飽和度效果套用至這個貓咪影像。 在這個模型中，效果會在建立後套用至影像。

![來源影像](images/composition-cat-source.png)
### <a name="setting-your-composition-basics"></a>設定您的組合基本知識

如需如何設定 Windows.UI.Composition 撰寫器、根 ContainerVisual，並與 Core Window 產生關聯的範例，請參閱 GitHub 上的[組合視覺化樹狀結構範例](http://go.microsoft.com/fwlink/?LinkId=785345)。

```cs
_compositor = new Compositor();
_root = _compositor.CreateContainerVisual();
_target = _compositor.CreateTargetForCurrentView();
_target.Root = _root;
_imageFactory = new CompositionImageFactory(_compositor)
Desaturate();
```

### <a name="creating-a-compositionsurface-brush"></a>建立 CompositionSurface 筆刷

```cs
CompositionSurfaceBrush surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(surfaceBrush);
```

### <a name="creating-compiling-and-applying-effects"></a>建立、編譯以及套用效果

1. 建立圖形效果

    ```cs
    var graphicsEffect = new SaturationEffect
    {
      Saturation = 0.0f,
      Source = new CompositionEffectSourceParameter("mySource")
    };
    ```

1. 編譯效果及建立筆刷效果

    ```cs
    var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);

    var catEffect = effectFactory.CreateBrush();
    catEffect.SetSourceParameter("mySource", surfaceBrush);
    ```

1. 在組合樹狀結構中建立 SpriteVisual 並套用效果

    ```cs
    var catVisual = _compositor.CreateSpriteVisual();
      catVisual.Brush = catEffect;
      catVisual.Size = new Vector2(219, 300);
      _root.Children.InsertAtBottom(catVisual);
    }
    ```

1. 建立您要載入的影像來源。

    ```cs
    CompositionImage imageSource = _imageFactory.CreateImageFromUri(new Uri("ms-appx:///Assets/cat.png"));
    CompositionImageLoadResult result = await imageSource.CompleteLoadAsync();
    if (result.Status == CompositionImageLoadStatus.Success)
    ```

1. 在 SpriteVisual 上調整大小並粉刷表面

    ```cs
    brush.Surface = imageSource.Surface;
    ```

1. 執行您的應用程式 – 您的結果應該是一隻去除飽和度的貓：

![去除飽和度的影像](images/composition-cat-desaturated.png)

## <a name="more-information"></a>其他資訊

- [Microsoft – 組合 GitHub](https://github.com/Microsoft/composition)
- [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878)
- [Twitter 上的 Windows 組合小組](https://twitter.com/wincomposition)
- [組合概觀](https://blogs.windows.com/buildingapps/2015/12/08/awaken-your-creativity-with-the-new-windows-ui-composition/)
- [視覺化樹狀結構基本知識](composition-visual-tree.md)
- [組合筆刷](composition-brushes.md)
- [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)
- [動畫概觀](composition-animation.md)
- [組合原生 DirectX 和 Direct2D 與 BeginDraw 和 EndDraw 的交互操作](composition-native-interop.md)
