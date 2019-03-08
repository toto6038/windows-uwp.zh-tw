---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})
assetID: 00aa2f3d-69a6-6d68-e99b-aad4b102aba3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindexdelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fbe1942d32ee8facc83f1c723cee2aedaa1078d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618223"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersindex"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})
從工作階段中移除指定的成員。

> [!IMPORTANT]
> 這個 URI 方法需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [URI 參數](#ID4ET)
  * [HTTP 狀態碼](#ID4E5)
  * [要求本文](#ID4EFB)
  * [回應主體](#ID4EOB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務設定識別項 (SCID)。 工作階段識別項的第 1 部分。|
| sessionTemplateName| 字串| 工作階段範本的目前執行個體的名稱。 第 2 部分的工作階段識別碼。|
| sessionName| GUID| 工作階段的唯一識別碼。 第 3 部分的工作階段識別碼。|

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EFB"></a>


## <a name="request-body"></a>要求本文
請參閱中的要求結構[MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)。  
<a id="ID4EOB"></a>


## <a name="response-body"></a>回應主體
請參閱中的回應結構[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)。  
<a id="ID4EYB"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E1B"></a>


##### <a name="parent"></a>父系

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)
