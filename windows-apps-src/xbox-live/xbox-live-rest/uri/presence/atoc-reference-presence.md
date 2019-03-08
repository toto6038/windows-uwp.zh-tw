---
title: 顯示狀態 URI
assetID: 4ba44d9c-8615-cacc-2eee-7ff5e7c74383
permalink: en-us/docs/xboxlive/rest/atoc-reference-presence.html
description: " 顯示狀態 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a46ecd48c2b0bf523ab234a5f20cf9ed6669e75
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632603"
---
# <a name="presence-uris"></a>顯示狀態 URI
 
本節提供從 Xbox Live 服務的通用資源識別元 (URI) 位址和相關聯的超文字傳輸通訊協定 (HTTP) 方法的詳細*存在*。
 
在 Xbox 360 上、 Windows Phone 裝置，或 Windows 上執行的遊戲可以使用這項服務。
 
這些 Uri 的網域是 userpresence.xboxlive.com。
 
您可以使用即時活動 (RTA) 服務，就可訂閱使用者的目前狀態的變更。
 
<a id="ID4ERB"></a>

 
## <a name="in-this-section"></a>本節內容

[/users/batch](uri-usersbatch.md)

&nbsp;&nbsp;一批使用者的存取存在。

[/users/me](uri-usersme.md)

&nbsp;&nbsp;存取目前使用者的目前狀態。

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

&nbsp;&nbsp;存取 PresenceRecord 適合我的群組。

[/users/xuid({xuid})](uri-usersxuid.md)

&nbsp;&nbsp;存取另一個使用者或用戶端的目前狀態。

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

&nbsp;&nbsp;存取標題或標題的使用者的存在。

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

&nbsp;&nbsp;存取群組 PresenceRecord。

[/users/xuid({xuid})/groups/{moniker}/broadcasting](uri-usersxuidgroupsmonikerbroadcasting.md)

&nbsp;&nbsp;存取出現在 URI 中 XUID 與相關群組 moniker 所指定的廣播使用者的目前狀態記錄。

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;存取 URI 中會出現 XUID 與相關的群組 moniker 所指定的廣播使用者計數。
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>父系 

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)

   