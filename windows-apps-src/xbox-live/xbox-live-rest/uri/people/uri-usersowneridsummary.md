---
title: /users/{ownerId}/summary
assetID: 63f8ed09-532d-381e-59e6-2849893df5bf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummary.html
description: " /users/{ownerId}/summary"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f8ad32fb2033c97a408ccb0f6cc6871b01caf5c5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617803"
---
# <a name="usersowneridsummary"></a>/users/{ownerId}/summary
從呼叫端的觀點來看的擁有者的相關摘要資料的存取。

  * [URI 參數](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| ownerId| 字串| 使用者正在存取的資源識別碼。 可能的值為"me"、 xuid({xuid}) 或 gt({gamertag})。 範例值： <code>me</code>， <code>xuid(2603643534573581)</code>， <code>gt(SomeGamertag)</code>|

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>有效的方法

[取得 (/users/ {ownerId} / s)](uri-usersowneridsummaryget.md)

&nbsp;&nbsp;取得擁有者相關的摘要資料，從呼叫端的觀點來看。

<a id="ID4E3B"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E5B"></a>


##### <a name="parent"></a>父系

[/users/{ownerId}/summary](uri-usersowneridsummaryget.md)
