---
title: 輔助 EDS API
assetID: 5729ab80-e88d-0190-fb61-bd0cc4f134f6
permalink: en-us/docs/xboxlive/rest/eds-apis.html
description: " 輔助 EDS API"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2f729359f52b09879e7227ede71e238fe63801
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603233"
---
# <a name="auxiliary-eds-apis"></a>輔助 EDS API

有數個娛樂探索服務 (EDS) Api 不直接提供內容的相關資訊，但提供有關如何使用服務，或協助磁碟機常見 UI 模型的一般資訊。

<a id="ID4EQ"></a>


## <a name="auxiliary-apis"></a>輔助 Api

| API| URI| 描述|
| --- | --- | --- |
| API 參數值| /{locale}/metadata| 可以對服務呼叫中使用的參數的可能值的列舉型別|
| 合併的內容分級產生器| /{locale}/contentRating| 建立其他 Api 中的可用值，篩選出可能令人反感或明確的內容。 如需詳細資訊，請參閱下方內容。|
| 合併的欄位名稱產生器| /{locale}/fields| 建立一個值可控制傳回哪些欄位的詳細資料 Api。 如需詳細資訊，請參閱下方內容。|

<a id="ID4EBC"></a>


### <a name="api-parameter-values"></a>API 參數值

此 API 會描述可與服務的參數。 因為當地語系化的文字隨附於每個參數，傳回的資訊是供用戶端 UI。

下列 Api 都不接受任何查詢參數。

| API| URI| 描述|
| --- | --- | --- | --- | --- | --- |
| 類型| /{locale}/metadata/mediaGroups| 媒體群組的完整清單|
| 媒體項目每個媒體群組的類型| /{locale}/metadata/mediaGroups/{mediaItemTypeGroup}/mediaItemTypes| 媒體清單項目指定的媒體群組內所包含的類型。|
| 所有的媒體項目類型| /{locale}/metadata/mediaItemTypes| 媒體項目類型的完整清單|
| 每個媒體項目類型的欄位| /{locale}/metadata/mediaItemTypes/{mediaItemType}/fields| 單一媒體項目類型中的欄位清單|
| 查詢 Refiners| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners| 查詢 refiners 支援給定的媒體項目類型的清單|
| 所有查詢矩陣的值| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}| 針對給定的媒體查詢指定的矩陣的值項目類型|
| 所有查詢矩陣子值| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}/subQueryRefinerValues| 指定的查詢矩陣值 (例如"subgenres 中指定的內容類型 」) 的子值的清單。 查詢矩陣值傳入做為查詢字串參數，名為"queryRefinerValue 」，這樣便可允許查詢矩陣值，要傳遞的 URI 詞幹禁止的字元。|
| 排序| /{locale}/metadata/mediaItemTypes/{mediaItemType}/sortOrders| 針對給定的媒體項目類型的排序順序的清單|

<a id="ID4EEF"></a>


### <a name="combined-content-rating-generator"></a>合併的內容分級產生器

強制家長控制兒童可以看到的內容是複雜的工作。 每個媒體項目類型有它自己的分級系統不僅這些分級系統可能會不同國家。 這表示，有數個不同的資料片段時必須指定正確篩選 所有項目。

而不是指定所有參數的所有 API 呼叫，此 API 會產生要傳遞至其他 Api 中的 combinedContentRating 參數，並仍然進行通訊的相同資訊的值。 這被為了讓 Api 使用和維護，變得更加容易，因為數個參數傳遞到這個 API 摺疊成單一、 可重複使用值的其他 api。

雖然最後可能會變更此 API 所傳回的確切值，但很少變更 （例如 EDS 的版本），並因此無法快取長的時間。 接受 combinedContentRating 參數會提供有意義的錯誤訊息中傳遞的值是否無效，即表示呼叫端只是任何 API 需要呼叫此 API，以取得更新的值。 如果 API 接受 combinedContentRating 參數，但沒有提供，沒有篩選的內容將根據 家長監護

> [!NOTE]
> 這並不表示會傳回僅 「 安全的 」 的內容，這表示會傳回所有的內容，包括可能的偏激內容）。



<a id="ID4EWF"></a>


### <a name="combined-field-name"></a>合併的欄位名稱

EDS 的 Api，根據預設，傳回非常小的最小集合的每個項目欄位：

   * 媒體項目類型
   * 媒體群組
   * Id
   * 名稱

若要取得詳細資訊，Api 會接受 「 欄位 」 參數來指定應該傳回哪一個額外的一段資料。 因為有許多可能的欄位，每個 API 呼叫的完整指定其名稱會大幅膨脹要求。 相反地，名稱可以傳遞到這個 API 會產生更小的值可以傳遞至其他 Api。

此參數會接受任何 API，提供的值必須在所有指定的媒體項目類型中的所有欄位的超集。 您不能指定不同的欄位不同的媒體項目類型的集合。 不過，如果欄位套用至一個媒體項目類型，但不是，它只會顯示在媒體項目類型有資料 (例如只有 AvatarItems 如果合併欄位名稱 API 的呼叫中包含"AvatarBodyType"，則會包含欄位)。

事實上是高可快取-此 API 傳回的值，它們不應該變更除了 EDS 的部署之間。 建議您，如果想要快取，快取持續時間不會超過使用者工作階段。

除了接受的實際欄位名稱，此 API 接受 「 全部 」 做為有效的值。 這會產生包含可指定每個欄位的值。 "All"值很可能只能用於開發、 偵錯和測試用途。

<a id="ID4ERG"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ETG"></a>


##### <a name="parent"></a>父系  

[其他參考](atoc-xboxlivews-reference-additional.md)


<a id="ID4E6G"></a>


##### <a name="further-information"></a>進一步的資訊

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)
