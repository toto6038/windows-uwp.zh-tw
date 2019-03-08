---
title: PUT (/handles/{handle-id}/session)
assetID: d8a3d473-1192-ec0c-3935-c301f4f61e03
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionput.html
description: " PUT (/handles/{handle-id}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a1872857d8b8e692f67e3c7b2a067ae86663c00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641603"
---
# <a name="put-handleshandle-idsession"></a>PUT (/handles/{handle-id}/session)
建立或更新的控制代碼取值的工作階段。

> [!IMPORTANT]
> 這個方法由 2015年多人遊戲，並只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本，搭配使用，而且需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4ECB)
  * [HTTP 狀態碼](#ID4ENB)
  * [要求本文](#ID4EUB)
  * [回應主體](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

此 HTTP/REST 方法會將新的或更新的工作階段寫入多人遊戲服務，使用提供的工作階段控制代碼識別碼。 結果會是代表新的或更新的工作階段，從伺服器傳回的物件。 這個方法可以前後加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionByHandleAsync**。

此方法的呼叫端會從播放程式來取得控制代碼識別碼**MultiplayerActivityDetails**物件。 或者，呼叫端從取得 ID 的通訊協定啟動之後使用者已接受遊戲的邀請。

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| handleId| GUID| 工作階段的控制代碼的唯一識別碼。|

<a id="ID4ENB"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EUB"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4E6B"></a>


## <a name="response-body"></a>回應主體

回應主體會不傳送任何物件。

<a id="ID4EKC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EMC"></a>


##### <a name="parent"></a>父系

[/handles/{handleId}/session](uri-handleshandleidsession.md)
