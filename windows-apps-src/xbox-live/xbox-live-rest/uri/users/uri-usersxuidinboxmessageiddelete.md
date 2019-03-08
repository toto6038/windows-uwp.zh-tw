---
title: DELETE (/users/xuid({xuid})/inbox/{messageId})
assetID: c54eede3-3e3b-2cbe-1be9-8bf3a48171bc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageiddelete.html
description: " DELETE (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 80ec2a462648177cc6bfc846b9c84278821b0e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594103"
---
# <a name="delete-usersxuidxuidinboxmessageid"></a>DELETE (/users/xuid({xuid})/inbox/{messageId})
刪除使用者的收件匣中的使用者訊息。 這些 Uri 的網域是`msg.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4ECB)
  * [Authorization](#ID4EPB)
  * [要求本文](#ID4E1B)
  * [HTTP 狀態碼](#ID4EHC)
  * [JavaScript 物件標記法 (JSON) 的回應](#ID4EAE)
  * [在資源上的隱私權設定的效果](#ID4EYF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註 
 
刪除作業是等冪。
 
此 API 支援只包含內容類型為"application/json"，都需要在每個呼叫的 HTTP 標頭。 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 參數 
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid | 不帶正負號的 64 位元整數 | Xbox 使用者識別碼 (XUID) 正提出要求者的播放程式。 | 
| messageId | string[50] | 正在擷取或刪除訊息的識別碼。 | 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Authorization 
 
您必須擁有您自己的使用者宣告，以刪除使用者訊息。
  
<a id="ID4E1B"></a>

 
## <a name="request-body"></a>要求本文 
 
此要求主體中不傳送任何物件。
  
<a id="ID4EHC"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼 
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 描述| 
| --- | --- | --- | --- | --- | 
| 204| 成功。| 
| 403| 無法轉換 XUID 或找不到有效的 XUID 宣告。| 
| 404| 無法剖析在 URI 中的訊息識別碼，或在 URI 中遺漏 XUID。| 
| 500| 一般的伺服器端錯誤。| 
  
<a id="ID4EAE"></a>

 
## <a name="javascript-object-notation-json-response"></a>JavaScript 物件標記法 (JSON) 的回應 
 
如果發生錯誤，服務可能會傳回 errorResponse 物件，其中可能包含服務的環境的值。
 
| 屬性| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| errorSource| 字串| 表示產生錯誤。| 
| errorCode| 整數| （可以是 null） 的錯誤相關聯的數字代碼。| 
| errorMessage| 字串| 如果設定為顯示詳細資料的錯誤詳細資料。| 
  
<a id="ID4EYF"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果 
 
只有您可以刪除您自己的使用者訊息。 
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>父系  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)

  
<a id="ID4ETG"></a>

 
##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>參考[標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)

   