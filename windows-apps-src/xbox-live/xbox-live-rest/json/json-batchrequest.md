---
title: BatchRequest (JSON)
assetID: 2ca34506-8801-efc5-7c83-3c9ec5572276
permalink: en-us/docs/xboxlive/rest/json-batchrequest.html
description: " BatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51073c71700613edcd7c22e18cc0c00a9222d7e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654153"
---
# <a name="batchrequest-json"></a>BatchRequest (JSON)
用來篩選顯示狀態資訊，例如使用者、 裝置和標題的屬性陣列。
<a id="ID4EN"></a>


## <a name="batchrequest"></a>BatchRequest

BatchRequest 物件具有下列規格。

| 成員| 類型| 描述|
| --- | --- | --- |
| 使用者| 字串陣列| 清單 XUIDs 的使用者存在您想要深入了解，最多為 1100 XUIDs 一次。|
| deviceTypes| 字串陣列| 您想要知道使用者所使用的裝置類型的清單。 如果陣列為空白，則預設為所有可能的裝置類型 （也就是 none 則濾除）。|
| 標題| 32 位元不帶正負號整數的陣列| 裝置清單輸入您想要了解其的使用者。 如果陣列為空白，則預設為所有可能的項目 （也就是 none 則濾除）。|
| level| 字串| 可能的值： <ul><li>使用者-get 使用者節點</li><li>裝置-取得使用者和裝置節點</li><li>標題-取得基本項目層級資訊</li><li>all-取得豐富的目前狀態資訊、 媒體資訊，或兩者</li></ul>預設值為"title"。| 
| onlineOnly| 布林值| 如果這個屬性為 true，批次作業將會篩選出離線使用者 （包括已隱匿的項目） 的記錄。 如果未提供，則會傳回線上和離線的使用者。|

<a id="ID4EAD"></a>


## <a name="sample-json-syntax"></a>範例 JSON 語法


```json
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}


```


<a id="ID4EJD"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ELD"></a>


##### <a name="parent"></a>父系

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>參考   
