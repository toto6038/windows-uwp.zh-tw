---
title: UpdateMetadataRequest (JSON)
assetID: 0bc210e3-c1dc-9267-e322-aadb9f0a074a
permalink: en-us/docs/xboxlive/rest/json-updatemetadatarequest.html
description: " UpdateMetadataRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a76e4b12e0ffadb112913775b500ac0d39d413d5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597763"
---
# <a name="updatemetadatarequest-json"></a>UpdateMetadataRequest (JSON)
應該針對剪輯更新中繼資料。 
<a id="ID4EN"></a>

 
## <a name="updatemetadatarequest"></a>UpdateMetadataRequest
 
UpdateMetadataRequest 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| userCaption| 字串| 變更使用者輸入所未當地語系化的字串之遊戲的剪輯。| 
| 可見度| [GameClipVisibility 列舉](../enums/gvr-enum-gameclipvisibility.md)| 在系統中發行，請變更遊戲剪輯的可見性。| 
| titleData| 字串| 標題特有的屬性包中。 大小上限：10 KB。| 
  
<a id="ID4EBC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 
變更使用者剪輯名稱和可見性：
 

```json
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
變更只會標題 （這只是範例，由於此欄位的結構描述是由呼叫者） 的屬性：
 

```json
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>參考 

[POST (/users/me/scids/{scid}/clips/{gameClipId})](../uri/dvr/uri-usersmescidclipsgameclipidpost.md)

   