---
Description: 為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 使用輸入範圍來變更觸控式鍵盤
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: 鍵盤、協助工具、瀏覽、焦點、文字、輸入、使用者互動
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: e6e140a1967ca3ffe7775f427ccae7a7e07c5ca6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165812"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>使用輸入範圍來變更觸控式鍵盤

為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。

### <a name="important-apis"></a>重要 API
- [InputScope](/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


當您的應用程式在具備觸控式螢幕的裝置上執行時，可以使用觸控式鍵盤輸入文字。 當使用者按下可編輯的輸入欄位（例如 **[TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** 或 **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)**）時，就會叫用觸控鍵盤。 您可以將文字控制項的 *輸入範圍* 設定為符合您希望使用者輸入的資料類型，讓使用者更快速且更輕鬆地在應用程式中輸入資料。 輸入範圍會提供控制項所預期之文字輸入類型的提示給系統，讓系統可以為該輸入類型提供專用的觸控式鍵盤配置。

例如，如果文字方塊僅用來輸入4位數的 PIN，請將 [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 屬性設定為 **Number**。 這會告訴系統顯示數字小鍵盤，方便使用者輸入 PIN。

> [!IMPORTANT]
> - 此資訊只適用於 SIP。 它並不適用於硬體鍵盤或 Windows [輕鬆存取] 選項中提供的 [螢幕小鍵盤]。
> - 輸入範圍並不會導致執行任何輸入驗證，也不會防止使用者透過硬體鍵盤或其他輸入裝置提供任何輸入。 您仍然必須視需要在程式碼中驗證輸入。

## <a name="changing-the-input-scope-of-a-text-control"></a>變更文字控制項的輸入範圍

您的應用程式中可用的輸入範圍是 **[InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)** 列舉的成員。 您可以將 ****TextBox**** 或 **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** 的 [InputScope](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 屬性設定成這些值的其中之一。

> [!IMPORTANT]
> **[PasswordBox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** 上的**[InputScope](/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** 屬性只支援**Password**和**NumericPin**值。 會略過其他任何值。

您會在這裡變更數個文字方塊的輸入範圍，使其符合每個文字方塊的預期資料。

**在 XAML 中變更輸入範圍**

1.  在您頁面的 XAML 檔案中，找出您想要變更之文字控制項的標記。
2.  將 [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 屬性新增到標記中，並指定符合預期輸入的 [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 值。

    以下是一些可能會出現在常見客戶連絡人表單上的文字方塊。 設定 [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 之後，就會為每個文字方塊顯示具有適當資料配置的觸控式鍵盤。

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**在程式碼中變更輸入範圍**

1.  在您頁面的 XAML 檔案中，找出您想要變更之文字控制項的標記。 如果未設定，請設定 [x:Name 屬性](../../xaml-platform/x-name-attribute.md)，以便在您的程式碼中參考控制項。

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  具現化新的 [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) 物件。

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  具現化新的 [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 物件。
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  將 [**InputScopeName**](/uwp/api/windows.ui.xaml.input.inputscopename.namevalue) 物件的 [**NameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 屬性設定為 [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 列舉的值。

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  將 [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 物件新增到 [**InputScope**](/uwp/api/windows.ui.xaml.input.inputscope.names) 物件的 [**Names**](/uwp/api/Windows.UI.Xaml.Input.InputScope) 集合。

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  將 [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) 物件設定為文字控制項的 [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 屬性值。

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

以下是所有的程式碼。

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

相同的步驟可以濃縮成這個速記的程式碼。

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>文字預測、拼字檢查及自動校正

[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 和 [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 控制項有數個影響 SIP 行為的屬性。 若要為使用者提供最佳的體驗，了解這些屬性如何影響使用觸控進行的文字輸入，就相當重要。

-   [**IsSpellCheckEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled) ─ 當為文字控制項啟用拼字檢查時，該控制項會與系統的拼字檢查引擎互動，將無法辨識的單字標示出來。 您可以點選單字以查看建議的校正清單。 預設會啟用拼字檢查功能。

    對於 **Default** 輸入範圍，這個屬性也可以自動大寫句子中的第一個字，並自動校正您輸入的文字。 這些自動校正功能在其他輸入範圍可能會停用。 如需詳細資訊，請參閱本主題稍後的表格。

-   [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) — 文字控制項啟用文字預測時，系統會顯示您可能正開始輸入的文字清單。 您可以從清單中選取單字，這樣您就不必輸入整個單字。 文字預測預設為啟用。

    如果輸入範圍不是 **Default**，則即使 [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) 屬性為 **true**，也可能停用文字預測功能。能。 如需詳細資訊，請參閱本主題稍後的表格。

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus) — 此屬性是 **true** 時，當焦點透過程式設計設定在文字控制項上時，系統將不會顯示 SIP。 取而代之的是，只有當使用者與控制項互動時，才會顯示鍵盤。

## <a name="touch-keyboard-index-for-windows"></a>適用于 Windows 的觸控鍵盤索引

這些表格會顯示 Windows 軟輸入面板， (一般輸入範圍值的 SIP) 版面配置。 針對每個輸入範圍，會列出對 **IsSpellCheckEnabled** 和 **IsTextPredictionEnabled** 屬性所啟用之功能的輸入範圍效果。 這並非可用之輸入範圍的完整清單。

> [!Tip] 
> 您可以藉由按下 **&123** 鍵來變更為數位和符號配置，然後按 [ **abcd** ] 鍵變更為字母配置，來切換字母佈局與數位和符號配置之間的大部分觸控鍵盤。

### <a name="default"></a>預設

`<TextBox InputScope="Default"/>`

預設的 Windows 觸控鍵盤。

![預設 Windows 觸控式鍵盤](images/input-scopes/default.png)
- 拼寫檢查：如果**IsSpellCheckEnabled**  =  **為 true**，則會啟用，如果**IsSpellCheckEnabled**  =  **為 false** ，則停用
- 自動校正：如果**IsSpellCheckEnabled**  =  **為 true**，則啟用，如果**IsSpellCheckEnabled**  =  **false** ，則停用
- 自動大寫：如果**IsSpellCheckEnabled**  =  **為 true**，則啟用，如果**IsSpellCheckEnabled**  =  **false** ，則停用
- 文字預測：如果**IsTextPredictionEnabled**  =  **為 true**，則啟用，如果**IsTextPredictionEnabled**  =  **false** ，則停用

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

預設的數字和符號鍵盤配置。

![Windows 貨幣觸控式鍵盤](images/input-scopes/currencyamountandsymbol.png)

- 包含 page left/right 鍵來顯示更多符號
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：一律停用
- 文字預測：預設為開啟，可以停用
 
### <a name="url"></a>Url

`<TextBox InputScope="Url"/>`

![Windows URL 觸控式鍵盤](images/input-scopes/url.png)

- 包含 **.com** 和![前往鍵](images/input-scopes/kbdgokey.png) (前往) 鍵。 按住 **.com** 鍵以顯示其他選項 (**. org**、 **.net**和區域特定的尾碼) 
- 包含 **：**、 **-** 和索引 **/** 鍵
- 拼字檢查：預設為關閉，可以啟用
- 自動校正：預設為關閉，可以啟用
- 自動大寫：預設為關閉，可以啟用
- 文字預測：預設為關閉，可以啟用


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![Windows 電子郵件地址觸控式鍵盤](images/input-scopes/emailsmtpaddress.png)
- 包含 **@** 和 **.com** 索引鍵。 按住 **.com** 鍵以顯示其他選項 (**. org**、 **.net**和區域特定的尾碼) 
- 包含 **_** 和索引 **-** 鍵
- 拼字檢查：預設為關閉，可以啟用
- 自動校正：預設為關閉，可以啟用
- 自動大寫：預設為關閉，可以啟用
- 文字預測：預設為關閉，可以啟用


### <a name="number"></a>Number

`<TextBox InputScope="Number"/>`

![Windows 數字觸控式鍵盤](images/input-scopes/number.png)
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：一律停用
- 文字預測：預設為開啟，可以停用

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![Windows 電話號碼觸控式鍵盤](images/input-scopes/telephonenumber.png)
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：一律停用
- 文字預測：預設為開啟，可以停用

### <a name="search"></a>搜尋

`<TextBox InputScope="Search"/>`

![Windows 搜尋觸控式鍵盤](images/input-scopes/search.png)
- 包含 **搜尋索引** 鍵，而不是 **Enter** 鍵
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：一律停用
- 文字預測：預設為開啟，可以停用

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![適用于漸進式搜尋的 Windows 觸控鍵盤](images/input-scopes/searchincremental.png)
- 與**預設值**相同的版面配置
- 拼字檢查：預設為關閉，可以啟用
- 自動校正：一律停用
- 自動大寫：一律停用
- 文字預測：一律停用

### <a name="formula"></a>Formula

`<TextBox InputScope="Formula"/>`

![適用于公式的 Windows 觸控鍵盤](images/input-scopes/formula.png)
- 包含 **=** 金鑰
- 也包含 **%** 、 **$** 和索引 **+** 鍵
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：一律停用
- 文字預測：預設為開啟，可以停用

### <a name="chat"></a>聊天

`<TextBox InputScope="Chat"/>`

![預設 Windows 觸控式鍵盤](images/input-scopes/default.png)
- 與**預設值**相同的版面配置
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：預設為開啟，可以停用
- 文字預測：預設為開啟，可以停用

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![預設 Windows 觸控式鍵盤](images/input-scopes/default.png)
- 與**預設值**相同的版面配置
- 拼字檢查：預設為關閉，可以啟用
- 自動校正：預設為關閉，可以啟用
- 自動大小寫：預設為關閉，可 (每個單字的第一個字母為大寫) 
- 文字預測：預設為關閉，可以啟用