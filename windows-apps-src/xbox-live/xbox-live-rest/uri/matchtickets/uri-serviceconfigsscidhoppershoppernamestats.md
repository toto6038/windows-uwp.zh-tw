---
title: /serviceconfigs/{scid}/hoppers/{name}/stats
assetID: 56bb4398-445b-e8c5-a4ce-1651576ee7e7
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestats.html
description: " /serviceconfigs/{scid}/hoppers/{name}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 04376505b76296e052ea431f2a4e5fcfeac7b9e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613933"
---
# <a name="serviceconfigsscidhoppersnamestats"></a>/serviceconfigs/{scid}/hoppers/{name}/stats

支援 「 取得 」 作業來擷取 hopper 的統計資料。

> [!IMPORTANT]
> 此 URI 是針對合約 103 或更新版本，搭配使用，因此需要 X Xbl-合約版本標頭項目：103 或更新版本上的每個要求。

<a id="ID4ER"></a>


## <a name="domain"></a>網域
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>備註
此 URI 支援值 xuid、 gt、 和我的目標使用者的組態中的擁有者識別碼。 只有票證的建立者可以刪除該票證，或擷取 URI 的狀態。  
<a id="ID4E6"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務組態 (SCID) 工作階段識別碼。|
| name| 字串| Hopper 名稱。|

<a id="ID4EEC"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](uri-serviceconfigsscidhoppershoppernamestatsget.md)

&nbsp;&nbsp;Hopper 取得統計資料。

<a id="ID4EQC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ESC"></a>


##### <a name="parent"></a>父系  

[配對的 Uri](atoc-reference-matchtickets.md)
