---
title: /handles/query
assetID: e00d31ad-b9ba-8e52-1333-83192eab0446
permalink: en-us/docs/xboxlive/rest/uri-handlesquery.html
description: " /handles/query"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eaa148972ce1e65056470a6c4082cb4e50de3f09
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598563"
---
# <a name="handlesquery"></a>/handles/query
支援 POST 作業來建立工作階段控制代碼的查詢。 

> [!NOTE] 
> 此 URI 使用 2015年多人遊戲，而且只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本搭配使用。  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>網域
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
此 URI 支援控制代碼的查詢。 不同於工作階段的查詢，也就是字串和批次查詢、 控制代碼的查詢會使用查詢處理器樣式。 最多 100 的控制代碼支援。  
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
無   
<a id="ID4EEB"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST （/處理/查詢）](uri-handlesquerypost.md)

&nbsp;&nbsp;建立工作階段控制代碼的查詢。

[POST (/handles/query?include=relatedInfo)](uri-handlesqueryincludepost.md)

&nbsp;&nbsp;建立包含相關的工作階段資訊的工作階段控制代碼的查詢。
 
<a id="ID4EQB"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ESB"></a>

 
##### <a name="parent"></a>父系 

[工作階段目錄的 Uri](atoc-reference-sessiondirectory.md)

   