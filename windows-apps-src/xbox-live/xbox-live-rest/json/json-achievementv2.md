---
title: Achievement (JSON)
assetID: d3b52f66-ddc7-e676-b419-82209caf71d6
permalink: en-us/docs/xboxlive/rest/json-achievementv2.html
description: " Achievement (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0e20f46a0d97cba496df5c6fb9cda14fbeccccd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628473"
---
# <a name="achievement-json"></a>Achievement (JSON)
成就物件 （第 2 版）。
<a id="ID4EN"></a>


## <a name="achievement"></a>成就

成就物件具有下列規格。 所有成員都是必要項目。

| 成員| 類型| 描述|
| --- | --- | --- |
| id| 字串| 資源識別項。|
| serviceConfigId| 字串| 此資源 SCID。 識別此成就與 title(s)。 |
| name| 字串| 當地語系化的成就名稱。|
| titleAssociations| 陣列[TitleAssociation](json-titleassociation.md)| TitleAssociation 陣列。|
| progressState| **ProgressState**列舉| 進度的狀態： <ul><li>不是正確 (0):成就進展處於未知狀態。</li><li>達成 (1):成就已解除鎖定。</li><li>inProgress (2):已鎖定的成就，但使用者所做進度解除鎖定的狀況。</li><li>notStarted (3):成就已鎖定且使用者未進行任何進度解除鎖定的狀況。</li></ul> | 
| 進展| [進展](json-progression.md)| 成就內使用者的進展。|
| mediaAssets| 陣列[MediaAsset](json-mediaasset.md)| 成就，例如映像識別碼相關聯之媒體資產。 |
| 平台| 字串| 平台的成就已贏得上。|
| isSecret| 布林值| Zda bude 成就是祕密。|
| description| 字串| 成就解除鎖定時的描述。|
| lockedDescription| 字串| 成就解除鎖定之前的描述。|
| productId| 字串| 隨附 ProductId 成就。|
| achievementType| **AchievementType**列舉| （相同為先前的類型，在舊版的成積） 的成就類型： <ul><li>不是正確 (0):未知的和不受支援的成就型別。</li><li>持續性 (1):沒有結束日期，隨時都可以解除鎖定的成就。</li><li>挑戰 (2):有特定的時間範圍期間可以是解除鎖定的成就。</li></ul> |
| participationType| **ParticipationType**列舉| 成就參與類型。 有效值為個人或群組。|
| timeWindow| TimeWindow| 時間範圍期間成就可能會解除鎖定。 僅支援挑戰。|
| 獎勵| 陣列[Reward](json-reward.md)| 解除鎖定時，所獲得的獎勵的集合。|
| estimatedTime| TimeSpan| 預估的時間而獲得會成就。|
| 深層連結| 字串| 標題到深層連結。|
| isRevoked| 布林值| 是否強制執行撤銷成就。|

<a id="ID4EIAAC"></a>


## <a name="sample-json-syntax"></a>範例 JSON 語法


```json
{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
          "requirements":
          [{
            "id":"12345678-1234-1234-1234-123456789012",
            "current":null,
            "target":"100"
          }],
          "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }

```


<a id="ID4ERAAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ETAAC"></a>


##### <a name="parent"></a>父系

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)
