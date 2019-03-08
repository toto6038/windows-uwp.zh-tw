---
title: TitleRecord (JSON)
assetID: 8e1bd699-e408-67c8-31da-2d968adfbc21
permalink: en-us/docs/xboxlive/rest/json-titlerecord.html
description: " TitleRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89baf7e9a737428d492246f1647a561a4a8170cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603983"
---
# <a name="titlerecord-json"></a>TitleRecord (JSON)
標題，包括其名稱和上次修改時間戳記的相關資訊。 
<a id="ID4EN"></a>

 
## <a name="titlerecord"></a>TitleRecord
 
TitleRecord DeviceRecord 或 LastSeenRecord，必須包含，但可能不會同時包含。
 
TitleRecord 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| id| 32 位元不帶正負號的整數| 資料錄的 TitleId。| 
| name| 字串| 標題的當地語系化的名稱。| 
| 活動 (activity)| [ActivityRecord](json-activityrecord.md)| 標題中使用者的活動。 只傳回如果深度是 「 全部 」。| 
| lastModified| DateTime| 上次更新記錄 UTC 時間戳記。| 
| 放置| 字串| 在使用者介面中的應用程式的位置。 可能值包括"fill"、 「 完整 」、 「 貼齊 」 或 「 背景 」。 預設為 「 完整 」 未放置的應用程式的功能的裝置。| 
| 狀態| 字串| 項目的狀態。 可以是 「 作用中 」 或 「 非作用中 」 （預設值）。 標題設定自己的活動和閒置的準則為基礎的狀態。| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    }
    
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUD"></a>

 
##### <a name="reference"></a>參考 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   