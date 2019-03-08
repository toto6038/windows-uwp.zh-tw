---
title: GET (/media/{marketplaceId}/contentRating)
assetID: e677acdb-d905-3bea-b0ca-6d8becd663c0
permalink: en-us/docs/xboxlive/rest/uri-medialocalecontentratingget.html
description: " GET (/media/{marketplaceId}/contentRating)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8d1cb9d09de8671d4cd3d61e96a8335412237e5c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641583"
---
# <a name="get-mediamarketplaceidcontentrating"></a>GET (/media/{marketplaceId}/contentRating)
取得內容分級語彙基元。 這些 Uri 的網域是`eds.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4ELB)
  * [查詢字串參數](#ID4EWB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
強制家長控制兒童可以看到的內容是複雜的工作。 每個媒體項目類型有它自己的分級系統不僅這些分級系統可能會不同國家。 這表示，有數個不同的資料片段時必須指定正確篩選 所有項目。
 
而不是指定所有參數的所有 API 呼叫，此 API 會產生值傳入**combinedContentRating**其他 Api 中的參數和仍然進行通訊的相同資訊。 這被為了讓 Api 使用和維護，變得更加容易，因為數個參數傳遞到這個 API 摺疊成單一、 可重複使用值的其他 api。
 
雖然最後可能會變更此 API 所傳回的確切值，但很少變更 （例如版本的娛樂探索服務 (EDS)），並因此無法快取長的時間。 任何 API 接受**combinedContentRating**參數會提供有意義的錯誤訊息，如果傳入的值無效，即的表示呼叫端就只需要呼叫此 API，以取得更新的值。 如果 API 接受**combinedContentRating**參數，但未提供，沒有篩選的內容會根據 家長監護的進行。 

> [!NOTE] 
> 這並不表示會傳回僅 「 安全的 」 內容-這表示會傳回所有的內容，包括可能明確的內容。 


  
<a id="ID4ELB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | 
| marketplaceId| 字串| 必要。 字串值取自<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4EWB"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| filterExplicit| 布林值| 選用。 篩選明確的音樂。| 
| filterFamilyOnlyApps| 布林值| 選用。 篩選未標記為系列易記的應用程式。| 
| filterUnrated| 布林值| 選用。 判斷是否為 未分級的內容應該包含在回應中與否。| 
| maxGameRating| 32 位元帶正負號的整數| 選用。 篩選的遊戲。| 
| maxMovieRating| 32 位元帶正負號的整數| 選用。 篩選特定層級上面的影片。| 
| maxTVRating| 32 位元帶正負號的整數| 選用。 篩選電視。| 
  
<a id="ID4E5D"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EAE"></a>

 
##### <a name="parent"></a>父系 

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

  
<a id="ID4EKE"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   