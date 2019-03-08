---
title: /media/{marketplaceId}/browse
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
description: " /media/{marketplaceId}/browse"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f692fb66580e20ffeefb3595b8cf9d795f504311
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589683"
---
# <a name="mediamarketplaceidbrowse"></a>/media/{marketplaceId}/browse
可讓瀏覽單一媒體群組內的項目。 瀏覽 API 可讓用戶端瀏覽從單一媒體群組內的項目。 可以使用而不使用接續 token skipItems 參數非循序存取資料的頁面。
 
此 API 也可讓瀏覽內指定的項目子系。 比方說，方法是針對 Xbox 360 遊戲識別碼和 MediaItemType 參數傳遞，這可讓瀏覽和 diltering 該項目，例如顯示圖片的項目或 DLC 遊戲的子系。
 
此 API 會接受查詢 Refiners。
 
擷取子系的一些案例包括：
 
   * 要追蹤相簿
   * 季節的系列
   * 影片的季節
   * 뮤 직 追蹤
   * 以專輯演出者
   * 遊戲，遊戲附加元件 （DLC、 顯示圖片、 佈景主題等）
  
這些 Uri 的網域是`eds.xboxlive.com`。
 
  * [URI 參數](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| marketplaceId| 字串| 必要。 字串值取自<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (media/{marketplaceId}/browse)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;可讓瀏覽單一媒體群組內的項目。 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>父系 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   