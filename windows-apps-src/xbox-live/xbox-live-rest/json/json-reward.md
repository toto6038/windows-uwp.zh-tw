---
title: Reward (JSON)
assetID: d1c92e8a-afbc-22c5-c0b5-6063963f8c4d
permalink: en-us/docs/xboxlive/rest/json-reward.html
description: " Reward (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d58d34263e7e0e90091c41c1df4fd5e078f5055
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607493"
---
# <a name="reward-json"></a>Reward (JSON)
成就與相關聯的報酬。
<a id="ID4EN"></a>


## <a name="reward"></a>獎勵

Reward 物件具有下列規格。

| 成員| 類型| 描述|
| --- | --- | --- |
| name| 字串| Reward 面對使用者的名稱。|
| description| 字串| Reward 面對使用者的描述。|
| value| 字串| 獎勵的值。|
| type| RewardType 列舉| Reward 類型： <ul><li>不是正確 (0):設定不明和不支援 reward 型別。</li><li>玩家分數 (1):Reward 會將點加入至播放程式的玩家分數。</li><li>inApp (2):Reward 定義，且項目所傳遞。</li><li>圖案 (3):Reward 是數位資產。</li></ul> | 
| valueType| ProgressValueDataType 列舉| 值的型別。 請參閱[需求 (JSON)](json-requirement.md)如需詳細資訊。|

<a id="ID4EBD"></a>


## <a name="sample-json-syntax"></a>範例 JSON 語法


```json
{
  "name":null,
  "description":null,
  "value":"10",
  "type":"Gamerscore",
  "valueType":"Int"
}

```


<a id="ID4EKD"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EMD"></a>


##### <a name="parent"></a>父系

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)
