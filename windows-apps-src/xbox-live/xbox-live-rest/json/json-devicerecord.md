---
title: DeviceRecord (JSON)
assetID: aca4f4d3-f9b4-8919-5b6d-5ae0fe11e162
permalink: en-us/docs/xboxlive/rest/json-devicerecord.html
description: " DeviceRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9746706c00a09cd8b64913b4ae8b5c3426551e48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646143"
---
# <a name="devicerecord-json"></a>DeviceRecord (JSON)
裝置，包括其型別和它的作用中的項目相關的資訊。 
<a id="ID4EN"></a>

 
## <a name="devicerecord"></a>DeviceRecord
 
DeviceRecord 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| type| 字串| 裝置的裝置類型。 可能值包括"D"、"Xbox360 」、 「 MoLIVE"(Windows)、"W"、"WindowsPhone7 」 和"PC"(G4WL)。 如果類型是未知 （適用於範例 iOS、 Android 或內嵌在網頁瀏覽器中的標題），則會傳回"Web"。| 
| 標題| 陣列[TitleRecord](json-titlerecord.md)| 此裝置上使用中的項目清單。| 
  
<a id="ID4EWB"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
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
  }]
    
```

  
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ENC"></a>

 
##### <a name="reference"></a>參考   