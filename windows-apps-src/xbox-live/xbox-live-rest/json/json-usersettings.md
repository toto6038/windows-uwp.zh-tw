---
title: UserSettings (JSON)
assetID: 17c030cb-05e0-f78e-5ab1-cdbd8b801ceb
permalink: en-us/docs/xboxlive/rest/json-usersettings.html
description: " UserSettings (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5451c59ab608105677a657ade41154bd2b622f5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655043"
---
# <a name="usersettings-json"></a>UserSettings (JSON)
傳回目前已驗證使用者的設定。 
<a id="ID4EN"></a>

 
## <a name="usersettings"></a>UserSettings
 
UserSettings 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| id| 32 位元不帶正負號的整數| 設定的識別碼。| 
| 來源| 32 位元不帶正負號的整數| 表示設定的來源。 | 
| titleId| 32 位元不帶正負號的整數| 標題與設定相關聯的識別項。 | 
| value| 8 位元不帶正負號整數的陣列| 表示設定的值。 正在擷取設定的用戶端必須了解要能夠讀取資料的表示法格式。 | 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
   "id":268697600,
   "source":1,
   "titleId":1297287259,
   "value":"CAAAAA=="
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   