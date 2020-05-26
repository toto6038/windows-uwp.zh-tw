---
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: 轉換概觀
description: 了解如何藉由變更 UI 中元素的相對座標系統，在 Windows 執行階段&\#160;API 中使用轉換。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 28dc4a62bf580da41d424c98c186413dc96a8aae
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775822"
---
# <a name="transforms-overview"></a>轉換概觀

了解如何藉由變更 UI 中元素的相對座標系統，在 Windows 執行階段 API 中使用轉換。 這可以用來調整個別 XAML 元素的外觀，例如在 x-y 空間中縮放、旋轉或轉換位置。

## <a name="span-idwhat_is_a_transform_spanspan-idwhat_is_a_transform_spanspan-idwhat_is_a_transform_spanwhat-is-a-transform"></a><span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>什麼是轉換？

「轉換」定義了如何將點從一個座標空間對應或轉換到另一個座標空間。 將轉換套用到 UI 元素時，會變更該 UI 元素轉譯為螢幕上 UI 一部分的方式。

轉換可分為四大類：轉譯、旋轉、縮放及扭曲 (傾斜)。 為了使用圖形 API 來變更 UI 元素的外觀，通常最簡單的方式就是建立一次只定義一個操作的轉換。 因此，Windows 執行階段為這些轉換分類中的每一個都定義了個別的類別：

-   [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)：藉由設定 [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x) 與 [**Y**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.y) 的值，在 x-y 空間中轉譯元素。
-   [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform)：藉由設定 [**CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centerx)、[**CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centery)、[**ScaleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scalex) 及 [**ScaleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scaleyproperty) 的值，根據中心點縮放轉換。
-   [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform)：藉由設定 [**Angle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle)、[**CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centerx) 及 [**CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centery) 的值，在 x-y 空間中旋轉。
-   [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform)：藉由設定 [**AngleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.anglex)、[**AngleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.angley)、[**CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.centerx) 及 [**CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centeryproperty) 的值，在 x-y 空間中扭曲或傾斜。

在這些當中，您最常針對 UI 案例使用的會是 [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) 與 [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform)。

您可以將不同的轉換結合使用，而有兩個 Windows 執行階段類別支援這麼做：[**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform) 與 [**TransformGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TransformGroup)。 在 **CompositeTransform** 中，轉換的套用順序依序是：縮放、扭曲、旋轉、轉譯。 如果您希望以不同的順序套用轉換，請以 **TransformGroup** 代替 **CompositeTransform**。 如需詳細資訊，請參閱 [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform)。

## <a name="span-idtransforms_and_layoutspanspan-idtransforms_and_layoutspanspan-idtransforms_and_layoutspantransforms-and-layout"></a><span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>轉換與配置

在 XAML 配置中，版面配置階段完成之後才會套用轉換，因此是在套用轉換前，先完成可用空間計算與其他配置決策。 因為會先進行配置，所以如果您轉換位於 [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) 儲存格或是會在配置期間配置空間的類似配置容器中的元素，有時會收到非預期的結果。 經過轉換的元素可能會顯示成被截斷或被遮住，因為它嘗試在分割其父容器內的空間時，繪製到未計算轉換後尺寸的區域。 您可能需要對轉換結果進行實驗並調整一些設定。 例如，您可能需要變更 **Center** 屬性，或是宣告配置空間的固定像素度量，而不倚賴調適型配置與比例縮放，以確保父容器配置足夠的空間。

**移轉注意事項：** Windows Presentation Foundation (WPF) 已有在版面配置階段前先套用轉換的 **LayoutTransform** 屬性。 但 Windows 執行階段 XAML 不支援 **LayoutTransform** 屬性。 (Microsoft Silverlight 也沒有此選項)。

或者，Windows 社區工具組會提供 [LayoutTransformControl](/windows/communitytoolkit/controls/LayoutTransformControl)，可將矩陣轉換應用於應用程式的任何 FrameworkElement。

## <a name="span-idapplying_a_transform_to_a_ui_elementspanspan-idapplying_a_transform_to_a_ui_elementspanspan-idapplying_a_transform_to_a_ui_elementspanapplying-a-transform-to-a-ui-element"></a><span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>將轉換套用到 UI 元素

當您將轉換套用到物件時，一般是用來設定 [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 屬性。 設定這個屬性實際上並不會將物件依像素逐步變更。 這個屬性實際上所做的是在該物件所在的區域座標空間內套用轉換。 然後，轉譯邏輯與操作 (配置後) 會轉譯經過結合的座標空間，看起來就像物件已變更了外觀且可能變更其配置位置 (如果已套用 [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform))。

根據預設，每個轉譯轉換都是以目標物件之區域座標系統的原點做為中心—即 (0,0)。 唯一的例外是 [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)，它沒有中心屬性可供設定，因為不論其中心在哪裡，轉譯效果都相同。 但就其他轉換而言，每個都有可設定 **CenterX** 與 **CenterY** 值的屬性。

每當您以 [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 來使用轉換時，請記住在會影響轉換行為的 [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 上有另一個屬性：[**RenderTransformOrigin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)。 **RenderTransformOrigin** 所宣告的是整個轉換應該要套用到元素的預設 (0,0) 點，還是要套用到該元素之相對座標空間中的某個其他原始點。 就一般元素來說，(0,0) 會將轉換置於左上角。 視您想要獲得的效果而定，可以選擇變更 **RenderTransformOrigin**，而不是調整轉換上的 **CenterX** 與 **CenterY** 值。 請注意，如果您同時套用了 **RenderTransformOrigin** 與 **CenterX** / **CenterY** 值，結果可能會相當混淆，尤其是當您以動畫顯示這當中任何一個值的時候。

基於點擊測試目的，已套用轉換的物件會在 x-y 空間中以和其視覺外觀一致的預期方式，繼續回應輸入。 例如，如果您已使用 [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) 將 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 在 UI 中橫向移動 400 像素，當使用者按下 **Rectangle** 以視覺方式出現的點時，該 **Rectangle** 會回應 [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) 事件。 如果使用者按下 **Rectangle** 在轉譯前的區域，您並不會收到錯誤事件。 對任何影響點擊測試的 z-index 考量來說，套用轉換並不會帶來任何不同；規範哪個元素處理在 x-y 空間點的輸入事件的 z-index 仍然使用容器中宣告的子系順序進行評估。 雖然對於 [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 物件的子元素而言，您可以透過將 [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) 附加屬性套用到子元素以調整順序，但該順序通常會與您在 XAML 中宣告元素的順序相同。

## <a name="span-idother_transform_propertiesspanspan-idother_transform_propertiesspanspan-idother_transform_propertiesspanother-transform-properties"></a><span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>其他轉換屬性

-   [**Brush.Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.transform)、[**Brush.RelativeTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.relativetransform)：這些會影響 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 在套用 **Brush** 以設定視覺屬性 (例如前景與背景) 的區域內如何使用座標空間。 這些轉換與最常見的筆刷 (通常以 [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 設定純色) 無關，但是以 [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 或 [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 繪製區域時，可能偶爾會有用。
-   [**Geometry.Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.geometry.transform)：您可以使用這個屬性，在將某個幾何圖形用於 [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data) 屬性值之前，先將轉換套用到該幾何圖形。

## <a name="span-idanimating_a_transformspanspan-idanimating_a_transformspanspan-idanimating_a_transformspananimating-a-transform"></a><span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>以動畫顯示轉換

[**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) 物件可以用動畫顯示。 若要以動畫顯示 **Transform**，請將相容類型的動畫套用到您想要以動畫顯示的屬性。 因為所有轉換屬性都是 [**Double**](https://docs.microsoft.com/dotnet/api/system.double) 類型，這通常表示您正使用 [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) 或 [**DoubleAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doubleanimationusingkeyframes) 物件定義動畫。 影響用於 [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 值之轉換的動畫並不會被視為相依式動畫，即使它們的持續時間並不為零也一樣。 如需有關相依式動畫的詳細資訊，請參閱[腳本動畫](/windows/uwp/design/motion/storyboarded-animations)。

如果您的動畫屬性會產生在純視覺外觀上轉換的類似效果，例如以動畫顯示 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 的 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 與 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 而不套用 [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)，則這類動畫幾乎一律會視為相依式動畫。 您將必須啟用動畫，而該動畫可能會有明顯的效能問題，尤其是當您嘗試在以動畫顯示物件時支援使用者互動的情況中。 基於該理由，建議使用轉換並以動畫顯示，而不要以動畫顯示動畫會被視為相依式動畫的任何其他屬性。

若要以轉換為目標，必須要有一個現有的 [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) 做為 [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 的值。 您一般會將適當轉換類型的元素放在初始 XAML 中，有時並不在該轉換上設定任何屬性。

您一般會使用間接目標設定技術將動畫套用到轉換的屬性。 如需有關間接目標設定語法的詳細資訊，請參閱[腳本動畫](/windows/uwp/design/motion/storyboarded-animations)與[屬性路徑語法](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax)。

控制項的預設樣式有時會將轉換的動畫定義為它們視覺狀態行為的一部分。 例如，[**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 的視覺狀態會使用以動畫顯示的 [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) 值來旋轉環圈中的點。

以下是一個如何以動畫顯示轉換的簡單範例。 在這種情況下，是以動畫來顯示 [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) 的 [**Angle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle)，將 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 繞著其視覺中心就地旋轉。 這個範例指名了 **RotateTransform**，因此不需要間接動畫目標設定，但是您可以選擇不指名轉換、指名套用轉換的元素，然後使用間接目標設定 (例如 `(UIElement.RenderTransform).(RotateTransform.Angle)`)。

```xml
<StackPanel Margin="15">
  <StackPanel.Resources>
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
       Storyboard.TargetName="myTransform"
       Storyboard.TargetProperty="Angle"
       From="0" To="360" Duration="0:0:5" 
       RepeatBehavior="Forever" />
    </Storyboard>
  </StackPanel.Resources>
  <Rectangle Width="50" Height="50" Fill="RoyalBlue"
   PointerPressed="StartAnimation">
    <Rectangle.RenderTransform>
      <RotateTransform x:Name="myTransform" Angle="45" CenterX="25" CenterY="25" />
    </Rectangle.RenderTransform>
  </Rectangle>
</StackPanel>
```

```xml
void StartAnimation (object sender, RoutedEventArgs e) {
    myStoryboard.Begin();
}
```

## <a name="span-idaccounting_for_coordinate_frames_of_reference_at_run_timespanspan-idaccounting_for_coordinate_frames_of_reference_at_run_timespanspan-idaccounting_for_coordinate_frames_of_reference_at_run_timespanaccounting-for-coordinate-frames-of-reference-at-run-time"></a><span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>說明執行階段的參考座標框架

[**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 有一個名為 [**TransformToVisual**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transformtovisual), 的方法，這個方法可以產生將兩個 UI 元素的參考座標框架相互關聯的 [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)。 如果您將根視覺項目當做第一個參數來傳遞，便可以使用這個方法將元素與應用程式的預設參考座標框架做比較。 如果您已經從不同元素擷取輸入事件，或正嘗試在不實際要求版面配置階段的情況下預測配置行為，這就有用處。

從指標事件取得的事件資料可提供 [**GetCurrentPoint**](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpoint.getcurrentpoint) 方法的存取權，您可以在該方法指定 *relativeTo* 參數，以將參考的座標框架變更成特定的元素，而非應用程式預設值。 這種方法只是在內部套用轉譯轉換，並在建立傳回的 [**PointerPoint**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint) 物件時，為您轉換 x-y 座標資料。

## <a name="span-iddescribing_a_transform_mathematicallyspanspan-iddescribing_a_transform_mathematicallyspanspan-iddescribing_a_transform_mathematicallyspandescribing-a-transform-mathematically"></a><span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>從數學觀點描述轉換

轉換可以用轉換矩陣來描述。 3×3 矩陣可用來描述二維 x-y 平面的轉換。 仿射轉換矩陣可以相乘來形成任何數目的線性轉換 (例如旋轉與扭曲 (傾斜))，後面再接著轉譯。 仿射轉換矩陣的最終欄位等於 (0, 0, 1)，因此，您只需要以數學描述指定前兩個欄位中的成員。

如果您具有數學背景，或是熟悉也使用矩陣描述座標空間轉換的圖形程式設計技術，那麼轉換的數學描述可能對您很有用。 有一個 [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) 衍生類別可讓您以轉換的 3×3 矩陣來直接表示該轉換：[**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform)。 **MatrixTransform** 具有 [**Matrix**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrixtransform.matrix) 屬性，這個屬性有一個包含下列六個屬性的結構：[**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11)、[**M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12)、[**M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21)、[**M22**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22)、[**OffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) 及 [**OffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety)。 每個 [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) 屬性都使用一個 **Double** 值，並且對應至仿射轉換矩陣的六個相關值 (第 1 與第 2 欄)。

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11)         | [**M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21)         | 0   |
| [**M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12)         | [**M22**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22)         | 0   |
| [**OffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) | [**OffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety) | 1   |

 

任何您能夠以 [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)、[**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform)、[**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) 或 [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform) 物件描述的轉換，都能夠同樣以 [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform) 搭配 [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) 值來描述。 但是您一般只會使用 **TranslateTransform** 及其他類別，因為與設定 **Matrix** 中的向量元件相比，將這些轉換類別的相關屬性概念化會比較容易。 以動畫顯示轉換的個別屬性也同樣比較容易；**Matrix** 實際上是一個結構而不是 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)，因此無法支援以動畫顯示的個別值。

有些可讓您套用轉換操作功能的 XAML 設計工具會將結果序列化為 [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform)。 在這種情況下，最好再次使用相同的設計工具變更轉換效果，並將 XAML 再次序列化，而不要自行嘗試直接操縱 XAML 中的 [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) 值。

## <a name="span-id3-d_transformsspanspan-id3-d_transformsspanspan-id3-d_transformsspan3-d-transforms"></a><span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>3D 轉換

在 Windows 10 中，XAML 導入新屬性 [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)，可讓使用者建立 UI 的 3D 效果。 若要執行這項操作，請使用 [**PerspectiveTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.perspectivetransform3d) 將共用 3D 透視或「相機」新增至場景，然後使用 [**CompositeTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.compositetransform3d) 在 3D 空間中轉換元素，就如同使用 [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform) 般。 如需有關 3D 轉換實作方式的討論，請參閱 [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)。

 如需僅套用至單一物件的簡易型 3D 效果，則可使用 [**UIElement.Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection) 屬性。 使用 [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) 做為此屬性的值，其等同於將固定透視轉換與一或多個 3D 轉換套用至元素。 在 [XAML UI 的 3D 透視效果](3-d-perspective-effects.md)中，會深入詳述有關此轉換類型的資訊。

## <a name="span-idrelated_topicsspanrelated-topics"></a><span id="related_topics"></span>相關主題

* [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)
* [XAML UI 的 3D 透視效果](3-d-perspective-effects.md)
* [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)
 

 





