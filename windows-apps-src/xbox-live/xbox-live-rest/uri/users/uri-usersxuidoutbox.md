---
title: /users/xuid({xuid})/outbox
assetID: 0b66b885-15ff-be55-f8be-e6e9d85d087e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutbox.html
description: " /users/xuid({xuid})/outbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 88f3f3753aeac99db0a8a53e0a2ddde21d034ac5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607543"
---
# <a name="usersxuidxuidoutbox"></a>/users/xuid({xuid})/outbox
提供僅限傳送的使用者存取的訊息寄件匣的 Xbox LIVE 服務。 這些 Uri 的網域是`msg.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數 
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid | 不帶正負號的 64 位元整數 | Xbox 使用者識別碼 (XUID) 正提出要求者的播放程式。 | 
  
<a id="ID4EXB"></a>

 
## <a name="valid-methods"></a>有效的方法 

[POST (/users/xuid({xuid})/outbox)](uri-usersxuidoutboxpost.md)

&nbsp;&nbsp;將指定的訊息傳送至收件者清單中。 
 
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>父系  

[使用者的 Uri](atoc-reference-users.md)

   