---
title: GameMessage (JSON)
assetID: c11606e6-701f-5807-4aef-5608c98ad831
permalink: en-us/docs/xboxlive/rest/json-gamemessage.html
description: " GameMessage (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a2bddd9e26b4716fd1e33c4b5bbde56672b5d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651293"
---
# <a name="gamemessage-json"></a>GameMessage (JSON)
JSON 物件，在遊戲工作階段的訊息佇列中定義之訊息的資料。 
<a id="ID4EN"></a>

  
 
GameMessage JSON 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| 資料| 8 位元不帶正負號整數的陣列| Base64 編碼的資料，遊戲的用戶端想要傳送給其他遊戲的用戶端。 這個值是不透明的伺服器。 | 
| senderXuid| 64 位元不帶正負號的整數| 將訊息傳送 media player Xbox 使用者識別碼。 | 
| sequenceNumber| 32 位元帶正負號的整數| 遊戲的訊息的序號。 這個值是由伺服器指派。 序號會保證會以單純方式遞增，但可能不連續。 是唯一的序號中訊息佇列，但不是訊息佇列之間。 | 
| queueIndex| 32 位元帶正負號的整數| 工作階段訊息佇列訊息的索引。 可能的值為 0-3。| 
| timeStamp| DateTime| 當遊戲的訊息佇列中由伺服器所建立，以 utc 表示的時間。 | 
  
<a id="ID4ERC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "queueIndex": 0,
    "sequenceNumber": 5,
    "senderXuid": 65536,
    "timestamp": "2011-06-23T18:49:50Z",
    "data": null
}
    
```

  
<a id="ID4E1C"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E3C"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EGD"></a>

 
##### <a name="reference"></a>參考 

[GameSession (JSON)](json-gamesession.md)

   