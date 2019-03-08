---
title: GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
assetID: 27318886-f084-d6a8-e582-3eb070ccbc38
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementidget.html
description: " GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 19083937d48d8c312215f1734513d83c59f52f0d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627503"
---
# <a name="get-usersxuidxuidachievementsscidachievementid"></a>GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
取得成就的詳細資料。 這些 Uri 的網域是`achievements.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
  * [Authorization](#ID4EAB)
  * [在資源上的隱私權設定的效果](#ID4E4C)
  * [必要的要求標頭](#ID4EPG)
  * [選擇性的要求標頭](#ID4EPH)
  * [要求本文](#ID4ECBAC)
  * [HTTP 狀態碼](#ID4ENBAC)
  * [回應主體](#ID4EBGAC)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的使用者正在存取的資源。 必須符合 XUID 的已驗證的使用者。| 
| scid| GUID| 其成就正在存取的服務組態的唯一識別碼。| 
| achievementid| 32 位元不帶正負號的整數| 正在存取的成就 （在指定的 SCID) 的唯一識別碼。| 
  
<a id="ID4EAB"></a>

 
## <a name="authorization"></a>Authorization
 
使用授權宣告 | 宣告| 必要？| 描述| 如果遺失的行為| 
| --- | --- | --- | --- | --- | --- | --- | 
| 使用者| 是| 在 Xbox LIVE 所提出的要求是有效的使用者。| 403 禁止 」| 
| 標題| 否| 呼叫端的標題。| AuthZ 而定。 自 2013 年 5 月 1 日起 AuthZ 所提供的宣告時遺失，且將會因此在沒有標記為 public 任何 SCIDs 來拒絕存取。| 
| 沙箱| 否| 沙箱應該從中擷取結果。| AuthZ 而定。 自 2013 年 5 月 1 日起 AuthZ 未提供的預設宣告遺漏時。| 
  
<a id="ID4E4C"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果
 
在資源上的隱私權設定的效果 | 提出要求的使用者| 目標使用者的隱私權設定| 行為| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 我| -| 如所述。| 
| friend| 每個人| 確定| 
| friend| 只有朋友| 確定| 
| friend| 封鎖| 禁止。| 
| 非 friend 使用者| 每個人| 確定| 
| 非 friend 使用者| 只有朋友| 禁止。| 
| 非 friend 使用者| 封鎖| 禁止。| 
| 協力廠商網站| 每個人| 確定| 
| 協力廠商網站| 只有朋友| 禁止。| 
| 協力廠商網站| 封鎖| 禁止。| 
  
<a id="ID4EPG"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。| 
  
<a id="ID4EPH"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.| 
| x-xbl-contract-version| 字串| 預設為 V1。| 
| Accept-Language| 字串| 所需的地區設定和後援 （例如，若為 FR-FR、 fr、 EN-GB、 en-us WW、 EN-US） 的清單。 成就服務會一起完成清單中，直到找到相符的當地語系化的字串。 如果找不到，它會嘗試比對來自使用者的 IP 位址的使用者語彙基元中定義的位置。 如果找不到仍然沒有相符的當地語系化的字串，它會使用與標題的開發人員/發行者所提供的預設字串。 | 
  
<a id="ID4ECBAC"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4ENBAC"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 已成功擷取工作階段。| 
| 301| 已永久移動| 服務已移動至不同的 URI。| 
| 307| 暫時重新導向| 已暫時變更此資源的 URI。| 
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 403| 已禁止| 要求不允許使用者或服務。| 
| 404| 找不到| 找不到指定的資源。| 
| 406| 無法接受| 不支援資源版本;應該會遭到拒絕，由 MVC 層。| 
| 408| 要求逾時| 該要求花了太多時間完成。| 
| 410| 不存在| 無法再使用所要求的資源。| 
  
<a id="ID4EBGAC"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4EHGAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
    "achievement":
    {
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
    }
}
         
```

   
<a id="ID4ERGAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ETGAC"></a>

 
##### <a name="parent"></a>父系 

[/users/xuid({xuid})/achievements/{scid}/{achievementid}](uri-usersxuidachievementsscidachievementid.md)

   