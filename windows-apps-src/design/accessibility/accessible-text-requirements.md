---
Description: 本主題說明應用程式中文字的協助工具最佳做法，方法是確保色彩和背景能夠滿足必要的對比率。
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: 協助工具文字的需求
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f5b87590736c4875214819f5c60a05edd47b1476
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339518"
---
# <a name="accessible-text-requirements"></a>協助工具文字的需求  




本主題說明應用程式中文字的協助工具最佳做法，方法是確保色彩和背景能夠滿足必要的對比率。 本主題也討論在通用 Windows 平台 (UWP) App 中文字元素可以擁有的 Microsoft 使用者介面自動化角色，以及圖形中文字的最佳做法。

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>對比率  
雖然使用者一直都可以選擇切換至高對比模式，但是您 App 對文字的設計應該將該選項視為最後的手段。 更好的做法是確定 App 文字符合專門為文字與背景間的對比層級所制定的指導方針。 對比層級的評估是根據各種不考慮色調的決定性技術。 例如，如果是綠色背景上的紅色文字，那麼患有色盲的使用者可能就看不清楚文字。 檢查並更正對比率可以防止這些類型的協助工具問題。

這裡記載的文字對比建議是以 web 協助工具標準為基礎，@no__t 0G18：確保文字（和文字影像）與文字 @ no__t-0 背後的背景之間，至少有4.5：1的對比比例。 此指導方針位於 *WCAG 2.0 的 W3C 技術*規格中。

為了提供無障礙功能，顯示的文字與背景的亮度對比率至少必須是 4.5:1。 例外的情況包括標誌以及不重要的文字，例如非作用中 UI 元素的文字。

也排除修飾性文字與沒有傳達任何意義的文字。 例如，如果使用隨機文字來建立背景，而且文字可以重新排列或替換，而不會破壞含意，則此類的文字視為修飾性文字，可以不用符合這項規定。

使用色彩對比工具確定可見文字的對比率是否可被接受。 請參閱 [WCAG 2.0 G18 的技術 (資源小節)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)，了解可以測試對比率的工具。

> [!NOTE]
> 《WCAG 2.0 的技術》中 G18 列出的一些工具不能與 UWP App 互動使用。 您可能需要手動在工具中輸入前景和背景的色彩值，或擷取 App UI 的螢幕畫面，然後針對螢幕擷取的影像執行對比率工具。

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>文字元素角色  
UWP App 可以使用這些預設元素 (一般稱為「文字元素」或「文字編輯控制項」)：

* [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)：角色為[**文字**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)：角色是[**編輯**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) （和溢位類別[**RichTextBlockOverflow**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblockoverflow)）：角色是[**文字**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)：角色是[**編輯**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)

當控制項報告它有一個 [**Edit**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) 角色，輔助技術會假設使用者有各種方法可變更這些值。 因此，如果您將靜態文字放到 [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 中，就會報告錯誤的角色，也向協助工具使用者報告錯誤的應用程式結構。

在適用於 XAML 的文字模型中，主要用於靜態文字的元素有兩個：[**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 和 [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)。 這兩個元素都不能是 [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) 子類別，因此它們都無法取得鍵盤焦點，或是以 Tab 順序顯示。 但是，這不表示輔助技術無法或不會讀取它們。 螢幕助讀程式通常設計成可支援多種模式以閱讀應用程式中的內容，包含超越焦點與 Tab 順序的專用閱讀模式或瀏覽模式，例如「虛擬游標」。 因此，請不要只是因為定位順序能將使用者帶至可設定焦點的容器，就將您的靜態文字放入該處。 輔助技術使用者預期定位順序中的每個項目都能進行互動，而如果他們在該處遇到靜態文字，則所帶來的混亂就會大過助益。 您應自行使用朗讀程式測試這一點，以了解透過螢幕助讀程式檢閱您 App 中靜態文字的使用者體驗。

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>自動建議的協助工具  
當使用者在輸入欄位輸入，且顯示可能建議的清單時，這類案例稱為自動建議。 這常見於電子郵件的「收件者」欄位、Windows 中的 Cortana 搜尋方塊、Microsoft Edge 的 URL 輸入欄位、「天氣」App 的位置輸入欄位等位置。 如果您是使用 XAML [**AutosuggestBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 或 HTML 內建控制項，則系統已經為您預設好這個體驗。 為了讓這個體驗無障礙，輸入欄位和清單必須相關聯。 在[實作自動建議](#implementing_auto-suggest)一節中有說明。

朗讀程式已經更新，讓這個體驗能透過特殊的建議模式成為無障礙類型體驗。 整體來說，適當地連結編輯欄位與清單之後，使用者將能：

* 在清單出現及清單關閉的時候知道
* 知道有多少建議可用
* 知道已選取的項目 (如果有)
* 能將朗讀程式焦點移動到清單
* 以其他閱讀模式瀏覽建議

![Suggestion list @ no__t-1<br/>
_建議清單的範例_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>實作自動建議  
為了讓這個體驗無障礙，輸入欄位和清單必須在 UIA 樹狀結構中相關聯。 在傳統型應用程式和 UWP App 中，此關聯分別是以 [UIA_ControllerForPropertyId](https://msdn.microsoft.com/windows/desktop/ee684017) 屬性和 [ControlledPeers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 屬性建立。

自動建議體驗大致上有 2 種類型。

**預設選項**  
如果清單中有預設選取的項目，在傳統型應用程式中，朗讀程式會尋找 [**UIA_SelectionItem_ElementSelectedEventId**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-event-ids) 事件，在 UWP app 中則會尋找要引發的 [**AutomationEvents.SelectionItemPatternOnElementSelected**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) 事件。 每當選取的項目變更 (使用者輸入其他字母而使建議項目更新，或使用者瀏覽清單)，就會引發 **ElementSelected** 事件。

預設選項 @ no__t-1 的 @no__t 0List<br/>
_預設選取專案的範例_

**沒有預設選項**  
如果沒有預設選取的項目 (如「天氣」App 的位置方塊)，則每當清單更新時，朗讀程式就會在清單上尋找要引發的傳統型 [**UIA_LayoutInvalidatedEventId**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-event-ids) 事件，或 UWP [**LayoutInvalidated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) 事件。

![List，沒有預設選取 @ no__t-1<br/>
_沒有預設選取專案的範例_

### <a name="xaml-implementation"></a>XAML 實作  
如果您是使用預設的 XAML [**AutosuggestBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)，則一切都已經為您設定好。 如果您是使用 [**TextBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox) 和清單建立自己的自動建議體驗，則您需要在 **TextBox** 上將清單設為 [**AutomationProperties.ControlledPeers**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers)。 每當新增或移除這個屬性時，您必須針對 [**ControlledPeers**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) 屬性引發 **AutomationPropertyChanged** 事件，同時也要引發您自己的 [**SelectionItemPatternOnElementSelected**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) 事件或 [**LayoutInvalidated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents) 事件，如本文稍早前所說明，這必須視您案例的類型而定。

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

請儘可能不要在圖形中加上文字。 例如，在影像來源檔案中包含的任何文字，會在 app 中顯示成 [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 元素，那麼輔助技術就無法自動存取或判讀。 如果您必須在圖形中使用文字，請確定您提供的 [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name) 值 (等同於「替代文字」)，包含文字或文字意義的摘要。 如果您從 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 的向量建立文字字元，或者利用 [**Glyphs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Glyphs) 建立文字字元，也需要考慮類似的因素。

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size-and-scale"></a>文字字型大小和尺規

當字型使用的字型太小時，使用者可能難以讀取應用程式中的文字，因此請確定您應用程式中的任何文字都是合理的大小。

當您這麼做之後，Windows 會包含各種協助工具工具和設定，讓使用者可以利用並調整其本身的需求，以及閱讀文字的喜好設定。 它們包括：

* 放大鏡工具可放大 UI 的選取區域。 您應該確定應用程式中的文字版面配置不會使其難以使用放大鏡進行閱讀。
* [設定] 中的全域規模和解析度設定 **-> 系統 > 顯示 > 縮放比例和版面**配置。 確切可用的調整大小選項可能會有所不同，因為這取決於顯示裝置的功能。
* [設定] 中的文字大小設定 **-> 輕鬆存取-> 顯示**。 調整 [**讓文字變大**] 設定，僅指定在所有應用程式和螢幕上支援控制項中的文字大小（所有 UWP 文字控制項都支援文字縮放體驗，而不需要任何自訂或範本）。 
> [!NOTE]
> [**讓所有專案變大**] 設定可讓使用者在主要畫面上，僅針對文字和應用程式指定其慣用的大小。

各種文字元素和控制項都有 [**IsTextScaleFactorEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.istextscalefactorenabled) 屬性。 這個屬性的預設值是 **true** 。 當**為 true**時，可以調整該元素中的文字大小。 縮放比例會影響具有較大**FontSize**的文字，而不會影響具有大型**FontSize**的文字。 您可以將元素的**IsTextScaleFactorEnabled**屬性設定為**false**，以停用自動調整大小。 

如需詳細資訊，請參閱[文字縮放](https://docs.microsoft.com/windows/uwp/design/input/text-scaling)。

將下列標記新增至應用程式並加以執行。 調整 [**文字大小**] 設定，並查看每個**TextBlock**會發生什麼事。

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

我們不建議您停用文字調整，因為在所有應用程式中，通用的 UI 文字調整會是使用者的重要協助工具體驗。

您也可以使用 [**TextScaleFactorChanged**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) 事件與 [**TextScaleFactor**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactor) 屬性，了解手機上 [文字大小] 設定的變更相關資訊。 方法如下：

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

**TextScaleFactor**的值是範圍中 \[1，2.25 @ no__t-2 的雙精度浮點數。 最小的文字依照此量放大。 舉例來說，您可以使用這個值來調整圖形以符合文字。 但是請記住，並非所有文字都會以相同比例縮放。 一般說來，較大的文字比較不容易受到縮放所影響。

這些類型都有 **IsTextScaleFactorEnabled** 屬性：  
* [**ContentPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter)
* [**控制項**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)和衍生類別
* [**FontIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FontIcon)
* [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)
* [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)
* [**TextElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.TextElement)和衍生類別

<span id="related_topics"/>

## <a name="related-topics"></a>相關主題  

* [文字縮放比例](https://docs.microsoft.com/windows/uwp/design/input/text-scaling)
* [協助工具](accessibility.md)
* [基本協助工具資訊](basic-accessibility-information.md)
* [XAML 文字顯示範例](https://go.microsoft.com/fwlink/p/?linkid=238579)
* [XAML 文字編輯範例](https://go.microsoft.com/fwlink/p/?linkid=251417)
* [XAML 協助工具範例](https://go.microsoft.com/fwlink/p/?linkid=238570) 
