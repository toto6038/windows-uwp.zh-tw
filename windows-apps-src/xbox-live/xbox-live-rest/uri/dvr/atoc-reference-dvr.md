---
title: 遊戲 DVR URI
assetID: 472f705e-bf28-7894-b1ba-80933d8746a6
permalink: en-us/docs/xboxlive/rest/atoc-reference-dvr.html
description: " 遊戲 DVR URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b4bfd6e51efce4c6ec85db99a10a44a776dcb840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617843"
---
# <a name="game-dvr-uris"></a>遊戲 DVR URI
 
本節提供從 Xbox Live 服務的通用資源識別元 (URI) 位址和相關聯的超文字傳輸通訊協定 (HTTP) 方法的詳細*遊戲 DVR*。
 
只有主控台可以記錄遊戲美工圖案，但可以存取任何裝置都可以顯示剪輯。
 
根據 URI 有問題的函式，這些 Uri 的網域是：
 
   *  gameclipsmetadata.xboxlive.com 
   *  gameclipstransfer.xboxlive.com 
  
<a id="ID4EZB"></a>

 
## <a name="in-this-section"></a>本節內容

[/public/scids/{scid}/clips](uri-publicscidclips.md)

&nbsp;&nbsp;存取公用的剪輯。 這個 URI 其實可以指定兩種形式`/public/scids/{scid}/clips`和`/public/titles/{titleId}/clips`。 如需詳細資訊，請參閱下方。

[/{uri}](uri-uri.md)

&nbsp;&nbsp;存取遊戲的剪輯資料。

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

&nbsp;&nbsp;存取初始上傳要求。

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

&nbsp;&nbsp;存取遊戲的剪輯資料和中繼資料。

[/users/{ownerId}/clips](uri-usersowneridclips.md)

&nbsp;&nbsp;存取使用者的剪輯清單。

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

&nbsp;&nbsp;如果已知找出它的所有識別碼，請從系統存取單一的遊戲多媒體項目。
 
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>父系 

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)

   