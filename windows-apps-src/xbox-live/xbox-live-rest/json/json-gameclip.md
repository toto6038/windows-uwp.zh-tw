---
title: GameClip (JSON)
assetID: 204cb702-4ce4-85a8-f231-3b4fb243405f
permalink: en-us/docs/xboxlive/rest/json-gameclip.html
description: " GameClip (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4cfc2ac0a635e4aacdc9eeefb5097c6bd946a518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646503"
---
# <a name="gameclip-json"></a>GameClip (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclip"></a>GameClip
 
遊戲剪輯物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| <b>gameClipId</b>| 字串| 指派至遊戲的剪輯的識別碼。| 
| <b>state</b>| GameClipState| 遊戲的美工圖案，在系統中的狀態。| 
| <b>dateRecorded</b>| DateTime| 日期和時間開始錄製，以 UTC （ISO 8601 格式）。| 
| <b>lastModified</b>| DateTime| 上次修改時間的遊戲的剪輯或它的中繼資料，以 UTC （ISO 8601 格式）。| 
| <b>userCaption</b>| 字串| 使用者輸入未當地語系化的字串之遊戲的剪輯。| 
| <b>type</b>| GameClipTypes| 美工圖案類型。 可以是多個值，因此如果是以逗號分隔。| 
| <b>source</b>| GameClipSource| 如何在剪輯的來源。| 
| <b>visibility</b>| GameClipVisibility| 遊戲的美工圖案，一旦發佈系統中的可見性。| 
| <b>durationInSeconds</b>| 32 位元不帶正負號的整數| 持續時間的遊戲的剪輯以秒為單位。| 
| <b>scid</b>| 字串| SCID 遊戲剪輯是相關聯。| 
| <b>rating</b>| 雙精確度浮點數| 遊戲的剪輯，範圍介於 0.0 到 5.0 中相關聯的評等。| 
| <b>ratingCount</b>| 32 位元不帶正負號的整數| 這個項目已經被評等的次數。| 
| <b>views</b>| 32 位元不帶正負號的整數| 遊戲的多媒體項目相關聯的檢視表的數目。| 
| <b>titleData</b>| 字串| 標題特有的屬性包中。| 
| <b>titleData</b>| 字串| 主控台特有的屬性包中。| 
| <b>thumbnails</b>| GameClipThumbnail 的陣列| GameClipThumbnail 物件的陣列。| 
| <b>gameClipUris</b>| GameClipUri 的陣列| GameClipUri 物件的陣列。| 
| <b>xuid</b>| 字串| 擁有者的遊戲的剪輯，封送處理字串為 XUID。| 
| <b>clipName</b>| 字串| 美工圖案的名稱，根據從標題管理系統中尋找要求的輸入法地區設定當地語系化的版本。| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-10-31T10:45:00Z",
     "clipName": "Forza 4",
     "userCaption": "My awesome car flip",
     "type": "DeveloperInitiated, Achievement",
     "source": "TitleDirect",
     "visibility": "Default",
     "durationInSeconds": 30,
     "scid": "ecb5497e-76d4-4a8a-870d-e76a26306b7d",
     "rating": 1.0,
     "views": 5,
     "thumbnails": [
       {
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
     ]
   }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   