---
title: XAML 屬性動畫
description: 瞭解如何使用通用 Windows 平臺 (UWP) 撰寫動畫，直接在 XAML UIElement 上建立屬性的動畫。
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a7ab119df407b45fb391aebf50df5d85f89ec0f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238293"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>以撰寫動畫製作 XAML 專案的動畫

本文將介紹新的屬性，讓您以撰寫動畫的效能和簡化 XAML 屬性的設定，建立 XAML UIElement 的動畫。

在 Windows 10 版本1809之前，您有2個選擇可在 UWP 應用程式中建立動畫：

- 使用類似[Storyboarded 動畫](storyboarded-animations.md)[的 XAML](/uwp/api/windows.ui.xaml.media.animation)結構，或在 ThemeAnimation 命名空間中使用 _* ThemeTransition_和 _*_ 類別。
- 使用以 [XAML 撰寫的視覺效果圖層](../../composition/using-the-visual-layer-with-xaml.md)中所述的組合動畫。

使用視覺效果層可提供比使用 XAML 結構更好的效能。 但是，使用 [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) 來取得專案的基礎組合 [視覺](/uwp/api/windows.ui.composition.visual) 物件，然後使用複合動畫以動畫顯示視覺效果，就比較複雜。

從 Windows 10 版本1809開始，您可以直接使用撰寫動畫來建立 UIElement 的屬性動畫，而不需要取得基礎組合視覺效果。

> [!NOTE]
> 若要在 UIElement 上使用這些屬性，您的 UWP 專案目標版本必須是1809或更新版本。 如需設定專案版本的詳細資訊，請參閱 [版本調整應用程式](../../debug-test-perf/version-adaptive-apps.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項資源庫</strong> 應用程式，請按一下這裡 <a href="xamlcontrolsgallery:/item/XamlCompInterop">開啟應用程式，並查看作用中的動畫 interop</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>新的呈現屬性取代舊的轉譯屬性

下表顯示您可以用來修改 UIElement 轉譯的屬性，也可以使用 [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)進行動畫。

| 屬性 | 類型 | 說明 |
| -- | -- | -- |
| [不透明度](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | 物件不透明度的程度 |
| [翻譯](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Shift 元素的 X/Y/Z 位置 |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 要套用至元素的轉換矩陣 |
| [縮放比例](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | 調整元素（以 CenterPoint 為中心） |
| [旋轉](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | 在 RotationAxis 和 CenterPoint 周圍旋轉元素 |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 旋轉軸 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 縮放和旋轉的中心點 |

TransformMatrix 屬性值會以下列順序與尺規、旋轉和平移屬性結合： TransformMatrix、縮放、旋轉、轉譯。

這些屬性不會影響專案的版面配置，因此修改這些屬性並不會造成新的[量值](/uwp/api/windows.ui.xaml.uielement.measure) / [排列](/uwp/api/windows.ui.xaml.uielement.arrange)傳遞。

這些屬性與撰寫 [視覺效果](/uwp/api/windows.ui.composition.visual) 類別上的 like 命名屬性具有相同的用途和行為 (除了轉譯，但不在 Visual) 上。

### <a name="example-setting-the-scale-property"></a>範例：設定尺規屬性

此範例示範如何設定按鈕的尺規屬性。

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>新舊屬性之間的相互獨佔性

> [!NOTE]
> 不 **透明度** 屬性不會強制執行本節所述的相互獨佔性。 無論您使用的是 XAML 或複合動畫，都可以使用相同的不透明度屬性。

可使用 CompositionAnimation 進行動畫的屬性會取代數個現有的 UIElement 屬性：

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [System.windows.uielement.rendertransformorigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

當您設定 (或建立) 任何新屬性的動畫時，不能使用舊的屬性。 相反地，如果您將 (或建立動畫) 任何舊的屬性，就無法使用新的屬性。

如果您使用 ElementCompositionPreview 自行使用下列方法來取得和管理視覺效果，也無法使用新的屬性：

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 嘗試混用這兩組屬性會導致 API 呼叫失敗，並產生錯誤訊息。

您可以藉由清除來切換一組屬性，不過為了簡單起見，不建議您這樣做。 如果屬性是由 DependencyProperty (（例如，UIElement）所支援，則為 UIElement。 ProjectionProperty) 支援投影，然後呼叫 System.windows.dependencyobject.clearvalue 將其還原為其「未使用」狀態。 否則 (例如，[小數位數] 屬性) ，請將屬性設定為其預設值。

## <a name="animating-uielement-properties-with-compositionanimation"></a>使用 CompositionAnimation 製作 UIElement 屬性的動畫

您可以使用 CompositionAnimation 來建立資料表中所列之轉譯屬性的動畫。 [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation)也可以參考這些屬性。

在 UIElement 上使用 [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) 和 [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) 方法，以建立 uielement 屬性的動畫。

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>範例：使用 Vector3KeyFrameAnimation 建立尺規屬性的動畫

此範例示範如何以動畫顯示按鈕的尺規。

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>範例：使用 ExpressionAnimation 以動畫顯示尺規屬性

頁面有兩個按鈕。 第二個按鈕的動畫是透過 scale) 作為第一個按鈕的大型 (兩倍。

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
- [轉換總覽](../layout/transforms.md)
