---
author: muhsinking
Description: Learn to write code for a custom Panel class, implementing ArrangeOverride and MeasureOverride methods, and using the Children property.
MS-HAID: dev\_ctrl\_layout\_txt.boxpanel\_example\_custom\_panel
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: BoxPanel，自訂面板範例 (Windows 應用程式)
ms.assetid: 981999DB-81B1-4B9C-A786-3025B62B74D6
label: BoxPanel, an example custom panel
template: detail.hbs
op-migration-status: ready
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7d29b85c7ec3ec9ec0114a3a49dff834f859511e
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2018
ms.locfileid: "6263032"
---
# <a name="boxpanel-an-example-custom-panel"></a>BoxPanel，自訂面板範例

 

學習如何撰寫自訂 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 類別程式碼、實作 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 和 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法，以及使用 [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 屬性。 

> **重要 API**: [**面板**](https://msdn.microsoft.com/library/windows/apps/br227511), [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711), [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 

範例程式碼示範一個自訂面板實作，但對於影響不同配置案例的自訂面板方法的配置概念則未多加說明。 如需有關這些配置概念以及如何才能套用到特定配置案例的詳細資訊，請參閱 [XAML 自訂面板概觀](custom-panels-overview.md)。

「面板」** 是一個物件，可在 XAML 配置系統執行和轉譯 app UI 時，為其所含的子元素提供配置行為。 您可以從 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 類別衍生自訂類別，為 XAML 配置定義自訂面板。 透過覆寫 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 與 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 方法，提供可度量和排列子元素的邏輯，即可提供面板行為。 本範例衍生自 **Panel**。 當您從 **Panel** 開始時，**ArrangeOverride** 和 **MeasureOverride** 方法沒有開始行為。 您的程式碼是提供一個讓 XAML 配置系統知道子元素並在 UI 中轉譯的入口。 因此，您的程式碼務必說明所有子元素，並遵循配置系統預期的模式。

## <a name="your-layout-scenario"></a>您的配置案例

當您定義自訂面板時，其實是在定義配置案例。

配置案例是透過下列各項來表達：

-   面板有子元素時會有什麼行為
-   面板何時會對自己的空間有所限制
-   面板的邏輯如何決定最後產生所呈現之子系 UI 配置的所有度量、放置位置和大小調整

請記住，這裡所示的 `BoxPanel` 僅適用於特定案例。 為了讓本範例以程式碼為主，我們將不會詳細說明案例，而是將焦點放在必要步驟與程式碼撰寫模式。 若您想要先深入了解案例，可以直接跳到[「`BoxPanel` 的案例」](#the-scenario-for-boxpanel)，然後再回來看程式碼。

## <a name="start-by-deriving-from-panel"></a>從 **Panel** 衍生著手

從 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 衍生一個自訂類別著手。 若要這樣做，最簡單的方法可能是從 Microsoft Visual Studio 的 [方案總管]**** 針對專案使用 [加入]**** |  [新增項目]**** | [類別]**** 內容功能表選項，為這個類別定義個別的程式碼檔案。 將類別 (和檔案) 命名為 `BoxPanel`。

因為類別的範本檔案並非專供通用 Windows 平台 (UWP) app 使用，所以一開始不會有許多 **using** 陳述式。 因此，請先新增 **using** 陳述式。 範本檔案開始位置也使用了幾個您可能不需要，且可以刪除的 **using** 陳述式。 以下是建議的 **using** 陳述式清單，可用以解析一般自訂面板程式碼所需的類型：

```CSharp
using System;
using System.Collections.Generic; // if you need to cast IEnumerable for iteration, or define your own collection properties
using Windows.Foundation; // Point, Size, and Rect
using Windows.UI.Xaml; // DependencyObject, UIElement, and FrameworkElement
using Windows.UI.Xaml.Controls; // Panel
using Windows.UI.Xaml.Media; // if you need Brushes or other utilities
```

現在您可以解析 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)，請將它當成`BoxPanel` 的基底類別。 此外，將 `BoxPanel` 公開：

```CSharp
public class BoxPanel : Panel
{
}
```

在類別層級定義一些會由數個邏輯函式共用，但不需要公開為公用 API 的 **int** 與 **double** 值。 在範例中，這些名稱如下：`maxrc`、`rowcount`、`colcount`、`cellwidth`、`cellheight`、`maxcellheight`、`aspectratio`。

當您完成這個動作之後，完整程式碼檔案看起來像這樣 (移除 **using** 上的註解，現在您知道為什麼有這些註解)：

```CSharp
using System;
using System.Collections.Generic;
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

public class BoxPanel : Panel 
{
    int maxrc, rowcount, colcount;
    double cellwidth, cellheight, maxcellheight, aspectratio;
}
```

從現在開始，我們會逐一向您顯示各個成員定義，可能是方法覆寫或支援項目 (例如相依性屬性)。 您能以任何順序將這些加入上述的基本架構，此外，在我們說明最終程式碼之前，將不會再次說明 **using** 陳述式或類別範圍的定義。

## **<a name="measureoverride"></a>MeasureOverride**


```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize;
    // Determine the square that can contain this number of items.
    maxrc = (int)Math.Ceiling(Math.Sqrt(Children.Count));
    // Get an aspect ratio from availableSize, decides whether to trim row or column.
    aspectratio = availableSize.Width / availableSize.Height;

    // Now trim this square down to a rect, many times an entire row or column can be omitted.
    if (aspectratio > 1)
    {
        rowcount = maxrc;
        colcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
    } 
    else 
    {
        rowcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
        colcount = maxrc;
    }

    // Now that we have a column count, divide available horizontal, that's our cell width.
    cellwidth = (int)Math.Floor(availableSize.Width / colcount);
    // Next get a cell height, same logic of dividing available vertical by rowcount.
    cellheight = Double.IsInfinity(availableSize.Height) ? Double.PositiveInfinity : availableSize.Height / rowcount;
           
    foreach (UIElement child in Children)
    {
        child.Measure(new Size(cellwidth, cellheight));
        maxcellheight = (child.DesiredSize.Height > maxcellheight) ? child.DesiredSize.Height : maxcellheight;
    }
    return LimitUnboundedSize(availableSize);
}
```

[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 實作的必要模式是循環顯示 [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 中的每一個元素。 一律在每一個元素上呼叫 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 方法。 **Measure** 有一個 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) 類型的參數。 這裡傳送的是面板可供特定子元素使用的大小。 因此，在您執行迴圈並開始呼叫 **Measure** 之前，必須先知道每個儲存格所能提供的空間。 您可以從 **MeasureOverride** 方法本身得知 *availableSize* 值。 那就是面板的父系呼叫 **Measure** 時使用的大小，它會在呼叫這個 **MeasureOverride** 時立即觸發。 一般邏輯就是制定一個配置，讓每個子元素藉以劃分面板整體 *availableSize* 的空間。 接下來，您可以將所劃分的大小傳遞至每個子元素的 **Measure**。

`BoxPanel` 劃分大小的方式很簡單：它將空間劃分為主要由項目數量控制的一些方塊。 方塊的大小是根據列與欄的計數和可用大小來劃分。 有時候會因不需要方形的其中一列或一欄，而將它捨棄，而使得面板在列與欄的比例上變成矩形而不是方形。 如需有關如何得出這個邏輯的詳細資訊，請直接跳到[BoxPanel 的案例](#the-scenario-for-boxpanel)。

度量階段有何作用？ 它在呼叫 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208921) 的每個元素上設定唯讀 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208952) 屬性的值。 因為步入排列階段之後，**DesiredSize** 會傳達進行排列時和最終轉譯中可有或應有的大小，所以最好能有 **DesiredSize** 值。 即使您自己的邏輯中不會使用 **DesiredSize**，但是系統仍需要它。

還有可能在 *availableSize* 的高度元件為無限時，使用此面板。 若是如此，面板不會有可供劃分的已知高度。 在這種情況下，度量階段的邏輯會通知各個子系，高度尚無界限。 方法是針對 [**Size.Height**](https://msdn.microsoft.com/library/windows/apps/br225995) 為無限的子系，將 [**Size**](https://msdn.microsoft.com/library/windows/apps/br208952) 傳送至 [**Measure**](https://msdn.microsoft.com/library/windows/apps/hh763910) 呼叫。 上述為有效做法。 呼叫 **Measure** 時，邏輯是將 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 設定為下列各項的最小值：傳遞至 **Measure** 的項目，或來自明確設定的 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 與 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 等係數的元素原始大小。

> [!NOTE]
> [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) 的內部邏輯也有這項行為：**StackPanel** 將無限的維度值傳送至子系的 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952)，表示方向維度的子系沒有限制。 **StackPanel** 一般會動態調整本身的大小，以容納堆疊中在該維度不斷增加的所有子系。

不過，面板本身不會從 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br225995) 傳回無限值的 [**Size**](https://msdn.microsoft.com/library/windows/apps/br208730)；造成在配置期間擲回例外狀況。 因此，部分邏輯是要找出任何子系要求的最大高度，並在面板本身的大小限制未提供儲存格高度時，使用該高度做為儲存格高度。 以下是先前程式碼中參照的協助程式函式 `LimitUnboundedSize`，會接受上述的最大儲存格高度，並用它提供面板一個可傳回的有限高度，以及確保在起始排列階段之前，`cellheight` 會是有限數字：

```CSharp
// This method is called only if one of the availableSize dimensions of measure is infinite.
// That can happen to height if the panel is close to the root of main app window.
// In this case, base the height of a cell on the max height from desired size
// and base the height of the panel on that number times the #rows.

Size LimitUnboundedSize(Size input)
{
    if (Double.IsInfinity(input.Height))
    {
        input.Height = maxcellheight * colcount;
        cellheight = maxcellheight;
    }
    return input;
}
```

## **<a name="arrangeoverride"></a>ArrangeOverride**

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
     int count = 1
     double x, y;
     foreach (UIElement child in Children)
     {
          x = (count - 1) % colcount * cellwidth;
          y = ((int)(count - 1) / colcount) * cellheight;
          Point anchorPoint = new Point(x, y);
          child.Arrange(new Rect(anchorPoint, child.DesiredSize));
          count++;
     }
     return finalSize;
}
```

[**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) 實作的必要模式是循環顯示 [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 中的每一個元素。 一律在每一個元素上呼叫 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 方法。

您是否注意到，執行計算的次數不如 [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 中頻繁；一般就是如此。 您已經從面板本身的 **MeasureOverride** 邏輯中，或從在度量階段期間所設定各個子系的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 值中，得知子系的大小。 不過，我們仍然需要決定各個子系在面板內的顯示位置。 在一般面板中，每個子系都應在不同的位置轉譯。 一般案例並不希望有會建立重疊元素的面板 (但如果那確實是您屬意的案例，還是可以建立有目的的重疊面板)。

這個面板依照列與欄的概念來排列。 列數與欄數已計算出來 (度量時的必要資訊)。 現在，列與欄的形狀加上已知的各儲存格大小所建構的邏輯，可為此面板包含的每個元素定義其轉譯位置 (`anchorPoint`)。 使用 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) 和從度量得知的 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)，當成建立 [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) 的兩個元件。 **Rect** 是 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 的輸入類型。

面板有時候需要裁剪內容。 若有必要執行上述動作，裁剪的大小即為 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 所示的大小，因為 [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) 邏輯將它設定為傳遞至 **Measure** 的項目或其他原始大小係數的最小值。 所以您通常不用在 [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) 期間特別檢查裁剪，系統會根據傳遞至每個 **Arrange** 呼叫的 **DesiredSize** 進行裁剪。

如果您可以利用其他方法得知定義轉譯位置所需的所有資訊，就不必在執行迴圈時計數。 例如，在 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 配置邏輯中，[**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 集合中的位置並不重要。 放置 **Canvas** 中每個元素所需的所有資訊都可在排列邏輯中讀取子系的 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 和 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) 值來得知。 `BoxPanel` 邏輯恰巧需要計數以便與 *colcount* 比較，因此在開始新列和位移 *y* 值時，已知該值。

輸入 *finalSize* 和從 [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br225995) 實作傳回的 [**Size**](https://msdn.microsoft.com/library/windows/apps/br208711) 通常會相同。 如需有關原因的詳細資訊，請參閱 [XAML 自訂面板概觀](custom-panels-overview.md)的＜**ArrangeOverride**＞一節。

## <a name="a-refinement-controlling-the-row-vs-column-count"></a>微調：控制列與欄計數

您可以依現況編譯和使用此面板。 不過，我們還要新增一個微調項目。 在剛才所示的程式碼中，邏輯會在外觀比例上最長的一側放置額外的列或欄。 但為了更方便控制儲存格形狀，即使面板本身的外觀比例為「直向」，可能還是需要選擇 4x3 的儲存格組合，而不是 3x4。 因此我們將新增一個選擇性的相依性屬性，供面板使用者設定以控制上述行為。 以下是非常基本的相依性屬性設定：

```CSharp
public static readonly DependencyProperty UseOppositeRCRatioProperty =
   DependencyProperty.Register("UseOppositeRCRatio", typeof(bool), typeof(BoxPanel), null);

public bool UseSquareCells
{
    get { return (bool)GetValue(UseOppositeRCRatioProperty); }
    set { SetValue(UseOppositeRCRatioProperty, value); }
}
```

以下是如何使用 `UseOppositeRCRatio` 影響度量邏輯。 事實上，它只是變更從 `maxrc` 衍生 `rowcount` 與 `colcount` 的方式和實際的外觀比例，因此每個儲存格都有對應的大小差異。 當 `UseOppositeRCRatio` 為 **true** 時，會先反轉實際外觀比例的值，然後才會用於列與欄計數。

```CSharp
if (UseOppositeRCRatio) { aspectratio = 1 / aspectratio;}
```

## <a name="the-scenario-for-boxpanel"></a>BoxPanel 的案例

`BoxPanel` 的特定案例是一個面板，其中如何劃分空間的主要決定因素之一，就是要知道子項目的數目，然後為面板劃分已知的可用空間。 面板原本呈矩形。 許多面板的運作方式是將其矩形空間再劃分成更多矩形；[**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 就是這樣處理其儲存格。 在 **Grid** 的案例中，是透過 [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) 與 [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606) 值來設定儲存格的大小，而且元素利用 [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) 與 [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/hh759774) 附加屬性，明確宣告其所進入的儲存格。 **Grid** 要有美觀的配置，通常需要事先知道子元素的數目，如此才能有足夠的儲存格，每個子元素也才能設定符合本身儲存格的附加屬性。

但如果子系的數目不定怎麼辦？ 上述情況絕對可能發生；App 程式碼可以將項目加入集合，以回應您認為其重要性達到需要更新 UI 的任何動態執行階段狀況。 如果您使用資料繫結支援集合/商業物件，會自動處理這類更新和更新 UI，所以這通常是偏好使用的技術 (請參閱[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946))。

但不是所有的應用程式案例都適合使用資料繫結。 有時您必須在執行階段建立新的 UI，並讓它們顯示。 `BoxPanel`  適用於這個案例。 因為 `BoxPanel` 是在計算中使用子系計數，並依新的配置來調整現有與新的子元素，將它們全部放入，所以數目不定的子項目不會造成問題。

再次擴充 `BoxPanel` 的進階案例 (這裡沒有說明) 可同時容納動態子系，並使用子系的 [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) 作為調整個別資料格大小更強大的因素。 此案例可以使用不同的列或欄大小或非格線形狀，以減少「浪費」的空間。 這樣做需要擬定策略，找出如何將多個不同大小與外觀比例的矩形全部放入包含的矩形 (包含美觀和最小的大小)。 `BoxPanel` 不是這麼做，它是使用較簡單的技術來劃分空間。 `BoxPanel`使用的技術是判斷大於子系計數的最少方形數目。 例如，9 個項目可以放在 3x3 方形。 10 個項目需要 4x4 方形。 不過，通常在移除起始方形的一列或一欄的情況下，還是可以放入項目以節省空間。 在 count=10 範例中，可放入 4x3 或 3x4 矩形。

您可能會疑惑，有 10 個項目的面板為什麼不選擇和項目數目完全符合的 5x2 矩形。 不過在實務上，會將面板的大小調整為較沒有強烈外觀比例的矩形。 採用最小二乘法技術，可讓調整大小邏輯與一般配置形狀搭配使用並運作良好，而且也不鼓勵將儲存格形狀調整為奇特的外觀比例。

## <a name="related-topics"></a>相關主題

**參考**

* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**面板**](https://msdn.microsoft.com/library/windows/apps/br227511)

**概念**

* [對齊、邊界及邊框間距](alignment-margin-padding.md)
