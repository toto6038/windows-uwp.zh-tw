---
author: muhsinking
Description: You can define custom panels for XAML layout by deriving a custom class from the Panel class.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_custom\_panels\_overview
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: XAML 自訂面板概觀
ms.assetid: 0CD395CD-E2AB-429D-BB49-56A71C5CC35D
label: XAML custom panels overview (Windows apps)
template: detail.hbs
op-migration-status: ready
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d9856d564ffd36226a841c38eba65df0b62ee306
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6850925"
---
# <a name="xaml-custom-panels-overview"></a>XAML 自訂面板概觀

 

*「面板」* 是一個物件，可在可延伸應用程式標記語言 (XAML) 配置系統執行和轉譯應用程式 UI 時，為其所含的子元素提供配置行為。 


> **重要 API**: [**面板**](https://msdn.microsoft.com/library/windows/apps/br227511), [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711), [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)

您可以從 [**面板**](https://msdn.microsoft.com/library/windows/apps/br227511) 類別衍生自訂類別，為 XAML 配置定義自訂面板。 透過覆寫 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 與 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)，提供可度量和排列子元素的邏輯，即可提供面板行為。

## <a name="the-panel-base-class"></a>**Panel** 基底類別


您可以直接從 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 類別衍生，或從其中一個未密封的實際面板類別 (例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 或 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)) 衍生，定義自訂面板類別。 因為可能難以解決已有配置行為的現有面板配置邏輯，所以從 **Panel** 衍生會比較容易。 此外，具有行為的面板可能會有和您的面板配置功能不相關的現有屬性。

您的自訂面板會從 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 中繼承以下 API：

-   [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 屬性。
-   [**Background**](https://msdn.microsoft.com/library/windows/apps/br227512)、[**ChildrenTransitions**](https://msdn.microsoft.com/library/windows/apps/br227515) 和 [**IsItemsHost**](https://msdn.microsoft.com/library/windows/apps/br227517) 屬性，以及相依性屬性識別元。 這些屬性都不是虛擬屬性，因此您通常不會覆寫或取代它們。 您的自訂面板案例通常不需要使用這些屬性，甚至也不需要使用它們來讀取值。
-   配置覆寫方法 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 與 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)。 這些原本是由 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 定義。 基底 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 類別不會覆寫這些方法，但 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 這類實際面板確實有覆寫實作，會當成機器碼實作並由系統執行。 為 **ArrangeOverride** 與 **MeasureOverride** 提供新的 (或附加) 實作，是定義自訂面板時需要投入心力的最主要部分。
-   [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)、[**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 及 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) 的所有其他 API，例如 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)、[**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 等。 您有時候會在配置覆寫中參考這些屬性值，但由於並非虛擬屬性，因此您通常不會覆寫或取代它們。

這裡的重點是說明 XAML 配置概念，以便您考慮自訂面板在配置中所有可能和應有的行為。 如果您想要直接進入自訂面板實作範例，請參閱 [BoxPanel，自訂面板範例](boxpanel-example-custom-panel.md)。

## <a name="the-children-property"></a>**Children** 屬性


因為從 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227514) 衍生的所有類別都使用 **Children** 屬性做為儲存集合中所含子元素之處，所以 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227511) 屬性與自訂面板相關。 **Children** 指定為 **Panel** 類別的 XAML 內容屬性，且從 **Panel** 衍生的所有類別都可繼承 XAML 內容屬性行為。 如果將屬性指定為 XAML 內容屬性，表示在標記中指定該屬性時，XAML 標記可以略過屬性元素，並將值設定為直接標記子系 (也就是「內容」)。 例如，若您從 **Panel** 衍生一個稱為 **CustomPanel** 的類別，其中未定義任何新行為，那麼您依然可以使用此標記：

```XAML
<local:CustomPanel>
  <Button Name="button1"/>
  <Button Name="button2"/>
</local:CustomPanel>
```

當 XAML 剖析器讀取此標記時，已知 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 是所有 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 衍生類型的 XAML 內容屬性，因此剖析器會將兩個 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 元素新增到 **Children** 屬性的 [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633) 值。 XAML 內容屬性可以在 UI 定義的 XAML 標記中，協助簡化父系-子系關係。 如需有關 XAML 內容屬性以及剖析 XAML 時如何填入集合屬性的詳細資訊，請參閱 [XAML 語法指南](https://msdn.microsoft.com/library/windows/apps/mt185596)。

維護 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 屬性值的集合類型是 [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633) 類別。 **UIElementCollection** 是一個強型別集合，使用 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 當成其強制的項目類型。 **UIElement** 是由數百個實際 UI 元素類型繼承的基底類型，因此這裡的類型強制會刻意鬆散。 但它確實會強制您不能將 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 當成 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 的直屬子系，且它通常表示只有預期要在 UI 中顯示並參與配置的元素，才會成為 **Panel** 中的子元素。

一般而言，只要依現況使用 [**Children**](https://msdn.microsoft.com/library/windows/apps/br208911) 屬性的特性，自訂面板即可透過 XAML 定義接受任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br227514) 子元素。 在進階案例中，您可以在配置覆寫中重複處理集合時，進一步檢查子元素類型。

除了循環顯示覆寫中的 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 集合之外，您的面板邏輯也可能會受 `Children.Count` 的影響。 您的邏輯可能會有部分根據項目數目來配置空間，而不是根據所需的大小和個別項目的其他特性。

## <a name="overriding-the-layout-methods"></a>覆寫配置方法


配置覆寫方法 ([**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 與 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)) 的基本模型是，逐一查看所有子系，並呼叫每個子元素的特定配置方法。 第一個配置週期會在 XAML 配置系統設定根視窗的視覺化項目時開始。 因為每個父系都會在其子系叫用配置，所以會將配置方法呼叫傳播到應該屬於配置的每一個可能 UI 元素。 XAML 配置中有兩個階段：一個是度量，接著是排列。

來自基底 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br208730) 類別的 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 與 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br227511) 不會有任何內建的配置方法行為。 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 的項目不會自動轉譯為 XAML 視覺化樹狀結構的一部分。 是否要讓配置程序知道項目，由您決定，方法是透過 **MeasureOverride** 與 **ArrangeOverride** 實作內的版面配置階段，在 **Children** 中找到的每個項目上叫用配置方法。

除非您另有繼承腹案，否則不需要在配置覆寫呼叫基底實作。 配置行為的原生方法 (若存在的話) 無論如何都會執行，即使不從覆寫呼叫基底實作，還是會發生原生行為。

在度量階段期間，配置邏輯會在各個子元素上呼叫 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法，以查詢每個子元素所需的大小。 呼叫 **Measure** 方法可建立 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 屬性的值。 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 傳回值就是面板本身所需的大小。

在排列階段期間，子元素的位置與大小是以 x-y 空間來決定，並準備轉譯配置組合。 您的程式碼必須在 [**Children**](https://msdn.microsoft.com/library/windows/apps/br208914) 的每個子元素上呼叫 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br227514)，如此一來，配置系統才能偵測出該元素屬於配置。 **Arrange** 呼叫是組合與轉譯的先驅；它會在提交組合進行轉譯時，通知配置系統要放置該元素的位置。

憑藉許多屬性與值，形成在執行階段運作的配置邏輯。 您可以將配置程序視為沒有子系的元素 (通常是位在 UI 最深處的巢狀元素)，這些是最先完成度量的元素。 這些元素在影響它們所需大小的子元素上沒有任何相依性。 它們可能有自己的所需大小，且在實際進行配置之前，這些就是大小建議。 接著，度量階段會在視覺化樹狀結構逐層向上進行，直到根元素有其度量值並完成所有度量為止。

候選配置必須要能整個顯示在目前的應用程式視窗內，不然會裁剪掉部分 UI。 面板通常是決定裁剪邏輯之處。 面板邏輯能決定 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 實作內可用的大小，且可能必須將大小限制推入子系，並將空間分配給子系，盡可能讓配置盡善盡美。 理想的配置結果是使用配置所有部分的各種不同屬性，但仍然能整個顯示在應用程式視窗內。 這不但需要好的面板配置邏輯實作，還需要在使用該面板建置 UI 的任何應用程式程式碼部分明智審慎的設計 UI。 若整體的 UI 設計包含的子元素過多，而無法全部顯示在應用程式，面板設計就不會美觀。

配置系統得以運作，大部分是因為任何以 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 為基礎的元素，在做為容器的子系時，本身已有一些繼承而來的行為。 例如，**FrameworkElement** 的 API 有些是通知配置行為，也些是配置運作時不可或缺者。 其中包括：

-   [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) (實際為 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 屬性)
-   [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 與 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)
-   [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 與 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)
-   [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)
-   [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 事件
-   [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) 與 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)
-   [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 與 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法
-   [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 與 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法：均已在 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 層級定義原生實作，負責處理元素層級的配置動作。

## **<a name="measureoverride"></a>MeasureOverride**


[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法有一個傳回值，當配置中面板的父系在該面板上呼叫 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208921) 方法時，配置系統使用此值做為面板本身一開始的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208952)。 方法內的邏輯選擇和其傳回值一樣重要，而且邏輯通常會影響傳回的值。

所有 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 實作都應該循環顯示 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)，並在每個子元素上呼叫 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法。 呼叫 **Measure** 方法可建立 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 屬性的值。 這會通知面板本身需要多少空間，以及如何在元素之間劃分這些空間，或如何調整特定子元素的大小。

以下是 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法非常基本的架構：

```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize; //TODO might return availableSize, might do something else
     
    //loop through each Child, call Measure on each
    foreach (UIElement child in Children)
    {
        child.Measure(new Size()); // TODO determine how much space the panel allots for this child, that's what you pass to Measure
        Size childDesiredSize = child.DesiredSize; //TODO determine how the returned Size is influenced by each child's DesiredSize
        //TODO, logic if passed-in Size and net DesiredSize are different, does that matter?
    }
    return returnSize;
}
```

元素在準備好進行配置時，通常是具有原始大小。 如果您為 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208921) 傳送的 *availableSize* 較小，完成度量階段之後，[**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208952) 可能會指出該原始大小。 如果原始大小大於您為 **Measure** 傳送的 *availableSize*，**DesiredSize** 就會受限於 *availableSize*。 這就是 **Measure** 的內部實作行為，您的配置覆寫應該將該行為納入考量。

有些元素沒有原始大小，因為它們的 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 的值為 **Auto**。 因為 **Auto** 值代表將元素的大小調整為最大可用大小 (當前的配置父系利用 *availableSize* 呼叫 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 來傳達)，所以這些元素使用整個 *availableSize*。 實際上，UI 一律會根據某個度量值來調整大小，即使是最上層視窗也是如此。最後，度量階段會將所有 **Auto** 值解析成父系限制，而所有 **Auto** 值元素都會取得實際度量值 (您可以在配置完成後，查看 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 與 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 來取得)。

您可以將大小傳送到至少有一個無限維度的 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)，表示該面板能嘗試調整本身的大小，以容納內容的大小。 每個接受測量的子元素會用它的原始大小設定 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值。 然後，在排列階段期間，面板通常會使用該大小進行排列。

即使未設定 [**Height**](https://msdn.microsoft.com/library/windows/apps/br209652) 或 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208709) 值，還是可以根據文字元素 (例如 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br208707)) 的文字字串與文字屬性計算出 [**ActualWidth**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 與 [**ActualHeight**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)，而且面板邏輯應要遵守這些維度。 裁剪的文字是特別不好的 UI 體驗。

即使實作未使用所需的大小度量，最好是在每個子元素上呼叫 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法，因為呼叫 **Measure** 時會觸發內部和原生行為。 在度量階段期間，務必要在每個子元素上呼叫 **Measure**，並在排列階段期間於元素上呼叫 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 方法，如此一來，該元素才能參與配置。 呼叫這些方法會在物件上設定內部旗標以及填入值 (例如 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 屬性)，這些值是建置視覺化樹狀結構和轉譯 UI 時，系統的配置邏輯所需的值。

在 [**Children**](https://msdn.microsoft.com/library/windows/apps/br208730) 中的每個子元素上呼叫 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208921) 時，會根據面板的邏輯解譯 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br227514) 或其他大小考量，而得到 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208952) 傳回值。 要怎樣使用來自子系的 **DesiredSize** 值，以及 **MeasureOverride** 傳回值應如何使用它們，完全取決於自訂邏輯的解譯。 因為 **MeasureOverride** 的輸入通常是面板父系建議的固定可用大小，所以您通常會經過修改後，才將值加總。 如果超過該大小，可能會裁剪面板本身。 您通常會比較子系的總大小和面板的可用大小，然後視需要加以調整。

### <a name="tips-and-guidance"></a>提示和指導方針

-   在理想的情況下，自訂面板應該適合當成 UI 組合中的第一個實際視覺化項目，可能位於 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)、[**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) 或另一個 XAML 頁面根目錄元素的直接下層。 在 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 實作中，不要經常未先檢查值便傳回輸入 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)。 如果傳回 **Size** 中有 **Infinity** 值，可能會在執行階段配置邏輯擲回例外狀況。 **Infinity** 值可能來自可捲動的主應用程式視窗，因此不會有最大高度。 其他可捲動的內容也可能有相同的行為。
-   [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 實作的另一個常見錯誤是傳回新的預設 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) (高度與寬度的值都是 0)。 您可以從那個值開始，而且如果面板判斷沒有任何應該轉譯的子系，它就可能是正確的值。 但是預設的 **Size** 會造成主機無法正確地調整您面板的大小。 它不會要求 UI 中的任何空間，因此不會取得空間也不會進行轉譯。 除此之外，您的所有面板程式碼可能都運作正常，但由於組成高度、寬度皆為零，因此您仍將看不到自己的面板或內容。
-   在覆寫內，避免將子元素轉換成 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 和使用計算為配置結果的屬性，特別是 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 與 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)。 對於最常見的案例，您可以讓邏輯以子系的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值為基礎，如此您將不需要任何與 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 或 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 相關的子元素屬性。 對於您知道元素類型且有其他資訊 (例如，影像檔的原始大小) 的特殊化案例，則可以使用元素的特殊化資訊，因為它不是配置系統主動更改的值。 配置邏輯中若包含配置計算的屬性，會大幅增加無意中定義配置迴圈的風險。 這些迴圈會造成無法建立有效配置，且系統會在迴圈無法復原時擲回 [**LayoutCycleException**](https://msdn.microsoft.com/library/windows/apps/hh673799)。
-   面板通常會將其可用空間劃分給多個子元素，雖然實際上劃分空間的方式不盡相同。 例如，[**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 實作的配置邏輯是使用其 [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606) 與 [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) 值，將空間分配給 **Grid** 儲存格，同時支援比例縮放與像素值。 如果是像素值，即已知各個子系可用的空間，所以就是傳送為格線樣式 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 的輸入大小。
-   面板本身可為項目間的邊框間距導入保留空間。 如果這麼做，務必將度量公開為不同於 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 或任何 **Padding** 屬性的屬性。
-   根據先前的版面配置階段，元素可能會有 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 與 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 屬性值。 如果值有所變更且有特殊邏輯要執行，應用程式 UI 程式碼可以在元素上放置 [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 的處理常式，但面板邏輯通常不需要利用事件處理來檢查變更。 因為與配置相關的屬性值已經變更，而且在適當情況下自動呼叫面板的 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 或 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)，所以配置系統已經決定何時重新執行配置。

## **<a name="arrangeoverride"></a>ArrangeOverride**


[**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 方法有一個 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) 傳回值，當配置中面板的父系在該面板上呼叫 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 方法時，配置系統會在轉譯面板本身時使用此值。 輸入 *finalSize* 和 **ArrangeOverride** 傳回的 **Size** 通常會相同。 若非如此，則表示該面板嘗試建立不同於配置宣告中其他參與者可用的大小。 最終大小是以先前透過面板程式碼執行配置的度量階段為基礎，因此通常不會傳回不同的大小：這表示您刻意忽略度量邏輯。

不要傳回含有 **Infinity** 元件的 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)。 嘗試使用這類 **Size** 會在內部配置擲回例外狀況。

所有 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 實作都應該循環顯示 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514)，並在每個子元素上呼叫 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 方法。 與 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 一樣，**Arrange** 沒有傳回值。 和 **Measure** 不同的是，不會將計算的屬性設為結果 (不過，有問題的元素通常會觸發 [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 事件)。

以下是 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 方法非常基本的架構：

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
    //loop through each Child, call Arrange on each
    foreach (UIElement child in Children)
    {
        Point anchorPoint = new Point(); //TODO more logic for topleft corner placement in your panel
       // for this child, and based on finalSize or other internal state of your panel
        child.Arrange(new Rect(anchorPoint, child.DesiredSize)); //OR, set a different Size 
    }
    return finalSize; //OR, return a different Size, but that's rare
}
```

沒有進行度量階段也能進行配置的排列階段。 不過，只限於配置系統已經判斷沒有任何屬性變更會影響先前度量的情況。 例如，若對齊方式改變，並不需要重新度量特定元素，因為它的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 不會隨著對齊選項改變而變化。 另一方面，如果配置中任何元素的 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 有所變更，則需要進行新的度量階段。 配置系統會自動偵測實際的度量變更，並再次叫用度量階段，然後執行另一個排列階段。

[**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 的輸入採用 [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) 值。 建構這個 **Rect** 最常見的方法是使用有 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 輸入與 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) 輸入的建構函式。 **Point** 是應該放置元素週框方塊左上角的點。 **Size** 是用來轉譯該特定元素的維度。 您通常會將該元素的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 當成這個 **Size** 值使用，因為針對配置中涉及的所有元素建立 **DesiredSize** 就是配置的度量階段的目的。 (度量階段會以反覆進行的方式決定全部的元素大小，讓配置系統在進入排列階段時能夠以最佳的方式放置元素)。

[**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 實作之間的差異通常在於面板用來判斷 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 元件如何排列每一子系的邏輯。 絕對位置面板 (例如 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)) 使用透過 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 與 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) 值，從每個元素取得的明確位置資訊。 空間劃分面板 (例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)) 有將可用空間劃分給儲存格的數學運算，而每個儲存格都會有表明應在何處放置和排列其內容的 x-y 值。 彈性面板 (例如 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)) 可能會自行擴充，以容納其方向維度中的內容。

除了您直接控制和傳送給 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 的項目之外，還有其他配置元素的位置影響。 這些來自於 **Arrange** 的內部原生實作，常見於所有 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 衍生類型，並可透過一些其他類型 (例如文字元素) 來增強。 例如，有些元素有邊界和對齊，有些則有邊框間距。 這些屬性通常會互相影響。 如需詳細資訊，請參閱[對齊、邊界及邊框間距](alignment-margin-padding.md)。

## <a name="panels-and-controls"></a>面板與控制項


避免將功能放入應該建立為自訂控制項的自訂面板。 面板的角色是呈現其中的所有子元素內容，做為自動執行的配置功能。 面板可以增加內容裝飾 (方法類似於 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 在呈現的元素周圍加上框線)，或執行其他與配置相關的調整，例如邊框間距。 但除了報告和使用來自子系的資訊，在擴充視覺化樹狀結構輸出時，這大約就是您所進行的工作。

如果有使用者可以存取的任何互動，您應撰寫自訂控制項，而不是面板。 例如，即使為了防止發生裁剪，面板也不應該針對其呈現的內容新增捲動檢視區，因為捲軸、縮圖等都屬於互動控制項 (內容可能會有捲軸，但您應該將它留給子邏輯決定。 不要在配置作業新增捲動，強制放置捲軸)。您可以建立控制項，同時撰寫自訂面板，當呈現控制項中的內容時，在該控制項的視覺化樹狀結構中扮演重要角色。 但控制項與面板應該是不同的程式碼物件。

區別控制項和面板一個很重要的原因是 Microsoft 使用者介面自動化與協助工具。 面板提供視覺配置行為，但不提供邏輯行為。 UI 元素的視覺顯示方式與 UI 的關係不大，通常對協助工具案例很重要。 協助工具的目的是公開在邏輯上對了解 UI 很重要的應用程式部分。 需要互動時，控制項應向使用者介面自動化基礎結構公開互動可能性。 如需詳細資訊，請參閱[自訂自動化對等](https://msdn.microsoft.com/library/windows/apps/mt297667)。

## <a name="other-layout-api"></a>其他配置 API


配置系統中還有一些其他 API，但並非由 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 宣告。 您可以將這些用於面板實作，或用於使用面板的自訂控制項。

-   [**UpdateLayout**](https://msdn.microsoft.com/library/windows/apps/br208989)、[**InvalidateMeasure**](https://msdn.microsoft.com/library/windows/apps/br208930) 及 [**InvalidateArrange**](https://msdn.microsoft.com/library/windows/apps/br208929) 是起始版面配置階段的方法。 **InvalidateArrange** 可能不會觸發度量階段，但其他兩種會。 永遠不要從配置方法覆寫內呼叫這些方法，因為幾乎可以確定它們會造成配置迴圈。 控制項程式碼通常也不需要呼叫它們。 大部分配置層面會藉由偵測架構定義的配置屬性 (例如 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 等) 的變更而自動觸發。
-   [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) 是元素的一些配置層面變更時所觸發的事件。 這並非面板特有；此事件是由 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 所定義。
-   [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br208742) 是只在版面配置階段完成後，並指出 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 或 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 因此有所變更時才會觸發的事件。 這是另一個 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 事件。 有時候會觸發 [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722)，但不會觸發 **SizeChanged**。 例如，內部內容可能已重新排列，但元素大小保持不變。


## <a name="related-topics"></a>相關主題

**參考**
* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**面板**](https://msdn.microsoft.com/library/windows/apps/br227511)

**概念**
* [對齊、邊界及邊框間距](alignment-margin-padding.md)
