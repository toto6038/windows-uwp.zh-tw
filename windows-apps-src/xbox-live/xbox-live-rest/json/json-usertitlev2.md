---
title: UserTitle (JSON)
assetID: 3e5767af-5704-8463-696b-42a7d2a1e231
permalink: en-us/docs/xboxlive/rest/json-usertitlev2.html
description: " UserTitle (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33901a5bde25fd17072c2b45d587a33209424378
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623023"
---
# <a name="usertitle-json"></a>UserTitle (JSON)
包含使用者項目資料。 
<a id="ID4EN"></a>

 
## <a name="usertitle"></a>UserTitle
 
UserTitle 物件具有下列規格。 所有屬性都是必要項目。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| lastUnlock| DateTime| 上一次贏得成就的時間。| 
| titleId| 32 位元不帶正負號的整數| 標題的唯一識別碼。| 
| titleVersion| 字串| 項目的版本。| 
| serviceConfigId| 字串| 標題相關聯之主要的服務組態集的識別碼。| 
| titleType| 字串| 標題的型別。| 
| 平台| 字串| 支援的平台。| 
| name| 字串| 此項目的文字名稱。 最大長度 22。| 
| earnedAchievements| 32 位元不帶正負號的整數| 成就的數目所獲得的標題，包括未鎖定的成就，以及成功完成的挑戰。| 
| currentGamerscore| 32 位元不帶正負號的整數| 這位使用者已獲得此標題中的總玩家分數。| 
| maxGamerscore| 32 位元不帶正負號的整數| 這個項目總數可能玩家分數。| 
  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   