---
author: Jwmsft
Description: "選項按鈕可以讓使用者從兩個以上的選項中選取一個選項。"
title: "選項按鈕的指導方針"
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: 95ddb1ddd1dfd318a5c491504c95f7833f98115e

---
# <a name="radio-buttons"></a>選項按鈕

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

選項按鈕可以讓使用者從兩個以上的選項中選取一個選項。 每個選項都由一個選項按鈕表示；使用者在選項按鈕群組中只能選取一個選項按鈕。

(如果您對名稱感到好奇，選項按鈕 (Radio Button) 的名稱來自於收音機 (Radio) 上的頻道預設按鈕。)

![選項按鈕](images/controls/radio-button.png)

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**RadioButton 類別**](https://msdn.microsoft.com/library/windows/apps/br227544)</li>
<li>[**Checked 事件**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx)</li>
<li>[**IsChecked 屬性**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx)</li>
</ul>
</div>


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用選項按鈕將二或多個互斥選項呈現給使用者，如下所示。

![選項按鈕群組](images/radiobutton_basic.png)

選項按鈕可讓應用程式中非常重要的選項變得更清楚和明顯。 當呈現的選項重要到需要更多螢幕空間，且其中選擇範圍的清晰度需要非常明確的選項時，請使用選項按鈕。

選項按鈕同樣強調所有選項，因此可能會對選項引起不必要的注意。 除非選項值得使用者額外的注意，否則請考慮使用其他控制項。 例如，如果在大多數情況下會建議大部分使用者使用預設選項，請改用[下拉式清單](lists.md)。

如果只有兩個互斥的選項，請將它們組成單一[核取方塊](checkbox.md)或[切換開關](toggles.md)。 例如，使用「我同意」的一個核取方塊來代替「我同意」和「我不同意」兩個選項按鈕。

![表示二元選擇的兩個方式](images/radiobutton_vs_checkbox.png)

使用者可以選取多個選項時，請改用[核取方塊](checkbox.md)或[清單方塊](lists.md)控制項。

![使用核取方塊選取多個選項](images/checkbox2.png)

當選項是有固定階段的數字時，如 10、20、30，請不要使用選項按鈕。 請改用[滑桿](slider.md)控制項。

如果有超過 8 個選項，請改用[下拉式清單](lists.md)、單選[清單方塊](lists.md)或[清單方塊](lists.md)。

如果可用的選項取決於應用程式目前的內容，或者可能會不斷地變化，請改用單選[清單方塊](lists.md)。

## <a name="example"></a>範例
Microsoft Edge 瀏覽器設定中的選項按鈕。

![Microsoft Edge 瀏覽器設定中的選項按鈕](images/control-examples/radio-buttons-edge.png)

## <a name="create-a-radio-button"></a>建立選項按鈕

選項按鈕以群組方式運作。 您可以用 2 種方式來將選項按鈕控制項方組︰
- 將它們放入同一個父容器。
- 將每個選項按鈕的 [**GroupName**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.radiobutton.groupname.aspx) 屬性設定為相同的值。

> **注意**&nbsp;&nbsp;透過鍵盤存取時，選項按鈕群組的操作就像單一控制項一樣。 使用 Tab 鍵只能存取已選取的選項，但是使用者可以使用方向鍵循環瀏覽群組。

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

選項按鈕有兩個狀態：*已選取* 或 *已清除*。 已選取選項按鈕時，其 [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx) 屬性為 **true**。 已清除選項按鈕時，其 **IsChecked** 屬性為 **false**。 按一下同一個群組中的另一個選項按鈕，即可清除選項按鈕，但無法藉由再按一次來清除。 不過，您可以將選項按鈕的 IsChecked 屬性設定為 **false**，以程式設計方式清除該選項按鈕。

## <a name="recommendations"></a>建議

-   確定一組選項按鈕的目的和目前狀態非常明確。
-   一律在使用者點選選項按鈕時顯示視覺化回饋。
-   在使用者與選項按鈕互動時顯示視覺化回饋。 選項按鈕狀態的範例有正常、已按下、已核選和已停用。 使用者點選選項按鈕以啟用相關的選項。 點選已啟用的選項不會停用它，但是點選另一個選項會將啟用轉移給該選項。
-   為觸控回饋和核取狀態保留視覺效果和動畫；在未核取狀態下，選項按鈕控制項應該顯示為未使用或未啟用 (但不是已停用)。
-   將選項按鈕的文字限制為單行。 您可以自訂選項按鈕的視覺效果，在主要文字行下方以較小的字型大小顯示選項的描述。
-   如果文字內容是動態的，請考慮如何調整按鈕的大小以及調整後周圍的視覺效果會發生什麼變化。
-   除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
-   在標籤元素內包含選項按鈕，這樣點選標籤就可以選取選項按鈕。
-   將標籤文字放在選項按鈕控制項後面，而非其前面或上方。
-   考慮自訂您的選項按鈕。 選項按鈕預設由兩個同心圓組成—內圓是填滿的 (核取選項按鈕就會顯示)，外圓是空心的—還有一些文字內容。 但是我們鼓勵您發揮創造力。 使用者可安心地和應用程式的內容直接互動。 所以您可以選擇以圖形或精巧的文字切換按鈕顯示實際內容。
-   不要在選項按鈕群組中放置超過 8 個選項。 當您需要呈現更多選項時，請改用[下拉式清單](lists.md)、[清單方塊](lists.md)或[清單檢視](lists.md)。
-   不要並列兩個選項按鈕群組。 當兩個選項按鈕群組並列時，很難判斷哪個按鈕屬於哪個群組。 使用群組標籤加以區隔。

## <a name="additional-usage-guidance"></a>其他用法指導方針

下圖顯示放置選項按鈕及其間隔的正確方法。

![一組選項按鈕](images/radiobutton_layout1.png)
## <a name="related-topics"></a>相關主題

**適用於設計人員**
- [按鈕的指導方針](buttons.md)
- [切換開關的指導方針](toggles.md)
- [核取方塊的指導方針](checkbox.md)
- [下拉式清單的指導方針](lists.md)
- [清單檢視和格線檢視控制項的指導方針](lists.md)
- [滑桿的指導方針](slider.md)
- [選取控制項的指導方針](lists.md)


**適用於開發人員 (XAML)**
- [**Windows.UI.Xaml.Controls RadioButton 類別**](https://msdn.microsoft.com/library/windows/apps/br227544)



<!--HONumber=Dec16_HO2-->


