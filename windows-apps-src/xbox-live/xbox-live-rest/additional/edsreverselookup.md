---
title: 影片的 EDS 反向對應
assetID: 773f7a8e-7571-3aec-96d6-478437696ea6
permalink: en-us/docs/xboxlive/rest/edsreverselookup.html
description: " 影片的 EDS 反向對應"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d535dec8a95eba4d486bfc6946e187e2da66ae49
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598433"
---
# <a name="eds-reverse-lookup-for-video"></a>影片的 EDS 反向對應
 
  * [反向查閱步驟](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="reverse-lookup-steps"></a>反向查閱步驟
 
娛樂探索服務 (EDS) 反向查閱支援所有的視訊媒體類型 (**MediaItemType.Movie**， **MediaItemType.TVSeries**， **MediaItemType.TVEpisode**， **MediaItemType.TVSeason**，以及**MediaItemType.TVShow**)，以及**MediaItemType.Unknown**。
 
反向對應需要 4 個參數傳遞： 
   * `idType=ScopedMediaId`
   * `ids=` 提供者的媒體識別碼
   * `ScopeIdType=Title`
   * `ScopeId=` 提供者標題識別碼
 
 
通常是反向的對應需要 2 個步驟： 
   * 如果它無法使用，請擷取提供者的媒體識別碼 （例如，從詳細資料的呼叫）。 

```cpp
GET /media/en-us/details?ids=4eeaf5b4-9af2-56e4-a738-68b48e954494&desiredMediaItemTypes=Movie&desired=Providers
```

 
   * 發出的呼叫使用反向對應**ProviderMediaId**欄位前一個回應： 

```cpp
GET /media/en-us/details?ids=047d19ca-3a7d-462c-bdbb-163543125583&idType=ScopedMediaId&desiredMediaItemTypes=Movie&fields=all&ScopeIdType=Title&ScopeId=0x5848085B
```

 
  
 
如果**ProviderMediaId**不是由 eds 擷取欄位，則此欄位必須是 URL 編碼的正確傳遞至 EDS。
  
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>父系  

[其他參考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4E3C"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

   