---
title: /handles/{handleId}
assetID: 5b722d3e-fe80-fec5-a26b-8b3db6422004
permalink: en-us/docs/xboxlive/rest/uri-handleshandleid.html
description: " /handles/{handleId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a312d3744e96755a899d73307a47c01e3dc79fd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632633"
---
# <a name="handleshandleid"></a>/handles/{handleId}
支援 DELETE 和 GET 作業適用於識別項所指定的工作階段控制代碼。 

> [!NOTE] 
> 此 URI 使用 2015年多人遊戲，而且只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本搭配使用。  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>網域
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | 
| handleId| GUID| 工作階段的控制代碼的唯一識別碼。| 
  
<a id="ID4ERB"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE (/handles/{handleId})](uri-handleshandleiddelete.md)

&nbsp;&nbsp;刪除控制代碼識別碼。 所指定的控制代碼

[取得 (/handles/ {控制代碼-識別碼})](uri-handleshandleidget.md)

&nbsp;&nbsp;擷取控制代碼識別碼。 所指定的控制代碼
 
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>父系 

[工作階段目錄的 Uri](atoc-reference-sessiondirectory.md)

   