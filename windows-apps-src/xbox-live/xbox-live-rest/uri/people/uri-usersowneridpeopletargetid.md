---
title: /users/{ownerId}/people/{targetid}
assetID: 9dd19e75-3b48-d7e0-fc65-6760c15ddf62
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetid.html
description: " /users/{ownerId}/people/{targetid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6238996abdeaca9b7a9a7a20d3f1ae9702e95a73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661743"
---
# <a name="usersowneridpeopletargetid"></a>/users/{ownerId}/people/{targetid}
存取個人的目標識別碼從呼叫者的使用者集合。 這些 Uri 的網域是`social.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| ownerId| 字串| 使用者正在存取的資源識別碼。 必須符合已驗證的使用者。 可能的值為"me"、 xuid({xuid}) 或 gt({gamertag})。| 
| targetid| 字串| 正在擷取其資料擁有者的人員清單，Xbox 使用者識別碼 (XUID) 或玩家代號從使用者的識別碼。 範例值： xuid(2603643534573581)、 gt(SomeGamertag)。| 
  
<a id="ID4EQB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-usersowneridpeopletargetidget.md)

&nbsp;&nbsp;取得依目標識別碼個人從呼叫者的使用者集合。
 
<a id="ID4E1B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E3B"></a>

 
##### <a name="parent"></a>父系 

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)

   