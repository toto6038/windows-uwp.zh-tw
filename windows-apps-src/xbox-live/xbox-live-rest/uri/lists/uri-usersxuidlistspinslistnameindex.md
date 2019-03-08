---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b56b563c72c206340aa2c1ce9f73aa8dfe50809d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641223"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
移動清單中的項目。 這些 Uri 的網域是`eplists.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數 
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| XUID| 字串| XUID 的使用者。| 
| listname| 字串| 要操作之清單的名稱。| 
| index| 字串| 指定要移動的項目目前的索引。 如果索引值為零或正整數，這是指的項目，目前的索引，並呼叫的要求主體應為空白。 不過，如果索引值為"-1"，必須指定要移動的項目 ItemId 或提供者/ProviderID 呼叫的要求主體中。 | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;將清單項目移至不同的位置，在清單中。
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>父系 

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)

   