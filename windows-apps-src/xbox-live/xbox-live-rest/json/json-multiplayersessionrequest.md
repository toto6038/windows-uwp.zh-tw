---
title: MultiplayerSessionRequest (JSON)
assetID: 2e311c6d-3427-5a39-1989-06dc08483057
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionrequest.html
description: " MultiplayerSessionRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 18929c060adeae47f0305422dd312e7410f93981
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628343"
---
# <a name="multiplayersessionrequest-json"></a>MultiplayerSessionRequest (JSON)
要求的 JSON 物件上作業所傳遞**MultiplayerSession**物件。 
<a id="ID4EQ"></a>

  
 
MultiplayerSessionRequest JSON 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| 常數| 物件| 使用工作階段範本，以產生工作階段的常數會合併的唯讀設定。 | 
| 內容 | 物件 | 要合併到工作階段屬性的變更。| 
| members.me | 物件| 常數和屬性的使用方式，就像其最上層的對應項目。 任何 PUT 方法要求使用者必須是成員的工作階段中，並將使用者新增，如有必要。 如果"me"指定為 null 時，提出要求的成員會移除從工作階段。 | 
| 成員 | 物件| 其他種物件，表示使用者新增至工作階段，做為索引鍵的以零為起始的索引。 在要求中的成員數目一律 0 開始，即使工作階段已經包含成員。 成員會加入至工作階段以其出現在要求中的順序。 對其所屬的使用者可以只設定成員屬性。 | 
| 伺服器 | 物件| 值，表示更新和工作階段的新增項目集的相關聯的伺服器參與者。 如果在伺服器指定為 null 時，工作階段移除該伺服器項目。 | 
  
<a id="ID4EZ"></a>

 
## <a name="request-structure"></a>要求結構
 

```json
{
  "constants": { /* Property Bag */ },
  "properties": { /* Property Bag */ },
  "members": {
    // Requires a service principal. Existing members can be deleted by index.
    // Not available on large sessions.
    "5": null,

    // Reservation requests must start with zero. New users will get added in order to the end of the session's member list.
    // Large sessions don't support reservations.
    "reserve_0": {
      "constants": { /* Property Bag */ }
    },
    "reserve_1": {
      "constants": { /* Property Bag */ }
    },

    // Requires a user principal with a xuid claim. Can be 'null' to remove myself from the session.
    "me": {
      "constants": { /* Property Bag */ },
      "properties": { /* Property Bag */ },
    }
  },

  // Requires a server principal.
  "servers": {
    // Can be any name. The value can be 'null' to remove the server from the session.
    "name": {
      "constants": {  /* Property Bag */ },
      "properties": {  /* Property Bag */ }
    }
  }
}
```

  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EMB"></a>

 
##### <a name="reference"></a>參考 

[MultiplayerSession (JSON)](json-multiplayersession.md)

 [PUT (/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} /sessions/ {sessionName})](../uri/sessiondirectory/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

   