---
Description: 使用視覺效果意見反應，在偵測、解釋和處理與 Windows 應用程式的互動時，顯示使用者。
title: 視覺化回饋
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: 視覺化回饋、焦點回饋、觸控回饋、觸控點視覺效果、輸入、互動
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1afc1c884a7a01ef1021f37476d1e29430c62e3c
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219831"
---
# <a name="guidelines-for-visual-feedback"></a>視覺化回饋的指導方針

使用視覺化回饋以向使用者顯示系統已偵測到、解譯及處理他們的互動。 視覺化回饋可以透過激發互動意願來協助使用者。 它會指出互動是否成功來改善使用者的控制感應。 它也會轉送系統狀態並減少錯誤。

> **重要 API**：[**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)、[**Windows.UI.Input**](/uwp/api/Windows.UI.Input)、[**Windows.UI.Core**](/uwp/api/Windows.UI.Core)

## <a name="recommendations"></a>建議

- 嘗試限制修改這些與您的設計目的直接相關的控制項範本，不然的話，多出來的變更會影響控制項和應用程式的效能與可存取性。 
    - 請參閱 [XAML 樣式](../controls-and-patterns/xaml-styles.md)以取得有關自訂控制項屬性的詳細資訊，包括可見狀態屬性。
    - 請參閱 [UserControl 類別](/uwp/api/windows.ui.xaml.controls.usercontrol)以取得有關變更控制項範本的詳細資訊
    - 如果您需要大幅變更控制項範本，請考慮建立自訂範本化控制項。 如需自訂範本化控制項的範例，請參閱[自訂編輯控制項範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)。
- 如果觸控視覺效果可能干擾應用程式的使用，就不要使用觸控視覺效果。 如需詳細資訊，請參閱 [**ShowGestureFeedback**](/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback)。
- 不要在非必要情況下顯示回饋。 除非您是要藉由顯示視覺化回饋來添加其他地方所無法提供的好處，否則請勿顯示視覺化回饋，以便讓 UI 保持簡潔、整齊。
- 不要嘗試對內建的 Windows 手勢進行大幅的視覺化回饋行為自訂，因為這可能會導致產生不一致和混淆的使用者體驗。

## <a name="additional-usage-guidance"></a>其他用法指導方針

接觸點視覺效果對於要求準確度和精確度的觸控互動相當重要。 例如，您的應用程式應該清楚地指示點選位置，讓使用者在未命中目標時能夠知到未命中、誤差有多少，以及必須要做什麼調整。

使用可用的預設 XAML 平台控制項，可確保您的應用程式在所有裝置上及在所有輸入情況下都能正確運作。 如果您的應用程式包含需要自訂回饋的自訂互動，那麼您應當確保回饋適當、可跨輸入裝置，而且不會讓使用者從工作上分心。 在遊戲或繪圖應用程式中這可能成為特別的問，視覺化回饋有可能與重要 UI 相衝突或遮蓋到重要 UI。

> [!Important]
> 建議您不要變更內建手勢的互動行為。

**跨裝置回饋**

視覺化回饋通常取決於輸入裝置 (觸控、觸控板、滑鼠、畫筆/手寫筆、鍵盤等等)。 例如，內建的滑鼠回饋通常涉及移動和變更游標，觸控和手寫筆需要的是接觸點視覺效果，而鍵盤輸入和瀏覽則是使用焦點矩形和醒目提示。

使用 [**ShowGestureFeedback**](/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback) 設定平台手勢的回饋行為。

如果自訂回饋 UI，請確定您提供支援且適合所有輸入模式的回饋。

以下是 Windows 中一些內建的接觸點視覺效果範例。

| ![觸控回饋](images/TouchFeedback.png) | ![滑鼠回饋](images/MouseFeedback.png) | ![手寫筆回饋](images/PenFeedback.png) | ![鍵盤回饋](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| 觸控視覺效果 | 滑鼠/觸控板視覺效果 | 手寫筆視覺效果 | 鍵盤視覺效果 |

## <a name="high-visibility-focus-visuals"></a>高可見度焦點視覺效果

所有 Windows 應用程式都支援在應用程式內的可互動控制項周圍使用更詳細定義的焦點視覺效果。 這些新的焦點視覺效果都是完全可自訂的，而且也可視需要予以停用。

針對 Xbox 和電視典型的 **10 英呎體驗**，Windows 支援**顯色焦點**，當透過遊樂器或鍵盤輸入聚焦時，這個光源效果會動畫顯示可聚焦的元素邊框，例如按鈕。

## <a name="color-branding--customizing"></a>色彩商標和自訂

### <a name="border-properties"></a>框線屬性

高可見度焦點視覺效果有兩個部分︰主要框線和次要框線。 主要框線的粗細為 **2px**，圍繞在次要框線「外」  。 次要框線的粗細為 **1px**，圍繞在主要框線「內」  。
![高可見度焦點視覺效果紅線](images/FocusRectRedlines.png)

若要變更框線類型 (主要或次要) 的粗細，請分別使用 **FocusVisualPrimaryThickness** 或 **FocusVisualSecondaryThickness**︰
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![高可見度焦點視覺效果邊界粗細](images/FocusMargin.png)

邊界是 [**Thickness**](/dotnet/api/system.windows.thickness) 類型的屬性，因此可將邊界自訂成只出現在控制項的特定邊。 請參閱以下內容： ![ 高可見度焦點視覺邊界粗細僅靠下](images/FocusThicknessSide.png)

邊界是控制項的視覺界限和焦點視覺效果 *次要框線*的起點之間的空間。 預設邊界會 **1px** 離開控制項界限。 您可以藉由變更 **FocusVisualMargin** 屬性，針對每個控制項來編輯此邊界：
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![高可見度焦點視覺效果邊界差異](images/FocusPlusMinusMargin.png)

*邊界為負值時，會將框線推離控制項的中心，邊界為正值時，則會將框線向控制項的中心靠攏。*

若要將控制項上的焦點視覺效果完全關閉，只要停用 **UseSystemFocusVisuals** 即可：
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

粗細、邊界或應用程式開發人員是否想要使用焦點視覺效果是在個別控制項上決定。

### <a name="color-properties"></a>色彩屬性

焦點視覺效果只有兩個色彩屬性︰主要框線色彩和次要框線色彩。 您可以在頁面層級上變更個別控制項的這些焦點視覺效果框線色彩，以及在全應用程式層級上全域變更這些色彩︰

若要設計全應用程式的焦點視覺效果商標，請覆寫系統筆刷：
```XAML
<SolidColorBrush x:Key="SystemControlFocusVisualPrimaryBrush" Color="DarkRed"/>
<SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="Pink"/>
```
![高可見度焦點視覺效果色彩變更](images/FocusRectColorChanges.png)

若要變更個別控制項上的色彩，只要編輯所需控制項上的焦點視覺效果屬性即可︰
```XAML
<Slider Width="200" FocusVisualPrimaryBrush="DarkRed" FocusVisualSecondaryBrush="Pink"/>
```

## <a name="related-articles"></a>相關文章

### <a name="for-designers"></a>適用於設計人員

- [移動瀏覽的指導方針](guidelines-for-panning.md)

### <a name="for-developers"></a>開發人員

- [自訂使用者互動](../layout/index.md)

### <a name="samples"></a>範例

- [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

### <a name="archive-samples"></a>封存範例

- [輸入：XAML 使用者輸入事件範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [輸入：裝置功能範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [輸入：觸控點擊測試範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 滾動、移動流覽和縮放範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [輸入：簡化的筆跡範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [輸入：Windows 8 手勢範例](/samples/browse/?redirectedfrom=MSDN-samples)
- [輸入：操作和手勢範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX 觸控輸入範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
 

 
