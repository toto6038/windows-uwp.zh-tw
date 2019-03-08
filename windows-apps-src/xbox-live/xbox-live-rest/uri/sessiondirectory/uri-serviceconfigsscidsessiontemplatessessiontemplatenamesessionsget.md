---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)
assetID: 9daac964-0b25-3430-fcfd-0f8658aceee1
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionsget.html
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1020c9d9c378a95070a7b0bf3faeb9d2c6751d51
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627473"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatenamesessions"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)
擷取工作階段範本文件。

> [!IMPORTANT]
> 這個 URI 方法需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4EKB)
  * [HTTP 狀態碼](#ID4EXB)
  * [要求本文](#ID4EAC)
  * [回應主體](#ID4EKC)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

此 HTTP/REST 方法會擷取工作階段提供篩選的範本資訊。 這個方法可以前後加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsAsync**。


> [!NOTE] 
> 如 2015年多人遊戲，呼叫這個方法<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync</b>。  



> [!NOTE] 
> 每次呼叫此方法必須包含關鍵字、 Xbox 使用者識別碼篩選器中，或兩者。 如果呼叫端沒有正確權限<i>私人</i>並<i>保留</i>參數，方法會傳回錯誤碼 403 禁止，是否實際存在的任何這類工作階段。  


<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- | --- | --- |
| scid| GUID| 服務設定識別項 (SCID)。 第 1 部分的工作階段識別碼。|
| 關鍵字| 字串| 關鍵字，可用來篩選到只以該字串識別的工作階段的結果。|
| xuid| GUID| Xbox 使用者要擷取工作階段的使用者識別碼。 使用者必須是作用中的工作階段中。 |
| 保留項目| 字串| 值，指出如果工作階段的清單中包含的使用者則未接受。 這個參數只能設定為 true。 此設定需要呼叫者必須具備伺服器層級存取工作階段中，或呼叫端的 XUID 宣告以符合 Xbox 使用者識別碼篩選器。 |
| 非使用中| 字串| 值，指出工作階段的清單包含這些使用者已接受，但不主動播放。 這個參數只能設定為 true。 |
| 私用| 字串| 值，指出是否工作階段的清單包含私用的工作階段。 這個參數只能設定為 true。 只有當查詢自己的工作階段，或是查詢伺服器對伺服器時，會是有效的。 將此參數設定為 true 呼叫者必須具備伺服器層級存取工作階段中，或呼叫端的 XUID 宣告以符合 Xbox 使用者識別碼篩選器。 |
| 可見度| 字串| 列舉值，指出用於篩選結果的可見性狀態。 目前這個參數可以只會設定為開啟包含開啟的工作階段。 請參閱<b>MultiplayerSessionVisibility</b>。 |
| version| 字串| 正整數，表示主要的工作階段的版本或較低的工作階段加入。 值必須是小於或等於要求的合約版本 100 模數。 |
| Take| 字串| 正整數，指出工作階段數目上限來擷取。|

<a id="ID4EXB"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EAC"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4EKC"></a>


## <a name="response-body"></a>回應主體

從這個方法傳回為工作階段的參考，包含某些工作階段資料包含內嵌的 JSON 陣列。


```cpp
{
    "results": [ {
            "xuid": "9876",    // If the session was found from a xuid, that xuid.
            "startTime": "2009-06-15T13:45:30.0900000Z",
            "sessionRef": {
                "scid": "foo",
                "templateName": "bar",
                "name": "session-seven"
            },
            "accepted": 4,    // Approximate number of non-reserved members.
            "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
            "visibility": "open",    // or "private", "visible", or "full"
            "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
            "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
            "keywords": [ "one", "two" ]
        }
    ]
}

```


<a id="ID4EUC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EWC"></a>


##### <a name="parent"></a>父系

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)
