---
Description: 使用者輸入時提供建議的文字輸入方塊。
title: 自動建議方塊的指導方針
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e8a4c347bd2baa51115ecd9315f923e205133a6e
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081266"
---
# <a name="auto-suggest-box"></a>自動建議方塊

使用 AutoSuggestBox 提供讓使用者在輸入時可從中選取建議的清單。

![自動建議方塊](images/controls/auto-suggest-box-open.png)

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](/windows/uwp/design/style/rounded-corner)。 WinUI 是 NuGet 套件，包含適用於 UWP 應用程式的新控制項與 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API**：[AutoSuggestBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox)[TextChanged 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged), [SuggestionChose 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen)[QuerySubmitted 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

如果您想要簡單且可自訂的控制項，允許文字搜尋帶有建議清單，請選擇自動建議方塊。

如需如何選擇正確文字控制項的詳細資訊，請參閱[文字控制項](text-controls.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/AutoSuggestBox">開啟應用程式並查看 AutoSuggestBox 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Groove 音樂 app 中的自動建議方塊。

![Groove 音樂 app 中的自動建議方塊](images/control-examples/auto-suggest-box-groove.png)

## <a name="anatomy"></a>結構
自動建議方塊的進入點，包含選用標頭和帶有選用提示文字的文字方塊：

![自動建議控制項的進入點範例](images/controls_autosuggest_entrypoint.png)

當使用者開始輸入文字，自動建議結果清單就會自動產生。 結果清單可顯示在文字輸入方塊的上方或下方。 [全部清除] 按鈕將會出現：

![展開的自動建議控制項範例](images/controls_autosuggest_expanded01.png)

## <a name="create-an-auto-suggest-box"></a>建立自動建議方塊

若要使用 AutoSuggestBox，您需要回應 3 個使用者動作。

- 變更文字 - 當使用者輸入文字時，更新建議清單。
- 選擇建議 - 當使用者選擇建議清單中的建議時，更新文字方塊。
- 提交查詢 - 當使用者提交查詢時，顯示查詢結果。

### <a name="text-changed"></a>變更文字

每當更新文字方塊的內容時，就會發生 [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged) 事件。 使用事件引數 [Reason](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason) 屬性判斷變更是否因為使用者輸入所造成。 如果變更原因是 **UserInput**，請根據輸入，篩選您的資料。 接著，將已篩選的資料設定為 AutoSuggestBox 的 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 以更新建議清單。

若要控制建議清單中顯示項目的方式，您可以使用 [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) 或 [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)。

- 若要顯示資料項目單一屬性的文字，請設定 DisplayMemberPath 屬性以選擇您物件的哪個屬性要顯示在建議清單中。
- 若要為清單中的每個項目自訂外觀，請使用 the ItemTemplate 屬性。

### <a name="suggestion-chosen"></a>選擇建議

當使用者使用鍵盤瀏覽建議清單時，您需要更新文字方塊中要符合的文字。

您可以設定 [TextMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textmemberpath) 屬性以選擇您資料物件的哪個屬性要顯示在文字方塊中。 如果您指定 TextMemberPath，就會自動更新文字方塊。 您通常應該為 DisplayMemberPath 和 TextMemberPath 指定相同的值，如此一來，建議清單和文字方塊中的文字便相同。

如果您需要顯示多個簡單的屬性，處理 [SuggestionChosen](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen) 事件，以便根據選取的項目，將自訂文字填入文字方塊。

### <a name="query-submitted"></a>提交查詢

請處理 [QuerySubmitted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted) 事件以執行適合您 App 的查詢動作，並向使用者顯示結果。

QuerySubmitted 事件會在使用者確認查詢字串時發生。 使用者可以透過下列其中一種方式確認查詢：
- 當焦點在文字方塊中時，按下 Enter 或按一下查詢圖示。 事件引數 [ChosenSuggestion](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion) 屬性為 **null**。
- 當焦點在建議清單中時，按下 Enter，按一下或點選項目。 事件引數 ChosenSuggestion property 包含從清單選取的項目。

在所有情況下，事件引數 [QueryText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext) 屬性都會包含來自文字方塊的文字。

以下是一個包含所需事件處理常式的簡單 AutoSuggestBox。

```xaml
<AutoSuggestBox PlaceholderText="Search" QueryIcon="Find" Width="200"
                TextChanged="AutoSuggestBox_TextChanged"
                QuerySubmitted="AutoSuggestBox_QuerySubmitted"
                SuggestionChosen="AutoSuggestBox_SuggestionChosen"/>
```

```csharp
private void AutoSuggestBox_TextChanged(AutoSuggestBox sender, AutoSuggestBoxTextChangedEventArgs args)
{
    // Only get results when it was a user typing,
    // otherwise assume the value got filled in by TextMemberPath
    // or the handler for SuggestionChosen.
    if (args.Reason == AutoSuggestionBoxTextChangeReason.UserInput)
    {
        //Set the ItemsSource to be your filtered dataset
        //sender.ItemsSource = dataset;
    }
}


private void AutoSuggestBox_SuggestionChosen(AutoSuggestBox sender, AutoSuggestBoxSuggestionChosenEventArgs args)
{
    // Set sender.Text. You can use args.SelectedItem to build your text string.
}


private void AutoSuggestBox_QuerySubmitted(AutoSuggestBox sender, AutoSuggestBoxQuerySubmittedEventArgs args)
{
    if (args.ChosenSuggestion != null)
    {
        // User selected an item from the suggestion list, take an action on it here.
    }
    else
    {
        // Use args.QueryText to determine what to do.
    }
}
```

## <a name="use-autosuggestbox-for-search"></a>使用 AutoSuggestBox 搜尋

使用 AutoSuggestBox 提供讓使用者在輸入時可從中選取建議的清單。

文字輸入方塊預設不會顯示查詢按鈕。 您可以設定 [QueryIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.queryicon) 屬性，以便在文字方塊右側新增包含指定圖示的按鈕。 例如，若要讓 AutoSuggestBox 看起來像是典型的搜尋方塊，請加入「Find」圖示，就像這樣：

```xaml
<AutoSuggestBox QueryIcon="Find"/>
```

以下是包含「尋找」圖示的 AutoSuggestBox。

![自動建議控制項的進入點範例](images/controls_autosuggest_entrypoint.png)

## <a name="dos-and-donts"></a>可行與禁止事項

-   當使用自動建議方塊執行搜尋，且輸入的文字沒有搜尋結果時，會顯示單行「沒有結果」訊息作為結果，讓使用者知道他們的搜尋要求已經執行：

    ![沒有搜尋結果的自動建議方塊範例](images/controls_autosuggest_noresults.png)

<!--
<div class="microsoft-internal-note">
**Globalization and localization checklist**

<table>
<tr>
<th>Vertical spacing</th><td>Use non-Latin characters for vertical spacing to ensure non-Latin scripts will display properly, including numbers.</td>
</tr>
<tr>
<th>Scrolling</th><td>When auto suggest text is selected, user should be able to scroll to end of string.</td>
</tr>
</table>
</div>
-->

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
