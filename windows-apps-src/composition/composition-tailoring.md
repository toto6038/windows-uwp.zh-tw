---
title: 撰寫量身打造
description: 使用組合 Api 來量身打造您的 UI、將效能優化，以及容納使用者設定和裝置特性。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d4aa82f70e9bad7a60a97b6b28f28f3dfd008c9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166402"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>使用 Windows UI 量身打造效果 & 體驗

Windows UI 提供許多美觀的效果、動畫和差異化方法。 不過，若要在建立成功的應用程式時，符合使用者對效能和自訂能力的預期，仍然是必要的部分。 通用 Windows 平臺支援一系列具有不同特性和功能的大型多樣化裝置。 若要為您的所有使用者提供內含體驗，您必須確保您的應用程式能在裝置上調整規模，並遵守使用者喜好設定。 UI 量身打造可提供有效率的方式來利用裝置的功能，並確保愉快和內含的使用者體驗。

UI 量身打造是一種廣泛的類別，包含適用于高效能、美觀的 UI （與下列領域相關）：

- 針對效果的使用者設定尊重和適應
- 適應動畫的使用者設定
- 將指定硬體功能的 UI 優化

在此，我們將討論如何使用上述區域中的視覺效果層來量身打造您的效果和動畫，但還有許多其他方法可量身打造您的應用程式，以確保絕佳的終端使用者體驗。 您可以使用指導檔，瞭解如何針對各種裝置 [量身打造您的 ui](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) ，並 [建立回應式 ui](../design/layout/responsive-design.md)。

## <a name="user-effects-settings"></a>使用者效果設定

使用者可能會基於各種原因來自訂其 Windows 體驗，應用程式應該遵守並適應。 使用者可以控制的其中一個區域，是變更他們在整個系統中看到使用的效果類型。

### <a name="transparency-effects-settings"></a>透明度效果設定

使用者可以自訂的其中一個效果設定是開啟/關閉透明效果。 這可以在 [個人化 > 色彩] 下的 [設定] 應用程式中找到，或透過 [設定] > 輕鬆存取 > 顯示。

![設定中的透明度選項](images/tailoring-transparency-setting.png)

開啟時，任何使用透明度的效果都會如預期般出現。 這適用于 Acrylic、HostBackdropBrush 或任何不是完全不透明的自訂效果圖形。

當關閉時，acrylic 材質會自動切換回純色，因為 XAML 的 acrylic 筆刷預設會聆聽此事件。 在此，我們會在未啟用透明效果時，看到計算機應用程式適當地切換回純色：

![具有 Acrylic ](images/tailoring-acrylic.png)
 ![ 計算機且 Acrylic 回應透明度設定的計算機](images/tailoring-acrylic-fallback.png)

不過，針對任何自訂效果，應用程式必須回應 [UISettings AdvancedEffectsEnabled](/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabled) 屬性或 [AdvancedEffectsEnabledChanged](/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) 事件，並切換出效果/效果圖形，以使用沒有透明度的效果。 範例如下：

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool advancedEffects = uisettings.AdvancedEffectsEnabled;
   uisettings.AdvancedEffectsEnabledChanged += Uisettings_AdvancedEffectsEnabledChanged;
}

private void Uisettings_AdvancedEffectsEnabledChanged(UISettings sender, object args)
{
    // TODO respond to sender.AdvancedEffectsEnabled
}
```

## <a name="animations-settings"></a>動畫設定

同樣地，應用程式應該接聽並回應 [UISettings AnimationsEnabled](/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) 屬性，以確保動畫會根據設定 > 輕鬆存取 > 顯示中的使用者設定來開啟或關閉。

![設定中的動畫選項](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>運用功能 API

藉由運用 [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) api，您可以偵測到哪些組合功能可供使用，並在指定的硬體上進行效能調整，並量身打造設計，以確保使用者在任何裝置上獲得高效能且美觀的體驗。 Api 提供一種方法來檢查硬體系統功能，以在各種外型規格之間執行適當的效果縮放。 這可讓您輕鬆地適當地調整應用程式，以建立美觀且流暢的使用者體驗。

此 API 提供方法和事件接聽程式，可用來對應用程式 UI 做出有效的調整決策。 這項功能會偵測系統如何處理複雜的組合和轉譯作業，然後將資訊傳回給開發人員使用的簡單模型。

### <a name="using-composition-capabilities"></a>使用組合功能

CompositionCapabilities 功能已運用於 Acrylic 材質之類的功能，其中的材質會根據案例和硬體回復為更具效能的效果。

您可以透過幾個簡單的步驟，將 API 新增至現有的程式碼。

1. 取得應用程式的函式中的功能物件。

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 為您的應用程式註冊功能變更事件接聽程式。

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 將內容加入至事件回呼方法以處理各種功能層級。 這可能會或可能不會類似下一個步驟。
1. 使用效果時，請先檢查功能物件。 請考慮使用條件式檢查或 switch control 語句，這取決於您要如何自訂效果。

    ```cs
    if (_capabilities.AreEffectsSupported())
    {
        // Add incremental effects updates here

        if (_capabilities.AreEffectsFast())
        {
            // Add more advanced effects here where applicable
        }
    }
    ```

您可以在 [WINDOWS UI Github](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities)存放庫中找到完整的範例程式碼。

## <a name="fast-vs-slow-effects"></a>快速與緩慢效果

根據在 CompositionCapabilities API 中提供的 [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) 和 [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) 方法提供的意見反應，應用程式可以決定是否要交換針對裝置優化的其他選項效果的昂貴或不支援效果。 已知某些效果的資源比其他更多，因此應謹慎使用，而且可以更輕鬆地使用其他效果。 不過，針對所有效果，請小心在連結和製作動畫時使用，因為某些案例或組合可能會變更效果圖形的效能特性。 以下是個別效果的一些經驗法則效能特性：

- 已知會有高效能影響的效果如下：高斯模糊、陰影遮罩、BackDropBrush、HostBackDropBrush 和圖層視覺效果。 針對低終端裝置，不建議使用這些 [ 功能 (功能等級 9.1-9.3) ](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)，而且應該在高階裝置上謹慎使用。
- 以中等效能影響的效果包括色彩矩陣、某些 Blend 效果 BlendModes (亮度、色彩、飽和度和色調) 、焦點、SceneLightingEffect 和 (（視) BorderEffect 的案例而定）。 這些影響可能適用于低終端裝置上的特定案例，但請小心在連結和製作動畫時使用。 建議限制使用於兩個或更少的時間，並只在轉換時製作動畫。
- 所有其他效果都有低效能影響，並可在製作動畫和連結時的所有合理案例中運作。

## <a name="related-articles"></a>相關文章

- [UWP 回應式設計技巧](../design/layout/responsive-design.md)
- [UWP 裝置量身打造](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)