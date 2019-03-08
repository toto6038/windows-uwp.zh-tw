---
title: TitleBlob (JSON)
assetID: fd1c904d-e8d0-f61f-e403-40b25bd4ac14
permalink: en-us/docs/xboxlive/rest/json-titleblob.html
description: " TitleBlob (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a0b17a46d1c71ffdf9098d4637ca59d840c90a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612583"
---
# <a name="titleblob-json"></a>TitleBlob (JSON)
包含標題，以從儲存體的相關資訊。 
<a id="ID4EP"></a>

 
## <a name="titleblob"></a>TitleBlob
 
TitleBlob 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| clientFileTime| DateTime| [選用]日期和時間的最後一個檔案上傳。| 
| displayName| 字串| [選用]顯示給使用者的檔案名稱。| 
| Etag| 字串| 標記中所使用的檔案下載和上傳要求。| 
| fileName| 字串| 檔案的名稱。| 
| size| 64 位元帶正負號的整數| 以位元組為單位的檔案大小。| 
| smartBlobType| 字串| [選用]資料型別。 可能的值為： 組態中，json，二進位。| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "fileName":"foo\bar\blob.txt,binary",
    "clientFileTime":"2012-01-01T01:02:03.1234567Z",
    "displayName":"Friendly Name",
    "size":12,
    "etag":"0x8CEB3E4F8F3A5BF",
    "smartBlobType":"binary"
}
      
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   