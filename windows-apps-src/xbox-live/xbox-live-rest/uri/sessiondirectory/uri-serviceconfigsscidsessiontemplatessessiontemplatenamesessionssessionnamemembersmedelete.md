---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)
assetID: aa5de623-7787-a47c-b7e4-305693b9fe35
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersmedelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3de35398f4685a0b0cfda1a251c65ed6a74956d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637193"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)
從工作階段中移除成員。

> [!IMPORTANT]
> 這個 URI 方法需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4E3)
  * [HTTP 狀態碼](#ID4EHB)
  * [要求本文](#ID4ENB)
  * [回應主體](#ID4EYB)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註
所有工作階段的成員資源作業都需要 Xbox 使用者識別碼 (XUID) 授權。  
<a id="ID4E3"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務設定識別項 (SCID)。 工作階段識別項的第 1 部分。|
| sessionTemplateName| 字串| 工作階段範本的目前執行個體的名稱。 第 2 部分的工作階段識別碼。|
| sessionName| GUID| 工作階段的唯一識別碼。 第 3 部分的工作階段識別碼。|

<a id="ID4EHB"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4ENB"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4EYB"></a>


## <a name="response-body"></a>回應主體
請參閱中的回應結構[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)。  
<a id="ID4EBC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EDC"></a>


##### <a name="parent"></a>父系

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)
