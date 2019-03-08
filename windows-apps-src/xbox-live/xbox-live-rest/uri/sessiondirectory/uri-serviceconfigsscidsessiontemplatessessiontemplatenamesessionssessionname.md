---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
assetID: 55ce6459-1714-49bc-6231-b547ddf04143
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da8d61634d0a849d51e2ab6ff143469ec05cc75c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657473"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
支援建立和擷取工作階段上的 PUT 和 GET 作業。
<a id="ID4EO"></a>


## <a name="domain"></a>網域
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| scid| GUID| 服務設定識別項 (SCID)。 工作階段識別項的第 1 部分。|
| sessionTemplateName| 字串| 工作階段範本的目前執行個體的名稱。 第 2 部分的工作階段識別碼。|
| sessionName| GUID| 工作階段的唯一識別碼。 第 3 部分的工作階段識別碼。| 

<a id="ID4EBC"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.md)

&nbsp;&nbsp;取得工作階段物件。

[PUT (/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} /sessions/ {sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

&nbsp;&nbsp;建立、 更新或加入工作階段。

<a id="ID4EOC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EQC"></a>


##### <a name="parent"></a>父系

[工作階段目錄的 Uri](atoc-reference-sessiondirectory.md)
