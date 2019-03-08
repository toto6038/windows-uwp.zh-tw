---
title: /handles/{handleId}/session
assetID: 4ed2dcf5-5d1f-91ce-4a3f-eb3ba68727bf
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsession.html
description: " /handles/{handleId}/session"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7b6990917437c22dd4d9282492e2a0eab37893b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628143"
---
# <a name="handleshandleidsession"></a>/handles/{handleId}/session
支援 PUT 和 GET 作業的工作階段中，使用控制代碼取值。 

> [!NOTE] 
> 此 URI 使用 2015年多人遊戲，而且只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本搭配使用。  

 

> [!NOTE] 
> 此 URI 是目前只能從外部存取 Xbox One 主控台與使用服務識別碼的伺服器。  

 
<a id="ID4ES"></a>

 
## <a name="domain"></a>網域
sessiondirectory.xboxlive.com  
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | 
| handleId| GUID| 工作階段的控制代碼的唯一識別碼。| 
  
<a id="ID4ESB"></a>

 
## <a name="valid-methods"></a>有效的方法

[取得 (/handles/ {handleId} / 工作階段)](uri-handleshandleidsessionget.md)

&nbsp;&nbsp;取得工作階段物件，指定控制代碼識別碼。 

[PUT (/handles/ {控制代碼-識別碼} / 工作階段)](uri-handleshandleidsessionput.md)

&nbsp;&nbsp;建立或更新的控制代碼取值的工作階段。
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>父系 

[工作階段目錄的 Uri](atoc-reference-sessiondirectory.md)

   