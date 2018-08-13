---
author: jwmsft
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: 組合筆刷
description: 筆刷會使用其輸出來繪製 Visual 的區域。 不同的筆刷有不同類型的輸出。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 103ecd24c35d75d3ea1d305d7294048dc628d2e2
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "1038568"
---
# <a name="composition-brushes"></a>組合筆刷
因為它已由筆刷繪製從 UWP 應用程式顯示於螢幕上的每個項目是可見的。 筆刷可以讓您以簡單、 實心色彩從延伸到影像或複雜的效果鏈結的繪圖內容繪製使用者介面 (UI) 物件。 本主題介紹的概念與 CompositionBrush painting。

請注意，使用 XAML UWP 應用程式時可選擇繪製[XAML 筆刷](/windows/uwp/design/style/brushes)或[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush)UIElement。 通常是更輕鬆地與建議您選擇 XAML 筆刷如果 XAML 筆刷支援您的案例。 例如，建立動畫按鈕，變更的文字或含有映像的圖案的填滿的色彩。 換句話說，如果您嘗試執行不支援的 like 與動畫的遮罩或動畫的九個格線拉長或效果鏈結 painting XAML 筆刷的某個項目，可以使用 CompositionBrush 繪製透過[UIElementXamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)。

使用視覺圖層時, 必須使用 CompositionBrush 繪製[SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)的區域。

-   [必要條件](./composition-brushes.md#prerequisites)
-   [[小畫家] 與 [CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [繪製具有純色](./composition-brushes.md#paint-with-a-solid-color)
    -   [繪製具有線性漸層](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [繪製具有映像](./composition-brushes.md#paint-with-an-image)
    -   [繪製具有自訂繪圖](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [繪製的影片](./composition-brushes.md#paint-with-a-video)
    -   [繪製具有篩選效果](./composition-brushes.md#paint-with-a-filter-effect)
    -   [搭配使用的不透明度遮罩 CompositionBrush 繪製](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [繪製具有使用 NineGrid 拉大 CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [使用背景像素為單位的 [小畫家]](./composition-brushes.md#paint-using-background-pixels)
-   [結合 CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [使用 XAML 筆刷和比較 CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [相關主題](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>必要條件
本概觀假設您已熟悉的基本組合應用程式結構[視覺層概觀 （英文）](visual-layer.md)中所述。

## <a name="paint-with-a-compositionbrush"></a>[小畫家] 與 [CompositionBrush

[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) 「 複製 」 具有其輸出結果的區域。 不同的筆刷有不同類型的輸出。 某些筆刷繪製純色、 漸層、 影像、 自訂繪圖或效果與其他人與區域。 也有特殊的筆刷修改其他筆刷的行為。 例如，不透明度遮罩可用來控制哪些區域繪製由 CompositionBrush 或九格線可以用來控制繪製區域時套用至 CompositionBrush 拉大。 CompositionBrush 可以是下列類型其中一項：

|類別                                   |詳細資料                                         |最早出現在|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |使用純色繪製區域                        |Windows 10 年 11 月更新 (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |繪製的[ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)內容區域|Windows 10 年 11 月更新 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |繪製具有組合影響的內容區域 |Windows 10 年 11 月更新 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |複製搭配使用的不透明度遮罩 CompositionBrush visual |Windows 10 紀念日 Update (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |搭配使用 NineGrid 拉大 CompositionBrush 繪製區域 |Windows 10 紀念日 Update sdk (14393) （英文)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|使用線性漸層繪製區域                    |Windows 10 屬於建立者更新 (內部 Preview sdk （英文))
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |繪製區域方式進行取樣背景像素的 [應用程式或直接背後的應用程式視窗的桌上型電腦上的像素。 另一個 CompositionBrush like CompositionEffectBrush 輸入 | Windows 10 紀念日 Update (SDK 14393)

### <a name="paint-with-a-solid-color"></a>繪製具有純色

[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)繪製區域滿單色。 有各種方式來指定 SolidColorBrush 的色彩。 例如，您可以指定其 alpha、 紅色、 藍色和綠色 (ARGB) 通道或使用其中一個預先定義的[色彩](https://docs.microsoft.com/uwp/api/windows.ui.colors)類別所提供的色彩。

以下圖例和程式碼示範一個小型的視覺化樹狀結構來建立矩形，此矩形是以黑色筆刷勾勒，並以色彩值為 0x9ACD32 的單色筆刷繪製。

![CompositionColorBrush](images/composition-compositioncolorbrush.png)

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual _colorVisual1, _colorVisual2;
CompositionColorBrush _blackBrush, _greenBrush;

_compositor = Window.Current.Compositor;
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
_colorVisual1= _compositor.CreateSpriteVisual();
_colorVisual1.Brush = _blackBrush;
_colorVisual1.Size = new Vector2(156, 156);
_colorVisual1.Offset = new Vector3(0, 0, 0);
_container.Children.InsertAtBottom(_colorVisual1);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
_colorVisual2 = _compositor.CreateSpriteVisual();
_colorVisual2.Brush = _greenBrush;
_colorVisual2.Size = new Vector2(150, 150);
_colorVisual2.Offset = new Vector3(3, 3, 0);
_container.Children.InsertAtBottom(_colorVisual2);
```

### <a name="paint-with-a-linear-gradient"></a>繪製具有線性漸層

[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)繪製區域與線性漸層。 線性漸層來不同列中的漸層停駐座標軸調合兩個以上的色彩。 您可以使用 GradientStop 物件之漸層和比例中指定的色彩。

下列圖例和程式碼會顯示具有使用紅色和黃色色彩 2 個停駐點繪製具有 LinearGradientBrush SpriteVisual。

![CompositionLinearGradientBrush](images/composition-compositionlineargradientbrush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionLinearGradientBrush _redyellowBrush;

_compositor = Window.Current.Compositor;

_redyellowBrush = _compositor.CreateLinearGradientBrush();
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Red));
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.Yellow));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = _redyellowBrush;
_gradientVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-an-image"></a>繪製具有映像

[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)以像素呈現到 ICompositionSurface 繪製區域。 例如，CompositionSurfaceBrush 可用來繪製區域與呈現到使用[LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API ICompositionSurface 表面的圖像。

下列圖例和程式碼會顯示 SpriteVisual 繪製與呈現到使用 LoadedImageSurface ICompositionSurface licorice 的點陣圖。 伸展大小及對齊點陣圖 visual 的邊界內可用的 CompositionSurfaceBrush 屬性。

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png)

```cs
Compositor _compositor;
SpriteVisual _imageVisual;
CompositionSurfaceBrush _imageBrush;

_compositor = Window.Current.Compositor;

_imageBrush = _compositor.CreateSurfaceBrush();

// The loadedSurface has a size of 0x0 till the image has been been downloaded, decoded and loaded to the surface. We can assign the surface to the CompositionSurfaceBrush and it will show up once the image is loaded to the surface.
LoadedImageSurface _loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_imageBrush.Surface = _loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _imageBrush;
_imageVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-custom-drawing"></a>繪製具有自訂繪圖
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)也可以用來繪製以像素呈現使用[Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) （或 D2D） ICompositionSurface 的區域。

下列程式碼會顯示 SpriteVisual 繪製與執行到 ICompositionSurface 轉譯文字使用 Win2D。 請注意，才可使用的 Win2D 您必須包含[Win2D NuGet](http://www.nuget.org/packages/Win2D.uwp)封裝 （英文） 插入專案。

```cs
Compositor _compositor;
CanvasDevice _device;
CompositionGraphicsDevice _compositionGraphicsDevice;
SpriteVisual _drawingVisual;
CompositionSurfaceBrush _drawingBrush;

_device = CanvasDevice.GetSharedDevice();
_compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(_compositor, _device);

_drawingBrush = _compositor.CreateSurfaceBrush();
CompositionDrawingSurface _drawingSurface = _compositionGraphicsDevice.CreateDrawingSurface(new Size(256, 256), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied);

using (var ds = CanvasComposition.CreateDrawingSession(_drawingSurface))
{
     ds.Clear(Colors.Transparent);
     var rect = new Rect(new Point(2, 2), (_drawingSurface.Size.ToVector2() - new Vector2(4, 4)).ToSize());
     ds.FillRoundedRectangle(rect, 15, 15, Colors.LightBlue);
     ds.DrawRoundedRectangle(rect, 15, 15, Colors.Gray, 2);
     ds.DrawText("This is a composition drawing surface", rect, Colors.Black, new CanvasTextFormat()
     {
          FontFamily = "Comic Sans MS",
          FontSize = 32,
          WordWrapping = CanvasWordWrapping.WholeWord,
          VerticalAlignment = CanvasVerticalAlignment.Center,
          HorizontalAlignment = CanvasHorizontalAlignment.Center
     }
);

_drawingBrush.Surface = _drawingSurface;

_drawingVisual = _compositor.CreateSpriteVisual();
_drawingVisual.Brush = _drawingBrush;
_drawingVisual.Size = new Vector2(156, 156);
```

同樣地，CompositionSurfaceBrush 也可以用來繪製搭配使用 Win2D interop SwapChain SpriteVisual。 [此範例](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample)提供如何使用 Win2D 繪製具有 swapchain SpriteVisual 的範例。

### <a name="paint-with-a-video"></a>繪製的影片
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)也可以用來繪製以像素呈現使用載入透過[MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer)類別的影片 ICompositionSurface 的區域。

下列程式碼會顯示 SpriteVisual 繪製載入到 ICompositionSurface 的影片。

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("http://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
var item = new MediaPlaybackItem(source);
_mediaPlayer.Source = item;
_mediaPlayer.IsLoopingEnabled = true;

// Get the surface from MediaPlayer and put it on a brush
_videoSurface = _mediaPlayer.GetSurface(_compositor);
_videoBrush = _compositor.CreateSurfaceBrush(_videoSurface.CompositionSurface);

_videoVisual = _compositor.CreateSpriteVisual();
_videoVisual.Brush = _videoBrush;
_videoVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-filter-effect"></a>繪製具有篩選效果

[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)使用繪製區域的 CompositionEffect 輸出。 視覺化的圖層中的效果可能會視為可用來建立動畫篩選效果套用至如色彩、 漸層、 影像、 影片、 swapchains、 您 UI 的地區或樹狀結構視覺效果的來源內容的集合。 來源內容通常是使用其他 CompositionBrush 來指定。

下列圖例和程式碼會顯示已套用 desaturation 篩選效果 cat 影像資料繪製 SpriteVisual。

![CompositionEffectBrush](images/composition-cat-desaturated.png)

```cs
Compositor _compositor;
SpriteVisual _effectVisual;
CompositionEffectBrush _effectBrush;

_compositor = Window.Current.Compositor;

var graphicsEffect = new SaturationEffect {
                              Saturation = 0.0f,
                              Source = new CompositionEffectSourceParameter("mySource")
                         };

var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
_effectBrush = effectFactory.CreateBrush();

CompositionSurfaceBrush surfaceBrush =_compositor.CreateSurfaceBrush();
LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/cat.jpg"));
SurfaceBrush.surface = loadedSurface;

_effectBrush.SetSourceParameter("mySource", surfaceBrush);

_effectVisual = _compositor.CreateSpriteVisual();
_effectVisual.Brush = _effectBrush;
_effectVisual.Size = new Vector2(156, 156);
```

如需有關建立使用 CompositionBrushes 效果看到 [ [Visual 層中的效果](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>繪製具有 CompositionBrush 套用的不透明度遮罩

[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)繪製 CompositionBrush 與區域與套用至其不透明度遮罩。 不透明度遮罩來源可以是任何 CompositionBrush 類型 CompositionColorBrush、 CompositionLinearGradientBrush、 CompositionSurfaceBrush、 CompositionEffectBrush 或 CompositionNineGridBrush。 必須以 CompositionSurfaceBrush 指定的不透明度遮罩。

下列圖例和程式碼會顯示與 CompositionMaskBrush 繪製 SpriteVisual。 遮罩來源是圓形的 CompositionLinearGradientBrush 這看起來像是遮罩以使用映像的圓上遮罩。

![CompositionMaskBrush](images/composition-compositionmaskbrush.png)

```cs
Compositor _compositor;
SpriteVisual _maskVisual;
CompositionMaskBrush _maskBrush;

_compositor = Window.Current.Compositor;

_maskBrush = _compositor.CreateMaskBrush();

CompositionLinearGradientBrush _sourceGradient = _compositor.CreateLinearGradientBrush();
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(0,Colors.Red));
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(1,Colors.Yellow));
_maskBrush.Source = _sourceGradient;

LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/circle.png"), new Size(156.0, 156.0));
_maskBrush.Mask = _compositor.CreateSurfaceBrush(loadedSurface);

_maskVisual = _compositor.CreateSpriteVisual();
_maskVisual.Brush = _maskBrush;
_maskVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>繪製具有使用 NineGrid 拉大 CompositionBrush

[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)繪製會伸展使用九格線象徵 CompositionBrush 與區域。 九格線象徵可讓您拉大邊緣和角落的 CompositionBrush 不同比其中心點。 來源的九個格線拉長可由任何類型 CompositionColorBrush 的 CompositionBrush、 CompositionSurfaceBrush 或 CompositionEffectBrush。

下列程式碼會顯示 SpriteVisual 繪製具有 CompositionNineGridBrush。 這會伸展使用九格線 CompositionSurfaceBrush 是遮罩的來源。

```cs
Compositor _compositor;
SpriteVisual _nineGridVisual;
CompositionNineGridBrush _nineGridBrush;

_compositor = Window.Current.Compositor;

_ninegridBrush = _compositor.CreateNineGridBrush();

// nineGridImage.png is 50x50 pixels; nine-grid insets, as measured relative to the actual size of the image, are: left = 1, top = 5, right = 10, bottom = 20 (in pixels)

LoadedImageSurface _imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/nineGridImage.png"));
_nineGridBrush.Source = _compositor.CreateSurfaceBrush(_imageSurface);

// set Nine-Grid Insets

_ninegridBrush.SetInsets(1, 5, 10, 20);

// set appropriate Stretch on SurfaceBrush for Center of Nine-Grid

sourceBrush.Stretch = CompositionStretch.Fill;

_nineGridVisual = _compositor.CreateSpriteVisual();
_nineGridVisual.Brush = _ninegridBrush;
_nineGridVisual.Size = new Vector2(100, 75);
```

### <a name="paint-using-background-pixels"></a>使用背景像素為單位的 [小畫家]

[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)複製與該內容背後的區域] 區域。 CompositionBackdropBrush 永不用在其本身，但改為做為另一個 CompositionBrush like EffectBrush 輸入。 例如，藉由使用 CompositionBackdropBrush 輸入柔邊效果，可以達到毛玻璃效果。

下列程式碼會顯示小型視覺樹狀目錄以建立使用 CompositionSurfaceBrush 和圖像上方毛玻璃覆疊映像。 藉由將填滿上方映像 EffectBrush SpriteVisual 建立毛玻璃重疊。 EffectBrush 使用 CompositionBackdropBrush 輸入柔邊效果。

```cs
Compositor _compositor;
SpriteVisual _containerVisual;
SpriteVisual _imageVisual;
SpriteVisual _backdropVisual;

_compositor = Window.Current.Compositor;

// Create container visual to host the visual tree
_containerVisual = _compositor.CreateContainerVisual();

// Create _imageVisual and add it to the bottom of the container visual.
// Paint the visual with an image.

CompositionSurfaceBrush _licoriceBrush = _compositor.CreateSurfaceBrush();

LoadedImageSurface loadedSurface = 
    LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_licoriceBrush.Surface = loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _licoriceBrush;
_imageVisual.Size = new Vector2(156, 156);
_imageVisual.Offset = new Vector3(0, 0, 0);
_containerVisual.Children.InsertAtBottom(_imageVisual)

// Create a SpriteVisual and add it to the top of the containerVisual.
// Paint the visual with an EffectBrush that applies blur to the content
// underneath the Visual to create a frosted glass effect.

GaussianBlurEffect blurEffect = new GaussianBlurEffect(){
                                    Name = "Blur",
                                    BlurAmount = 1.0f,
                                    BorderMode = EffectBorderMode.Hard,
                                    Source = new CompositionEffectSourceParameter("source");
                                    };

CompositionEffectFactory blurEffectFactory = _compositor.CreateEffectFactory(blurEffect);
CompositionEffectBrush _backdropBrush = blurEffectFactory.CreateBrush();

// Create a BackdropBrush and bind it to the EffectSourceParameter source

_backdropBrush.SetSourceParameter("source", _compositor.CreateBackdropBrush());
_backdropVisual = _compositor.CreateSpriteVisual();
_backdropVisual.Brush = _licoriceBrush;
_backdropVisual.Size = new Vector2(78, 78);
_backdropVisual.Offset = new Vector3(39, 39, 0);
_containerVisual.Children.InsertAtTop(_backdropVisual);
```

## <a name="combining-compositionbrushes"></a>結合 CompositionBrushes
CompositionBrushes 一些使用其他 CompositionBrushes 做為輸入。 例如，使用 SetSourceParameter 方法可以用來設定其他 CompositionBrush CompositionEffectBrush 輸入。 下表概述 CompositionBrushes 的支援的組合。 請注意，使用不受支援的組合會擲回例外狀況。

<table>
<tbody>
<tr>
<th>Brush</th>
<th>EffectBrush.SetSourceParameter()</th>
<th>MaskBrush.Mask</th>
<th>MaskBrush.Source</th>
<th>NineGridBrush.Source</th>
</tr>
<tr>
<td>CompositionColorBrush</td>
<td>是</td>
<td>是</td>
<td>是</td>
<td>是</td>
</tr>
<tr>
<td>CompositionLinear<br />GradientBrush</td>
<td>是</td>
<td>是</td>
<td>是</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionSurfaceBrush</td>
<td>是</td>
<td>是</td>
<td>是</td>
<td>是</td>
</tr>
<tr>
<td>CompositionEffectBrush</td>
<td>否</td>
<td>否</td>
<td>是</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionMaskBrush</td>
<td>否</td>
<td>否</td>
<td>否</td>
<td>否</td>
</tr>
<tr>
<td>CompositionNineGridBrush</td>
<td>是</td>
<td>是</td>
<td>是</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionBackdropBrush</td>
<td>是</td>
<td>否</td>
<td>否</td>
<td>否</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>使用 XAML 筆刷和比較 CompositionBrush

下表提供案例和是否 XAML 或組合筆刷使用規定繪製 UIElement 或應用程式中的 SpriteVisual 時的清單。 

> [!NOTE]
> 如果 CompositionBrush 所建議的 XAML UIElement，則會假設使用 XamlCompositionBrushBase 封裝 CompositionBrush。

|案例                                                                   | XAML UIElement                                                                                                |組合 SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|使用純色繪製區域                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|使用動畫的色彩繪製區域                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|使用靜態漸層繪製區域                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|繪製具有動畫的漸層停駐點的區域                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|繪製與圖像區域                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|使用網頁繪製區域                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |無
|使用 NineGrid 拉大圖像來繪製區域                         |[圖像控制項](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用動畫 NineGrid 拉大繪製區域                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用 swapchain 繪製區域                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |具有 swapchain interop [CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|繪製區域的影片                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |具有媒體 interop [CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|使用自訂的 2D 繪圖繪製區域                                       |從 Win2D [CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm)                                                                                                 |具有 Win2D interop [CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|使用非動畫遮罩繪製區域                                       |若要定義遮罩使用 XAML[的圖形](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|使用動畫遮罩繪製區域                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|繪製具有動畫的篩選效果區域                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|繪製具有效果套用至背景像素為單位的區域        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>相關主題

[組合原生 DirectX 和 Direct2D interop BeginDraw 與 EndDraw](composition-native-interop.md)

[XAML 筆刷 interop 與 XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
