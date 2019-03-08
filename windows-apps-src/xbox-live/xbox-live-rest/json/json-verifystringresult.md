---
title: VerifyStringResult (JSON)
assetID: 272c688e-179e-c7e9-086b-e76d0d4bcb57
permalink: en-us/docs/xboxlive/rest/json-verifystringresult.html
description: " VerifyStringResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b01793222be80efccdca1f24f5226a2e9ff78064
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599483"
---
# <a name="verifystringresult-json"></a>VerifyStringResult (JSON)
對應到送出至每個字串的程式碼會造成[/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md)。
<a id="ID4ER"></a>


## <a name="verifystringresult"></a>VerifyStringResult

VerifyStringResult 物件具有下列規格。

| 成員| 類型| 描述|
| --- | --- | --- |
| resultCode| 32 位元不帶正負號的整數| 必要。 HResult 程式碼對應至提交的字串。|
| offendingString| 字串| 必要。 導致拒絕字串的字串值。|

<a id="ID4EXB"></a>


## <a name="sample-json-syntax"></a>範例 JSON 語法


```json
{
    "verifyStringResult":
    [
        {"resultCode": "1", "offendingString": "badword"},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
    ]
}

```


#### <a name="common-hresult-values"></a>常見的 HResult 值

| 值| 錯誤名稱|
| --- | --- | --- | --- | --- |
| 0| 成功|
| 1| 具冒犯意味的字串|
| 2| 字串太長|
| 3| 未知的錯誤|

<a id="ID4ELD"></a>


## <a name="see-also"></a>請參閱

<a id="ID4END"></a>


##### <a name="parent"></a>父系

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>參考

[POST （/系統/字串/驗證）](../uri/stringserver/uri-systemstringsvalidatepost.md)
