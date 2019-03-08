---
title: POST (/users/xuid({xuid})/devices/current/titles/current)
assetID: d5e2d12d-ba75-2d04-2805-c69a4c143f73
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentpost.html
description: " POST (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b29a0bc796712d7b7c44a1fe8512f99bf09eb4fc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649523"
---
# <a name="post-usersxuidxuiddevicescurrenttitlescurrent"></a>POST (/users/xuid({xuid})/devices/current/titles/current)
更新使用者的目前狀態的標題。 這些 Uri 的網域是`userpresence.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4EEB)
  * [Authorization](#ID4EPB)
  * [必要的要求標頭](#ID4ENE)
  * [選擇性的要求標頭](#ID4ERG)
  * [要求本文](#ID4ERH)
  * [回應主體](#ID4EKAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
此 URI 可供非主控台平台上的所有項目，來新增和更新的目前狀態、 豐富的目前狀態和項目的媒體目前狀態資料。
 
**SandboxId**現在從 XToken 中的宣告擷取並強制執行。 如果**SandboxId**不存在，則娛樂探索服務 (EDS) 將會擲回 400 不正確的要求時發生錯誤。
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的目標使用者。| 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Authorization
 
| 類型| 必要| 描述| 如果遺失的回應| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| Xbox 使用者識別碼 (XUID) 的呼叫端| 403 禁止 」| 
| titleId| 是| 標題的 titleId| 403 禁止 」| 
| deviceId| 是適用於所有 Windows 和 Web 除外| 呼叫端的 deviceId| 403 禁止 」| 
| deviceType| 是適用於 Web 以外的所有| 呼叫端的裝置類型| 403 禁止 」| 
| sandboxId| 是適用於來自的呼叫 | 呼叫端的沙箱| 403 禁止 」| 
  
<a id="ID4ENE"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。| 
| x-xbl-contract-version| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 範例值：3，vnext。| 
| Content-Type| 字串| 範例值在要求主體的 mime 類型： application/json。| 
| Content-Length| 字串| 要求主體的長度。 範例值：312.| 
| 主機| 字串| 伺服器的網域名稱。 範例值： presencebeta.xboxlive.com。| 
  
<a id="ID4ERG"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.| 
  
<a id="ID4ERH"></a>

 
## <a name="request-body"></a>要求本文
 
Request 物件是[TitleRequest](../../json/json-titlerequest.md)。 只有在主體中實際存在屬性會更新。 任何屬性，都不屬於本文但存在於伺服器上將不會修改。
 
<a id="ID4EAAAC"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
{
  id:"12341234",
  placement:"snapped",
  state:"active"
}
      
```

   
<a id="ID4EKAAC"></a>

 
## <a name="response-body"></a>回應主體
 
如果成功，HTTP 狀態碼 200 或 201 已建立的是傳回，視情況。
 
若發生錯誤 （HTTP 4xx 或 5xx），適當的錯誤資訊會傳回回應主體中。
  
<a id="ID4EVAAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EXAAC"></a>

 
##### <a name="parent"></a>父系 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

  
<a id="ID4EBBAC"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   