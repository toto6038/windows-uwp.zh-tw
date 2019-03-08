---
title: EDS 參數
assetID: 9475b427-53bc-697b-6d24-1787320260b7
permalink: en-us/docs/xboxlive/rest/edsparameters.html
description: " EDS 參數"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5412bcebc73dfd25d81b2073527e64e81de1bba4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655413"
---
# <a name="eds-parameters"></a>EDS 參數

<a id="ID4EO"></a>


## <a name="query-string-parameters"></a>查詢字串參數

這些查詢參數不一定是接受所有[娛樂探索服務 (EDS) Api](../uri/marketplace/atoc-reference-marketplace.md)，但所有接受一個以上的 API。

| 參數| 類型| 描述|
| --- | --- | --- |
| combinedContentRating| 字串| 選用。 請參閱[取得 (/media/ {marketplaceId} / contentRating)](../uri/marketplace/uri-medialocalecontentratingget.md)。|
| continuationToken| 字串| 選用。 接續 token 是不透明的 blob，其中包含服務需要重新編頁，在某些情況下進行的資訊。 如果省略此值，結果的第一頁會傳回 （其中頁面大小取決於 maxItems 參數），以及可用來取得結果的第二個頁面的接續權杖。 第二個頁面會包含第三個頁面的結果，接續 token 等等。|
| 所需| 字串| 選用。 請參閱 < 結合的欄位名稱 API。|
| desiredMediaItemTypes| 字串陣列| 選用。 這個參數會決定要在回應中傳回的項目類型。|
| 網域| 字串| 選用。 Domain 參數會決定的遊戲和應用程式的 「 marketplace 」 內容中呼叫的用戶端。 預設的網域會是 「 現代化 」，指出用戶端只可尋求 Xbox One 的內容。 如果用戶端想要切換至 介面的 Xbox 360 主機內容的網域，它必須指定網域為"Xbox360 」。 目前，跨網域不支援結果。 可能值為： <ul><li>Xbox360</li><li>現代化</li></ul> SingleMediaGroupSearch 只支援 「 Xbox360"值。 支援的瀏覽和詳細資料。 crossMediaGroupSearch 不支援，且會傳回 400 錯誤。|
| 欄位| 字串| 選用。 請參閱[取得 (/media/ {marketplaceId} / 欄位)](../uri/marketplace/uri-medialocalefieldsget.md)。|
| firstPartyOnly| 布林值| 選擇性的篩選參數。 決定是否只會將第一方的內容會傳回或是否兩個第一方和第三方內容會從查詢傳回。 |
| freeOnly| 布林值| 選擇性的篩選參數。 會限制僅使內容的結果。|
| GroupBy| TK| GroupBy 參數用來協助分類成群組，結果集，而不是單一結果集。 指定此參數，將會修改結果集，以傳回多個項目清單，其中每個貯體中的項目數由 maxItems 參數。 <ul><li>MediaGroup-結果會依 MediaGroup。</li></ul> |
| hasTrailer| 布林值| 選擇性的篩選參數。 決定是否傳回的項目必須包含結尾，或如果具有結尾是選擇性的。 如果值為 true，所有項目必須有結尾。|
| id| 字串| 選用。 如果提供，會限制為僅具有指定 ID 的項目子系的結果 如果提供這個參數，則也必須指定 MediaItemType。 |
| 識別碼| 字串陣列| 必要。 所有將傳回詳細資料 （最多 10 個） 識別碼。 注意任何識別碼，包含將放在不合法的字元 （ProviderContentId 類型識別碼是正常的完整 Url 本身提供，因此包含不合法的字元） 的 URL 必須以 URL 編碼的正確傳送至 EDS。|
| idType| 字串| 選用。 識別碼為 id 參數傳入的型別。 有效值包括： <ul><li>標準 (Bing/Marketplace)</li><li>XboxHexTitle （播放在主控台上的應用程式）</li></ul>  所有提供識別碼必須共用相同的識別碼。 如果省略此值，則所有識別碼，將都假設為 Canonical。|
| latestOnly| 布林值| 選擇性的篩選參數。 將結果限制為只使用最新的發行日期。|
| maxItems| 32 位元帶正負號的整數| 選用。 決定應從呼叫傳回的項目數目上限。 有效的值是介於 1 到 25 日 （含) 之間的數字。 如果省略參數的預設值為 25。|
| mediaGroup| 字串| 選用。 媒體的群組識別碼。 所有提供識別碼必須共用相同媒體的群組。|
| MediaItemType| 字串| 選用。 Id 參數中指定 ID 的項目類型。 如果提供的 id 參數，則也必須指定此參數。|
| orderBy| 字串| 必要。 OrderBy 參數會決定應該如何排序傳回的項目。 此欄位的一般值如下，但某些 Api 可能支援其他的值。<ul><li>playCountDaily-次數所播放媒體，最新的一天。</li><li>freeAndPaidCountDaily-依計數的免費和付費購買項目，最新的一天。</li><li>paidCountAllTime-由唯一的付費購買，任何時候的次數。</li><li>paidCountDaily-計數只付費購買項目，最新的一天。</li><li>digitalReleaseDate-下載可用的日期。</li><li>releaseDate-依據提供的日期，以存放區，將回到數位發行日期 （如果有的話）。</li><li>userRatings-依平均使用者評等。</li></ul> |
| preferredProvider| 字串| 選用。 如果使用者具有慣用內容提供者，例如 Comcast Xfinity 或 Verizon FIOS 可以傳遞該提供者的識別碼中。 雖然實際的項目順序不會變更，請針對每個項目，指定提供者的資訊將顯示在提供者的清單頂端，（如果慣用的內容提供者有可用的項目）。|
| q| 字串| 必要。 查詢用於搜尋的字詞。|
| queryRefiners| 字串陣列| 選用。 請參閱清單[EDS 查詢 Refiners](edsqueryrefiners.md)。|
| 關聯性| 字串| 選用。 會使用 ID 參數做為基底來搜尋符合指定的關聯性類型的其他產品的篩選器： <ul><li>bundledWith-尋找套件組合產品的 ID 參數的這些套組的一部分。</li><li>bundledProducts-尋找 ID 參數所指定套件組合中包含的產品。</li></ul>  使用此參數，將會傳回只會顯示在 marketplace （可傳回在瀏覽呼叫中） 的產品。 如果組合有隱藏的產品，它仍然是套件組合的一部分，但不是會傳回這些結果中。|
  | ScopeId | 字串 | 這個參數用於反向對應案例中，視訊媒體。 |
  | ScopeIdType | 字串 | 這個參數用於反向對應案例中，視訊媒體。 可能的值：標題。 |
  | skipItems | 32 位元帶正負號的整數 | 選用。 在非跨群組案例中的分頁，skipItems 參數用來判斷已查看多少個項目 （以及因此何種項目應該會顯示第一次在結果集）。 值是以 0 為基礎，因此 skipItems = 0 （或只不提供 skipItems） 開始擷取清單開頭。 skipItems = 3 會略過清單中的前三個項目，並開始擷取與第四個項目。 |
  | subscriptionLevel | 字串陣列 | 選擇性的篩選參數。 SubscriptionLevel 參數判斷類型的訂用帳戶的使用者具有 （例如使用者是否具備付費的訂用帳戶或免費的訂用帳戶）。 可能的值如下所示。 <ul><li>金級：使用者已付費的訂用帳戶</li><li>銀級：使用者有可用的訂用帳戶。</li></ul>  |
| TargetDevices| 字串| EDS 提供的彈性來篩選提供的目標裝置。 供應項目 （ProviderContent 或可用性） 傳回的項目可以限制為目標裝置。|
| topRatedOnly| 布林值| 選擇性的篩選參數。 將結果限制為只有自動評等的內容。|

<a id="ID4EGEAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EIEAC"></a>


### <a name="parent"></a>父系

[其他參考](atoc-xboxlivews-reference-additional.md)


<a id="ID4EUEAC"></a>


### <a name="further-information"></a>進一步的資訊

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)
