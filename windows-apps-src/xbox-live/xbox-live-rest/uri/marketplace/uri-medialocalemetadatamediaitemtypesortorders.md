---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
assetID: 221c440d-c448-11e9-f455-6d0f95fc16ef
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypesortorders.html
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e807f1a29712f55fc2e404e3905c7c69eb50d166
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640923"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypesortorders"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
存取可用來排序指定的 mediaitem 類型和指定的版本 EDS 的訂單。 這些 Uri 的網域是`eds.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| marketplaceId| 字串| 必要。 字串值取自<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
| mediaitemtype| 字串| 必要。 其中一個值從[取得 (/media/ {marketplaceId} / 中繼資料/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)。| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders)](uri-medialocalemetadatamediaitemtypesortordersget.md)

&nbsp;&nbsp;列出可用的排序順序指定的 mediaitem 型別和 EDS 指定的版本。
 
<a id="ID4ELC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ENC"></a>

 
##### <a name="parent"></a>父系 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EXC"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   