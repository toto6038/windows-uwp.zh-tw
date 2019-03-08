---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting/count
assetID: 535c8d46-7001-c31e-3e9d-82ad275095ae
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingcount.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting/count"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a39bc9c3302ba26949700774997355a216fe70d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651353"
---
# <a name="usersxuidxuidgroupsmonikerbroadcastingcount"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting/count
存取 URI 中會出現 XUID 與相關的群組 moniker 所指定的廣播使用者計數。 這些 Uri 的網域是`userpresence.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 字串| Xbox 使用者識別碼 (XUID) 與群組中 XUIDs 相關的使用者。| 
| moniker| 字串| 定義使用者群組的字串。 目前唯一接受的 moniker 是 「 人 」，以大寫 'P'。| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )](uri-usersxuidgroupsmonikerbroadcastingcountget.md)

&nbsp;&nbsp;擷取與 URI 中會出現 XUID 群組 moniker 所指定的廣播使用者計數。
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>父系 

[目前狀態的 Uri](atoc-reference-presence.md)

   