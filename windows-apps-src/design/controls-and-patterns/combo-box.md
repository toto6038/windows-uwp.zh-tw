---
Description: 使用者輸入時提供建議的文字輸入方塊。
title: 下拉式方塊和清單方塊
label: Combo box and list box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 31b3bcc2388a98941fc5e8aa44d18beee53de5c7
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081101"
---
# <a name="combo-box-and-list-box"></a>下拉式方塊和清單方塊

使用下拉式方塊 (也稱為下拉式清單) 來呈現使用者可以從中選取的項目清單。 下拉式方塊一開始為精簡狀態，展開可顯示可選取的項目清單。 清單方塊與下拉式方塊類似，但前者無法摺疊，也沒有精簡狀態。 您可以在本文結尾深入了解清單方塊。

關閉下拉式方塊後，它會顯示目前的選取項目，若沒有已選取的項目則是空的。 當使用者展開下拉式方塊時，它會顯示可選取的項目清單。

![精簡狀態下的下拉式清單範例](images/combo_box_collapsed.png)

> _精簡狀態下具有標頭的下拉式方塊。_

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](/windows/uwp/design/style/rounded-corner)。 WinUI 是 NuGet 套件，包含適用於 UWP 應用程式的新控制項與 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API：** [ComboBox 類別](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、[IsEditable 屬性](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)、[Text 屬性](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、[TextSubmitted 事件](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

- 若要讓使用者從能夠以單行文字充分表示的一組項目中選取單一值，請使用下拉式清單。
- 若要顯示包含多行文字或影像的項目，請使用清單或資料格檢視，而不要使用下拉式清單。
- 如果項目少於五個，請考慮使用[選項按鈕](radio-button.md) (如果是單選) 或[核取方塊](checkbox.md) (如果是複選)。
- 如果選項項目在您應用程式流程中的重要性為次要，請使用下拉式方塊。 如果針對大部分使用者在大多數情況下建議使用預設選項，則使用清單方塊來顯示所有項目可能會讓使用者浪費過多注意力在選項上。 您可以藉由使用下拉式方塊來節省空間，以及減少注意力分散的情形。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/ComboBox">開啟應用程式並查看 ComboBox 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
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

直接將物件新增至 [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)集合，或將 [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性設為資料來源，即可填入下拉式方塊。 新增至 ComboBox 的項目會包裝在 [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) 容器中。

以下是在 XAML 中新增項目的簡單下拉式方塊。

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

下列範例示範如何將下拉式方塊繫結至 FontFamily 物件集合。

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

如同 ListView 和 GridView，ComboBox 衍生自[選取器](/uwp/api/windows.ui.xaml.controls.primitives.selector)，因此您可以用相同的標準方式取得使用者的選取項目。

您可以使用 [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 屬性來取得或設定下拉式方塊的已選取項目，以及使用 [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) 屬性來取得或設定所選項目的索引。

若要在選取的資料項目上取得特定屬性的值，您可以使用 [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) 屬性。 在此情況下，設定 [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) 來指定要從所選項目上的哪個屬性取得此值。

> [!TIP]
> 如果您設定 SelectedItem 或 SelectedIndex 來表示預設選取項目，如果在填入下拉式方塊 Items 集合前設定此屬性，就會發生例外狀況。 除非您在 XAML 中定義您的 Items，否則最好要處理下拉式方塊 Loaded 事件，並在 Loaded 事件處理常式中設定 SelectedItem 或 SelectedIndex。

您可以在 XAML 中繫結至這些屬性，或處理 [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)事件來回應選取項目變更。

在事件處理常式程式碼中，您可以從 [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 屬性取得所選的項目。 您可以從 [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) 屬性取得先前選取的項目 (如果有的話)。 AddedItems 和 RemovedItems 集合各自只包含 1 個項目，因為下拉式方塊並不支援多個選取項目。

這個範例示範如何處理 SelectionChanged 事件，以及如何繫結至選取的項目。

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

根據預設，當使用者對清單中的項目按一下、點選或按 Enter 鍵來認可其選取項目時，就會發生 SelectionChanged 事件，且下拉式方塊會關閉。 當使用者使用鍵盤方向鍵瀏覽開啟的下拉式方塊清單時，選取項目不會變更。

若要製作一個在使用者使用方向鍵尋覽開啟的清單時能「即時更新」的下拉式方塊 (例如字型選取下拉式清單)，請將 [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) 設定為 [Always](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger)。 當焦點變更為已開啟清單中的另一個項目時，這會導致 SelectionChanged 事件發生。

#### <a name="selected-item-behavior-change"></a>選取的項目行為變更

在 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本中，所選項目的行為會更新以支援可編輯的下拉式方塊。

在 SDK 17763 以前，SelectedItem 屬性的值 (因此 SelectedValue 和 SelectedIndex) 必須在下拉式方塊的 Items 集合中。 使用上述範例，設定 `colorComboBox.SelectedItem = "Pink"` 會導致：

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

在 SDK 17763 和更新版本中，SelectedItem 屬性的值 (因此 SelectedValue 和 SelectedIndex) 不一定要在下拉式方塊的 Items 集合中。 使用上述範例，設定 `colorComboBox.SelectedItem = "Pink"` 會導致：

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>文字搜尋

下拉式方塊可自動支援在其集合內的搜尋。 當焦點在一個已開啟或關閉的下拉式方塊上時，如果使用者在實體鍵盤上輸入字元，就會顯示與使用者的字串相符的候選項目。 在瀏覽長清單時，這項功能特別有幫助。 例如，與包含狀態清單的下拉式清單進行互動時，使用者可以按 "w" 鍵來顯示 "Washington 以供快速選取。 文字搜尋不區分大小寫。

您可以將 [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) 屬性設定為 **false**，以停用這項功能。

## <a name="make-a-combo-box-editable"></a>讓下拉式方塊可進行編輯

> [!IMPORTANT]
> 這項功能需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本。

根據預設，下拉式方塊可讓使用者從預先定義的選項清單中選取。 不過，有時候清單只包含一部分的有效值，而且使用者應該能夠輸入其他未列出的值。 若要支援這項功能，您可以讓下拉式方塊可進行編輯。

若要讓下拉式方塊可進行編輯，請將 [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) 屬性設定為 **true**。 然後，處理 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 事件來處理使用者所輸入的值。

根據預設，當使用者認可自訂文字時，就會更新 SelectedItem 值。 在 TextSubmitted 事件引數中將 **Handled**設定為 **true**，即可覆寫此行為。 當事件標記為已處理時，下拉式方塊不會在事件之後採取進一步動作，並將維持編輯中狀態。 將不會更新 SelectedItem。

這個範例顯示簡單的可編輯下拉式方塊。 此清單包含簡單的字串，並使用使用者所輸入的任何值。

「最近使用的名稱」選擇器可讓使用者輸入自訂字串。 'RecentlyUsedNames' 清單包含可供使用者選擇的一些值，但使用者也可以新增自訂值。 'CurrentName' 屬性表示目前輸入的名稱。

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>提交的文字

您可以處理 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 事件，以處理使用者所輸入的值。 在事件處理常式中，您通常會驗證使用者所輸入的值是否有效，然後在您的應用程式中使用此值。 視情況而定，您也可以將此值新增至下拉式方塊的選項清單中，以供未來使用。

符合下列條件時，就會發生 TextSubmitted 事件：

- IsEditable 屬性為 **true**
- 使用者在下拉式方塊清單中輸入不符合現有項目的文字
- 使用者按 Enter 鍵，或從下拉式方塊移出焦點。

如果使用者輸入文字，然後向上或向下巡覽清單，則不會發生 TextSubmitted 事件。

### <a name="sample---validate-input-and-use-locally"></a>範例 - 驗證輸入並在本機使用

在此範例中，字型大小選擇器包含一組對應至字型大小坡形的值，但使用者可以輸入不在清單中的字型大小。

當使用者新增不在清單中的值時，字型大小會更新，但此值不會新增至字型大小清單。

如果新輸入的值無效，您可使用 SelectedValue，將 Text 屬性還原為最後一個已知良好的值。

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
        // Update the app's font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn't update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>範例 - 驗證輸入並新增至清單

在此，「最愛色彩選擇器」包含最常見的最愛色彩 (紅色、藍色、綠色、橘色)，但使用者可以輸入不在清單中的最愛色彩。 當使用者新增有效的色彩 (例如粉紅色) 時，新輸入的色彩會新增到清單並設為有效的「最愛色彩」。

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
        // Mark the event as handled so the framework doesn't update the selected item.
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

## <a name="list-boxes"></a>清單方塊

清單方塊可讓使用者從集合中選擇一個項目或多個項目。 清單方塊類似下拉式清單，不同的是清單方塊一律會開啟；清單方塊沒有精簡 (非展開) 狀態。 如果無法一次顯示所有項目，則可以捲動清單中的項目。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

- 當清單中的項目很重要而必須特別顯示，以及當有足夠的螢幕實際可用空間可以顯示完整的清單時，清單方塊很有用。
- 清單方塊應該讓使用者在進行重大選擇時，能夠注意到整組替代項目。 相反地，下拉式清單一開始就會吸引使用者對於選取的項目的注意。
- 如果有下列情形，則避免使用清單方塊：
    - 清單中只有非常少量的項目。 總是有相同的 2 個選項的單選清單方塊，最好以[選項按鈕](radio-button.md)的形式呈現。 清單中有 3 個或 4 個靜態項目時，也可以考慮使用選項按鈕。
    - 清單方塊屬於單選形式，且總是有相同的 2 個選項暗示彼此互異，例如「開」與「關」。 請使用單一核取方塊或切換開關。
    - 有非常大量的項目。 針對較長的清單最好選擇資料格檢視與清單檢視。 針對清單非常冗長的分組資料，最好使用語意式縮放。
    - 項目為連續數值。 如果是這種情況，請考慮[滑桿](slider.md)。
    - 對於大部分案例中的多數使用者，建議選取項目為使用應用程式流程中重要性居次的項目或預設選項。 請改用下拉式清單。

### <a name="recommendations"></a>建議

- 清單方塊中理想的項目範圍是 3 到 9 個。
- 即使其中的項目大不相同，清單方塊仍能正常運作。
- 盡可能將清單方塊的大小設定為不需要移動瀏覽或捲動項目清單。
- 確認清單方塊的用途和目前選取的項目顯而易見。
- 保留觸控回饋和已選取項目狀態的視覺化效果與動畫。
- 清單方塊項目的文字內容以一行為限。 如果項目是視覺元素，您可以自訂項目的大小。 如果項目包含多行文字或影像，請改用資料格檢視或清單檢視。
- 除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
- 不要使用清單方塊執行命令，或動態顯示或隱藏其他控制項。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。
- [AutoSuggestBox 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>相關文章

- [文字控制項](text-controls.md)
- [拼字檢查](text-controls.md)
- [搜尋](search.md)
- [TextBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Windows.UI.Xaml.Controls PasswordBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length 屬性](https://docs.microsoft.com/dotnet/api/system.string.length)
