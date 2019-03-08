---
title: PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: e3e4f164-ac5e-cbd9-8c05-2e1ac00dc55e
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.html
description: " PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d35b3f89f8b866a5236e8f5ac91eb37d9a82d306
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598553"
---
# <a name="put-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
建立、 更新或加入工作階段。

> [!IMPORTANT]
> 這個 URI 方法需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4EYB)
  * [HTTP 狀態碼](#ID4EFC)
  * [要求本文](#ID4EOC)
  * [回應主體](#ID4E4C)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

此 HTTP/REST 方法會建立聯結，或更新的工作階段，根據相同的 JSON 要求主體範本的哪些子集會傳送。 成功時，它會傳回**MultiplayerSession**包含回應從伺服器傳回的物件。 在它的屬性可能會不同於在傳入的屬性**MultiplayerSession**物件。 這個方法可以前後加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionAsync**。

工作階段建立和更新作業會使用 PUT 和 application/json 主體，代表要套用的變更。 作業具有等冪性，也就是多個應用程式相同的變更會有任何額外的作用。

JSON 要求主體鏡像工作階段資料結構。 所有欄位和子欄位都是選擇性的。

PUT 方法的工作階段建立或加入模式的 wire 格式如下所示。

> [!NOTE]
> 請謹慎使用此模式。 Upates 套用盲目地，不論目前的工作階段狀態。



```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



PUT 方法的工作階段更新模式的 wire 格式如下所示。

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



PUT 方法來更新工作階段屬性的電傳格式如下所示。 它相當於工作階段 URI 的 PUT 作業具有以下物件只讓屬性成為主體。 差異在於，這項作業會傳回錯誤碼 404 找不到如果工作階段不存在。 此作業可支援 If-match 標頭。

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001/properties HTTP/1.1
         Content-Type: application/json

         { "system": { }, "custom": { } }

```



<a id="ID4EYB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- | --- |
| scid| GUID| 服務設定識別項 (SCID)。 工作階段識別項的第 1 部分。|
| sessionTemplateName| 字串| 工作階段範本的目前執行個體的名稱。 第 2 部分的工作階段識別碼。|
| sessionName| GUID| 工作階段的唯一識別碼。 第 3 部分的工作階段識別碼。|

<a id="ID4EFC"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EOC"></a>


## <a name="request-body"></a>要求本文

以下是建立或加入工作階段的範例要求主體。 下列要求主體的成員是選擇性的。 在要求中禁止使用所有其他可能的成員。

| 成員| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- |
| 常數| 物件| 使用工作階段範本，以產生工作階段的常數會合併的唯讀設定。 |
| 內容 | 物件 | 要合併到工作階段屬性的變更。|
| members.me | 物件| 常數和屬性的使用方式，就像其最上層的對應項目。 任何 PUT 方法要求使用者必須是成員的工作階段中，並將使用者新增，如有必要。 如果"me"指定為 null 時，提出要求的成員會移除從工作階段。 |
| 成員 | 物件| 其他種物件，表示使用者新增至工作階段，做為索引鍵的以零為起始的索引。 在要求中的成員數目一律 0 開始，即使工作階段已經包含成員。 成員會加入至工作階段以其出現在要求中的順序。 對其所屬的使用者可以只設定成員屬性。 |
| 伺服器 | 物件| 值，表示更新和工作階段的新增項目集的相關聯的伺服器參與者。 如果在伺服器指定為 null 時，工作階段移除該伺服器項目。 |



```cpp
{
  "properties": {
    "custom": {"KANWE": "MGMSY"},
    "system": {}
  },
  "constants": {
    "custom": {},
    "system": {"visibility": "open"}
  },
  "members": {
    "reserve_0": {
    "constants": {
      "custom": {"type": "leader"},
      "system": {"xuid": "5500461"} }}
   }
}

```


<a id="ID4E4C"></a>


## <a name="response-body"></a>回應主體

建立或加入工作階段的範例回應主體：


```cpp
{
  "contractVersion": 104,
  "correlationId": "0FE81338-EE96-46E3-A3B5-2DBBD6C41C3B",
  "nextTimer": "2009-06-15T13:45:30.0900000Z",

  "initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
  },

  "hostCandidates": [ "ab90a362", "99582e67" ],

  "constants": {
    "system": {"visibility": "open"},
    "custom": {}
  },

  "properties": {
     "system": { "turn": [] },
     "custom": { "myProperty": "myValue" }
  },

  "members": {
      "1": {
        "properties": {
        "system": { },
        "custom": { }
      },

      "constants": {
        "system": { "xuid": "5500461" },
        "custom": { }
      }

      "gamertag": "stacy",
      "deviceToken": "9f4032ba7",
      "reserved": true,
      "activeTitleId": "8397267",
      "joinTime": "2009-06-15T13:45:30.0900000Z",
      "turn": true,
      "initializationFailure": "latency",
      "initializationEpisode": 1,
      "next": 4
    },
  },

  "membersInfo": {
      "first": 1,
      "next": 4,
      "count": 1,
      "accepted": 0
  },

  "servers": {
      "name": {
        "constants": { },
        "properties": { }
      }
  }
}

```


<a id="ID4EID"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EKD"></a>


##### <a name="parent"></a>父系

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
