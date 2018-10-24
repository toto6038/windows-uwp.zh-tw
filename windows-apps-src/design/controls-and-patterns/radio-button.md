---
author: QuinnRadich
Description: Radio buttons let users select one option from two or more choices.
title: 選項按鈕的指導方針
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 48d830b388fee8a0007447a66aa58e3794cfaae0
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2018
ms.locfileid: "5445536"
---
# <a name="radio-buttons"></a>選項按鈕

> **重要 API**：[RadioButton 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)、[Checked 事件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)、[IsChecked 屬性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

選項按鈕可讓使用者從集合中選取一個選項。 每個選項都由一個選項按鈕表示，而且使用者在選項按鈕群組中只能選取一個選項按鈕 

(如果您真想知道選項按鈕的名稱，其實就是以收音機的頻道預設按鈕來命名)。

![選項按鈕](images/controls/radio-button.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用選項按鈕將二或多個互斥選項呈現給使用者。

![選項按鈕群組](images/radiobutton_basic.png)

當使用者需要查看所有選項來做選擇時，使用選項按鈕。 由於選項按鈕同樣強調所有選項，因此可能會對選項引起不必要的注意。 除非選項值得使用者特別注意，否則請考慮使用其他控制項。 例如，如果在大多數情況下會建議大部分使用者使用預設選項，請改用[下拉式清單](lists.md)。

![下拉式清單](images/combo_box_collapsed.png)

如果只有兩個互斥的選項，請將它們組成單一[核取方塊](checkbox.md)或[切換開關](toggles.md)。 例如，使用「我同意」的一個核取方塊來代替「我同意」和「我不同意」兩個選項按鈕。

![表示二元選擇的兩個方式](images/radiobutton_vs_checkbox.png)

使用者可以選取多個選項時，請使用[核取方塊](checkbox.md)。

![使用核取方塊選取多個選項](images/checkbox2.png)

當選項是有固定步距的數字 (10、20、30) 時，請使用[滑桿](slider.md)控制項。

![滑桿控制項](images/controls/slider.png)

如果有 8 個以上的選項，請使用[下拉式清單](lists.md)或[清單方塊](lists.md)。

![下拉式方塊](images/combo_box_scroll.png)

如果可用的選項取決於應用程式目前的內容，或者可能會不斷地變化，請使用單選[清單方塊](lists.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/RadioButton">開啟應用程式並查看 RadioButton 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Microsoft Edge 瀏覽器設定中的選項按鈕。

![Microsoft Edge 瀏覽器設定中的選項按鈕](images/control-examples/radio-buttons-edge.png)

## <a name="create-a-radio-button"></a>建立選項按鈕

選項按鈕以群組方式運作。 您可以用 2 種方式來將選項按鈕控制項方組︰
- 將它們放入同一個父容器。
- 將每個選項按鈕的 [GroupName](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) 屬性設定為相同的值。

在這個範例中，第一個選項按鈕群組位於相同的堆疊面板中，藉此以隱含方式群組化。 第二個群組分成 2 個堆疊面板，所以是依 GroupName 明確分組。

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

選項按鈕群組看起來就像這樣。

![兩個群組中的選項按鈕](images/radio-button-groups.png)

選項按鈕有兩個狀態：*已選取* 或 *已清除*。 已選取選項按鈕時，其 [IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) 屬性為 **true**。 已清除選項按鈕時，其 **IsChecked** 屬性為 **false**。 按一下同一個群組中的另一個選項按鈕，即可清除選項按鈕，但無法藉由再按一次來清除。 不過，您可以將選項按鈕的 IsChecked 屬性設定為 **false**，以程式設計方式清除該選項按鈕。 您可以取得 **IsChecked** 屬性的 **Value**，將 **IsChecked** 屬性與 bool 實際做比較。

## <a name="recommendations"></a>建議事項

-   確定一組選項按鈕的目的及目前狀態非常明確。
-   將選項按鈕的文字限制為單行。
-   如果文字內容是動態的，請考慮如何調整按鈕的大小以及調整後周圍的視覺效果會發生什麼變化。
-   除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
-   不要將兩個選項按鈕群組並排。 當兩個選項按鈕群組並排時，很難判斷哪個按鈕屬於哪個群組。

## <a name="additional-usage-guidance"></a>其他用法指導方針

下圖顯示放置選項按鈕及其間隔的正確方法。

![一組選項按鈕](images/radiobutton-layout.png)

![選項按鈕間距指導方針](images/radiobutton-redlines.png)

## <a name="related-topics"></a>相關主題

**適用於設計人員**
- [按鈕](buttons.md)
- [切換開關](toggles.md)
- [核取方塊](checkbox.md)
- [清單和下拉式方塊](lists.md)
- [滑桿](slider.md)

**適用於開發人員 (XAML)**
- [RadioButton 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
