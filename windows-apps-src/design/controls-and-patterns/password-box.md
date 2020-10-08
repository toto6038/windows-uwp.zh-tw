---
Description: 密碼方塊是一個文字輸入方塊，會基於隱私權考量而隱藏輸入其中的字元。
title: 密碼方塊的指導方針
ms.assetid: 332B04D6-4FFE-42A4-8B3D-ABE8266C7C18
dev.assetid: 4BFDECC6-9BC5-4FF5-8C63-BB36F6DDF2EF
label: Password box
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3ba2df600e294aab959a090d8b5a5f8645a40d55
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749724"
---
# <a name="password-box"></a>密碼方塊

密碼方塊是一個文字輸入方塊，會基於隱私權考量而隱藏輸入其中的字元。 密碼方塊看起來就像文字方塊，只不過它會將已輸入的文字轉譯成預留位置字元。 您可以設定預留位置字元。

根據預設，密碼方塊會提供一種方式，讓使用者能夠藉由按住顯示按鈕來檢視他們的密碼。 您可以停用顯示按鈕，或提供替代機制來顯示密碼，例如核取方塊。

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](../style/rounded-corner.md)。 WinUI 是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **平台 API**：[PasswordBox 類別](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)、[Password 屬性](/uwp/api/windows.ui.xaml.controls.passwordbox.password)、[PasswordChar 屬性](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchar)、[PasswordRevealMode 屬性](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode)、[PasswordChanged 事件](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchanged)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用 **PasswordBox** 控制項，以收集密碼或其他私人資料，例如身分證字號。

如需如何選擇正確文字控制項的詳細資訊，請參閱[文字控制項](text-controls.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/PasswordBox">開啟應用程式並查看 PasswordBox 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

密碼方塊有數個狀態，包括這些值得注意的狀態。

靜止的密碼方塊可以顯示提示文字，讓使用者知道其用途：

![處於靜止狀態的密碼方塊與提示文字](images/passwordbox-rest-hinttext.png)

當使用者在密碼方塊中輸入時，預設行為是顯示將輸入文字隱藏的項目符號：

![密碼方塊焦點狀態輸入文字](images/passwordbox-focus-typing.png)

按右邊的 [顯示] 按鈕可窺看正輸入的密碼文字：

![密碼方塊文字已顯示](images/passwordbox-text-reveal.png)

## <a name="create-a-password-box"></a>建立密碼方塊

使用 [Password](/uwp/api/windows.ui.xaml.controls.passwordbox.password) 屬性來取得或設定 PasswordBox 的內容。 您可以在處理常式中針對 [PasswordChanged](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchanged) 事件執行此動作，在使用者輸入密碼時執行驗證。 您可以使用另一個事件 (例如按鈕 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click))，在使用者完成文字輸入之後執行驗證。

以下是適用於密碼方塊控制項的 XAML，該控制項展示 PasswordBox 的預設外觀。 當使用者輸入密碼時，您會檢查該控制項以確認是否為「Password」常值。 如果是，您會向使用者顯示訊息。

```xaml
<StackPanel>  
  <PasswordBox x:Name="passwordBox" Width="200" MaxLength="16"
             PasswordChanged="passwordBox_PasswordChanged"/>

  <TextBlock x:Name="statusText" Margin="10" HorizontalAlignment="Center" />
</StackPanel>   
```

```csharp
private void passwordBox_PasswordChanged(object sender, RoutedEventArgs e)
{
    if (passwordBox.Password == "Password")
    {
        statusText.Text = "'Password' is not allowed as a password.";
    }
    else
    {
        statusText.Text = string.Empty;
    }
}
```
以下是執行此程式碼且使用者輸入「Password」時的結果。

![含驗證訊息的密碼方塊](images/passwordbox-revealed-validation.png)

### <a name="password-character"></a>密碼字元

您可以設定 [PasswordChar](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordchar) 屬性，來變更用來設定密碼遮罩的字元。 這裡會使用星號來取代預設的項目符號。

```xaml
<PasswordBox x:Name="passwordBox" Width="200" PasswordChar="*"/>
```

結果看起來就像這樣。

![含自訂字元的密碼方塊](images/passwordbox-custom-char.png)

### <a name="headers-and-placeholder-text"></a>標頭與預留位置文字

您可以使用 [Header](/uwp/api/windows.ui.xaml.controls.passwordbox.header) 和 [PlaceholderText](/uwp/api/windows.ui.xaml.controls.passwordbox.placeholdertext) 屬性來提供 PasswordBox 的內容。 當您有多個方塊時 (例如，在表單上變更密碼)，這特別有用。

```xaml
<PasswordBox x:Name="passwordBox" Width="200" Header="Password" PlaceholderText="Enter your password"/>
```

![處於靜止狀態的密碼方塊與提示文字](images/passwordbox-rest-hinttext.png)

### <a name="maximum-length"></a>長度上限

藉由設定 [MaxLength](/uwp/api/windows.ui.xaml.controls.passwordbox.maxlength) 屬性，來指定使用者可以輸入的字元數目上限。 沒有任何屬性可用來指定最小長度，但您可以在 app 程式碼中檢查密碼長度並執行任何其他驗證。

## <a name="password-reveal-mode"></a>密碼顯示模式

PasswordBox 有一個內建按鈕，使用者按下該按鈕就會顯示密碼文字。 以下是使用者動作的結果。 當使用者放開按鈕後，密碼就會自動再次隱藏。

![密碼方塊文字已顯示](images/passwordbox-text-reveal.png)

### <a name="peek-mode"></a>預覽模式

預設會顯示密碼顯示按鈕 (或「預覽」按鈕)。 使用者必須持續按住該按鈕才能檢視密碼，如此便能維持高等級的安全性。

[PasswordRevealMode](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode) 屬性的值並非決定是否要讓使用者看見密碼顯示按鈕的唯一因素。 其他因素包括控制項是否會以大於最小寬度的形式來顯示、PasswordBox 是否具有焦點，以及文字輸入欄位是否包含至少一個字元。 密碼顯示按鈕只有在 PasswordBox 第一次收到焦點且輸入字元時才會顯示。 如果 PasswordBox 失去焦點，然後重新取得焦點，除非將密碼清除並重新開始輸入字元，否則顯示按鈕將不會再次顯示。

> **注意**&nbsp;&nbsp;在 Windows 10 之前，預設不會顯示密碼顯示按鈕。 如果您 app 的安全性需要一律隱藏密碼，請務必將 PasswordRevealMode 設為 Hidden。

### <a name="hidden-and-visible-modes"></a>隱藏和顯示模式

其他的 [PasswordRevealMode](/uwp/api/Windows.UI.Xaml.Controls.PasswordRevealMode) 列舉值 (**Hidden** 和 **Visible**) 會隱藏密碼顯示按鈕，並讓您能夠以程式設計的方式來管理是否要隱藏密碼。

若要一律隱藏密碼，請將 PasswordRevealMode 設定為 Hidden。 除非您需要一律隱藏密碼，否則，您可以提供自訂的 UI，讓使用者能夠在 Hidden 和 Visible 之間切換 PasswordRevealMode。 例如，您可以使用核取方塊來切換是否遮蔽密碼，如下列範例所示。 您也可以使用其他控制項 (例如 [ToggleButton](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton))，讓使用者能夠切換模式。

這個範例示範如何使用 [CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)，讓使用者能夠切換 PasswordBox 的顯示模式。

```xaml
<StackPanel Width="200">
    <PasswordBox Name="passwordBox1"
                 PasswordRevealMode="Hidden"/>
    <CheckBox Name="revealModeCheckBox" Content="Show password"
              IsChecked="False"
              Checked="CheckBox_Changed" Unchecked="CheckBox_Changed"/>
</StackPanel>
```

```csharp
private void CheckBox_Changed(object sender, RoutedEventArgs e)
{
    if (revealModeCheckBox.IsChecked == true)
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Visible;
    }
    else
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Hidden;
    }
}
```

這個 PasswordBox 看起來就像這樣。

![含自訂顯示按鈕的密碼方塊](images/passwordbox-custom-reveal.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>選擇文字控制項的正確鍵盤

為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。 PasswordBox 只支援 **Password** 和 **NumericPin** 輸入範圍值。 會略過其他任何值。

如需如何使用輸入範圍的詳細資訊，請參閱[使用輸入範圍來變更觸控式鍵盤](../input/use-input-scope-to-change-the-touch-keyboard.md)。

## <a name="recommendations"></a>建議

-   如果密碼方塊的用途不清楚，請使用標籤或預留位置文字。 無論文字輸入方塊是否包含值，都會顯示標籤。 預留位置文字會顯示在文字輸入方塊內，只要輸入值就會消失。
-   為密碼方塊指定一個適合可輸入的值範圍的寬度。 每個語言的單字長度都不相同，所以如果您希望您的應用程式能夠全球化，請考慮到當地語系化。
-   不要在密碼輸入方塊旁邊放置另一個控制項。 密碼方塊有一個顯示密碼按鈕，可以讓使用者驗證所輸入的密碼，而在旁邊放置一個控制項，使用者可能會在嘗試與另一個控制項互動時，不小心顯示密碼。 為了避免這種情況發生，請在密碼輸入方塊與另一個控制項之間保留一些間距，或是將另一個控制項放到下一行。
-   考慮呈現兩個建立帳戶的密碼方塊：一個用於新密碼，第二個用來確認新密碼。
-   只顯示一個用於登入的單一密碼方塊。
-   當密碼方塊是用來輸入 PIN 時，請考慮在輸入最後一個數字時便提供立即回應，而不是使用確認按鈕。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

[文字控制項](text-controls.md)

- [拼字檢查指導方針](text-controls.md)
- [新增搜尋](/previous-versions/windows/apps/hh465231(v=win.10))
- [文字輸入的指導方針](text-controls.md)
- [TextBox 類別](/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Windows.UI.Xaml.Controls PasswordBox 類別](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length 屬性](/dotnet/api/system.string.length)
