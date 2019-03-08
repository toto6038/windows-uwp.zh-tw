---
title: /serviceconfigs/{scid}/hoppers/{hoppername}
assetID: ba1e129d-b4c4-6535-46ce-fd184465c85f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppername.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1db069f59eeba565a257531907d6be0b1dcd189d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598423"
---
# <a name="serviceconfigsscidhoppershoppername"></a>/serviceconfigs/{scid}/hoppers/{hoppername}

支援 POST 作業，以建立比對的票證。

> [!IMPORTANT]
> 此 URI 是針對合約 103 或更新版本，搭配使用，因此需要 X Xbl-合約版本標頭項目：103 或更新版本上的每個要求。

<a id="ID4ER"></a>


## <a name="domain"></a>網域
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務組態 (SCID) 工作階段識別碼。|
| hoppername | 字串 | Hopper 名稱。 |

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>有效的方法

[POST (/serviceconfigs/ {scid} /hoppers/ {hoppername})](uri-serviceconfigsscidhoppershoppernamepost.md)

&nbsp;&nbsp;建立指定的比對的票證。

<a id="ID4EFC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EHC"></a>


##### <a name="parent"></a>父系  

[配對的 Uri](atoc-reference-matchtickets.md)
