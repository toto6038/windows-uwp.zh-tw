---
Description: This article is an overview of the concepts and technologies related to accessibility scenarios for Universal Windows Platform (UWP) apps.
ms.assetid: AA053196-F331-4CBE-B032-4E9CBEAC699C
title: 協助工具概觀
label: Accessibility overview
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 50e6c68841440120b783713ef0a591e39a7c7eec
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7720374"
---
# <a name="accessibility-overview"></a>協助工具概觀  




本文提供通用 Windows 平台 (UWP) App 協助工具案例相關概念和技術的概觀。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]

<span id="Accessibility_and_your_app"/>
<span id="accessibility_and_your_app"/>
<span id="ACCESSIBILITY_AND_YOUR_APP"/>

## <a name="accessibility-and-your-app"></a>協助工具與應用程式  
身心障礙或功能損傷有許多種類，其中包括行動、視力、色彩分辨、聽力、說話、認知以及識字能力。 不過，只要遵守本文提供的指導方針，就可以滿足大部分的需求。 這表示提供：

* 鍵盤互動和螢幕助讀程式的支援。
* 使用者自訂的支援，例如字型、縮放設定 (放大)、色彩以及高對比設定。
* UI 組件的替代項目或補充項目。

XAML 的控制項提供內建的鍵盤以及輔助技術 (例如螢幕助讀程式) 支援，充分利用原本已經支援 UWP App、HTML 和其他 UI 技術的協助工具架構。 這種內建的支援提供基本協助工具，您只要設定一些屬性，便能輕易地自訂這些協助工具。 如果建立自己的自訂 XAML 元件和控制項，也可以使用「自動化對等」** 的概念，將類似的支援新增至這些控制項。

此外，利用資料繫結、樣式以及範本功能，便可以輕鬆實作支援顯示設定動態變更與替代 UI 的文字。

<span id="UI_Automation"/>
<span id="ui_automation"/>
<span id="UI_AUTOMATION"/>

## <a name="ui-automation"></a>UI 自動化  
對於協助工具支援，主要是來自針對 Microsoft UI 自動化架構的整合支援。 該支援是透過基底類別和針對控制項類型之類別實作的內建行為，以及使用者介面自動化提供者 API 的介面表示法來提供。 每一個控制項類別會使用自動化對等和自動化模式的使用者介面自動化概念，將控制項的角色和內容回報給使用者介面自動化用戶端。 使用者介面自動化會將 App 視為最上層視窗，然後透過使用者介面自動化架構，該 App 內所有與協助工具相關的內容都可供使用者介面自動化用戶端使用。 如需使用者介面自動化的詳細資訊，請參閱[使用者介面自動化概觀](https://msdn.microsoft.com/library/windows/desktop/Ee684076)。

<span id="Assistive_technology"/>
<span id="assistive_technology"/>
<span id="ASSISTIVE_TECHNOLOGY"/>

## <a name="assistive-technology"></a>輔助技術  
很多的使用者無障礙需求，都是透過使用者或工具安裝的輔助技術產品，再加上作業系統提供的各種設定，方可滿足。 其中包括螢幕助讀程式、螢幕放大以及高對比設定等功能。

輔助技術產品包括各式各樣的軟體和硬體。 這些產品可以透過標準鍵盤介面以及協助工具架構，將內容的資訊與 UI 的結構回報給螢幕助讀程式以及其他的輔助技術。 輔助技術產品範例包括：

* 螢幕小鍵盤，可以讓使用者使用指標工具替代鍵盤來輸入文字。
* 語音辨識軟體，可以將說出來的話變成輸入的文字。
* 螢幕助讀程式，可以將文字變成口說的話或者其他形式，例如點字。
* 朗讀程式螢幕助讀程式，這是 Windows 的專屬組件。 朗讀程式具有一個觸控模式，在沒有鍵盤可供使用時，可透過處理觸控手勢來執行螢幕助讀工作。
* 可以調整顯示器或區域的程式或設定，例如，高對比佈景主題、顯示器的 DPI 設定或放大鏡工具。

App 如果具有出色的鍵盤和螢幕助讀程式支援，通常也可以在各種的輔助技術產品上正常運作。 在許多情況下，UWP App 不用修改任何的資訊或結構，即可用在這些產品上。 不過，為了將協助工具體驗最佳化或者實作其他支援，您可能要修改某些設定。

您可用來配合輔助技術以測試基本協助工具案例的部分選項，可以參閱[協助工具測試](accessibility-testing.md)。

<span id="Screen_reader_support_and_basic_accessibility_information"/>
<span id="screen_reader_support_and_basic_accessibility_information"/>
<span id="SCREEN_READER_SUPPORT_AND_BASIC_ACCESSIBILITY_INFORMATION"/>

## <a name="screen-reader-support-and-basic-accessibility-information"></a>螢幕助讀程式支援和基本的協助工具資訊  
螢幕助讀程式可以讓使用者存取應用程式中的文字，將應用程式中的文字用其他格式呈現出來，例如語音或點字輸出。 螢幕助讀程式的行為，取決於軟體以及軟體的使用者設定。

例如，當使用者啟動或切換到要檢視的應用程式之後，某些螢幕助讀程式會朗讀整個應用程式 UI，讓使用者在嘗試瀏覽功能前，獲得所有可用的資訊內容。 某些螢幕助讀程式在個別控制項於 Tab 瀏覽時收到焦點後，也會朗讀個別控制項的相關文字。 這樣可以讓使用者在瀏覽應用程式的輸入控制項時，能夠了解目前的方向位置。 朗讀程式是螢幕助讀程式的一個例子，它會根據使用者的選擇提供兩種操作方式。

為了幫助使用者了解或瀏覽 app，螢幕助讀程式或任何其他輔助技術需要的最重要資訊是 app 元素組件的「無障礙名稱」**。 在許多情況下，控制項或元素已經有一個無障礙名稱，這個名稱是從您另外提供的其他屬性值計算得出的。 使用已計算的名稱最常見的情況是與一個支援和顯示內部文字的元素搭配使用。 至於其他元素，有時候您需要遵循元素結構的最佳做法，才能透過其他方法提供無障礙名稱。 而且有時候，您需要提供一個明確要當作 app 協助工具的無障礙名稱。 如需有多少計算的值適用於一般 UI 元素，以及一般無障礙名稱的詳細資訊，請參閱[基本協助工具資訊](basic-accessibility-information.md)。

還有其他數個自動化屬性可用，其中包括下一節描述的鍵盤屬性。 不過，並不是所有螢幕助讀程式都支援一切的自動化屬性。 一般情況下，您應該設定所有適用的自動化屬性並進行測試，為螢幕助讀程式盡可能提供最廣泛的支援。

<span id="Keyboard_support"/>
<span id="keyboard_support"/>
<span id="KEYBOARD_SUPPORT"/>

## <a name="keyboard-support"></a>鍵盤支援  
為了提供出色的鍵盤支援，您必須確定應用程式的各項功能都可以配合鍵盤的操作。 如果 App 大部分使用標準控制項，而且不使用任何的自訂控制項，那麼就可以節省許多工作。 基本的 XAML 控制項模型提供內建的鍵盤支援，其中包括 Tab 瀏覽、文字輸入以及控制項特有的支援。 當作配置容器 (例如面板) 的元素，使用配置順序建立預設的 Tab 順序。 這種順序通常是用於 UI 協助工具表示的正確 Tab 順序。 如果您使用 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 控制項來顯示資料，它們會提供內建的方向鍵瀏覽功能。 或者，如果您使用 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 控制項，這個控制項會預先處理按鈕啟用的空格鍵或 Enter 鍵。

如需鍵盤支援各層面的詳細資訊，包括定位順序及按鍵型啟用或瀏覽，請參閱[鍵盤協助工具](keyboard-accessibility.md)。

<span id="Media_and_captioning"/>
<span id="media_and_captioning"/>
<span id="MEDIA_AND_CAPTIONING"/>

## <a name="media-and-captioning"></a>媒體和字幕  
您通常是透過 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) 物件顯示視聽媒體。 您可以使用 **MediaElement** API，控制媒體播放。 為了達到協助工具的目的，請視需要提供讓使用者播放、暫停，和停止媒體的控制項。 有時候，媒體包含其他專門為協助工具而準備的元件，例如字幕或含旁白的替代音軌。

<span id="Accessible_text"/>
<span id="accessible_text"/>
<span id="ACCESSIBLE_TEXT"/>

## <a name="accessible-text"></a>協助工具文字  
與協助工具相關的三個主要文字層面：

* 工具必須判斷要以 Tab 順序瀏覽的一部分讀取文字，或者只以整體文件表示的一部分讀取文字。 您可以選擇適當的元素來顯示文字，或者調整這些文字元素的屬性，協助做出正確的判斷。 每一個文字元素都有特定用途，而且該用途通常都有一個對應的使用者介面自動化角色。 元素使用不當會造成將錯誤的角色報告給使用者介面自動化，而且會混淆輔助技術使用者。
* 許多使用者會有視覺方面的限制，除非背景的對比夠強，否則他們不容易看清楚文字。 應用程式設計人員如果沒有這種視覺方面的限制，無法一眼就看出這會對使用者產生多大的影響。 例如，以患有色盲的使用者為例，設計時選擇的色彩不合適，會造成某些使用者無法看清楚文字。 最初為網頁內容而制定的協助工具建議定義了一些對比標準，可以避免 app 出現上述問題。 如需詳細資訊，請參閱[無障礙文字需求](accessible-text-requirements.md)。
* 許多因為太小而導致使用者看不清楚的文字。 您可以一開始就在應用程式的 UI 中將文字設定為合理的大小，以避免這個問題。 不過，對於需要顯示大量文字，或者需要同時顯示文字和其他視覺元素的應用程式而言，這種做法會是一項挑戰。 遇到這種情況時，請確定應用程式可以與能夠放大畫面的系統功能正確互動，這樣應用程式中包含的任何文字就會隨之一起放大 (某些使用者會變更 DPI 值來做為其無障礙輔助。 該選項可以在 **\[輕鬆存取\]** 的 **\[讓螢幕上的內容更大一些\]** 中變更，這會重新導向到 **\[外觀及個人化\]** / **\[顯示\]** 的 **\[控制台\]** UI。)

<span id="Supporting_high-contrast_themes"/>
<span id="supporting_high-contrast_themes"/>
<span id="SUPPORTING_HIGH-CONTRAST_THEMES"/>

## <a name="supporting-high-contrast-themes"></a>支援高對比佈景主題  
UI 控制項會使用一種視覺表示，定義為佈景主題的 XAML 資源字典的一部分。 這些佈景主題中有一或多個是專門供系統設定為高對比時使用。 當使用者切換到高對比時，透過動態查詢資源字典的適當佈景主題，您的所有 UI 控制項也會使用適當的高對比佈景主題。 請確定您沒有透過指定明確的樣式或使用其他樣式設定技術，防止載入高對比佈景主題以及覆寫您的樣式變更等方法停用佈景主題。 如需詳細資訊，請參閱[高對比佈景主題](high-contrast-themes.md)。

<span id="Design_for_alternative_UI"/>
<span id="design_for_alternative_ui"/>
<span id="DESIGN_FOR_ALTERNATIVE_UI"/>

## <a name="design-for-alternative-ui"></a>替代 UI 設計  
設計應用程式時，請考量行動不便、視障以及聽障人士如何使用您的應用程式。 因為輔助技術產品大量使用標準 UI，即使未針對協助工具做出任何調整，也應該盡可能提供良好的鍵盤以及螢幕助讀程式支援。

在許多情況下，您可以利用多種技術傳達重要資訊，藉此吸引更多的使用者。 例如，您可以使用圖示或色彩資訊來突顯資訊，以協助色盲使用者。另外，除了聲音之外，您也可以顯示視覺警示，以協助聽障人士。

需要時，您可以提供替代的無障礙使用者介面元素，全面移除不重要的元素和動畫，與提供其他簡化方式，讓使用者更容易操作。 以下的程式碼範例示範如何根據使用者設定，顯示一個 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/BR227647) 執行個體，用於取代另一個執行個體。

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">

  <CheckBox x:Name="ShowAccessibleUICheckBox" Click="ShowAccessibleUICheckBox_Click">
    Show Accessible UI
  </CheckBox>

  <UserControl x:Name="ContentBlock">
    <local:ContentPage/>
  </UserControl>

</StackPanel>
```

Visual Basic
```vb
Private Sub ShowAccessibleUICheckBox_Click(ByVal sender As Object,
    ByVal e As RoutedEventArgs)

    If (ShowAccessibleUICheckBox.IsChecked.Value) Then
        ContentBlock.Content = New AccessibleContentPage()
    Else
        ContentBlock.Content = New ContentPage()
    End If
End Sub
```

C#
```csharp
private void ShowAccessibleUICheckBox_Click(object sender, RoutedEventArgs e)
{
    if ((sender as CheckBox).IsChecked.Value)
    {
        ContentBlock.Content = new AccessibleContentPage();
    }
    else
    {
        ContentBlock.Content = new ContentPage();
    }
}
```

<span id="Verification_and_publishing"/>
<span id="verification_and_publishing"/>
<span id="VERIFICATION_AND_PUBLISHING"/>

## <a name="verification-and-publishing"></a>驗證和發佈  
如需協助工具宣告及發佈 App 的相關詳細資訊，請參閱[市集中的協助工具](accessibility-in-the-store.md)。

> [!NOTE]
> 宣告 App 提供無障礙功能的這個動作只與 Microsoft Store 有關。

<span id="Assistive_technology_support_in_custom_controls"/>
<span id="assistive_technology_support_in_custom_controls"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_CUSTOM_CONTROLS"/>

## <a name="assistive-technology-support-in-custom-controls"></a>自訂控制項中的輔助技術支援  
當您建立自訂的控制項時，建議您也實作或擴充一或多個 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 子類別，以提供協助工具支援。 在某些情況下，只要您使用與基本控制項類別所使用的相同對等類別，衍生類別的自動化支援在基本層級就已經足夠。 不過，您還是應該進行測試，而且最好的做法還是建議您實作對等，這樣對等才能正確的回報新控制項類別的類別名稱。 實作自訂自動化對等有幾個步驟。 如需詳細資訊，請參閱[自訂自動化對等](custom-automation-peers.md)。

<span id="Assistive_technology_support_in_apps_that_support_XAML___Microsoft_DirectX_interop"/>
<span id="assistive_technology_support_in_apps_that_support_xaml___microsoft_directx_interop"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_APPS_THAT_SUPPORT_XAML___MICROSOFT_DIRECTX_INTEROP"/>

## <a name="assistive-technology-support-in-apps-that-support-xaml--microsoft-directx-interop"></a>應用程式中可支援 XAML / Microsoft DirectX 互通性的輔助技術支援  
以 XAML UI (使用 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/Dn252834) 或 [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/Hh702041)) 裝載的 Microsoft DirectX 內容預設並非無障礙內容。 [XAML SwapChainPanel DirectX 互通性範例](http://go.microsoft.com/fwlink/p/?LinkID=309155)說明如何針對裝載的 DirectX 內容建立使用者介面自動化對等。 這項技術可透過使用者介面自動化讓裝載的內容成為無障礙內容。

## <a name="related-topics"></a>相關主題  
* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/BR209179)
* [協助工具設計](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [協助工具](accessibility.md)
* [開始使用朗讀器](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)
