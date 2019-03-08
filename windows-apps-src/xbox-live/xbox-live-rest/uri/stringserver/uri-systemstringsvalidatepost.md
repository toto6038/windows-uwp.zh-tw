---
title: POST (/system/strings/validate)
assetID: 6a59bc0b-8edd-87bf-efaf-f16efa3bedf7
permalink: en-us/docs/xboxlive/rest/uri-systemstringsvalidatepost.html
description: " POST (/system/strings/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 70e86567f449674c7a046e072437d9ee715dc6d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595503"
---
# <a name="post-systemstringsvalidate"></a>POST (/system/strings/validate)
可接受用於驗證的字串陣列，並傳回一個陣列的相同大小的結果。 這些 Uri 的網域是`client-strings.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [必要的要求標頭](#ID4EIB)
  * [要求本文](#ID4ELC)
  * [HTTP 狀態碼](#ID4E4C)
  * [回應主體](#ID4ETF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
每個結果，表示對應的字串是否可接受在 Xbox LIVE，並包含產生錯誤的字串，如果適用的話。
 
相同的字串一律會產生相同的結果。 如果您收到不成功的結果時，分析結果，並據此修改的字串。
 
 

> [!NOTE] 
> 產生<b>VerifyStringResult</b>會只報告的違規的第一個字，在字串中。 可能有其他違規字串內的文字。 如果您打算將違規的字組，以便使字串可使用時，您應該取代有問題的單字或子字串，並重新驗證要尋找其他有問題的子字串的字串。  

 
  
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | 
| Authorization| 驗證權杖。 範例：XBL3.0 x=[hash];[token].| 
| x-xbl-contract-version| 整數 API 合約版本。 必須是 1 或 2 此 api。| 
  
<a id="ID4ELC"></a>

 
## <a name="request-body"></a>要求本文
 
要求主體是字串陣列，陣列的大小沒有限制和每個字串的 512 個字元。
 
<a id="ID4ETC"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
{
    "stringstoVerify":
    [
        "Poppycock",
        "The quick brown fox jumped over the lazy dog.",
        "Hey, want to hang out?",
        "oh noes"
    ]
}
      
```

   
<a id="ID4E4C"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 200| 確定| 已成功處理的所有字串。 這不一定表示所有字串都有正面的 Hresult。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 403| 已禁止| 要求不允許使用者或服務。| 
| 406| 無法接受| 遺漏<b>內容類型： application/json</b>標頭。| 
| 408| 要求逾時| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
  
<a id="ID4ETF"></a>

 
## <a name="response-body"></a>回應主體
 
傳回的陣列[VerifyStringResult (JSON)](../../json/json-verifystringresult.md)，為要求陣列相同的大小。
  
<a id="ID4EAG"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ECG"></a>

 
##### <a name="parent"></a>父系 

[/system/strings/validate](uri-systemstringsvalidate.md)

   