---
ms.assetid: b7a4ac8a-d91e-461b-a060-cc6fcea8e778
title: 使用視覺層搭配 XAML
description: 了解使用視覺層 API 搭配現有 XAML 內容來建立進階動畫及效果的技術。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ae9bc0f6d53181a88b02ecda19b3aed745febe40
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8889991"
---
# <a name="using-the-visual-layer-with-xaml"></a>使用視覺層搭配 XAML

大部分使用視覺層功能的應用程式，都會使用 XAML 來定義主要 UI 內容。 在 Windows 10 年度更新版中，XAML 架構和視覺層中的新功能可更輕鬆地結合這兩項技術，以建立令人驚艷的使用者體驗。
XAML 與視覺層互通性功能可用來建立單獨使用 XAML API 所無法提供的進階動畫與效果。 這包括：

- 筆刷效果，例如模糊和毛玻璃
- 動態光源效果
- 捲動驅動動畫及視差
- 自動配置動畫
- 完美像素陰影

這些效果和動畫可以套用至現有的 XAML 內容，因此您不需要大幅重組您的 XAML 應用程式即可利用新的功能。
配置動畫、陰影和模糊效果，涵蓋在以下的＜做法＞一節中 如需實作視差的程式碼範例，請參閱 [ParallaxingListItems 範例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)。  [WindowsUIDevLabs 存放機制](https://github.com/Microsoft/WindowsUIDevLabs) 也已經有實作動畫、陰影和效果的數個其他範例。

## <a name="the-xamlcompositionbrushbase-class"></a>XamlCompositionBrushBase 類別

**XamlCompositionBrush** 會針對使用**CompositionBrush**繪製區域的 XAML 筆刷，提供基底類別。 這可用來輕鬆地將模糊或毛玻璃這類組合效果套用至 XAML UI 元素。

請參閱[**筆刷**](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)小節，以取得搭配使用筆刷與 XAML UI 的詳細資訊。

如需程式碼範例，請參閱 [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) 的參考頁面。

## <a name="the-xamllight-class"></a>XamlLight 類別

**XamlLight** 會針對使用**CompositionLight**動態讓區域變亮的 XAML 光源效果，提供基底類別。

請參閱[**光源**](xaml-lighting.md)小節，以取得使用光源 (包括光源 XAML UI 元素) 的詳細資訊。

如需程式碼範例，請參閱 [**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) 的參考頁面。

## <a name="the-elementcompositionpreview-class"></a>ElementCompositionPreview 類別

[**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.aspx) 是靜態類別，提供 XAML 和視覺層互通性功能。 如需視覺層及其功能的概觀，請參閱[視覺層](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer)。 **ElementCompositionPreview** 類別會提供下列方法︰

-   [**GetElementVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx)：取得「交出」Visual，它用來轉譯此元素
-   [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual.aspx)︰設定「交入」Visual 做為此元素視覺化樹狀結構的最後一個子項。 這個 Visual 將會在其餘元素的頂端繪製。 
-   [**GetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx)：抓取使用 **SetElementChildVisual** 的 Visual 集合
-   [**GetScrollViewerManipulationPropertySet**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx)：取得可根據 **ScrollViewer** 中的捲動位移，用來建立 60fps 動畫的物件

## <a name="remarks-on-elementcompositionpreviewgetelementvisual"></a>ElementCompositionPreview.GetElementVisual 備註

**ElementCompositionPreview.GetElementVisual** 會傳回「交出」Visual，它用來轉譯指定的 **UIElement**。 屬性 (例如 **Visual.Opacity**、**Visual.Offset** 和 **Visual.Size**) 是根據 UIElement 狀態為基礎，透過 XAML 架構設定。 這可以使用例如隱含重新定位動畫的技術 (請參閱＜做法＞**)。

請注意，由於 **Offset** 和 **Size** 會設定做為 XAML 架構配置的結果，開發人員在對這些屬性進行修改或是產生動畫效果時應該小心謹慎。 當配置中元素的左上角與其父項的位置相同時，開發人員應僅對 Offset 進行修改或產生動畫效果。 Size 通常不應修改，但存取此屬性可能會很有用。 例如，以下的「陰影」和「毛玻璃」範例就使用交出 Visual 的 Size 作為動畫的輸入。

另請留意，更新的交出 Visual 屬性將不會反映在對應的 UIElement 中。 例如，將 **UIElement.Opacity** 設定為 0.5 會將所對應交出 Visual 的 Opacity 設定為 0.5。 不過，將交出 Visual 的 **Opacity** 設定為 0.5 會導致內容以 50% 的透明度顯示，但不會變更所對應 UIElement 的 Opacity 屬性。

### <a name="example-of-offset-animation"></a>**Offset** 動畫範例

#### <a name="incorrect"></a>錯誤

```xaml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### <a name="correct"></a>正確

```xaml
<Border>
    <Canvas Margin="5">
        <Image x:Name="MyImage" />
    </Canvas>
</Border>
```

```csharp
// This works because the Canvas parent doesn’t generate a layout offset.
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

## <a name="the-elementcompositionpreviewsetelementchildvisual-method"></a>**ElementCompositionPreview.SetElementChildVisual** 方法

**ElementCompositionPreview.SetElementChildVisual** 可讓開發人員提供將顯示為元素視覺化樹狀結構成員的「交入」Visual。 這可讓開發人員建立「組合島」，其中以 Visual 為主的內容可顯示於 XAML UI 內。 開發人員應該謹慎使用這項技術，因為以 Visual 為主的內容將不會有 XAML 所保證的相同協助工具及使用者體驗。 因此，通常建議這項技術僅在實作自訂效果時使用，例如下方＜做法＞一節中的內容。

## <a name="getalphamask-methods"></a>**GetAlphaMask** 方法

[**Image**](https://msdn.microsoft.com/library/windows/apps/br242752)、[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 和 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 都會實作稱為 **GetAlphaMask** 的方法，它會傳回代表灰階影像與元素形狀的 **CompositionBrush**。 這個 **CompositionBrush** 可做為組合 **DropShadow** 的輸入，如此陰影就可以反映元素的形狀而不是矩形。 這可讓您使用 Alpah 和形狀，針對文字和影像建立完美像素、以輪廓為主的陰影。 如需 API 的範例，請參閱下方的＜陰影＞**。

## <a name="recipes"></a>做法

### <a name="reposition-animation"></a>重新定位動畫

使用組合隱含動畫，開發人員可以自動對元素配置中的變更產生相對於其父項的動畫。 例如，如果您變更下方按鈕的 **Margin**，則它會自動對其新的配置位置產生動畫。

#### <a name="implementation-overview"></a>實作概觀

1. 取得目標元素的交出 **Visual**
1. 建立 **ImplicitAnimationCollection**，它可以自動對 **Offset** 屬性中的變更產生動畫效果
1. 將 **ImplicitAnimationCollection** 與支援的 Visual 產生關聯

```xaml
<Button x:Name="RepositionTarget" Content="Click Me" />
```

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeRepositionAnimation(RepositionTarget);
}

private void InitializeRepositionAnimation(UIElement repositionTarget)
{
    var targetVisual = ElementCompositionPreview.GetElementVisual(repositionTarget);
    Compositor compositor = targetVisual.Compositor;

    // Create an animation to animate targetVisual's Offset property to its final value
    var repositionAnimation = compositor.CreateVector3KeyFrameAnimation();
    repositionAnimation.Duration = TimeSpan.FromSeconds(0.66);
    repositionAnimation.Target = "Offset";
    repositionAnimation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");

    // Run this animation when the Offset Property is changed
    var repositionAnimations = compositor.CreateImplicitAnimationCollection();
    repositionAnimations["Offset"] = repositionAnimation;

    targetVisual.ImplicitAnimations = repositionAnimations;
}
```

### <a name="drop-shadow"></a>陰影

將完美像素陰影套用到 **UIElement**，例如包含圖片的 **Ellipse**。 因為陰影需要由 app 建立的 **SpriteVisual**，我們必須建立「裝載」元素，它會使用 **ElementCompositionPreview.SetElementChildVisual** 包含 **SpriteVisual**。

#### <a name="implementation-overview"></a>實作概觀

1. 取得裝載元素的交出 **Visual**
2. 建立 Windows.UI.Composition **DropShadow**
3. 透過遮罩，從目標元素取得其形狀來設定 **DropShadow**
    - **DropShadow** 預設是矩形，因此如果目標是矩形就不必這樣做
4. 將陰影附加到新的 **SpriteVisual**，然後將 **SpriteVisual** 設定為裝載元素的子項
5. 使用 **ExpressionAnimation**，繫結 **SpriteVisual** 的大小與裝載的大小

```xaml
<Grid Width="200" Height="200">
    <Canvas x:Name="ShadowHost" />
    <Ellipse x:Name="CircleImage">
        <Ellipse.Fill>
            <ImageBrush ImageSource="Assets/Images/2.jpg" Stretch="UniformToFill" />
        </Ellipse.Fill>
    </Ellipse>
</Grid>
```

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

private void InitializeDropShadow(UIElement shadowHost, Shape shadowTarget)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(shadowHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a drop shadow
    var dropShadow = compositor.CreateDropShadow();
    dropShadow.Color = Color.FromArgb(255, 75, 75, 80);
    dropShadow.BlurRadius = 15.0f;
    dropShadow.Offset = new Vector3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask = shadowTarget.GetAlphaMask();

    // Create a Visual to hold the shadow
    var shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
   ElementCompositionPreview.SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual.StartAnimation("Size", bindSizeAnimation);
}
```

以下兩個清單顯示 [C + + / WinRT](https://aka.ms/cppwinrt) 和 [C + + / CX](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx) 對等的上一個使用相同 XAML 結構的 C&#35; 程式碼。

```cppwinrt
#include <winrt/Windows.UI.Composition.h>
#include <winrt/Windows.UI.Xaml.h>
#include <winrt/Windows.UI.Xaml.Hosting.h>
#include <winrt/Windows.UI.Xaml.Shapes.h>
...
MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost(), CircleImage());
}

int32_t MyProperty();
void MyProperty(int32_t value);

void InitializeDropShadow(Windows::UI::Xaml::UIElement const& shadowHost, Windows::UI::Xaml::Shapes::Shape const& shadowTarget)
{
    auto hostVisual{ Windows::UI::Xaml::Hosting::ElementCompositionPreview::GetElementVisual(shadowHost) };
    auto compositor{ hostVisual.Compositor() };

    // Create a drop shadow
    auto dropShadow{ compositor.CreateDropShadow() };
    dropShadow.Color(Windows::UI::ColorHelper::FromArgb(255, 75, 75, 80));
    dropShadow.BlurRadius(15.0f);
    dropShadow.Offset(Windows::Foundation::Numerics::float3{ 2.5f, 2.5f, 0.0f });
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask(shadowTarget.GetAlphaMask());

    // Create a Visual to hold the shadow
    auto shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow(dropShadow);

    // Add the shadow as a child of the host in the visual tree
    Windows::UI::Xaml::Hosting::ElementCompositionPreview::SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    auto bindSizeAnimation{ compositor.CreateExpressionAnimation(L"hostVisual.Size") };
    bindSizeAnimation.SetReferenceParameter(L"hostVisual", hostVisual);

    shadowVisual.StartAnimation(L"Size", bindSizeAnimation);
}
```

```cpp
#include "WindowsNumerics.h"

MainPage::MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

void MainPage::InitializeDropShadow(Windows::UI::Xaml::UIElement^ shadowHost, Windows::UI::Xaml::Shapes::Shape^ shadowTarget)
{
    auto hostVisual = Windows::UI::Xaml::Hosting::ElementCompositionPreview::GetElementVisual(shadowHost);
    auto compositor = hostVisual->Compositor;

    // Create a drop shadow
    auto dropShadow = compositor->CreateDropShadow();
    dropShadow->Color = Windows::UI::ColorHelper::FromArgb(255, 75, 75, 80);
    dropShadow->BlurRadius = 15.0f;
    dropShadow->Offset = Windows::Foundation::Numerics::float3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow->Mask = shadowTarget->GetAlphaMask();

    // Create a Visual to hold the shadow
    auto shadowVisual = compositor->CreateSpriteVisual();
    shadowVisual->Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
    Windows::UI::Xaml::Hosting::ElementCompositionPreview::SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    auto bindSizeAnimation = compositor->CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation->SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual->StartAnimation("Size", bindSizeAnimation);
}
```

### <a name="frosted-glass"></a>毛玻璃

建立可將背景內容模糊及濃淡的效果。 請注意，開發人員必須安裝 Win2D NuGet 套件才能使用效果。 如需安裝指示，請參閱 [Win2D 首頁](http://microsoft.github.io/Win2D/html/Introduction.htm)。

#### <a name="implementation-overview"></a>實作概觀

1.  取得裝載元素的交出 **Visual**
2.  使用 Win2D 和 **CompositionEffectSourceParameter** 建立模糊效果樹狀結構
3.  根據效果樹狀結構建立 **CompositionEffectBrush**
4.  將 **CompositionEffectBrush** 的輸入設定為 **CompositionBackdropBrush**，如此可讓效果套用至 **SpriteVisual** 後方的內容
5.  將 **CompositionEffectBrush** 設為新的 **SpriteVisual** 的內容，並將 **SpriteVisual** 設為裝載元素的子項。 您可以改為使用 XamlCompositionBrushBase。
6.  使用 **ExpressionAnimation**，將 **SpriteVisual** 的大小與裝載的大小繫結

```xaml
<Grid Width="300" Height="300" Grid.Column="1">
    <Image
        Source="Assets/Images/2.jpg"
        Width="200"
        Height="200" />
    <Canvas
        x:Name="GlassHost"
        Width="150"
        Height="300"
        HorizontalAlignment="Right" />
</Grid>
```

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeFrostedGlass(GlassHost);
}

private void InitializeFrostedGlass(UIElement glassHost)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(glassHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a glass effect, requires Win2D NuGet package
    var glassEffect = new GaussianBlurEffect
    { 
        BlurAmount = 15.0f,
        BorderMode = EffectBorderMode.Hard,
        Source = new ArithmeticCompositeEffect
        {
            MultiplyAmount = 0,
            Source1Amount = 0.5f,
            Source2Amount = 0.5f,
            Source1 = new CompositionEffectSourceParameter("backdropBrush"),
            Source2 = new ColorSourceEffect
            {
                Color = Color.FromArgb(255, 245, 245, 245)
            }
        }
    };

    //  Create an instance of the effect and set its source to a CompositionBackdropBrush
    var effectFactory = compositor.CreateEffectFactory(glassEffect);
    var backdropBrush = compositor.CreateBackdropBrush();
    var effectBrush = effectFactory.CreateBrush();

    effectBrush.SetSourceParameter("backdropBrush", backdropBrush);

    // Create a Visual to contain the frosted glass effect
    var glassVisual = compositor.CreateSpriteVisual();
    glassVisual.Brush = effectBrush;

    // Add the blur as a child of the host in the visual tree
    ElementCompositionPreview.SetElementChildVisual(glassHost, glassVisual);

    // Make sure size of glass host and glass visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    glassVisual.StartAnimation("Size", bindSizeAnimation);
}
```

## <a name="additional-resources"></a>其他資源

- [視覺層概觀](https://msdn.microsoft.com/windows/uwp/composition/visual-layer)
- [**ElementCompositionPreview** 類別](https://msdn.microsoft.com/library/windows/apps/mt608976)
- [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs) 有進階的 UI 和組合範例
- [BasicXamlInterop 範例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
- [ParallaxingListItems 範例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)
