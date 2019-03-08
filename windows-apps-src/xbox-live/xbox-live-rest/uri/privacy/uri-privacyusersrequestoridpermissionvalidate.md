---
title: /users/{requestorId}/permission/validate
assetID: 400a9721-bf43-76df-4cd1-9f2ae6ca5035
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidate.html
description: " /users/{requestorId}/permission/validate"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a062fd417bae37fd66c944e0e534ef7a50de5fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599363"
---
# <a name="usersrequestoridpermissionvalidate"></a>/users/{requestorId}/permission/validate
 
  * [URI 參數](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| requestorId| 字串| 必要。 執行此動作的使用者識別碼。 可能的值為<code>xuid({xuid})</code>和<code>me</code>。 這必須是登入的使用者。 範例值： <code>xuid(0987654321)</code>。| 
  
<a id="ID4ETB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidateget.md)

&nbsp;&nbsp;取得關於是否允許使用者執行指定的動作，與目標使用者是或否回應。

[POST (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidatepost.md)

&nbsp;&nbsp;取得一份是或否解答是否允許使用者執行的一組目標使用者的指定的動作。
 
<a id="ID4EAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ECC"></a>

   [隱私權 Uri](atoc-reference-privacyv2.md)

 [PermissionId 列舉](../../enums/privacy-enum-permissionid.md)

   