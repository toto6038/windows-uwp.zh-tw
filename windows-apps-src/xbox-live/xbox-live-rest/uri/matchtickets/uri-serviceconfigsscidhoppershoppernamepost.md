---
title: POST (/serviceconfigs/{scid}/hoppers/{hoppername})
assetID: 8cbf62aa-d639-e920-1e39-099133af17f8
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamepost.html
description: " POST (/serviceconfigs/{scid}/hoppers/{hoppername})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7682b92ec61c98679904825e360d73318e9fee90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659833"
---
# <a name="post-serviceconfigsscidhoppershoppername"></a>POST (/serviceconfigs/{scid}/hoppers/{hoppername})

建立指定的比對的票證。

> [!IMPORTANT]
> 這個方法是針對與合約 103 或更新版本，搭配使用，因此需要 X Xbl-合約版本標頭項目：103 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4E5)
  * [Authorization](#ID4EJB)
  * [HTTP 狀態碼](#ID4E3C)
  * [要求本文](#ID4EFD)
  * [回應主體](#ID4E3G)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

此 HTTP/REST 方法會建立具有特定名稱的識別碼 (SCID) 層級的服務組態在 hopper 的相符項目票證。 這個方法可以前後加**Microsoft.Xbox.Services.Matchmaking.MatchmakingService.CreateMatchTicketAsync**方法。  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務組態 (SCID) 工作階段識別碼。|
| hoppername | 字串 | Hopper 名稱。 |

<a id="ID4EJB"></a>


## <a name="authorization"></a>Authorization

| 類型| 必要| 描述| 如果遺失的回應|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 特殊權限與裝置類型| 是| 當使用者的裝置類型設定為主控台時，只有在其宣告中的多人的權限的使用者可以對 「 配對 」 服務進行呼叫。 | 403|
| 裝置類型| 是| 當使用者的裝置類型不存在，或設為非主控台，到要比標題不能僅限主控台的標題。 | 403|
| 標題識別碼/證明的採購/裝置類型| 是| 會比對到標題必須允許指定的標題宣告、 裝置類型組合的配對。 | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EFD"></a>


## <a name="request-body"></a>要求本文

<a id="ID4ELD"></a>


### <a name="required-members"></a>必要的成員

| 成員| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| serviceConfig| GUID| SCID 工作階段。|
| hopperName| 字串| Hopper 的名稱。|
| giveUpDuration| 32 位元帶正負號的整數| 最長等待時間 （整數秒數）。|
| preserveSession| 列舉| 值，指出是否在工作階段會為要比對的工作階段重複使用。 可能值為 「 永遠 」 和 「 從未 」。 |
| ticketSessionRef| MultiplayerSessionReference| 工作階段中的播放程式或群組目前正在播放 MultiplayerSessionReference 物件。 |
| ticketAttributes| 物件的集合| 屬性和群組的播放程式的相關使用者所提供的值。|

<a id="ID4EXF"></a>


### <a name="prohibited-members"></a>禁止的成員

所有其他成員均不得在要求中。

<a id="ID4ECG"></a>


### <a name="sample-request"></a>範例要求

所參考的工作階段**ticketSessionRef**可以建立符合票證，並在工作階段必須包含要比對，以及其播放程式特定屬性的播放程式之前，必須建立物件。 每個玩家必須建立或加入針對 MPSD，將相關聯的相符項目屬性加入至工作階段的工作階段。 比對屬性會放在每個播放器上呼叫 matchAttrs 的自訂屬性欄位。

「 建立或加入要求提交給**https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templatename}/sessions/{sessionname}** 和可能看起來像這樣：


```cpp
{
   "members": {
     "me": {
       "constants": {
         "system": {
           "xuid": 2535285330879750
         }
      },
      "properties": {
         "custom": {
           "matchAttrs": {
             "skill": 5,
             "ageRange": "teenager"
           }
         }
      }
    }
  }
}

```


一旦建立工作階段，標題就可以呼叫配對服務，以建立該工作階段票證。


> [!NOTE] 
> 標題可以讓使用者重試此呼叫，但不是應該重試它自動資料失敗時。  



```cpp
POST /serviceconfigs/{scid}/hoppers/{hoppername}

{
  "giveUpDuration":10,
  "preserveSession": "never",
  "ticketSessionRef": {
     "scid": "ABBACDDC-0000-0000-0000-000000000001",  
     "templateName": "TestTemplate",
     "name": "5E55104-0000-0000-0000-000000000001"
  },
  "ticketAttributes": {
    "desiredMap": "Test Map",
    "desiredGameType": "Test GameType"
  }
}

```


<a id="ID4E3G"></a>


## <a name="response-body"></a>回應主體

| 成員| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ticketId| GUID| 正在建立的票證識別碼。|
| waitTime| 32 位元帶正負號的整數| 平均等候時間 hopper （整數秒數）。|


```cpp
{
  "ticketId":"0584338f-a2ff-4eb9-b167-c0e8ddecae72",
  "waitTime":60
}

```


<a id="ID4EHAAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EJAAC"></a>


##### <a name="parent"></a>父系  

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)
