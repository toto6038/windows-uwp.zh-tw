---
title: /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
assetID: 0983dad0-59b7-45b7-505d-603e341fe0cc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeople.html
description: " /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 85a6470a64ceef3b154384d1ca859fb28733aad3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632573"
---
# <a name="usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
存取社交 （等級） 排行榜。
這些 Uri 的網域是`leaderboards.xboxlive.com`。

  * [URI 參數](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| xuid| 字串| 使用者的識別碼。|
| scid| 字串| 服務組態，其中包含所存取之資源的識別碼。|
| statname| 字串| 正在存取的使用者統計資料資源的唯一識別碼。|
| 所有\|最愛| 列舉| 是否要排名的統計資料的值 （分數），目前使用者的所有已知的連絡人，或指定為最愛的人的該使用者的連絡人。|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite})](uri-usersxuidscidstatnamepeopleget.md)

&nbsp;&nbsp;傳回的統計資料值 （分數） 是已知的所有連絡人，目前的使用者或指定為最愛的人的該使用者的連絡人的排名社交排行榜。

<a id="ID4EYC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E1C"></a>


##### <a name="parent"></a>父系

[排行榜 Uri](atoc-reference-leaderboard.md)
