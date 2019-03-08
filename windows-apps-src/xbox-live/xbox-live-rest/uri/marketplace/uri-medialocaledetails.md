---
title: /media/{marketplaceId}/details
assetID: bc8758ed-2f90-b501-5c3f-6f253f02d754
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetails.html
description: " /media/{marketplaceId}/details"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f58e5247c3fd52e84a3a9bab28c6926f74e864e3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655703"
---
# <a name="mediamarketplaceiddetails"></a>/media/{marketplaceId}/details
傳回優惠詳細資料和中繼資料相關的一或多個項目。 這些 Uri 的網域是`eds.xboxlive.com`。
 
API 與相關的 API 和瀏覽 API 的詳細資料 (當 passin 識別碼中的) 為這些 API 會傳回明確或隱含的方式，與 fiven 識別碼相關聯的其他項目的相關資訊，而詳細資料 API 會傳回其他資訊相關的相同的項目。
 
不同的媒體項目類型的多個識別碼可以傳入單一呼叫 （只要它們不是型別的 ProviderContentID-請參閱下方），但它們全都必須屬於相同的媒體群組。 不過，有幾個呼叫端不了解媒體群組的用戶端案例。 此 API 會支援這藉由允許 「 不明 」 媒體群組 sepcial 值在下列情況：
 
   * 識別碼 = XboxHexTitle，讓出 AppType 或遊戲類型的項目
   * 識別碼 = ProviderContentId，讓出 MovieType 或 TVType 項目
  
下圖摘要說明整個對應的識別碼的類型，即可提供哪些媒體群組：
 
| 識別碼類型| AppType| GameType| MovieType| MusicArtistType| MusicType| TVType| WebVideoType| 不明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 標準| Y| Y| Y| Y| Y| Y| Y| N| 
| ZuneCatalog| N| N| Y| Y| Y| Y| N| N| 
| ZuneMediaInstance| N| N| Y| N| Y| Y| N| N| 
| 產品| Y| Y| Y| N| Y| Y| N| N| 
| AMG| N| N| N| N| Y| N| N| N| 
| MediaNet| N| N| N| N| Y| N| N| N| 
| XboxHexTitle| Y| Y| N| N| N| N| N| Y| 
| ProviderContentId| N| N| Y| N| N| Y| N| Y| 
 
  * [參數資訊](#ID4EEH)
  * [URI 參數](#ID4EUH)
 
<a id="ID4EEH"></a>

 
## <a name="parameter-notes"></a>參數資訊
 
<a id="ID4EIH"></a>

 
### <a name="providercontentid"></a>ProviderContentId
 
這是用來查閱提供者特定的識別碼。例如 Netflix 識別碼或 Hulu 識別碼。
 
ProviderContentId 識別碼時，會接受單一值。 這是因為 ProviderContentIds 是唯一的識別碼可包含類型 '。 ' 字元。 因為 '。 ' 字元也是我們使用 Id 之間的分隔符號之間的 delimieter Id 之間的功能沒有模稜兩可，什麼是 「 識別碼 」 本身的一部分。 API 的 rest ProviderContentIds 的運作方式相同，除了大量查閱功能。
   
<a id="ID4EUH"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| marketplaceId| 字串| 必要。 字串值取自<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4EWAAC"></a>

 
## <a name="valid-methods"></a>有效的方法

[取得 (/media/ {marketplaceId} / 詳細資料)](uri-medialocaledetailsget.md)

&nbsp;&nbsp;傳回優惠詳細資料和中繼資料相關的一或多個項目。 
 
<a id="ID4EABAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ECBAC"></a>

 
##### <a name="parent"></a>父系 

[Marketplace Uri](atoc-reference-marketplace.md)

  
<a id="ID4EMBAC"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   