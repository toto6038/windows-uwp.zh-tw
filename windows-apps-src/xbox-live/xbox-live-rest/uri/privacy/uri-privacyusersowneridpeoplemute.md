---
title: /users/{ownerId}/people/mute
assetID: efb929d8-79a7-83f0-c348-c92ced42bc05
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemute.html
description: " /users/{ownerId}/people/mute"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86010d11ff0a7ebe188766f51bc3160a4f2befa4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641543"
---
# <a name="usersowneridpeoplemute"></a>/users/{ownerId}/people/mute
存取使用者的靜音清單。

  * [URI 參數](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| ownerId| 字串| 必要。 使用者正在存取的資源識別碼。 可能的值為"me" <code>xuid({xuid})</code>，或 gt({gamertag})。 必須是已驗證的使用者。 範例值： <code>xuid(2603643534573581)</code>， <code>gt(SomeGamertag)</code>。 大小上限： 無。 |

<a id="ID4ETB"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/users/{ownerId}/people/mute)](uri-privacyusersowneridpeoplemuteget.md)

&nbsp;&nbsp;取得使用者的 [靜音] 清單。

<a id="ID4E4B"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E6B"></a>


##### <a name="parent"></a>父系

[隱私權 Uri](atoc-reference-privacyv2.md)
