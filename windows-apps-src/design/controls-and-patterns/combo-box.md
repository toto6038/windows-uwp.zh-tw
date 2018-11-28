---
Description: A text entry box that provides suggestions as the user types.
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
ms.openlocfilehash: fb286b591881d16ff08dae9fe12770644d886acf
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7968033"
---
# <a name="combo-box"></a>下拉式方塊

請使用下拉式方塊 （也稱為下拉式清單） 來呈現使用者可從選取的項目清單。 下拉式方塊開始為精簡狀態，並展開以顯示可選取項目清單。

下拉式方塊關閉時，它會顯示目前的選取範圍或如果沒有任何選取的項目是空的。 當使用者擴充下拉式方塊時，它會顯示可選取項目的清單。

> **重要 Api**: [ComboBox 類別](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)， [IsEditable 屬性](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)、 [Text 屬性](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)， [TextSubmitted 事件](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

精簡狀態含有標頭下拉式方塊。

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
    <p>如果您已安裝的<strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，按一下這裡<a href="xamlcontrolsgallery:/item/ComboBox">開啟應用程式</a>並查看下拉式方塊中的動作。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
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

藉由新增物件直接到[Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)集合，或將[ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)屬性繫結至資料來源，您可以填入下拉式方塊。 項目新增到下拉式方塊會包裝在[ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem)容器中。

以下是在 XAML 中新增項目與簡單下拉式方塊。

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue<x:String>
    <x:String>Green<x:String>
    <x:String>Red<x:String>
    <x:String>Yellow<x:String>
</ComboBox>
```

下列範例示範 FontFamily 物件的集合以繫結下拉式方塊。

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

ListView 和 GridView，例如 ComboBox 被衍生自[選取器](/uwp/api/windows.ui.xaml.controls.primitives.selector)，因此您可以在相同的標準方式取得使用者的選擇。

您可以取得或設定下拉式方塊選取項目使用[SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)屬性，並由取得或設定選取的項目索引使用[SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)屬性。

若要取得上選取的資料項目的特定屬性的值，您可以使用[SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue)屬性。 在此案例中，設定來指定哪個屬性來取得的值從選取的項目上[SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) 。

> [!TIP]
> 如果您設定 SelectedItem 或 SelectedIndex 指出預設選取項目，如果之前填入下拉式方塊項目集合時，已設定的屬性，就會發生例外狀況。 除非您在 XAML 中定義您的項目，最好是處理下拉式方塊 Loaded 的事件，並將 SelectedItem 或 SelectedIndex 設定中的 Loaded 的事件處理常式。

您可以在 XAML 中，這些屬性繫結，或處理[SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)事件來回應選取項目變更。

在事件處理常式程式碼中，您可以從[SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems)屬性取得選取的項目。 您可以從[SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems)屬性來取得先前選取的項目 （如果有的話）。 AddedItems 和 RemovedItems 集合每個包含只有 1 個項目，因為下拉式方塊不支援多個選取項目。

這個範例示範如何處理 SelectionChanged 事件，以及如何將繫結到選取的項目。

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

根據預設，當使用者按下、 點選，或按下 enter 鍵的項目上認可其選取項目，在清單中和關閉的下拉式方塊時，就會發生 SelectionChanged 事件。 當使用者使用鍵盤方向鍵巡覽開啟下拉式方塊清單，不會變更選取項目。

若要建立的下拉式方塊 「 動態更新 」，而使用者瀏覽到開啟清單使用方向鍵 （例如字型選取項目下拉式），請將[SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger)設定為[永遠](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger)。 這會導致 SelectionChanged 事件發生時焦點變更為另一個開啟的清單中的項目。

#### <a name="selected-item-behavior-change"></a>選取的項目行為變更

在 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，行為的選取項目會更新以支援可編輯的下拉式方塊。

在 SDK 17763，SelectedItem 屬性的值之前 (因此 SelectedValue 和 SelectedIndex) 才能能在下拉式方塊的項目集合中。 使用上一個範例中，設定`colorComboBox.SelectedItem = "Pink"`會導致：

- SelectedItem = null
- SelectedValue = null
- SelectedIndex =-1

在 SDK 17763 和更新版本，SelectedItem 屬性的值 (因此 SelectedValue 和 SelectedIndex) 並不需要是下拉式方塊的項目集合中。 使用上一個範例中，設定`colorComboBox.SelectedItem = "Pink"`會導致：

- SelectedItem = 粉紅
- SelectedValue = 粉紅
- SelectedIndex =-1

### <a name="text-search"></a>文字搜尋

下拉式方塊可自動支援在其集合內的搜尋。 當焦點在一個已開啟或關閉的下拉式方塊上時，如果使用者在實體鍵盤上輸入字元，就會顯示與使用者的字串相符的候選項目。 在瀏覽長清單時，這項功能特別有幫助。 例如時與下拉式包含狀態清單的互動，, 使用者可以按"w"鍵來檢視顯示"Washington"以供快速選取。 文字搜尋不區分大小寫。

您可以將[IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled)屬性設**為 false**來停用這項功能。

## <a name="make-a-combo-box-editable"></a>讓下拉式方塊可編輯

> [!IMPORTANT]
> 此功能需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本。

根據預設，下拉式方塊可讓使用者選取從預先定義的選項清單。 不過，有許多的情況，其中的清單包含有效的值的子集，而使用者應該能夠輸入並未列出的其他值。 若要支援這種情況，您可以讓下拉式方塊可編輯。

若要讓下拉式方塊可編輯， [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)屬性設**為 true**。 然後，處理[TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)事件，以便處理使用者輸入的值。

根據預設，當使用者將其認可的自訂文字時，會更新 SelectedItem 值。 您可以藉由將**Handled**設定為**true** TextSubmitted 事件引數中覆寫這個行為。 當會將事件標示為已處理時，下拉式方塊會進行任何進一步的動作，事件之後，會留在編輯狀態。 SelectedItem 就無法更新。

這個範例示範簡單的可編輯下拉式方塊。 清單包含簡單的字串，並輸入時，會使用使用者輸入的任何值。

「 最近使用的名稱 「 選擇器可讓使用者輸入自訂字串。 'RecentlyUsedNames' 清單包含使用者可以選擇從，某些值，但是使用者也可以新增新的自訂值。 'CurrentName' 屬性代表目前輸入的名稱。

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>送出的文字

您可以處理[TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)事件來處理使用者輸入的值。 在事件處理常式中，您將通常會驗證使用者輸入的值是有效，然後使用您的應用程式中的值。 根據情形，您也可能會新增值以供未來使用選項下拉式方塊的清單。

當符合這些條件，就會發生 TextSubmitted 事件：

- IsEditable 屬性是**true**
- 在使用者輸入文字不符合下拉式方塊清單中的現有項目
- 在使用者按下 enter 鍵，或從下拉式方塊中移動焦點。

如果使用者在輸入文字，然後瀏覽清單向上或向下 TextSubmitted 事件不會發生。

### <a name="sample---validate-input-and-use-locally"></a>範例-驗證輸入，並在本機使用

在此範例中，字型大小選擇包含一組值對應到字型大小坡形，但是使用者可以輸入不是在清單中的字型大小。

當使用者加入不在清單中，字型大小更新，但值的值不會新增到清單的字型大小。

如果新輸入的值不是有效的您可以使用 SelectedValue 來還原 Text 屬性的最後一個已知的良好的值。

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

### <a name="sample---validate-input-and-add-to-list"></a>範例-驗證輸入，並將新增到清單

在這裡，「 最愛的色彩選擇器 」 包含最常見最愛的色彩紅色、 藍色、 綠色 （橘色），但是使用者可能會輸入最愛的色彩不在清單中。 當使用者加入 （例如粉紅） 是有效的色彩時，新輸入的色彩已新增到清單，並設定為使用中的 「 最愛的色彩 」。

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

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。
- [AutoSuggestBox 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>相關文章

- [文字控制項](text-controls.md)
- [拼字檢查](text-controls.md)
- [搜尋](search.md)
- [TextBox 類別](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 類別](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length property](https://msdn.microsoft.com/library/system.string.length.aspx)
