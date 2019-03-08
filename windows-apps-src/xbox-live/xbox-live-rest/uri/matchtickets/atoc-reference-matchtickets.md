---
title: 配對 URI
assetID: 667b02a9-6f34-8165-001b-ee8782575202
permalink: en-us/docs/xboxlive/rest/atoc-reference-matchtickets.html
description: " 配對 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c52abca7ed49d4a5e14520095ae944938b86f093
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590463"
---
# <a name="matchmaking-uris"></a>配對 URI
 
本章節會提供詳細的通用資源識別元 (URI) 位址和相關聯的超文字傳輸通訊協定 (HTTP) 方法，從 Xbox Live 服務的服務配對。 
 
<a id="ID4E6"></a>

 
## <a name="domain"></a>網域
momatch.xboxlive.com  
<a id="ID4EEB"></a>

 
## <a name="service-version"></a>服務版本
 
這些 HTTP/REST Uri 的呼叫端必須傳遞值 103 或更新版本或 Xbl-合約-更新版本，指定的服務版本的娛樂探索服務 (EDS) 的 HTTP 標頭。 
  
<a id="ID4ELB"></a>

 
## <a name="system-objects-and-properties"></a>系統物件和屬性
 
目前，所有服務組態的配對，就會都發生以手動的方式，使用的服務組態一部分[Xbox 開發人員入口網站 (XDP)](https://xdp.xboxlive.com)或是[合作夥伴中心](https://partner.microsoft.com/dashboard)。 配對中的一些資訊也會反映為 MPSD 定義物件中。 
 
主要用來設定配對的 JSON 物件定義於[MatchTicket (JSON)](../../json/json-matchticket.md)並[HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)。 請注意，所有符合票證必須定義**ticketSessionRef**物件，提供包含要與其他人進行比對的球員的播放程式的多人遊戲工作階段的參考。 
  
<a id="ID4EBC"></a>

 
## <a name="in-this-section"></a>本節內容

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

&nbsp;&nbsp;支援 POST 作業，以建立比對的票證。 

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

&nbsp;&nbsp;支援 「 取得 」 作業來擷取 hopper 的統計資料。

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

&nbsp;&nbsp;支援刪除作業的相符項目票證。
 
<a id="ID4ENC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EPC"></a>

   [MatchTicket (JSON)](../../json/json-matchticket.md)

 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)

 [工作階段目錄的 Uri](../sessiondirectory/atoc-reference-sessiondirectory.md)

  
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>父系 

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)

   