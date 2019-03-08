---
title: POST (/users/me/resetreputation)
assetID: 1a4fed45-f608-dac2-3384-2ee493112f7b
permalink: en-us/docs/xboxlive/rest/uri-usersmeresetreputationpost.html
description: " POST (/users/me/resetreputation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fc770673ac7e4034004f58032c7fa66cb28413e7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632683"
---
# <a name="post-usersmeresetreputation"></a>POST (/users/me/resetreputation)
可讓強制執行小組，將目前使用者的信譽分數為一些任意的值之後 （舉例來說） 帳戶攔截。 這些 Uri 的網域是`reputation.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [Authorization](#ID4E5)
  * [必要的要求標頭](#ID4ETB)
  * [要求本文](#ID4END)
  * [HTTP 狀態碼](#ID4EDE)
  * [回應主體](#ID4EFH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
由所有的沙箱，零售，除了任何其他協力廠商和零售版，但基於測試目的的所有沙箱中的使用者，也可能會呼叫這個方法。 請注意，此要求設定使用者的 「 基本 」 信譽分數，他的正面回饋意見加權將所有會歸零。這些基底的分數加上他大使額外加上他粉絲獎金，將會進行此呼叫後的使用者的實際信譽。
  
<a id="ID4E5"></a>

 
## <a name="authorization"></a>Authorization
 
從夥伴：零售沙箱中， **PartnerClaim**強制執行小組的; 所有其他的沙箱**PartnerClaim**。
 
從使用者：所有的沙箱，零售，除了**XuidClaim**並**TitleClaim**。
  
<a id="ID4ETB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
從所有項目：**內容類型： application/json**。
 
從夥伴：**X Xbl-合約版本**（目前版本為 101）， **X-Xbl-沙箱**。
 
從使用者：**X Xbl-合約版本**（目前的版本會是 101）。
 
| 標頭| 類型| 描述| 
| --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。| 
| X-RequestedServiceVersion|  | 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：101.| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>要求本文
 
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>範例要求
 
要求本文是一項簡單[ResetReputation (JSON)](../../json/json-resetreputation.md)文件。
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 200| 確定| 確定。| 
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 404| 找不到| 找不到指定的資源。| 
| 500| 內部伺服器錯誤| 伺服器發生未預期的狀況，導致無法完成要求。| 
| 503| 服務無法使用| 要求已節流處理，以秒為單位 （例如 5 秒之後） 的用戶端重試值之後，再次嘗試要求。| 
  
<a id="ID4EFH"></a>

 
## <a name="response-body"></a>回應主體
 
成功時，回應主體是空的。 在失敗時， [ServiceError (JSON)](../../json/json-serviceerror.md)會傳回文件。
 
<a id="ID4ERH"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
         
```

   
<a id="ID4E2H"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E4H"></a>

 
##### <a name="parent"></a>父系 

[/users/me/resetreputation](uri-usersmeresetreputation.md)

   