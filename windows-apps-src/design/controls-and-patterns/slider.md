---
author: QuinnRadich
Description: Lets the user set a value in a given range.
title: 滑桿
ms.assetid: 7EC7EA33-BE7E-4FD5-B205-B8FA7B729ACC
label: Sliders
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a284625bdfa76fd1fe41948f01e64e1bea3d2644
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2018
ms.locfileid: "4128519"
---
# <a name="sliders"></a>滑桿

 

滑桿是一個控制項，透過讓使用者沿著軌跡移動 Thumb 控制項，從一定範圍內選取值。

> **重要 API**：[Slider 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx)、[Value 屬性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx)、[ValueChanged 事件](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx)

![滑桿控制項](images/controls/slider.png)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

如果您希望使用者能夠設定既定且連續的值 (例如音量或亮度)，或是某個範圍的離散值 (例如螢幕解析度設定)，可以使用滑桿。

當您知道使用者將值視為相對數量而不是數字值時，滑桿是很好的選擇。 例如，使用者想要將音訊音量設定為低或中，而不是設定值為 2 或 5。

請不要為二元設定使用滑桿。 請改用[切換開關](toggles.md)。

以下是決定是否使用滑桿時的一些其他考量因素：

-   **設定看起來是否像相對數量？** 如果不是，請使用[選項按鈕](radio-button.md)或[清單方塊](lists.md)。
-   **該設定是否為已知的確切數值？** 如果是，請使用數字[文字方塊](text-box.md)。
-   **在變更設定時，獲得即時回應的效果是否為使用者帶來益處？** 如果是，請使用滑桿。 例如，藉由立即看到色調、飽和或光度值變更後的效果，能讓使用者更易於選擇色彩。
-   **設定的範圍是否包含四個或更多值？** 如果不是，請使用[選項按鈕](radio-button.md)。
-   **使用者是否能變更該值？** 滑桿的用意是提供使用者互動。 如果使用者無法變更值，請改用唯讀文字。

如果您正在決定使用滑桿或數值文字方塊，則在下列情況下請使用數值文字方塊：

-   螢幕空間很小。
-   使用者似乎慣於使用鍵盤。

如果是下列情況，請使用滑桿：

-   使用者可受益於立即回應。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/Slider">開啟應用程式並查看 Slider 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Windows Phone 上用來控制音量的滑桿。

![Windows Phone 上用來控制音量的滑桿](images/control-examples/slider-phone.png)

用來變更 Windows 顯示設定中文字大小的滑桿。

![用來變更 Windows 顯示設定中文字大小的滑桿](images/control-examples/slider-display-settings.png)

## <a name="create-a-slider"></a>建立滑桿

以下是如何在 XAML 中建立滑桿。

```xaml
<Slider x:Name="volumeSlider" Header="Volume" Width="200"
        ValueChanged="Slider_ValueChanged"/>
```

以下是如何在程式碼中建立滑桿。

```csharp
Slider volumeSlider = new Slider();
volumeSlider.Header = "Volume";
volumeSlider.Width = 200;
volumeSlider.ValueChanged += Slider_ValueChanged;

// Add the slider to a parent container in the visual tree.
stackPanel1.Children.Add(volumeSlider);
```

您要從 [Value](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.value.aspx) 屬性取得並設定滑桿的值。 若要回應值變更，您可以使用資料繫結來繫結 Value 屬性，或處理 [ValueChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.rangebase.valuechanged.aspx) 事件。

```csharp
private void Slider_ValueChanged(object sender, RangeBaseValueChangedEventArgs e)
{
    Slider slider = sender as Slider;
    if (slider != null)
    {
        media.Volume = slider.Value;
    }
}
```

## <a name="recommendations"></a>建議

-   調整控制項的大小，方便使用者設定想要的值。 設定分散值時，確定使用者可輕鬆地使用滑鼠選取任何值。 確定滑桿的端點永遠在檢視邊界內。
-   在使用者選取時或選取後提供立即回饋 (可行的話)。 例如，Windows 音量控制項發出嗶聲，指示所選取的音訊音量。
-   使用標籤顯示值的範圍。 例外：如果滑桿是垂直方向，而頂端標籤為最大、高、多或對等值，則您可以省略其他標籤，因為意思已經很清楚了。
-   停用滑桿時，也會停用所有相關聯的標籤或視覺回饋。
-   設定滑桿的滑動方向和 (或) 方向時，請考慮文字的閱讀方向。 有些語言中的指令碼是從左至右閱讀，有些語言則是右至左。
-   不要使用滑桿做為進度指示器。
-   不要變更預設的滑桿縮圖大小。
-   如果值的範圍很大而且使用者很可能從範圍中的數個具有代表性的值選擇時，請不要建立連續滑桿。 而是將這些值做為允許選擇的間距。 例如，如果時間值可能長達 1 個月，但使用者只需要從 1 分鐘、1 小時、1 天或 1 個月中挑選，那麼請建立只有 4 個間距點的滑桿。

## <a name="additional-usage-guidance"></a>其他用法指導方針

### <a name="choosing-the-right-layout-horizontal-or-vertical"></a>選擇正確的配置：水平或垂直

您可以將滑桿的方向設定為水平或垂直。 使用這些指導方針來決定要使用哪種配置。

-   使用原有的方向。 例如，如果滑桿表示真實世界中通常會垂直顯示的值 (例如溫度)，請使用垂直方向。
-   如果控制項是用來在媒體 (如視訊應用程式) 進行搜尋，請使用水平方向。
-   在可以朝某個方向 (水平或垂直) 平移的頁面中使用滑桿時，滑桿的方向應該與平移方向不同。 否則，使用者可能在嘗試移動瀏覽頁面時撥動到滑桿而不小心變更到值。
-   如果您仍然不確定要使用哪個方向，請使用最適合您頁面配置的方向。

### <a name="range-direction"></a>範圍方向

範圍方向是指您從目前值將滑桿滑動到最大值時，移動滑桿的方向。

-   若是垂直滑桿，無論讀取方向為何，請在滑桿頂端放入最大值。 例如音量滑桿，永遠將最大音量設定放在滑桿的頂端。 至於其他類型的值 (例如每週的天數)，請按照頁面的閱讀方向。
-   若是水平樣式，在從左到右的頁面配置中，請將較低值放在滑桿的左側，在從右到左的頁面配置中則放在滑桿右側。
-   上述指導方針的一個例外是媒體搜尋列：永遠將較低的值放置在滑桿的左邊。

### <a name="steps-and-tick-marks"></a>間距和刻度標記

-   如果您不希望在滑桿的最小值和最大值之間允許任意值，請使用間距點。例如，如果您使用滑桿來指定要購買的電影票張數，請不要允許浮點數值。 請將間距值指定為 1。
-   如果您指定間距 (也稱作貼齊點)，請確認最後一個間距對齊滑桿的最大值。
-   當您想要對使用者顯示主要或重要值的位置時，請使用刻度標記。 例如，控制縮放的滑桿可以包含 50%、100% 及 200% 的刻度標記。
-   當使用者需要知道設定的近似值時，請顯示刻度標記。
-   當使用者需要知道所選擇設定的確實值，不需與控制項互動時，請顯示刻度和值標籤。 或者，使用者可以使用值工具提示查看確實值。
-   當間距點不明顯時，請一律使用刻度標記。 例如，如果滑桿的寬度為 200 像素且具有 200 個貼齊點，您可以隱藏刻度標記，因為使用者不會注意到貼齊行為。 但是如果僅有 10 個貼齊點時，請顯示刻度標記。

### <a name="labels"></a>標籤

-   **滑桿標籤**

    滑桿標籤指示滑桿的用途。

    -   使用標籤，但不包含結尾標點符號 (這是所有控制項標籤慣例)。
    -   如果滑桿將其大部分的標籤放在控制項上方，則將標籤放置在滑桿上方。
    -   如果滑桿將大部分標籤放在控制項的旁邊，則將標籤放置在滑桿兩側。
    -   避免將標籤放置在滑桿下方，因為使用者在觸控滑桿時，手指有可能會誤觸標籤。
-   **範圍標籤**

    範圍或填滿標籤描述滑桿的最小值及最大值。

    -   在滑桿範圍的兩端建立標籤，如果是垂直方向，則不需要建立標籤。
    -   如果可能的話，每個標籤僅使用一個字。
    -   請勿使用結束標點符號。
    -   確定這些標籤為描述性文字，而且平行放置。 範例：最大/最小、多/少、低/高、小聲/大聲。
-   **值標籤**

    值標籤顯示滑桿目前的值。

    -   如果您需要值標籤，請將它顯示在滑桿下方。
    -   把文字放在與控制項相對的位置中間並包括單位 (例如像素)。
    -   因為擦選時會蓋住滑桿的捲動方塊，所以請考慮以其他方式 (使用標籤或其他視覺物件) 來顯示目前的值。 滑桿設定文字大小可以在滑桿旁呈現一些正確大小的範例文字。

### <a name="appearance-and-interaction"></a>外觀和互動

滑桿是由軌道和捲動方塊組成。 軌道是一個列 (可選擇性顯示各種樣式的刻度記號)，代表可以輸入的值範圍。 捲動方塊則是選取器，使用者點選軌道或在上面來回擦選即可定位。

滑桿具有大型觸控目標。 為了維持觸控存取性，滑桿應該放置在離顯示邊緣夠遠的距離。

設計自訂滑桿的時候，請考慮以盡可能不擁擠的方式為使用者呈現所有的資訊。 如果使用者需要知道單位才能了解設定，請使用值標籤；找出充滿創意的方式用圖形呈現這些值。 例如，控制音量的滑桿可以在滑桿的音量最低處顯示沒有音波的喇叭圖形，而在音量最高處顯示有音波的喇叭圖形。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。

## <a name="related-topics"></a>相關主題
- [切換開關](toggles.md)
- [Slider 類別](https://msdn.microsoft.com/library/windows/apps/br209614)
