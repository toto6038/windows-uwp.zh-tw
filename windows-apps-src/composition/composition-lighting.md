---
title: 撰寫光源
description: 撰寫光源 Api 可用來將動態 3D 光源新增至您的應用程式。
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 733ce75942a05482ade88c1510e788f1cbd515d4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602203"
---
# <a name="using-lights-in-windows-ui"></a>使用 Windows UI 中的光線

Windows.UI.Composition Api 可讓您建立即時的動畫和效果。 撰寫光源可讓 2D 應用程式中的 3D 光源。 在此概觀中，我們會執行透過如何設定組合的號誌、 識別視覺效果，以接收每個光線，和使用效果來定義對您的內容資料的功能。

> [!NOTE]
> 如何讀取[XamlLight](/uwp/api/windows.ui.xaml.media.xamllight)物件套用[CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight)照亮 XAML Uielement，請參閱[XAML 光源](xaml-lighting.md)。

撰寫光源可讓您建立有趣藉由使用的 UI:

- 若要啟用擬真案例，例如音樂的播放場景場景中其他物件的淺色獨立的轉換。
- 能夠配對光線的物件，因此它們會一起移動獨立於其餘的場景方向，以啟用案例，例如 Fluent[揭露](/windows/uwp/design/style/reveal)反白顯示。
- 為群組，以建立資料和深度的光線和整個場景轉換。

撰寫光源支援三個主要概念：**Light**，**目標**，以及**SceneLightingEffect**。

## <a name="light"></a>亮

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight)可讓您建立各種光線，並將它們放在座標空間。 這些 led 燈號是以您想要識別亮起光線的視覺效果為目標。

### <a name="light-types"></a>光線類型

| 類型 | 描述 |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | 藉由在場景中的所有項目會反映發出出現的非方向性光線的光源。 |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | 無限大遙遠的光源發出以單一方向的光線。 例如太陽。 |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | 發出在所有方向的光線的燈光的點來源。 例如燈泡。 |
| [SpotLight](/uwp/api/windows.ui.composition.spotlight) | 發出內部和外部圓錐體光線的光源。 例如手電筒。 |

## <a name="targets"></a>目標

當號誌目標視覺效果 (加入[目標](/uwp/api/windows.ui.composition.compositionlight.targets)清單)，視覺效果及其所有子系已知悉並回應此光線的來源。 這可以是項目和點光線方向的動畫回應 PointLight 來源根目錄中的樹狀結構和所有視覺效果下方的設定一樣簡單。

**ExclusionsFromTargets**可讓您能夠以類似的方式，與新增目標移除視覺效果或樹狀子目錄的視覺效果的光源。 在樹狀目錄中已排除的視覺效果的根的子系不會如此一來會亮起。

### <a name="sample-targets"></a>範例 （目標）

在下列範例中，我們會使用 CompositionPointLight 目標 XAML TextBlock。

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

將動畫加入至 點光線的位移，輕鬆實現閃爍效果。

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

請參閱完整[文字 Shimmer](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer)在 WindowUIDevLabs 範例鉛字盤，若要了解更多範例。

## <a name="restrictions"></a>限制

有數個時判斷哪些內容將會亮起 CompositionLight 所要考量的因素。

概念 | 詳細資料
--- | ---
**環境光線** | 新增至場景的非環境的光線，將會關閉所有現有的光線。  不針對非環境的光線的項目會顯示黑色。  若要照亮周圍不自然的方式，以針對光線的視覺效果，其他的燈號搭配使用的環境光線。
**號誌的數目** | 您可以使用任意組合的任何兩個非環境的組合燈號，指向您的 UI。 環境的燈號不會限制;找出，點和較遠的光線。
**存留期** | CompositionLight 可能會遇到存留期條件 (範例： 記憶體回收行程可能會回收光線的物件，才能使用)。  我們建議您保留燈光的參考，以協助管理存留期的應用程式的成員身分加入燈號。
**轉換** | 號誌必須放在節點上方 UI 使用類似的效果[觀點來看轉換](/windows/uwp/design/layout/3-d-perspective-effects)正確繪製視覺結構中。
**目標和座標空間** | CoordinateSpace 是號誌屬性必須設定所有的視覺空間。 CompositionLight.Targets 必須 CoordinateSpace 樹狀結構中。

## <a name="lighting-properties"></a>光源屬性

根據使用的燈光的類型，光線可以有衰減和空間的屬性。 並非所有光線類型均使用所有的屬性。

屬性 | 描述
--- | ---
**Color** | [色彩](/uwp/api/windows.ui.color)燈光。 光源的色彩值由定義[D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) Diffuse、 Ambient，以及定義發出色彩的反射。 光源的燈號; 使用 RGBA 值不使用 alpha 色彩元件。
**方向** | 燈光的方向。 指定燈光會指出指標的方向相對於其[CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace)視覺化。
**座標空間** | 每個視覺項目具有隱含的 3D 座標空間。 X 方向是從左到右。 Y 方向是從上到下。 Z 方向是從平面的點。 原始的點，此座標的左上角的 視覺效果，且單位是裝置獨立像素 (DIP)。 在此座標所定義的燈號位移。
**內部和外部錐形** | 聚光燈發出的光線錐體分兩部分︰明亮的內錐和外錐體。 複合功能可讓您控制內部和外部的圓錐形角度和色彩。
**Offset** | 光源相對於其座標空間 Visual 的位移。

> [!NOTE]
> 當多個號誌叫用相同的視覺效果，或每當光線色彩值取得夠大，無法超過 1.0 時，可能會因為光線色彩色板的固定變更燈光的色彩。

### <a name="advanced-lighting-properties"></a>進階光源屬性

屬性 | 描述
--- | ---
**濃度** | 控制燈光的亮度。
**衰減** | 衰減控制光線強度如何隨著範圍屬性指定的最大距離減少。  常數，可以使用 Quadradic 和線性衰減屬性。

## <a name="getting-started-with-lighting"></a>Getting Started with 光源

請遵循下列一般步驟來新增燈號：

- 建立並放置燈號：建立號誌，並將它們放在指定的座標空間。
- 識別要光線的物件：目標黑暗中相關的視覺效果。
- [選用]定義如何個別物件因應指示燈：使用自訂淺色反映來顯示 SpriteVisual EffectBrush 使用 SceneLightingEffect。 反映預設支援子系的光源的 CoordinateSpace 的光源。  使用 SceneLightingEffect 繪製視覺效果會覆寫該視覺效果的預設光源。

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)用來修改套用至的內容預設光源[SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual)目標[CompositionLight](/uwp/api/windows.ui.composition.compositionlight)。

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)經常用於材質的建立。 SceneLightingEffect 會影響使用當您想要達到更為複雜，例如啟用映像上的反射屬性及/或法線貼圖提供深度的幻影。 SceneLightingEffect 讓您能夠使用反射光源和擴散的數量等的光源屬性來自訂您的 UI。 您可以進一步自訂可讓您分別可混合使用，並撰寫您的內容使用不同的光源反應的效果管線的其餘部分的光源效果。

> [!NOTE]
> 場景光線不會產生陰影;它是著重於 2D 轉譯的效果。  它不會納入考量 3D 光源案例需要將真實光源模型，包括陰影。


屬性 | 描述
--- | ---
**法線貼圖** | NormalMaps 建立的紋理，燈光正常指向會更亮，因此一般指離開調光器的效果。 若要加入您的目標 visual 使用 NormalMap [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)使用 LoadedImageSurface 載入 NormalMap 資產。
**環境** | 環境屬性主要用來控制整體的色彩反映。
**Specular** | 反射反映物件，使其出現閃亮上建立反白顯示。 您可以控制的反射反映層級和發亮的層級。  這些屬性會操作來建立材質的效果，例如 shinny 金屬等或光面紙張。
**擴散** | 顆漫射型的反映 scatters 往所有方向的光線。
**Reflectance 模型** | [Reflectance 模型](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel)可讓您選擇之間[Blinn Phong](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader)與實際基礎 Blinn Phong。  當您想要有壓縮反射反白顯示，您會選擇實際基礎 Blinn Phong。

### <a name="sample-scenelightingeffect"></a>範例 (SceneLightingEffect)

下列範例示範如何將 SceneLightingEffect 法線貼圖。

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

- [建立的資料和視覺分層中的光線](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [光源概觀](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [光源的數學運算](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub 存放庫](https://github.com/Microsoft/WindowsUIDevLabs)
