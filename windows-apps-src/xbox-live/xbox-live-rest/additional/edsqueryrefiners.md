---
title: EDS 查詢精簡器
assetID: ab5c066a-a48b-3042-351d-d0a15f663276
permalink: en-us/docs/xboxlive/rest/edsqueryrefiners.html
description: " EDS 查詢精簡器"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c00ff971e05003ec88c47d3803e565f6e9406c47
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611463"
---
# <a name="eds-query-refiners"></a>EDS 查詢精簡器
 
<a id="ID4EO"></a>

  
 
下列參數可用來調整一組項目更具針對性的娛樂探索服務 (EDS) 查詢。 這些參數都不需要任何 API，但他們會接受在接受查詢 refiners 任何 API。
 
參數名稱可以傳遞做為值到任何 「 queryRefiners"參數。 這接著會傳回套用查詢矩陣重複要求時要傳回的項目數目，依每個值的查詢矩陣細分。
 
以下是這個可能的運作方式實際上：
 
   * 瀏覽 api 呼叫，包括參數 「 queryRefiners = 內容類型 」。
   * 此 API 會傳回八種遊戲。 項目，除了有項目每個內容類型的清單便會傳回，以及多少個項目屬於該內容類型。 對於遊戲時，這可能是"射擊：3，謎題：5".
   * 進行第二個查詢。 它等同於第一天，不同之處在於 「 內容類型 = 射擊"會加入。
   * 回應現在包含只有三個遊戲，全部都屬於 「 射擊 」 分類。
  
| 參數| 資料類型| 描述| 
| --- | --- | --- | 
| <b>decade</b>| 字串| 十年，在其中的所有項目必須已發行。| 
| <b>genre</b>| 字串陣列| 所有的項目必須具有的內容類型清單。| 
| <b>labelOwner</b>| 字串| 音樂與關聯的標籤演出者、 專輯或播放軌。| 
| <b>network</b>| 字串陣列| 建立項目的網路。| 
| <b>studio</b>| 字串陣列| 建立項目的 studio。| 
| <b>xboxAppCategories</b>| 字串陣列| 所有的 Xbox 應用程式必須有的分類清單。| 
| <b>xboxAvatarClothes</b>| 字串陣列| Xbox 顯示圖片的所有項目必須 clothing 型別的清單。| 
| <b>xboxAvatarStores</b>| 字串陣列| 所有 Xbox 虛擬人偶項目必須都屬於的商店的清單。| 
| <b>xboxGamePublisherBits</b>| 字串陣列| 必須設定所有的遊戲類型項目或 AppType 項目的遊戲發行商位元的清單。| 
| <b>xboxIsBrowsable</b>| 布林值| 如果<b>，則為 true</b>，會傳回完整的遊戲不是直接可採取動作，除了可採取動作的內容。 預設值為<b>false</b>。| 
| <b>xboxHasChildMediaItemTypes</b>| 字串陣列| 所有傳回的項目，與遊戲的媒體群組必須有子系的媒體項目類型是其中一個提供的值。| 
  
<a id="ID4EEF"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EGF"></a>

 
##### <a name="parent"></a>父系  

[其他參考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ESF"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)

   