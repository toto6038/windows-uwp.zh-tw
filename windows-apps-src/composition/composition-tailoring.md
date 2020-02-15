---
title: 撰寫量身打造
description: 使用撰寫 Api 來量身打造您的 UI、針對效能優化，以及配合使用者設定和裝置特性。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 95a7355241f9ba4cc7b4bb743b78ac09169d65d9
ms.sourcegitcommit: 2747d9266e1678fca96d3822ce47499ca91a2c70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213675"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>使用 Windows UI 量身打造效果 & 體驗

Windows UI 提供許多美觀的效果、動畫，以及用於區別的方法。 不過，符合使用者對效能和自訂能力的期望，仍然是建立成功應用程式的必要部分。 通用 Windows 平臺支援一系列大型、多樣化的裝置，其具有不同的特性和功能。 若要為您的所有使用者提供內含體驗，您必須確保您的應用程式會在裝置間調整，並尊重使用者喜好設定。 UI 量身打造可以提供有效率的方式來利用裝置的功能，並確保您的經驗和內含的使用者體驗更加愉快。

UI 量身打造是一個廣泛的類別，其中包含適合下列領域的高效能、美觀 UI：

- 尊重和適應使用者設定的效果
- 容納動畫的使用者設定
- 優化指定硬體功能的 UI

在這裡，我們將討論如何使用上述區域中的視覺分層來量身打造您的效果和動畫，但還有許多其他方法可調整您的應用程式，以確保絕佳的使用者體驗。 指引檔可讓您瞭解如何針對各種裝置[量身打造您的 ui](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) ，以及如何[建立回應式 ui](/windows/uwp/design/layout/responsive-design)。

## <a name="user-effects-settings"></a>使用者效果設定

使用者可以基於各種原因自訂其 Windows 體驗，應用程式應遵循並適應。 使用者可以控制的其中一個區域，是變更他們在整個系統中看到的效果類型。

### <a name="transparency-effects-settings"></a>透明度效果設定

使用者可以自訂的其中一個效果設定是開啟/關閉透明度效果。 這可以在 個人化 > 色彩 底下的 設定 應用程式中找到，或透過 設定 應用程式 > 輕鬆存取 > 顯示。

![[設定] 中的 [透明度] 選項](images/tailoring-transparency-setting.png)

開啟時，任何使用透明度的效果都會如預期般出現。 這適用于壓克力、HostBackdropBrush 或任何不是完全不透明的自訂效果圖形。

當關閉時，壓克力材質會自動切換回純色，因為 XAML 的壓克力筆刷預設會接聽這個事件。 在這裡，我們看到計算機應用程式在未啟用透明效果時，會適當地回到純色：

使用壓克力](images/tailoring-acrylic.png)
![計算機的 ![計算機，並以壓克力回應透明度設定](images/tailoring-acrylic-fallback.png)

不過，針對任何自訂的效果，應用程式必須回應[UISettings 的 AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabled)屬性或[AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)事件，並切換出效果/效果圖形，以使用沒有透明度的效果。 範例如下：

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

同樣地，應用程式應該接聽並回應[UISettings 的 AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled)屬性，以確保動畫會根據 [設定] 中的使用者設定來開啟或關閉，> 輕鬆存取 > 顯示。

![[設定] 中的動畫選項](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>利用功能 API

藉由利用[CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) api，您可以偵測出哪些組合功能可供使用，並在指定的硬體上進行效能調整，並量身打造設計，以確保使用者在任何裝置上都能獲得高效能且美觀的體驗。 Api 提供了一種方法來檢查硬體系統功能，以在各種外型規格中執行正常的效果調整。 這可讓您輕鬆適當地量身打造應用程式，以建立美觀且順暢的使用者體驗。

此 API 提供方法和事件接聽程式，可用來對應用程式 UI 進行調整決策。 此功能會偵測系統如何處理複雜的組合和轉譯作業，然後將資訊傳回給開發人員使用的簡單易用模型。

### <a name="using-composition-capabilities"></a>使用組合功能

CompositionCapabilities 功能已用於壓克力材質之類的功能，其中的材質會根據案例和硬體而回復為更具效能的效果。

您可以使用幾個簡單的步驟，將 API 新增至現有的程式碼。

1. 取得應用程式的函式中的功能物件。

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 為您的應用程式註冊功能已變更的事件接聽程式。

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 將內容新增至事件回呼方法，以處理各種功能層級。 這不一定會與下面的下一個步驟類似。
1. 使用效果時，請先檢查功能物件。 請考慮使用條件式檢查或 switch 控制語句，視您要如何量身打造效果而定。

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

您可以在[WINDOWS UI Github](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities)存放庫中找到完整的範例程式碼。

## <a name="fast-vs-slow-effects"></a>快速和緩慢效果

根據 CompositionCapabilities API 中提供的[AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported)和[AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast)方法的意見反應，應用程式可以決定針對裝置優化的其他效果，交換昂貴或不受支援的效果。 已知某些影響的資源會比其他效果高，而且應該謹慎使用，而其他效果則可以更自由地使用。 不過，針對所有的效果，當連結和製作動畫時，請小心使用，因為某些案例或組合可能會變更效果圖形的效能特性。 以下是個別效果的效能特性的一些經驗法則：

- 已知具有高效能影響的效果，如下所示–高斯模糊、陰影遮罩、BackDropBrush、HostBackDropBrush 和圖層視覺效果。 這不建議用於低端裝置[（功能層級 9.1-9.3）](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)，而且應該在高階裝置上謹慎使用。
- 影響效能的影響包括色彩矩陣、特定 Blend 效果 BlendModes （亮度、色彩、飽和度和色調）、焦點、SceneLightingEffect 和（視案例而定） BorderEffect。 這些效果可能適用于低端裝置上的特定案例，但請小心在連結和製作動畫時使用。 建議將使用限制為僅限兩個或更少，並在轉換時製作動畫。
- 所有其他的效果都會對效能造成影響，並在製作動畫和連結時于所有合理的情況下工作。

## <a name="related-articles"></a>相關文章

- [UWP 回應式設計技術](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP 裝置量身打造](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
