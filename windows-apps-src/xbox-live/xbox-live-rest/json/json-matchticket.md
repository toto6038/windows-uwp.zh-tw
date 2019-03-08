---
title: MatchTicket (JSON)
assetID: 12617677-47f2-e517-af53-5ab9687eea2a
permalink: en-us/docs/xboxlive/rest/json-matchticket.html
description: " MatchTicket (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4bc638dfe7735856295ed92f35e244213be7bc1e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608333"
---
# <a name="matchticket-json"></a>MatchTicket (JSON)
JSON 物件，代表符合票證，用來找出透過多人連線的工作階段目錄 (MPSD) 的其他播放程式播放程式。 
<a id="ID4EN"></a>

  
 
MatchTicket JSON 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| serviceConfig| GUID| 服務組態 (SCID) 工作階段識別碼。| 
| hopperName| 字串| Hopper 放置於此票證的名稱。| 
| giveUpDuration| 32 位元帶正負號的整數| 最長等待時間 （整數秒數）。| 
| preserveSession| 列舉| 值，指出是否必須為要比對的工作階段重複使用工作階段。 可能的值為 「 永遠 」 或 「 從未 」。 | 
| ticketSessionRef| MultiplayerSessionReference| <b>MultiplayerSessionReference</b>工作階段中的播放程式或群組目前正在播放的物件。 這個成員一律是必要的。 | 
| ticketAttributes| 物件的陣列| 播放程式的使用者提供的屬性和票證相關的值集合。| 
| 播放程式| 物件的陣列| 播放程式物件集合，每個使用者提供屬性的屬性包。 | 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
        "serviceConfig": "07617C5B-3423-4505-B6C6-10A16E1E5DDB",
        "hopperName": "TestHopper",
        "giveUpDuration": 10,
        "preserveSession": "never",
        "ticketSessionRef": {
        "scid": "AFFEABDF-0000-0000-0000-000000000001",
        "templateName": "TestTemplate",
        "sessionName": "5E551041-0000-0000-0000-000000000001"
    },
    "ticketAttributes": {
        "desiredMap": "Test Map",
        "desiredGameType": "Test GameType"
    },
    "players": [
        {
            "xuid": 123412345123,
            "playerAttributes": {
                "skill": 5,
                "ageRange": "teenager"
            }
        },
        {
          "xuid": 123412345124,
          "playerAttributes": {
              "skill": 15,
              "ageRange": "twenty-something"
          }
        }
      ]
    }
  
    
```

  
<a id="ID4EEB"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EGB"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   