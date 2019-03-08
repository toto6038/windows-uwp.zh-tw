---
title: GET (/users/xuid({xuid})/achievements)
assetID: 381d49d1-7a4b-4a1e-1baf-cf674f7e0d54
permalink: en-us/docs/xboxlive/rest/uri-achievementsusersxuidachievementsgetv2.html
description: " GET (/users/xuid({xuid})/achievements)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 720170e88808db7ef95b88896fbca4f1cda4a091
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655263"
---
# <a name="get-usersxuidxuidachievements"></a>GET (/users/xuid({xuid})/achievements)
取得成果上標題、 解除鎖定的使用者，或有進行中的使用者定義的清單。 這些 Uri 的網域是`achievements.xboxlive.com`。
 
  * [URI 參數](#ID4EX)
  * [查詢字串參數](#ID4ECB)
  * [Authorization](#ID4ENF)
  * [必要的要求標頭](#ID4ESG)
  * [選擇性的要求標頭](#ID4ESH)
  * [要求本文](#ID4EIBAC)
  * [回應主體](#ID4ETBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 正在存取的 （資源） 的使用者。 必須符合 XUID 的已驗證的使用者。| 
  
<a id="ID4ECB"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
| 參數| 必要| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| <b>skipItems</b>| 否| 32 位元帶正負號的整數| 傳回指定數目的項目之後開始的項目。 例如， <b>skipItems ="3"</b>會擷取項目開頭的第四個項目擷取。 | 
| <b>continuationToken</b>| 否| 字串| 傳回項目，始於指定的接續 token。 | 
| <b>maxItems</b>| 否| 32 位元帶正負號的整數| 若要從集合中，可以結合傳回的項目數目上限<b>skipItems</b>並<b>continuationToken</b>要傳回的項目範圍。 服務可能會提供預設值，如果<b>maxItems</b>不存在，而且可能會傳回少於<b>maxItems</b>，即使尚未傳回結果的最後一頁。 | 
| <b>titleId</b>| 否| 字串| 傳回的結果篩選條件。 接受一或多個以逗號分隔、 decimal 標題的識別碼。| 
| <b>unlockedOnly</b>| 否| 布林值| 篩選傳回的結果。 如果設定為<b>，則為 true</b>，將只傳回使用者解除鎖定的成就。 預設值為<b>false</b>。| 
| <b>possibleOnly</b>| 否| 布林值| 篩選傳回的結果。 如果設定為<b>，則為 true</b>，會傳回所有可能的結果，但不是解除鎖定中繼資料-只從 XMS 的成就資訊。 預設值為<b>false</b>。| 
| <b>types</b>| 否| 字串| 傳回的結果篩選條件。 可以是"Persistent"或 「 挑戰 」。 預設值為所有支援的類型。| 
| <b>orderBy</b>| 否| 字串| 指定要傳回結果的順序。 可以是 「 排序 」、"Title"、"UnlockTime 」 或 「 EndingSoon"。 預設值是 「 未排序的 」。| 
  
<a id="ID4ENF"></a>

 
## <a name="authorization"></a>Authorization
 
| 宣告| 必要？| 描述| 如果遺失的行為| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 使用者| 呼叫端是授權的 Xbox LIVE 使用者。| 呼叫端必須是有效的使用者，在 Xbox LIVE。| 403 禁止 」| 
  
<a id="ID4ESG"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。| 
  
<a id="ID4ESH"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。預設值：1.| 
| <b>x-xbl-contract-version</b>| 32 位元不帶正負號的整數| 如果有的話，設定為 2，此 API 的 V2 版本將會使用。 否則，V1。| 
| <b>Accept-Language</b>| 字串| 所需的地區設定和後援 （例如，若為 FR-FR、 fr、 EN-GB、 en-us WW、 EN-US） 的清單。 成就服務會一起完成清單中，直到找到相符的當地語系化的字串。 如果找不到，它會嘗試比對來自使用者的 IP 位址的使用者語彙基元中定義的位置。 如果找不到仍然沒有相符的當地語系化的字串，它會使用與標題的開發人員/發行者所提供的預設字串。 | 
  
<a id="ID4EIBAC"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4ETBAC"></a>

 
## <a name="response-body"></a>回應主體
 
如果呼叫成功，服務會傳回的陣列[成就 (JSON)](../../json/json-achievementv2.md)物件並[PagingInfo (JSON)](../../json/json-paginginfo.md)物件。
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
    "achievements":
    [{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
                "achievementState":"Achieved",
                "requirements":null,
                "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
        }],
        "pagingInfo":
        {
                "continuationToken":null,
                "totalRecords":1
        }
}
         
```

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>父系 

[/users/xuid({xuid})/achievements](uri-achievementsusersxuidachievementsv2.md)

  
<a id="ID4E2CAC"></a>

 
##### <a name="reference"></a>參考 

[成就 (JSON)](../../json/json-achievementv2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [分頁參數](../../additional/pagingparameters.md)

   