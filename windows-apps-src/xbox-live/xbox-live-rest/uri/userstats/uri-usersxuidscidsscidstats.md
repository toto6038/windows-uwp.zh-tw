---
title: /users/xuid({xuid})/scids/{scid}/stats
assetID: 3cf9ffd4-9a8b-2658-402b-2e933f7f6f1b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstats.html
description: " /users/xuid({xuid})/scids/{scid}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 53a6c7bb0e7390b024b01e221d8061316a80509e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650413"
---
# <a name="usersxuidxuidscidsscidstats"></a>/users/xuid({xuid})/scids/{scid}/stats
存取服務組態，領域設定為代表指定之使用者的使用者統計資料名稱的逗號分隔清單。 這些 Uri 的網域是`userstats.xboxlive.com`。
 
  * [URI 參數](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| GUID| Xbox 使用者識別碼 (XUID) 代表其存取的服務組態上的使用者。| 
| scid| GUID| 服務組態，其中包含所存取之資源的識別碼。| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-usersxuidscidsscidstatsget.md)

&nbsp;&nbsp;取得代表指定之使用者的使用者統計資料名稱的逗號分隔清單範圍的服務組態。

[取得值的中繼資料](uri-usersxuidscidsscidstatsgetvaluemetadata.md)

&nbsp;&nbsp;取得一份指定的統計資料，包括統計資料值，指定的服務組態中的使用者相關聯的中繼資料。
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>父系 

[使用者統計資料 Uri](atoc-reference-userstats.md)

   