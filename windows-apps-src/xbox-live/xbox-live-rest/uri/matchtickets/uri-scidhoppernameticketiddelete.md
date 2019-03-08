---
title: DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})
assetID: d9ff3f21-aa70-af41-afa1-9a9244fcdb95
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketiddelete.html
description: " DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fdd28cb94b31102d9af98aa95afde45424dadce9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589663"
---
# <a name="delete-serviceconfigsscidhoppershoppernameticketsticketid"></a>DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})

移除符合票證。

> [!IMPORTANT]
> 這個方法是針對與合約 103 或更新版本，搭配使用，因此需要 X Xbl-合約版本標頭項目：103 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4E2)
  * [Authorization](#ID4EGB)
  * [HTTP 狀態碼](#ID4EOC)
  * [要求本文](#ID4EXC)
  * [回應主體](#ID4ECD)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

此 HTTP/REST 方法會從具名 hopper 在服務組態識別碼 (SCID) 層級刪除指定的票證識別碼。 這個方法可以前後加**Microsoft.Xbox.Services.Matchmaking.MatchmakingService.DeleteMatchTicketAsync**。  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務組態 (SCID) 工作階段識別碼。|
| name| 字串| Hopper 名稱。|
| ticketId| GUID| 票證識別碼。|

<a id="ID4EGB"></a>


## <a name="authorization"></a>Authorization

| 類型| 必要| 描述| 如果遺失的回應|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID （使用者識別碼）| 是| 提出要求的使用者必須是票證工作階段票證所參考的成員。| 403|
| 特殊權限與裝置類型| 是| 當使用者的裝置類型設定為主控台時，只有在其宣告中的多人的權限的使用者可以對 「 配對 」 服務進行呼叫。| 403|

<a id="ID4EOC"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EXC"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4ECD"></a>


## <a name="response-body"></a>回應主體

回應主體會不傳送任何物件。

<a id="ID4EPD"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ERD"></a>


##### <a name="parent"></a>父系  

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)
