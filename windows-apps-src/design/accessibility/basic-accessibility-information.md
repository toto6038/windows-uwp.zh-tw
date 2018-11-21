---
author: Xansky
Description: Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
title: 公開基本的協助工具資訊
label: Expose basic accessibility information
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d6b9d76dd20c4537fadf8c0701c200740c31b784
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7557344"
---
# <a name="expose-basic-accessibility-information"></a>公開基本的協助工具資訊  



基本協助工具資訊通常分類為名稱、角色以及值。 本主題描述的程式碼可協助您的 App 公開輔助技術所需的基本資訊。

<span id="accessible_name"/>
<span id="ACCESSIBLE_NAME"/>

## <a name="accessible-name"></a>無障礙名稱  
無障礙名稱是螢幕助讀程式用來宣告 UI 元素的簡單描述性文字字串。 設定 UI 元素的無障礙名稱時，名稱的意義必須有助於了解 UI 的內容或與 UI 的互動。 這類元素通常包含影像、輸入欄位、按鈕、控制項以及區域。

這個表格描述如何定義或取得 XAML UI 中各種元素類型的無障礙名稱。

| 元素類型 | 說明 |
|--------------|-------------|
| 靜態文字 | 若為 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 和 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) 元素，則會從可見的 (內部) 文字自動判斷無障礙名稱。 該元素中的所有文字都會當作名稱使用。 請參閱[來自內部文字的名稱](#name_from_inner_text)。 |
| 影像 | XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 元素與 **img** 及類似元素的 HTML **alt** 屬性沒有直接的相似項目。 請使用 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 提供名稱，或使用輔助字幕技術。 請參閱[影像的無障礙名稱](#images)。 |
| 表單元素 | 表單元素的無障礙名稱應該和元素的顯示標籤相同。 請參閱 [Labels 和 LabeledBy](#labels)。 |
| 按鈕和連結 | 根據預設值，按鈕或連結的無障礙名稱是以可見的文字為基礎，使用與[來自內部文字的名稱](#name_from_inner_text)中所述之規則相同的規則。 如果按鈕只包含一個影像，則使用 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 提供與設計的按鈕動作相當的純文字。 |

大部分的容器元素 (例如面板) 不會將它們的內容升級成無障礙名稱。 原因在於應該報告名稱和對應角色的是項目內容，而不是其容器。 容器元素可能會報告這個元素在 Microsoft 使用者介面自動化表示法中有子元素，因此輔助技術邏輯可以周遊這個元素。 但是使用輔助技術的使用者通常不需要知道容器為何，所以大部分的容器都沒有名稱。

<span id="role_value"/>
<span id="ROLE_VALUE"/>

## <a name="role-and-value"></a>角色和值  
屬於 XAML 詞彙的控制項和其他 UI 元素會實作使用者介面自動化支援，進而將角色和值報告為定義的一部分。 您可以使用使用者介面自動化工具檢查控制項的角色和值資訊，或者是閱讀每個控制項 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 實作的說明文件。 UI 自動化架構可用的角色定義在 [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182) 列舉中。 使用者介面自動化用戶端 (例如輔助技術) 能夠呼叫使用者介面自動化架構使用控制項的 **AutomationPeer** 來公開的方法，以取得角色資訊。

並不是所有控制項都有值。 沒有值的控制項會透過支援的對等和模式，向 UI 自動化報告此資訊。 例如，[**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 表單元素有值。 輔助技術可以是使用者介面自動化用戶端，因此能夠找到一個值並知道值是什麼。 在這個特殊案例中，**TextBox** 可透過 [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550) 定義來支援 [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) 模式。

> [!NOTE]
> 如果您是使用 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 或其他技術明確提供無障礙名稱，則不要在無障礙名稱中包含控制項角色或類型資訊所使用的文字。 例如，名稱中不要有像是 "button" 或 "list" 此類字串。 角色和類型資訊來自不同的使用者介面自動化屬性 (**LocalizedControlType**) 而這些屬性是由使用者介面自動化支援的預設控制項所提供。 許多輔助技術會將 **LocalizedControlType** 附加到無障礙名稱，因此無障礙名稱中的角色如果重複，就會造成單字不必要的重複。 例如，如果您為 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 控制項提供的無障礙名稱為 "button"，或者在名稱的最後一部分包含 "button"，則螢幕助讀程式可能會將這個控制項讀做 "button button"。 您應該使用朗讀程式來測試這個層面的協助工具資訊。

<span id="Influencing_the_UI_Automation_tree_views"/>
<span id="influencing_the_ui_automation_tree_views"/>
<span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"/>

## <a name="influencing-the-ui-automation-tree-views"></a>影響使用者介面自動化樹狀檢視  
使用者介面自動化架構含有樹狀檢視的概念，其中使用者介面自動化用戶端可以使用三種可能的檢視來擷取 UI 中元素間的關係：原始、控制項及內容。 控制項檢視是使用者介面自動化用戶端通常所使用的檢視，因為它提供 UI 中可互動元素的良好表示法和組織。 測試工具通常可以讓您選擇在工具呈現元素的組織時要使用哪一種樹狀檢視。

根據預設，當使用者介面自動化架構呈現通用 Windows 平台 (UWP) App 的 UI 時，所有 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 衍生的類別和一些其他元素將會出現在控制項檢視中。 但是，有時您會基於 UI 組合的緣故而不想讓元素出現在控制項檢視中，因為該元素正在複製資訊，或者呈現與協助工具案例無關緊要的資訊。 使用附加屬性 [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/Dn251788) 來變更在樹狀檢視中公開元素的方法。 如果您在 **Raw** 樹狀結構中放置元素，則大多數的輔助技術不會報告該元素為其檢視的一部分。 若要查看如何在現有控制項中運作的一些範例，請在文字編輯器中開啟 generic.xaml 設計參考 XAML 檔案，並在範本中搜尋 **AutomationProperties.AccessibilityView**。

<span id="name_from_inner_text"/>
<span id="NAME_FROM_INNER_TEXT"/>

## <a name="name-from-inner-text"></a>來自內部文字的名稱  
為了讓可見 UI 中已經存在的字串可以更容易當成無障礙名稱值，很多控制項和其他 UI 元素會根據元素的內部文字或來自內容屬性的字串值，自動判斷預設的無障礙名稱。

* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)、[**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)、[**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 以及 **RichTextBlock**，每一個都會將 **Text** 屬性的值升級為預設的無障礙名稱。
* 任何 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) 子類別都會使用反覆的 "ToString" 技術在它的 [**Content**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) 值內尋找字串，然後將這些字串升級為預設的無障礙名稱。

> [!NOTE]
> 根據使用者介面自動化的規定，無障礙名稱長度不可以超過 2048 個字元。 如果自動判斷無障礙名稱所使用的字串超過字元上限，則會截斷無障礙名稱超出的部分。

<span id="images"/>
<span id="IMAGES"/>

## <a name="accessible-names-for-images"></a>影像的無障礙名稱
為了支援螢幕助讀程式以及 UI 各個元素的基本識別資訊，您有時必須為影像和圖表 (不包括任何純裝飾或結構性元素) 這類的非文字資訊提供替代文字。 這些元素沒有內部文字，因此，無障礙名稱將不會有計算的值。 您可以按照以下範例，設定 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 附加屬性，以直接設定無障礙名稱。

XAML
```xml
<!-- Comment -->
<Image Source="product.png"
  AutomationProperties.Name="An image of a customer using the product."/>
```

您也可以考慮加入文字輔助字幕，讓它顯示在可見 UI 中，它同時也可以當作是影像內容中與標籤相關的協助工具資訊。 這裡提供一個範例：

XAML
```xml
<Image HorizontalAlignment="Left" Width="480" x:Name="img_MyPix"
  Source="snoqualmie-NF.jpg"
  AutomationProperties.LabeledBy="{Binding ElementName=caption_MyPix}"/>
<TextBlock x:Name="caption_MyPix">Mount Snoqualmie Skiing</TextBlock>
```

<span id="labels"/>
<span id="LABELS"/>

## <a name="labels-and-labeledby"></a>標籤和 LabeledBy  
將標籤與表單元素建立關聯的較好做法是使用 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 搭配 **x:Name** 來表示標籤文字，然後在表單元素上設定 [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 附加屬性以根據 XAML 名稱參照標籤 **TextBlock**。 如果您使用這種模式，則當使用者按一下標籤時，焦點會移至相關的控制項，然後輔助技術可以將標籤文字當作表單欄位的無障礙名稱。 以下範例示範這種技術。

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_FirstName">First name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_FirstName}"
      Name="tbFirstName" Width="100"/>
   </StackPanel>
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_LastName">Last name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_LastName}"
      Name="tbLastName" Width="100"/>
   </StackPanel>
 </StackPanel>
```

<span id="accessible_description"/>
<span id="ACCESSIBLE_DESCRIPTION"/>

## <a name="accessible-description-optional"></a>無障礙描述 (選用)  
無障礙描述提供特殊 UI 元素的額外協助工具資訊。 光靠無障礙名稱並不足以表達元素的用途時，您通常會提供無障礙描述。

只有使用者按 Caps Lock+F 要求元素的詳細資訊時，朗讀程式 (一種螢幕助讀程式) 才會朗讀元素的無障礙描述。

無障礙名稱的用途為識別控制項，不是詳細記載它的行為。 如果簡要描述不足以說明控制項，則除了 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 之外，您還可以設定 [**AutomationProperties.HelpText**](https://msdn.microsoft.com/library/windows/apps/Hh759765) 附加屬性。

<span id="Testing_accessibility_early_and_often"/>
<span id="testing_accessibility_early_and_often"/>
<span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"/>

## <a name="testing-accessibility-early-and-often"></a>及早並經常測試協助工具  
最後，支援螢幕助讀程式的最好方式，就是利用螢幕助讀程式親自測試您的應用程式。 這樣做會為您示範螢幕助讀程式的行為以及顯示應用程式可能遺漏的基本協助工具資訊。 接下來，您可以調整 UI 或使用者介面自動化屬性值。 如需詳細資訊，請參閱[協助工具測試](accessibility-testing.md)。

您可以用來測試協助工具的其中一個工具稱為 **AccScope**。 **AccScope** 工具特別有用，因為您可以看見 UI 的視覺表示法，該 UI 會呈現輔助技術如何將您的 app 當成動畫樹狀結構來檢視。 特別是提供了朗讀程式模式，讓您可以檢視朗讀程式如何從您的 app 中取得文字，以及如何組織 UI 中的元素。 AccScope 的設計讓您能夠在整個 app 開發週期中使用它，而且它非常有用，即使是在初步設計階段期間也一樣。 如需詳細資訊，請參閱 [AccScope](https://msdn.microsoft.com/library/windows/desktop/Dn433239)。

<span id="Accessible_names_from_dynamic_data"/>
<span id="accessible_names_from_dynamic_data"/>
<span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"/>

## <a name="accessible-names-from-dynamic-data"></a>動態資料的無障礙名稱  
Windows 支援許多控制項，而這些控制項可以透過名為「資料繫結」** 的功能來顯示相關資料來源的值。 將資料項目填入清單時，您可能需要使用一種技術，在填入初始清單之後為這些資料繫結的清單項目設定無障礙名稱。 如需詳細資訊，請參閱 [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)的＜案例 4＞。

<span id="Accessible_names_and_localization"/>
<span id="accessible_names_and_localization"/>
<span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"/>

## <a name="accessible-names-and-localization"></a>無障礙名稱和當地語系化  
為了確保無障礙名稱也是經過當地語系化的元素，應使用正確的技術將可當地語系化的字串儲存成資源，然後使用 [x:Uid directive](https://msdn.microsoft.com/library/windows/apps/Mt204791) 值來參照資源關係。 如果無障礙名稱來自於明確設定的 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 用法，請確定字串也可當地語系化。

請注意，附加屬性 (例如 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) 屬性) 會使用適用於資源名稱的特定合格語法，如此在套用到特定元素時資源便會參考附加屬性。 例如，在套用到名為 `MediumButton` 的 UI 元素時 [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) 的資源名稱為：`MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name`。

<span id="related_topics"/>

## <a name="related-topics"></a>相關主題  
* [協助工具](accessibility.md)
* [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)
* [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [協助工具測試](accessibility-testing.md)
