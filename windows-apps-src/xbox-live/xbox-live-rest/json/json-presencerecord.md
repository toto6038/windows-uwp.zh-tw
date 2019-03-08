---
title: PresenceRecord (JSON)
assetID: 414e6ef5-f7bd-70d0-7386-7aa1c3a56e21
permalink: en-us/docs/xboxlive/rest/json-presencerecord.html
description: " PresenceRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6d531352c4336e00c93a91e7c945602ab69695f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622583"
---
# <a name="presencerecord-json"></a>PresenceRecord (JSON)
單一使用者的線上的目前狀態的相關資料。
<a id="ID4EN"></a>


## <a name="presencerecord"></a>PresenceRecord

PresenceRecord 物件具有下列規格。

| 成員| 類型| 描述|
| --- | --- | --- |
| xuid| 字串| Xbox 使用者識別碼 (XUID) 的目標使用者。 提供的目前狀態資料是針對此使用者。|
| 裝置| 陣列[DeviceRecord](json-devicerecord.md)| 使用者的裝置記錄的清單。|
| 狀態| 字串| 在 Xbox LIVE 的使用者活動。 可能的值： <ul><li>線上：使用者必須至少一個裝置記錄。</li><li>消失：使用者是登入 Xbox LIVE，但是非使用中的任何標題。</li><li>離線：使用者沒有在任何裝置上。</li></ul> | 
| lastSeen| [LastSeenRecord](json-lastseenrecord.md)| 使用者有無有效 DeviceRecords 時，只使用最近一次看到的資訊。 如果物件已從快取中，移除其資料可能不會傳回，因為沒有持續性存放區。|

<a id="ID4E2C"></a>


## <a name="sample-json-syntax"></a>範例 JSON 語法


```json
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:"W8",
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page"
      }
    }]
  }]
}

```


<a id="ID4EED"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EGD"></a>


##### <a name="parent"></a>父系

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)


<a id="ID4EQD"></a>


##### <a name="reference"></a>參考

[POST （/使用者/批次）](../uri/presence/uri-usersbatchpost.md)

 [取得 (/ 使用者/我)](../uri/presence/uri-usersmeget.md)

 [DELETE (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentdelete.md)

 [GET (/users/xuid({xuid}))](../uri/presence/uri-usersxuidget.md)
