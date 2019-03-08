---
title: Profile (JSON)
assetID: b92b1750-c2df-39b6-6c5c-f9e8068c8097
permalink: en-us/docs/xboxlive/rest/json-profile.html
description: " Profile (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7299fcb4d375a3fc35ad67306b70f5fa4afde963
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607513"
---
# <a name="profile-json"></a>Profile (JSON)
使用者的個人設定檔設定。 
<a id="ID4EN"></a>

 
## <a name="profile"></a>設定檔
 
設定檔物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| AppDisplayName| 字串| 在應用程式中顯示的名稱。 這可能是使用者的 「 真實名稱 」 或其玩家代號、 根據隱私權。 這項設定代表使用者的識別字串，其應該用於應用程式中顯示。| 
| GameDisplayName| 字串| 在遊戲中顯示的名稱。 這可能是使用者的 「 真實名稱 」 或其玩家代號、 根據隱私權。 這項設定代表使用者的識別字串，其應該用於遊戲中的顯示。| 
| Gamertag| 字串| 使用者的玩家代號。| 
| AppDisplayPicRaw| 字串| 未經處理的應用程式顯示圖片 URL （如下所示）。| 
| GameDisplayPicRaw| 字串| 原始遊戲的顯示圖片 URL （如下所示）。| 
| AccountTier| 字串| 使用者有何種帳戶？ Gold、 Silver 或 FamilyGold 嗎？| 
| TenureLevel| 32 位元不帶正負號的整數| 有幾年使用者已經使用 Xbox Live？| 
| 玩家分數| 32 位元不帶正負號的整數| 使用者的玩家分數。| 
  


> [!NOTE] 
> 圖片可以是使用者的 '實際圖片' 或其 XboxOne gamerpic，根據隱私權。 這些設定代表使用者的圖片 url 是用來在用戶端顯示。 此映像可能為空白 （表示使用者尚未設定任何圖片）。 


 
未經處理的 URL 是可調整大小的 URL。 它可以用來指定下列其中一種大小和格式使用附加`&format={format}&w={width}&h={height}`uri:
 
格式： png
 
大小：64 x 64，208 x 208，424 x 424
 
<a id="ID4E2D"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E4D"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   