---
author: jwmsft
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: 組合筆刷
description: 筆刷會使用其輸出來繪製 Visual 的區域。 不同的筆刷有不同類型的輸出。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 730d5ae9062fe39533cd615facaf5beaa7d02ffd
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7439781"
---
# <a name="composition-brushes"></a>組合筆刷
從 UWP 應用程式顯示在螢幕上的所有項目都是可見的因為它已由筆刷繪製。 筆刷可讓您與內容涵蓋範圍可從簡單、 純色色彩影像或繪圖到複雜的效果鏈繪製使用者介面 (UI) 物件。 本主題介紹使用 CompositionBrush 繪製的概念。

請注意，當使用 XAML UWP app，您可以選擇來繪製[XAML 筆刷](/windows/uwp/design/style/brushes)或[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush)UIElement。 一般而言，它是更容易且建議您選擇的 XAML 筆刷，如果您的案例支援的 XAML 筆刷。 例如，以動畫顯示按鈕，變更文字或影像的圖形的填滿的色彩。 相反地，如果您嘗試執行一些動作不支援的 XAML 筆刷像繪圖以產生動畫效果的遮罩或產生動畫效果的九宮格 stretch 或效果鏈，您可以使用 CompositionBrush 來繪製[編組 UIElementXamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)。

使用視覺層時，必須使用 CompositionBrush 來繪製[SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)的區域。

-   [必要條件](./composition-brushes.md#prerequisites)
-   [使用 CompositionBrush 小畫家](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [使用純色繪製](./composition-brushes.md#paint-with-a-solid-color)
    -   [繪製時的線性漸層](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [使用影像繪製](./composition-brushes.md#paint-with-an-image)
    -   [使用自訂繪圖繪製](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [繪製含有視訊](./composition-brushes.md#paint-with-a-video)
    -   [繪製具有篩選的效果](./composition-brushes.md#paint-with-a-filter-effect)
    -   [繪製 CompositionBrush 與不透明度遮罩](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [使用 NineGrid stretch CompositionBrush 繪製](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [使用背景像素的小畫家](./composition-brushes.md#paint-using-background-pixels)
-   [結合 CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [使用 XAML 筆刷與 CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [相關主題](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>必要條件
這個概觀假設您已熟悉基本的組合應用程式中，結構[視覺層概觀](visual-layer.md)中所述。

## <a name="paint-with-a-compositionbrush"></a>使用 CompositionBrush 小畫家

[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) 」 來繪製 」 使用其輸出區域。 不同的筆刷有不同類型的輸出。 某些筆刷繪製區域的純色，其他人使用漸層、 影像、 自訂繪製或效果。 也有特殊修改行為的其他筆刷的筆刷。 例如，不透明度遮罩可用來控制哪些區域繪製 CompositionBrush，或九宮格可以用來控制繪製區域時，套用至 CompositionBrush stretch。 CompositionBrush 可以是下列類型的其中一個：

|類別                                   |詳細資料                                         |中導入|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |使用純色繪製區域                        |Windows 10 11 月更新 (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |繪製[ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)的內容區域|Windows 10 11 月更新 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |使用組合效果的內容繪製區域 |Windows 10 11 月更新 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |來繪製視覺與 CompositionBrush 與不透明度遮罩 |Windows 10 年度更新版 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |搭配使用 NineGrid stretch CompositionBrush 繪製區域 |Windows 10 年度更新版 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|繪製區域時的線性漸層                    |Windows 10 Fall Creators Update (Insider Preview SDK)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |藉由取樣之應用程式的背景像素或直接後方在桌面上的應用程式視窗的像素繪製區域。 做為輸入另一個 CompositionBrush 像 CompositionEffectBrush | Windows 10 年度更新版 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>使用純色繪製

[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)來繪製區域的純色。 有各種不同的方式來指定色彩的 SolidColorBrush。 例如，您可以指定其 alpha、 紅色、 藍色，及綠色 (ARGB) 的通道，或使用其中一種[色彩](https://docs.microsoft.com/uwp/api/windows.ui.colors)類別所提供的預先定義色彩。

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

### <a name="paint-with-a-linear-gradient"></a>繪製時的線性漸層

[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)繪製區域時的線性漸層。 線性漸層線條，漸層軸跨混合兩個或多個色彩。 您可以使用 GradientStop 物件，指定的色彩漸層和其位置。

以下圖例和程式碼示範使用 2 個停駐點使用紅色和黃色色彩與 LinearGradientBrush 繪製 SpriteVisual。

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

### <a name="paint-with-an-image"></a>使用影像繪製

[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)與轉譯到 ICompositionSurface 像素繪製區域。 例如，CompositionSurfaceBrush 可用來繪製區域的影像呈現到使用[LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API ICompositionSurface 表面上。

以下圖例和程式碼會顯示 SpriteVisual 繪製的轉譯到使用 LoadedImageSurface ICompositionSurface 甘草點陣圖。 CompositionSurfaceBrush 屬性可用來延展與對齊視覺邊界內的點陣圖。

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

### <a name="paint-with-a-custom-drawing"></a>使用自訂繪圖繪製
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)也可用來繪製區域的像素的轉譯使用[Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) （或 D2D） ICompositionSurface。

下列程式碼顯示使用文字執行轉譯到 ICompositionSurface 上繪製 SpriteVisual 使用 Win2D。 請注意，若要使用的 Win2D 您需要包含[Win2D NuGet](http://www.nuget.org/packages/Win2D.uwp)套件到您的專案。

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

同樣地，CompositionSurfaceBrush 也可用來繪製搭配使用 Win2D 互通性還 SpriteVisual。 [這個範例](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample)會提供如何使用 Win2D 來繪製 SpriteVisual 還使用的範例。

### <a name="paint-with-a-video"></a>繪製含有視訊
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)也可用來繪製區域的使用[MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer)類別透過載入視訊呈現 ICompositionSurface 從像素。

下列程式碼顯示 SpriteVisual 繪製與載入到 ICompositionSurface 影片。

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

### <a name="paint-with-a-filter-effect"></a>繪製具有篩選的效果

[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)繪製 CompositionEffect 輸出的區域。 視覺層中的效果可能會視為可產生動畫效果的篩選器效果套用至來源內容，例如色彩，漸層、 影像、 影片、 交換鏈結，您的 UI 的地區或視覺效果的樹狀結構的集合。 來源內容通常是使用另一個 CompositionBrush 來指定。

以下圖例和程式碼會顯示 SpriteVisual 繪製的一已套用的篩選器效果去飽和度的貓映像。

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

如需建立使用 CompositionBrushes 效果的詳細資訊，請參閱[視覺層中的效果](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>CompositionBrush 繪製使用套用不透明度遮罩

[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)使用繪製區域 CompositionBrush 與不透明度遮罩套用到它。 不透明度遮罩的來源可以是任何型別 CompositionColorBrush，CompositionLinearGradientBrush、 CompositionSurfaceBrush、 CompositionEffectBrush 或 CompositionNineGridBrush CompositionBrush。 必須為 CompositionSurfaceBrush 指定不透明度遮罩。

以下圖例和程式碼會顯示糖 CompositionMaskBrush SpriteVisual。 遮罩的來源是 CompositionLinearGradientBrush 這已被遮蔽看起來就像作為遮罩使用圓形的影像的圓形。

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>使用 NineGrid stretch CompositionBrush 繪製

[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)繪製區域時所使用的九宮格隱喻會向兩邊延伸 CompositionBrush 的。 九宮格隱喻，可讓您比其中心以不同方式伸展邊緣和 CompositionBrush 的圓角。 藉由任何型別 CompositionColorBrush CompositionBrush、 CompositionSurfaceBrush 或 CompositionEffectBrush 可以九宮格 stretch 的來源。

下列程式碼示範使用 CompositionNineGridBrush 繪製 SpriteVisual。 遮罩的來源是可使用九宮格會向兩邊延伸 CompositionSurfaceBrush。

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

### <a name="paint-using-background-pixels"></a>使用背景像素的小畫家

[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)繪製區域的區域後方的內容。 CompositionBackdropBrush 永遠不會使用它，但改為使用做為輸入到另一個 CompositionBrush 像 EffectBrush。 例如，使用 CompositionBackdropBrush 做為模糊效果的輸入，您可以達到毛玻璃效果。

下列程式碼顯示小型的視覺化樹狀結構來建立使用 CompositionSurfaceBrush 和毛玻璃重疊影像上方的影像。 毛玻璃覆疊是由放置填滿影像上方 EffectBrush SpriteVisual 建立。 EffectBrush 使用 CompositionBackdropBrush 作為模糊效果的輸入。

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
數個 CompositionBrushes 使用其他 CompositionBrushes 做為輸入。 例如，使用 SetSourceParameter 方法可以用來設定另一個 CompositionBrush 作為 CompositionEffectBrush 的輸入。 下表概述 CompositionBrushes 支援的組合。 請注意，使用不支援的組合將會擲回例外狀況。

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


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>使用 XAML 筆刷與 CompositionBrush

下表提供一份案例和繪製 UIElement 或 SpriteVisual 在您的應用程式時，是否規定 XAML 或組合筆刷使用。 

> [!NOTE]
> 如果 CompositionBrush 建議為 XAML uielement，它假設 CompositionBrush 封裝使用 XamlCompositionBrushBase。

|案例                                                                   | XAML UIElement                                                                                                |組合 SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|使用純色繪製區域                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|繪製區域的動畫效果的色彩                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|繪製區域的靜態漸層                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|繪製區域的動畫的漸層停駐點                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|繪製區域的影像                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|繪製區域的網頁                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |N/A
|影像使用 NineGrid stretch 繪製區域                         |[影像控制項](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|繪製區域的動畫 NineGrid stretch                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|繪製區域的還                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)以還互通性
|繪製區域的影片                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)與媒體互通性
|使用自訂的 2D 繪圖繪製區域                                       |從 Win2D [CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)以 Win2D 互通性
|繪製區域的非動畫遮罩                                       |使用 XAML[圖形](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)來定義的遮罩   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|繪製區域的動畫的遮罩                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|繪製區域的篩選器動畫的效果                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|使用效果套用至背景像素繪製區域        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>相關主題

[組合原生 DirectX 與 Direct2D 互通性與 BeginDraw 和 EndDraw](composition-native-interop.md)

[XamlCompositionBrushBase 與 XAML 筆刷互通性](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
