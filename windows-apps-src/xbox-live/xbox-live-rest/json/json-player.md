---
title: Player (JSON)
assetID: eaf6d082-869b-d2d3-d548-5cef65e54541
permalink: en-us/docs/xboxlive/rest/json-player.html
description: " Player (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5967cbfecd47c5675926bd45939442c45dda7b6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622493"
---
# <a name="player-json"></a>Player (JSON)
包含播放程式的資料，在遊戲工作階段中。 
<a id="ID4EN"></a>

 
## <a name="player"></a>播放程式
 
Player 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| customData| 8 位元不帶正負號整數的陣列| 1024 個位元組的 Base64 編碼特定遊戲的玩家資料。 這個值是不透明的伺服器。| 
| 玩家代號| 字串| 玩家代號 — 最多 15 個字元，播放程式。 用戶端在識別播放程式時，應該在 UI 中使用此值。 | 
| isCurrentlyInSession| 布林值| 表示是否播放程式目前在工作階段，或保留工作階段。| 
| seatIndex| 32 位元帶正負號的整數| 工作階段中的播放程式索引。| 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的播放程式。| 
  
<a id="ID4E3C"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "xuid": 2533274790412952,
    "gamertag":"MyTestUser",
    "seatindex": 3
    "customData":"AIHJ2?iE?/jiKE!l5S=T..."
    "isCurrentlyInSession":"true"
}
    
```

  
<a id="ID4EFD"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EHD"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference"></a>參考 

[GameSession (JSON)](json-gamesession.md)

   