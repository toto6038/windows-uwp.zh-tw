---
title: POST ({itemID})
assetID: 2c3c166b-e638-cfb9-d68e-9f8ab9a838d3
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurlpost.html
description: " POST ({itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 877986ce9d48269295a68dbfd644f14785916b88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640853"
---
# <a name="post-itemid"></a>POST ({itemID})
表示已使用的全部或部分可取用的清查項目和遞減的要求量可使用的數量。
這些 Uri 的網域是`inventory.xboxlive.com`。

  * [註解](#ID4EX)
  * [URI 參數](#ID4EQB)
  * [要求本文](#ID4E2B)
  * [回應主體](#ID4ENC)

<a id="ID4EX"></a>


## <a name="remarks"></a>備註

   * 如果呼叫端要求會耗用的數量超過剩餘的供應項目，此呼叫將會遭到拒絕。
   * 使用要求呼叫端的數量必須大於 0 的正整數。 使用值為 0 或更低的呼叫將會遭到拒絕。
   * 如果呼叫端提供空白的交易識別碼，將會拒絕要求。
   * 如果有的話，讓您將能夠判斷哪一個標題所報告的耗用量，就會記錄標題宣告。
   * 使用相同之 transactionId 的其他文章將會忽略某些時間週期。


> [!NOTE]
> <b>x xbl-合約版本標頭</b>此 API 是"4"。


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| itemID| 字串| 單數的清查項目的每一位使用者的唯一識別碼|

<a id="ID4E2B"></a>


## <a name="request-body"></a>要求本文

<a id="ID4EBC"></a>


### <a name="sample-request"></a>範例要求


```cpp
{
  "transactionId": String
  "removeQuantity": Int
}

```


移除 [數量] 欄位可讓呼叫者指出他們想要移除與取用剩餘的數量可使用的數量。 交易識別碼欄位提供一個方法來限制兩次計算相同的使用方式的風險時使用取用內容的作業重試的呼叫者。

<a id="ID4ENC"></a>


## <a name="response-body"></a>回應主體

文章中，假設它通過驗證，並指派適當的授權內容的回應的接收具有相同之 transactionId acknolodgement 傳遞給在 POST 要求、 需求的可取用的項目 URL 以及項目的新服務數量值。

<a id="ID4EVC"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
  "transactionId": String
  "url": String
  "newQuantity": Int
}

```


<a id="ID4E6C"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EBD"></a>


##### <a name="parent"></a>父系

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)


<a id="ID4ELD"></a>


##### <a name="further-information"></a>進一步的資訊

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)
