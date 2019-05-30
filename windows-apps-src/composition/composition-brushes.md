---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: 組合筆刷
description: 筆刷會使用其輸出來繪製 Visual 的區域。 不同的筆刷有不同類型的輸出。
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 41d3a84de1aa9e7440d5396775bd66d9c9e09d41
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361515"
---
# <a name="composition-brushes"></a>組合筆刷
您畫面上來自 UWP 應用程式的一切項目都會顯示出來，因為是用筆刷繪製的。 筆刷可讓您使用從簡單純色、影像或繪圖到複雜效果鏈的內容，來繪製使用者介面 (UI) 物件。 本主題介紹使用 CompositionBrush 繪製的概念。

請注意，在使用 XAML UWP app 時，您可以選擇使用 [XAML 筆刷](/windows/uwp/design/style/brushes)或 [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) 繪製 UIElement。 如果 XAML 筆刷支援您的案例，通常會建議您選擇 XAML 筆刷，因為這也是比較簡單的方法。 例如，動畫顯示按鈕色彩、使用影像變更文字或形狀的填滿。 相反地，如果您嘗試執行不支援 XAML 筆刷繪製動畫的遮罩或動畫的九宮格縮放效果鏈結與類似的項目，可以使用 CompositionBrush 繪製使用 UIElement [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)。

使用視覺層時，必須使用 CompositionBrush 繪製 [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) 區域。

-   [必要條件](./composition-brushes.md#prerequisites)
-   [使用 CompositionBrush 繪製](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [使用純色繪製](./composition-brushes.md#paint-with-a-solid-color)
    -   [使用線形漸層繪製](./composition-brushes.md#paint-with-a-linear-gradient) 
    -   [使用放射狀漸層繪製](./composition-brushes.md#paint-with-a-radial-gradient)
    -   [使用影像繪製](./composition-brushes.md#paint-with-an-image)
    -   [使用自訂繪圖繪製](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [繪製含有視訊](./composition-brushes.md#paint-with-a-video)
    -   [使用篩選器效果繪製](./composition-brushes.md#paint-with-a-filter-effect)
    -   [使用具有不透明度遮罩 CompositionBrush 繪製](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [繪製使用 NineGrid stretch CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [使用背景像素繪製](./composition-brushes.md#paint-using-background-pixels)
-   [結合 CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [使用 XAML 的筆刷 vs。CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [相關的主題](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>先決條件
這個概觀假設您已熟悉基本「組合」應用程式的結構，如[視覺層概觀](visual-layer.md)所述。

## <a name="paint-with-a-compositionbrush"></a>使用 CompositionBrush 繪製

[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) 使用其輸出「繪製區域。 不同的筆刷有不同類型的輸出。 有些筆刷使用純色來繪製區域，有些筆刷使用漸層、影像、自訂繪圖或效果來繪製區域。 還有一些特殊筆刷，可修改其他筆刷的行為。 例如，不透明度遮罩可用來控制 CompositionBrush 繪製哪個區域，而九宮格可用來控制繪製區域時套用至 CompositionBrush 的延展。 CompositionBrush 可以是下列其中一個型別：

|類別                                   |詳細資料                                         |引進版本|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |使用純色繪製區域                        |Windows 10 版本 1511 (10586 SDK)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |使用 [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface) 的內容繪製區域|Windows 10 版本 1511 (10586 SDK)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |使用組合效果的內容繪製區域 |Windows 10 版本 1511 (10586 SDK)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |使用 CompositionBrush 搭配不透明度遮罩繪製視覺效果 |Windows 10 版本 1607 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |使用 CompositionBrush 搭配 NineGrid 延展繪製區域 |Windows 10 版本 1607 (SDK 14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|使用線形漸層繪製區域                    |Windows 10 版本 1709 (SDK 16299)
|[CompositionRadialGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionradialgradientbrush)|使用放射狀漸層繪製區域                    |Windows 10 版本 1903 (Insider Preview SDK)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |從應用程式或桌面上應用程式視窗背後的像素來取樣背景像素，用以繪製區域。 做為另一個 CompositionBrush (例如 CompositionEffectBrush) 的輸入 | Windows 10 版本 1607 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>使用純色繪製

[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) 使用純色繪製區域。 指定 SolidColorBrush 的色彩有很多種方式。 例如，您可以指定其 Alpha、紅色、藍色及綠色 (ARGB) 頻道或使用 [Colors](https://docs.microsoft.com/uwp/api/windows.ui.colors) 類別所提供的其中一個預先定義色彩。

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

### <a name="paint-with-a-linear-gradient"></a>使用線性漸層繪製

[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) 使用線性漸層來繪製區域。 線性漸層會沿著一條線 (漸層軸) 混合兩個或更多色彩。 您可以使用 GradientStop 物件來指定漸層中的色彩及其位置。

下圖和程式碼顯示使用 LinearGradientBrush 搭配 2 個停駐點 (使用紅色和黃色) 來繪製的 SpriteVisual。

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

### <a name="paint-with-a-radial-gradient"></a>使用放射狀漸層繪製

A [CompositionRadialGradientBrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush)使用放射狀漸層繪製區域。 放射狀漸層混合兩個以上的色彩與漸層從橢圓形的中心開始和結束時間橢圓形的半徑。 GradientStop 物件用來定義 漸層中的 色彩和其位置。

下圖和程式碼顯示使用 2 個 GradientStops 使用 RadialGradientBrush 繪製 SpriteVisual。

![CompositionRadialGradientBrush](images/radial-gradient-brush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionRadialGradientBrush RGBrush;

_compositor = Window.Current.Compositor;

RGBrush = _compositor.CreateRadialGradientBrush();
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Aquamarine));
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.DeepPink));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = RGBrush;
_gradientVisual.Size = new Vector2(200, 200);
```

### <a name="paint-with-an-image"></a>使用影像繪製

[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 使用轉譯至 ICompositionSurface 的像素來繪製區域。 例如，可以使用 CompositionSurfaceBrush 搭配以 [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API 轉譯到 ICompositionSurface 表面的影像來繪製區域。

下圖和程式碼顯示使用以 LoadedImageSurface 將甘草糖點陣圖轉譯到 ICompositionSurface 來繪製的 SpriteVisual。 CompositionSurfaceBrush 的屬性可用來在視覺效果的界限內延展和對齊點陣圖。

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
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 也可用來使用以 [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm) (或 D2D) 轉譯之 ICompositionSurface 中的像素繪製區域。

下列程式碼顯示使用 Win2D 將文字執行轉譯到 ICompositionSurface 來繪製的 SpriteVisual。 請注意，若要使用 Win2D，您需要將 [Win2D NuGet](https://www.nuget.org/packages/Win2D.uwp) 套件加入您的專案。

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

同樣地，CompositionSurfaceBrush 也可以使用 SwapChain 搭配 Win2D 互通性來繪製 SpriteVisual。 [這個範例](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample)示範如何使用 Win2D 搭配 swapchain 來繪製 SpriteVisual。

### <a name="paint-with-a-video"></a>使用影片繪製
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 也可用來使用透過 [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) 類別所載入影片轉譯之 ICompositionSurface 中的像素繪製區域。

下列程式碼顯示使用載入至 ICompositionSurface 的影片來繪製的 SpriteVisual。

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("https://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
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

### <a name="paint-with-a-filter-effect"></a>使用濾鏡效果繪製

[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) 使用 CompositionEffect 的輸出來繪製區域。 視覺層中的效果可被視為套用至一系列來源內容 (例如色彩、漸層、影像、影片、swapchain、UI 的區域或視覺效果的樹狀結構) 的可產生動畫濾鏡效果。 來源內容通常使用其他 CompositionBrush 來指定。

下圖和程式碼顯示使用已套用去飽和度濾鏡效果的貓咪影像來繪製的 SpriteVisual。

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

如需使用 CompositionBrushes 建立效果的詳細資訊，請參閱[視覺層中的效果](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>使用已套用不透明度遮罩的 CompositionBrush 繪製

[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) 會使用已套用不透明度遮罩的 CompositionBrush 來繪製區域。 不透明度遮罩的來源可以是型別為 CompositionColorBrush、CompositionLinearGradientBrush、CompositionSurfaceBrush、CompositionEffectBrush 或 CompositionNineGridBrush 的任何 CompositionBrush。 不透明度遮罩必須指定為 CompositionSurfaceBrush。

下列圖和程式碼會顯示使用 CompositionMaskBrush 繪製的 SpriteVisual。 遮罩的來源是 CompositionLinearGradientBrush，套用遮罩後看起來像是使用圓形影像做為遮罩的圓形。

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>使用 CompositionBrush 搭配 NineGrid 延展繪製

[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) 使用以九宮格比喻延展的 CompositionBrush 來繪製區域。 九宮格比喻可讓您以不同的方式延展 CompositionBrush 的邊緣與角落及其中心。 九宮格延展的來源可以是型別為 CompositionColorBrush、CompositionSurfaceBrush 或 CompositionEffectBrush 的任何 CompositionBrush。

下列圖和程式碼會顯示使用 CompositionNineGridBrush 繪製的 SpriteVisual。 遮罩的來源是使用九宮格延展的 CompositionSurfaceBrush。

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

### <a name="paint-using-background-pixels"></a>使用背景像素繪製

[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) 使用區域背後的內容來繪製區域。 CompositionBackdropBrush 絕不會單獨使用，而是做為其他 CompositionBrush (例如 EffectBrush) 的輸入。 例如，使用 CompositionBackdropBrush 做為模糊效果的輸入，您可以獲得毛玻璃效果。

下列程式碼顯示小型視覺化樹狀結構，使用 CompositionSurfaceBrush 並在影像上覆疊毛玻璃來建立影像。 毛玻璃覆疊是透過在影像上放置以 EffectBrush 填滿的 SpriteVisual 來建立的。 EffectBrush 使用 CompositionBackdropBrush 做為模糊效果的輸入。

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
有幾個 CompositionBrushes 使用其他 CompositionBrushes 做為輸入。 例如，使用 SetSourceParameter 方法可以用來設定其他 CompositionBrush 做為 CompositionEffectBrush 的輸入。 下表概述支援的 CompositionBrushes 組合。 請注意，使用不支援的組合會擲回例外狀況。

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
<td>否</td>
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
<td>否</td>
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
<td>否</td>
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


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>使用 XAML 的筆刷 vs。CompositionBrush

下表列出案例清單以及在您的應用程式中繪製 UIElement 或 SpriteVisual 時是否規定使用 XAML 或組合筆刷。 

> [!NOTE]
> 如果建議對 XAML UIElement 使用 CompositionBrush，則假設 CompositionBrush 是使用 XamlCompositionBrushBase 封裝。

|狀況                                                                   | XAML UIElement                                                                                                |組合 SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|使用純色繪製區域                                             |[SolidColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|使用動畫色彩繪製區域                                          |[SolidColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|使用靜態漸層繪製區域                                       |[LinearGradientBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|使用動畫漸層停駐點繪製區域                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|使用影像繪製區域                                                |[ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)                                     |[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)
|使用網頁繪製區域                                               |[WebViewBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)                                   |N/A
|使用搭配 NineGrid 延展的影像繪製區域                         |[影像控制項](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用動畫 NineGrid 延展繪製區域                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用 swapchain 繪製區域                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 搭配 swapchain 互通性
|使用影片繪製區域                                                 |[MediaElement](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/media-playback)                                                                                                  |[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 搭配媒體互通性
|使用自訂 2D 繪圖繪製區域                                       |來自 Win2D 的 [CanvasControl](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm)                                                                                                 |[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 搭配 Win2D 互通性
|使用非動畫遮罩繪製區域                                       |使用 XAML [形狀](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)來定義遮罩   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|使用動畫遮罩來繪製區域                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|使用動畫濾鏡效果來繪製區域                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|使用套用至背景像素的效果繪製區域        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>相關主題

[撰寫原生 DirectX 和 Direct2D interop BeginDraw 與 EndDraw](composition-native-interop.md)

[與 XamlCompositionBrushBase XAML 筆刷交互操作](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
