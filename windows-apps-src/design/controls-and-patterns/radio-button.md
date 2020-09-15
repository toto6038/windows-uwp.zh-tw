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
ms.openlocfilehash: 4953b5adc1953bac83b90271b4042e3b9f13c3f2
ms.sourcegitcommit: 083ddf840ab42bb48b4892fc2876ecbf698e481b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89615528"
---
# <a name="radio-buttons"></a>選項按鈕

選項按鈕可讓使用者從兩個或多個互斥但相關的選項集合中選取一個選項。 選項按鈕一律會在群組中使用，而且每個選項都會由群組中的一個選項按鈕來表示。

在預設狀態下，不會選取 RadioButtons 群組中的任何選項按鈕。 也就是說，清除所有選項按鈕。 不過，一旦使用者選取了選項按鈕，就無法取消選取按鈕而讓群組還原到其初始清除狀態。

RadioButtons 群組的單一行為會與[核取方塊](checkbox.md) (可支援多重選取和取消選取或清除) 的單一行為有所區別。

:::image type="content" source="images/controls/radio-button.png" alt-text="RadioButtons 群組的範例，其中已選取一個選項按鈕":::

**取得 Windows UI 程式庫**

| &nbsp; | &nbsp; |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | RadioButtons控制項包含在 Windows UI 程式庫中；該程式庫是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。 |

> **Windows UI 程式庫 API**：[RadioButtons 類別](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)、[SelectedItem 屬性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)、[SelectedIndex 屬性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)、[SelectionChanged 事件](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
>
> **平台 API**：[RadioButton 類別](/uwp/api/Windows.UI.Xaml.Controls.RadioButton)、[IsChecked 屬性](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)、[Checked 事件](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)

> [!IMPORTANT]
> **RadioButtons 與 RadioButton**
>
>有兩種方式可建立選項按鈕群組。
>
>- 從 WinUI 2.3 開始，我們建議使用 **[RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)** 控制項。 此控制項可簡化版面配置、處理鍵盤瀏覽和協助工具，以及支援繫結至資料來源。
>- 您可以使用個別 **[RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)** 控制項的群組。 如果您的應用程式未使用 WinUI 2.3 或更新版本，則這是唯一的選項。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用選項按鈕可讓使用者從兩個或更多互斥選項中選取。

:::image type="content" source="images/radiobutton_basic.png" alt-text="一個 RadioButtons 群組，其中已選取一個選項按鈕":::

當使用者需要先查看所有選項再做選擇時，請使用選項按鈕。 選項按鈕同等地強調所有選項，這表示有些選項可能會引起不必要或不想要的注意。

除非所有選項都值得同等注意，否則請考慮使用其他控制項。 例如，若要為大多數使用者以及在大部分情況下建議單一最佳選項，請使用[下拉式方塊](combo-box.md)，將該最佳選項顯示為預設選項。

:::image type="content" source="images/combo_box_collapsed.png" alt-text="顯示預設選項的下拉式方塊":::

如果只有兩個可以明確表達為單一二元選擇的可能選項 (例如「開啟/關閉」或「是/否」)，請將這兩個選項結合成單一的[核取方塊](checkbox.md)或[切換開關](toggles.md)控制項。 例如，使用「我同意」的單一核取方塊來代替「我同意」和「我不同意」兩個選項按鈕。

請勿對單一的二元選擇使用兩個選項按鈕：

:::image type="content" source="images/radiobutton-vs-checkbox-rb.png" alt-text="呈現二元選擇的兩個選項按鈕":::

請改為使用核取方塊：

:::image type="content" source="images/radiobutton-vs-checkbox-cb.png" alt-text="核取方塊是呈現二進位選擇的不錯替代方式":::

使用者可以選取多個選項時，請使用[核取方塊](checkbox.md)。

:::image type="content" source="images/checkbox2.png" alt-text="核取方塊支援多重選取":::

當使用者的選項位於某個範圍內的值 (例如，*10、20、30、...100*) 時，請使用[滑桿](slider.md)控制項。

:::image type="content" source="images/controls/slider.png" alt-text="滑桿控制項，其中顯示值範圍內的某個值":::

如果有超過 8 個選項，請使用[下拉式方塊](combo-box.md)。

:::image type="content" source="images/combo_box_scroll.png" alt-text="顯示多個選項的清單方塊":::

如果可用的選項取決於應用程式目前的內容，或者可能會不斷地變化，請使用清單控制項。

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

## <a name="winui-radiobuttons-overview"></a>WinUI RadioButtons 概觀

[RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons) 控制項已將鍵盤存取和瀏覽行為最佳化。 這些改善可同時協助鍵盤和協助工具進階使用者在選項清單中更快速且輕鬆地移動。

除了這些改善之外，RadioButtons 群組中個別選項按鈕的預設視覺版面配置也已透過自動化方向、間距和邊界設定進行最佳化。 此最佳化消除了指定這些屬性的需求，因為您在使用更基本的群組控制項 例如 [StackPanel](../layout/layout-panels.md#stackpanel) 或 [Gird](../layout/layout-panels.md#grid) 時可能必須這麼做。

### <a name="navigating-a-radiobuttons-group"></a>瀏覽 RadioButtons 群組

`RadioButtons` 控制項具有特殊的瀏覽行為，可協助鍵盤使用者更快速且更輕鬆地瀏覽清單。

#### <a name="keyboard-focus"></a>鍵盤焦點

`RadioButtons` 控制項支援兩種狀態：

- 未選取任何選項按鈕
- 已選取一個選項按鈕

後面幾節將說明控制項在這兩種狀態中各自的焦點行為。

##### <a name="no-radio-button-is-selected"></a>未選取任何選項按鈕

未選取任何選項按鈕時，清單中的第一個選項按鈕會取得焦點。

> [!NOTE]
> 不會選取從初始 Tab 瀏覽接收 Tab 焦點的項目。

|不具 Tab 焦點的清單 | 具有初始 Tab 焦點的清單|
|:--:|:--:|
| ![不具 Tab 焦點，而且沒有已選取項目的清單](images/radiobutton-no-selected-item-no-tab-focus.png) | ![具有初始 Tab 焦點，而且沒有已選取項目的清單](images/radiobutton-no-selected-item-tab-focus.png)|

##### <a name="one-radio-button-is-selected"></a>已選取一個選項按鈕

當使用者透過 Tab 鍵進入已選取選項按鈕的清單時，選取的選項按鈕就會取得焦點。

|不具 Tab 焦點的清單 | 具有初始 Tab 焦點的清單 |
|:--:|:--:|
| ![不具 Tab 焦點和已選取項目的清單](images/radiobutton-selected-item-no-tab-focus.png) | ![具有初始 Tab 焦點和已選取項目的清單](images/radiobutton-selected-item-tab-focus.png)|

#### <a name="keyboard-navigation"></a>鍵盤瀏覽

如需一般鍵盤瀏覽行為的詳細資訊，請參閱[鍵盤互動 - 瀏覽](../input/keyboard-interactions.md#navigation)。

當 `RadioButtons` 群組中的項目已有焦點時，使用者可以使用方向鍵在群組內的項目之間進行「內部瀏覽」。 向上鍵和向下鍵會移至 XAML 標記中所定義的「下一個」或「上一個」邏輯項目。 向左鍵和向右鍵則會進行空間上的移動。

##### <a name="navigation-within-single-column-or-single-row-layouts"></a>單欄或單列版面配置內的瀏覽

在單欄或單列的版面配置中，鍵盤瀏覽會產生下列行為：

|單欄 | 單一資料列|
|:--|:--|
| ![單欄 RadioButtons 群組中的鍵盤瀏覽範例](images/radiobutton-keyboard-navigation-single-column.png)</br>向上鍵和向下鍵會在項目之間移動。</br>向左鍵和向右鍵鍵不會執行任何動作。 | ![單列 RadioButtons 群組中的鍵盤瀏覽範例](images/radiobutton-keyboard-navigation-single-row.png)<br/>向左鍵和向上鍵會移至上一個項目，向右鍵和向下鍵則會移至下一個項目。 |

##### <a name="navigation-within-multi-column-multi-row-layouts"></a>多欄、多列版面配置內的瀏覽

在多欄、多列的格狀版面配置中，鍵盤瀏覽會產生以下行為：

|向左鍵/向右鍵| 向上鍵/向下鍵 |
|:--|:--|
| ![多欄/列 RadioButtons 群組中的水平鍵盤瀏覽範例](images/radiobutton-keyboard-navigation-multi-column-row-1.png)</br>向左鍵和向右鍵會在某列的項目之間水平移動焦點。 | ![多欄/列 RadioButtons 群組中的垂直鍵盤瀏覽範例](images/radiobutton-keyboard-navigation-multi-column-row-2.png)<br/>向上鍵和向下鍵會在某欄的項目之間垂直移動焦點。 |
| ![焦點位於某欄最後一個項目的水平鍵盤瀏覽範例](images/radiobutton-keyboard-navigation-multi-column-row-3.png)</br> 當焦點位於某欄最後一個項目上，並按下向右鍵或向左鍵時，焦點會移至下一欄或上一欄的最後一個項目 (如果有的話)。 | ![焦點位於某欄最後一個項目的垂直鍵盤瀏覽範例](images/radiobutton-keyboard-navigation-multi-column-row-4.png)<br/>當焦點位於某欄最後一個項目上，並按下向下鍵時，焦點會移至下一欄的第一個項目 (如果有的話)。 當焦點位於某欄第一個項目上，並按下向上鍵時，焦點會移至上一欄的最後一個項目 (如果有的話)。 |

如需詳細資訊，請參閱[鍵盤互動](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items)。

###### <a name="wrapping"></a>換行

RadioButtons 群組不會將焦點從第一列或第一欄換行至最後一列或最後一欄，也不會從最後一列或最後一欄換行至第一列或第一欄。 這是因為當使用螢幕助讀程式時，界限的意義以及開始和結束的清楚指示會遺失，這使得視力受損的使用者難以瀏覽清單。

`RadioButtons` 控制項也不支援列舉，因為控制項的用意是要包含合理的項目數 (請參閱[這是正確的控制項嗎？](#is-this-the-right-control))。

### <a name="selection-follows-focus"></a>選取項目追隨焦點

當您使用鍵盤在 `RadioButtons` 群組中的項目之間瀏覽時，當焦點從某個項目移至下一個項目時，就會選取新聚焦的項目，並清除先前聚焦的項目。

|鍵盤瀏覽之前 | 鍵盤瀏覽之後|
|:--|:--|
| ![鍵盤瀏覽之前的焦點和選取範例](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*鍵盤瀏覽之前的焦點和選取範例* | ![鍵盤瀏覽之後的焦點和選取範例](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*鍵盤瀏覽之後的焦點和選取範例，其中的向下鍵會將焦點移至選項按鈕 3、選取此選項按鈕，然後清除選項按鈕 2* |

您可以使用「Ctrl+方向鍵」來進行瀏覽，以便移動焦點而不變更選取項目。 焦點移動之後，您可以使用空格鍵來選取焦點目前所在的項目。

#### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>使用 Xbox 遊戲台和遙控器進行瀏覽

如果您使用 Xbox 遊戲台或遙控器在選項按鈕之間移動，則會停用「選取項目追隨焦點」行為，而且使用者必須按下 "A" 鍵才能選取焦點目前所在的選項按鈕。

## <a name="accessibility-behavior"></a>協助工具行為

下表描述朗讀程式如何處理 `RadioButtons` 群組和宣告的內容。 這種行為取決於使用者如何設定朗讀程式詳細資料喜好設定。

| 動作 | 朗讀程式公告 |
|:--|:--|
| 焦點移至選取的項目 | 「_名稱_ RadioButton 已選取第 _x_ 項，共 _N_ 項」 |
|焦點移至未選取的項目<br> *(如果使用 Ctrl-方向鍵或 Xbox 遊戲台進行瀏覽，<br>這表示選取項目不會追隨焦點。)* | 「_名稱_ RadioButton 未選取第 _x_ 項，共 _N_ 項」  |

> [!NOTE]
> 朗讀程式針對每個項目所公告的_**名稱**_ 是 [AutomationProperties.Name](/uwp/api/windows.ui.xaml.automation.automationproperties.nameproperty) 已連結屬性的值 (如果可供項目使用)；否則，該名稱會是項目的 [ToString](/dotnet/api/system.object.tostring?view=dotnet-uwp-10.0) 方法所傳回的值。
>
> _**x**_ 是目前項目的編號。 _**N**_ 是群組中的項目總數。

## <a name="create-a-winui-radiobuttons-group"></a>建立 WinUI RadioButtons 群組

`RadioButtons` 控制項會使用類似 [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) 的內容模型。 這表示您可以：

- 將項目直接新增至 [Items](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.items) 集合，或將資料繫結至其 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) 屬性，來進行填入作業。
- 使用 [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) 或 [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) 屬性，以取得並設定要選取的選項。
- 處理要在選擇了選項時採取動作的 [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) 事件。

> [!TIP]
> 在這整份文件中，我們使用 XAML 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已將此新增至我們的 [Page](/uwp/api/windows.ui.xaml.controls.page) 元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在後方的程式碼中，我們也使用 C# 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已在檔案頂端新增了此 **using** 陳述式：`using muxc = Microsoft.UI.Xaml.Controls;`

在這裡，您將宣告有三個選項的簡單 `RadioButtons` 控制項。 您可以設定 [Header](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.header) 屬性來為群組提供標籤，以及設定 `SelectedIndex` 屬性來提供預設選項。

```xaml
<muxc:RadioButtons Header="Background color"
                   SelectedIndex="0"
                   SelectionChanged="BackgroundColor_SelectionChanged">
    <x:String>Red</x:String>
    <x:String>Green</x:String>
    <x:String>Blue</x:String>
</muxc:RadioButtons>
```

結果如下所示：

:::image type="content" source="images/radiobuttons-default-group.png" alt-text="三個選項按鈕的群組":::

若要在使用者選取選項時採取動作，請處理 [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) 事件。 在這裡，您會變更名為 "ExampleBorder" (`<Border x:Name="ExampleBorder" Width="100" Height="100"/>`) 的 [Border](/uwp/api/windows.ui.xaml.controls.border) 元素背景色彩。

```csharp
private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ExampleBorder != null && sender is muxc.RadioButtons rb)
    {
        string colorName = rb.SelectedItem as string;
        switch (colorName)
        {
            case "Red":
                ExampleBorder.Background = new SolidColorBrush(Colors.Red);
                break;
            case "Green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
        }
    }
}
```

> [!TIP]
> 您也可以從 [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 屬性取得所選的項目。 只會有一個位於索引 0 的已選取項目，因此您可以獲得如下的已選取項目：`string colorName = e.AddedItems[0] as string;`。

### <a name="selection-states"></a>選取狀態

選項按鈕有兩個狀態：已選取或已清除。 在 `RadioButtons` 群組中選取了選項時，您可以從 [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) 屬性獲得其值，並從 [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) 屬性獲得其在集合中的位置。 如果使用者選取同一個群組中的另一個選項按鈕，則可清除選項按鈕，但如果使用者再次選取，則無法將其清除。 不過，您可以將選項按鈕群組設定為 `SelectedItem = null` 或 `SelectedIndex = -1`，以程式設計方式清除該選項按鈕。 (將 `SelectedIndex` 設定為 `Items` 集合範圍以外的任何值，會產生未選取任何項目的狀態)。

### <a name="radiobuttons-content"></a>RadioButtons 內容

在上述範例中，您已使用簡單的字串填入 `RadioButtons` 控制項。 此控制項會提供幾個選項按鈕，並使用字串作為每一個選項按鈕的標籤。

不過，您可以使用任何物件來填入 `RadioButtons` 控制項。 一般來說，您會希望物件提供可作為文字標籤的字串標記法。 在某些情況下，可能適合使用影像來取代文字。

在這裡，我們使用 [SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon) 元素來填入控制項。

```xaml
<muxc:RadioButtons Header="Choose the icon without an arrow">
    <SymbolIcon Symbol="Back"/>
    <SymbolIcon Symbol="Attach"/>
    <SymbolIcon Symbol="HangUp"/>
    <SymbolIcon Symbol="FullScreen"/>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-symbolicon.png" alt-text="具有符號圖示的群組選項按鈕":::

您也可以使用個別的 [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 控制項來填入 `RadioButtons` 項目。 我們稍後會討論這個特殊案例。 請參閱 [RadioButtons 群組中的 RadioButton 控制項]()。

能夠使用任何物件的優點是，您可以將 `RadioButtons` 控制項繫結至資料模型中的自訂類型。 下一節將示範這種情況。

### <a name="data-binding"></a>資料繫結

`RadioButtons` 控制項支援將資料繫結至其 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) 屬性。 這個範例會示範如何將控制項繫結至自訂資料來源。 這個範例的外觀和功能與先前的背景色彩範例相同，但在這裡，色彩筆刷會儲存在資料模型中，而不會建立在 `SelectionChanged` 事件處理常式中。

```xaml
 <muxc:RadioButtons Header="Background color"
                    SelectedIndex="0"
                    SelectionChanged="BackgroundColor_SelectionChanged"
                    ItemsSource="{x:Bind colorOptionItems}"/>
```

```c#
public sealed partial class MainPage : Page
{
    // Custom data item.
    public class ColorOptionDataModel
    {
        public string Label { get; set; }
        public SolidColorBrush ColorBrush { get; set; }

        public override string ToString()
        {
            return Label;
        }
    }

    List<ColorOptionDataModel> colorOptionItems;

    public MainPage1()
    {
        this.InitializeComponent();

        colorOptionItems = new List<ColorOptionDataModel>();
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Red", ColorBrush = new SolidColorBrush(Colors.Red) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Green", ColorBrush = new SolidColorBrush(Colors.Green) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Blue", ColorBrush = new SolidColorBrush(Colors.Blue) });
    }

    private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        var option = e.AddedItems[0] as ColorOptionDataModel;
        ExampleBorder.Background = option?.ColorBrush;
    }
}
```

### <a name="radiobutton-controls-in-a-radiobuttons-group"></a>RadioButtons 群組中的 RadioButton 控制項

您可以使用個別的 [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 控制項來填入 `RadioButtons` 項目。 您可以執行此動作來存取某些屬性，例如 `AutomationProperties.Name`；或者，您可能有現有的 `RadioButton` 程式碼，但想要利用 `RadioButtons` 的版面配置和瀏覽。

```xaml
<muxc:RadioButtons Header="Background color">
    <RadioButton Content="Red" Tag="red" AutomationProperties.Name="red"/>
    <RadioButton Content="Green" Tag="green" AutomationProperties.Name="green"/>
    <RadioButton Content="Blue" Tag="blue" AutomationProperties.Name="blue"/>
</muxc:RadioButtons>
```

當您使用 `RadioButtons` 群組中的 `RadioButton` 控制項時，`RadioButtons` 控制項會知道如何呈現 `RadioButton`，所以您最後不會得到兩個選取項目圓形。

不過，有一些您應該注意的行為。 我們建議您在個別控制項或 `RadioButtons` 上處理狀態和事件，但不要同時在兩者上進行，以免發生衝突。

下表顯示這兩個控制項的相關事件和屬性。

|RadioButton  |RadioButtons  |
|---------|---------|
|[已核取](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked)、[未核取](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked)、[按一下](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) |    [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) \(英文\) |
|[IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)  | [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)，[SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) |

如果您在個別 `RadioButton` 上處理事件 (例如 `Checked` 或 `Unchecked`)，同時也處理 `RadioButtons.SelectionChanged` 事件，則這兩個事件都會引發。 `RadioButton` 事件會先發生，然後 `RadioButtons.SelectionChanged` 事件再發生，這可能會導致衝突。

`IsChecked`、`SelectedItem` 和 `SelectedIndex` 屬性會保持同步。 變更其中一個屬性會更新其他兩個屬性。

[RadioButton.GroupName](/uwp/api/windows.ui.xaml.controls.radiobutton.groupname) 屬性會遭到忽略。 此群組是由 `RadioButtons` 控制項建立的。

### <a name="defining-multiple-columns"></a>定義多欄

根據預設，`RadioButtons` 控制項會在單欄中垂直排列其選項按鈕。 您可以設定 [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns) 屬性，讓控制項將選項按鈕排列在多欄中。 (當您這麼做時，選項按鈕會按照以欄為主的順序進行配置，項目會由上往下填入，然後由左往右填入)。

```xaml
<muxc:RadioButtons Header="Options" MaxColumns="3">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
    <x:String>Item 6</x:String>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-multi-column.png" alt-text="兩個三欄群組中的選項按鈕":::

> [!TIP]
> 若要讓項目排列在單一水平列中，請將 `MaxColumns` 設定為等於群組中的項目數。

## <a name="create-your-own-radiobutton-group"></a>建立您自己的 RadioButton 群組

> [!Important]
> 除非您使用舊版的 WinUI，否則我們建議使用 WinUI `RadioButtons` 控制項將 `RadioButton` 元素分組。

選項按鈕以群組方式運作。 您可以透過下列兩種方式之一，將個別 [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 控制項分組：

- 將它們放入同一個父容器。
- 將每個選項按鈕的 [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) 屬性設定為相同的值。

在這個範例中，第一個選項按鈕群組位於相同的堆疊面板中，藉此以隱含方式群組化。 第二個群組會分割成兩個堆疊面板，因此會使用 `GroupName` 來將其明確地分組到單一群組中。

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 1 - implicit grouping -->
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="white" Checked="BGRadioButton_Checked"
                         IsChecked="True"/>
        </StackPanel>
    </StackPanel>

    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 2 - grouped by GroupName -->
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" Tag="green" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" Tag="yellow" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" Tag="blue" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" Tag="white"  GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="ExampleBorder"
            BorderBrush="#FFFFD700" Background="#FFFFFFFF"
            BorderThickness="10" Height="50" Margin="0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "white":
                ExampleBorder.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "green":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "blue":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "white":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

這兩個 `RadioButton` 控制項群組看起來像這樣：

:::image type="content" source="images/radio-button-groups.png" alt-text="兩個群組中的選項按鈕":::

### <a name="radio-button-states"></a>選項按鈕狀態

選項按鈕有兩個狀態：已選取或已清除。 已選取選項按鈕時，其 [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) 屬性為 `true`。 已選取選項按鈕時，其 `IsChecked` 屬性為 `false`。 如果使用者選取同一個群組中的另一個選項按鈕，則可清除選項按鈕，但如果使用者再次選取，則無法將其清除。 不過，您可以將選項按鈕的 `IsChecked` 屬性設定為 `false`，以程式設計方式清除該選項按鈕。

### <a name="visuals-to-consider"></a>要考慮的視覺效果

個別 `RadioButton` 控制項的預設間距與 `RadioButtons` 群組所提供的間距不同。 若要將 `RadioButtons` 間距套用到個別 `RadioButton` 控制項，請使用 `0,0,7,3` 的 `Margin` 值，如下所示。

```xaml
<StackPanel>
    <StackPanel.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="Margin" Value="0,0,7,3"/>
        </Style>
    </StackPanel.Resources>
    <TextBlock Text="Background"/>
    <RadioButton Content="Item 1"/>
    <RadioButton Content="Item 2"/>
    <RadioButton Content="Item 3"/>
</StackPanel>
```

下列影像顯示群組中的選項按鈕慣用間距。

:::image type="content" source="images/radiobutton-layout.png" alt-text="顯示一組選項按鈕的影像，這些選項按鈕垂直排列":::

:::image type="content" source="images/radiobutton-redline.png" alt-text="顯示選項按鈕間距指導方針的影像":::

> [!NOTE]
> 如果您使用 WinUI RadioButtons 控制項，則間距、邊界和方向均已最佳化。

## <a name="recommendations"></a>建議

- 確定一組選項按鈕的目的和目前狀態明確。
- 將選項按鈕的文字標籤限制為單行。
- 如果文字標籤是動態的，請考慮按鈕將如何自動調整大小以及調整後周圍的任何視覺效果會發生什麼變化。
- 除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
- 不要將兩個 RadioButtons 群組並排。 當兩個 RadioButtons 群組並列時，使用者很難判斷哪個按鈕屬於哪個群組。

## <a name="get-the-sample-code"></a>取得範例程式碼

- 若要以互動式格式取得所有 XAML 控制項，請參閱 [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery)。

## <a name="related-topics"></a>相關主題

- [按鈕](buttons.md)
- [切換開關](toggles.md)
- [核取方塊](checkbox.md)
- [清單和下拉式方塊](lists.md)
- [滑桿](slider.md)
- [RadioButtons 類別](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
- [RadioButton 類別](/uwp/api/windows.ui.xaml.controls.radiobutton) (英文)
