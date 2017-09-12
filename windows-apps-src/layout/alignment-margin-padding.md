---
author: Jwmsft
Description: "使用對齊、邊界及邊框間距來改變頁面上元素的版面配置"
title: "通用 Windows 平台 (UWP) app 的對齊、邊界及邊框間距"
ms.assetid: 9412ABD4-3674-4865-B07D-64C7C26E4842
label: Alignment, margin, and padding
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 2023959126195116cf05443070326fddd512425b
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2017
---
# <a name="alignment-margin-and-padding"></a>對齊、邊界及邊框間距

除了維度屬性 (寬度、高度及限制) 之外，元素還可以有對齊、邊界及邊框間距屬性，這些屬性會在元素進行版面配置階段並在 UI 中轉譯時影響配置行為。 對齊、邊界、邊框間距及維度等屬性之間具有關係，這些關係在放置 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 物件時具有一種典型的邏輯流程，以致於會視情況有時使用這些值，有時忽略這些值。

## <a name="alignment-properties"></a>對齊屬性

[**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) 與 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749) 屬性描述應該如何在父元素分配的配置空間內放置子元素。 同時使用這些屬性，容器的配置邏輯便能夠將子元素放在容器內 (面板或控制項)。 對齊屬性的用途是向調適型配置容器提示所需的配置，因此基本上是在 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 子系上設定，並由另一個 **FrameworkElement** 容器父系來解譯。 對齊值可以指定元素要與哪個方向的邊緣對齊，或是置中對齊。 不過，兩個對齊屬性的預設值都是 **Stretch**。 採用 **Stretch** 對齊，元素會填滿提供給它們的配置空間。 **Stretch** 是預設值，可在沒有明確度量值或配置的度量階段未提供任何 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值的情況下，較容易使用調適型配置技術。 使用此預設時，不會有明確高度/寬度無法放進容器內而被裁剪 (除非調整每個容器大小) 的風險。

> [!NOTE]
> 一般的配置原則是，最好只對特定主要元素套用度量值，並讓其他元素使用調適型配置行為。 這樣可隨時在使用者調整最上層應用程式視窗時，提供彈性的配置行為。

 
如果彈性容器內有 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 與 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 值或裁剪的情況，即使將 **Stretch** 設定為對齊值，配置仍是由其容器的行為控制。 在面板中，已經被 **Height** 與 **Width** 去除的 **Stretch** 值，即有如 **Center** 值。

如果沒有原有或經過計算的高度與寬度值，這些維度值在數學上來說是 **NaN** (非數字)。 元素會等待其配置容器提供維度。 執行配置之後，使用 **Stretch** 對齊的元素將取得 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 與 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 屬性的值。 子元素的 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 與 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 中會繼續保留 **NaN** 值，讓調適型行為能夠再次執行，例如，當與配置相關的變更 (例如調整應用程式視窗大小) 導致進入另一個配置循環的時候。

文字元素 (例如 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)) 通常不會有明確宣告的寬度，但是會有經過計算的寬度，您可以利用 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 來查詢，而該寬度同樣會取消 **Stretch** 對齊。 ([**FontSize**](https://msdn.microsoft.com/library/windows/apps/br209657) 屬性與其他文字屬性以及文字本身都已提示預定的配置大小。 您通常不會希望延展文字。) 在控制項內當成內容使用的文字有相同的效果；需要呈現的文字會導致 **ActualWidth** 的計算，而這也會將所需的寬度和大小轉換為包含的控制項。 文字元素也會有根據每行字型大小、換行及其他文字屬性決定的 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)。

[**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 這類面板已有其他配置邏輯 (列與欄定義，以及在元素上設定的附加屬性，例如 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795)，用來指出要在哪個儲存格中繪製)。 在此情況下，對齊屬性會影響該儲存格區域內的內容對齊方式，但儲存格結構與大小是由 **Grid** 上的設定來控制。

項目控制項有時會顯示項目基底類型為資料的項目。 這種情況與 [**ItemsPresenter**](https://msdn.microsoft.com/library/windows/apps/br242843) 有關。 雖然資料本身不是 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 衍生類型，但 **ItemsPresenter** 卻是，所以您可以為展示器設定 [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) 和 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)，且該對齊可套用到呈現在項目控制項中的資料項目。

當上層配置容器的維度中有額外的可用空間時，對齊屬性才有相關。 如果配置容器已經裁剪內容，對齊只會影響將套用裁剪的元素區域。 例如，若您設定 `HorizontalAlignment="Left"`，會裁剪元素右邊的大小。

## <a name="margin"></a>邊界

[**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 屬性描述配置情況中某元素與其對等元素之間的距離，還有某元素與內含該元素的容器內容區域之間的距離。 如果您將元素視為週框方塊或矩形，而其維度為 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 與 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)，**Margin** 配置適用於該矩形外部的區域，而且不會將像素新增到 **ActualHeight** 與 **ActualWidth**。 用於點擊測試與執行輸入事件時，同樣不會將邊界視為元素的一部分。

在一般配置行為中，[**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 值的元件是最後受限制的，並且只有在 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 與 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 已經受限制直到為 0 之後，這些元件才會受限制。 因此，在容器已經裁剪或限制元素的情況下，請謹慎處理邊界；否則邊界可能會是造成元素無法轉譯的原因 (因為在套用邊界之後，其中一個維度已經限制為 0)。

使用 `Margin="20"` 這類語法，可以讓邊界值一致。 使用此語法時，會對元素一致套用 20 像素的邊界，左、上、右、下的邊界都是 20 像素。 邊界值也可以是四個不同的值，每個值都描述一個不同的邊界，以左、上、右、下的順序套用。 例如，`Margin="0,10,5,25"`。 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 屬性的基礎類型是 [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) 結構，此結構擁有將 [**Left**](https://msdn.microsoft.com/library/windows/apps/hh673893)、[**Top**](https://msdn.microsoft.com/library/windows/apps/hh673840)、[**Right**](https://msdn.microsoft.com/library/windows/apps/hh673881) 及 [**Bottom**](https://msdn.microsoft.com/library/windows/apps/hh673775) 值儲存為個別 **Double** 值的屬性。

邊界可以累加。 例如，若兩個元素各指定一致的邊界為 10 個像素，而兩者是方向不拘的相鄰對等元素，則元素之間的距離會是 20 個像素。

允許使用負值邊界。 不過，使用負值邊界經常會造成裁剪或過度繪製對等元素，因此負值邊界並不是常用的技術。

適當使用 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 屬性可以非常精確地控制元素的轉譯位置，以及其相鄰元素與子系的轉譯位置。 當您在 Visual Studio 的 XAML 設計工具內使用拖曳到位置元素的元素時，將看到修改的 XAML 通常會有該元素的 **Margin** 值，用來將您的位置變更序列化回 XAML 中。

[**Block**](https://msdn.microsoft.com/library/windows/apps/br244379) 類別是 [**Paragraph**](https://msdn.microsoft.com/library/windows/apps/br244503) 的基底類別，也有 [**Margin**](https://msdn.microsoft.com/library/windows/apps/jj191725) 屬性。 它在 **Paragraph** 的上層容器 (通常是 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/br227565) 或 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 物件) 內的放置方式，以及如何將多個段落放在與 [**RichTextBlock.Blocks**](https://msdn.microsoft.com/library/windows/apps/br244347) 集合的其他 **Block** 對等項目相關的位置，有類似效果。

## <a name="padding"></a>邊框間距

**Padding** 屬性描述某元素與任何子元素或其中所含內容之間的距離。 如果元素允許有多個子系，則會將內容視為可加入所有內容的單一週框方塊。 例如，如果有一個包含兩個項目的 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803)，就會在包含這些項目的週框方塊周圍套用 [**Padding**](https://msdn.microsoft.com/library/windows/apps/br209459)。 就該容器的度量與排列階段計算而言，**Padding** 會從可用的大小中進行扣除，並且當容器本身經歷其容器的版面配置階段時，會成為所需大小值的一部分。 和 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) 不同，**Padding** 不是 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 的屬性，而且實際上，有數個類別都定義自己專屬的 **Padding** 屬性：

-   [**Control.Padding**](https://msdn.microsoft.com/library/windows/apps/br209459)：繼承所有 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 衍生類別。 不是所有控制項都有內容，因此對於某些控制項而言 (例如 [**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/dn279268))，沒有必要設定屬性。 如果控制項有邊框 (請參閱 [**Control.BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209399))，將會在該邊框內套用邊框間距。
-   [**Border.Padding**](https://msdn.microsoft.com/library/windows/apps/br209263)：定義 [**BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209256)/[**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209254) 所建立的矩形線條與 [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258) 元素之間的空間。
-   [**ItemsPresenter.Padding**](https://msdn.microsoft.com/library/windows/apps/hh968021)：影響針對項目控制項中的項目產生的視覺化外觀，在每個項目周圍放上指定的邊框間距。
-   [**TextBlock.Padding**](https://msdn.microsoft.com/library/windows/apps/br209673) 與 [**RichTextBlock.Padding**](https://msdn.microsoft.com/library/windows/apps/br227596)：展開文字元素的文字周圍的週框方塊。 這些文字元素沒有 **Background**，所以與文字元素的容器所套用的其他配置行為相較之下，不容易看出文字的邊框間距。 因此很少使用文字元素邊框間距，而是較常在包含的 [**Block**](https://msdn.microsoft.com/library/windows/apps/jj191725) 容器使用 [**Margin**](https://msdn.microsoft.com/library/windows/apps/br244379) 設定 (對 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/br227565) 來說)。

不論上述哪一種情況，相同的元素也具有 **Margin** 屬性。 若同時套用邊界與邊框間距，則是兩者相加，也就是說，外部容器與任何內部內容之間的外觀距離等於邊界加上邊框間距。 如果對內容、元素或容器套用不同的背景值，就有可能在轉譯中看到邊界結束與邊框間距開始的交界處。

## <a name="dimensions-height-width"></a>維度 (Height、Width)

[**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208718) 的 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208751) 與 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208706) 屬性通常會在進行版面配置階段時，影響對齊、邊界及邊框間距屬性的行為。 特別是，**Height** 與 **Width** 值的實際數字會取消 **Stretch** 對齊，還會升級成可能元件的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值，該值是在進行配置的度量階段時建立。 **Height** 與 **Width** 都有限制屬性：**Height** 值可以利用 [**MinHeight**](https://msdn.microsoft.com/library/windows/apps/br208731) 與 [**MaxHeight**](https://msdn.microsoft.com/library/windows/apps/br208726) 來限制，而 **Width** 值可以利用 [**MinWidth**](https://msdn.microsoft.com/library/windows/apps/br208733) 與 [**MaxWidth**](https://msdn.microsoft.com/library/windows/apps/br208728) 來限制。 此外，[**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 與 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 都是經過計算的唯讀屬性，只在版面配置階段完成之後才會包含有效值。 如需有關維度與限制或經過計算的屬性如何相互關聯的詳細資訊，請參閱 [**FrameworkElement.Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 與 [**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 中的＜備註＞。

## <a name="related-topics"></a>相關主題

**參考**

* [**FrameworkElement.Height**](https://msdn.microsoft.com/library/windows/apps/br208718)
* [**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751)
* [**FrameworkElement.HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720)
* [**FrameworkElement.VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)
* [**FrameworkElement.Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)
* [**Control.Padding**](https://msdn.microsoft.com/library/windows/apps/br209459)
