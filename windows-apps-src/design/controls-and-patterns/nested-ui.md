---
Description: Use nested UI to enable multiple actions on a list item
title: 清單項目中的巢狀 UI
label: Nested UI in list items
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 60a29717-56f2-4388-a9ff-0098e34d5896
pm-contact: chigy
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8edb38b8ae91d836e283a8eb37830850bf504db4
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8464118"
---
# <a name="nested-ui-in-list-items"></a>清單項目中的巢狀 UI

 

巢狀 UI 是一種使用者介面 (UI)，能夠公開包含在可同時接受獨立焦點之容器內的巢狀可動作控制項。

您可以使用巢狀 UI 來向使用者顯示其他可協助加快執行重要動作的選項。 不過公開越多動作，將會使 UI 變得越加複雜。 請務必在選擇使用此 UI 模式時額外注意此狀況。 本文提供可協助您針對您特定的 UI 判斷最佳動作的指導方針。

> **重要 API**：[ListView 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)、[GridView 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)

在本文中，我們將會針對在 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 和 [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) 項目中建立巢狀 UI 進行討論。 雖然本節不會討論其他的巢狀 UI 情況，但這些概念都是共通的。 在您開始前，請先熟悉在 UI 中使用 ListView 或 GridView 控制項的一般指導方針，這些指導方針可以在[清單](lists.md)和[清單檢視和方格檢視](listview-and-gridview.md)文章中找到。

在本文中，我們所使用的「清單」**、「清單項目」** 及「巢狀 UI」** 等詞彙的定義如下：
- 「清單」** 指的是包含在清單檢視或方格檢視中的項目集合。
- 「清單項目」** 指的是清單上使用者可以採取動作的個別項目。
- 「巢狀 UI」** 指的是位於清單項目內，可以獨立於項目清單本身讓使用者採取動作的 UI 元素。

![巢狀 UI 組件](images/nested-ui-example-1.png)

> 注意&nbsp;&nbsp; ListView 和 GridView 都是衍生自 [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx) 類別，因此它們具有相同功能，但會以不同方式顯示資料。 在本文中，當我們討論清單時，其中的資訊將同時適用於 ListView 和 GridView 控制項。

## <a name="primary-and-secondary-actions"></a>主要和次要動作

建立具有清單的 UI 時，請考慮使用者會針對那些清單項目所採取的動作。  

- 使用者可以按一下項目來執行動作嗎？
    - 一般而言，按一下清單項目會起始一項動作，但那並非是絕對的。
- 使用者可能採取的動作是否超過一項？
    - 例如，點選清單中的電子郵件將會開啟該封電子郵件。 不過，使用者可能會有其他想在不先開啟電子郵件的情況下執行的動作，例如刪除該封電子郵件。 如果使用者能夠直接在清單中存取此動作，將會非常有幫助。
- 動作應該以何種方式向使用者公開？
    - 請考慮到所有輸入類型。 某些巢狀 UI 的形式可能很適合某種輸入方法，但卻無法搭配其他方法使用。  

「主要動作」** 為使用者按下清單項目時所預期發生的情況。

「次要動作」** 通常是與清單項目相關聯的加速器。 這些加速器可以是用於清單管理，或是與清單項目相關的動作。

## <a name="options-for-secondary-actions"></a>次要動作的選項

建立清單 UI 時，請先確定您已考慮到所有 UWP 所支援的輸入方法。 如需不同輸入種類的詳細資訊，請參閱[輸入基本資訊](../input/index.md)。

當您確定 App 已支援所有 UWP 所支援的輸入之後，您應該決定該 App 的次要動作是否具有足夠的重要性，需要在主要清單中公開為加速器。 請記得，公開越多動作，將會使 UI 變得越加複雜。 您是否真的需要在主要清單 UI 中公開次要動作？還是可以將它們放到別處？

如果有其他動作是所有輸入隨時都需要使用的，您可以考慮在主要清單 UI 中公開那些動作。

如果您決定不需要在主要清單 UI 中放置次要動作，還有數種其他方法可以將它們公開給使用者。 以下為一些可以考慮放置次要動作的位置選項。

### <a name="put-secondary-actions-on-the-detail-page"></a>將次要動作放在詳細資料頁面上

將次要動作放在按下清單項目後會瀏覽到的頁面上。 當您使用主要/詳細資料模式時，詳細資料頁面通常是個放置次要動作的好位置。

如需詳細資訊，請參閱[主要/詳細資料模式](master-details.md)。

### <a name="put-secondary-actions-in-a-context-menu"></a>將次要動作放在操作功能表中

將次要動作放在使用者能以滑鼠右鍵按一下或是長按來存取的操作功能表中。 這能在不需要載入詳細資料頁面的情況下，讓使用者可以執行動作 (例如刪除電子郵件)。 同時在詳細資料頁面上提供這些選項是一個良好的做法，因為操作功能表的目的是做為加速器，而非主要 UI。

如果要在輸入是來自控制器或遙控器的情況下公開次要動作，我們建議您使用操作功能表。

如需詳細資訊，請參閱[操作功能表和飛出視窗](menus.md)。

### <a name="put-secondary-actions-in-hover-ui-to-optimize-for-pointer-input"></a>將次要動作放在暫留 UI 中以針對游標輸入最佳化

如果您預期 App 經常會搭配游標輸入 (例如滑鼠和手寫筆) 使用，並想要使次要動作可以方便提供那些輸入使用，您可以只在暫留上顯示次要動作。 此加速器只會在使用游標輸入時可見，因此請務必同時使用其他選項來支援其他輸入類型。

![於暫留上顯示的巢狀 UI](images/nested-ui-hover.png)


如需詳細資訊，請參閱[互動](../input/mouse-interactions.md)。

## <a name="ui-placement-for-primary-and-secondary-actions"></a>主要和次要動作的 UI 放置

如果您決定在主要清單 UI 中公開次要動作，我們建議您遵循下列指導方針。

當您建立具有主要和次要動作的清單項目時，請將主要動作放在左方，並將次要動作放在右方。 對於閱讀方式是從左到右的文化來說，使用者會將位於清單項目左方的動作視為主要動做。

在這些範例中，我們所討論的清單 UI，其項目主要會以水平方向做為流向 (其寬度大於其高度)。 不過，您可能會有更加方正，或是高度大於寬度的清單項目。 通常這些項目會用於方格中。 針對這些項目，如果清單不會垂直捲動，您可以將次要動作放在清單項目的下方，而非清單項目的右方。

## <a name="consider-all-inputs"></a>請考慮到所有輸入

當您決定使用巢狀 UI 時，請同時評估所有輸入類型的使用者體驗。 如先前所述，巢狀 UI 很適合某些輸入類型。 不過它並不一定可以搭配其他類型使用。 鍵盤、控制器及遙控輸入特別可能在使用巢狀 UI 元素時遇到困難。 請務必遵循下列指導方針，以確保您的 UWP 能夠搭配所有輸入類型使用。

## <a name="nested-ui-handling"></a>巢狀 UI 處理

當您有超過一個動作巢狀於清單項目中時，我們建議使用本指導方針來處理使用鍵盤、控制器、遙控器或其他非游標輸入的瀏覽。

### <a name="nested-ui-where-list-items-perform-an-action"></a>清單項目會執行動作的巢狀 UI

如果您具有巢狀元素的清單 UI 支援叫用、選取 (單一或多個) 或拖放作業等動作，我們建議使用下列方向鍵技巧來瀏覽您的巢狀 UI 元素。

![巢狀 UI 組件](images/nested-ui-navigation.png)

**控制器**

當輸入是來自控制器時，將會提供此使用者體驗：

- 如果焦點位於 **A** 上，右方向鍵會將焦點放到 **B** 上。
- 如果焦點位於 **B** 上 ，右方向鍵會將焦點放到 **C** 上。
- 如果焦點位於 **C** 上，右方向鍵會沒有選項，或是在清單右方具有可設定焦點的元素時，將焦點放到那裡。
- 如果焦點位於 **C** 上，左方向鍵會將焦點放到 **B** 上。
- 如果焦點位於 **B** 上，左方向鍵會將焦點放到 **A** 上。
- 如果焦點位於 **A** 上，左方向鍵會沒有選項，或是在清單左方具有可設定焦點的元素時，將焦點放到那裡。
- 如果焦點位於 **A**、**B** 或 **C** 上，下方向鍵會將焦點放到 **D** 上。
- 如果焦點位於清單項目左方的 UI 元素上，右方向鍵會將焦點放到 **A** 上。
- 如果焦點位於清單項目右方的 UI 元素上，左方向鍵會將焦點放到 **A** 上。

**鍵盤**

當輸入是來自鍵盤時，以下將是使用者會獲得的體驗：

- 如果焦點位於 **A** 上，Tab 鍵會將焦點放到 **B** 上。
- 如果焦點位於 **B** 上，Tab 鍵會將焦點放到 **C** 上。
- 如果焦點位於 **C** 上，Tab 鍵會將焦點放到定位順序中的下一個可設定焦點 UI 元素。
- 如果焦點位於 **C** 上，Shift+Tab 鍵會將焦點放到 **B** 上。
- 如果焦點位於 **B** 上，Shift+Tab 或向左鍵會將焦點放到 **A** 上。
- 如果焦點位於 **A** 上，Shift+Tab 鍵會將焦點放到反向定位順序中的下一個可設定焦點 UI 元素。
- 如果焦點位於 **A**、**B** 或 **C** 上，向下鍵會將焦點放到 **D** 上。
- 如果焦點位於清單項目左方的 UI 元素上，Tab 鍵會將焦點放到 **A** 上。
- 如果焦點位於清單項目右方的 UI 元素上，Shift+Tab 鍵會將焦點放到 **C** 上。

如果要達成此 UI，請在清單上將 [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) 設為 **true**。 [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) 可以是任何值。

如需實作此項目的程式碼，請參閱本文的[範例](#example)一節。

### <a name="nested-ui-where-list-items-do-not-perform-an-action"></a>清單項目不會執行動作的巢狀 UI

您使用清單檢視的原因，可能是因為它能提供虛擬化並最佳化捲動行為，因此您不會將某個動作與清單項目做出關聯。 這類 UI 通常只會針對群組元素使用清單項目，並確保它們會以單一的集合進行捲動。

這種 UI 通常會比先前的範例更加複雜，並具有許多使用者可採取動作的巢狀元素。

![巢狀 UI 組件](images/nested-ui-grouping.png)


如果要達成此 UI，請在清單上設定下列屬性：
- 將 [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) 設為 **None**。
- 將 [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) 設為 **false**。
- 將 [IsFocusEngagementEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isfocusengagementenabled.aspx) 設為 **true**。

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="False" >
    <ListView.ItemContainerStyle>
         <Style TargetType="ListViewItem">
             <Setter Property="IsFocusEngagementEnabled" Value="True"/>
         </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

當清單項目不會執行動作時，我們建議使用本指導方針來處理使用控制器或鍵盤的瀏覽。

**控制器**

當輸入是來自控制器時，將會提供此使用者體驗：

- 如果焦點位於清單項目上，下方向鍵會將焦點放到下一個清單項目上。
- 如果焦點位於清單項目上，左/右方向鍵會沒有選項，或是在清單右方具有可設定焦點的元素時，將焦點放到那裡。
- 如果焦點位於清單項目上，'A' 按鈕會將焦點放在以「從上到下、從左到右」為優先順序的巢狀 UI 上。
- 位於巢狀 UI 內時，將會遵循 XY 焦點瀏覽模型。  焦點將只能在目前清單項目內的巢狀 UI 中四處瀏覽，直到使用者按下 'B' 按鈕為止，那將會把焦點放回清單項目上。

**鍵盤**

當輸入是來自鍵盤時，以下將是使用者會獲得的體驗：

- 如果焦點位於清單項目上，向下鍵會將焦點放到下一個清單項目上。
- 如果焦點位於清單項目上，按下向左/向右鍵會沒有選項。
- 如果焦點位於清單項目上，按下 Tab 鍵會將焦點放到巢狀 UI 項目中的下一個定位停駐點。
- 如果焦點位於其中一個巢狀 UI 項目上，按下 Tab 鍵將會以定位順序周遊巢狀 UI 項目。  當所有巢狀 UI 項目皆已周遊完畢，系統會將焦點放到 ListView 之後，定位順序中的下一個控制項。
- Shift+Tab 所做出的行為會與 Tab 行為的方向相反。

## <a name="example"></a>範例

此範例示範如何實作[清單項目會執行動作的巢狀 UI](#nested-ui-where-list-items-perform-an-action)。

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="True"
          ChoosingItemContainer="listview1_ChoosingItemContainer"/>
```

```csharp
private void OnListViewItemKeyDown(object sender, KeyRoutedEventArgs e)
{
    // Code to handle going in/out of nested UI with gamepad and remote only.
    if (e.Handled == true)
    {
        return;
    }

    var focusedElementAsListViewItem = FocusManager.GetFocusedElement() as ListViewItem;
    if (focusedElementAsListViewItem != null)
    {
        // Focus is on the ListViewItem.
        // Go in with Right arrow.
        Control candidate = null;

        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                var rawPixelsPerViewPixel = DisplayInformation.GetForCurrentView().RawPixelsPerViewPixel;
                GeneralTransform generalTransform = focusedElementAsListViewItem.TransformToVisual(null);
                Point startPoint = generalTransform.TransformPoint(new Point(0, 0));
                Rect hintRect = new Rect(startPoint.X * rawPixelsPerViewPixel, startPoint.Y * rawPixelsPerViewPixel, 1, focusedElementAsListViewItem.ActualHeight * rawPixelsPerViewPixel);
                candidate = FocusManager.FindNextFocusableElement(FocusNavigationDirection.Right, hintRect) as Control;
                break;
        }

        if (candidate != null)
        {
            candidate.Focus(FocusState.Keyboard);
            e.Handled = true;
        }
    }
    else
    {
        // Focus is inside the ListViewItem.
        FocusNavigationDirection direction = FocusNavigationDirection.None;
        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadUp:
            case Windows.System.VirtualKey.GamepadLeftThumbstickUp:
                direction = FocusNavigationDirection.Up;
                break;
            case Windows.System.VirtualKey.GamepadDPadDown:
            case Windows.System.VirtualKey.GamepadLeftThumbstickDown:
                direction = FocusNavigationDirection.Down;
                break;
            case Windows.System.VirtualKey.GamepadDPadLeft:
            case Windows.System.VirtualKey.GamepadLeftThumbstickLeft:
                direction = FocusNavigationDirection.Left;
                break;
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                direction = FocusNavigationDirection.Right;
                break;
            default:
                break;
        }

        if (direction != FocusNavigationDirection.None)
        {
            Control candidate = FocusManager.FindNextFocusableElement(direction) as Control;
            if (candidate != null)
            {
                ListViewItem listViewItem = sender as ListViewItem;

                // If the next focusable candidate to the left is outside of ListViewItem,
                // put the focus on ListViewItem.
                if (direction == FocusNavigationDirection.Left &&
                    !listViewItem.IsAncestorOf(candidate))
                {
                    listViewItem.Focus(FocusState.Keyboard);
                }
                else
                {
                    candidate.Focus(FocusState.Keyboard);
                }
            }

            e.Handled = true;
        }
    }
}

private void listview1_ChoosingItemContainer(ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    if (args.ItemContainer == null)
    {
        args.ItemContainer = new ListViewItem();
        args.ItemContainer.KeyDown += OnListViewItemKeyDown;
    }
}
```

```csharp
// DependencyObjectExtensions.cs definition.
public static class DependencyObjectExtensions
{
    public static bool IsAncestorOf(this DependencyObject parent, DependencyObject child)
    {
        DependencyObject current = child;
        bool isAncestor = false;

        while (current != null && !isAncestor)
        {
            if (current == parent)
            {
                isAncestor = true;
            }

            current = VisualTreeHelper.GetParent(current);
        }

        return isAncestor;
    }
}
```
