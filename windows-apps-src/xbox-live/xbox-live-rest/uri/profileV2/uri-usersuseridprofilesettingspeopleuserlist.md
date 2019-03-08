---
title: /users/{userId}/profile/settings/people/{userList}?settings={settings}
assetID: 0ba20eba-f0ab-28ab-61d3-b4f9e4c07bc5
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlist.html
description: " /users/{userId}/profile/settings/people/{userList}?settings={settings}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 24b58c817156a7c372a8e6acfab895e6b7c51207
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636963"
---
# <a name="usersuseridprofilesettingspeopleuserlistsettingssettings"></a>/users/{userId}/profile/settings/people/{userList}?settings={settings}
存取使用者的設定檔，或使用者，以使用者的 Moniker 的支援。 這些 Uri 的網域是`profile.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| userId| 字串| 可以是 'xuid(12345)'、 'gt(myGamertag)，' me'。| 
| userList| 字串| 若要取得設定的人員具名的清單。 目前，人是唯一支援的清單。| 
  
<a id="ID4E1B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{userId}/profile/settings/people/{userList})](uri-usersuseridprofilesettingspeopleuserlistget.md)

&nbsp;&nbsp;取得使用者的設定檔，或使用者，以使用者的 Moniker 的支援。
 
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>父系 

[設定檔的 Uri](atoc-reference-profiles.md)

 [設定檔 (JSON)](../../json/json-profile.md)

   