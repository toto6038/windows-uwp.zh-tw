---
title: /users/xuid({xuid})/achievements/{scid}/{achievementid}
assetID: 656a6d63-1a11-b0a5-63d2-2b010abd62e7
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementid.html
description: " /users/xuid({xuid})/achievements/{scid}/{achievementid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 00c577f60b67f15f75c47b5e737ca12819695110
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599443"
---
# <a name="usersxuidxuidachievementsscidachievementid"></a>/users/xuid({xuid})/achievements/{scid}/{achievementid}
傳回有關的成就，包括其設定中繼資料和使用者專屬資料的詳細資料。 

> [!NOTE] 
> 只支援平台。 

 
這些 Uri 的網域是`achievements.xboxlive.com`。
 
  * [URI 參數](#ID4E2)
 
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的使用者正在存取的資源。 必須符合 XUID 的已驗證的使用者。| 
| scid| GUID| 其成就正在存取的服務組態的唯一識別碼。| 
| achievementid| 32 位元不帶正負號的整數| 正在存取的成就 （在指定的 SCID) 的唯一識別碼。| 
  
<a id="ID4EMC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})](uri-usersxuidachievementsscidachievementidget.md)

&nbsp;&nbsp;取得成就的詳細資料。
 
<a id="ID4EWC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EYC"></a>

 
##### <a name="parent"></a>父系 

[成就 Uri](atoc-reference-achievementsv2.md)

   