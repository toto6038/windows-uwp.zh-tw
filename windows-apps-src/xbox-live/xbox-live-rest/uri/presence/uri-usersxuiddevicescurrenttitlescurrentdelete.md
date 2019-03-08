---
title: DELETE (/users/xuid({xuid})/devices/current/titles/current)
assetID: 3bf75247-0a2a-0e4c-afcc-9e7654a89648
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentdelete.html
description: " DELETE (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dd916fee5455276d45e4437e4ee90cacbde30bf9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604213"
---
# <a name="delete-usersxuidxuiddevicescurrenttitlescurrent"></a>DELETE (/users/xuid({xuid})/devices/current/titles/current)
移除存在的結尾標題，而不是等待[PresenceRecord](../../json/json-presencerecord.md)到期。 這些 Uri 的網域是`userpresence.xboxlive.com`。
 
  * [URI 參數](#ID4EZ)
  * [Authorization](#ID4EEB)
  * [必要的要求標頭](#ID4ERD)
  * [選擇性的要求標頭](#ID4EVF)
  * [要求本文](#ID4EVG)
  * [回應主體](#ID4EAH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的目標使用者。| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Authorization
 
| 類型| 必要| 描述| 如果遺失的回應| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| Xbox 使用者識別碼 (XUID) 的呼叫端| 403 禁止 」| 
| titleId| 是| 標題的 titleId| 403 禁止 」| 
| deviceId| 是適用於所有 Windows 和 Web 除外| 呼叫端的 deviceId| 403 禁止 」| 
| deviceType| 是適用於 Web 以外的所有| 呼叫端的裝置類型| 403 禁止 」| 
  
<a id="ID4ERD"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。| 
| x-xbl-contract-version| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 範例值：3，vnext。| 
| Content-Type| 字串| 範例值在要求主體的 mime 類型： application/json。| 
| Content-Length| 字串| 要求主體的長度。 範例值：312.| 
| 主機| 字串| 伺服器的網域名稱。 範例值： presencebeta.xboxlive.com。| 
  
<a id="ID4EVF"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.| 
  
<a id="ID4EVG"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4EAH"></a>

 
## <a name="response-body"></a>回應主體
 
如果成功，沒有回應主體會傳回 HTTP 狀態碼。
 
如果發生錯誤 （HTTP 4xx 或 5xx），適當的錯誤資訊會傳回回應主體中。
  
<a id="ID4ELH"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ENH"></a>

 
##### <a name="parent"></a>父系 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

   