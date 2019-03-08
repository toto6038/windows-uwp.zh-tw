---
title: MultiplayerActivityDetails (JSON)
assetID: f982aa5e-2694-4ef9-bc55-6c099a3cf9ec
permalink: en-us/docs/xboxlive/rest/json-multiplayeractivitydetails.html
description: " MultiplayerActivityDetails (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 188bcebb8d6bff879f30dcc83d7039fbcbfae0b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658103"
---
# <a name="multiplayeractivitydetails-json"></a>MultiplayerActivityDetails (JSON)
JSON 物件，表示**Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**。 

> [!NOTE] 
> 此物件藉由 2015年多人遊戲，並只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本搭配使用。  

 
<a id="ID4ES"></a>

  
 
MultiplayerActivityDetails JSON 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | --- | 
| SessionReference| MultiplayerSessionReference| A <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference</b>物件，表示工作階段的識別資訊。| 
| HandleId| 64 位元不帶正負號的整數| 對應至活動的控制代碼識別碼。| 
| TitleId| 32 位元不帶正負號的整數| 標題識別碼應該啟動，才能加入活動。| 
| 可見度| MultiplayerSessionVisibility| A <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b>值，指出工作階段的可見性狀態。| 
| JoinRestriction| MultiplayerSessionJoinRestriction| A <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionJoinRestriction</b>值，指出工作階段的結合限制。 這項限制適用於 [可見] 欄位設定為 「 開啟 」。| 
| 關閉| 布林值| 工作階段暫時關閉的聯結，則為 false 否則，其值為 true。| 
| OwnerXboxUserId| 64 位元不帶正負號的整數| Xbox 使用者擁有活動的成員識別碼。| 
| MaxMembersCount| 32 位元不帶正負號的整數| 總計的插槽數目。| 
| MembersCount| 32 位元不帶正負號的整數| 佔用的位置數目。| 
  
<a id="ID4E3D"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
  "results": [{
    "id": "11111111-ebe0-42da-885f-033860a818f6",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "party",
      "name": "e3a836aeac6f4cbe9bcab985494d3175"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": true,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  },
  {
    "id": "11111111-ebe0-42da-885f-033860a818f7",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "TitleStorageTestDefault",
      "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": false,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  }]
}
    
```

  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   