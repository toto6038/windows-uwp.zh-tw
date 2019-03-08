---
title: /users/{ownerId}/clips
assetID: cc200b89-dc63-9ab5-b037-7a910f46dae9
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclips.html
description: " /users/{ownerId}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cd711777bcdf0b073dd0821222049b03aa35a23c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599513"
---
# <a name="usersowneridclips"></a>/users/{ownerId}/clips
存取使用者的剪輯清單。 這些 uri 的網域`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。
 
  * [URI 參數](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| ownerId| 字串| 正在存取的資源之使用者的使用者身分識別。 支援的格式: 「 我 」 或 「 xuid(123456789)"。 最大長度：16.| 
  
<a id="ID4EVB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{ownerId}/clips)](uri-usersowneridclipsget.md)

&nbsp;&nbsp;擷取使用者的剪輯清單。
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>父系 

[遊戲 DVR Uri](atoc-reference-dvr.md)

   