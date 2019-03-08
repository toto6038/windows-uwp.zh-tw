---
title: /media/{marketplaceId}/singleMediaGroupSearch
assetID: f5599db7-4050-640e-db96-2df01a007c07
permalink: en-us/docs/xboxlive/rest/uri-medialocalesinglemediagroupsearch.html
description: " /media/{marketplaceId}/singleMediaGroupSearch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b26b4c2dc51ef5591480372aa9908a49d2f8cbe2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661753"
---
# <a name="mediamarketplaceidsinglemediagroupsearch"></a>/media/{marketplaceId}/singleMediaGroupSearch
可讓搜尋單一媒體群組內的項目。 請注意，可以使用而不使用接續 token skipItems 參數非循序存取此搜尋所傳回的資料頁面。 此 API 會接受查詢 Refiners。
 
這些 Uri 的網域是`eds.xboxlive.com`。
 
  * [URI 參數](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| marketplaceId| 字串| 必要。 字串值取自<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4EYB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (media/{marketplaceId}/singleMediaGroupSearch)](uri-medialocalesinglemediagroupsearchget.md)

&nbsp;&nbsp;可讓搜尋單一媒體群組內的項目。 
 
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>父系 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EOC"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   