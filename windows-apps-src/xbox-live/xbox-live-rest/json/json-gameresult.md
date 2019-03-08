---
title: GameResult (JSON)
assetID: 43d863c0-2179-ae46-5d4a-2f08cd44b667
permalink: en-us/docs/xboxlive/rest/json-gameresult.html
description: " GameResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b408b1aaae5e6f54958a016575c4a2c37765f1e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602333"
---
# <a name="gameresult-json"></a>GameResult (JSON)
JSON 物件，表示描述的遊戲工作階段結果的資料。 
<a id="ID4EN"></a>

  
 
GameResult JSON 物件具有下列成員。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| blob| 8 位元不帶正負號整數的陣列| 自訂標題特定結果的資料。| 
| 結果| 字串| 播放程式的參與遊戲的工作階段的結果。 有效值為 「 贏得 」、 「 遺失 」，或 「 結合 」。 | 
| 分數| 64 位元帶正負號的整數| 在遊戲工作階段中的播放程式收到的分數。| 
| 時間| 64 位元帶正負號的整數| 玩家的遊戲工作階段的時間。| 
| xuid| 64 位元不帶正負號的整數| 套用結果的播放程式 Xbox 使用者識別碼。| 
  
<a id="ID4EPC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
   "xuid": 2533274790412952,
   "outcome": "Win",
   "score": 100
}
    
```

  
<a id="ID4EYC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E1C"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   