---
title: POST (/users/xuid({xuid})/deleteuserdata)
assetID: 8be13ff9-5d42-43a1-f2fa-d550d845a552
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddeleteuserdatapost.html
description: " POST (/users/xuid({xuid})/deleteuserdata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab43079dbba3729ff39f3a2116c377c3b73142a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604063"
---
# <a name="post-usersxuidxuiddeleteuserdata"></a>POST (/users/xuid({xuid})/deleteuserdata)
完全重設的測試使用者的信譽資料。 僅供測試。

  * [註解](#ID4EQ)
  * [URI 參數](#ID4E5)
  * [Authorization](#ID4EJB)
  * [必要的要求標頭](#ID4E3B)
  * [HTTP 狀態碼](#ID4EHC)
  * [回應主體](#ID4EJF)

<a id="ID4EQ"></a>


## <a name="remarks"></a>備註

呼叫此 API 會從使用者移除所有的意見項目和評價資料。 合作夥伴可能會呼叫此 API，針對任何沙箱零售除外。 強制執行小組可能會呼叫此 API 使用任何沙箱識別碼。

這些 Uri 的網域是`reputation.xboxlive.com`。 連接埠 10443 一律會呼叫此 URI。

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 正在刪除其資料的使用者。|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Authorization

零售沙箱中， **PartnerClaim**強制執行小組。

所有其他的沙箱**PartnerClaim**並**SandboxIdClaim**。

<a id="ID4E3B"></a>


## <a name="required-request-headers"></a>必要的要求標頭

**內容類型： application/json**並**X Xbl-合約版本**（目前的版本會是 101）。

<a id="ID4EHC"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- |
| 200| 確定| 已成功擷取工作階段。|
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。|
| 401| 未經授權| 要求需要使用者驗證。|
| 404| 找不到| 找不到指定的資源。|
| 500| 內部伺服器錯誤| 伺服器發生未預期的狀況，導致無法完成要求。|
| 503| 服務無法使用| 要求已節流處理，以秒為單位 （例如 5 秒之後） 的用戶端重試值之後，再次嘗試要求。|

<a id="ID4EJF"></a>


## <a name="response-body"></a>回應主體

無成功;否則，請[ServiceError (JSON)](../../json/json-serviceerror.md)文件。

<a id="ID4EWF"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EYF"></a>


##### <a name="parent"></a>父系

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)
