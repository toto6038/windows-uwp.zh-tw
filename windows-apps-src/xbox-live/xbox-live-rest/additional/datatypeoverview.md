---
title: 資料類型概觀
assetID: c154a6fa-e7b2-4652-f6fc-f946f74480e9
permalink: en-us/docs/xboxlive/rest/datatypeoverview.html
description: " 資料類型概觀"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 62932a921d51a988a5533d7ee08f4968bb67a29d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659653"
---
# <a name="data-type-overview"></a>資料類型概觀
 
Xbox Live 服務會使用各種不同的身分識別與驗證相關的資料類型。 本主題提供這些類型的概觀。
 
| 類型| 描述| 
| --- | --- | 
| 玩家代號| 使用者的唯一、 人類看得懂的畫面名稱。| 
| 播放程式| JSON 物件，其中包含使用者的 XUID 和玩家代號、 是否播放程式仍參與工作階段中，與自訂資料的小型 blob，在工作階段 （或 「 基座 」），以及玩家的索引。| 
| profile| 使用者設定檔的 URI 位址和 HTTP 方法，通常是使用者的 UserSettings，透過存取，但也可能包括 gamercard、 玩家代號、 XUID，等等的相關資訊。| 
| 設定| 其中一個 UserSettings 物件中的標題專屬設定。| 
| UserClaims| 包含使用者的 XUID 和玩家代號的簡單 JSON 物件。| 
| UserSettings| JSON 物件，包含標題特有的設定或目前已驗證之使用者的喜好設定的集合。 UserSettings 可以包含任意的資料，可能與遊戲內活動。| 
| XUID| 使用者的 Xbox 使用者識別碼，唯一的不帶正負號長整數。 不是要人類看得懂。| 
 
<a id="ID4E6D"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EBE"></a>

 
##### <a name="parent"></a>父系  

[其他參考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ENE"></a>

 
##### <a name="reference--player-jsonjsonjson-playermd"></a>參考[Player (JSON)](../json/json-player.md)

 [UserClaims (JSON)](../json/json-userclaims.md)

 [UserSettings (JSON)](../json/json-usersettings.md)

   