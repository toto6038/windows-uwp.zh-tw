---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting
assetID: cf8319f6-46a2-b263-ea4c-f1ce403b571b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcasting.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 98eaa60204e3c98eb1b09a13372f7b0c084a6608
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651463"
---
# <a name="usersxuidxuidgroupsmonikerbroadcasting"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting
存取出現在 URI 中 XUID 與相關群組 moniker 所指定的廣播使用者的目前狀態記錄。 這些 Uri 的網域是`userpresence.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 字串| Xbox 使用者識別碼 (XUID) 與群組中 XUIDs 相關的使用者。| 
| moniker| 字串| 定義使用者群組的字串。 目前唯一接受的 moniker 是 「 人 」，以大寫 'P'。| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting )](uri-usersxuidgroupsmonikerbroadcastingget.md)

&nbsp;&nbsp;擷取與 URI 中會出現 XUID 群組 moniker 所指定的廣播使用者的目前狀態記錄。
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>父系 

[目前狀態的 Uri](atoc-reference-presence.md)

   