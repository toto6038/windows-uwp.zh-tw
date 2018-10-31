---
author: jwmsft
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: 組合視覺效果
description: 「組合視覺效果」構成組合 API 的所有其他功能所使用和倚賴的視覺化樹狀結構。 API 可讓開發人員定義及建立一或多個視覺物件，每個物件各代表視覺化樹狀結構中的單一節點。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8e43d5697a8fe4536e3574d93bda5836133ec8a8
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5816497"
---
# <a name="composition-visual"></a>組合視覺效果

「組合視覺效果」構成組合 API 的所有其他功能所使用和倚賴的視覺化樹狀結構。 API 可讓開發人員定義及建立一或多個視覺物件，每個物件各代表視覺化樹狀結構中的單一節點。

## <a name="visuals"></a>視覺效果

有三個構成視覺化樹狀結構的視覺效果類型，外加一個含有多個影響視覺效果內容之子類別的基底筆刷類別：

- [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) – 基底物件，大多數的屬性都在這裡，並且會被其他視覺物件繼承。
- [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) – 衍生自 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)，並且會新增建立子系的能力。
- [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) – 衍生自 [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810)，並且會新增與筆刷建立關聯的能力，以便讓「視覺效果」能夠轉譯像素 (包括影像、效果或純色)。

您可以使用 [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398) 和其子類別 (包括[**CompositionColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)、[**CompositionSurfaceBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 和 [**CompositionEffectBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush))，將內容和效果套用至 SpriteVisuals。 若要深入了解筆刷，請參閱 [**CompositionBrush 概觀**](https://docs.microsoft.com/windows/uwp/composition/composition-brushes) (英文)。

## <a name="the-compositionvisual-sample"></a>CompositionVisual 範例

在此我們將以一個範例程式碼為例，說明先前所列三個不同的視覺効果類型。 雖然這個範例並未涵蓋像是「動畫」或更複雜效果的概念，但是它包含所有這些系統所使用的構成要素 (本文結尾處會列出完整的範例程式碼。)

在此範例中，有一些可以按一下並在螢幕上拖曳的單色方形。 在方形上按一下時，它將會顯示在最上層、 旋轉 45 度，並在拖曳時變成不透明。

這裡說明一些使用 API 的基本概念，包括：

- 建立撰寫器
- 使用 CompositionColorBrush 來建立 SpriteVisual
- 裁剪視覺效果
- 旋轉視覺效果
- 設定不透明度
- 變更視覺效果在集合中的位置

## <a name="creating-a-compositor"></a>建立撰寫器

建立 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 並將它儲存在變數中以做為處理站，是相當簡單的工作。 以下程式碼片段示範如何建立一個新的 **Compositor**：

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>建立 SpriteVisual 和 ColorBrush

使用 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 可讓您在需要物件時輕鬆建立物件， 例如 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) 和 [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399)：

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

雖然這只是幾行的程式碼，但卻展示了一個強大的概念，亦即：[**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) 物件是效果系統的核心。 **SpriteVisual** 可在色彩、影像及效果建立上，提供絕佳的彈性和相互作用。 **SpriteVisual** 是單一的視覺效果類型，可以使用筆刷 (在此案例中為單色) 填滿 2D 矩形。

## <a name="clipping-a-visual"></a>裁剪視覺效果

[**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 也可以用來建立對 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 的裁剪。 以下範例來自使用 [**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) 來修剪視覺效果之每一面的樣本：

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

與 API 中的其他物件相同，[**InsetClip**](https://msdn.microsoft.com/library/windows/apps/Dn706825) 的屬性也可以套用動畫效果。

## <a name="span-idrotatingaclipspanspan-idrotatingaclipspanspan-idrotatingaclipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>旋轉裁剪

您可以使用旋轉來轉換 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)。 請注意，[**RotationAngle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.rotationangle) 同時支援弧度與角度。 預設使用的是弧度，但是也可以輕鬆指定成角度，如下列程式碼片段所示：

```cs
child.RotationAngleInDegrees = 45.0f;
```

「旋轉」只是 API 所提供來簡化這些工作的一組轉換元件的其中一例。 其他還包括「位移」、「縮放」、「方向」、RotationAxis 及 4x4 TransformMatrix。

## <a name="setting-opacity"></a>設定不透明度

設定視覺效果的不透明度相當簡單，只要使用浮點值即可。 例如，在此範例中，所有方形開始的不透明度都是 .8：

```cs
visual.Opacity = 0.8f;
```

與旋轉相同，[**Opacity**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.opacity) 屬性也可以套用動畫效果。

## <a name="changing-the-visuals-position-in-the-collection"></a>變更視覺效果在集合中的位置

「組合 API」允許使用數種方法來變更 Visual 在 [**VisualCollection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection) 中的位置。 它可以透過 [**InsertAbove**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertabove) 放在另一個 Visual 上、透過 [**InsertBelow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertbelow) 放在另一個 Visual 下、透過 [**InsertAtTop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertattop) 移至頂端，或透過 [**InsertAtBottom**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visualcollection.insertatbottom) 移至底端。

在此範例中，已點選過的 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 被排序在最上方：

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>完整範例

在這個完整範例中，將上述所有概念一併搭配使用來建構和執行一個簡單的 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 物件樹狀結構，以在不使用 XAML、WWA 或 DirectX 的情況下變更不透明度。 這個範例示範如何建立和新增子系 **Visual** 物件，以及如何變更屬性。

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using Windows.ApplicationModel.Core;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Core;

namespace compositionvisual
{
    class VisualProperties : IFrameworkView
    {
        //------------------------------------------------------------------------------
        //
        // VisualProperties.Initialize
        //
        // This method is called during startup to associate the IFrameworkView with the
        // CoreApplicationView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Initialize(CoreApplicationView view)
        {
            _view = view;
            _random = new Random();
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.SetWindow
        //
        // This method is called when the CoreApplication has created a new CoreWindow,
        // allowing the application to configure the window and start producing content
        // to display.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.SetWindow(CoreWindow window)
        {
            _window = window;
            InitNewComposition();
            _window.PointerPressed += OnPointerPressed;
            _window.PointerMoved += OnPointerMoved;
            _window.PointerReleased += OnPointerReleased;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerPressed
        //
        // This method is called when the user touches the screen, taps it with a stylus
        // or clicks the mouse.
        //
        //------------------------------------------------------------------------------

        void OnPointerPressed(CoreWindow window, PointerEventArgs args)
        {
            Point position = args.CurrentPoint.Position;

            //
            // Walk our list of visuals to determine who, if anybody, was selected
            //
            foreach (var child in _root.Children)
            {
                //
                // Did we hit this child?
                //
                Vector3 offset = child.Offset;
                Vector2 size = child.Size;

                if ((position.X >= offset.X) &&
                    (position.X < offset.X + size.X) &&
                    (position.Y >= offset.Y) &&
                    (position.Y < offset.Y + size.Y))
                {
                    //
                    // This child was hit. Since the children are stored back to front,
                    // the last one hit is the front-most one so it wins
                    //
                    _currentVisual = child as ContainerVisual;
                    _offsetBias = new Vector2((float)(offset.X - position.X),
                                              (float)(offset.Y - position.Y));
                }
            }

            //
            // If a visual was hit, bring it to the front of the Z order
            //
            if (_currentVisual != null)
            {
                ContainerVisual parent = _currentVisual.Parent as ContainerVisual;
                parent.Children.Remove(_currentVisual);
                parent.Children.InsertAtTop(_currentVisual);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerMoved
        //
        // This method is called when the user moves their finger, stylus or mouse with
        // a button pressed over the screen.
        //
        //------------------------------------------------------------------------------

        void OnPointerMoved(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual is selected, drag it with the pointer position and
            // make it opaque while we drag it
            //
            if (_currentVisual != null)
            {
                //
                // Set up the properties of the visual the first time it is
                // dragged. This will last for the duration of the drag
                //
                if (!_dragging)
                {
                    _currentVisual.Opacity = 1.0f;

                    //
                    // Transform the first child of the current visual so that
                    // the image is rotated
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngleInDegrees = 45.0f;
                        child.CenterPoint = new Vector3(_currentVisual.Size.X / 2, _currentVisual.Size.Y / 2, 0);
                        break;
                    }

                    //
                    // Clip the visual to its original layout rect by using an inset
                    // clip with a one-pixel margin all around
                    //
                    var clip = _compositor.CreateInsetClip();
                    clip.LeftInset = 1.0f;
                    clip.RightInset = 1.0f;
                    clip.TopInset = 1.0f;
                    clip.BottomInset = 1.0f;
                    _currentVisual.Clip = clip;

                    _dragging = true;
                }

                Point position = args.CurrentPoint.Position;
                _currentVisual.Offset = new Vector3((float)(position.X + _offsetBias.X),
                                                    (float)(position.Y + _offsetBias.Y),
                                                    0.0f);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerReleased
        //
        // This method is called when the user lifts their finger or stylus from the
        // screen, or lifts the mouse button.
        //
        //------------------------------------------------------------------------------

        void OnPointerReleased(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual was selected, make it transparent again when it is
            // released and restore the transform and clip
            //
            if (_currentVisual != null)
            {
                if (_dragging)
                {
                    //
                    // Remove the transform from the first child
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngle = 0.0f;
                        child.CenterPoint = new Vector3(0.0f, 0.0f, 0.0f);
                        break;
                    }

                    _currentVisual.Opacity = 0.8f;
                    _currentVisual.Clip = null;
                    _dragging = false;
                }

                _currentVisual = null;
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Load
        //
        // This method is called when a specific page is being loaded in the
        // application.  It is not used for this application.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Load(string unused)
        {

        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Run
        //
        // This method is called by CoreApplication.Run() to actually run the
        // dispatcher's message pump.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Run()
        {
            _window.Activate();
            _window.Dispatcher.ProcessEvents(CoreProcessEventsOption.ProcessUntilQuit);
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Uninitialize
        //
        // This method is called during shutdown to disconnect the CoreApplicationView,
        // and CoreWindow from the IFrameworkView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Uninitialize()
        {
            _window = null;
            _view = null;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.InitNewComposition
        //
        // This method is called by SetWindow(), where we initialize Composition after
        // the CoreWindow has been created.
        //
        //------------------------------------------------------------------------------

        void InitNewComposition()
        {
            //
            // Set up Windows.UI.Composition Compositor, root ContainerVisual, and associate with
            // the CoreWindow.
            //

            _compositor = new Compositor();

            _root = _compositor.CreateContainerVisual();



            _compositionTarget = _compositor.CreateTargetForCurrentView();
            _compositionTarget.Root = _root;

            //
            // Create a few visuals for our window
            //
            for (int index = 0; index < 20; index++)
            {
                _root.Children.InsertAtTop(CreateChildElement());
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.CreateChildElement
        //
        // Creates a small sub-tree to represent a visible element in our application.
        //
        //------------------------------------------------------------------------------

        Visual CreateChildElement()
        {
            //
            // Each element consists of three visuals, which produce the appearance
            // of a framed rectangle
            //
            var element = _compositor.CreateContainerVisual();
            element.Size = new Vector2(100.0f, 100.0f);

            //
            // Position this visual randomly within our window
            //
            element.Offset = new Vector3((float)(_random.NextDouble() * 400), (float)(_random.NextDouble() * 400), 0.0f);

            //
            // The outer rectangle is always white
            //
            //Note to preview API users - SpriteVisual and Color Brush replace SolidColorVisual
            //for example instead of doing
            //var visual = _compositor.CreateSolidColorVisual() and
            //visual.Color = Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF);
            //we now use the below

            var visual = _compositor.CreateSpriteVisual();
            element.Children.InsertAtTop(visual);
            visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
            visual.Size = new Vector2(100.0f, 100.0f);

            //
            // The inner rectangle is inset from the outer by three pixels all around
            //
            var child = _compositor.CreateSpriteVisual();
            visual.Children.InsertAtTop(child);
            child.Offset = new Vector3(3.0f, 3.0f, 0.0f);
            child.Size = new Vector2(94.0f, 94.0f);

            //
            // Pick a random color for every rectangle
            //
            byte red = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte green = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte blue = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            child.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, red, green, blue));

            //
            // Make the subtree root visual partially transparent. This will cause each visual in the subtree
            // to render partially transparent, since a visual's opacity is multiplied with its parent's
            // opacity
            //
            element.Opacity = 0.8f;

            return element;
        }

        // CoreWindow / CoreApplicationView
        private CoreWindow _window;
        private CoreApplicationView _view;

        // Windows.UI.Composition
        private Compositor _compositor;
        private CompositionTarget _compositionTarget;
        private ContainerVisual _root;
        private ContainerVisual _currentVisual;
        private Vector2 _offsetBias;
        private bool _dragging;

        // Helpers
        private Random _random;
    }


    public sealed class VisualPropertiesFactory : IFrameworkViewSource
    {
        //------------------------------------------------------------------------------
        //
        // VisualPropertiesFactory.CreateView
        //
        // This method is called by CoreApplication to provide a new IFrameworkView for
        // a CoreWindow that is being created.
        //
        //------------------------------------------------------------------------------

        IFrameworkView IFrameworkViewSource.CreateView()
        {
            return new VisualProperties();
        }


        //------------------------------------------------------------------------------
        //
        // main
        //
        //------------------------------------------------------------------------------

        static int Main(string[] args)
        {
            CoreApplication.Run(new VisualPropertiesFactory());

            return 0;
        }
    }
}
```
