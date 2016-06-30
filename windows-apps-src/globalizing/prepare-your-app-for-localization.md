---
author: DelfCo
Description: "準備 app 以針對其他市場、語言或地區進行當地語系化。"
title: "準備 app 以進行當地語系化"
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
label: Prepare your app for localization
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: e52a5322767677859e32ccbecf4951745c49f36f

---

# 準備 app 以進行當地語系化





準備 app 以針對其他市場、語言或地區進行當地語系化。 在您開始之前，請務必閱讀[可行與禁止事項](guidelines-and-checklist-for-globalizing-your-app.md)。

## <span id="use_resource_files_and_qualifiers."></span><span id="USE_RESOURCE_FILES_AND_QUALIFIERS."></span>使用資源檔案和限定詞


請務必在資源檔中指定您應用程式的 UI 字串，而不是將它們放在您的程式碼中 如需詳細資訊，請參閱[將 UI 字串放入資源](put-ui-strings-into-resources.md)。

在影像或其他檔案資源的檔案或資料夾中，以適當的語言標記指定它們的資源。 請注意，當地語系化影像、音訊及視訊需要使用大量的系統資源，因此，最好盡量使用中性媒體資產。 若要深入了解，請參閱[如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。

## <span id="add_contextual_comments."></span><span id="ADD_CONTEXTUAL_COMMENTS."></span>加入內容相關的註解


在應用程式的資源檔案中加入當地語系化的註解。 當地語系化人員可以看見這些註解，而且這些註解應該提供內容相關的資訊，以協助當地語系化人員準確翻譯資源。 這些註解也應該在資源上提供足夠的限制資訊，讓翻譯不會破壞軟體。 當地語系化人員可以選擇使用 Makepri.exe 工具記錄註解。

**XAML：**Resw 檔案 (針對使用 XAML 的應用程式在 Visual Studio 中建立的資源) 有一個註解元素。 例如：

```XML
<data name="String1">
    <value>Hello World</value>
    <comment>A greeting (This is a comment to the localizer)</comment>
</data>
```

**HTML：**Resjson 檔案 (針對使用 HTML 的應用程式在 Visual Studio 中建立的資源) 允許開頭為底線之欄位中的中繼資料，例如註解：

```json
{
    "String1"  : "Hello World",
    "_String1.comment" : "A greeting (This is a comment to the localizer)"
}
```

## <span id="localize_sentences_instead_of_words."></span><span id="LOCALIZE_SENTENCES_INSTEAD_OF_WORDS."></span>當地語系化句子而不是字詞。


考量以下字串："The {0} could not be synchronized" ({0} 不能同步)。

其中的 {0} 可能會以各種字詞來取代，例如約會、工作或文件。 雖然這個範例似乎對於英文沒有問題，不過在德文版的對應句子中不見得適用。 請注意，在下列德文句子中，範本字串 ("Der"、"Die"、"Das") 中的部分字詞需要符合參數化的字詞：

| 英文                                    | 德文                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. (約會不能同步。) | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized. (工作不能同步。)        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized. (文件不能同步。)    | Das Dokument konnte nicht synchronisiert werden. |

 

在另一個範例中，假設有一個句子 "Remind me in {0} minute(s)" (在 {0} 分鐘後提醒我)。 使用 "minute(s)" (分鐘) 適用於英文，但其他語言可能會使用不同的詞彙。 例如，波蘭文會根據上下文使用 "minuta"、"minuty" 或 "minut"。

若要解決這個問題，請將完整句子當地語系化，而不是單一字詞。 這看起來好像是額外的工作而且不是很好的解決方案，但卻是最佳解決方案，因為：

-   將針對所有語言顯示清楚的錯誤訊息。
-   您的當地語系化人員不需要詢問要用來取代字串的內容。
-   在應用程式完成之後出現這種問題時，您不需要實作耗費成本的程式碼修正。

## <span id="ensure_the_correct_parameter_order."></span><span id="ENSURE_THE_CORRECT_PARAMETER_ORDER."></span>確保正確的參數順序。


不要假設所有的語言都會以相同順序使用參數。 例如，假設有一個字串是 "Every %s %s"，其中第一個 %s 會以月份名稱取代，而第二個 %s 會以一個月中的某一個日期取代。 這個範例在英文版中可以運作，但是在將應用程式當地語系化為德文版時便會失敗，因為日期和月份會以相反的順序顯示。

若要解決這個問題，請將這個字串變更為 "Every %1 %2"，如此一來，就可以根據語言來交換順序。

## <span id="don_t_over_localize."></span><span id="DON_T_OVER_LOCALIZE."></span>不要過度當地語系化。


當地語系化特定的字串，不是標記。 請考量以下範例：

| 過度當地語系化的字串                   | 適度當地語系化的字串 |
|:--------------------------------------- |:-------------------------- |
| &lt;連結&gt;使用規定&lt;/連結&gt;   | 使用規定               |
| &lt;連結&gt;隱私權原則&lt;/連結&gt; | 隱私權原則             |

 

在資源中包含上面的 &lt;link&gt; 標記，表示會當地語系化。 這樣會讓標記失去作用。 只有字串本身需要當地語系化。 通常，您應該將標記視為程式碼，應該讓標記與當地語系化的內容有所區隔。 不過，某些長字串應該包含標記，以保留內容並確保順序。

## <span id="do_not_use_the_same_strings_in_dissimilar_contexts."></span><span id="DO_NOT_USE_THE_SAME_STRINGS_IN_DISSIMILAR_CONTEXTS."></span>不要在不同的內容中使用相同字串。


重複使用字串看似最佳解決方案，但是如果相同的字詞或片語含有不同的意義或內容，就會產生當地語系化的問題。

如果兩個字串的內容一樣，可以重複使用字串。 例如，您可以在音效音量和音樂音量重複使用 "Volume" (音量) 這個字串，因為這兩者都是指稱聲音的強度。 您不應該重複使用相同的字串來指稱硬碟的磁碟區，因為內容和意義都不同，而且字詞的翻譯方式可能也不一樣。

另一個範例是使用字串 "on" (開) 和 "off" (關)。 在英文中，"on" (開) 和 "off" (關) 可以用於飛安模式、藍牙及裝置的切換。 但是在義大利文中，翻譯會根據內容是開啟或關閉而定。 您需要針對每個內容建立一對字串。

此外，像是 "text" 或 "fax" 的字串在英文中可以用來做為動詞和名詞，這會在翻譯過程中產生混淆。 替代方式是改為建立適合動詞和名詞格式的個別字串。 當您不確定內容是否相同時，為了慎重起見，請使用不同的字串。

## <span id="identify_resources_with_unique_attributes."></span><span id="IDENTIFY_RESOURCES_WITH_UNIQUE_ATTRIBUTES."></span>利用唯一的屬性識別資源。


資源識別碼不區分大小寫，在每個資源檔案中都必須是唯一的。 存取資源時，請使用資源識別碼，而不是資源的實際值。 資源識別碼不會變更，但是資源的實際值會根據語言而變更。

請確定使用有意義的資源識別碼提供其他內容以進行翻譯。

在傳送字串資源進行翻譯之後，請勿變更資源識別碼。 當地語系化團隊會使用資源識別碼，來追蹤資源中的新增、刪除及更新。 資源識別碼中的變更 (又稱為「資源識別碼轉換」) 需要重新翻譯字串，因為會出現像是字串已刪除或加入其他字串的情況。

## <span id="choose_an_appropriate_translation_approach."></span><span id="CHOOSE_AN_APPROPRIATE_TRANSLATION_APPROACH."></span>選擇適當的翻譯方式。


將字串分成資源檔案之後，就可以進行翻譯。 翻譯字串的理想時機是在完成專案中的字串之後，這通常是在專案結束之前發生。 您可以透過一些方法處理翻譯程序。 這取決於要翻譯的字串數量、要翻譯的語言數量，以及完成翻譯的方式 (例如，內部作業和雇用外部廠商)。

請考慮以下選項：

-   **直接在專案中開啟資源檔案進行翻譯。** 這個方式適用於含有少量字串的專案，以及需要翻譯為兩到三種語言的專案。 如果開發人員了解多種語言，而且願意處理翻譯程序，則適用這個案例。 這個處理方式的好處是非常快速，不需要工具，而且可以將錯譯的風險降至最低，但是無法擴充。 特別是使用不同語言的資源很容易就變得不同步，因而產生不良的使用者經驗且不易維護。
-   **字串資源檔案為 XML 或 ResJSON 文字格式，因此可以使用任何文字編輯器處理翻譯。 接著將翻譯的檔案複製回專案中。** 這個處理方式的風險在於翻譯人員可能會意外編輯 XML 標記，但是它讓翻譯人員可以在 Microsoft Visual Studio 專案以外的地方進行處理。 這個處理方式適合需要翻譯少數語言的專案。 XLIFF 格式是特別設計用於當地語系化的 XML 格式，一些當地語系化廠商或當地語系化工具應該都能支援。 您可以使用[多語應用程式工具組](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx)，從其他資源檔案 (如 .resw 或 .resjson) 產生 XLIFF 檔案。

其他檔案也可能需要提交給當地語系化人員進行翻譯，例如影像或音訊檔案。 通常，我們不建議建立具備文化特性的相依檔案，因為很難進行當地語系化。

此外，請考慮下列建議：

-   **使用當地語系化工具。** 有許多當地語系化工具可以用來剖析資源檔案，並且讓翻譯人員只能編輯可翻譯的字串。 這個方法可以降低翻譯人員不小心編輯到 XML 標記的風險。 但是它的缺點是要學習使用新的工具及當地語系化程序。 如果是含有大量字串但少量語言的專案，則適合使用當地語系化工具。 若要深入了解，請參閱[如何使用多語應用程式工具組](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx)。
-   **使用當地語系化廠商。** 如果您的專案包含大量的字串且需要翻譯為多種語言，請考慮透過當地語系化廠商來進行。 當地語系化廠商可以提供工具和程序，以及翻譯資源檔案的相關建議。 這是一個理想的解決方案，但也是最耗費成本的選項，而且可能會增加翻譯內容的往返時間。
-   **讓當地語系化人員掌握最新資訊。** 讓當地語系化人員知道字串應視為名詞或動詞。 使用詞彙工具為當地語系化人員說明使用的字詞。 以正確的文法清楚表達字串，盡量使用非技術性的說法，避免造成混淆。

## <span id="keep_access_keys_and_labels_consistent."></span><span id="KEEP_ACCESS_KEYS_AND_LABELS_CONSISTENT."></span>保持便捷鍵和標籤一致。


「同步處理」協助工具中使用的便捷鍵與顯示當地語系化的便捷鍵是一項挑戰，因為這兩個字串資源分屬於兩個不同的區段。 務必為標籤字串提供註解，例如： `Make sure that the emphasized shortcut key  is synchronized with the access key.`

**HTML：**

您可以依照如下所示的實作。 同樣地，請務必適當地註解標籤字串，以便將它連結到存取金鑰定義。

```HTML
<label id="theLabel" data-win-res="{accessKey: 'theLabelAccessKey'}" for="xPrinterRedirection" accessKey="L">The <u>L</u>abel</label>
<input type="checkbox" value="OFF" id="xPrinterRedirection" name="xPrinterRedirection" />
```

## <span id="support_furigana_for_japanese_strings_that_can_be_sorted."></span><span id="SUPPORT_FURIGANA_FOR_JAPANESE_STRINGS_THAT_CAN_BE_SORTED."></span>支援可以排序的日文字串假名註解。


日文漢字字元具備可根據使用它們的字詞和內容而有一個以上發音的獨特屬性。 這會在嘗試排序日文命名的物件 (例如，應用程式名稱、檔案、歌曲等) 時發生問題。 日文漢字過去通常是以機器可以了解的順序 (稱為 XJIS) 來排序。 可惜因為這個排序順序不是拼音的順序，所以對人們而言不是很有用。

假名註解可以藉由允許使用者或建立者為他們所使用的字元指定拼音來解決這個問題。 如果您使用下列程序將假名註解加入應用程式名稱，可以確定它會在應用程式清單的正確位置進行排序。 如果應用程式名稱包含漢字字元，而且當使用者的 UI 語言或排序順序設定為日文時未提供假名註解，則 Windows 會盡最大的努力產生適當的發音。 不過，要排序的應用程式名稱可能會包含少見或獨特的讀法，而不是較常見的讀法。 因此，日文應用程式 (特別是名稱中包含漢字字元的應用程式) 的最佳做法是，在日文當地語系化程序期間提供假名註解版的應用程式名稱。

1.  新增 "ms-resource:Appname" 做為「套件顯示名稱」與「應用程式顯示名稱」。
2.  在 strings 底下建立 ja-JP 資料夾，然後新增兩個資源檔案，如下所示：

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3.  在一般 ja-JP 的 Resources.resw 中：為應用程式名稱「希蒼」新增字串資源
4.  在日文假名註解資源的 Resources.altform-msft-phonetic.resw 中：為應用程式名稱「のあ」新增假名註解值

使用者可以使用假名註解值「のあ」(noa) 和拼音值 (從輸入法 (IME) 使用 **GetPhonetic** 函式)「まれあお」(mare-ao)，搜尋應用程式名稱「希蒼」。

排序會遵循**地區控制台**格式：

-   在日文版使用者地區設定下，
    -   如果已啟用假名註解，「希蒼」會排序於「の」下方。
    -   如果遺漏了假名註解，「希蒼」會排序於「ま」下方。
-   在非日文版使用者地區設定下，
    -   如果已啟用假名註解，「希蒼」會排序於「の」下方。
    -   如果遺漏了假名註解，「希蒼」會排序於「漢字」下方。

## <span id="related_topics"></span>相關主題


* [全球化和當地語系化的可行與禁止事項](guidelines-and-checklist-for-globalizing-your-app.md)
* [將 UI 字串放入資源](put-ui-strings-into-resources.md)
* [如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
 

 






<!--HONumber=Jun16_HO4-->


