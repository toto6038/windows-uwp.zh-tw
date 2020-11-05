---
description: Numberbox 是可用來顯示和編輯數字的控制項。
title: 數字方塊
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4bba2f90d7240b454c4dbd5331251e306f946af7
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030911"
---
# <a name="number-box"></a>數字方塊

代表可用來顯示和編輯數字的控制項。 這可支援基本方程式的驗證、遞增逐步執行及計算內嵌計算，例如乘法、除法、加法和減法。

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **NumberBox** 控制項需要 Windows UI 程式庫，該程式庫是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫概觀](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

**Windows UI 程式庫 API：** [NumberBox 類別](/uwp/api/microsoft.ui.xaml.controls.NumberBox)

> [!TIP]
> 在這整份文件中，我們使用 XAML 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已將此新增至我們的 [Page](/uwp/api/windows.ui.xaml.controls.page) 元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在後方的程式碼中，我們也使用 C# 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已在檔案頂端新增了此 **using** 陳述式：`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

您可使用 NumberBox 控制項來擷取和顯示數學輸入。 如果您需要可接受數字以外內容的可編輯文字方塊，請使用 [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 控制項。 如果您需要可接受密碼或其他敏感性輸入的可編輯文字方塊，請參閱 [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)。 如果您需要文字方塊來輸入搜尋字詞，請參閱 [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)。 如果您需要輸入或編輯格式化文字，請參閱 [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡來<a href="xamlcontrolsgallery:/item/TextBox">開啟應用程式並查看 NumberBox 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-a-simple-numberbox"></a>建立簡單的 NumberBox

以下的 XAML 適用於示範預設外觀的基本 NumberBox。 使用 [x:Bind](../../xaml-platform/x-bind-markup-extension.md#property-path)，以確保向使用者顯示的資料會與您應用程式中儲存的資料保持同步。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![顯示 0 的聚焦輸入欄位。](images/numberbox-basic.png)

### <a name="labeling-numberbox"></a>標記 NumberBox

如果 NumberBox 的目的不清楚，請使用 `Header` 或 `PlaceholderText`。 無論 NumberBox 是否包含值，都會顯示 `Header`。

```xaml
<muxc:NumberBox Header="Enter a number:"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![NumberBox 上方顯示為「輸入運算式:」的標頭。](images/numberbox-header.png)

`PlaceholderText` 會顯示在 NumberBox 內，而只有在 `Value` 設定為 NaN 或使用者清除輸入時才會顯示。

```xaml
<muxc:NumberBox PlaceholderText="1+2^2"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![NumberBox，其中包含顯示為「A + B」的預留位置文字。](images/numberbox-placeholder-text.png)

### <a name="enable-calculation-support"></a>啟用計算支援

將 `AcceptsExpression` 屬性設定為 true，可讓 NumberBox 使用作業標準順序來評估基本內嵌運算式，例如乘法、除法、加法和減法。 當失去焦點或使用者按下 Enter 鍵時，就會觸發評估。 評估運算式之後，就不會保留運算式的原始形式。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    AcceptsExpression="True" />
```

### <a name="increment-and-decrement-stepping"></a>遞增和遞減逐步執行

使用 `SmallChange` 屬性，設定當 NumberBox 為焦點且使用者執行下列動作時，NumberBox 內的值會變更多少：

- 捲動
- 按向上鍵
- 按向下鍵

使用 `LargeChange` 屬性，設定當 NumberBox 為焦點且使用者按下 PageUp 或 PageDown 鍵時，NumberBox 內的值會變更多少。

使用 `SpinButtonPlacementMode` 屬性來啟用按鈕，按一下這些按鈕可依據 `SmallChange` 屬性所指定的數量來遞增或遞減 NumberBox 中的值。 如果最大或最小值會被另一個步驟超過，就會停用這些按鈕。

將 `SpinButtonPlacementMode` 設定為 `Inline`，讓按鈕顯示在控制項旁邊。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Inline" />
```

![旁邊有向下箭號按鈕和向上箭號按鈕的 NumberBox。](images/numberbox-spinbutton-inline.png)

將 `SpinButtonPlacementMode` 設定為 `Compact`，讓按鈕只有在 NumberBox 為焦點時才顯示為飛出視窗。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Compact" />
```

![其中有小圖示顯示向上箭號和向下箭號的 NumberBox。](images/numberbox-spinbutton-compact-non-visible.png)

![有向下箭號按鈕和向上箭號按鈕在更高圖層側面浮動的 NumberBox。](images/numberbox-spinbutton-compact-visible.png)

### <a name="enabling-input-validation"></a>啟用輸入驗證

將 `ValidationMode` 設定為 `InvalidInputOverwritten`，可讓 NumberBox 在失去焦點或按下 Enter 鍵觸發評估時，以最後一個有效的值覆寫不是數字也不合乎公式的無效輸入。

```xaml
<muxc:NumberBox Header="Quantity"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    ValidationMode="InvalidInputOverwritten" />
```

將 `ValidationMode` 設定為 `Disabled`，允許設定自訂輸入驗證。

至於小數點和逗號，使用者所使用的格式將由針對 NumberBox 設定的格式所取代。 不會觸發輸入驗證錯誤。

### <a name="formatting-input"></a>格式化輸入

[數位格式](/uwp/api/windows.globalization.numberformatting)可用於格式化 Numberbox 的值，其做法是設定格式化類別的執行個體，並將其指派給 `NumberFormatter` 屬性。 小數、貨幣、百分比和重要數字是一些可用的數字格式化類別。 請注意，捨入也是由數字格式化屬性所定義。

以下範例示範如何使用 DecimalFormatter 來格式化 NumberBox 的值，使其具有一個整數數字、兩個分數數字，以及進位到最接近 0.25：

```xaml
<muxc:NumberBox  x:Name="FormattedNumberBox"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

```csharp
private void SetNumberBoxNumberFormatter()
{
    IncrementNumberRounder rounder = new IncrementNumberRounder();
    rounder.Increment = 0.25;
    rounder.RoundingAlgorithm = RoundingAlgorithm.RoundUp;

    DecimalFormatter formatter = new DecimalFormatter();
    formatter.IntegerDigits = 1;
    formatter.FractionDigits = 2;
    formatter.NumberRounder = rounder;
    FormattedNumberBox.NumberFormatter = formatter;
}
```

![顯示值為 0.00 的 NumberBox。](images/numberbox-formatted.png)

至於小數點和逗號，使用者所使用的格式將由針對 NumberBox 設定的格式所取代。 不會觸發輸入驗證錯誤。

## <a name="remarks"></a>備註

### <a name="input-scope"></a>輸入範圍

`Number` 將用於[輸入範圍](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)。 此輸入範圍適合使用數字 0-9。 這可能會遭到覆寫，但不會明確支援替代的 InputScope 類型。

### <a name="not-a-number"></a>不是數位

當 NumberBox 的輸入清除時，`Value` 會設定為 `NaN`，表示沒有數值存在。

### <a name="expression-evaluation"></a>運算式評估

NumberBox 會使用中置標記法來評估運算式。 依照優先順序，可允許的運算子如下：

* ^
* */
* +-

請注意，括弧可用來覆寫優先順序規則。

## <a name="recommendations"></a>建議

* `Text` 和 `Value` 可讓您輕鬆地將 NumberBox 的值當作 String 或 Double 擷取，而不需要轉換不同類型的值。 以程式設計方式改變 NumberBox 的值時，建議透過 `Value` 屬性來這麼做。 `Value` 會在初始設定時覆寫 `Text`。 初始設定之後，對其中一項所做的變更會擴散到另一項，但是一貫地透過 `Value` 進行程式設計變更，有助於避免 NumberBox 會透過 `Text` 接受非數值字元的概念誤解。
* 使用 `Header` 或 `PlaceholderText` 來通知使用者 NumberBox 只接受以數值字元作為輸入。 以拼寫表示的數字 (例如 "one") 將不會解析為接受的值。
