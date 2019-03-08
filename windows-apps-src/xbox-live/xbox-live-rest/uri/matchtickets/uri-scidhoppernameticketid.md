---
title: /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}
assetID: 25deb7fe-859c-01d2-d14f-455a36c08a7c
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketid.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9722d06af64c5fd24f53485a1bcfe765f89b08cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627313"
---
# <a name="serviceconfigsscidhoppershoppernameticketsticketid"></a>/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}

支援刪除作業的相符項目票證。

> [!IMPORTANT]
> 此 URI 是針對合約 103 或更新版本，搭配使用，因此需要 X Xbl-合約版本標頭項目：103 或更新版本上的每個要求。

<a id="ID4ER"></a>


## <a name="domain"></a>網域
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>備註
此 URI 支援值 xuid、 gt、 和我的目標使用者的組態中的擁有者識別碼。  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務組態 (SCID) 工作階段識別碼。|
| name| 字串| Hopper 名稱。|
| ticketId| GUID| 票證識別碼。|

<a id="ID4EJC"></a>


## <a name="valid-methods"></a>有效的方法

[DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})](uri-scidhoppernameticketiddelete.md)

&nbsp;&nbsp;移除符合票證。

<a id="ID4ETC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EVC"></a>


##### <a name="parent"></a>父系  

[配對的 Uri](atoc-reference-matchtickets.md)
