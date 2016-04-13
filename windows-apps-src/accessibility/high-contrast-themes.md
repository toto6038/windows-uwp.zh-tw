---
Description: 描述使用高對比佈景主題時，確保通用 Windows 平台 (UWP) app 可以正常運作的步驟。
title: 高對比佈景主題
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
label: High-contrast themes
template: detail.hbs
---

高對比佈景主題
=============================================================================

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


描述使用高對比佈景主題時，確保通用 Windows 平台 (UWP) app 可以正常運作的步驟。

根據預設，UWP App 可支援高對比佈景主題。 如果使用者已選擇要讓系統使用來自系統設定或協助工具的高對比佈景主題，架構就會自動針對 UI 中的控制項和元件，使用產生高對比配置和呈現方式的色彩和樣式設定。

這項預設支援所根據的是使用預設佈景主題和範本。 這些佈景主題和範本會將系統色彩當作資源定義來參考，當系統使用高對比模式時，資源來源就會自動變更。 不過，如果您的控制項使用自訂範本、佈景主題以及樣式，請小心，不要停用內建的高對比支援。 如果設定樣式時使用了其中一個適用於 Microsoft Visual Studio 的 XAML 設計工具，只要您定義的範本與預設範本有明顯的差異，此設計工具除了產生主要的佈景主題之外，還會另外產生高對比佈景主題。 獨立的佈景主題字典會放到 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807) 集合，它是 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 元素的專用屬性。

如需佈景主題與控制項範本的詳細資訊，請參閱[快速入門：控制項範本](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374)。 查看特定控制項的 XAML 資源字典及佈景主題，並了解佈景主題的建構方式，以及它們是如何參考針對每個可能的高對比設定類似卻又不同的資源，通常是非常有參考性的。

<span id="Detecting_when_a_high-contrast_theme_is_enabled"> </span> <span id="detecting_when_a_high-contrast_theme_is_enabled"> </span> <span id="DETECTING_WHEN_A_HIGH-CONTRAST_THEME_IS_ENABLED"> </span>偵測何時啟用高對比佈景主題
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

UWP app 可以使用 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 類別的成員，偵測高對比佈景主題目前的設定。 [
            **HighContrast**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrast) 屬性會判斷目前是否選取高對比佈景主題。 如果將 **HighContrast** 設定為 **true**，則下一步是檢查 [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrastscheme) 屬性的值，以取得使用的高對比佈景主題名稱。 「白底黑字」和「黑底白字」一般是程式碼應該回應的 **HighContrastScheme** 值。 XAML 定義的 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 索引鍵不可包含空格，因此在資源字典中，這些佈景主題的索引鍵通常是「HighContrastWhite」和 「HighContrastBlack」。 您也應該針對預設的高對比佈景主題準備後援邏輯，以便在這個值是其他字串時使用。 [XAML 高對比範例](http://go.microsoft.com/fwlink/p/?linkid=254993)會顯示這種情況下的邏輯。

**注意** 請確定您是從 app 已初始化且已經顯示內容的範圍內呼叫 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 建構函式。

 

App 可在執行時切換成使用高對比資源值。 只要在樣式或範本 XAML 中使用 [{ThemeResource} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt185591)來要求資源，就可以達到這個目的。 預設佈景主題 (generic.xaml) 都會使用這項 {ThemeResource} 標記延伸技巧，因此，如果您使用預設控制項佈景主題，就會獲得這項行為。 如果您在自訂範本和樣式中也使用了這項 {ThemeResource} 標記延伸資源技巧，則自訂控制項或自訂控制項樣式也能夠執行這項工作。

<span id="related_topics"> </span>相關主題
-----------------------------------------------

* [協助工具](accessibility.md)
* [UI 對比和設定範例](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML 高對比範例](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)
 

 





<!--HONumber=Mar16_HO3-->


