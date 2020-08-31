---
title: 組合光源
description: 組合光源 Api 可以用來將動態3D 光源新增至您的應用程式。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5531382ef46346a40844a8eb5a5a77c0ad565fbb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154702"
---
# <a name="using-lights-in-windows-ui"></a>在 Windows UI 中使用燈光

Windows. 撰寫 Api 可讓您建立即時動畫和效果。 組合光源可在2D 應用程式中啟用3D 光源。 在此總覽中，我們將逐步解說如何設定組合燈光、識別要接收每個光線的視覺效果，以及使用效果來定義內容的材質。

> [!NOTE]
> 若要閱讀 [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) 物件如何套用 [COMPOSITIONLIGHTS](/uwp/api/Windows.UI.Composition.CompositionLight) 以照亮 xaml UIElements，請參閱 [xaml 光源](xaml-lighting.md)。

撰寫光源可讓您藉由下列方式建立有趣的 UI：

- 轉換與場景中其他物件無關的輕量，以啟用像是音樂播放場景的沉浸式案例。
- 將物件與燈光配對的能力，讓它們與場景的其餘部分分開移動，以啟用如流暢 [顯示](../design/style/reveal.md) 醒目提示的情節。
- 將燈光和整個場景轉換成群組，以建立材質和深度。

組合光源支援三個主要概念： **燈光**、 **目標**和 **SceneLightingEffect**。

## <a name="light"></a>淺色

[CompositionLight](/uwp/api/windows.ui.composition.compositionlight) 可讓您建立各種燈光，並將它們放在座標空間中。 這些燈光會以您想要識別的視覺效果作為視覺效果。

### <a name="light-types"></a>燈光類型

| 類型 | 說明 |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | 發出由場景中所有專案所反映之非方向光線的燈來源。 |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | 無限長的遠距光源，以單一方向發出燈光。 就像 sun 一樣。 |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | 光線的點來源，會以所有方向發出燈光。 和燈泡一樣。 |
| [聚光燈](/uwp/api/windows.ui.composition.spotlight) | 發出燈光內部和外部圓錐的光源。 就像是一個。 |

## <a name="targets"></a>目標

當燈光以 Visual (新增至 [目標](/uwp/api/windows.ui.composition.compositionlight.targets) 清單) ，視覺效果及其所有子系都可感知並回應此光源。 這可以像是在樹狀結構的根目錄設定 PointLight 來源一樣簡單，而且下列所有視覺效果都會回應點燈方向的動畫。

**ExclusionsFromTargets** 可讓您以類似新增目標的方式，移除視覺效果或視覺效果子樹的光源。 結果不會讓樹狀結構中由已排除的視覺效果所根的子系成為亮。

### <a name="sample-targets"></a>範例 (目標) 

在下列範例中，我們使用 CompositionPointLight 來以 XAML TextBlock 為目標。

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

藉由將動畫新增至點燈的位移，可輕鬆達成 shimmering 效果。

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

若要深入瞭解，請參閱 WindowUIDevLabs 範例中的完整 [文字 Shimmer](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 14393/TextShimmer) 範例。

## <a name="restrictions"></a>限制

當您決定要 CompositionLight 哪些內容時，需要考慮幾個因素。

概念 | 詳細資料
--- | ---
**環境光線** | 將非環境光線新增至場景將會關閉所有現有的燈。  非環境光線設為目標的專案會顯示為黑色。  若要以自然方式照亮未以光為目標的視覺效果，請使用環境光源搭配其他燈光。
**燈光數目** | 您可以使用任意組合中的任何兩個非環境組合燈光來鎖定您的 UI。 不限制環境燈;點、點和遠處燈是。
**存留期** | CompositionLight 可能會遇到存留期狀況 (範例：垃圾收集行程可能會在使用) 之前回收燈光物件。  建議您將燈光以成員的形式新增，以協助應用程式管理存留期，以保留您的燈光參考。
**轉換** | 您必須將燈光放置在 UI 的上方節點中，使用視覺效果中的 [觀點轉換](../design/layout/3-d-perspective-effects.md) ，才能正確地繪製。
**目標和座標空間** | CoordinateSpace 是必須設定所有燈光屬性的視覺空間。 CompositionLight 必須在 CoordinateSpace 樹狀結構內。

## <a name="lighting-properties"></a>光源屬性

視使用的光源類型而定，燈光可以有衰減和空間的屬性。 並非所有光線類型均使用所有的屬性。

屬性 | 說明
--- | ---
**色彩** | 光線的 [色彩](/uwp/api/windows.ui.color) 。 光源色彩值是由「 [D3D](../graphics-concepts/light-properties.md) 擴散」、「環境」和「反射」定義，以定義要發出的色彩。 光源針對燈光使用 RGBA 值;未使用 Alpha 色彩元件。
**方向** | 光線的方向。 光線指向的方向是指定為相對於其 [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) 視覺效果。
**座標空間** | 每個視覺效果都有隱含的3D 座標空間。 X 方向是從左至右。 Y 方向是從上到下。 Z 方向是平面的點。 此座標的原始點是視覺效果的左上角，而單位是與裝置無關的圖元 (DIP) 。 在此座標中定義的光線位移。
**內部和外部圓錐** | 聚光燈發出的光線錐體分兩部分︰明亮的內錐和外錐體。 組合可讓您控制內部和外部的錐形角度和色彩。
**Offset** | 光源相對於其座標空間視覺效果的位移。

> [!NOTE]
> 當多個燈觸及相同的視覺效果，或每次光線的色彩值夠大以超過1.0 時，光線的色彩可能會因為燈光色頻的固定而改變。

### <a name="advanced-lighting-properties"></a>Advanced 光源屬性

屬性 | 說明
--- | ---
**強度** | 控制光線的亮度。
**衰減** | 衰減控制光線強度如何隨著範圍屬性指定的最大距離減少。  您可以使用常數、Quadradic 和線性衰減屬性。

## <a name="getting-started-with-lighting"></a>具有光源的消費者入門

請遵循下列一般步驟來新增燈光：

- 建立並放置燈光：建立燈光，然後將它們放在指定的座標空間中。
- 將物件識別為淺色：關聯視覺效果的目標燈。
- 參數定義個別物件對燈光的反應：搭配使用 SceneLightingEffect 與 EffectBrush 來自訂淺反映以顯示 SpriteVisual。 反映預設值支援光線來源 CoordinateSpace 之子系的光源。  以 SceneLightingEffect 繪製的視覺效果會覆寫該視覺效果的預設光源。

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)可用來修改套用至[CompositionLight](/uwp/api/windows.ui.composition.compositionlight)目標[SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual)內容的預設光源。

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 經常用於建立材料。 當您想要達成更複雜的結果時（例如啟用影像上的反射屬性，以及/或使用一般地圖提供深度的幻影），SceneLightingEffect 就是一個效果。 SceneLightingEffect 可讓您使用反射和漫射量等光源屬性來自訂您的 UI。 您可以使用其餘效果管線進一步自訂光源效果，讓您可以個別地混合和撰寫與內容不同的光源反應。

> [!NOTE]
> 場景光源不會產生陰影;它是著重于2D 轉譯的效果。  它不會考慮包含實際光源模型的3D 光源案例，包括陰影在內。


屬性 | 說明
--- | ---
**一般地圖** | NormalMaps 會建立紋理的效果，在正常指向光線的情況下將會更亮，而正常的地方則會很暗。 若要將 NormalMap 新增至您的目標視覺效果，請使用 [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) ，以使用 LoadedImageSurface 來載入 NormalMap 資產。
**環境** | 環境屬性大多用來控制整體的色彩反映。
**反射** | 反射反映會在物件上建立醒目顯示，讓它們看起來很亮。 您可以控制反射反映的層級以及最適合的程度。  系統會操作這些屬性來建立 shinny 貴金屬或光澤紙等材質效果。
**擴散** | 漫射反映 scatters 所有方向的光線。
**反射率模型** | [反射率模型](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) 可讓您選擇 [Blinn Phong](/visualstudio/designers/how-to-create-a-basic-phong-shader) 和實際的 Blinn Phong。  當您想要壓縮高光時，可以選擇實際的 Blinn Phong。

### <a name="sample-scenelightingeffect"></a>範例 (SceneLightingEffect) 

下列範例顯示如何將一般地圖新增至 SceneLightingEffect。

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>相關文章

- [在視覺分層中建立材質和燈光](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [光源總覽](../graphics-concepts/lighting-overview.md)
- [CompositionCapabilities API](/uwp/api/windows.ui.composition.compositioncapabilities)
- [燈光數學](../graphics-concepts/mathematics-of-lighting.md)
- [SceneLightingEffect](/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub 存放庫](https://github.com/microsoft/WindowsCompositionSamples)