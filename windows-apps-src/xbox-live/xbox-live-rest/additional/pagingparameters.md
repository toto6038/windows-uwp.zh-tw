---
title: 分頁參數
assetID: f8d059fd-30e7-be60-0a46-c9a833c400c6
permalink: en-us/docs/xboxlive/rest/pagingparameters.html
description: " 分頁參數"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fe80e1666f9eab8caf7e0bbdb2b537bd7661c9a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627293"
---
# <a name="paging-parameters"></a>分頁參數
 
某些 Xbox Live 服務 Uri 會傳回 JavaScript 物件標記法 (JSON) 物件的集合。 這些集合可透過分頁，藉由指定分頁參數為附加至 URI 查詢字串的一部分。 分頁參數的完整清單如下。 允許分頁參數的所有 Uri 會都連結到此頁面底部。
 
<a id="ID4E2"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數 
 
| 參數| 必要| 類型| 描述| 
| --- | --- | --- | --- | 
| continuationToken| 否| 字串| 傳回項目，始於指定的接續 token。 | 
| maxItems| 否| 32 位元帶正負號的整數| 若要從集合中，可以結合傳回的項目數目上限<b>skipItems</b>並<b>continuationToken</b>要傳回的項目範圍。 服務可能會提供預設值，如果<b>maxItems</b>不存在，而且可能會傳回少於<b>maxItems</b>，即使尚未傳回結果的最後一頁。 | 
| skipItems| 否| 32 位元帶正負號的整數| 傳回指定數目的項目之後開始的項目。 例如， <b>skipItems ="3"</b>會擷取項目開頭的第四個項目擷取。 | 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>父系  

[其他參考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference--get-usersxuidxuidachievementsuriachievementsuri-achievementsusersxuidachievementsgetv2md"></a>參考[GET (/users/xuid({xuid})/achievements)](../uri/achievements/uri-achievementsusersxuidachievementsgetv2.md)

   