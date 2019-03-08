---
title: GET (/media/{marketplaceId}/fields)
assetID: 1d535344-8522-0fd1-4daa-cd0f0a0f1121
permalink: en-us/docs/xboxlive/rest/uri-medialocalefieldsget.html
description: " GET (/media/{marketplaceId}/fields)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cc3ae8abfe04b7a0b9728d07b9488f9ed7c27816
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606673"
---
# <a name="get-mediamarketplaceidfields"></a>GET (/media/{marketplaceId}/fields)
取得欄位的語彙基元。 這些 Uri 的網域是`eds.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4EGC)
  * [查詢字串參數](#ID4ERC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
娛樂探索服務 (EDS) Api，根據預設，會傳回非常小的最小集合的每個項目欄位：
 
   * 媒體項目類型
   * 媒體群組
   * Id
   * 名稱
  
若要取得詳細資訊，請接受 Api**欄位**參數，指定應該傳回哪一個額外的一段資料。 因為有許多可能的欄位，每個 API 呼叫的完整指定其名稱會大幅膨脹要求。 相反地，名稱可以傳遞到這個 API 會產生更小的值可以傳遞至其他 Api。
 
此參數會接受任何 API，提供的值必須在所有指定的媒體項目類型中的所有欄位的超集。 您不能指定不同的欄位不同的媒體項目類型的集合。 不過，如果欄位套用至一個媒體項目類型，但不是，它只會顯示在媒體項目類型有資料 (比方說，如果"AvatarBodyType 」 包含在呼叫時，才[取得 (/media/ {marketplaceId} / 欄位)](uri-medialocalefields.md)，只有AvatarItems 將會包含欄位）。
 
事實上是高可快取-此 API 傳回的值，它們不應該變更除了 EDS 的部署之間。 建議您，如果想要快取，快取持續時間不會超過使用者工作階段。
 
除了接受的實際欄位名稱，此 API 接受 「 全部 」 做為有效的值。 這會產生包含可指定每個欄位的值。 只針對開發、 偵錯和測試用途，建議使用 「 全部 」 的值。
 
或者，您可以傳送`desired={list of fields separated by a '.'}`接受任何 api**欄位**語彙基元。
 
您無法傳入兩者**所需**並**欄位**在一起。
  
<a id="ID4EGC"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| marketplaceId| 字串| 必要。 字串值取自<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4ERC"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 所需| 字串| 必要。 '。 '-分隔的欄位應該傳回的最小集合除了清單。 並非所有指定的欄位都一定會傳回每個物件 （資料可能只是沒有特定的項目），但會傳回非最小的集合不此處指定的任何欄位。 | 
  
<a id="ID4EMD"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EOD"></a>

 
##### <a name="parent"></a>父系 

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

  
<a id="ID4EYD"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   