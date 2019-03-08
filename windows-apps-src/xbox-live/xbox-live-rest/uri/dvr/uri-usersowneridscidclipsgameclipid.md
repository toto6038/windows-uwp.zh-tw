---
title: /users/{ownerId}/scids/{scid}/clips/{gameClipId}
assetID: 49b68418-71f1-c5a2-3a9b-869fd1fa663c
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipid.html
description: " /users/{ownerId}/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7ea92e89d54df17e8d82084d840a7ee9ef7d032
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658333"
---
# <a name="usersowneridscidsscidclipsgameclipid"></a>/users/{ownerId}/scids/{scid}/clips/{gameClipId}
如果已知找出它的所有識別碼，請從系統存取單一的遊戲多媒體項目。 這些 uri 的網域`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。
 
  * [URI 參數](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| ownerId| 字串| 正在存取的資源之使用者的使用者身分識別。 支援的格式: 「 我 」 或 「 xuid(123456789)"。 最大長度：16.| 
| scid| 字串| 服務組態正在存取的資源識別碼。 必須符合 SCID 的已驗證的使用者。| 
| gameClipId| 字串| 遊戲剪輯所存取之資源的識別碼。| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})](uri-usersowneridscidclipsgameclipidget.md)

&nbsp;&nbsp;如果已知找出它的所有識別碼，請從系統取得單一的遊戲多媒體項目。
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>父系 

[遊戲 DVR Uri](atoc-reference-dvr.md)

   