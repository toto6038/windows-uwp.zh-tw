---
title: 市集 URI
assetID: 27b6035f-84b9-67a8-6a12-85c450d18a58
permalink: en-us/docs/xboxlive/rest/atoc-reference-marketplace.html
description: " 市集 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9fd8112c6e16b3e9d9fb70c34381e88ba5aa6273
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593873"
---
# <a name="marketplace-uris"></a>市集 URI

本節提供從 Xbox Live 服務的通用資源識別元 (URI) 位址和相關聯的超文字傳輸通訊協定 (HTTP) 方法的詳細*marketplace*服務，也就是娛樂探索服務 (EDS)。

執行在 Xbox 360 上、 Windows Phone 裝置上，或在 Xbox.com 的遊戲可以使用這項服務。

這些 Uri 的網域是 eds.xboxlive.com 和 inventory.xboxlive.com。

<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>本節內容

[/users/me/inventory](uri-inventory.md)

&nbsp;&nbsp;存取清查目前提供的使用者相關聯的集合。

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)

&nbsp;&nbsp;完整設定的特定需求的可取用的清查項目的詳細資料的存取。

[/inventory/{itemID}](uri-inventoryitemurl.md)

&nbsp;&nbsp;完整設定的特定存貨項目的詳細資料的存取。

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

&nbsp;&nbsp;來自數個不同的媒體群組存取項目。

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

&nbsp;&nbsp;可讓瀏覽單一媒體群組內的項目。

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

&nbsp;&nbsp;存取內容分級語彙基元。

[/media/{marketplaceId}/details](uri-medialocaledetails.md)

&nbsp;&nbsp;傳回優惠詳細資料和中繼資料相關的一或多個項目。

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

&nbsp;&nbsp;存取欄位的語彙基元。

[/media/{marketplaceId}/metadata/mediaGroups](uri-medialocalemetadatamediagroups.md)

&nbsp;&nbsp;指定之 EDS 版本列出所有支援 mediaGroups。

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

&nbsp;&nbsp;存取可用的 mediaItemTypes 每個媒體 EDS 的指定之版本的群組。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

&nbsp;&nbsp;從其中一個可望指定的 mediaitemtype 和 EDS 指定的版本的資料存取的欄位。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners](uri-medialocalemetadatamediaitemtypequeryrefiners.md)

&nbsp;&nbsp;針對給定的媒體項目類型，請存取查詢 refiners。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

&nbsp;&nbsp;存取指定的查詢，矩陣名稱和指定的媒體項目類型可接受的值。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

&nbsp;&nbsp;存取子的值，指定的查詢矩陣值 (例如，"subgenres 中指定的內容類型 」) 的清單。

[/media/{marketplaceId}/metadata/mediaItemTypes](uri-medialocalemetadatamediaitemtypes.md)

&nbsp;&nbsp;存取指定之 EDS 版本的所有支援的 mediaItemTypes。

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

&nbsp;&nbsp;存取可用來排序指定的 mediaitem 類型和指定的版本 EDS 的訂單。

[/media/{marketplaceId}/singleMediaGroupSearch](uri-medialocalesinglemediagroupsearch.md)

&nbsp;&nbsp;可讓搜尋單一媒體群組內的項目。

<a id="ID4EFD"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EHD"></a>


##### <a name="parent"></a>父系

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)


<a id="ID4ERD"></a>


##### <a name="further-information"></a>進一步的資訊

[其他參考](../../additional/atoc-xboxlivews-reference-additional.md)
