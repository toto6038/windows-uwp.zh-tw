---
title: /media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes
assetID: fc096def-ac64-76c6-09f8-8f33a6bb47a0
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediagroupsmediaitemtypes.html
description: " /media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d014cbcf4e42e2f07c3cc32ceeb557a3c8537871
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637063"
---
# <a name="mediamarketplaceidmetadatamediagroupsmediagroupmediaitemtypes"></a>/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes
存取可用的 mediaItemTypes 每個媒體 EDS 的指定之版本的群組。 這些 Uri 的網域是`eds.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| marketplaceId| 字串| 必要。 字串值取自<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
| mediagroup| 字串| 必要。 其中一個值從[取得 (/media/ {marketplaceId} / 中繼資料/mediaGroups)](uri-medialocalemetadatamediagroupsget.md)。| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)

&nbsp;&nbsp;列出可用的 mediaItemTypes 每個媒體 EDS 的指定之版本的群組。
 
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

   