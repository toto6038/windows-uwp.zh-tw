---
author: Jwmsft
title: XAML 屬性的動畫
description: XAML 項目使用組合動畫產生動畫效果。
ms.author: jimwalk
ms.date: 09/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.openlocfilehash: a03ffc8d5ea78ee6cbdf78feaae7ba1cd1448f37
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5398561"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>XAML 項目使用組合動畫產生動畫效果

這篇文章導入了新的屬性，可讓您在 XAML UIElement，使用組合動畫的效能和輕鬆設定 XAML 屬性產生動畫效果。

在 Windows 10 版本 1809 之前, 您有 2 個選項來建置您的 UWP app 中的動畫：

- 使用[腳本動畫](storyboarded-animations.md)，例如 XAML 建構或 _* ThemeTransition_和 _* ThemeAnimation_ [Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation)命名空間中的類別。
- [使用視覺層搭配 XAML](../../composition/using-the-visual-layer-with-xaml.md)中所述，請使用組合動畫。

使用視覺層提供較佳的效能比使用 XAML 建構。 但使用[ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview)呼叫以取得元素的基礎組合[視覺](/uwp/api/windows.ui.composition.visual)物件，並再以動畫顯示的視覺效果使用組合動畫會使用更複雜。

從 Windows 10，版本 1809，您可以產生動畫效果上 UIElement 直接使用組合動畫，而不需要以取得基礎的組合視覺效果的屬性。

> [!NOTE]
> 若要在 UIElement 上使用這些屬性，您 UWP 專案的目標版本必須是 1809年或更新版本。 如需設定您專案的版本的詳細資訊，請參閱[版本調適型應用程式](../../debug-test-perf/version-adaptive-apps.md)。

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>新的轉譯內容取代舊的轉譯內容

下表顯示可用來修改的 UIElement，也[CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)以動畫顯示的轉譯內容。

| 屬性 | 類型 | 描述 |
| -- | -- | -- |
| [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | 雙線 | 物件的不透明度 |
| [Translation](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Shift 元素的 X/Y/Z 位置 |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 將套用到元素的轉換矩陣 |
| [Scale](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | 縮放比例項目，在中心點上置中 |
| [旋轉](/uwp/api/windows.ui.xaml.uielement.rotation) | 浮點數 | 旋轉 RotationAxis 及中心點周圍的項目 |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 軸的旋轉 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 縮放比例及旋轉中心點 |

TransformMatrix 屬性值結合的縮放、 旋轉和轉譯的屬性，以下列順序： TransformMatrix、 縮放、 旋轉、 轉譯。

這些屬性不會影響元素的版面配置，所以，修改這些屬性不會造成新的[度量](/uwp/api/windows.ui.xaml.uielement.measure)/[排列](/uwp/api/windows.ui.xaml.uielement.arrange)階段。

這些屬性都有相同的用途和行為類似名為的屬性上 composition [Visual](/uwp/api/windows.ui.composition.visual)類別 （除了轉譯，不在視覺）。

### <a name="example-setting-the-scale-property"></a>範例： 設定縮放比例屬性

這個範例示範如何在按鈕上設定縮放屬性。

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>新的舊屬性之間的相互專屬性

> [!NOTE]
> **Opacity**屬性不會強制執行本節中所述相互專屬性。 不論您是使用 XAML 或 composition 動畫，您可以使用相同的 Opacity 屬性。

可以使用 CompositionAnimation 來製作動畫效果的屬性會取代項目適用於數個現有的 UIElement 屬性：

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

當您設定 （或產生動畫效果） 的任何新的屬性時，您無法使用的舊屬性。 相反地，如果您設定 （或產生動畫效果） 的任何舊屬性，您無法使用新的屬性。

您也無法使用新的屬性如果您使用 ElementCompositionPreview 取得及管理自行使用這些方法的 「 視覺效果：

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 嘗試混合使用兩個屬性集，會造成 API 呼叫失敗，並產生的錯誤訊息。

它是可能但為了簡單起見不建議方法清除它們，切換從一組屬性。 如果屬性都由 DependencyProperty 支援 （例如，UIElement.Projection 由 UIElement.ProjectionProperty），然後呼叫 ClearValue 將它還原到其 「 未使用 」 狀態。 （例如縮放屬性），否則將屬性設定為其預設值。

## <a name="animating-uielement-properties-with-compositionanimation"></a>使用 CompositionAnimation UIElement 屬性產生動畫效果

您可以使用 CompositionAnimation 表中所列的轉譯內容產生動畫效果。 這些屬性也可以參考的[ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation)。

使用 UIElement 上的[StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation)和[StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation)方法 UIElement 屬性產生動畫效果。

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>範例： 讓 Vector3KeyFrameAnimation 縮放屬性產生動畫效果

這個範例示範如何以動畫顯示按鈕的縮放比例。

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>範例： 讓使用 ExpressionAnimation 縮放屬性產生動畫效果

頁面有兩個按鈕。 第二個按鈕以動畫顯示兩次會一樣大 （透過 scale) 做為第一個按鈕。

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>相關主題

- [腳本動畫](storyboarded-animations.md)
- [使用視覺層搭配 XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [轉換概觀](../layout/transforms.md)