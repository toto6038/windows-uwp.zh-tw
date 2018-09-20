---
author: daneuber
title: 量身訂做的組合
description: 使用組合 Api 量身打造您的 UI、 效能、 最佳化及容納使用者設定和裝置特性。
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 66384c4df3195ae0fff35ae5dd7e1b1983204068
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "4085997"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>量身訂做的效果與使用 Windows UI 體驗

Windows UI 提供許多的美觀效果、 動畫以及表示區分。 不過，符合使用者的期望的效能和自訂仍然是建立成功的應用程式的必要部分。 通用 Windows 平台支援大型的各種不同裝置系列的這有不同的特色和功能。 若要為所有使用者提供的全人的體驗，您需要確保您的應用程式的縮放比例跨裝置尊重使用者的喜好設定。 量身訂做的 UI 可以提供有效率的方式來利用裝置的功能，並確保令人容易接受以及全人使用者體驗。

量身訂做的 UI 是一種廣泛類別，包含適用於高效能，美觀的 UI，根據下列幾個方面：

- 遵守以及適應效果的使用者設定
- 協調動畫的使用者設定
- 最佳化特定的硬體功能的 UI

在這裡，我們會討論如何量身打造您的效果和動畫具有視覺層中上述的區域，但有許多其他方法來量身打造您的應用程式，以確保有絕佳使用者體驗。 提供有關如何針對各種裝置和[建立回應式 UI](/design/layout/responsive-design.md)的 [[量身打造您的 UI](/design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)指導方針文件。

## <a name="user-effects-settings"></a>使用者效果設定

使用者可能會自訂他們的 Windows 體驗的各種不同的理由，這應該在尊重及適應應用程式。 使用者可以控制的一個區域已變更效果他們看到用在他們的系統的類型。

### <a name="transparency-effects-settings"></a>透明度效果設定

一個這類使用者可自訂的效果設定為開啟/關閉透明度效果。 這可以在個人化設定應用程式中找到 > 色彩，或透過設定應用程式 > [輕鬆存取 > 顯示。

![在設定中的透明度選項](images/tailoring-transparency-setting.png)

開啟時，任何使用透明度的效果會如預期般運作。 這適用於壓克力風格、 HostBackdropBrush 或任何不是完全不透明的自訂效果圖形。

關閉時，壓克力材質會自動回復為純色因為 XAML 的壓克力筆刷根據預設，有接聽此事件。 在這裡，我們看到小算盤應用程式適當地後援的純色時不會啟用透明度效果：

![以壓克力的 [小算盤]](images/tailoring-acrylic.png)
![[小算盤] 以回應透明度設定的壓克力](images/tailoring-acrylic-fallback.png)

不過，任何自訂效果的應用程式需要回應[UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)屬性或[AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)事件及切換效果/效果圖形使用不具有任何透明度的效果。 以下是此範例：

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

同樣地，應用程式應接聽和回應[UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled)屬性，以確保動畫是開啟或關閉根據設定中的使用者設定 > [輕鬆存取 > 顯示。

![在設定中的動畫選項](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>利用 API 的功能

利用[CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) Api，您可以偵測到哪些組合功能可以和高效能在指定的硬體，並量身打造設計，以確保使用者在任何裝置上取得高效能和美觀的體驗。 Api 提供方法來檢查硬體系統功能，才能實作一級縮放各種不同的尺寸規格的效果。 這可讓您輕鬆進行適當量身打造的應用程式以建立美觀和無縫的一般使用者體驗。

這個 API 提供方法和事件接聽程式，可以用來進行縮放的應用程式 UI 的決策的效果。 此功能偵測複雜組合和轉譯作業，系統可以處理的程度，並接著利用的開發人員輕鬆取用模型中傳回的資訊。

### <a name="using-composition-capabilities"></a>使用組合功能

CompositionCapabilities 功能是已經被針對運用了功能，例如壓克力材質，其中材質會回復到更多的高效能效果，取決於案例和硬體。

API 可以新增至現有的程式碼，在幾個簡單步驟中。

1. 取得您的應用程式建構函式中的功能物件。

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 註冊您的應用程式的功能已變更的事件接聽程式。

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 將內容新增至事件的回呼方法，以處理各種不同功能層級。 這可能會或可能不是類似下列的下一個步驟。
1. 當使用效果，請先檢查功能物件。 請考慮使用條件式檢查或切換控制陳述式，取決於您想要量身打造效果的方式。

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

[Windows UI Github 存放庫](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities)上可找到完整的範例程式碼。

## <a name="fast-vs-slow-effects"></a>快速與緩慢的效果

根據意見反應提供[AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported)和[AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast)方法中 CompositionCapabilties API，應用程式可以決定能為他們選擇已進行最佳化的其他效果昂貴或不受支援的效果適用於裝置。 某些效果已知為以一致的方式較大量的資源比其他應該謹慎，使用與其他效果可以使用更自由地。 所有的效果，不過，小心應該使用時鏈結，並產生動畫效果做為某些案例或組合可能會變更效果圖形的效能特性。 以下是一些法則的效能特性個別的效果：

- 已知為高效能的影響的效果如下所示 – 高斯模糊、 陰影遮罩、 BackDropBrush、 HostBackDropBrush，和視覺層。 這些不建議使用的低階裝置[（功能層級 9.1 9.3）](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)，並在高階裝置上應該謹慎使用。
- 中型效能影響的效果包含色彩矩陣，特定的混合效果 BlendModes （亮度、 色彩、 飽和，以及色調） 焦點、 SceneLightingEffect，以及 （取決於案例） BorderEffect。 這些效果可能與某些案例低階裝置上運作，但鏈結，並產生動畫效果時，應該使用小心。 建議限制為兩個或更少的使用，並建立只轉換動畫。
- 所有其他效果有低效能的影響，且在所有合理的案例時產生動畫效果和鏈結中運作。

## <a name="related-articles"></a>相關文章

- [UWP 回應式設計技術](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP 裝置量身訂做](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
