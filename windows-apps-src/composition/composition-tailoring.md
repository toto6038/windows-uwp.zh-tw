---
title: 量身訂做的組合
description: 您可以使用 撰寫 Api 來量身打造您的 UI、 使效能最佳化，並配合使用者設定和裝置特性。
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bcc9a6d89a143d8fd03d73dbd83b832ed9513ee2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644413"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>量身訂做的效果與使用 Windows UI 體驗

Windows UI 會提供許多的美麗的效果、 動畫和表示差異化。 不過，滿足使用者期望的效能和自訂能力仍建立成功的應用程式不可或缺的一部分。 通用 Windows 平台支援大型、 不同系列的裝置，而有不同的特性與功能。 為了提供您的所有使用者 （含） 的經驗，您需要確保您的應用程式擴展跨裝置遵守使用者喜好設定。 量身訂做的 UI 可以提供有效率的方式，來運用裝置的功能，並確保愉快且內含使用者體驗。

量身訂做的 UI 是廣泛的類別，包含適用於高效能，美麗的 UI，相對於下列區域：

- 尊重與適應之效果的使用者設定
- 配合動畫的使用者設定
- 最佳化 UI 特定的硬體功能

在這裡，我們將討論如何調整您的效果和動畫，與上述區域中視覺化的圖層，但有許多其他方式來調整您的應用程式，以確保絕佳的使用者體驗。 Docs 指引是有提供關於如何[量身訂做您的 UI](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)針對各種裝置並[建立回應式 UI](/windows/uwp/design/layout/responsive-design)。

## <a name="user-effects-settings"></a>使用者效果設定

使用者可以自訂各種不同的原因，而應用程式應該尊重，並適應其 Windows 體驗。 使用者可以控制的其中一個區域會變更的作用他們會看到系統使用的類型。

### <a name="transparency-effects-settings"></a>投影片效果設定

一個使用者可以自訂這類效果設定為開啟/關閉透明效果。 這可在個人化設定應用程式 > 色彩，或透過 [設定] 應用程式 > 輕鬆存取 > 顯示。

![設定中的 透明度選項](images/tailoring-transparency-setting.png)

開啟時，任何使用透明效果的效果會出現如預期般運作。 這適用於 Acrylic、 HostBackdropBrush 或任何自訂效果圖形不是完全不透明。

關閉時，壓克力資料會自動切換回純色，因為 XAML 的壓克力筆刷所預設具有接聽此事件。 在這裡，我們會看到 [小算盤] 應用程式適當地切換回純色未啟用透明效果時：

![計算機與 Acrylic](images/tailoring-acrylic.png)
![Acrylic 回應透明度設定的計算機](images/tailoring-acrylic-fallback.png)

不過，針對任何自訂會影響應用程式必須回應[UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)屬性或[AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)事件和 switch out 影響/影響若要使用具有不透明效果的圖形。 這個範例如下：

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

同樣地，應用程式應該接聽和回應[UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled)以確保動畫的屬性開啟或關閉根據設定中的使用者設定 > 輕鬆存取 > 顯示。

![設定中的 [動畫] 選項](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>利用 API 的能力

利用[CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) Api，您可以偵測哪些組合上的功能都可用，且高效能指定的硬體和量身訂做設計，以確保使用者會在任何取得 「 高效能和美觀的體驗裝置。 Api 會提供方法來檢查硬體系統功能，才能實作依正常程序在各種外型規格調整的效果。 這比較容易適當地調整建立美觀的應用程式和無縫式的使用者體驗。

此 API 會提供方法和事件接聽程式，可用來進行調整的應用程式 UI 的決策的影響。 此功能會偵測程度系統可以處理複雜的組合和轉譯作業，並接著開發人員利用簡單取用模型中傳回的資訊。

### <a name="using-composition-capabilities"></a>使用組合功能

壓克力材料，其中資料會回復到更多的高效能生效，取決於案例和硬體等功能已運用了 CompositionCapabilities 功能。

API 可以將現有的程式碼，以幾個簡單的步驟。

1. 取得您的應用程式建構函式中的功能物件。

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 註冊您的應用程式的功能已變更的事件接聽程式。

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 將內容新增至事件的回呼方法，以處理不同的功能層級。 這可能會或可能不會類似下列的下一個步驟。
1. 使用時效果，請先檢查的功能物件。 請考慮使用條件式檢查，或切換控制陳述式，根據您要自訂效果的方式。

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

完整的範例程式碼都位於[Windows UI Github 儲存機制](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities)。

## <a name="fast-vs-slow-effects"></a>快速與緩慢的效果

根據從提供的意見反應[AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported)並[AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) CompositionCapabilities API 中的方法，應用程式可以決定要交換的昂貴或不受支援的效果他們選擇其他效果專為裝置進行最佳化。 已知是較大量的資源比其他部份效果，以及應該謹慎使用，並可以更自由地使用其他效果。 所有的效果，不過，照護應該時使用鏈結，而且在某些情況下為組合以動畫顯示可能會變更影響圖形的效能特性。 以下是一些經驗法則效能特性之個別的效果：

- 已知有高的效能影響的效果如下所示-高斯模糊，陰影的遮罩、 BackDropBrush、 HostBackDropBrush，和圖層視覺化。 這些不建議使用低階裝置[（功能層級 9.1 9.3）](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)，並應該謹慎使用高階裝置上。
- 與中度的效能影響的影響包括色彩矩陣，（亮度、 色彩、 飽和度和 Hue） 特定 Blend 效果 BlendModes 焦點、 SceneLightingEffect，和 （視案例而定） BorderEffect。 這些效果可能低結束裝置上時使用特定案例，但鏈結和製作動畫時，就應該使用照護。 建議您限制使用兩個或更少，並以動畫顯示轉換只。
- 所有其他的效果很低的效能的影響，而且在所有的合理案例時建立動畫，並將鏈結中運作。

## <a name="related-articles"></a>相關文章

- [UWP 的回應式設計技術](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP 裝置量身訂做](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
