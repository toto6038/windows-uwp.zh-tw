---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
assetID: a6c2fa17-8bed-d0df-d7ff-db1aa60f44b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b0ecadb543434383ef32c7fd2a749760d5bcc98
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608453"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
支援刪除作業，若要移除的工作階段的成員。
<a id="ID4EO"></a>


## <a name="domain"></a>網域
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="remarks"></a>備註

所有工作階段的成員資源作業都需要在 Xbox 使用者識別碼 (XUID) 使用者宣告的授權。

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| scid| GUID| 服務設定識別項 (SCID)。 工作階段識別項的第 1 部分。|
| sessionTemplateName| 字串| 工作階段範本的目前執行個體的名稱。 第 2 部分的工作階段識別碼。|
| sessionName| GUID| 工作階段的唯一識別碼。 第 3 部分的工作階段識別碼。|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>有效的方法

[DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersmedelete.md)

&nbsp;&nbsp;從工作階段中移除成員。

<a id="ID4EYC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E1C"></a>


##### <a name="parent"></a>父系

[工作階段目錄的 Uri](atoc-reference-sessiondirectory.md)
