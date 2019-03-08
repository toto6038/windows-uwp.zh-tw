---
title: /users/xuid({xuid})/devices/current/titles/current
assetID: f149c68b-9874-e348-4e1d-6acf5d305c49
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrent.html
description: " /users/xuid({xuid})/devices/current/titles/current"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0b5c17f1791d69ca8a4c902b6c37736c4da28a31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645663"
---
# <a name="usersxuidxuiddevicescurrenttitlescurrent"></a>/users/xuid({xuid})/devices/current/titles/current
存取標題或標題的使用者的存在。 這些 Uri 的網域是`userpresence.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的目標使用者。| 
  
<a id="ID4EUB"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentdelete.md)

&nbsp;&nbsp;移除存在的結尾標題，而不是等待[PresenceRecord](../../json/json-presencerecord.md)到期。

[POST (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentpost.md)

&nbsp;&nbsp;更新使用者的目前狀態的標題。
 
<a id="ID4EBC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EDC"></a>

 
##### <a name="parent"></a>父系 

[目前狀態的 Uri](atoc-reference-presence.md)

   