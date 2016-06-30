---
author: DelfCo
Description: "當您要將應用程式全球化以適應更廣泛的使用對象，或是針對特定的市場將應用程式當地語系化時，請遵循這些最佳做法。"
Search.Refinement.TopicID: 180
title: "全球化與當地語系化的指導方針"
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
label: Do's and don'ts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: bdbe6b3e319aa90a78660c664f1603bac93399ca

---

# 全球化和當地語系化的可行與禁止事項





**重要 API**

-   [**全球化**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
-   [**Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**資源**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039)

當您要將 App 全球化以適應更廣泛的使用對象，或是針對特定的市場將 App 當地語系化時，請遵循這些最佳做法。



## <span id="guidelines_for_internationalization"></span><span id="GUIDELINES_FOR_INTERNATIONALIZATION"></span>全球化

為 UI 選擇全球適用的詞彙和影像、使用 [**Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) API 格式化應用程式資料，以及避免對位置或語言進行假設，讓您的應用程式為輕鬆地適應不同市場做好準備。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">建議</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>使用正確格式的數字、日期、時間、地址及電話號碼。</p></td>
<td align="left"><p>針對數字、日期、時間及其他資料格式所使用的格式設定會根據文化特性、地區、語言及市場而所有不同。 如果您正在顯示數字、日期、時間或其他資料，請使用[<strong>全球化</strong>](https://msdn.microsoft.com/library/windows/apps/br206813) API 取得適用於特定對象的格式。</p></td>
</tr>
<tr class="even">
<td align="left"><p>支援國際紙張大小。</p></td>
<td align="left"><p>最常用的紙張大小會因為國家或地區而不同，所以，如果您包含根據紙張大小而定的功能 (例如，列印)，請確定可支援常用的國際紙張大小並加以測試。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>支援國際度量單位和貨幣。</p></td>
<td align="left"><p>即使最普遍的是公制系統和英制系統，但是，不同的國家或地區還是會使用不同的單位和比例。 如果您的內容與度量相關 (例如長度、溫度或區域)，請使用 [<strong>CurrenciesInUse</strong>](https://msdn.microsoft.com/library/windows/apps/br206793) 屬性來取得正確的系統度量。</p></td>
</tr>
<tr class="even">
<td align="left"><p>正確顯示文字和字型。</p></td>
<td align="left"><p>理想的字型、字型大小和文字的方向會根據不同市場而有所不同。</p>
<p>如需更多資訊，請參閱[<strong>調整配置和字型並支援 RTL</strong>](adjust-layout-and-fonts--and-support-rtl.md)。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>使用 Unicode 進行字元編碼。</p></td>
<td align="left"><p>根據預設，最新版的 Microsoft Visual Studio 會使用 Unicode 字元編碼所有文件。 如果您使用不同的編輯器，務必以適當的 Unicode 字元編碼儲存來源檔案。 所有 Windows 執行階段 API 都會傳回 UTF-16 編碼的字串。</p></td>
</tr>
<tr class="even">
<td align="left"><p>記錄輸入的語言。</p></td>
<td align="left"><p>當應用程式要求使用者輸入文字時，記錄輸入的語言。 這可確保在稍後顯示輸入時，會以正確的格式顯示給使用者。 使用 [<strong>CurrentInputMethodLanguage</strong>](https://msdn.microsoft.com/library/windows/apps/hh700658) 屬性來取得正確的輸入語言。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>不要使用語言來假設使用者的位置，也不要使用位置來假設使用者使用的語言。</p></td>
<td align="left"><p>在 Windows 中，使用者的語言和位置是不同的概念。 使用者可以使用語言的特殊地區變體 (例如，en-gb 是英國使用的英文，但是使用者可能在完全不同的國家或地區)。 考量您的 app 是否需要關於使用者語言 (例如針對 UI 文字) 或位置 (例如針對授權問題) 的知識。</p>
<p>如需更多資訊，請參閱[<strong>管理語言和地區</strong>](manage-language-and-region.md)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>請勿使用方言和隱喻。</p></td>
<td align="left"><p>特定族群 (例如，文化特性和年齡) 使用的語言，由於只有該族群使用，所以很難了解或翻譯。 同樣地，不見得人人都能理解隱喻的深層含意。 例如，只有滑雪愛好者才知道 &quot;bluebird&quot; 的其中含意，但是非滑雪愛好者，就不明白其中的含意。 如果您計畫將 App 當地語系化，並使用非正式的聲音或語調，請務必向當地語系化人員仔細解釋您真正想表達的意思和語氣。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>不要使用技術專業用語、縮寫或首字母縮略字。</p></td>
<td align="left"><p>來自其他文化特性或地區且沒有相關技術背景的使用者或人員很難了解技術專業語言，而且不容易翻譯這種語言。 一般人不會在日常交談中使用這些字詞。 技術專業語言通常會出現在錯誤訊息中，用以識別硬體和軟體問題。 有時可能需要使用這類語言，但是您應該將字串重新改寫為非技術專業用語。</p></td>
</tr>
<tr class="even">
<td align="left"><p>不要使用具有冒犯意味的影像。</p></td>
<td align="left"><p>適合您自己文化特性的影像，在其他文化特性中可能具有冒犯意味或容易產生誤解。 避免使用宗教符號、動物，或是與國旗或政治動向相關聯的色彩組合。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>避免在地圖或參照地區時引發政治爭議。</p></td>
<td align="left"><p>地圖可能包含有爭議性的地區或國家邊界，而它們經常會成為政治爭議的來源。 請注意，任何用來選取國家的 UI 都要將它表示為「國家/地區」&quot;&quot;。 在清單中將有爭議性的領土標籤標記為「國家」&quot;&quot;(像是在地址表單中)，可能會為您帶來麻煩。</p></td>
</tr>
<tr class="even">
<td align="left"><p>不要單獨使用字串比較來比較語言標記。</p></td>
<td align="left"><p>BCP-47 語言標記很複雜。 在比較語言標記時會產生許多問題，包括比對指令碼資訊、傳統標記及多個地區變體的問題。 Windows 中的資源管理系統會為您處理比對工作。 您可以使用任何語言指定一組資源，系統就會為使用者和 app 選擇適當的資源。</p>
<p>如需資源管理得更多資訊，請參閱[<strong>定義 app 資源</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>不要假設排序總是依字母順序。</p></td>
<td align="left"><p>對於不是使用拉丁文書寫體的語言，是依據像是拼音、筆劃數目及其他因素來排序。 即使是使用拉丁文書寫體的語言，也不一定會依字母順序排序。 例如，在某些文化特性中，電話簿可能不是依字母順序排序。 系統可以為您處理排序，但是如果您建立自己的排序演算法，務必將目標市場所使用的排序方法納入考量。</p></td>
</tr>
</tbody>
</table>

 

## <span id="guidelines_for_localization"></span><span id="GUIDELINES_FOR_LOCALIZATION"></span>當地語系化

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">建議</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>將 UI 字串和影像等資源與程式碼分離。</p></td>
<td align="left"><p>設計您的應用程式時，讓字串和影像等資源從程式碼分離。 這樣的設計比較容易獨立維護和當地語系化這些資源，並且可以針對不同的縮放係數、協助工具選項以及一些其他使用者與電腦內容進行自訂。</p>
<p>從應用程式的程式碼分離字串資源，以建立和語言無關的單一程式碼基底。 一律從應用程式程式碼和標記分離字串，並將它們放在資源檔案中，像是 ResW 或 ResJSON 檔案。</p>
<p>使用 Windows 的資源基礎結構，以選擇符合使用者執行階段環境的資源。</p></td>
</tr>
<tr class="even">
<td align="left"><p>隔離其他可當地語系化的資源檔案。</p></td>
<td align="left"><p>取得其他需要當地語系化的檔案，例如包含要翻譯文字的影像，或是因文化敏感度而需要變更的影像，並將它們放入以語言名稱標記的資料夾中。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>設定預設語言，並標記所有資源，即使是預設語言中的資源也一樣。</p></td>
<td align="left"><p>總是在應用程式資訊清單中為您的 app 設定適當的預設語言 (package.appxmanifest)。 預設語言決定當使用者無法使用 app 所支援的任何語言時要使用的語言。 利用他們的語言標記預設語言資源 (例如 en-us/Logo.png)，如此一來，系統便能分辨資源所使用的是哪種語言，以及該語言在特定情況中的使用情況。</p></td>
</tr>
<tr class="even">
<td align="left"><p>判斷需要當地語系化的 app 資源。</p></td>
<td align="left"><p>如果您需要將應用程式當地語系化以適應其他市場，需要變更什麼？ 需要將文字字串翻譯為其他語言。 可能需要針對其他文化特性調整影像。 考量當地語系化會對應用程式使用的其他資源 (例如，音訊或視訊) 產生何種影響。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>在程式碼和標記中使用資源識別碼以參照資源。</p></td>
<td align="left"><p>標記中不使用影像的字串常值或特定檔案名稱，改用資源的參照。 請務必針對每個資源使用唯一的識別碼。 如需詳細資訊，請參閱[<strong>如何使用限定詞命名資源</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965324)。</p>
<p>接聽當系統變更且開始使用不同組的限定詞時所引發的事件。 重新處理文件，以便載入正確的資源。</p></td>
</tr>
<tr class="even">
<td align="left"><p>讓文字大小變大。</p></td>
<td align="left"><p>動態配置文字緩衝區，因為文字大小可能會在翻譯時擴大。 如果您必須使用靜態緩衝區，請設定為特別大 (或許是英文字串長度的兩倍)，以容納翻譯字串時可能會擴大的長度。 使用者介面可以使用的空間也可能受到限制。 若要容納當地語系化的語言，請確保字串長度大約會比英文所需的長度還要長 40%。 對於非常短的字串 (例如，單一字詞)，您可能需要 300% 以上的空間。 此外，在控制項中啟用多行支援及文字換行功能，可以保留更多空間來顯示每一個字串。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>支援鏡像。</p></td>
<td align="left"><p>文字對齊和閱讀順序可以是從左至右 (例如，英文)，或從右至左 (RTL) (例如，阿拉伯文或希伯來文)。 如果您正在將產品當地語系化為使用和您自己的語言不同閱讀順序的語言，請確定 UI 元素的配置支援鏡像。 像是上一頁按鈕、UI 轉換效果及影像等項目都可能需要鏡像。</p>
<p>如需更多資訊，請參閱[<strong>調整配置和字型並支援 RTL</strong>](adjust-layout-and-fonts--and-support-rtl.md)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>註解字串。</p></td>
<td align="left"><p>確保已針對字串進行適當註解，而且只將需要翻譯的字串提供給當地語系化人員。 過度當地語系化是常見的問題來源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>使用簡短的字串。</p></td>
<td align="left"><p>較短的字串更容易翻譯，而且能夠重複使用翻譯。 重複使用翻譯可以節省金錢，因為相同的字串不需重複傳送給當地語系化人員。</p>
<p>某些當地語系化工具可能不支援長度大於 8192 個字元的字串，因此，請將字串長度保持在 4000 個字元以下。</p></td>
</tr>
<tr class="even">
<td align="left"><p>提供包含完整句子的字串。</p></td>
<td align="left"><p>提供包含完整句子的字串，而不是將一個句子分割成個別的字詞，因為字詞的翻譯會根據它們在句子中的位置而定。 此外，不要假設含有多個參數的片語在每個語言中，都會以相同的順序來保有這些參數。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>為當地語系化最佳化影像和音訊檔案。</p></td>
<td align="left"><p>避免使用影像中的文字或音訊檔案中的語音，以減少當地語系化成本。 如果您正在當地語系化為使用和您自己的語言不同閱讀方向的語言，使用對稱式影像和效果能夠更輕易地支援鏡像。</p></td>
</tr>
<tr class="even">
<td align="left"><p>不要在不同內容中重複使用字串。</p></td>
<td align="left"><p>不要在不同內容中重複使用字串，因為即使像「on」&quot;&quot;和「off」&quot;&quot;這種簡單的字都可能因內容而有不同的翻譯。</p></td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>相關文章


**範例**
* [應用程式資源和當地語系化範例](http://go.microsoft.com/fwlink/p/?linkid=254478)
* [全球化喜好設定範例](http://go.microsoft.com/fwlink/p/?linkid=231608)
 

 






<!--HONumber=Jun16_HO4-->


