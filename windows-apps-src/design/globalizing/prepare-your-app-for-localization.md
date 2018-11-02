---
author: stevewhims
Description: A localized app is one that can be localized to other markets, languages, or regions without uncovering any functional defects in the app. The most essential property of a localizable app is that its executable code has been cleanly separated from its localizable resources.
title: 讓您的應用程式可當地語系化
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.author: stwhi
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: 48244889dd927f41d0998214cf1120377c4bb251
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5975016"
---
# <a name="make-your-app-localizable"></a>讓您的應用程式可當地語系化

當地語系化的應用程式是可為其他市場、語言或地區當地語系化，而不會使應用程式產生任何功能缺失的應用程式。 可當地語系化應用程式最重要的屬性，便是其可執行的程式碼和可當地語系化的資源完全分離。 因此，您應判斷您應用程式的哪些資源需要進行當地語系化。 詢問自己，若您需要將應用程式當地語系化以適應其他市場，需要變更什麼。

我們也建議您先熟悉[全球化指導方針](guidelines-and-checklist-for-globalizing-your-app.md)。

## <a name="put-your-strings-into-resources-files-resw"></a>將您的字串放入資源檔 (.resw)

請不要在您的命令式程式碼、XAML 標記，或您的應用程式封裝資訊清單中硬式編碼字串常值。 相反的，請將您的字串放入資源檔 (.resw)，使其可獨立於您應用程式的建置二進位檔之外，適應不同的本地市場。 如需詳細資料，請參閱[當地語系化您 UI 和應用程式封裝資訊清單中的字串](../../app-resources/localize-strings-ui-manifest.md)。

該主題也會示範如何將註解新增到您的預設資源檔 (.resw) 中。 例如，若您採用非正式的口吻或語調，請確認有在註解中解釋。 同時，為將費用降至最低，請確認您只提供需要翻譯的字串給翻譯人員。

在您的應用程式封裝資訊清單來源檔案 (`Package.appxmanifest` 檔案) 中為您的應用程式設定適當的預設語言。 預設語言會決定當使用者的慣用語言與任何您應用程式支援的語言不相符時要使用的語言。 利用他們的語言標記所有您的資源 (包括您的預設語言，例如 `\Assets\en-us\Logo.png`)，如此一來，系統便能分辨資源所使用的是哪種語言，以及該語言在特定情況中的使用情況。

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>針對語言量身打造您的影像和其他檔案資源

理論上，您可以全球化您的影像&mdash;意即使該影像與文化無關。 針對任何無法進行該操作的影像和其他檔案資源，請盡可能的視需要為其建立許多不同變體，並將適當的語言限定詞置入他們的檔案或資料名稱。 若要深入了解，請參閱[針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md))。

為最小化當地語系化成本，請不要將文字或文化敏感的材料放入要使用的影像中。 適合您自己文化特性的影像，在其他文化特性中可能具有冒犯意味或容易產生誤解。 避免使用特定文化的影像，例如信箱，因為該影像在世界各地並不常見。 避免宗教符號、動物、政治或特定性別的影像。 展示肉體、身體部位或手勢也是相當敏感的主題。 若您無法完全避免這些內容，您的影像便需要完全當地語系化。 如果您正在當地語系化為使用和您自己的語言不同閱讀方向的語言，使用對稱式影像和效果能夠更輕易地支援鏡像。

也請避免在影像中使用文字，以及在音訊/視訊檔案中使用語音。

## <a name="the-use-of-color-in-your-app"></a>您應用程式中使用的色彩

留意使用的色彩。 使用與國旗或政治動向相關聯的色彩組合可能會產生問題。 色彩的選擇可能需要經過文化專家的檢閱。 使用色彩時也可能會產生存取性問題。 若您試圖使用色彩傳達意義，您也應使用其他方式傳達相同的資訊，例如尺寸、形狀或標籤。

## <a name="consider-factoring-your-strings-into-sentences"></a>考慮將您的字串重構成句子

使用尺寸適當的字串。 短字串較容易翻譯，並可進行翻譯回收 (可節省費用，因為相同的字串不會傳送給負責當地語系化的人員超過一次)。 此外，非常長的字串可能不受當地語系化工具支援。

但此指導方針的風險是在不同內容中重複使用字串。 即使是簡單的單字，例如 &quot;on&quot; 和 &quot;off&quot; 都可能會根據內容的不同而有不同的翻譯。 在英文中，"on" (開) 和 "off" (關) 可以用於飛安模式、藍牙及裝置的切換。 但是在義大利文中，翻譯會根據內容是開啟或關閉而定。 您需要針對每個內容建立一對字串。 如果兩個字串的內容一樣，可以重複使用字串。 例如，您可以在音效音量和音樂音量重複使用 "Volume" (音量) 這個字串，因為這兩者都是指稱聲音的強度。 您不應該重複使用相同的字串來指稱硬碟的磁碟區，因為內容和意義都不同，而且字詞的翻譯方式可能也不一樣。

此外，像是 "text" 或 "fax" 的字串在英文中可以用來做為動詞和名詞，這會在翻譯過程中產生混淆。 替代方式是改為建立適合動詞和名詞格式的個別字串。 當您不確定內容是否相同時，為了慎重起見，請使用不同的字串。

簡而言之，請將您的字串構築為可在所有內容中使用的片段。 有時候字串可能必須是完整的句子。

請考慮下列字串:"{0}無法同步。 」

以各種字詞來取代{0}，例如 「 約會 」、 「 工作 」 或 「 文件 」。 雖然這個範例似乎對於英文沒有問題，但可能不見得適用於所有案例中的對應句子，例如德文。 請注意，在下列德文句子中，範本字串 ("Der"、"Die"、"Das") 中的部分字詞需要符合參數化的字詞：

| 英文                                    | 德文                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. (約會不能同步。) | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized. (工作不能同步。)        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized. (文件不能同步。)    | Das Dokument konnte nicht synchronisiert werden. |

在另一個範例，假設有一個句子 「 提醒我在{0}minute。 」 使用 "minute(s)" (分鐘) 適用於英文，但其他語言可能會使用不同的詞彙。 例如，波蘭文會根據上下文使用 "minuta"、"minuty" 或 "minut"。

若要解決這個問題，請將完整句子當地語系化，而不是單一字詞。 這看起來好像是額外的工作而且不是很好的解決方案，但卻是最佳解決方案，因為：

-   將針對所有語言顯示文法正確的訊息。
-   您的翻譯人員不需要詢問要用來取代字串的內容。
-   在應用程式完成之後出現這種問題時，您不需要實作耗費成本的程式碼修正。

## <a name="other-considerations-for-strings"></a>其他字串考量項目

避免使用您預設語言中的俚語和象徵物。 定族群 (例如，文化特性和年齡) 使用的語言，由於只有該族群使用，所以很難了解或翻譯。 同樣地，不見得人人都能理解隱喻的深層含意。 例如，只有滑雪愛好者才知道 &quot;bluebird&quot; 的其中含意，但是非滑雪愛好者，就不明白其中的含意。

不要使用技術專業用語、縮寫或首字母縮略字。 來自其他文化特性或地區且沒有相關技術背景的使用者或人員很難了解技術專業語言，而且不容易翻譯這種語言。 一般人不會在日常交談中使用這些字詞。 技術性的語言通常會出現在錯誤訊息中，用來識別硬體和軟體問題，但您的字串應「只有在使用者需要該層級的資訊，並且可採取動作或尋找能採取動作的人員」** 時，才以技術性的語言呈現。

在您的字串中使用非正式的口吻或語調是有效的選擇。 您可以在您的預設資源檔 (.resw) 中使用註解表示該意圖。

## <a name="pseudo-localization"></a>虛擬當地語系化

虛擬當地語系化您的應用程式，以發現任何可當地語系化性問題。 虛擬當地語系化是一種當地語系化的測試執行，或公開測試。 您會產生一組並未真正翻譯的資源。他們只是看起來像是已翻譯而已。 您的字串長度會比預設語言要長 40% (舉例)，當中還帶有分隔符號，讓您可以瞥見他們在 UI 中是否遭到截斷。

## <a name="geopolitical-awareness"></a>地緣政治感知

避免在地圖或參照地區時引發政治爭議。 地圖可能包含有爭議性的地區或國家邊界，而它們經常會成為政治爭議的來源。 請注意，任何用來選取國家的 UI 都要將它表示為「國家/地區」&quot;&quot;。 在清單中將有爭議性的領土標籤列為「國家」&quot;&quot;&mdash;像是在地址表單中&mdash;，可能會冒犯某些使用者。

## <a name="language--and-region-changed-events"></a>語言和地區變更事件

訂閱當系統語言和地區設定變更時引發的事件。 執行此作業，以視情況重新載入資源。 如需詳細資訊，請參閱[更新字串以回應限定詞值變更事件](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events)和[更新影像以回應限定詞值變更事件](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)。

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>在格式化字串時確定參數的順序正確

不要假設所有的語言都會以相同順序表達參數。 例如，請考慮此格式：

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

此範例中的格式化字串在英文 (美國) 中可正常運作。 但，例如，在德文 (德國) 中則不適當，因為日期和月份的顯示順序是相反的。 確保翻譯人員知道每個參數的意圖，以便他們可以反轉格式化字串中格式化項目的順序 (例如，「{1} {0}」) 為目標語言適當的。

## <a name="dont-over-localize"></a>不要過度當地語系化

只提交自然語言給翻譯人員；而非程式語言或標記。 `<link>` 標籤並非自然語言。 請思考這些範例。

| 不要當地語系化                   | 當地語系化 |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;使用規定&lt;/link&gt;   | 使用規定               |
| &lt;link&gt;隱私權原則&lt;/link&gt; | 隱私權原則             |

在您的資源檔 (.resw) 中包含 `<link>` 標籤表示其可能會當地語系化。 這會使該標籤無效。 若您有很長的字串，其中包含為維持內容和確認順序的標記，請在註解中註明不需翻譯哪些內容。

## <a name="choose-an-appropriate-translation-approach"></a>選擇適當的翻譯方式

將字串分成資源檔案之後，就可以進行翻譯。 翻譯字串的理想時機是在完成專案中的字串之後，這通常是在專案結束之前發生。 您可以透過一些方法處理翻譯程序。 這取決於要翻譯的字串數量、要翻譯的語言數量，以及完成翻譯的方式 (例如，內部作業和雇用外部廠商)。

請考慮採用下列選項。

- **直接在專案中開啟資源檔案進行翻譯。** 這個方式適用於含有少量字串，且需要翻譯為兩到三種語言的專案。 如果開發人員了解多種語言，而且願意處理翻譯程序，則適用這個案例。 這個處理方式的好處是非常快速，不需要工具，而且可以將錯譯的風險降至最低。 但無法擴充。 特別是使用不同語言的資源很容易就變得不同步，因而產生不良的使用者經驗且不易維護。
- **字串資源檔案為 XML 或 ResJSON 文字格式，因此可以使用任何文字編輯器處理翻譯。 接著將翻譯的檔案複製回專案中。** 這個處理方式的風險在於翻譯人員可能會意外編輯 XML 標記，但是它讓翻譯人員可以在 Microsoft Visual Studio 專案以外的地方進行處理。 這個處理方式適合需要翻譯少數語言的專案。 XLIFF 格式是特別設計用於當地語系化的 XML 格式，一些當地語系化廠商或當地語系化工具應該都能支援。 您可以使用[多語應用程式工具組](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx)，從其他資源檔案 (如 .resw 或 .resjson) 產生 XLIFF 檔案。

其他檔案也可能需要提交給當地語系化人員進行翻譯，例如影像或音訊檔案。

此外，請考慮這些建議。

- **使用當地語系化工具。** 有許多當地語系化工具可以用來剖析資源檔案，並且讓翻譯人員只能編輯可翻譯的字串。 這個方法可以降低翻譯人員不小心編輯到 XML 標記的風險。 但是它的缺點是要學習使用新的工具及當地語系化程序。 如果是含有大量字串但少量語言的專案，則適合使用當地語系化工具。 若要深入了解，請參閱[如何使用多語應用程式工具組](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx)。
- **使用當地語系化廠商。** 如果您的專案包含大量的字串且需要翻譯為多種語言，請考慮透過當地語系化廠商來進行。 當地語系化廠商可以提供工具和程序，以及翻譯資源檔案的相關建議。 這是一個理想的解決方案，但也是最耗費成本的選項，而且可能會增加翻譯內容的往返時間。

## <a name="keep-access-keys-and-labels-consistent"></a>保持便捷鍵和標籤一致

「同步處理」協助工具中使用的便捷鍵與顯示當地語系化的便捷鍵是一項挑戰，因為這兩個字串資源分屬於兩個不同的區段。 務必為標籤字串提供註解，例如： `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>支援可以排序的日文字串假名註解

日文漢字字元具備可根據使用他們的字詞而有一個以上閱讀方式 (發音) 的屬性。 這會在嘗試排序日文命名的物件 (例如，應用程式名稱、檔案、歌曲等) 時發生問題。 日文漢字過去通常是以機器可以了解的順序 (稱為 XJIS) 來排序。 可惜因為這個排序順序不是拼音的順序，所以對人們而言不是很有用。

「假名註解」** 可以藉由允許使用者或建立者為他們所使用的字元指定拼音來解決這個問題。 如果您使用下列程序將假名註解加入應用程式名稱，可以確定它會在應用程式清單的正確位置進行排序。 如果應用程式名稱包含漢字字元，而且當使用者的 UI 語言或排序順序設定為日文時未提供假名註解，則 Windows 會盡最大的努力產生適當的發音。 不過，要排序的應用程式名稱可能會包含少見或獨特的讀法，而不是較常見的讀法。 因此，日文應用程式 (特別是名稱中包含漢字字元的應用程式) 的最佳做法是，在日文當地語系化程序期間提供假名註解版的應用程式名稱。

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

## <a name="related-topics"></a>相關主題

* [全球化指導方針](guidelines-and-checklist-for-globalizing-your-app.md)
* [將 UI 及應用程式套件資訊清單中的字串當地語系化](../../app-resources/localize-strings-ui-manifest.md)
* [針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [調整配置和字型並支援 RTL](adjust-layout-and-fonts--and-support-rtl.md)
* [更新影像以回應限定詞值變更事件](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>範例

* [應用程式資源和當地語系化範例](http://go.microsoft.com/fwlink/p/?linkid=254478)
