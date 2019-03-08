---
title: POST (/batch)
assetID: f5997c8e-82c7-0fba-9991-ce1116db4830
permalink: en-us/docs/xboxlive/rest/uri-batchpost.html
description: " POST (/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a854fc830c87afbf675a379599916bf3db919539
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645833"
---
# <a name="post-batch"></a>POST (/batch)
POST 跨多個項目可做為多個播放程式統計資料的複雜的批次要求的 GET 方法的方法。 這些 Uri 的網域是`userstats.xboxlive.com`。
 
<a id="ID4ET"></a>

 
## <a name="remarks"></a>備註
 
標題的開發人員可以標示為開啟或限制 XDP 或合作夥伴中心的統計資料。 排行榜是開啟的統計資料。 開啟的統計資料可以存取 Smartclass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 應用程式，只要在沙箱授權的使用者。 對沙箱的使用者授權是透過 XDP 或合作夥伴中心管理。
  
  * [註解](#ID4ET)
  * [註解](#ID4EFB)
  * [Authorization](#ID4EUB)
  * [必要的要求標頭](#ID4ETC)
  * [選擇性的要求標頭](#ID4E3D)
  * [要求本文](#ID4EAF)
  * [HTTP 狀態碼](#ID4EWF)
  * [回應主體](#ID4ENBAC)
 
<a id="ID4EFB"></a>

 
## <a name="remarks"></a>備註
 
呼叫端會提供使用者、 服務組態識別碼 (SCIDs) 和一份每 SCIDs 要擷取這些統計資料的統計資料名稱的陣列中的訊息內文。
 
您可能會發現它更有用檢閱簡單、 單一統計資料[取得](uri-usersxuidscidsscidstatsget.md)閱讀本頁的更複雜，批次模式之前的方法。
  
<a id="ID4EUB"></a>

 
## <a name="authorization"></a>Authorization
 
沒有內容隔離和存取控制案例實作授權邏輯。
 
   * 排行榜和使用者的統計資料可以讀取來自任何平台上的用戶端，前提是呼叫者送出要求的有效 XSTS 語彙基元。 寫入會很明顯地限制為支援的用戶端。
   * 標題的開發人員可以標示為開啟或限制 XDP 或合作夥伴中心的統計資料。 排行榜是開啟的統計資料。 開啟的統計資料可以存取 Smartclass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 應用程式，只要在沙箱授權的使用者。 對沙箱的使用者授權是透過 XDP 或合作夥伴中心管理。
  
下列範例會檢查虛擬程式碼：
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4ETC"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。| 
  
<a id="ID4E3D"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 名稱/的組建編號應該導向此要求的服務。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>要求本文
 
<a id="ID4EIF"></a>

 
### <a name="sample-request"></a>範例要求
 
下列的張貼內容會通知服務，從兩個不同的 SCIDs 為兩個不同的使用者要求四個統計資料。
 

```cpp
{    
"requestedusers": [
                1234567890123460,
                1234567890123234
            ],
            "requestedscids": [
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c1212",
                    "requestedstats": [
                        "Test4FirefightKills",
                        "Test4FirefightHeadshots"
                    ]
                },
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c0343",
                    "requestedstats": [
                        "OverallTestKills",
                        "TestHeadshots"
                    ]
                }
            ] 
}
      
```

   
<a id="ID4EWF"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 已成功擷取工作階段。| 
| 304| 未修改| 資源尚未經過修改自上次要求。| 
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 403| 已禁止| 要求不允許使用者或服務。| 
| 404| 找不到| 找不到指定的資源。| 
| 406| 無法接受| 不支援資源版本。| 
| 408| 要求逾時| 不支援資源版本;應該會遭到拒絕，由 MVC 層。| 
  
<a id="ID4ENBAC"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4EXBAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{    
   "users":[          
       {    
      "xuid": "123456789"
        "gamertag": "WarriorSaint",
        "scids":[
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c1212",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":7
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":4
             }]
                        },
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c0343",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":3434
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":41
             }]
          }],
                   },
    {    
                   "gamertag":"TigerShark",
                   "xuid":1234567890123234
        "scids":[
          {
             "scid":"123456789",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":63
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":12
             }]
                        },
          {
"scid":"987654321",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":375
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":34
             }]
          }],
                   }]
}
         
```

   
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>父系 

[/batch](uri-batch.md)

   