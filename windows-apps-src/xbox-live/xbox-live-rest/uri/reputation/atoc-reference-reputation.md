---
title: 評價 URI
assetID: d1cb76c0-86a4-8c51-19f6-5f223b517d46
permalink: en-us/docs/xboxlive/rest/atoc-reference-reputation.html
description: " 評價 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 93d6d6e6acfd8fa39bd9d26c87ed99362d2c88d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614003"
---
# <a name="reputation-uris"></a>評價 URI
 
本節提供從 Xbox Live 服務的通用資源識別元 (URI) 位址和相關聯的超文字傳輸通訊協定 (HTTP) 方法的詳細**Microsoft.Xbox.Services.Social.ReputationService**. Uri 的信譽的網域是 reputation.xboxlive.com。 可能是一般的 URI 表示式 https://reputation.xboxlive.com/users/xuid(2533274790412952)/feedback。 
 
信譽服務會使用意見反應，如中所述[意見反應 (JSON)](../../json/json-feedback.md)來計算信譽打分數。 此分數會儲存在金鑰 ReputationOverall 底下為該使用者的 [統計資料] 區域中。 如需有關如何擷取使用者統計資料的詳細資訊，請參閱 <<c0> [ 取得 (/users/xuid({xuid})/scids/{scid}/stats)](../userstats/uri-usersxuidscidsscidstatsget.md)。 
 
在所有平台上的遊戲可以使用信譽服務。
 
<a id="ID4EMB"></a>

 
## <a name="in-this-section"></a>本節內容

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

&nbsp;&nbsp;如果您想要在您的遊戲，而不被使用命令介面中新增意見反應 選項，請使用從您的標題。

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

&nbsp;&nbsp;項目的服務使用您的標題介面外的批次形式傳送意見反應。

[/users/me/resetreputation](uri-usersmeresetreputation.md)

&nbsp;&nbsp;可讓強制執行小組，來存取目前使用者的信譽分數。

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)

&nbsp;&nbsp;完全重設的測試使用者的信譽資料。 僅供測試。

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

&nbsp;&nbsp;可讓強制執行小組，來存取指定的使用者的信譽分數。
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>父系 

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)

   