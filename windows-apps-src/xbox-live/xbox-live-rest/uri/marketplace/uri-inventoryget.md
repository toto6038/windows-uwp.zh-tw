---
title: GET (/users/me/inventory)
assetID: 7b74dd08-2854-319d-3ed0-ddee75d922b9
permalink: en-us/docs/xboxlive/rest/uri-inventoryget.html
description: " GET (/users/me/inventory)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 31c787108fad84f06b02ded3958f9d2f99727cbe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632203"
---
# <a name="get-usersmeinventory"></a>GET (/users/me/inventory)
提供一組的目前呼叫端提供的使用者相關聯的清查。
這些 Uri 的網域是`inventory.xboxlive.com`。

  * [註解](#ID4EV)
  * [查詢字串參數](#ID4EHB)
  * [範例要求](#ID4EDE)
  * [回應主體](#ID4ERE)

<a id="ID4EV"></a>


## <a name="remarks"></a>備註

沒有原則檢查，強制執行，或篩選將會發生這個呼叫的一部分。 呼叫端可以選擇將查詢參數傳遞以縮小傳回之結果的範圍。

呼叫端可以分頁瀏覽使用接續 token 的結果為前一個回應透過提供： **/users/me/inventory？ continuationToken = continuationTokenString**。

呼叫端可能進行詳細資料的特定項目 url 的 API 呼叫，若要查看特定項目的相關資訊。

<a id="ID4EHB"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 參數| 類型| 描述|
| --- | --- | --- |
| 可用性| 字串| 要傳回的項目目前的可用性。 預設值為 「 可用 」 目前的日期介於開始日期和結束之間的傳回項目日期範圍。 其他值包括"All"，就會傳回所有的項目和 「 無法使用 」，它會傳回項目，為其目前的日期落在外部開始日期和結束日期範圍，因此目前無法使用它。 |
| 容器| 字串| 選用。 如果您的遊戲的產品識別碼設定值，然後從清查結果僅包括有關該遊戲中的項目。 從您的伺服器，以篩選到特定的遊戲產品的結果呼叫清查時，這是特別有用。|
| expandSatisfyingEntitlements| 字串| 指出如果回應包含使用者在結果中傳回的所有滿足權利的旗標。 預設值為"false"。 使用此參數值為"true"，會授與使用者透過滿足權利，例如包裝在一起的項目，移轉到 Xbox One，訂用帳戶權益的 Xbox 360 購買任何產品時的結果會新增等等。 此值為"false"時則會傳回結果並不會個別的包含項目中只套組的 ProductID 例如父項目。 **注意：** 使用此參數值為"true"時才支援 itemType 參數不包含在 URI 中，否則您會收到 HTTP 400 錯誤。 |  
  | productIds | 字串 |  您想要從使用者的清查，特別是擷取的 Productid 的集合，並以分隔 '、'。  如果使用者在他們的清查結果中沒有提供的 ProductID，該項目不會出現在結果中從 API 呼叫。 如果為 true 傳入的組合以及 expandSatisfyingEntitlements 參數集 productID，組合中所包含的所有項目會傳回呼叫結果 （不論，您可以指定在查詢字串中的其 Productid）。   |
  | 狀態 | 字串 | 要傳回之項目的狀態。 預設值為"all"，它會傳回所有項目。 其他值處於"Enabled"、 表示已啟用該唯一 itemsthat 應該傳回，「 已擱置 」，表示只會擱置的項目應該會傳回，「 已過期 」，這表示應傳回的已過期的項目，」取消"，指出應該傳回針對已取消項目，且"Renewed"，表示應傳回的只有已更新的項目。  |

除此之外，資源可支援標準的分頁機制。

<a id="ID4EDE"></a>


## <a name="sample-request"></a>範例要求

這個 URI 方法的完整網域名稱是 `https://inventory.xboxlive.com/users/me/inventory.
         `

> [!NOTE] 
> 哪些使用者會被視為取決於權杖提供，其中可能包含多個使用者。 若要讓單一使用者的清查，您也必須在您想要以獨佔方式考慮特定使用者，提供使用者雜湊。

.

<a id="ID4ERE"></a>


## <a name="response-body"></a>回應主體

如果呼叫成功，服務就會傳回清查項目的陣列。 請參閱[inventoryItem (JSON)](../../json/json-inventoryitem.md)。

<a id="ID4E4E"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
  "pagingInfo": {
    "continuationToken": string,
    "totalItems": int
  },
  "items":
  {
    "url": string,
    "itemType": "Music",
    "titleId": string,
    "containers": string,
    "obtained": DateTime,
    "startDate": DateTime,
    "endDate": DateTime,
    "state": "Enabled"  
}

```


<a id="ID4EHF"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EJF"></a>


##### <a name="parent"></a>父系

[/users/me/inventory](uri-inventory.md)


<a id="ID4ETF"></a>


##### <a name="further-information"></a>進一步的資訊

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)
