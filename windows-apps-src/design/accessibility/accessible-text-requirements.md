---
author: Xansky
Description: This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: 協助工具文字的需求
label: Accessible text requirements
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 39979ed3fa2fcd85cbf1f1b73d7c37b2dce38f20
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5986683"
---
# <a name="accessible-text-requirements"></a>協助工具文字的需求  




本主題描述 App 中文字的協助工具最佳做法，方法是確保色彩和背景能夠滿足必要的對比率。 本主題也討論在通用 Windows 平台 (UWP) App 中文字元素可以擁有的 Microsoft 使用者介面自動化角色，以及圖形中文字的最佳做法。

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>對比率  
雖然使用者一直都可以選擇切換至高對比模式，但是您 App 對文字的設計應該將該選項視為最後的手段。 更好的做法是確定 App 文字符合專門為文字與背景間的對比層級所制定的指導方針。 對比層級的評估是根據各種不考慮色調的決定性技術。 例如，如果是綠色背景上的紅色文字，那麼患有色盲的使用者可能就看不清楚文字。 檢查並更正對比率可以防止這些類型的協助工具問題。

此處記載的文字對比建議是以網頁協助工具標準為基礎，[G18：確定文字 (和文字的圖像) 和文字背景之間的對比率至少是 4.5:1](http://go.microsoft.com/fwlink/p/?linkid=221823)。 此指導方針位於 *WCAG 2.0 的 W3C 技術*規格中。

為了提供無障礙功能，顯示的文字與背景的亮度對比率至少必須是 4.5:1。 例外的情況包括標誌以及不重要的文字，例如非作用中 UI 元素的文字。

也排除修飾性文字與沒有傳達任何意義的文字。 例如，如果使用隨機文字來建立背景，而且文字可以重新排列或替換，而不會破壞含意，則此類的文字視為修飾性文字，可以不用符合這項規定。

使用色彩對比工具確定可見文字的對比率是否可被接受。 請參閱 [WCAG 2.0 的技術 G18 (資源一節)](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources) (英文)，了解可以測試對比率的工具。

> [!NOTE]
> 《WCAG 2.0 的技術》中 G18 列出的一些工具不能與 UWP App 互動使用。 您可能需要手動在工具中輸入前景和背景的色彩值，或擷取 App UI 的螢幕畫面，然後針對螢幕擷取的影像執行對比率工具。

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>文字元素角色  
UWP App 可以使用這些預設元素 (一般稱為「文字元素」** 或「文字編輯控制項」**)：

* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)：角色是 [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)：角色是 [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) (以及 overflow 類別[**RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.richtextblockoverflow))：角色是 [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/BR227548)：角色是 [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182)

當控制項報告它有一個 [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182) 角色，輔助技術會假設使用者有各種方法可變更這些值。 因此，如果您將靜態文字放到 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 中，就會報告錯誤的角色，也向協助工具使用者報告錯誤的應用程式結構。

在適用於 XAML 的文字模型中，主要用於靜態文字的元素有兩個：[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)。 這兩個元素都不能是 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 子類別，因此它們都無法取得鍵盤焦點，或是以 Tab 順序顯示。 但是，這不表示輔助技術無法或不會讀取它們。 螢幕助讀程式通常設計成可支援多種模式以閱讀應用程式中的內容，包含超越焦點與 Tab 順序的專用閱讀模式或瀏覽模式，例如「虛擬游標」。 因此，請不要只是因為定位順序能將使用者帶至可設定焦點的容器，就將您的靜態文字放入該處。 輔助技術使用者預期定位順序中的每個項目都能進行互動，而如果他們在該處遇到靜態文字，則所帶來的混亂就會大過助益。 您應自行使用朗讀程式測試這一點，以了解透過螢幕助讀程式檢閱您 App 中靜態文字的使用者體驗。

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>自動建議的協助工具  
當使用者在輸入欄位輸入，且顯示可能建議的清單時，這類案例稱為自動建議。 這常見於電子郵件的 **\[收件者\]** 欄位、Windows 中的 Cortana 搜尋方塊、Microsoft Edge 的 URL 輸入欄位、「天氣」App 的位置輸入欄位等位置。 如果您是使用 XAML [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox) 或 HTML 內建控制項，則系統已經為您預設好這個體驗。 為了讓這個體驗無障礙，輸入欄位和清單必須相關聯。 在[實作自動建議](#implementing_auto-suggest)一節中有說明。

朗讀程式已經更新，讓這個體驗能透過特殊的建議模式成為無障礙類型體驗。 整體來說，適當地連結編輯欄位與清單之後，使用者將能：

* 在清單出現及清單關閉的時候知道
* 知道有多少建議可用
* 知道已選取的項目 (如果有)
* 能將朗讀程式焦點移動到清單
* 以其他閱讀模式瀏覽建議

![建議清單](images/autosuggest-list.png)<br/>
_建議清單的範例_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>實作自動建議  
為了讓這個體驗無障礙，輸入欄位和清單必須在 UIA 樹狀結構中相關聯。 在傳統型應用程式和 UWP App 中，此關聯分別是以 [UIA_ControllerForPropertyId](https://msdn.microsoft.com/windows/desktop/ee684017) 屬性和 [ControlledPeers](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 屬性建立。

自動建議體驗大致上有 2 種類型。

**預設選取**  
如果清單中有預設選取的項目，在傳統型應用程式中，朗讀程式會尋找 [**UIA_SelectionItem_ElementSelectedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223) 事件，在 UWP app 中則會尋找要引發的 [**AutomationEvents.SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) 事件。 每當選取的項目變更 (使用者輸入其他字母而使建議項目更新，或使用者瀏覽清單)，就會引發 **ElementSelected** 事件。

![包含預設選取項目的清單](images/autosuggest-default-selection.png)<br/>
_有預設選取項目的範例_

**沒有預設選取**  
如果沒有預設選取的項目 (如「天氣」App 的位置方塊)，則每當清單更新時，朗讀程式就會在清單上尋找要引發的傳統型 [**UIA_LayoutInvalidatedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223 ) 事件，或 UWP [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) 事件。

![不含預設選取項目的清單](images/autosuggest-no-default-selection.png)<br/>
_沒有預設選取項目的範例_

### <a name="xaml-implementation"></a>XAML 實作  
如果您是使用預設的 XAML [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox)，則一切都已經為您設定好。 如果您是使用 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox) 和清單建立自己的自動建議體驗，則您需要在 **TextBox** 上將清單設為 [**AutomationProperties.ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers)。 每當新增或移除這個屬性時，您必須針對 [**ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 屬性引發 **AutomationPropertyChanged** 事件，同時也要引發您自己的 [**SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) 事件或 [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) 事件，如本文稍早前所說明，這必須視您案例的類型而定。

### <a name="html-implementation"></a>HTML 實作  
如果您是使用 HTML 中的內建控制項，則已經為您對應 UIA實作。 以下是已經為您設定好的實作範例：

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 如果要建立您自己的控制項，您必須設定自己的 ARIA 控制項，這在 W3C 標準中有說明。

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>圖形中的文字  
請儘可能不要在圖形中加上文字。 例如，在影像來源檔案中包含的任何文字，會在 app 中顯示成 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 元素，那麼輔助技術就無法自動存取或判讀。 如果您必須在圖形中使用文字，請確定您提供的 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 值 (等同於「替代文字」)，包含文字或文字意義的摘要。 如果您從 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 的向量建立文字字元，或者利用 [**Glyphs**](https://msdn.microsoft.com/library/windows/apps/BR209921) 建立文字字元，也需要考慮類似的因素。

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size"></a>文字字型大小  
如果應用程式中的文字使用太小的字型，則會造成許多讀者難以閱讀這些文字。 您可以一開始就在應用程式的 UI 中將文字設定為合理的大小，以避免這個問題。 Windows 中還有其他輔助技術，而這些技術通常能夠讓使用者變更應用程式或顯示器的檢視大小。

* 某些使用者會變更主要顯示器的 DPI 值來做為其無障礙輔助。 該選項可以在 **\[輕鬆存取\]** 的 **\[讓螢幕上的內容更大一些\]** 中變更，這會重新導向到 **\[外觀及個人化\]** / **\[顯示\]** 的 **\[控制台\]** UI。 確切有哪些大小設定選項可供使用，則會根據顯示裝置的功能而有所不同。
* 放大鏡工具可以放大 UI 的選取區域。 不過，很難使用放大鏡工具來閱讀文字。

<span id="Text_scale_factor"/>
<span id="text_scale_factor"/>
<span id="TEXT_SCALE_FACTOR"/>

## <a name="text-scale-factor"></a>文字縮放比例  
各種文字元素和控制項都有 [**IsTextScaleFactorEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.istextscalefactorenabled) 屬性。 這個屬性的預設值是 **true** 。 當其值為 **true** 時，手機上的一個名為 **\[文字大小調整\]** 的設定 (**\[設定\] &gt; \[輕鬆存取\]**) 就會放大該元素中的文字。 這項調整對小型 **FontSize** 文字的影響程度，遠大於對大型 **FontSize** 文字的影響程度。 不過您可以將元素的 **IsTextScaleFactorEnabled** 屬性設為 **false**，以停用自動放大。 嘗試用這個標記來調整手機的 **\[文字大小\]** 設定，然後看看 **TextBlock** 會發生什麼狀況：

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

不過，請勿經常停用自動放大，因為在所有 app 上全域縮放 UI 文字，對於使用者而言是一個重要的無障礙體驗，他們將預期這個體驗也可以在您的 app 中運作。

您也可以使用 [**TextScaleFactorChanged**](https://msdn.microsoft.com/library/windows/apps/Dn633867) 事件與 [**TextScaleFactor**](https://msdn.microsoft.com/library/windows/apps/Dn633866) 屬性，了解手機上 **\[文字大小\]** 設定的變更相關資訊。 方法如下：

C#
```csharp
{
    ...
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    uiSettings.TextScaleFactorChanged += UISettings_TextScaleFactorChanged;
    ...
}

private async void UISettings_TextScaleFactorChanged(Windows.UI.ViewManagement.UISettings sender, object args)
{
    var messageDialog = new Windows.UI.Popups.MessageDialog(string.Format("It's now {0}", sender.TextScaleFactor), "The text scale factor has changed");
    await messageDialog.ShowAsync();
}
```

**TextScaleFactor** 的值是範圍 \[1,2\] 的兩倍。 最小的文字依照此量放大。 舉例來說，您可以使用這個值來調整圖形以符合文字。 但是請記住，並非所有文字都會以相同比例縮放。 一般說來，較大的文字比較不容易受到縮放所影響。

這些類型都有 **IsTextScaleFactorEnabled** 屬性：  
* [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378)
* [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 和衍生類別
* [**FontIcon**](https://msdn.microsoft.com/library/windows/apps/Dn279514)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)
* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)
* [**TextElement**](https://msdn.microsoft.com/library/windows/apps/BR209967) 和衍生類別

<span id="related_topics"/>

## <a name="related-topics"></a>相關主題  
* [協助工具](accessibility.md)
* [基本協助工具資訊](basic-accessibility-information.md)
* [XAML 文字顯示範例](http://go.microsoft.com/fwlink/p/?linkid=238579)
* [XAML 文字編輯範例](http://go.microsoft.com/fwlink/p/?linkid=251417)
* [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570) 
