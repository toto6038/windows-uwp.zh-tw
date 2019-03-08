---
title: InitialUploadRequest (JSON)
assetID: 8b8bce98-cb5f-bbaf-5564-9be2f58d749b
permalink: en-us/docs/xboxlive/rest/json-initialuploadrequest.html
description: " InitialUploadRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2fbb2417f743d97b487ad16abd241fcb5eea62bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646633"
---
# <a name="initialuploadrequest-json"></a>InitialUploadRequest (JSON)
主體的 POST 遊戲剪輯上傳要求。 
<a id="ID4EN"></a>

 
## <a name="initialuploadrequest"></a>InitialUploadRequest
 
InitialUploadRequest 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| <b>greatestMomentId</b>| 字串| 要做為美工圖案的名稱使用文字字串識別碼。 這是受管理，而且在組態檔中項目的當地語系化標題的開發人員。| 
| <b>userCaption</b>| 字串| 選用。 替代使用者輸入名稱長度上限為 250 個字元的遊戲剪輯。| 
| <b>sessionRef</b>| 字串| 選用。 在這項作業完成錄製遊戲工作階段的參考。| 
| <b>dateRecorded</b>| DateTime| 開始錄製，以 utc 表示的時間。 封送處理為 ISO 8601 格式的字串 (請參閱<a href="https://www.w3.org/TR/NOTE-datetime">日期和時間格式</a>如需詳細資訊)。| 
| <b>durationInSeconds</b>| 32 位元不帶正負號的整數| 美工圖案，以秒為單位的長度。| 
| <b>expectedBlocks</b>| 32 位元不帶正負號的整數| 選用。 檔案會劃分成的區塊數目。 如果檔案不會傳送單一要求中，省略。| 
| <b>fileSize</b>| 32 位元不帶正負號的整數| 以位元組為單位的影片將上傳的檔案大小。| 
| <b>type</b>| [GameClipType 列舉](../enums/gvr-enum-gamecliptypes.md)| 美工圖案，封送處理為列舉型別是以逗號分隔的字串值的型別。| 
| <b>source</b>| [GameClipSource 列舉](../enums/gvr-enum-gameclipsource.md)| 指定如何在剪輯的來源，封送處理為列舉型別的字串值。| 
| <b>visibility</b>| [GameClipVisibility 列舉](../enums/gvr-enum-gameclipvisibility.md)| 指定的可見性的遊戲的剪輯，一旦發佈系統中。| 
| <b>titleData</b>| 字串| 選用。 標題特有的屬性，這個項目相關聯的屬性包。 儲存並傳回做為是。 標題的開發人員可以使用此欄位來保存自己的中繼資料，關於美工圖案。| 
| <b>titleData</b>| 字串| 選用。 主控台特有的屬性，這個項目相關聯的屬性包。 儲存並傳回做為是。 主控台平台可以使用此欄位來保存自己的中繼資料，關於美工圖案。| 
| <b>systemProperties</b>| 字串| 選用。 主控台特有的屬性，這個項目相關聯的屬性包。 儲存和現狀傳回。 主控台平台可以使用此欄位來保存自己的中繼資料，關於美工圖案。| 
| <b>usersInSession</b>| 字串陣列| 選用。 目前的工作階段中的使用者清單。| 
| <b>thumbnailSource</b>| [ThumbnailSource 列舉](../enums/gvr-enum-thumbnailsource.md)| 選用。 在縮圖的來源。| 
| <b>thumbnailOffsetMillseconds</b>| 32 位元帶正負號的整數| 指定位移的產生縮圖的位移 （以毫秒為單位）。 只指定何時<b>thumbnailSource</b>設定位移。| 
| <b>savedByUser</b>| 布林值| 選用。 設定儲存到使用者的配額，而非 FIFO 儲存體的剪輯。 預設為 false。| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
   "greatestMomentId": "123abc",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   