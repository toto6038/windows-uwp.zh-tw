---
title: DELETE (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
assetID: 0a0355f6-940f-c959-bd5e-63b81872c1da
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapathandfilenametype-delete.html
description: " DELETE (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5183d2f8e919484d5b622d5e4d035360638850c1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598543"
---
# <a name="delete-trustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>DELETE (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
刪除的檔案。 這些 Uri 的網域是`titlestorage.xboxlive.com`。
 
  * [URI 參數](#ID4EX)
  * [Authorization](#ID4EEB)
  * [必要的要求標頭](#ID4ERB)
  * [選擇性的要求標頭](#ID4E1C)
  * [要求本文](#ID4EWD)
  * [HTTP 狀態碼](#ID4EDE)
  * [回應主體](#ID4EUBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數 
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 不帶正負號的 64 位元整數| Xbox 使用者識別碼 (XUID) 播放程式的人員提出要求。| 
| scid| guid| 要查閱的服務組態的識別碼。| 
| pathAndFileName| 字串| 要存取之項目的路徑和檔案名稱。 有效的字元 （最多且包含在最後斜線） 的路徑部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)，並正斜線 （/）。路徑部分可能是空的。有效的字元 （在最後斜線之後的所有內容） 中的檔案名稱部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)、 句號 （.），以及連字號 （-）。 檔案名稱不能是空的、 以句號結束或包含兩個連續的句號。| 
| type| 字串| 資料的格式。 可能的值為二進位或 json。| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Authorization 
 
要求必須包含有效的 Xbox LIVE 授權標頭。 如果呼叫端不允許存取此資源中，服務會傳回 403 禁止 回應。 如果標頭無效或遺失，則服務會傳回 401 未授權的回應。 
  
<a id="ID4ERB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API 合約版本。| 
| Authorization| XBL3.0 x=[hash];[token]| STS 驗證語彙基元。 驗證要求所傳回的語彙基元取代 STSTokenString。 如需關於擷取之 STS 權杖，以及建立授權標頭的詳細資訊，請參閱驗證和授權的 Xbox LIVE 服務要求。| 
  
<a id="ID4E1C"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 指定必須符合現有的項目來完成此作業的 ETag。| 
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>要求本文 
 
此要求主體中不傳送任何物件。
  
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼 
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定 | 已成功，刪除檔案，或不存在。| 
| 201| 建立時間 | 建立實體。| 
| 400| 錯誤的要求 | 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權 | 要求需要使用者驗證。| 
| 403| 已禁止 | 要求不允許使用者或服務。| 
| 404| 找不到 | 找不到指定的資源。| 
| 406| 無法接受 | 不支援資源版本。| 
| 408| 要求逾時 | 該要求花了太多時間完成。| 
| 500| 內部伺服器錯誤 | 伺服器發生未預期的狀況，導致無法完成要求。| 
| 503| 服務無法使用 | 要求已節流處理，以秒為單位 （例如 5 秒之後） 的用戶端重試值之後，再次嘗試要求。| 
  
<a id="ID4EUBAC"></a>

 
## <a name="response-body"></a>回應主體 
 
回應主體會不傳送任何物件。
  
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>父系  

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersxuidscidssciddatapathandfilenametype.md)

   