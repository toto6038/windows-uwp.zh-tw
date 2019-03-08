---
title: TitleRequest (JSON)
assetID: 43aeb6f9-726d-9260-e2ba-f005ea688bf1
permalink: en-us/docs/xboxlive/rest/json-titlerequest.html
description: " TitleRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a90f42c2f830ba6f04f77a1acaba067a2746a062
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593793"
---
# <a name="titlerequest-json"></a>TitleRequest (JSON)
標題的相關資訊的要求。 
<a id="ID4EN"></a>

 
## <a name="titlerequest"></a>TitleRequest
 
TitleRequest 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| id| 32 位元不帶正負號的整數| 標題的識別碼。| 
| 活動 (activity)| [ActivityRequest](json-activityrequest.md)| 在標題資訊，包括豐富的目前狀態和媒體資訊，如果有的話。| 
| 狀態| 字串| 是否使用中的使用者。 這被必要欄位將標示為非作用中的使用者。 預設為 「 作用中 」。| 
| 放置| 字串| 標題位置模式。 可能值包括 「 完整 」，以 「 填滿 」，「 貼齊 」 或 「 背景 」。 預設為"full"。| 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
  id:"12341234",
  placement:"snapped",
  state:"active",
  activity:
  {
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"82b11353-08ba-48ca-9f1a-21627b189b0f"
    }
  }
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>參考 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   