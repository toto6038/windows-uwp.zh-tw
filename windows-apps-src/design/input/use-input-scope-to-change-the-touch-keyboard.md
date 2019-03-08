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
ms.openlocfilehash: 1350c6e0eae057386fb721a358f71acb19c4efc1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591763"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>使用輸入範圍來變更觸控式鍵盤

為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。

### <a name="important-apis"></a>重要 API
- [InputScope](https://msdn.microsoft.com/library/windows/apps/hh702632)
- [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/hh702028)


當您的應用程式在具備觸控式螢幕的裝置上執行時，可以使用觸控式鍵盤輸入文字。 當使用者點選可編輯的輸入欄位 (例如 **[TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)** 或 **[RichEditBox](https://msdn.microsoft.com/library/windows/apps/br227548)**) 時，就會叫用觸控式鍵盤。 您可以設定文字控制項的「輸入範圍」，使它符合您預期使用者輸入的資料類型，讓使用者在您的 App 中輸入資料時更加快速方便。 輸入範圍會提供控制項所預期之文字輸入類型的提示給系統，讓系統可以為該輸入類型提供專用的觸控式鍵盤配置。

例如，如果文字方塊只用來輸入 4 位數 PIN，請將 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 屬性設定為 **Number**。 這會告訴系統顯示數字鍵台配置，方便使用者輸入 PIN。

> [!IMPORTANT]
> - 此資訊只適用於 SIP。 它並不適用於硬體鍵盤或 Windows [輕鬆存取] 選項中提供的 [螢幕小鍵盤]。
> - 輸入範圍並不會導致執行任何輸入驗證，也不會防止使用者透過硬體鍵盤或其他輸入裝置提供任何輸入。 您仍然必須視需要在程式碼中驗證輸入。

## <a name="changing-the-input-scope-of-a-text-control"></a>變更文字控制項的輸入範圍

您的應用程式中可用的輸入範圍是 **[InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/hh702028)** 列舉的成員。 您可以將 ****TextBox**** 或 **[RichEditBox](https://msdn.microsoft.com/library/windows/apps/br209683)** 的 [InputScope](https://msdn.microsoft.com/library/windows/apps/br227548) 屬性設定成這些值的其中之一。

> [!IMPORTANT]
> **[InputScope](https://msdn.microsoft.com/library/windows/apps/dn996570)** 屬性**[PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)** 僅支援**密碼**與**NumericPin**值。 會略過其他任何值。

您會在這裡變更數個文字方塊的輸入範圍，使其符合每個文字方塊的預期資料。

**若要變更在 XAML 中的輸入的範圍**

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

**若要變更程式碼中的輸入的範圍**

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

[  **TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 和 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 控制項有數個影響 SIP 行為的屬性。 若要為使用者提供最佳的體驗，了解這些屬性如何影響使用觸控進行的文字輸入，就相當重要。

-   [**IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688)— 啟用拼字檢查時，文字控制項，控制項互動時使用系統的拼字檢查引擎標示無法辨識的字。 您可以點選單字以查看建議的校正清單。 預設會啟用拼字檢查功能。

    對於 **Default** 輸入範圍，這個屬性也可以自動大寫句子中的第一個字，並自動校正您輸入的文字。 這些自動校正功能在其他輸入範圍可能會停用。 如需詳細資訊，請參閱本主題稍後的表格。

-   [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690)— 文字控制項啟用文字預測時，系統會顯示您可能會從輸入的單字清單。 您可以從清單中選取單字，這樣您就不必輸入整個單字。 文字預測預設為啟用。

    如果輸入範圍不是 **Default**，則即使 [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690) 屬性為 **true**，也可能停用文字預測功能。能。 如需詳細資訊，請參閱本主題稍後的表格。

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](https://msdn.microsoft.com/library/windows/apps/dn299273)— 當這個屬性是 **，則為 true**，它就會防止系統顯示 SIP 焦點時以程式設計方式設定上的文字控制項。 取而代之的是，只有當使用者與控制項互動時，才會顯示鍵盤。

## <a name="touch-keyboard-index-for-windows"></a>Windows 的觸控式鍵盤索引

這些資料表會顯示一般的輸入的範圍值的 Windows 軟體輸入面板 (SIP) 配置。 針對每個輸入範圍，會列出對 **IsSpellCheckEnabled** 和 **IsTextPredictionEnabled** 屬性所啟用之功能的輸入範圍效果。 這並非可用之輸入範圍的完整清單。

> [!Tip] 
> 您可以藉由按下切換大部分字母和數字和符號版面配置之間的觸控式鍵盤 **& 123**鍵以將變更為數字和符號的版面配置，然後按**abcd**機碼將變更為字母的版面配置。

### <a name="default"></a>預設值

`<TextBox InputScope="Default"/>`

預設 Windows 觸控式鍵盤。

![預設 Windows 觸控式鍵盤](images/input-scopes/default.png)
- 拼字檢查：如果 **IsSpellCheckEnabled** = **true**，則為啟用；如果 **IsSpellCheckEnabled** = **false**，則為停用
- 自動校正：如果 **IsSpellCheckEnabled** = **true**，則為啟用；如果 **IsSpellCheckEnabled** = **false**，則為停用
- 自動校正大寫：如果 **IsSpellCheckEnabled** = **true**，則為啟用；如果 **IsSpellCheckEnabled** = **false**，則為停用
- 文字預測：如果 **IsTextPredictionEnabled** = **true**，則為啟用；如果 **IsTextPredictionEnabled** = **false**，則為停用

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

預設的數字和符號鍵盤配置。

![Windows 貨幣觸控式鍵盤](images/input-scopes/currencyamountandsymbol.png)

- 包含頁面左/右索引鍵，以顯示更多的符號
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：一律停用
- 文字預測：預設為開啟，可以停用
 
### <a name="url"></a>URL

`<TextBox InputScope="Url"/>`

![Windows URL 觸控式鍵盤](images/input-scopes/url.png)

- 包含 [.com] 和 [前往鍵]![](images/input-scopes/kbdgokey.png) (前往) 鍵。 按住不放 **.com**鍵以顯示其他選項 (**.org**， **.net**，和特定區域的後置詞)
- 包含 **:**， **-**，和**/** 金鑰
- 拼字檢查：預設為關閉，可以啟用
- 自動校正：預設為關閉，可以啟用
- 自動大寫：預設為關閉，可以啟用
- 文字預測：預設為關閉，可以啟用


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![Windows 電子郵件地址觸控式鍵盤](images/input-scopes/emailsmtpaddress.png)
- 包含 [@] 和 [.com] 鍵。 按住不放 **.com**鍵以顯示其他選項 (**.org**， **.net**，和特定區域的後置詞)
- 包含 **_** 並**-** 金鑰
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
- 包含**搜尋**而不是索引鍵**Enter**金鑰
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：一律停用
- 文字預測：預設為開啟，可以停用

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![累加搜尋的 Windows 觸控式鍵盤](images/input-scopes/searchincremental.png)
- 與相同的配置**預設**
- 拼字檢查：預設為關閉，可以啟用
- 自動校正：一律停用
- 自動大寫：一律停用
- 文字預測：一律停用

### <a name="formula"></a>公式

`<TextBox InputScope="Formula"/>`

![Windows 觸控式鍵盤的公式](images/input-scopes/formula.png)
- 包含**=** 金鑰
- 也包含**%**， **$**，以及**+** 金鑰
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：一律停用
- 文字預測：預設為開啟，可以停用

### <a name="chat"></a>Chat

`<TextBox InputScope="Chat"/>`

![預設 Windows 觸控式鍵盤](images/input-scopes/default.png)
- 與相同的配置**預設**
- 拼字檢查：預設為開啟，可以停用
- 自動校正：預設為開啟，可以停用
- 自動大寫：預設為開啟，可以停用
- 文字預測：預設為開啟，可以停用

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![預設 Windows 觸控式鍵盤](images/input-scopes/default.png)
- 與相同的配置**預設**
- 拼字檢查：預設為關閉，可以啟用
- 自動校正：預設為關閉，可以啟用
- 自動大寫： 關閉預設的情況下，可以啟用 （每個單字的第一個字母大寫）
- 文字預測：預設為關閉，可以啟用
