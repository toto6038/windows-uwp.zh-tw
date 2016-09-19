---
author: jaster
ms.assetid: 
title: "使用視覺層搭配 XAML"
description: "了解使用視覺層 API 搭配現有 XAML 內容來建立進階動畫及效果的技術。"
translationtype: Human Translation
ms.sourcegitcommit: dfda33c70224f32d9c3e8877eabdfcd965521757
ms.openlocfilehash: 00d663b130202f4513cd1a9d82baed4068d909d3

---

# 使用視覺層搭配 XAML

## 簡介

大部分使用視覺層功能的 app，都會使用 XAML 來定義主要 UI 內容。 在 Windows 10 年度更新版中，XAML 架構和視覺層中的新功能可更輕鬆地結合這兩項技術，以建立令人驚艷的使用者體驗。
XAML 與視覺層「互通性」功能可用來建立單獨使用 XAML API 所無法提供的進階動畫與效果。 這包括：

-              捲動驅動動畫及視差
-              自動配置動畫
-              完美像素陰影
-              模糊和毛玻璃效果

這些效果和動畫可以套用至現有的 XAML 內容，因此您不需要大幅重組您的 XAML app 即可利用新的功能。
配置動畫、陰影和模糊效果，涵蓋在以下的＜做法＞一節中 如需實作視差的程式碼範例，請參閱 [ParallaxingListItems 範例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)。  [WindowsUIDevLabs 存放機制](https://github.com/Microsoft/WindowsUIDevLabs) 也已經有實作動畫、陰影和效果的數個其他範例。

## **ElementCompositionPreview** 類別

[**ElementCompositionPreview**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.aspx) 是靜態類別，提供 XAML 和視覺層互通性功能。 如需視覺層及其功能的概觀，請參閱[視覺層](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer)。 **ElementCompositionPreview** 類別會提供下列方法︰

-   [**GetElementVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx)：取得「交出」Visual，它用來轉譯此元素
-   [**SetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual.aspx)︰設定「交入」Visual 做為此元素視覺化樹狀結構的最後一個子項。 這個 Visual 將會在其餘元素的頂端繪製。 
-   [**GetElementChildVisual**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx)：抓取使用 **SetElementChildVisual** 的 Visual 集合
-   [**GetScrollViewerManipulationPropertySet**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx)：取得可根據 **ScrollViewer** 中的捲動位移，用來建立 60fps 動畫的物件

## **ElementCompositionPreview.GetElementVisual** 備註

**ElementCompositionPreview.GetElementVisual** 會傳回「交出」Visual，它用來轉譯指定的 **UIElement**。 屬性 (例如 **Visual.Opacity**、**Visual.Offset** 和 **Visual.Size**) 是根據 UIElement 狀態為基礎，透過 XAML 架構設定。 這可以使用例如隱含重新定位動畫的技術 (請參閱＜做法＞**)。

請注意，由於 **Offset** 和 **Size** 會設定做為 XAML 架構配置的結果，開發人員在對這些屬性進行修改或是產生動畫效果時應該小心謹慎。 當配置中元素的左上角與其父項的位置相同時，開發人員應僅對 Offset 進行修改或產生動畫效果。 Size 通常不應修改，但存取此屬性可能會很有用。 例如，以下的「陰影」和「毛玻璃」範例就使用交出 Visual 的 Size 作為動畫的輸入。

另請留意，更新的交出 Visual 屬性將不會反映在對應的 UIElement 中。 例如，將 **UIElement.Opacity** 設定為 0.5 會將所對應交出 Visual 的 Opacity 設定為 0.5。 不過，將交出 Visual 的 **Opacity** 設定為 0.5 會導致內容以 50% 的透明度顯示，但不會變更所對應 UIElement 的 Opacity 屬性。

### **Offset** 動畫範例

#### 錯誤

```xml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### 正確

```xml
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

## **ElementCompositionPreview.SetElementChildVisual** 方法

**ElementCompositionPreview.SetElementChildVisual** 可讓開發人員提供將顯示為元素視覺化樹狀結構成員的「交入」Visual。 這可讓開發人員建立「組合島」，其中以 Visual 為主的內容可顯示於 XAML UI 內。 開發人員應該謹慎使用這項技術，因為以 Visual 為主的內容將不會有 XAML 所保證的相同協助工具及使用者體驗。 因此，通常建議這項技術僅在實作自訂效果時使用，例如下方＜做法＞一節中的內容。

## **GetAlphaMask** 方法

[**Image**](https://msdn.microsoft.com/library/windows/apps/br242752)、[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 和 [**Shape**](https://msdn.microsoft.com/library/windows/apps/br243377) 都會實作稱為 **GetAlphaMask** 的方法，它會傳回代表灰階影像與元素形狀的 **CompositionBrush**。 這個 **CompositionBrush** 可做為組合 **DropShadow** 的輸入，如此陰影就可以反映元素的形狀而不是矩形。 這可讓您使用 Alpah 和形狀，針對文字和影像建立完美像素、以輪廓為主的陰影。 如需 API 的範例，請參閱下方的＜陰影＞**。

## 做法

### 重新定位動畫

使用組合隱含動畫，開發人員可以自動對元素配置中的變更產生相對於其父項的動畫。 例如，如果您變更下方按鈕的 **Margin**，則它會自動對其新的配置位置產生動畫。

#### 實作概觀

1.            取得目標元素的交出 **Visual**
2.            建立 **ImplicitAnimationCollection**，它可以自動對 **Offset** 屬性中的變更產生動畫效果
3.            將 **ImplicitAnimationCollection** 與支援的 Visual 產生關聯

#### XAML

```xml
<Button x:Name="RepositionTarget" Content="Click Me" />
```

#### C&#35;

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

### 陰影

將完美像素陰影套用到 **UIElement**，例如包含圖片的 **Ellipse**。 因為陰影需要由 app 建立的 **SpriteVisual**，我們必須建立「裝載」元素，它會使用 **ElementCompositionPreview.SetElementChildVisual** 包含 **SpriteVisual**。

#### 實作概觀

1.            取得裝載元素的交出 **Visual**
2.            建立 Windows.UI.Composition **DropShadow**
3.            透過遮罩，從目標元素取得其形狀來設定 **DropShadow**
    - **DropShadow** 預設是矩形，因此如果目標是矩形就不必這樣做
4.            將陰影附加到新的 **SpriteVisual**，然後將 **SpriteVisual** 設定為裝載元素的子項
5.            使用 **ExpressionAnimation**，將 **SpriteVisual** 的大小與裝載的大小繫結

#### XAML

```xml
<Grid Width="200" Height="200">
    <Canvas x:Name="ShadowHost" />
    <Ellipse x:Name="CircleImage">
        <Ellipse.Fill>
            <ImageBrush ImageSource="Assets/Images/2.jpg" Stretch="UniformToFill" />
        </Ellipse.Fill>
    </Ellipse>
</Grid>
```

#### C&#35;

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

### 毛玻璃

建立可將背景內容模糊及濃淡的效果。 請注意，開發人員必須安裝 Win2D NuGet 套件才能使用效果。 如需安裝指示，請參閱 [Win2D 首頁](http://microsoft.github.io/Win2D/html/Introduction.htm)。

#### 實作概觀

1.            取得裝載元素的交出 **Visual**
2.            使用 Win2D 和 **CompositionEffectSourceParameter** 建立模糊效果樹狀結構
3.            根據效果樹狀結構建立 **CompositionEffectBrush**
4.            將 **CompositionEffectBrush** 的輸入設定為 **CompositionBackdropBrush**，如此可讓效果套用至 **SpriteVisual** 後方的內容
5.            將 **CompositionEffectBrush** 設為新的 **SpriteVisual** 的內容，並將 **SpriteVisual** 設為裝載元素的子項
6.            使用 **ExpressionAnimation**，將 **SpriteVisual** 的大小與裝載的大小繫結

#### XAML

```xml
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

#### C&#35;

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeFrostedGlass(GlassHost);
}

private void InitializedFrostedGlass(UIElement glassHost)
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

## 其他資源：

-   [視覺層概觀](https://msdn.microsoft.com/en-us/windows/uwp/graphics/visual-layer)
-   [**ElementCompositionPreview** 類別](https://msdn.microsoft.com/library/windows/apps/mt608976)
-   [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs) 有進階的 UI 和組合範例
-   [BasicXamlInterop 範例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
-   [ParallaxingListItems 範例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)



<!--HONumber=Aug16_HO3-->


