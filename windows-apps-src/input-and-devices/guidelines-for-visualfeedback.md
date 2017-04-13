---
author: Karl-Bridge-Microsoft
Description: "使用視覺化回饋以向使用者顯示系統已偵測到、解譯及處理他們與 Windows 市集應用程式的互動。"
title: "視覺化回饋"
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: "視覺化回饋、焦點回饋、觸控回饋、觸控點視覺效果、輸入、互動"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 422d396e9810d6ebe083a6346c8ec8f032ef1fb3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="guidelines-for-visual-feedback"></a>視覺化回饋的指導方針
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

使用視覺化回饋以向使用者顯示系統已偵測到、解譯及處理他們的互動。 視覺化回饋可以透過激發互動意願來協助使用者。 它會指出互動是否成功來改善使用者的控制感應。 它也會轉送系統狀態並減少錯誤。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)</li>
<li>[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)</li>
<li>[**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)</li>
</ul>
</div>

## <a name="recommendations"></a>建議

-   儘可能嘗試與原始控制項範本保持接近，以獲得最佳的控制項及應用程式效能。
-   如果觸控視覺效果可能干擾應用程式的使用，就不要使用觸控視覺效果。 如需詳細資訊，請參閱 [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969)。
-   不要在非必要情況下顯示回饋。 除非您是要藉由顯示視覺化回饋來添加其他地方所無法提供的好處，否則請勿顯示視覺化回饋，以便讓 UI 保持簡潔、整齊。
-   不要嘗試對內建的 Windows 手勢進行大幅的視覺化回饋行為自訂，因為這可能會導致產生不一致和混淆的使用者體驗。

## <a name="additional-usage-guidance"></a>其他用法指導方針

接觸點視覺效果對於要求準確度和精確度的觸控互動相當重要。 例如，您的應用程式應該清楚地指示點選位置，讓使用者在未命中目標時能夠知到未命中、誤差有多少，以及必須要做什麼調整。

使用可用的預設 XAML 平台控制項，可確保您的應用程式在所有裝置上及在所有輸入情況下都能正確運作。 如果您的應用程式包含需要自訂回饋的自訂互動，那麼您應當確保回饋適當、可跨輸入裝置，而且不會讓使用者從工作上分心。 在遊戲或繪圖應用程式中這可能成為特別的問，視覺化回饋有可能與重要 UI 相衝突或遮蓋到重要 UI。

[!IMPORTANT] 建議您不要變更內建手勢的互動行為。 

**跨裝置回饋**

視覺化回饋通常取決於輸入裝置 (觸控、觸控板、滑鼠、畫筆/手寫筆、鍵盤等等)。 例如，內建的滑鼠回饋通常涉及移動和變更游標，觸控和手寫筆需要的是接觸點視覺效果，而鍵盤輸入和瀏覽則是使用焦點矩形和醒目提示。

使用 [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969) 設定平台手勢的回饋行為。

如果自訂回饋 UI，請確定您提供支援且適合所有輸入模式的回饋。

以下是 Windows 中一些內建的接觸點視覺效果範例。

| ![觸控回饋](images/TouchFeedback.png) | ![滑鼠回饋](images/MouseFeedback.png) | ![手寫筆回饋](images/PenFeedback.png) | ![鍵盤回饋](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| 觸控視覺效果 | 滑鼠/觸控板視覺效果 | 手寫筆視覺效果 | 鍵盤視覺效果 |

## <a name="high-visibility-focus-visuals"></a>高可見度焦點視覺效果

所有 Windows 應用程式都支援在應用程式內的可互動控制項周圍使用更詳細定義的焦點視覺效果。 這些新的焦點視覺效果都是完全可自訂的，而且也可視需要予以停用。

## <a name="color-branding--customizing"></a>色彩商標和自訂

**框線屬性**

高可見度焦點視覺效果有兩個部分︰主要框線和次要框線。 主要框線的粗細為 **2px**，圍繞在次要框線*「外」*。 次要框線的粗細為 **1px**，圍繞在主要框線*「內」*。
![高可見度焦點視覺效果紅線](images/FocusRectRedlines.png)

若要變更框線類型 (主要或次要) 的粗細，請分別使用 **FocusVisualPrimaryThickness** 或 **FocusVisualSecondaryThickness**︰
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![高可見度焦點視覺效果邊界粗細](images/FocusMargin.png)

邊界是 [**Thickness**](https://msdn.microsoft.com/library/system.windows.thickness) 類型的屬性，因此可將邊界自訂成只出現在控制項的特定邊。 參見下方：![高可見度焦點視覺效果邊界粗細 (僅限底部)](images/FocusThicknessSide.png)

邊界是控制項視覺界限與焦點視覺效果*「次要框線」*起始位置之間的空間。 預設邊界是距離控制項界限 **1px**。 您可以編輯個別控制項的這個邊界，方法是變更 **FocusVisualMargin** 屬性︰
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

**色彩屬性**

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

**適用於設計人員**
* [移動瀏覽的指導方針](guidelines-for-panning.md)

**適用於開發人員**
* [自訂使用者互動](https://msdn.microsoft.com/library/windows/apps/mt185599)

**範例**
* [基本輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [使用者互動模式範例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**封存範例**
* [輸入：XAML 使用者輸入事件範例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [輸入：觸控點擊測試範例](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 捲動、移動瀏覽和縮放範例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [輸入：簡化的筆跡範例](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [輸入：Windows 8 手勢範例](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [輸入：操作和手勢 (C++) 範例](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 觸控輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 
