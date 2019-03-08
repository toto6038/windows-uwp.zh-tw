---
title: /users/me/scids/{scid}/clips/{gameClipId}
assetID: f5bead69-4fc9-f551-39cb-c8754645ac88
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipid.html
description: " /users/me/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3cbe2d2c996b466fd94287129f1add0868f05b05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661773"
---
# <a name="usersmescidsscidclipsgameclipid"></a>/users/me/scids/{scid}/clips/{gameClipId}
存取遊戲的剪輯資料和中繼資料。 這些 uri 的網域`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。
 
  * [URI 參數](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| 字串| 服務組態正在存取的資源識別碼。 必須符合 SCID 的已驗證的使用者。| 
| gameClipId| 字串| 遊戲剪輯所存取之資源的識別碼。| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE (/users/me/scids/{scid}/clips/{gameClipId})](uri-usersmescidclipsgameclipiddelete.md)

&nbsp;&nbsp;刪除遊戲的剪輯

[POST (/users/me/scids/{scid}/clips/{gameClipId})](uri-usersmescidclipsgameclipidpost.md)

&nbsp;&nbsp;更新遊戲剪輯使用者專屬資料的中繼資料。
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>父系 

[遊戲 DVR Uri](atoc-reference-dvr.md)

   