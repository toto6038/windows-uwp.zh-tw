---
title: /users/{ownerId}/people/avoid
assetID: 13dc3a10-ed04-4600-3afb-aa95a4139a06
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoid.html
description: " /users/{ownerId}/people/avoid"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d0ae745f825d6afda87167859b12bcc52b899f18
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607483"
---
# <a name="usersowneridpeopleavoid"></a>/users/{ownerId}/people/avoid
存取使用者的避免清單

  * [URI 參數](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| ownerId| 字串| 必要。 使用者正在存取的資源識別碼。 可能的值為<code>xuid({xuid})</code>。 必須是已驗證的使用者。 範例值： <code>xuid(2603643534573581)</code>。 大小上限： 無。 |

<a id="ID4ERB"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/users/{ownerId}/people/avoid)](uri-privacyusersxuidpeopleavoidget.md)

&nbsp;&nbsp;取得使用者避免清單。

<a id="ID4E2B"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E4B"></a>


##### <a name="parent"></a>父系

[隱私權 Uri](atoc-reference-privacyv2.md)
