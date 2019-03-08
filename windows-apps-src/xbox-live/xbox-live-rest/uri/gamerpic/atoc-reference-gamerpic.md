---
title: 玩家圖示 URI
assetID: 811ab696-c433-aa54-90d8-66614ad09901
permalink: en-us/docs/xboxlive/rest/atoc-reference-gamerpic.html
description: " 玩家圖示 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a8ba79784ed73ae62e7fe8d65c626c3ebc6003a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618383"
---
# <a name="gamerpic-uris"></a>玩家圖示 URI
 
本節提供有關 Gamerpic 通用資源識別元 (URI) 位址和相關聯的超文字傳輸通訊協定 (HTTP) 方法的詳細資料從 Xbox Live 服務*gamerpics*。
 
這些 Uri 的網域是`gamerpics.xboxlive.com`。
 
Gamerpic 服務的設計可讓使用者更多的個人化選項，藉由允許產生 gamerpic 其遊戲的字元之使用者的能力授與標題 （遊戲的字元，在此案例中是指在遊戲 protagonist; 它可能是個人汽車、 的太空船或使用者控制標題中的任何其他實體)。
 
產生標題 gamerpic 的基本流程如下所示：
 
   * 標題會提供能夠建立自己的遊戲中字元的映像中的使用者。 
     * 如果沒有，標題可以再訊息它們並沒有適當的權限的使用者。
     * 如果使用者沒有權限，使用者可以繼續建立其字元 gamerpic。
  
   * 使用者建立映像及標題傳送給 gamerpic 服務，1080 x 1080.png 檔案。
   * 此服務會將影像儲存，並為使用者的新 gamerpic 設定的影像。
   * 呼叫使用者的 gamerpic 任何體驗會收到更新的映像。
  
設定標題 gamerpic 的能力會受到強制執行專用權限 (211)。 如果強制執行撤銷權限，使用者將無法儲存標題 gamerpic，，則服務會傳回 403。 標題應該呼叫 CheckPrivilege 來確認使用者可以共用的內容 (priv 211)。
 
目前，若要使用這項服務，您的標題必須是列入允許清單。 若要要求核准，傳送電子郵件`slsgamerpics@microsoft.com`。
 
<a id="ID4EGC"></a>

 
## <a name="in-this-section"></a>本節內容

[/users/me/gamerpic](uri-usersmegamerpic.md)

&nbsp;&nbsp;存取 1080 x 1080 gamerpic。
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>父系 

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)

   