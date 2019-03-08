---
title: GameSession (JSON)
assetID: 325af9ae-a9a9-5b36-8342-1aff5aff01d1
permalink: en-us/docs/xboxlive/rest/json-gamesession.html
description: " GameSession (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca7276ccdc13d896d19873811b4fa9df9a831cd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646513"
---
# <a name="gamesession-json"></a>GameSession (JSON)
JSON 物件，代表遊戲資料的多人連線的工作階段。 
<a id="ID4ER"></a>

  
 
GameSession JSON 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| creationTime| DateTime| 日期和時間的工作階段建立時，以 utc 格式。 | 
| customData| 8 位元不帶正負號整數的陣列| 1024 位元組的遊戲特定工作階段資料。 這個值是不透明的伺服器。 | 
| displayName| 字串| 顯示遊戲的名稱與工作階段，最大長度為 128 個字元。 這個值是不透明的伺服器。 | 
| hasEnded| 布林值| 如果工作階段已結束，則為 true 和 false 否則。 設定此欄位，則為 true 的標示為唯讀的遊戲的工作階段正在提交至工作階段時，防止進一步的資料。 | 
| isClosed| 布林值| 如果工作階段已關閉，且沒有更多玩家可以經過新增，則為 false，則為 true。 如果此值為 true，則會拒絕加入工作階段的要求。 | 
| maxPlayers| 32 位元帶正負號的整數| 播放程式可以同時在工作階段中的最大數目。 值範圍是 2 到 16 個。 一旦工作階段包含播放程式的最大數目，進一步加入工作階段的要求會遭到拒絕。 | 
| playersCanBeRemovedBy| PlayerAcl| 值，指出哪些人員可以從工作階段中移除其他播放程式播放程式。 可能的值為 NoOne、 Self 和 AnyPlayer。 | 
| 名冊| 播放程式物件的陣列| 工作階段中的播放程式陣列。 名冊包含目前的播放程式和先前的工作階段中，但是剩下的玩家。 永遠不會變更的球員名單中的順序。 新的播放程式會加入至陣列的結尾。 | 
| seatsAvailable| 32 位元帶正負號的整數| 仍然可以加入工作階段之前的播放程式數目上限為止的球員數目。 這個值是唯讀的而且永遠會小於 maxPlayers 欄位的值。 | 
| sessionId| 字串| 建立工作階段時，由 MPSD 指派工作階段識別碼。 存取儲存在工作階段的資訊時，這個值通常會包含在 URI 中。| 
| titleId| 32 位元不帶正負號的整數| 建立遊戲的工作階段標題的識別碼。| 
| Variant| 32 位元帶正負號的整數| 遊戲的 variant。 這個值是不透明的伺服器。| 
| 可見度| VisibilityLevel| 值，指出工作階段的可見性。 可能值為：PlayersCurrentlyInSession PlayersEverInSession，以及在每個人。| 
  
<a id="ID4EEF"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "maxPlayers": 4,
    "seatsAvailable": 4,
    "isClosed": false,
    "hasEnded": false,
    "roster": [],
    "visibility": "PlayersCurrentlyInSession",
    "playersCanBeRemovedBy": "Self",
 }
    
```

  
<a id="ID4ENF"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EPF"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EZF"></a>

 
##### <a name="reference"></a>參考 

[GameMessage (JSON)](json-gamemessage.md)

 [GameSessionSummary (JSON)](json-gamesessionsummary.md)

 [播放程式 (JSON)](json-player.md)

   