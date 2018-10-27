---
author: QuinnRadich
Description: Used to select or deselect action items. Can be used for a single list item or for multiple list items.
title: 核取方塊
ms.assetid: 6231A806-287D-43EE-BD8D-39D2FF761914
label: Check boxes
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9c926f9dc7a87b83550bb2cd3a5bff856ec27866
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5698146"
---
# <a name="check-boxes"></a>核取方塊

 

核取方塊可用於選取或取消選取動作項目。 它可以用於單一項目或使用者可從中選擇的多個項目清單。 這個控制項有三個選項狀態：未選取、已選取，以及不確定。 當子選項集合同時含有未選取和已選取狀態時，請使用不確定狀態。

> **重要 API**：[CheckBox 類別](https://msdn.microsoft.com/library/windows/apps/br209316)、[Checked 事件](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx)、[IsChecked 屬性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx)

![核取方塊狀態範例](images/templates-checkbox-states-default.png)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

針對二元 (是/否) 選項 (例如用於 [記住我?]) 使用**單一核取方塊** 登入案例或是用於服務合約條款。

![針對個別選項使用單一核取方塊](images/checkbox1.png)

對於二元選項，將主要之間的差異 **核取方塊** 和 [切換開關](toggles.md) 核取方塊針對狀態而切換開關是針對動作。 您可以延遲認可核取方塊互動 (例如，作為表格提交的一部分)，此時應該立即認可切換開關互動。 此外，多個選項僅允許使用核取方塊。

針對使用者從不會互斥的選項群組中，選擇一個或多個項目的複選案例，請使用**多個核取方塊**。

當使用者可以選取任何選項組合時，建立核取方塊群組。

![使用核取方塊選取多個選項](images/checkbox2.png)

將選項分組時，您可以使用不確定的核取方塊來代表整個群組。 當使用者選取群組中部分而非全部的子項目時，請使用核取方塊的不確定狀態。

![用來顯示混合選項的核取方塊](images/checkbox3.png)

**核取方塊**和**選項按鈕**控制項都能讓使用者從選項清單中選取。 核取方塊可讓使用者選取一個選項組合。 相反地，選項按鈕可讓使用者從互斥的選項中進行單一選擇。 當有一個以上選項但僅能選取一個時，請改用選項按鈕。

## <a name="examples"></a>範例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/CheckBox">開啟應用程式並查看 CheckBox 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-checkbox"></a>建立核取方塊

若要指派核取方塊的標籤，請使用 [Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx) 屬性。 標籤會顯示在核取方塊旁邊。

這個 XAML 會建立單一核取方塊，以便在可提交表單之前用來同意服務條款。 

```xaml
<CheckBox x:Name="termsOfServiceCheckBox" 
          Content="I agree to the terms of service."/>
```

以下是使用程式碼建立的同一個核取方塊。

```csharp
CheckBox checkBox1 = new CheckBox();
checkBox1.Content = "I agree to the terms of service.";
```

### <a name="bind-to-ischecked"></a>繫結到 IsChecked

使用 [IsChecked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx) 屬性，以判斷是否選取或清除核取方塊。 您可以將 IsChecked 屬性的值繫結到另一個二進位值。 不過，因為 IsChecked 是[可為 null](https://msdn.microsoft.com/library/windows/apps/b3h38hb0.aspx) 的布林值，所以您必須使用值轉換器，才能將它繫結到布林值。

在這個範例中，用於同意服務條款之核取方塊的 **IsChecked** 屬性會繫結到 [提交] 按鈕的 [IsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled.aspx) 屬性。 唯有當您同意服務條款時，才會啟用 \[提交\] 按鈕。

> 請注意&nbsp;&nbsp;我們只顯示相關的程式碼。 如需資料繫結和值轉換器的詳細資訊，請參閱[資料繫結概觀](../../data-binding/data-binding-quickstart.md)。

```xaml
...
<Page.Resources>
    <local:NullableBooleanToBooleanConverter x:Key="NullableBooleanToBooleanConverter"/>
</Page.Resources>

...

<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind termsOfServiceCheckBox.IsChecked, 
                        Converter={StaticResource NullableBooleanToBooleanConverter}, Mode=OneWay}"/>
</StackPanel>
```

```csharp
public class NullableBooleanToBooleanConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, string language)
    {
        if (value is bool?)
        {
            return (bool)value;
        }
        return false;
    }

    public object ConvertBack(object value, Type targetType, object parameter, string language)
    {
        if (value is bool)
            return (bool)value;
        return false;
    }
}
```

### <a name="handle-click-and-checked-events"></a>處理 Click 和 Checked 事件

若要在核取方塊狀態變更時執行動作，您可以處理 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件，或者 [Checked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx) 和 [Unchecked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.unchecked.aspx) 事件。 

**Click** 事件會在已核取的狀態變更時發生。 如果要處理 Click 事件，請使用 **IsChecked** 屬性來判斷核取方塊的狀態。

**Checked** 和 **Unchecked** 事件會單獨發生。 如果您處理這些事件，您應該同時處理這兩個事件，以回應核取方塊中的狀態變更。

在下列範例中，我們會示範如何處理 Click 事件，以及 Checked 和 Unchecked 事件。

多個核取方塊可以共用相同的事件處理常式。 這個範例會建立四個核取方塊來選取披薩餡料。 這四個核取方塊會共用相同的 **Click** 事件處理常式，來更新選取的餡料清單。

```XAML
<StackPanel Margin="40">
    <TextBlock Text="Pizza Toppings"/>
    <CheckBox Content="Pepperoni" x:Name="pepperoniCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Beef" x:Name="beefCheckbox" 
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Mushrooms" x:Name="mushroomsCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Onions" x:Name="onionsCheckbox"
              Click="toppingsCheckbox_Click"/>

    <!-- Display the selected toppings. -->
    <TextBlock Text="Toppings selected:"/>
    <TextBlock x:Name="toppingsList"/>
</StackPanel>
```

以下是 Click 事件的事件處理常式。 每次按一下核取方塊時，它會檢查核取方塊以查看哪些方塊已核取，並更新選取的餡料清單。

```csharp
private void toppingsCheckbox_Click(object sender, RoutedEventArgs e)
{
    string selectedToppingsText = string.Empty;
    CheckBox[] checkboxes = new CheckBox[] { pepperoniCheckbox, beefCheckbox,
                                             mushroomsCheckbox, onionsCheckbox };
    foreach (CheckBox c in checkboxes)
    {
        if (c.IsChecked == true)
        {
            if (selectedToppingsText.Length > 1)
            {
                selectedToppingsText += ", ";
            }
            selectedToppingsText += c.Content;
        }
    }
    toppingsList.Text = selectedToppingsText;
}
```

### <a name="use-the-indeterminate-state"></a>使用不確定狀態

CheckBox 控制項是繼承自 [ToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.aspx)，且可以有三種狀態︰ 

狀態 | 屬性 | 值
------|----------|------
已核取 | IsChecked | **true** 
未核取 | IsChecked | **false** 
不確定 | IsChecked | **null** 

針對要報告不確定狀態的核取方塊，您必須將 [IsThreeState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.isthreestate.aspx) 屬性設為 **true**。 

將選項分組時，您可以使用不確定的核取方塊來代表整個群組。 當使用者選取群組中部分而非全部的子項目時，請使用核取方塊的不確定狀態。

在下列範例中，[全選] 核取方塊已將其 IsThreeState 屬性設為 **true**。 針對 [全選] 核取方塊，如果已核取所有子元素，就表示已核取，如果未核取所有子元素，就表示未核取，否則即為不確定狀態。

```xaml
<StackPanel>
    <CheckBox x:Name="OptionsAllCheckBox" Content="Select all" IsThreeState="True" 
              Checked="SelectAll_Checked" Unchecked="SelectAll_Unchecked" 
              Indeterminate="SelectAll_Indeterminate"/>
    <CheckBox x:Name="Option1CheckBox" Content="Option 1" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
    <CheckBox x:Name="Option2CheckBox" Content="Option 2" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" IsChecked="True"/>
    <CheckBox x:Name="Option3CheckBox" Content="Option 3" Margin="24,0,0,0"
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
</StackPanel>
```

```csharp
private void Option_Checked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void Option_Unchecked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void SelectAll_Checked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = true;
}

private void SelectAll_Unchecked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = false;
}

private void SelectAll_Indeterminate(object sender, RoutedEventArgs e)
{
    // If the SelectAll box is checked (all options are selected), 
    // clicking the box will change it to its indeterminate state.
    // Instead, we want to uncheck all the boxes,
    // so we do this programatically. The indeterminate state should
    // only be set programatically, not by the user.

    if (Option1CheckBox.IsChecked == true &&
        Option2CheckBox.IsChecked == true &&
        Option3CheckBox.IsChecked == true)
    {
        // This will cause SelectAll_Unchecked to be executed, so
        // we don't need to uncheck the other boxes here.
        OptionsAllCheckBox.IsChecked = false;
    }
}

private void SetCheckedState()
{
    // Controls are null the first time this is called, so we just 
    // need to perform a null check on any one of the controls.
    if (Option1CheckBox != null)
    {
        if (Option1CheckBox.IsChecked == true &&
            Option2CheckBox.IsChecked == true &&
            Option3CheckBox.IsChecked == true)
        {
            OptionsAllCheckBox.IsChecked = true;
        }
        else if (Option1CheckBox.IsChecked == false &&
            Option2CheckBox.IsChecked == false &&
            Option3CheckBox.IsChecked == false)
        {
            OptionsAllCheckBox.IsChecked = false;
        }
        else
        {
            // Set third state (indeterminate) by setting IsChecked to null.
            OptionsAllCheckBox.IsChecked = null;
        }
    }
}
```

## <a name="dos-and-donts"></a>可行與禁止事項

-   確認核取方塊的目的和目前狀態非常明確。
-   將核取方塊文字內容限制在兩行以內。
-   核取方塊標籤的措辭陳述應該是有核取記號表示「是」，沒有核取記號則是「否」。
-   除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
-   如果文字內容是動態的，請考慮如何調整控制項的大小以及調整後周圍的視覺項目會發生什麼變化。
-   如果有兩個或多個互斥選項可供選擇，請考慮使用[選項按鈕](radio-button.md)。
-   不要並列核取方塊群組。 使用群組標籤分隔群組。
-   不要使用核取方塊做為開/關控制項或執行命令；請改用切換開關。
-   不要使用核取方塊顯示其他控制項 (例如對話方塊)。
-   使用不確定狀態，指示為部分子選項 (而非全部子選項) 設定某個選項。
-   使用不確定狀態時，使用附屬核取方塊來顯示選取或未選取哪些選項。 設計讓使用者可以看到子選項的 UI。
-   不要使用不確定狀態來表示第三個狀態。 不確定狀態是用來指示選項僅適用於部分子選項 (而非全部子選項)。 所以，不要讓使用者直接設定不確定狀態。 對於不應該執行的範例，此核取方塊使用不確定狀態來指示中等辣度：

    ![不確定核取方塊](images/checkbox4_spicy.png)

    改用含三個選項的選項按鈕群組：「不辣」、「辣」以及「非常辣」。

    ![含有三個選項的選項按鈕群組：「不辣」、「辣」以及「非常辣」](images/spicyoptions.png)

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [CheckBox 類別](https://msdn.microsoft.com/library/windows/apps/br209316) 
- [選項按鈕](radio-button.md)
- [切換開關](toggles.md)
