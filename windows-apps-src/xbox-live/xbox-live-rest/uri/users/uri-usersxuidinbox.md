---
title: /users/xuid({xuid})/inbox
assetID: 352740c6-42e2-0000-495d-bf384dc3e941
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinbox.html
description: " /users/xuid({xuid})/inbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8ded70b32dfd291d17a43a1741b26710f681a397
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640893"
---
# <a name="usersxuidxuidinbox"></a>/users/xuid({xuid})/inbox
提供給使用者的存取權的 Xbox LIVE 服務的訊息收件匣。 這些 Uri 的網域是`msg.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數 
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid | 不帶正負號的 64 位元整數 | Xbox 使用者識別碼 (XUID) 正提出要求者的播放程式。 | 
| messageId | string[50] | 正在擷取或刪除訊息的識別碼。 | 
  
<a id="ID4EDC"></a>

 
## <a name="valid-methods"></a>有效的方法 

[GET (/users/xuid({xuid})/inbox)](uri-usersxuidinboxget.md)

&nbsp;&nbsp;從服務中擷取指定的數目的使用者訊息摘要。 

[DELETE (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageiddelete.md)

&nbsp;&nbsp;刪除使用者的收件匣中的使用者訊息。

[GET (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageidget.md)

&nbsp;&nbsp;擷取特定的使用者訊息，將它標示為已讀取服務的詳細的訊息文字。 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>父系  

[使用者的 Uri](atoc-reference-users.md)

   