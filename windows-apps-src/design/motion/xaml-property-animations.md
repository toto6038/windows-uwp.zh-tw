---
title: XAML 屬性動畫
description: 以動畫顯示組合動畫的 XAML 項目。
ms.date: 09/13/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 81da1e769ab171e47a4f4046e8ec7e7c84ecf2d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630353"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>以動畫顯示組合動畫的 XAML 項目

這篇文章介紹新的屬性，可讓您以動畫顯示 XAML UIElement 組合動畫的效能和易於設定 XAML 屬性。

在 Windows 10 版本 1809 之前, 中，您必須建置的 UWP app 中的動畫的 2 個選項：

- 使用 XAML 的建構，例如[動畫建立圖片敘述](storyboarded-animations.md)，或有 _* ThemeTransition_並 _* ThemeAnimation_中的類別[Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation)命名空間。
- 中所述，使用組合的動畫[XAML 中使用視覺分層](../../composition/using-the-visual-layer-with-xaml.md)。

使用視覺圖層提供更佳的效能比使用 XAML 建構。 但使用[ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview)取得的項目之基礎組合[視覺化](/uwp/api/windows.ui.composition.visual)物件，並再建立動畫的視覺物件的組合動畫是較為複雜，無法使用。

從 Windows 10 版本 1809，您可以以動畫顯示屬性上使用以取得基礎組合視覺效果撰寫動畫，而不需要直接將 uielement 設。

> [!NOTE]
> 若要使用這些屬性的 UIElement，1809年或更新版本，必須是您的 UWP 專案目標版本。 如需設定您的專案版本的詳細資訊，請參閱[版本的自適性應用程式](../../debug-test-perf/version-adaptive-apps.md)。

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>新的轉譯屬性取代舊的轉譯屬性

下表顯示您可用來修改的 UIElement，還能夠變成動畫與呈現的屬性[CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)。

| 屬性 | 類型 | 描述 |
| -- | -- | -- |
| [不透明度](/uwp/api/windows.ui.xaml.uielement.opacity) | 雙聲道 | 物件的不透明度 |
| [轉譯](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | 移動元素的 X/Y/Z 位置 |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 轉換矩陣套用至項目 |
| [縮放](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | 縮放中心點上置中的項目 |
| [旋轉](/uwp/api/windows.ui.xaml.uielement.rotation) | 浮點數 | 將元素周圍的 RotationAxis 和中心點旋轉 |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 旋轉軸 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 縮放和旋轉中心點 |

TransformMatrix 屬性值結合的小數位數、 旋轉和轉譯的屬性，順序如下：TransformMatrix，小數位數，旋轉，轉譯。

這些屬性不會影響項目的版面配置，因此，修改這些屬性不會造成新[量值](/uwp/api/windows.ui.xaml.uielement.measure)/[排列](/uwp/api/windows.ui.xaml.uielement.arrange)傳遞。

這些屬性具有相同用途和行為類似名稱屬性組合[視覺化](/uwp/api/windows.ui.composition.visual)（除了轉譯，這並不是視覺效果上） 的類別。

### <a name="example-setting-the-scale-property"></a>範例：設定小數位數屬性

此範例示範如何設定按鈕上的小數位數屬性。

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>有新的和舊的屬性之間的互斥性

> [!NOTE]
> **不透明度**屬性不會強制使用有這一節所述的互斥性。 不論您是使用 XAML 或撰寫的動畫，您可以使用相同的不透明度屬性。

可以使用 CompositionAnimation 動畫的屬性會取代數個現有的 UIElement 屬性：

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

當您設定 （或以動畫顯示） 的任何新的屬性時，您無法使用舊的屬性。 相反地，如果您設定 （或以動畫顯示） 的任何舊的屬性，您無法使用新的屬性。

您也無法使用新的屬性。 如果您使用 ElementCompositionPreview 取得並管理您自己使用這些方法的視覺效果：

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 嘗試混合使用這兩組屬性，將會導致 API 呼叫失敗並產生錯誤訊息。

可以在一組屬性，即可切換清除，但為求簡化不建議。 如果此屬性做為後盾的 DependencyProperty （比方說，UIElement.Projection 受到 UIElement.ProjectionProperty），然後呼叫 ClearValue 將它還原至其 「 未使用 」 的狀態。 （例如，小數位數屬性），否則為表示將屬性設定為其預設值。

## <a name="animating-uielement-properties-with-compositionanimation"></a>使用 CompositionAnimation 的 UIElement 屬性建立動畫

您可以建立與 CompositionAnimation 表所列的轉譯屬性的動畫。 這些屬性也可以藉由參考[ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation)。

使用[StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation)並[StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) UIElement 以動畫顯示 UIElement 屬性上的方法。

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>範例：以動畫顯示 Vector3KeyFrameAnimation 的小數位數屬性

此範例示範如何以動畫顯示的小數位數 按鈕。

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>範例：以動畫顯示 ExpressionAnimation 的小數位數屬性

頁面有兩個按鈕。 第二個按鈕會展示動畫至會兩次 （透過小數位數） 一樣大為第一個按鈕。

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

- [建立圖片敘述的動畫](storyboarded-animations.md)
- [使用 XAML 的視覺化的圖層](../../composition/using-the-visual-layer-with-xaml.md)
- [轉換概觀](../layout/transforms.md)
