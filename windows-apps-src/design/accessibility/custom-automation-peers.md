---
author: Xansky
Description: Describes the concept of automation peers for Microsoft UI Automation, and how you can provide automation support for your own custom UI class.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: 自訂自動化對等
label: Custom automation peers
template: detail.hbs
ms.author: mhopkins
ms.date: 07/13/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 37f75686c51e876cd4e6b608d0f527d3e32e4e24
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5887384"
---
# <a name="custom-automation-peers"></a>自訂自動化對等  

說明 Microsoft 使用者介面自動化的自動化對等，以及如何提供自訂 UI 類別的自動化支援。

自動化用戶端可以使用 UI 自動化提供的架構來檢查或操作不同 UI 平台和架構的使用者介面。 如果您正在撰寫通用 Windows 平台 (UWP) App，則您用於 UI 的類別已提供使用者介面自動化支援。 您可以從現有的非密封類別衍生新的類別，藉此定義新的 UI 控制項或支援類別。 在進行這個程序的時候，您的類別可以新增預設使用者介面自動化支援所沒有的協助工具支援行為。 在這種情況下，您應該從基底實作使用的 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 類別衍生以延伸現有使用者介面自動化支援，將任何需要的支援新增至對等實作，然後通知通用 Windows 平台 (UWP) 控制項基礎結構必須建立您的新對等。

使用者介面自動化不僅可以啟用無障礙應用程式和輔助技術 (例如螢幕助讀程式)，也可以啟用品質保證 (測試) 程式碼。 無論是哪一種情況，UI 自動化用戶端均可以利用您應用程式外部的其他程式碼檢查使用者介面元素，以及模擬使用者與您應用程式的互動。 如需所有平台的 UI 自動化的廣義相關資訊，請參閱 [UI 自動化概觀](https://msdn.microsoft.com/library/windows/desktop/Ee684076)。

有兩種獨特的對象會使用使用者介面自動化架構。

* **使用者介面自動化 *用戶端*** 會呼叫使用者介面自動化 API，以了解目前向使用者顯示的所有 UI。 例如，螢幕助讀程式之類的輔助技術會做為 UI 自動化用戶端。 UI 以樹狀目錄來呈現相關的自動化元素。 UI 自動化用戶端可能一次只呈現一個應用程式，或是整個樹狀結構。 使用者介面自動化用戶端可以使用使用者介面自動化 API 來瀏覽樹狀，以及讀取或變更自動化元素中的資訊。
* **使用者介面自動化*提供者*** 會實作 API，這些 API 會公開其在應用程式中引入的 UI 元素，進而將資訊提供給使用者介面自動化樹狀。 在建立新的控制項時，您應該成為 UI 自動化提供者案例中的參與者。 身為提供者，您應該確保所有的 UI 自動化用戶端可以在提供無障礙功能及測試時使用 UI 自動化架構與您的控制項互動。

一般而言，使用者介面自動化架構中有兩個平行的 API：一個適用於使用者介面自動化用戶端，而另一個具有類似名稱的 API 則適用於使用者介面自動化提供者。 本主題的大部分內容涵蓋適用於使用者介面自動化提供者的 API，特別是讓提供者能夠在該 UI 架構中具備擴充性的類別和介面。 我們偶爾會提到使用者介面自動化用戶端所使用的使用者介面自動化 API，這主要是為了讓您了解部分觀點，或提供與用戶端和提供者 API 相關聯的查詢表格。 如需用戶端觀點的詳細資訊，請參閱[使用者介面自動化用戶端程式設計人員指南](https://msdn.microsoft.com/library/windows/desktop/Ee684021)。

> [!NOTE]
> 使用者介面自動化用戶端通常不會使用 Managed 程式碼，一般也不會實作為 UWP App (它們通常是傳統型應用程式)。 使用者介面自動化的基礎為標準的實作或架構，而不是特定的實作或架構。 許多現有的使用者介面自動化用戶端 (包括螢幕助讀程式之類的輔助技術產品) 使用元件物件模型 (COM) 介面與使用者介面自動化、系統和在子視窗中執行的應用程式互動。 如需 COM 介面以及如何撰寫使用 COM 的使用者介面自動化用戶端的詳細資訊，請參閱[使用者介面自動化基礎](https://msdn.microsoft.com/library/windows/desktop/Ee684007)。

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>判斷自訂 UI 類別之 UI 自動化支援的現有狀態。  
開始嘗試為自訂控制項實作自動化對等前，您應該測試基礎類別和它的自動化對等是否已提供您需要的無障礙功能或自動化支援。 很多時候，組合使用 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 實作、特定對等以及它們實作的模式即可提供基本但令人滿意的無障礙功能體驗。 至於是否真的可以提供令人滿意的無障礙功能，則取決於您為那些公開給控制項與其基礎類別的物件模型做了多少的變更。 另外，這也取決於您在基礎類別新增的項目是否與範本協定中的新 UI 元素或控制項的視覺外觀相關。 有時候您的變更可能會產生需要額外協助工具支援的新操作功能。

即使使用現有的基礎對等類別只能支援基本的無障礙功能，但對於自動化測試而言，最佳做法仍然是定義對等，讓您可以將準確的 **ClassName** 資訊報告給使用者介面自動化。 如果您要撰寫協力廠商專用的控制項，這項考慮尤其重要。

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>自動化對等類別  
UWP 是利用現有 UI 自動化技術和舊版的 Managed 程式碼 UI 架構 (例如 Windows Forms、Windows Presentation Foundation (WPF) 以及 Microsoft Silverlight) 所使用的慣例建立而成的。 很多控制項類別以及它們的函式與用途都是源自於舊版的 UI 架構。

按照慣例，對等類別名稱的開頭是控制項類別名稱，結尾則是 "AutomationPeer"。 例如，[**ButtonAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242458) 是 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 控制項類別的對等類別。

> [!NOTE]
> 基於本主題的目的，當您實作控制項對等時，我們會將協助工具相關的屬性視為更重要的物件。 但是，對於使用者介面自動化支援的一般概念，您應該依據[使用者介面自動化提供者程式設計人員指南](https://msdn.microsoft.com/library/windows/desktop/Ee671596)與[使用者介面自動化基礎](https://msdn.microsoft.com/library/windows/desktop/Ee684007)中記載的建議來實作對等。 這些主題沒有涵蓋用來在 UWP 架構中為 UI 自動化提供資訊的特定 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) API，不過它們確實有描述用來識別您類別的屬性，或是提供其他資訊或互動。

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>對等、模式及控制項類型  
「控制項模式」** 是一種介面實作，可以將控制項功能的特定層面公開給 UI 自動化用戶端。 UI 自動化用戶端使用透過控制項模式公開的屬性與方法來抓取控制項的功能資訊，或者在執行階段操縱控制項的行為。

控制項模式會提供分類和公開控制項功能的方法，而這個方法與控制項類型或控制項外觀無關。 例如，顯示表格式介面的控制項會使用 **Grid** 控制項模式公開表格的欄數與列數，並讓使用者介面自動化用戶端可以從表格中抓取項目。 其他範例包括，使用者介面用戶端可以使用 **Invoke** 控制項模式來表示可以被叫用的控制項 (例如按鈕)，並使用 **Scroll** 控制項模式來表示有捲軸的控制項 (例如，清單方塊、清單檢視或下拉式方塊)。 每種控制項模式分別描述個別的功能類型，您可以結合不同的控制項模式來描述特定控制項所支援的完整功能。

控制項模式與 UI 的關聯，正如同介面與 COM 物件的關聯一樣。 在 COM 中，您可以查詢物件以詢問支援哪些介面，然後使用這些介面來存取功能。 在 UI 自動化中，UI 自動化用戶端可以查詢 UI 自動化元素來找出它所支援的控制項模式，然後透過支援的控制項模式所公開的屬性、方法、事件以及結構，以與元素及其對等控制項進行互動。

自動化對等的主要用途之一，就是向使用者介面自動化用戶端報告 UI 元素可透過其對等項目支援的控制項模式。 為了達到這個目的，使用者介面自動化提供者會透過覆寫 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 方法，實作變更 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 方法行為的新對等。 使用者介面自動化用戶端會進行呼叫，而使用者介面自動化提供者會將這些呼叫對應成呼叫 **GetPattern**。 使用者介面自動化用戶端會查詢它們要互動的每個特定模式。 如果對等支援這種模式，就會將物件參考傳回給自己，否則會傳回 **null**。 如果不是傳回 **null**，則使用者介面用戶端會預期它可以呼叫模式介面的 API 做為用戶端，來與該控制項模式互動。

使用「控制項類型」** 可以廣泛定義對等所代表的控制項功能。 這與控制項模式的概念不同，因為模式會通知使用者介面自動化可以得到什麼資訊，或可以透過特定介面執行什麼動作，而控制項類型則是存在於更高的一個層級。 每個控制項類型都有這些使用者介面自動化層面的相關指導方針：

* 使用者介面自動化控制項模式：一個控制項類型可能支援一個以上的模式，每個模式都代表一個不同的資訊或互動的分類。 每個控制類型都有一組控制項必須支援、一組選用以及一組控制項不得支援的控制項模式。
* 使用者介面自動化屬性值：每個控制項類型都有一組控制項必須支援的屬性。 這些是一般屬性 (如[使用者介面自動化屬性概觀](https://msdn.microsoft.com/library/windows/desktop/Ee671594)所述)，不是模式特定屬性。
* 使用者介面自動化事件：每個控制項類型都有一組控制項必須支援的事件。 同樣地，這些都是一般的，不是模式特定的，如[使用者介面自動化事件概觀](https://msdn.microsoft.com/library/windows/desktop/Ee671221)所述。
* 使用者介面自動化樹狀結構：每個控制項類型皆定義控制項在使用者介面自動化樹狀結構中必須如何顯示。

不論架構的自動化對等是如何實作的，使用者介面自動化用戶端功能都與 UWP 無關，事實上，現有的使用者介面自動化用戶端 (例如輔助技術) 有可能會使用其他程式設計模型 (例如 COM)。 在 COM 中，用戶端可以對實作要求模式或一般使用者介面自動化架構的 COM 控制項模式介面執行 **QueryInterface**，以便檢查屬性、事件或樹狀結構。 如果是模式，使用者介面自動化架構會將該介面程式碼，封送處理到在 app 的使用者介面自動化提供者及相關對等上執行的 UWP 程式碼中。

為 Managed 程式碼架構 (如使用 C\# 或 Microsoft Visual Basic 的 UWP app) 實作控制項模式時，可以使用 .NET Framework 介面而不使用 COM 介面來呈現這些模式。 例如，Microsoft .NET 提供者實作使用者介面自動化模式介面時，其 **Invoke** 模式為 [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582)。

如需控制項模式、提供者介面及其用途的清單，請參閱[控制項模式和介面](control-patterns-and-interfaces.md)。 如需控制項類型的清單，請參閱[使用者介面自動化控制項類型概觀](https://msdn.microsoft.com/library/windows/desktop/Ee671197)。

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>如何實作控制項模式的指導方針  
控制項模式及其用途屬於使用者介面自動化架構較廣泛定義的一部分，而不僅適用於 UWP app 的協助工具支援。 當您實作控制項模式時，應確定您的實作方式與 MSDN 及使用者介面自動化規格所記載的指導方針相符。 如果您正在尋找指導方針，一般只要使用 MSDN 主題即可，不需要參考規格。 每個模式的指導方針都記載在：[實作 UI 自動化控制項模式](https://msdn.microsoft.com/library/windows/desktop/Ee671292)。 您會注意到這個領域下的每個主題都有＜實作指導方針與慣例＞小節和＜必要成員＞小節。 指導方針通常會參照[提供者的控制項模式介面](https://msdn.microsoft.com/library/windows/desktop/Ee671201)參考中相關控制項模式介面的特定 API。 這些介面是原生/COM 介面 (而它們的 API 使用 COM 樣式語法)。 但是您在該處看到的所有內容，在 [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225) 命名空間中都有對等的項目。

如果您使用的是預設自動化對等並針對其行為進行擴充，則這些對等是以符合使用者介面自動化指導方針的方式撰寫。 如果它們支援控制項模式，您就可以倚賴該項符合[實作使用者介面自動化控制項模式](https://msdn.microsoft.com/library/windows/desktop/Ee671292)指導方針的模式支援。 如果某個控制項對等回報它代表使用者介面自動化所定義的控制項類型，則表示該對等已經遵循[支援使用者介面自動化控制項類型](https://msdn.microsoft.com/library/windows/desktop/Ee671633)所記載的指導方針。

不過，您可能需要控制項模式或控制項類型的其他指導方針，以便在您的對等實作中遵循使用者介面自動化建議。 這對於您正在實作模式或控制類型支援尚未在 UWP 控制項中成為預設實作的情況下，尤其適用。 例如，所有預設的 XAML 控制項中都沒有實作用於註解的模式。 但是您可能有一個應用程式廣泛地使用註解，因此想要讓該功能顯示成可供存取。 對此案例而言，您的對等應該實作 [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493)，而且可能應將自己回報成具有適當的屬性的 **Document** 控制項類型，以指出您的文件支援註解。

建議您使用所看到適用於[實作使用者介面自動化控制項模式](https://msdn.microsoft.com/library/windows/desktop/Ee671292)下的模式或[支援使用者介面自動化控制項類型](https://msdn.microsoft.com/library/windows/desktop/Ee671633)下的控制項類型的指導方針，做為方向與一般指導方針。 您甚至可以嘗試遵循一些 API 連結來取得有關 API 用途的描述與備註。 不過，如需 UWP App 之程式設計所需的特定語法，請在 [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225) 命名空間內找出對等的 API，使用這些參考頁面以取得詳細資訊。

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>內建的自動化對等類別  
在一般情況下，如果元素接受來自使用者的 UI 活動，或者如果元素包含輔助技術 (用於呈現 App 的互動式或有意義的 UI) 使用者需要的資訊，則這些元素就會實作自動化對等類別。 並非所有 UWP 視覺元素都有自動化對等。 實作自動化對等的類別範例有 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)。 不實作自動化對等的類別範例有 [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) 和以 [**Panel**](https://msdn.microsoft.com/library/windows/apps/BR227511) 為基礎的類別 (例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 和 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267))。 **Panel** 沒有對等，因為它提供一種純視覺的配置行為。 使用者沒有任何與協助工具相關的方式來與 **Panel** 互動。 不管 **Panel** 包含什麼子元素，都會向使用者介面自動化樹狀目錄報告為樹狀目錄中下一個可用且含對等或元素表示法之父項元素的子元素。

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>使用者介面自動化與 UWP 處理程序界限  
一般而言，存取 UWP App 的使用者介面自動化用戶端程式碼會執行跨處理序。 使用者介面自動化架構基礎結構能夠讓資訊跨越處理程序的界限。 這個概念在[使用者介面自動化基礎](https://msdn.microsoft.com/library/windows/desktop/Ee684007)中有更詳盡的說明。

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
所有從 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 衍生的類別都包含受保護的虛擬方法 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer)。 自動化對等的物件初始化順序會呼叫 **OnCreateAutomationPeer**，以取得每一個控制項的自動化對等物件，然後建立一個使用者介面自動化樹狀目錄以用於執行階段。 使用者介面自動化程式碼可以使用對等來取得控制項特性與功能的相關資訊，並使用其控制項模式來模擬互動式功能。 支援自動化的自訂控制項必須覆寫 **OnCreateAutomationPeer**，然後傳回一個衍生自 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 的類別執行個體。 例如，如果自訂控制項是從 [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/BR227736) 類別衍生的，則 **OnCreateAutomationPeer** 傳回的物件應該是從 [**ButtonBaseAutomationPeer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer) 衍生的。

如果您在撰寫自訂控制項類別並想要一併提供新的自動化對等，您應該覆寫自訂控制項的 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 方法，如此一來，它才能傳回新的對等執行個體。 您的對等類別必須從 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 直接或間接衍生。

例如，以下程式碼會宣告自訂控制項 `NumericUpDown` 應該將對等 `NumericUpDownPeer` 用於使用者介面自動化。

```csharp
using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}
```

```vb
Public Class NumericUpDown
    Inherits RangeBase
    ' other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class
```

```cppwinrt
// NumericUpDown.idl
namespace MyNamespace
{
    runtimeclass NumericUpDown : Windows.UI.Xaml.Controls.Primitives.RangeBase
    {
        NumericUpDown();
        Int32 MyProperty;
    }
}

// NumericUpDown.h
...
struct NumericUpDown : NumericUpDownT<NumericUpDown>
{
    ...
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer()
    {
        return winrt::make<MyNamespace::implementation::NumericUpDownAutomationPeer>(*this);
    }
};
```

```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDownAutomationPeer(this);
    }
};
```

> [!NOTE]
> [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 實作最好只用於初始化自訂自動對等的新執行個體就好，以擁有者方式傳送呼叫控制項，然後傳回該執行個體。 請勿在這個方法中嘗試執行其他邏輯。 特別是任何可能會破壞同一個呼叫內的 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 的邏輯，這些邏輯會造成未預期的執行階段行為。

在典型的 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 實作中，因為方法覆寫的範圍與其餘的控制項類別相同，所以 *owner* 會被指定成 **this** 或 **Me**。

實際的對等類別定義可以在與控制項相同的程式碼檔案中完成，或者在獨立的程式碼檔案中完成。 對等定義全部放置在 [**Windows.UI.Xaml.Automation.Peers**](https://msdn.microsoft.com/library/windows/apps/BR242563) 命名空間中，它與提供對等的控制項是不同的命名空間。 只要您參考 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 方法呼叫所需的命名空間，您也可以選擇在獨立的命名空間中宣告對等。

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>選擇正確的對等基礎類別  
確定您的 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 衍生自基礎類別，而該基礎類別與衍生來源的控制項類別有完全符合的現有對等邏輯。 在前面的範例中，因為 `NumericUpDown` 衍生自 [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863)，所以有可用來做為對等項目基礎的 [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) 類別。 使用與衍生控制項本身的方法最接近的相符對等類別，至少可以避免覆寫部分的 [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) 功能，因為基底對等類別已經實作它了。

基礎 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 類別沒有對應的對等類別。 如果您需要與衍生自 **Control** 的自訂控制項對應的對等類別，請從 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 衍生自訂的對等類別。

如果從 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) 直接衍生，該類別就不會有預設的自動化對等行為，因為沒有參考對等類別的 [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) 實作。 所以請務必實作 **OnCreateAutomationPeer** 以使用自己的對等，或者如果控制項已有足夠的協助工具支援，則使用 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 當作對等。

> [!NOTE]
> 您通常會從 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 而不是從 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 衍生。 如果您確實是從 **AutomationPeer** 直接衍生，則您將需要複製許多基本協助工具支援，而這類支援是來自 **FrameworkElementAutomationPeer**。

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>初始化自訂對等類別  
自動化對等應該定義一個類型安全建構函式，而這個建構函式會使用擁有者控制項的執行個體來進行基本初始化。 在下一個範例中，實作會將 *owner* 值傳送至 [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) 基底，最後這個 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 會使用 *owner* 來設定 [**FrameworkElementAutomationPeer.Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner)。

```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

```cppwinrt
// NumericUpDownAutomationPeer.idl
import "NumericUpDown.idl";
namespace MyNamespace
{
    runtimeclass NumericUpDownAutomationPeer : Windows.UI.Xaml.Automation.Peers.AutomationPeer
    {
        NumericUpDownAutomationPeer(NumericUpDown owner);
        Int32 MyProperty;
    }
}

// NumericUpDownAutomationPeer.h
...
struct NumericUpDownAutomationPeer : NumericUpDownAutomationPeerT<NumericUpDownAutomationPeer>
{
    ...
    NumericUpDownAutomationPeer(MyNamespace::NumericUpDown const& owner);
};
```

```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>AutomationPeer 的核心方法  
基於 UWP 基礎結構的理由，自動化對等的可覆寫方法是一組方法配對的一部分：使用者介面自動化提供者做為使用者介面自動化用戶端轉送點使用的公開存取方法，以及 UWP 類別可以覆寫以影響行為的受保護 "Core" 自訂方法。 方法配對預設會連接在一起，透過這種方式，呼叫存取方法時一定會叫用已經實作提供者的平行 "Core" 方法，或者當作從基礎類別叫用預設實作的後援機制。

為自訂控制項實作對等時，請從想要公開自訂控制項獨特行為的基礎自動化對等類別來覆寫任何 "Core" 方法。 使用者介面自動化程式碼會呼叫對等類別的公用方法，以取得控制項的相關資訊。 若要提供控制項的相關資訊，當控制項實作與設計所建立的協助工具或其他使用者介面自動化案例不同於基礎自動化對等類別支援的使用者介面自動化案例時，請覆寫名稱以 "Core" 結束的每個方法。

當您定義新的對等類別時，至少應該實作 [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) 方法，如以下範例所示。

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> 您可能會想將字串儲存為常數，而不是直接放置在方法的內文中，全視您的喜好而定。 在 [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) 中，您將不需要當地語系化這個字串。 每當使用者介面自動化用戶端需要使用當地語系化字串時會使用 **LocalizedControlType** 屬性，而不是 **ClassName**。

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

部分輔助技術在報告使用者介面自動化樹狀目錄中的項目特性 (使用者介面自動化 **Name** 以外的其他資訊) 時，會直接使用 [**GetAutomationControlType**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) 值。 如果您的控制項與衍生的控制項有很大的差異，而且您希望報告的控制項類型與控制項使用之基礎對等類別報告的控制項類型不同，就必須實作對等並覆寫對等實作中的 [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore)。 如果衍生自如 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) 或 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) 的一般化基礎類別 (這些基礎類別不提供控制項類型的精確資訊)，這就特別重要。

實作的 [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) 會傳回 [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182) 值來描述您的控制項。 雖然您可以傳回 **AutomationControlType.Custom**，不過您應該傳回一個更明確的控制項類型 (只要它可以準確描述控制項的主要情況)。 範例如下。

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> 除非指定 [**AutomationControlType.Custom**](https://msdn.microsoft.com/library/windows/apps/BR209182)，否則您不需要實作 [**GetLocalizedControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore) 將 **LocalizedControlType** 屬性值提供給用戶端。 使用者介面自動化通用基礎結構為 **AutomationControlType.Custom** 以外的每個可能的 **AutomationControlType** 值提供已轉譯的字串。

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern 和 GetPatternCore  
對等的 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 實作會傳回物件，這些物件支援輸入參數中要求的模式。 具體而言，使用者介面自動化用戶端會呼叫轉送到提供者 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 方法的方法，並指定表示要求模式的 [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) 列舉值。 覆寫 **GetPatternCore** 之後應該會傳回一個可以實作指定模式的物件。 該物件也就是對等本身，因為對等在報告它支援模式時必須實作對應的模式介面。 如果您的對等沒有模式的自訂實作，但是您確信對等的基底實作了模式，則可以從您的 **GetPatternCore** 呼叫基礎類型的 **GetPatternCore** 實作。 如果對等不支援模式，對等的 **GetPatternCore** 應該要傳回 **null**。 不過，通常並不會直接從您的實作傳回 **null**，而是會倚賴呼叫基底實作來針對任何不支援的模式傳回 **null**。

支援某個模式時，[**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 實作可以傳回 **this** 或 **Me**。 我們希望只要傳回的值不是 **null**，使用者介面自動化用戶端就會將 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 傳回值轉換成要求的模式介面。

如果對等類別繼承自另一個對等，則所有需要的支援和模式報告均已由基礎類別進行處理，不需要實作 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore)。 例如，如果您正在實作一個衍生自 [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863) 的範圍控制項，而且您的對等衍生自 [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506)，則該對等會為 [**PatternInterface.RangeValue**](https://msdn.microsoft.com/library/windows/apps/BR242496) 傳回它自己，而且已有支援該模式的 [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) 介面實作。

雖然這個範例不是文字程式碼，但近似於 [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) 中已經有的 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 實作。


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}
```

如果您實作的對等無法從基礎對等類別得到所有需要的支援，或者您想要變更或新增您的對等可以支援的基礎繼承模式，則您應該覆寫 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 讓使用者介面自動化用戶端可以使用這些模式。

如需使用者介面自動化支援的 UWP 實作中可用的提供者模式清單，請參閱 [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225)。 每一個模式都有對應的 [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) 列舉值，這個值說明使用者介面自動化用戶端如何在 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 呼叫中要求模式。

對等可以報告它支援多個模式。 如果可支援多個模式，則此覆寫應該包含每個支援之 [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) 值的傳回路徑邏輯，並傳回每個相符的對等。 呼叫者一次將只能要求一個介面，而且由呼叫者決定是否轉換成正確的介面。

以下範例示範自訂對等的 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 覆寫。 它報告兩種模式的支援：[**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) 和 [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653)。 這裡的控制項是一個媒體顯示控制項，它可以顯示全螢幕 (切換模式)，而且有一個可以讓使用者在其中選取位置 (範圍控制項) 的進度列。 這個程式碼來自 [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)。


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}
```

<span id="Forwarding_patterns_from_sub-elements"/>
<span id="forwarding_patterns_from_sub-elements"/>
<span id="FORWARDING_PATTERNS_FROM_sub-elementS"/>

### <a name="forwarding-patterns-from-sub-elements"></a>來自子元素的轉送模式  
[**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 方法實作也可以指定某個子元素或組件做為其主機的模式提供者。 這個範例模擬 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) 如何將捲動模式處理傳輸到它的內部 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527) 控制項對等。 為了指定子元素來進行模式處理，這個程式碼會取得子元素物件、使用 [**FrameworkElementAutomationPeer.CreatePeerForElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement) 方法來建立子元素的對等，然後傳回新的對等。


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) && (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}
```

<span id="Other_Core_methods"/>
<span id="other_core_methods"/>
<span id="OTHER_CORE_METHODS"/>

### <a name="other-core-methods"></a>其他 Core 方法  
在主要案例中，您的控制項可能需要支援鍵盤對等功能；如需為什麼必須這樣做的詳細資訊，請參閱[鍵盤協助工具](keyboard-accessibility.md)。 實作按鍵支援是控制項程式碼而非對等程式碼中的必要部分，因為它是控制項邏輯的一部分，但是對等類別應該覆寫 [**GetAcceleratorKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) 和 [**GetAccessKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) 方法，以便向 UI 自動化用戶端報告所使用的按鍵。 報告按鍵資訊的字串可能需要當地語系化，因此應該來自資源，而不是硬式編碼字串。

如果您替某個支援集合的類別提供對等，最好是衍生自已經提供這類集合支援的功能類別和對等類別。 如果您無法這樣做，維護子集合的控制項對等可能必須覆寫與集合相關的對等方法 [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore)，這樣才能正確向 UI 自動化樹狀目錄報告父系-子系關係。

實作 [**IsContentElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) 和 [**IsControlElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) 方法，指示您的控制項是包含資料內容還是在使用者介面中使用互動式角色 (或二者)。 根據預設值，這兩種方法都會傳回 **true**。 這些設定可提升輔助技術 (例如螢幕助讀程式) 的可用性，而這些輔助技術則可以使用這些方法來篩選自動化樹狀目錄。 如果 [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) 方法會將模式處理傳輸到子元素對等，子元素對等的 **IsControlElementCore** 方法便可傳回 **false** 以在自動化樹狀目錄中隱藏子元素對等。

有些控制項可以支援標籤，其中的文字標籤部分會提供非文字部分的資訊，或是控制項會被設計成與 UI 中另一個控制項具有已知的標籤關係。 如果能夠提供有用的類別行為，則可以覆寫 [**GetLabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) 來提供這個行為。

[**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) 和 [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) 主要用在自動測試的情況下。 如果您想要支援控制項進行自動測試，則需要覆寫這些方法。 範圍類型控制項可能就需要這樣做 (您不能在範圍類型控制項中只提供單一點)，因為使用者按一下座標空間時會對範圍產生不同的效果。 例如，預設的 [**ScrollBar**](https://msdn.microsoft.com/library/windows/apps/BR209745) 自動化對等會覆寫 **GetClickablePointCore** 以傳回「不是數字」的 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值。

[**GetLiveSettingCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) 會影響使用者介面自動化 **LiveSetting** 值的控制項預設值。 如果您希望控制項傳回 [**AutomationLiveSetting.Off**](https://msdn.microsoft.com/library/windows/apps/JJ191519) 以外的值，則可以覆寫這個值。 如需 **LiveSetting** 代表什麼的詳細資訊，請參閱 [**AutomationProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516)。

如果控制項有可以對應到 [**AutomationOrientation**](https://msdn.microsoft.com/library/windows/apps/BR209184) 的可設定方向屬性，您可以覆寫 [**GetOrientationCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getorientationcore)。 [**ScrollBarAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242522) 和 [**SliderAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242546) 類別都有這個屬性。

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>FrameworkElementAutomationPeer 中的基底實作  
[**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 的基底實作提供一些 UI 自動化資訊，這些資訊是從在架構層級定義的各種配置及行為屬性轉譯而來。

* [**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore)：根據已知的配置特性傳回 [**Rect**](https://msdn.microsoft.com/library/windows/apps/BR225994) 結構。 如果 [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) 是 **true**，傳回 0 值的 **Rect**。
* [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore)：只要是非零的 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)，則根據已知的配置特性傳回 **BoundingRectangle** 結構。
* [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore)：具有更廣泛的行為，本文無法全部概述；請參閱 [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore)。 它基本上會嘗試轉譯 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) 的任何已知內容或包含內容的相關類別中的字串。 此外，如果 [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 有值，它的 **Name** 值會被做為 **Name**。
* [**HasKeyboardFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore)：根據擁有者的 [**FocusState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.focusstate) 和 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) 屬性加以評估。 不是控制項的元素一定會傳回 **false**。
* [**IsEnabledCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isenabledcore)：如果它是 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390)，則根據擁有者的 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) 屬性加以評估。 不是控制項的元素一定會傳回 **true**。 就傳統的互動觀點而言，這不表示會啟用擁有者，而是表示即使擁有者沒有 **IsEnabled** 屬性，還是會啟用對等。
* [**IsKeyboardFocusableCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore)：如果擁有者為 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390)，則傳回 **true**；否則傳回 **false**。
* [**IsOffscreenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore)：擁有者元素或其任何父項上的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility) 如果是 [**Collapsed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visibility)，就等於 [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) 的值是 **true**。 例外：[**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842) 物件擁有者的父項即使不可見，此物件仍然可以是可見的物件。
* [**SetFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.setfocuscore)：呼叫 [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161)。
* [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent)：從擁有者呼叫 [**FrameworkElement.Parent**](https://msdn.microsoft.com/library/windows/apps/BR208739)，並查詢適當的對等。 它不是與 "Core" 方法成對的覆寫方法，所以您不能變更這個行為。

> [!NOTE]
> 預設的 UWP 對等會使用實作 UWP 的內部原生程式碼來實作行為，而不一定會使用實際的 UWP 程式碼。 您將無法透過通用語言執行平台 (CLR) 反射或其他技術，看到實作的程式碼或邏輯。 您也看不到針對基礎對等行為的子類別特定覆寫項目所顯示的獨特參考頁面。 例如，[**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550) 的 [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore) 可能有其他行為，這些行為在 **AutomationPeer.GetNameCore** 參考頁面中不會加以描述，而且也不會有 **TextBoxAutomationPeer.GetNameCore** 的參考頁面。 甚至不會有 **TextBoxAutomationPeer.GetNameCore** 參考頁面。 請改為閱讀最直接的對等類別參考主題，並查看＜備註＞小節中的實作附註。

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>Peer 和 AutomationProperties  
自動化對等應該為控制項的協助工具相關資訊提供正確的預設值。 請注意，任何使用控制項的 App 程式碼都可以在控制項執行個體中包含 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) 附加屬性以覆寫部分的行為。 呼叫者可以為預設控制項或者為自訂控制項執行上述動作。 例如，下列 XAML 會建立一個具有兩個自訂使用者介面自動化屬性的按鈕： `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

如需 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) 附加屬性的詳細資訊，請參閱 [基本的協助工具資訊](basic-accessibility-information.md)。

有些 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) 方法會因為一般協定對於使用者介面自動化提供者報告資訊的預期方式而存在。不過通常不會在控制項對等中實作這些方法。 這是因為資訊其實應該由套用至應用程式程式碼 (在特定 UI 中使用控制項) 的 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) 值提供。 例如，大部分的應用程式會套用 [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) 值，藉此在 UI 中不同的兩個控制項之間定義標籤關係。 不過，[**LabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) 會在代表控制項內資料或項目關係的特定對等中實作，例如使用標頭部分來標示資料欄位部分、在項目上標示它們的容器，或者類似的情況。

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>實作模式  
讓我們看看如何透過實作展開-摺疊的控制項模式介面，為控制項編寫一個可以實作展開-摺疊行為的對等。 只要呼叫 [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) 時搭配 [**PatternInterface.ExpandCollapse**](https://msdn.microsoft.com/library/windows/apps/BR242496) 值，這個對等就會為展開-摺疊行為提供協助工具。 然後，對等應該繼承模式 ([**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider)) 的提供者介面，並為該提供者介面的每一個成員提供實作。 在這種情況下，介面需要覆寫 3 個成員：[**Expand**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand)、[**Collapse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse)、[**ExpandCollapseState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate)。

在類別本身的 API 設計中事先計劃協助工具對您有很大的幫助。 只要您的行為可能是透過在 UI 中進行一般互動的使用者要求所得到的結果，或者是可能透過自動化提供者模式要求，請提供 UI 可以回應或自動化模式可以呼叫的單一方法。 例如，如果您的控制項有一些按鈕組件，它們的連接事件處理常式會展開或摺疊控制項，而且有與這些動作對等的鍵盤功能，請讓這些事件處理常式呼叫您在對等內文 [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/desktop/Ee671242) 中呼叫的 [**Expand**](https://msdn.microsoft.com/library/windows/apps/BR242570) 或 [**Collapse**](https://msdn.microsoft.com/library/windows/apps/BR242569) 實作。 透過通用的邏輯方法同樣也可以確定控制項的視覺狀態會被更新，以便使用統一的方式來顯示邏輯狀態，無論行為的叫用方式為何。

典型的實作是提供者 API 先呼叫 [**Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) 以便在執行階段存取控制項執行個體。 接著即可在該物件上呼叫必要的行為方法。


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

另一個實作是控制項本身可以參考其對等。 從控制項引發自動化事件時，這個實作是常見的模式，因為 [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) 方法是對等方法。

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>UI 自動化事件  

UI 自動化事件分成下列類別。

| 事件 | 說明 |
|-------|-------------|
| 屬性變更 | 當 UI 自動化元素或控制項模式上的屬性變更時觸發。 例如，如果用戶端需要監視應用程式核取方塊控制項，它可以登錄以接聽 [**ToggleState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itoggleprovider.togglestate) 屬性上屬性變更事件。 核取或取消核取核取方塊控制項時，提供者會觸發事件，而用戶端就可以視需要採取動作。 |
| 元素動作 | 因使用者或程式設計活動而讓 UI 變更時觸發；例如，按一下按鈕或透過 **Invoke** 模式叫用按鈕時。 |
| 結構變更 | UI 自動化樹狀目錄的結構變更時觸發。 當新的 UI 項目顯示、隱藏或從桌面上移除時，結構會變更。 |
| 全域變更 | 發生會影響用戶端全域的動作時觸發；如焦點從一個元素移轉至另一個，或子視窗關閉時。 有些事件不一定表示 UI 的狀態變更了。 例如，如果使用者按 Tab 鍵移到文字輸入欄位，然後按一下按鈕來更新欄位，那麼即使使用者沒有實際變更文字，還是會觸發 [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.textchanged) 事件。 處理事件時，用戶端應用程式可能需要先檢查是否有任何實際的變更，然後再採取動作。 |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>AutomationEvents 識別碼  
UI 自動化事件由 [**AutomationEvents**](https://msdn.microsoft.com/library/windows/apps/BR209183) 值加以識別。 列舉的值是唯一識別事件類型的值。

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>引發事件  
UI 自動化用戶端可以訂閱自動化事件。 在自動化對等模型中，自訂控制項的對等必須呼叫 [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) 方法，以報告與協助工具有關的控制項狀態變更。 同樣地，當主要的 UI 自動化屬性值變更時，自訂控制項對等應該呼叫 [**RaisePropertyChangedEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent) 方法。

下一個程式碼範例示範如何從控制項定義程式碼中取得對等物件，然後呼叫方法從該對等觸發事件。 最佳的做法是，程式碼判斷這個事件類型是否有任何接聽程式。 有接聽程式時才觸發事件並建立對等物件，以避免不必要的額外負荷，並協助控制項保持回應狀態。


```csharp
if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}
```

<span id="Peer_navigation"/>
<span id="peer_navigation"/>
<span id="PEER_NAVIGATION"/>

## <a name="peer-navigation"></a>對等瀏覽  
找到自動化對等之後，使用者介面自動化用戶端可以呼叫對等物件的 [**GetChildren**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildren) 和 [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent) 方法，以便瀏覽 app 的對等結構。 藉由對等的 [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) 方法實作，即可支援在控制項內的 UI 元素之間進行瀏覽。 「使用者介面自動化」系統會呼叫這個方法以建立控制項內包含的子元素樹狀目錄；例如，清單方塊中的清單項目。 [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) 中的預設 **GetChildrenCore** 方法會周遊元素的視覺化樹狀結構，以建立自動化對等的樹狀結構。 自訂控制項可以覆寫這個方法以將子元素的不同呈現方式公開給自動化用戶端，進而傳回可用以傳達資訊或允許使用者互動之元素的自動化對等。

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>對文字模式的原生自動化支援  
有些預設的 UWP app 自動化對等可針對文字模式 ([**PatternInterface.Text**](https://msdn.microsoft.com/library/windows/apps/BR242496)) 提供控制項模式支援。 但是它們是透過原生方法提供這項支援，而相關的對等在 (受管理的) 繼承中並不會提及 [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) 介面。 儘管如此，在受管理的或非受管理的使用者介面自動化用戶端針對模式查詢對等時，它仍會回報對文字模式的支援，並在呼叫用戶端 API 時，針對部分模式提供行為。

如果您打算從其中一個 UWP App 文字控制項衍生對等，並且也建立一個衍生自其中一個文字相關對等的自訂對等，請查看對等的＜備註＞小節，以了解有關模式的任何原生層級支援。 您只要從受管理的提供者介面實作呼叫基底實作，就可以存取自訂對等中的原生基礎行為，但是比較不容易修改基底實作的行為，因為對等及其擁有者控制項上的原生介面並未公開。 一般而言，您應該依原樣使用基底實作 (僅呼叫基底)，或是以您自己的 Managed 程式碼完全取代該功能，而不呼叫基底實作。 後者是一個進階案例，您需要非常熟悉控制項所使用的文字服務架構，以便在使用該架構時支援協助工具需求。

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties.AccessibilityView  
除了提供自訂對等，您也可以在 XAML 中設定 [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview)，來調整任何控制項執行個體的樹狀結構檢視表示法。 這不會在對等類別中一併實作，但是我們將在此處提及它，因為它不論是與自訂控制項還是您所自訂之範本的整體協助工具支援，都有密切關係。

**AutomationProperties.AccessibilityView** 的主要適用情況是要刻意將範本中的特定控制項從「使用者介面自動化」檢視中省略，因為它們對整個控制項的協助工具檢視並無任何有意義的貢獻。 為了避免發生這種情況，請將 **AutomationProperties.AccessibilityView** 設為 "Raw"。

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>從自動化對等擲回例外狀況  
您正在為您的自動化對等支援所實作的 API 可以擲回例外狀況。 預期任何負責接聽的使用者介面自動化用戶端都相當健全，足以在擲回大部分的例外狀況之後繼續執行。 該接聽程式極可能正在查看一個包含您自己應用程式以外之應用程式的全啟動自動化樹狀結構，因此，如果只因在用戶端呼叫其 API 時，樹狀結構的一個區域擲回對等型例外狀況，就將整個用戶端關閉，這樣的用戶端設計無法被接受。

對於傳遞到對等中的參數，可接受驗證輸入，例如，如果傳遞的是 **null**，則擲回 [**ArgumentNullException**](https://msdn.microsoft.com/library/windows/apps/system.argumentnullexception)，但這對您的實作來說不是有效值。 不過，如果後續有對等所執行的操作，請記住，對等與裝載控制項的互動具有非同步特性。 對等所執行的任何操作不一定會封鎖控制項中的 UI 執行緒 (而且也不應該封鎖)。 因此，您可能會遇到一些情況，就是在建立對等或第一次呼叫自動化對等方法時，有某個物件可供使用或具有特定屬性，但是在同時，控制項狀態已經變更。 針對這些情況，有兩個專用的例外狀況可供提供者擲回：

* 如果您無法根據傳遞給您 API 的原始資訊來存取對等的擁有者或相關對等元素，請擲回 [**ElementNotAvailableException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotavailableexception)。 例如，您可能有一個對等嘗試執行其方法，但是已經從 UI 移除擁有者，例如已經被關閉的強制回應對話方塊。 以非 .NET 用戶端來說，這會對應至 [**UIA\_E\_ELEMENTNOTAVAILABLE**](https://msdn.microsoft.com/library/windows/desktop/Ee671218)。
* 如果擁有者仍然存在，但是該擁有者處於 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled)`=`**false** 之類的模式，而會封鎖您對等嘗試完成的某些特定程式設計變更，請擲回 [**ElementNotEnabledException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotenabledexception)。 以非 .NET 用戶端來說，這會對應至 [**UIA\_E\_ELEMENTNOTENABLED**](https://msdn.microsoft.com/library/windows/desktop/Ee671218)。

除此之外，就對等從它們的對等支援擲回的例外狀況來說，對等應該相當保守。 大多數用戶端無法處理來自對等的例外狀況，並將這些例外狀況轉換成使用者在與用戶端進行互動時的可動作選項。 因此，與每次對等嘗試執行操作無效時都擲回例外狀況相比，有時無作業和攔截例外狀況而不在對等實作中重新擲回，會是一個較佳的策略。 此外，也請考量大多數使用者介面自動化用戶端都不是以 Managed 程式碼撰寫。 大多數都是以 COM 撰寫，而且只會在每次呼叫使用者介面自動化用戶端方法時檢查 **HRESULT** 中是否有 **S\_OK**，以最終存取您的對等。

<span id="related_topics"/>

## <a name="related-topics"></a>相關主題  
* [協助工具](accessibility.md)
* [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472)
* [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)
* [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer)
* [控制項模式和介面](control-patterns-and-interfaces.md)
