---
title: /serviceconfigs/{scid}/batch
assetID: eb1b510f-d92e-ae9b-a3e6-0edf58b4f075
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatch.html
description: " /serviceconfigs/{scid}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 208ee92106563372dd4d92a8c800cc08f513e8c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589713"
---
# <a name="serviceconfigsscidbatch"></a>/serviceconfigs/{scid}/batch
支援在服務組態的識別項的層級的批次查詢的 POST 作業。

> [!IMPORTANT]
> 這個方法由 2015年多人遊戲，並只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本，搭配使用，而且需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

<a id="ID4ER"></a>


## <a name="domain"></a>網域
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務設定識別項 (SCID)。 工作階段識別項的第 1 部分。|

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>有效的方法

[POST (/serviceconfigs/ {scid} / 批次)](uri-serviceconfigsscidbatchpost.md)

&nbsp;&nbsp;建立批次查詢中，於多個 Xbox 使用者識別碼的服務組態。

<a id="ID4E3B"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E5B"></a>


##### <a name="parent"></a>父系

[工作階段目錄的 Uri](atoc-reference-sessiondirectory.md)
