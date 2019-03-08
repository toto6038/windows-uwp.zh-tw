---
title: 工作階段目錄 URI
assetID: e3ba951d-b21f-0014-c358-2603d549d118
permalink: en-us/docs/xboxlive/rest/atoc-reference-sessiondirectory.html
description: " 工作階段目錄 URI"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9492ff3272af830404a546c9b01d62178adbac96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651193"
---
# <a name="session-directory-uris"></a>工作階段目錄 URI

本章節提供詳細的通用資源識別元 (URI) 位址和相關聯的超文字傳輸通訊協定 (HTTP) 方法從 Xbox Live 服務的多人遊戲工作階段目錄 (MPSD)。


> [!NOTE] 
> 適用於執行 Xbox 360 上、 Windows Phone 裝置上，或在 Xbox.com 遊戲的項目可以使用工作階段目錄 Uri。  


  * [網域](#ID4EUB)
  * [服務版本](#ID4EZB)
  * [系統物件和屬性](#ID4EAC)
  * [控制代碼](#ID4EBE)

<a id="ID4EUB"></a>


## <a name="domain"></a>網域
sessiondirectory.xboxlive.com  
<a id="ID4EZB"></a>


## <a name="service-version"></a>服務版本

下列 REST Uri 的呼叫端必須傳遞值 104/105 或更新版本或 Xbl-合約-更新版本，指定的服務版本的娛樂探索服務 (EDS) 的 HTTP 標頭。

<a id="ID4EAC"></a>


## <a name="system-objects-and-properties"></a>系統物件和屬性

設定其工作階段與範本，MPSD，請使用 directory 會強制執行，並將解譯的固定結構描述符合工作階段的 JSON 物件的數目。 在各種不同的工作階段目錄的 Uri 所支援的方法呼叫中，這些物件會驗證和合併，根據支援的結構描述。 多人遊戲的組態相關聯的主要 JSON 物件如下：

   *  [MultiplayerActivityDetails (JSON)](../../json/json-multiplayeractivitydetails.md)
   *  [MultiplayerSession (JSON)](../../json/json-multiplayersession.md)
   *  [MultiplayerSessionReference (JSON)](../../json/json-multiplayersessionreference.md)
   *  [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)


相關聯特別關心遊戲的 JSON 物件︰

   *  [GameMessage (JSON)](../../json/json-gamemessage.md)
   *  [GameResult (JSON)](../../json/json-gameresult.md)
   *  [GameSession (JSON)](../../json/json-gamesession.md)
   *  [GameSessionSummary (JSON)](../../json/json-gamesessionsummary.md)


<a id="ID4EBE"></a>


## <a name="handles"></a>控制代碼

只有 2015年多，工作階段可以存取透過工作階段控制代碼。 已新增數個 Uri 來提供功能來支援控制代碼。  
<a id="ID4EFE"></a>


## <a name="in-this-section"></a>本節內容

[/handles](uri-handles.md)

&nbsp;&nbsp;支援 POST 作業來設定顯示在 Xbox One 儀表板的使用者體驗，並邀請工作階段的成員，如有必要，則使用者的目前活動的工作階段。

[/handles/{handleId}](uri-handleshandleid.md)

&nbsp;&nbsp;支援 DELETE 和 GET 作業適用於識別項所指定的工作階段控制代碼。

[/handles/{handleId}/session](uri-handleshandleidsession.md)

&nbsp;&nbsp;支援 PUT 和 GET 作業的工作階段中，使用控制代碼取值。

[/handles/query](uri-handlesquery.md)

&nbsp;&nbsp;支援 POST 作業來建立工作階段控制代碼的查詢。

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)

&nbsp;&nbsp;支援在服務組態的識別項的層級的批次查詢的 POST 作業。

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)

&nbsp;&nbsp;支援取得作業以擷取一組工作階段的文件。

[/serviceconfigs/{scid}/sessiontemplates](uri-serviceconfigsscidsessiontemplates.md)

&nbsp;&nbsp;支援取得作業以擷取一組 MPSD 工作階段範本。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}](uri-serviceconfigsscidsessiontemplatessessiontemplatename.md)

&nbsp;&nbsp;支援取得作業以擷取一組工作階段範本名稱。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)

&nbsp;&nbsp;支援工作階段範本層級建立批次查詢的 POST 作業。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)

&nbsp;&nbsp;支援擷取工作階段的範本，以及指定的範本名稱的一組 GET 作業。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)

&nbsp;&nbsp;支援建立和擷取工作階段上的 PUT 和 GET 作業。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)

&nbsp;&nbsp;支援刪除作業，移除指定的工作階段的成員。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)

&nbsp;&nbsp;支援刪除作業，若要移除的工作階段的成員。

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)

&nbsp;&nbsp;支援刪除作業，若要移除指定的伺服器工作階段。

<a id="ID4ESF"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EUF"></a>

   [配對的 Uri](../matchtickets/atoc-reference-matchtickets.md)


<a id="ID4E1F"></a>


##### <a name="parent"></a>父系

[統一資源識別元 (URI) 參考](../atoc-xboxlivews-reference-uris.md)
