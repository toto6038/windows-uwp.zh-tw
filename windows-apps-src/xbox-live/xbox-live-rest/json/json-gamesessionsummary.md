---
title: GameSessionSummary (JSON)
assetID: 50cf91ba-29d3-1260-7643-bcb3f8d74fc0
permalink: en-us/docs/xboxlive/rest/json-gamesessionsummary.html
description: " GameSessionSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8dace19404ae7c8b1d1ef296a21c874e4dd14c6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613383"
---
# <a name="gamesessionsummary-json"></a>GameSessionSummary (JSON)
JSON 物件，代表遊戲的工作階段的摘要資料。 
<a id="ID4EN"></a>

  
 
GameSessionSummary JSON 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| creationTime| DateTime| 日期和時間的工作階段建立時，以 utc 格式。 | 
| customData| 8 位元不帶正負號整數的陣列| 1024 位元組的遊戲特定工作階段資料。 這個值是不透明的伺服器。 | 
| displayName| 字串| 顯示遊戲的名稱與工作階段，最大長度為 128 個字元。 這個值是不透明的伺服器。 | 
| hasEnded| 布林值| 如果工作階段已結束，則為 true 和 false 否則。 設定此欄位，則為 true 的標示為唯讀的遊戲的工作階段正在提交至工作階段時，防止進一步的資料。 | 
| sessionId| 字串工作階段識別碼。 | 
| titleId| 32 位元不帶正負號的整數| 建立遊戲的工作階段標題的識別碼。| 
| Variant| 32 位元帶正負號的整數| 遊戲的 variant。 這個值是不透明的伺服器。| 
  
<a id="ID4EID"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "hasEnded": false,
}
    
```

  
<a id="ID4ERD"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ETD"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>參考 

[GameSession (JSON)](json-gamesession.md)

   