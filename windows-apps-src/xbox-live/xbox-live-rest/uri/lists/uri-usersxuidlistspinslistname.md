---
title: /users/xuid(xuid)/lists/PINS/{listname}
assetID: b6421b11-fcd1-cfdb-c1fa-6cab3dab89d9
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistname.html
description: " /users/xuid(xuid)/lists/PINS/{listname}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f9610b400e9530f86e264cea30bfdfdd1b09c8d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627993"
---
# <a name="usersxuidxuidlistspinslistname"></a>/users/xuid(xuid)/lists/PINS/{listname}
在清單中存取項目。 這些 Uri 的網域是`eplists.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 字串| Xbox 使用者識別碼 (XUID)。| 
| listtype| 字串| 清單 （其使用方式和它的作用是） 的型別。 一律 「 釘選 」 這些相關方法。| 
| listname| 字串| 清單的名稱 （若要處理的給定 listtype 的清單）。 一律 「 XBLPins"的項目在 Pin 中。| 
  
<a id="ID4EGC"></a>

 
## <a name="valid-methods"></a>有效的方法

[DELETE](uri-usersxuidlistspinslistnamedelete.md)

&nbsp;&nbsp;從清單移除項目。

[GET](uri-usersxuidlistspinslistnameget.md)

&nbsp;&nbsp;傳回清單的內容。

[POST](uri-usersxuidlistspinslistnamepost.md)

&nbsp;&nbsp;項目插入清單的查詢字串參數為基礎的索引處**insertIndex**。

[PUT](uri-usersxuidlistspinslistnameput.md)

&nbsp;&nbsp;更新清單，以根據要求主體中的每個項目針對指定的索引中的項目。
 
<a id="ID4EZC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>父系 

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)

   