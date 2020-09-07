---
description: 了解如何使用選項按鈕，讓使用者從兩個或多個互斥但相關的選項集合中選取一個選項。
title: 選項按鈕的指導方針
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 06/10/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7d09eaefff193a8283fd4bad68528b8976e0b63b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169782"
---
# <a name="radio-buttons"></a>選項按鈕

選項按鈕可讓使用者從兩個或多個互斥但相關的選項集合中選取一個選項。 每個選項都是以一個選項按鈕表示。

在預設狀態下，不會選取 RadioButtons 群組中的任何選項按鈕。 也就是說，清除所有選項按鈕。 不過，在選取了選項按鈕後，就無法還原群組的已清除狀態。

RadioButtons 群組的單一行為會與[核取方塊](checkbox.md) (可支援多重選取和取消選取或清除) 的單一行為有所區別。

![RadioButtons 群組的範例，其中已選取一個選項按鈕](images/controls/radio-button.png)

## <a name="get-the-windows-ui-library"></a>取得 Windows UI 程式庫

| &nbsp; | &nbsp; |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | RadioButtons控制項包含在 Windows UI 程式庫中；該程式庫是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。 |

**Windows UI 程式庫 API**： 
* [RadioButtons 類別](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
* [SelectionChanged 事件](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
* [SelectedItem 屬性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)
* [SelectedIndex 屬性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)

**平台 API**： 
* [RadioButton 類別](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) (英文)
* [Checked 事件](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)
* [IsChecked 屬性](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用選項按鈕可讓使用者從兩個或更多互斥選項中選取。

![一個 RadioButtons 群組，其中已選取一個選項按鈕](images/radiobutton_basic.png)

當使用者需要先查看所有選項再做選擇時，請使用選項按鈕。 選項按鈕同等地強調所有選項，這表示有些選項可能會引起不必要或不想要的注意。 

除非所有選項都值得同等注意，否則請考慮使用其他控制項。 例如，若要為大多數使用者以及在大部分情況下建議單一最佳選項，請使用[下拉式方塊](combo-box.md)，將該最佳選項顯示為預設選項。

![顯示預設選項的下拉式方塊](images/combo_box_collapsed.png)

如果只有兩個選項，而且彼此互斥，請將其組成單一[核取方塊](checkbox.md)或[切換開關](toggles.md)控制項。 例如，使用「我同意」的單一核取方塊來代替「我同意」和「我不同意」兩個選項按鈕。

![核取方塊是呈現二進位選擇的不錯替代方式](images/radiobutton_vs_checkbox.png)

使用者可以選取多個選項時，請使用[核取方塊](checkbox.md)。

![核取方塊支援多重選取](images/checkbox2.png)

當使用者的選項位於某個範圍內的值 (例如，*10、20、30、...100*) 時，請使用[滑桿](slider.md)控制項。

![滑桿控制項，其中顯示值範圍內的某個值](images/controls/slider.png)

如果有八個或以上選項，請使用[下拉式方塊](combo-box.md)。

![顯示多個選項的清單方塊](images/combo_box_scroll.png)

> [!NOTE]
> 如果可用的選項取決於應用程式目前的內容，或者可能會不斷地變化，請使用清單控制項。

## <a name="radiobuttons-behavior"></a>RadioButtons 行為

鍵盤存取和導覽行為已在 [RadioButton 類別](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041)中進行最佳化。 這些改善可同時協助鍵盤和協助工具進階使用者在選項清單中更快速且輕鬆地移動。

除了這些改善之外，RadioButtons 群組中個別選項按鈕的預設視覺版面配置也已透過自動化方向、間距和邊界設定進行最佳化。 此最佳化消除了指定這些屬性的需求，因為您在使用更基本的群組控制項 例如 [StackPanel](../layout/layout-panels.md#stackpanel) 或 [Gird](../layout/layout-panels.md#grid) 時可能必須這麼做。

### <a name="navigating-a-radiobuttons-group"></a>瀏覽 RadioButtons 群組

RadioButtons 控制項支援兩種狀態：

- 未選取任何選項按鈕
- 已選取一個選項按鈕

下列兩節涵蓋這兩種選項按鈕焦點行為。

#### <a name="no-radio-button-is-selected"></a>未選取任何選項按鈕

未選取任何選項按鈕時，清單中的第一個選項按鈕會取得焦點。

> [!NOTE]
> 不會選取從初始 Tab 瀏覽接收 Tab 焦點的項目。

|不具 Tab 焦點的清單 | 具有初始 Tab 焦點的清單|
|:--:|:--:|
| ![不具 Tab 焦點的清單](images/radiobutton-no-selected-item-no-tab-focus.png) | ![具有初始 Tab 焦點的清單](images/radiobutton-no-selected-item-tab-focus.png)|

#### <a name="one-radio-button-is-selected"></a>已選取一個選項按鈕

選取選項按鈕且使用者透過 Tab 鍵進入清單時，選取的選項按鈕就會取得焦點。

|不具 Tab 焦點的清單 | 具有初始 Tab 焦點的清單 |
|:--:|:--:|
| ![不具 Tab 焦點的清單](images/radiobutton-selected-item-no-tab-focus.png) | ![具有初始 Tab 焦點的清單](images/radiobutton-selected-item-tab-focus.png)|


### <a name="keyboard-navigation"></a>鍵盤瀏覽

當使用者具有選項按鈕選項的單一列或欄，且項目已經收到 Tab 焦點時，他們可以使用方向鍵，在 RadioButtons 控制項內的項目之間進行「內部瀏覽」。 如需鍵盤瀏覽行為的詳細資訊，請參閱[鍵盤互動 - 瀏覽](../input/keyboard-interactions.md#navigation)。

若為 RadioButtons 控制項，當選項清單僅垂直排列時，向上鍵和向下鍵會在項目之間移動，而向左鍵和向右鍵不會執行任何動作。 不過，在僅水平排列的清單中，向左/向右和向上/向下鍵都會以相同方式在項目之間移動。

![單一欄或單一列 RadioButtons 群組中的鍵盤瀏覽範例](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*單一欄或單一列 RadioButtons 群組中的鍵盤瀏覽範例*

#### <a name="navigating-within-multi-column-or-multi-row-layouts"></a>在多欄或多列版面配置內瀏覽

在以欄為主的順序中，焦點會從上到下、由左到右移動。 當焦點位於欄中的最後一個項目上，按下向下鍵時，焦點會移至下一欄的第一個項目。 反之亦會發生此相同行為：當焦點設定至欄中的第一個項目上，按下向上鍵時，焦點移至上一欄的最後一個項目。

![多欄/列 RadioButtons 群組中的鍵盤瀏覽範例](images/radiobutton-keyboard-navigation-multi-column-row.png)

以列為主順序 (項目依序由左至右、由上至下填入)，當焦點位於列中的最後一個項目上，按下向右鍵時，焦點會移至下一列中的第一個項目。 反之亦會發生此相同行為：當焦點設定至列中的第一個項目上，按下向左鍵時，焦點移至上一列的最後一個項目。

如需詳細資訊，請參閱[鍵盤互動](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items)。

##### <a name="wrapping"></a>換行

RadioButtons 群組不會換行。 這是因為當使用螢幕助讀程式時，界限的意義以及開始和結束的清楚指示會遺失，這使得視力受損的使用者難以瀏覽清單。 RadioButtons 控制項也不支援列舉，因為控制項的用意是要包含合理的項目數 (請參閱[這是正確的控制項嗎？](#is-this-the-right-control))。

## <a name="selection-follows-focus"></a>選取項目追隨焦點

當使用者使用鍵盤在 RadioButtons 清單中的項目之間瀏覽 (其中已經選取一個項目) 時，當焦點從某個項目移至下一個項目時，就會選取新聚焦的項目，並清除先前聚焦的項目。

|鍵盤瀏覽之前 | 鍵盤瀏覽之後|
|:--|:--|
| ![鍵盤瀏覽之前的焦點和選取範例](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*鍵盤瀏覽之前的焦點和選取範例* | ![鍵盤瀏覽之後的焦點和選取範例](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*鍵盤瀏覽之後的焦點和選取範例，其中的向下或向右鍵會將焦點移至選項按鈕 3、選取此選項按鈕，然後清除選項按鈕 2* |

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>使用 Xbox 遊戲台和遙控器進行瀏覽

如果使用者使用 Xbox 遊戲台或遙控器在選項按鈕之間移動，則會停用「選取項目追隨焦點」行為，而且使用者必須按下 "A" 按鈕才能選取聚焦的選項按鈕。

## <a name="accessibility-behavior"></a>協助工具行為

下表描述朗讀程式如何處理 RadioButtons 群組和宣告的內容。 這種行為取決於使用者如何設定朗讀程式詳細資料喜好設定。

| 初始焦點 | 焦點移至選取的項目 |
|:--|:--|
| 「群組名稱」RadioButton 集合具有焦點，且已選取 N 個項目的項目 x | 如果選取 RadioButton「名稱」，則項目 x 具有焦點。 |
| 「群組名稱」RadioButton 集合具有焦點，且未選取任何項目| 如果未選取 RadioButton「名稱」，則項目 x 具有焦點。 <br> 如果使用者使用 shift-方向鍵，則沒有任何選取項目跟隨焦點。 |

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="The XAML Controls Gallery app icon"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請<a href="xamlcontrolsgallery:/item/RadioButton">開啟應用程式以查看 RadioButtons 控制項的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="using-the-winui-radiobuttons-control"></a>使用 WinUI RadioButtons 控制項

如果您使用 [WinUI](https://github.com/microsoft/microsoft-ui-xaml)，建議使用 [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons) 控制項。

RadioButtons 控制項很容易設定和使用，並可確保適當且預期的鍵盤和朗讀程式行為。

在下列程式碼中，您宣告具有三個選項的基本 RadioButtons 控制項：

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```
結果顯示在下圖中：

![兩個群組中的選項按鈕](images/default-radiobutton-group.png)

### <a name="defining-multiple-columns"></a>定義多欄

您可藉由指定 [MaxColumns 屬性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns)，宣告多欄的 RadioButtons 控制項。

```xaml
<muxc:RadioButtons Header="App Mode" MaxColumns="3">
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
</muxc:RadioButtons>
```

![兩個三欄群組中的選項按鈕](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>資料繫結

RadioButtons 控制項支援使用其 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) 屬性的資料繫結，如下列程式碼片段所示。

```xaml
<RadioButtons Header="App Mode" ItemsSource="{x:Bind radioButtonItems}" />
```

```c#
public sealed partial class MainPage : Page
{
    public class OptionDataModel
    {
        public string Label;
        public override string ToString()
        {
            return Label;
        }
    }

    List<OptionDataModel> radioButtonItems;

    public MainPage()
    {
        this.InitializeComponent();

        radioButtonItems = new List<OptionDataModel>();
        radioButtonItems.Add(new OptionDataModel() { label = "Item 1" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 2" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 3" });
    }
}
```

## <a name="create-your-own-radiobuttons-group"></a>建立您自己的 RadioButtons 群組

> [!Important]
> 除非您使用舊版的 WinUI，否則我們建議使用 WinUI RadioButtons 控制項將 RadioButton 元素分組。

選項按鈕以群組方式運作。 您可以使用下列兩種方式之一，將選項按鈕分組：

- 將它們放入同一個父容器。
- 將每個選項按鈕的 [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) 屬性設定為相同的值。

在這個範例中，第一個選項按鈕群組位於相同的堆疊面板中，藉此以隱含方式群組化。 第二個群組分成兩個堆疊面板，所以是依 GroupName 明確分組。

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

下圖顯示此 RadioButtons 群組的呈現方式：

![兩個群組中的選項按鈕](images/radio-button-groups.png)

## <a name="radio-button-states"></a>選項按鈕狀態

選項按鈕有兩個狀態：已選取或已清除。 已選取選項按鈕時，其 [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) 屬性為 `true`。 已選取選項按鈕時，其 IsChecked 屬性為 `false`。 如果使用者選取同一個群組中的另一個選項按鈕，則可清除選項按鈕，但如果使用者再次選取，則無法將其清除。 不過，您可以將選項按鈕的 IsChecked 屬性設定為 `false`，以程式設計方式清除該選項按鈕。

## <a name="recommendations"></a>建議

- 確定一組選項按鈕的目的和目前狀態明確。
- 將選項按鈕的文字標籤限制為單行。
- 如果文字標籤是動態的，請考慮按鈕將如何自動調整大小以及調整後周圍的任何視覺效果會發生什麼變化。
- 除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
- 不要將兩個 RadioButtons 群組並排。 當兩個 RadioButtons 群組並列時，使用者很難判斷哪個按鈕屬於哪個群組。

### <a name="visuals-to-consider"></a>要考慮的視覺效果

下列影像顯示在 RadioButtons 群組中排列選項按鈕的最佳方式。

![顯示一組選項按鈕的影像，這些選項按鈕垂直排列](images/radiobutton-layout.png)

![顯示選項按鈕間距指導方針的影像](images/radiobutton-redline.png)

> [!NOTE]
> 如果您使用 WinUI RadioButtons 控制項，則間距、邊界和方向均已最佳化。

## <a name="get-the-sample-code"></a>取得範例程式碼

- 若要以互動式格式取得所有 XAML 控制項，請參閱 [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery)。 

## <a name="related-topics"></a>相關主題

### <a name="for-designers"></a>適用於設計人員

- [按鈕](buttons.md)
- [切換開關](toggles.md)
- [核取方塊](checkbox.md)
- [清單和下拉式方塊](lists.md)
- [滑桿](slider.md)

### <a name="for-developers-xaml"></a>適用於開發人員 (XAML)

- [RadioButton 類別](/uwp/api/windows.ui.xaml.controls.radiobutton) (英文)