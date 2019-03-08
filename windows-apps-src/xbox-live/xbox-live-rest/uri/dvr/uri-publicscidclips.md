---
title: /public/scids/{scid}/clips
assetID: 55a1f0ae-08bb-6d1e-a1da-cbeb481c42ee
permalink: en-us/docs/xboxlive/rest/uri-publicscidclips.html
description: " /public/scids/{scid}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: db279d546780ed40158894f73ecb84687ef35ba6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627973"
---
# <a name="publicscidsscidclips"></a>/public/scids/{scid}/clips
存取公用的剪輯。 這個 URI 其實可以指定兩種形式`/public/scids/{scid}/clips`和`/public/titles/{titleId}/clips`。 如需詳細資訊，請參閱下方。 此 URI 的網域是`gameclipsmetadata.xboxlive.com`。
 
  * [URI 參數](#ID4E1)
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| 字串| 公用的剪輯的主要服務設定識別項。| 
| titleid| 字串| 公用的剪輯的 titleId。 無法為 SCID 相同的 URI 中指定。 如果指定，將用來查閱主要 SCID 中。| 
  
<a id="ID4E6B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/public/scids/{scid}/clips)](uri-publicscidclipsget.md)

&nbsp;&nbsp;列出公用的剪輯。
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>父系 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

   