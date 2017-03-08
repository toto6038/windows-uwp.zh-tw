---
author: scottmill
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: "組合筆刷"
description: "筆刷會使用其輸出來繪製 Visual 的區域。 不同的筆刷有不同類型的輸出。"
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 9affb4fab1931c7584d86bfb07797345788c28f9
ms.lasthandoff: 02/07/2017

---
# <a name="composition-brushes"></a>組合筆刷

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

筆刷會使用其輸出來繪製 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 的區域。 不同的筆刷有不同類型的輸出。 「組合 API」提供三種筆刷類型：

-   [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) 會使用純色繪製視覺效果
-   [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 會使用組合表面的內容來繪製視覺效果
-   [**CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) 會使用組合效果的內容來繪製視覺效果

所有筆刷都會繼承 [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398)；它們是由 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 直接或間接建立，並且是與裝置無關的資源。 雖然筆刷與裝置無關，但是 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 和 [**CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) 會使用來自與裝置相關的組合表面內容來繪製 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)。

-   [先決條件](./composition-brushes.md#prerequisites)
-   [色彩基本知識](./composition-brushes.md#color-basics)
    -   [Alpha 模式](./composition-brushes.md#alpha-modes)
-   [使用色彩筆刷](./composition-brushes.md#using-color-brush)
-   [使用表面筆刷](./composition-brushes.md#using-surface-brush)
-   [設定延展與對齊方式](./composition-brushes.md#configuring-stretch-and-alignment)

## <a name="prerequisites"></a>先決條件

這個概觀假設您已熟悉基本「組合」應用程式的結構，如[組合 UI](visual-layer.md) 所述。

## <a name="color-basics"></a>色彩基本知識

在使用 [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) 進行繪製之前，您必須先選擇色彩。 「組合 API」使用「Windows 執行階段」結構 (即色彩) 來呈現色彩。 「色彩」結構使用 sRGB 編碼。 sRGB 編碼將色彩分為四個頻道：Alpha、紅色、綠色及藍色。 每個元件都是以一般範圍為 0.0 到 1.0 的浮點值代表。 值為 0.0 時，表示完全沒有該色彩，而值為 1.0 時，則表示全部為該色彩。 就 Alpha 元件而言，0.0 代表完全透明的色彩，1.0 則代表完全不透明的色彩。

### <a name="alpha-modes"></a>Alpha 模式

[**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) 中的色彩值一律會解譯為直接 Alpha。

## <a name="using-color-brush"></a>使用色彩筆刷

若要建立色彩筆刷，請呼叫 Compositor.[**CreateColorBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositor.createcolorbrush.aspx) 方法，這會傳回 [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399)。 **CompositionColorBrush** 的預設色彩是 \#00000000。 以下圖例和程式碼示範一個小型的視覺化樹狀結構來建立矩形，此矩形是以黑色筆刷勾勒，並以色彩值為 0x9ACD32 的單色筆刷繪製。

![CompositionColorBrush](images/composition-compositioncolorbrush.png)
```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual1, visual2;
CompositionColorBrush _blackBrush, _greenBrush; 

_compositor = new Compositor();
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
visual1 = _compositor.CreateSpriteVisual();
visual1.Brush = _blackBrush;
visual1.Size = new Vector2(156, 156);
visual1.Offset = new Vector3(0, 0, 0);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
Visual2 = _compositor.CreateSpriteVisual();
Visual2.Brush = _greenBrush;
Visual2.Size = new Vector2(150, 150);
Visual2.Offset = new Vector3(3, 3, 0);
```

不同於其他的筆刷，建立 [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) 是相當不耗費資源的操作。 您可以在每次轉譯時都建立 **CompositionColorBrush** 物件，這對效能幾乎沒有什麼影響。

## <a name="using-surface-brush"></a>使用表面筆刷

[**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 會使用組合表面 (由 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) 物件表示) 來繪製視覺效果。 下圖顯示一個以甘草糖點陣圖繪製並使用 D2D 轉譯到 **ICompositionSurface** 的方形視覺效果。

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png) 第一個範例會將組合表面初始化以與筆刷搭配使用。 組合表面是使用協助程式方法 (採用 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 和 Url 做為字串的 LoadImage) 建立的。 它會從 Url 載入影像、將該影像轉譯到 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819)，然後將該表面設定為 **CompositionSurfaceBrush** 的內容。 請注意，**ICompositionSurface** 只會以機器碼公開，因此 LoadImage 方法是在機器碼中實作。

```cs
LoadImage(Brush,
          "ms-appx:///Assets/liqorice.png");
```

若要建立表面筆刷，請呼叫 Compositor.[**CreateSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositor.createsurfacebrush.aspx) 方法。 這個方法會傳回 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 物件。 以下程式碼說明可用來以 **CompositionSurfaceBrush** 的內容繪製視覺效果的程式碼。

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual;
CompositionSurfaceBrush _surfaceBrush;

_surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(_surfaceBrush, "ms-appx:///Assets/liqorice.png");
visual.Brush = _surfaceBrush;
```

## <a name="configuring-stretch-and-alignment"></a>設定延展與對齊方式

有時，[**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 的 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) 內容不會完全填滿所繪製之視覺效果的區域。 當發生這種情況時，「組合 API」會使用筆刷的 [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx)、[**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) 及 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) 模式設定來決定如何填滿剩餘的區域。

-   [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) 和 [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) 的類型是浮點數，並且可用來控制筆刷在視覺邊界內的位置。
    -   值 0.0 會將筆刷的左/上角與視覺效果的左/上角對齊
    -   值 0.5 會將筆刷中央與視覺效果的中央對齊
    -   值 1.0 會將筆刷的右/下角與視覺效果的右/下角對齊
-   [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) 屬性接受以下由 [**CompositionStretch**](https://msdn.microsoft.com/library/windows/apps/Dn706786) 列舉 所定義的值：
    -   None：筆刷不會延展以填滿視覺邊界。 請小心使用此「延展」設定：如果筆刷比視覺邊界大， 筆刷的內容將會受到裁剪。 您可以使用 [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) 和 [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) 屬性來控制用來繪製視覺邊界 的筆刷部分。
    -   Uniform：筆刷會配合視覺邊界調整大小；會保留筆刷的外觀比例。 這是預設值。
    -   UniformToFill：筆刷會調整大小來完全填滿視覺邊界；會保留筆刷的外觀比例。
    -   Fill：筆刷會配合視覺邊界調整大小。 由於筆刷的高度和寬度會單獨調整，因此可能不會保留筆刷的原始外觀比例。 也就是說，筆刷可能會為了完全填滿視覺邊界而扭曲變形。

 

## <a name="related-topics"></a>相關主題
[組合原生 DirectX 和 Direct2D 與 BeginDraw 和 EndDraw 的交互操作](composition-native-interop.md)





