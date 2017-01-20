---
author: Karl-Bridge-Microsoft
Description: "為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。"
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "使用輸入範圍來變更觸控式鍵盤"
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: "鍵盤、協助工具、瀏覽、焦點、文字、輸入、使用者互動"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: 482530931fe5764f65d2564107318c272c5c7b7f
ms.openlocfilehash: caaa6228f2d5b2bb6566ccb285d90a396a1caf01

---

# <a name="use-input-scope-to-change-the-touch-keyboard"></a>使用輸入範圍來變更觸控式鍵盤
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632)</li>
<li>[**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028)</li>
</ul>
</div>


當您的應用程式在具備觸控式螢幕的裝置上執行時，可以使用觸控式鍵盤輸入文字。 當使用者點選可編輯的輸入欄位 (例如 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 或 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548)) 時，就會叫用觸控式鍵盤。 您可以設定文字控制項的「輸入範圍」**，使它符合您預期使用者輸入的資料類型，讓使用者在您的應用程式中輸入資料時更加快速方便。 輸入範圍會提供控制項所預期之文字輸入類型的提示給系統，讓系統可以為該輸入類型提供專用的觸控式鍵盤配置。

例如，如果文字方塊只用來輸入 4 位數 PIN，請將 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 屬性設定為 **Number**。 這會告訴系統顯示數字鍵台配置，方便使用者輸入 PIN。

> **重要**&nbsp;&nbsp;
- 此資訊只適用於 SIP。 它並不適用於硬體鍵盤或 Windows [輕鬆存取] 選項中提供的 [螢幕小鍵盤]。
- 輸入範圍並不會導致執行任何輸入驗證，也不會防止使用者透過硬體鍵盤或其他輸入裝置提供任何輸入。 您仍然必須視需要在程式碼中驗證輸入。

## <a name="changing-the-input-scope-of-a-text-control"></a>變更文字控制項的輸入範圍

您的應用程式中可用的輸入範圍是 [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028) 列舉的成員。 您可以將 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 或 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 的 **InputScope** 屬性設定成這些值的其中之一。

> **重要**&nbsp;&nbsp;[**InputScope**](https://msdn.microsoft.com/library/windows/apps/dn996570) 屬性 (其位於 [**PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519) 上) 僅支援 **Password** 及 **NumericPin** 值。 會忽略任何其他的值。

您會在這裡變更數個文字方塊的輸入範圍，使其符合每個文字方塊的預期資料。

**在 XAML 中變更輸入範圍**

1.  在您頁面的 XAML 檔案中，找出您想要變更之文字控制項的標記。
2.  將 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 屬性新增到標記中，並指定符合預期輸入的 [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028) 值。

    以下是一些可能會出現在常見客戶連絡人表單上的文字方塊。 設定 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 之後，就會為每個文字方塊顯示具有適當資料配置的觸控式鍵盤。

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**在程式碼中變更輸入範圍**

1.  在您頁面的 XAML 檔案中，找出您想要變更之文字控制項的標記。 如果未設定，請設定 [x:Name 屬性](https://msdn.microsoft.com/library/windows/apps/mt204788)，以便在您的程式碼中參考控制項。

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  具現化新的 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025) 物件。

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  具現化新的 [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027) 物件。
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  將 [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702032) 物件的 [**NameValue**](https://msdn.microsoft.com/library/windows/apps/hh702027) 屬性設定為 [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028) 列舉的值。

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  將 [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027) 物件新增到 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702034) 物件的 [**Names**](https://msdn.microsoft.com/library/windows/apps/hh702025) 集合。

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  將 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025) 物件設定為文字控制項的 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 屬性值。

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

[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 和 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 控制項有數個影響 SIP 行為的屬性。 若要為使用者提供最佳的體驗，了解這些屬性如何影響使用觸控進行的文字輸入，就相當重要。

-   [**IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688) ─ 當為文字控制項啟用拼字檢查時，該控制項會與系統的拼字檢查引擎互動，將無法辨識的單字標示出來。 您可以點選單字以查看建議的校正清單。 預設會啟用拼字檢查功能。

    對於 **Default** 輸入範圍，這個屬性也可以自動大寫句子中的第一個字，並自動校正您輸入的文字。 這些自動校正功能在其他輸入範圍可能會停用。 如需詳細資訊，請參閱本主題稍後的表格。

-   [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690) — 文字控制項啟用文字預測時，系統會顯示您可能正開始輸入的文字清單。 您可以從清單中選取單字，這樣您就不必輸入整個單字。 文字預測預設為啟用。

    如果輸入範圍不是 **Default**，則即使 [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690) 屬性為 **true**，也可能停用文字預測功能。能。 如需詳細資訊，請參閱本主題稍後的表格。

    **注意**&nbsp;&nbsp;在行動裝置系列上，文字預測和拼字校正是顯示在鍵盤上方區域的 SIP 中。 如果 [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690) 設為 **false**，則即使 [**IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688) 為 **true**，SIP 的這個部分也會隱藏，並且會停用自動校正。

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](https://msdn.microsoft.com/library/windows/apps/dn299273) — 此屬性是 **true** 時，當焦點透過程式設計設定在文字控制項上時，系統將不會顯示 SIP。 取而代之的是，只有當使用者與控制項互動時，才會顯示鍵盤。

## <a name="touch-keyboard-index-for-windows-and-windows-phone"></a>適用於 Windows 和 Windows Phone 的觸控式鍵盤索引

這些表格會針對通用輸入範圍值顯示桌面和行動裝置上的螢幕輸入面板 (SIP) 配置。 針對每個輸入範圍，會列出對 **IsSpellCheckEnabled** 和 **IsTextPredictionEnabled** 屬性所啟用之功能的輸入範圍效果。 這並非可用之輸入範圍的完整清單。

> **注意**&nbsp;&nbsp;行動裝置上的 SIP 大小越小，就會讓您針對行動裝置應用程式設定正確的輸入範圍特別重要。 如這裡所示，Windows Phone 提供更多樣的專用鍵盤配置。 文字欄位如果在 Windows 市集應用程式中不需要設定輸入範圍，不過在 Windows Phone 市集應用程式中設定輸入範圍可能會有好處。

> **秘訣**&nbsp;&nbsp;大部分觸控式鍵盤皆可供您在字母配置與數字和符號配置之間切換。 在 Windows 上，請切換 **&amp;123** 鍵。 在 Windows Phone 上，按 [&amp;123]**** 鍵以變更為數字和符號配置，然後按 [abcd]**** 鍵以變更為字母配置。

### <a name="default"></a>預設值

`<TextBox InputScope="Default"/>`

預設鍵盤。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![預設 Windows 觸控式鍵盤](images/input-scopes/kbdpcdefault.png) | ![預設 Windows Phone 觸控式鍵盤](images/input-scopes/kbdwpdefault.png) |

功能可用性：

-   拼字檢查：如果 **IsSpellCheckEnabled** = **true**，則為啟用；如果 **IsSpellCheckEnabled** = **false**，則為停用
-   自動校正：如果 **IsSpellCheckEnabled** = **true**，則為啟用；如果 **IsSpellCheckEnabled** = **false**，則為停用
-   自動校正大寫：如果 **IsSpellCheckEnabled** = **true**，則為啟用；如果 **IsSpellCheckEnabled** = **false**，則為停用
-   文字預測：如果 **IsTextPredictionEnabled** = **true**，則為啟用；如果 **IsTextPredictionEnabled** = **false**，則為停用

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

預設的數字和符號鍵盤配置。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows 貨幣觸控式鍵盤](images/input-scopes/kbdpccurrencyamountandsymbol.png)<br>也包含向左翻頁/向右翻頁鍵來顯示更多符號。| ![Windows Phone 貨幣觸控式鍵盤](images/input-scopes/kbdwpcurrencyamountandsymbol.png) |
|功能可用性：<ul><li>拼字檢查：預設為開啟，可以停用</li><li>自動校正：一律停用</li><li>自動大寫：一律停用</li><li>文字預測：一律停用</li></ul>與 **Number** 和 **TelephoneNumber** 相同。 | 功能可用性：<ul><li>拼字檢查：預設為開啟，可以停用</li><li>自動校正：預設為開啟，可以停用</li><li>自動大寫：一律停用</li><li>文字預測：預設為開啟，可以停用</li>| 

### <a name="url"></a>URL

`<TextBox InputScope="Url"/>`

包含 [.com]**** 和 [前往鍵]![](images/input-scopes/kbdgokey.png) (前往) 鍵。 長按 [.com]**** 鍵不放以顯示其他選項 ([.org]****、[.net]**** 和地區特定尾碼)。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows URL 觸控式鍵盤](images/input-scopes/kbdpcurl.png)<br>也包含 [:]****、[-]**** 及 [/]**** 鍵。| ![Windows Phone URL 觸控式鍵盤](images/input-scopes/kbdwpurl.png)<br>長按句點鍵可顯示其他選項 ( - + &quot; / &amp; : , )。 |
|功能可用性：<ul><li>拼字檢查：預設為開啟，可以停用</li><li>自動校正：預設為開啟，可以停用</li><li>自動大寫：一律停用</li><li>文字預測：一律停用</li></ul> | 功能可用性：<ul><li>拼字檢查：預設為關閉，可以啟用</li><li>自動校正：預設為關閉，可以啟用</li><li>自動大寫：預設為關閉，可以啟用</li><li>文字預測：預設為關閉，可以啟用</li></ul> |

### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

包含 [@]**** 和 [.com]**** 鍵。 長按 [.com]**** 鍵不放以顯示其他選項 ([.org]****、[.net]**** 和地區特定尾碼)。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows 電子郵件地址觸控式鍵盤](images/input-scopes/kbdpcemailsmtpaddress.png)<br>也包含 [_]**** 和 [-]**** 鍵。| ![Windows Phone 電子郵件地址觸控式鍵盤](images/input-scopes/kbdwpemailsmtpaddress.png)<br>按住句點鍵可顯示其他選項 ( - _ , ; )。 |
|功能可用性：<ul><li>拼字檢查：預設為開啟，可以停用</li><li>自動校正：預設為開啟，可以停用</li><li>自動大寫：一律停用</li><li>文字預測：一律停用</li></ul> | 功能可用性：<ul><li>拼字檢查：預設為關閉，可以啟用</li><li>自動校正：預設為關閉，可以啟用</li><li>自動大寫：預設為關閉，可以啟用</li><li>文字預測：預設為關閉，可以啟用</li></ul> |

### <a name="number"></a>Number

`<TextBox InputScope="Number"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows 數字觸控式鍵盤](images/input-scopes/kbdpccurrencyamountandsymbol.png)| ![Windows Phone 數字觸控式鍵盤](images/input-scopes/kbdwpnumber.png)<br>鍵盤包含數字和小數點。 按住小數點鍵可顯示其他選項 ( , - )。 |
|與 **CurrencyAmountAndSymbol** 和 **TelephoneNumber** 相同。 | 功能可用性：<ul><li>拼字檢查：一律停用</li><li>自動校正：一律停用</li><li>自動大寫：一律停用</li><li>文字預測：一律停用</li></ul> |

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows 電話號碼觸控式鍵盤](images/input-scopes/kbdpccurrencyamountandsymbol.png)| ![Windows Phone 電話號碼觸控式鍵盤](images/input-scopes/kbdwptelephonenumber.png)<br>鍵盤會模擬電話數字鍵台。 按住句點鍵可顯示其他選項 ( , ( ) X . )。 按住 0 鍵可輸入 +。 |
|與 **CurrencyAmountAndSymbol** 和 **TelephoneNumber** 相同。 | 功能可用性：<ul><li>拼字檢查：一律停用</li><li>自動校正：一律停用</li><li>自動大寫：一律停用</li><li>文字預測：一律停用</li></ul> |

### <a name="search"></a>搜尋

`<TextBox InputScope="Search"/>`

包含 [搜尋]**** 鍵，而不是 [Enter]**** 鍵。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows 搜尋觸控式鍵盤](images/input-scopes/kbdpcsearch.png)| ![Windows Phone 搜尋觸控式鍵盤](images/input-scopes/kbdwpsearch.png)|
|功能可用性：<ul><li>拼字檢查：預設為開啟，可以停用</li><li>自動校正：一律停用</li><li>自動大寫：一律停用</li><li>文字預測：預設為開啟，可以停用</li></ul> | 功能可用性：<ul><li>拼字檢查：預設為開啟，可以停用</li><li>自動校正：預設為開啟，可以停用</li><li>自動大寫：一律停用</li><li>文字預測：預設為開啟，可以停用</li></ul> |

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![預設 Windows 觸控式鍵盤](images/input-scopes/kbdpcdefault.png)<br>與 **Default** 的配置相同。| ![預設 Windows Phone 觸控式鍵盤](images/input-scopes/kbdwpdefault.png)|
|功能可用性：<ul><li>拼字檢查：預設為關閉，可以啟用</li><li>自動校正：一律停用</li><li>自動大寫：一律停用</li><li>文字預測：一律停用</li></ul> | 與 **Default** 相同。 |

### <a name="formula"></a>公式

`<TextBox InputScope="Formula"/>`

包含 [=]**** 鍵。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows 電話公式觸控式鍵盤](images/input-scopes/kbdpcformula.png)<br>也包含 [%]****、[$]**** 及 [+]**** 鍵。| ![Windows Phone 公式觸控式鍵盤](images/input-scopes/kbdwpformula.png)<br>按住句點鍵可顯示其他選項 ( - ! ? , ). 按住 [=]**** 鍵可顯示其他選項 ( ( ) : &lt; &gt; )。 |
|功能可用性：<ul><li>拼字檢查：預設為關閉，可以啟用</li><li>自動校正：一律停用</li><li>自動大寫：一律停用</li><li>文字預測：一律停用</li></ul> | 功能可用性：<ul><li>拼字檢查：預設為開啟，可以停用</li><li>自動校正：預設為開啟，可以停用</li><li>自動大寫：一律停用</li><li>文字預測：預設為開啟，可以停用</li></ul> |

### <a name="chat"></a>Chat

`<TextBox InputScope="Chat"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![預設 Windows 觸控式鍵盤](images/input-scopes/kbdpcdefault.png)<br>與 **Default** 的配置相同。| ![預設 Windows Phone 觸控式鍵盤](images/input-scopes/kbdwpdefault.png)<br>與 **Default** 的配置相同。|
|功能可用性：<ul><li>拼字檢查：預設為關閉，可以啟用</li><li>自動校正：一律停用</li><li>自動大寫：一律停用</li><li>文字預測：一律停用</li></ul> | 功能可用性：<ul><li>拼字檢查：預設為開啟，可以停用</li><li>自動校正：預設為開啟，可以停用</li><li>自動大寫：預設為開啟，可以停用</li><li>文字預測：預設為開啟，可以停用</li></ul> |

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![預設 Windows 觸控式鍵盤](images/input-scopes/kbdpcdefault.png)<br>與 **Default** 的配置相同。| ![Windows Phone 名稱或電話號碼觸控式鍵盤](images/input-scopes/kbdwpnameorphonenumber.png)<br>包含 [;]**** 和 [@]**** 鍵。 [&amp;123]**** 鍵已取代為 [123]**** 鍵，會開啟電話鍵台 (請參閱**TelephoneNumber**)。|
|功能可用性：<ul><li>拼字檢查：預設為開啟，可以停用</li><li>自動校正：一律停用</li><li>自動大寫：一律啟用</li><li>文字預測：一律停用</li></ul> | 功能可用性：<ul><li>拼字檢查：預設為關閉，可以啟用</li><li>自動校正：預設為關閉，可以啟用</li><li>自動大寫：預設為關閉，可以啟用。 每個字的第一個字母大寫。</li><li>文字預測：預設為關閉，可以啟用</li></ul> |



<!--HONumber=Dec16_HO3-->


