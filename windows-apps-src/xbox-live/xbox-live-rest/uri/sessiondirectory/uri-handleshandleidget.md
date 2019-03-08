---
title: GET (/handles/{handle-id})
assetID: c95b5ab5-d56a-f70d-20d8-afb48d122ccd
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidget.html
description: " GET (/handles/{handle-id})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 501d36f4d1ac079af15d6bb7f35a90d5328fc8db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598463"
---
# <a name="get-handleshandle-id"></a>GET (/handles/{handle-id})
擷取控制代碼識別碼。 所指定的控制代碼

> [!IMPORTANT]
> 這個方法由 2015年多人遊戲，並只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本，搭配使用，而且需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4EDB)
  * [HTTP 狀態碼](#ID4EOB)
  * [要求本文](#ID4EUB)
  * [回應主體](#ID4E5B)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

這個 HTTP/REST 方法會取得使用者的目前活動的工作階段中，指定控制代碼。 傳回為工作階段物件，其所有屬性。 這個方法可以前後加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**。

此方法的呼叫端會從播放程式來取得控制代碼識別碼**MultiplayerActivityDetails**物件。 或者，呼叫端從取得 ID 的通訊協定啟動之後使用者已接受遊戲的邀請。

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| handleId| GUID| 工作階段的控制代碼的唯一識別碼。|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EUB"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4E5B"></a>


## <a name="response-body"></a>回應主體
請參閱中的回應結構[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)。  
<a id="ID4EKC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EMC"></a>


##### <a name="parent"></a>父系

[/handles/{handleId}](uri-handleshandleid.md)
