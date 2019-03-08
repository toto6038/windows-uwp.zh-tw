---
Description: 使用者輸入時提供建議的文字輸入方塊。
title: 下拉式方塊 （下拉式清單）
label: Combo box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 21a6c698fa0e07587e2c25ae827dc6654a8ced9d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618623"
---
# <a name="combo-box"></a>下拉式方塊

您可以使用下拉式方塊 （也稱為下拉式清單） 來呈現使用者可以從選取的項目清單。 下拉式方塊會以壓縮的狀態啟動，並展開以顯示一份可選取的項目。

關閉下拉式方塊時，它會顯示目前的選取範圍或如果沒有選取的項目是空的。 當使用者展開下拉式方塊時，它會顯示可選取的項目清單。

> **重要的 Api**:[ComboBox 類別](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)， [IsEditable 屬性](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)，[文字屬性](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)， [TextSubmitted 事件](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

下拉式方塊的標頭壓縮狀態。

![精簡狀態下的下拉式清單範例](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

- 若要讓使用者從能夠以單行文字充分表示的一組項目中選取單一值，請使用下拉式清單。
- 若要顯示包含多行文字或影像的項目，請使用清單或資料格檢視，而不要使用下拉式清單。
- 如果項目少於五個，請考慮使用[選項按鈕](radio-button.md) (如果是單選) 或[核取方塊](checkbox.md) (如果是複選)。
- 如果選項項目在您應用程式流程中的重要性為次要，請使用下拉式方塊。 如果針對大部分使用者在大多數情況下建議使用預設選項，則使用清單方塊來顯示所有項目可能會讓使用者浪費過多注意力在選項上。 您可以藉由使用下拉式方塊來節省空間，以及減少注意力分散的情形。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您有<strong style="font-weight: semi-bold">XAML 控制項陳列庫</strong>應用程式安裝，請按一下這裡可<a href="xamlcontrolsgallery:/item/ComboBox">開啟 應用程式，並查看作用中的下拉式方塊</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項陳列庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

精簡狀態下的下拉式清單可以顯示標頭。

![精簡狀態下的下拉式清單範例](images/combo_box_collapsed.png)

雖然下拉式方塊可延展來支援較長的字串長度，但是請避免使用過長而難以閱讀的字串。

![具有長文字字串的下拉式清單範例](images/combo_box_listitemstate.png)

如果下拉式方塊中的集合夠長，將會顯示捲軸以容納它。 將清單中的項目以邏輯方式分組。

![下拉式清單中捲軸的範例](images/combo_box_scroll.png)

## <a name="create-a-combo-box"></a>建立下拉式方塊

您填入下拉式方塊，直接將物件加入[項目](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)集合或繫結[ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)到資料來源的屬性。 新增至下拉式方塊項目會包裝在[ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem)容器。

以下是在 XAML 中加入的項目與簡單的下拉式方塊。

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

下列範例會示範繫結下拉式方塊 FontFamily 物件的集合。

```xaml
<ComboBox x:Name="FontsCombo" Header="Fonts" Height="44" Width="296"
          ItemsSource="{x:Bind fonts}" DisplayMemberPath="Source"/>
```

```csharp
ObservableCollection<FontFamily> fonts = new ObservableCollection<FontFamily>();

public MainPage()
{
    this.InitializeComponent();
    fonts.Add(new FontFamily("Arial"));
    fonts.Add(new FontFamily("Courier New"));
    fonts.Add(new FontFamily("Times New Roman"));
}
```

### <a name="item-selection"></a>項目選取

如同 ListView 和 GridView，ComboBox 衍生自[選取器](/uwp/api/windows.ui.xaml.controls.primitives.selector)，因此您可以在相同的標準方式取得使用者的選取項目。

您可以取得或設定下拉式方塊的選取的項目利用[SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)屬性，並取得或設定使用選取的項目索引[SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)屬性。

若要取得特定屬性的值在選取的資料項目上，您可以使用[SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue)屬性。 在此案例中，設定[SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath)上選取的項目，以取得此值從指定的屬性。

> [!TIP]
> 如果您設定 SelectedItem 或 selectedindex 等都表示預設選取項目，如果此屬性設定才會填入下拉式方塊項目集合，就會發生例外狀況。 除非您在 XAML 中定義您的項目，最好是處理下拉式方塊 Loaded 的事件，並設定 SelectedItem 或 selectedindex 等都在 Loaded 的事件處理常式。

您可以將繫結至 XAML，這些屬性，或處理[SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)回應選取範圍變更事件。

在事件處理常式程式碼，您可以取得選取的項目，從[SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems)屬性。 您可以從取得先前選取的項目 （如果有的話） [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems)屬性。 AddedItems 和 RemovedItems 集合每個包含只有 1 個項目，因為下拉式方塊並不支援多個選取項目。

此範例示範如何處理 SelectionChanged 事件，以及如何繫結至選取的項目。

```xaml
<StackPanel>
    <ComboBox x:Name="colorComboBox" Width="200"
              Header="Colors" PlaceholderText="Pick a color"
              SelectionChanged="ColorComboBox_SelectionChanged">
        <x:String>Blue</x:String>
        <x:String>Green</x:String>
        <x:String>Red</x:String>
        <x:String>Yellow</x:String>
    </ComboBox>

    <Rectangle x:Name="colorRectangle" Height="30" Width="100"
               Margin="0,8,0,0" HorizontalAlignment="Left"/>

    <TextBlock Text="{x:Bind colorComboBox.SelectedIndex, Mode=OneWay}"/>
    <TextBlock Text="{x:Bind colorComboBox.SelectedItem, Mode=OneWay}"/>
</StackPanel>
```

```csharp
private void ColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add "using Windows.UI;" for Color and Colors.
    string colorName = e.AddedItems[0].ToString();
    Color color;
    switch (colorName)
    {
        case "Yellow":
            color = Colors.Yellow;
            break;
        case "Green":
            color = Colors.Green;
            break;
        case "Blue":
            color = Colors.Blue;
            break;
        case "Red":
            color = Colors.Red;
            break;
    }
    colorRectangle.Fill = new SolidColorBrush(color);
}
```

#### <a name="selectionchanged-and-keyboard-navigation"></a>SelectionChanged 和鍵盤瀏覽

根據預設，當使用者按一下、 點選，或按下的項目上的 Enter 鍵認可其選取項目，清單中，並關閉下拉式方塊時，就會發生 SelectionChanged 事件。 當使用者瀏覽開啟的下拉式方塊的清單，使用鍵盤方向鍵時，不會變更選取項目。

若要將下拉式方塊 「 即時更新 」，而使用者使用 （例如字型選擇下拉式選單） 使用方向鍵來巡覽開啟的清單，將[SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger)要[永遠](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger)。 這會導致 SelectionChanged 事件發生時的焦點變更至開啟的清單中的另一個項目。

#### <a name="selected-item-behavior-change"></a>選取的項目行為變更

在 Windows 10 版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本中，選取項目的行為會更新以支援可編輯的下拉式方塊。

之前 SDK 17763，SelectedItem 屬性的值 (因此 SelectedValue 和 SelectedIndex) 必須在下拉式方塊的項目集合中。 使用上述範例中，設定`colorComboBox.SelectedItem = "Pink"`導致：

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

Sdk 17763 及更新版本、 SelectedItem 屬性的值 (因此 SelectedValue 和 SelectedIndex) 不需要在下拉式方塊的項目集合。 使用上述範例中，設定`colorComboBox.SelectedItem = "Pink"`導致：

- SelectedItem = 粉紅
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>文字搜尋

下拉式方塊可自動支援在其集合內的搜尋。 當焦點在一個已開啟或關閉的下拉式方塊上時，如果使用者在實體鍵盤上輸入字元，就會顯示與使用者的字串相符的候選項目。 在瀏覽長清單時，這項功能特別有幫助。 比方說，當與下拉式清單包含一份狀態互動，使用者可以按"w"索引鍵來快速選取項目帶入檢視的 「 Washington 」。 搜尋文字不區分大小寫。

您可以設定[IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled)屬性設**false**停用這項功能。

## <a name="make-a-combo-box-editable"></a>製作可編輯的下拉式方塊

> [!IMPORTANT]
> 這項功能需要 Windows 10 版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本。

根據預設，下拉式方塊可讓使用者從預先定義的選項清單中選取。 不過，有清單只包含一部分有效的值，而且使用者應該能夠輸入其他未列出的值。 若要支援此功能，您可以讓下拉式方塊可進行編輯。

若要讓下拉式方塊可進行編輯，設定[IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)屬性設 **，則為 true**。 然後，處理[TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)事件來處理使用者輸入的值。

根據預設，當使用者認可的自訂文字，會更新 SelectedItem 值。 您可以藉由設定覆寫這個行為**Handled**要 **，則為 true** TextSubmitted 事件引數中。 事件標記為已處理時，下拉式方塊會採取任何進一步的動作，在事件之後，並將維持編輯的狀態。 將不會更新 SelectedItem。

這個範例示範簡單的可編輯下拉式方塊。 此清單包含簡單的字串，並輸入，會使用使用者輸入任何值。

[最近使用的名稱] 選擇器可讓使用者輸入自訂的字串。 'RecentlyUsedNames' 清單包含使用者可供選擇，某些值，但使用者也可以加入新的自訂值。 'CurrentName' 屬性表示目前輸入的名稱。

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>送出的文字

您可以處理[TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)事件來處理使用者輸入的值。 事件處理常式中，您會通常會驗證使用者輸入的值有效，然後使用您的應用程式中的值。 視情況而定，您也可能會將值加入供未來使用的選項下拉式方塊的清單。

符合這些條件時，就會發生 TextSubmitted 事件：

- IsEditable 屬性是 **，則為 true**
- 使用者輸入不符合現有的項目下拉式方塊清單中的文字
- 使用者按下 Enter，或從下拉式方塊中移動焦點。

如果使用者輸入文字，然後會向上或向下巡覽清單，則不會發生 TextSubmitted 事件。

### <a name="sample---validate-input-and-use-locally"></a>範例-驗證輸入，並在本機上使用

在此範例中，字型大小選擇器包含一組值對應至字型的大小遞增，但使用者可能輸入不在清單中的字型大小。

當使用者將不在清單中，字型的大小更新，但值的值不會加入至清單中的字型大小。

如果新輸入的值不是有效的最後一個還原的 Text 屬性的情況下，您在使用 SelectedValue 已知良好的值。

```xaml
<ComboBox x:Name="fontSizeComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfFontSizes}"
          TextSubmitted="FontSizeComboBox_TextSubmitted"/>
```

```csharp
private void FontSizeComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (byte.TryParse(e.Text, out double newValue))
    {
        // Update the app’s font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>範例-驗證輸入，並新增至清單

在這裡，「 最愛的色彩選擇器 」 包含最常見最愛的色彩 （紅色、 藍色、 綠色、 橘色），但使用者可能輸入不在清單最愛的色彩。 當使用者將是有效的色彩 （例如粉紅色） 時，輸入新的色彩加入清單，並設定為使用 「 最愛的色彩 」。

```xaml
<ComboBox x:Name="favoriteColorComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfColors}"
          TextSubmitted="FavoriteColorComboBox_TextSubmitted"/>
```

```csharp
private void FavoriteColorComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (IsValid(e.Text))
    {
        FavoriteColor newColor = new FavoriteColor()
        {
            ColorName = e.Text,
            Color = ColorFromStringConverter(e.Text)
        }
        ListOfColors.Add(newColor);
    }
    else
    {
        // If the item is invalid, reject it but do not revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        e.Handled = true;
    }
}

bool IsValid(string Text)
{
    // Validate that the string is: not empty; a color.
}
```

## <a name="dos-and-donts"></a>可行與禁止事項

- 將下拉式方塊項目的文字內容限制為單行。
- 以最合乎邏輯的順序排序下拉式方塊中的項目。 將相關選項群組在一起並將最常用的選項放在頂端。 以字母順序排序名稱、以數字順序排序數字，以及以時間順序排序日期。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以互動式格式查看所有 XAML 控制項。
- [AutoSuggestBox 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>相關文章

- [文字控制項](text-controls.md)
- [拼字檢查](text-controls.md)
- [搜尋](search.md)
- [TextBox 類別](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 類別](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 屬性](https://msdn.microsoft.com/library/system.string.length.aspx)
