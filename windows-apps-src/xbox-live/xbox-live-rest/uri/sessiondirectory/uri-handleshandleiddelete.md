---
title: DELETE (/handles/{handleId})
assetID: 71cceff4-3a74-dc05-12db-cfe3f20a8aea
permalink: en-us/docs/xboxlive/rest/uri-handleshandleiddelete.html
description: " DELETE (/handles/{handleId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 354f3563c48139edc5d5cc041e8304998af55620
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613403"
---
# <a name="delete-handleshandleid"></a>DELETE (/handles/{handleId})
刪除控制代碼識別碼。 所指定的控制代碼

> [!IMPORTANT]
> 這個方法由 2015年多人遊戲，並只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本，搭配使用，而且需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4EAB)
  * [HTTP 狀態碼](#ID4ELB)
  * [要求本文](#ID4ESB)
  * [回應主體](#ID4E4B)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註
此 HTTP/REST 方法刪除指定之識別碼的控制代碼，並清除使用者的目前活動的工作階段。 這個方法可以前後加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.ClearActivityAsync**。  
<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| handleId| GUID| 工作階段的控制代碼的唯一識別碼。|

<a id="ID4ELB"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4ESB"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4E4B"></a>


## <a name="response-body"></a>回應主體

回應主體會不傳送任何物件。

<a id="ID4EIC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EKC"></a>


##### <a name="parent"></a>父系

[/handles/{handleId}](uri-handleshandleid.md)
