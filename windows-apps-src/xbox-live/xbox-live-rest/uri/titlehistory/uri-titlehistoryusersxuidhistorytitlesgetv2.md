---
title: GET (/users/xuid({xuid})/history/titles)
assetID: c0a6cb3b-bab6-03b8-a79e-87defaaa6421
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesgetv2.html
description: " GET (/users/xuid({xuid})/history/titles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cadbf38385bbc321ef5bf23eb93c3fbc5c1a2417
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655583"
---
# <a name="get-usersxuidxuidhistorytitles"></a>GET (/users/xuid({xuid})/history/titles)
取得一份標題為其使用者已解除鎖定，或對其成就的進度。 此 API 不會傳回標題播放或啟動使用者的完整歷程記錄。 這些 Uri 的網域是`achievements.xboxlive.com`。
 
  * [URI 參數](#ID4EY)
  * [查詢字串參數](#ID4EDB)
  * [Authorization](#ID4EFD)
  * [選擇性的要求標頭](#ID4EGE)
  * [要求本文](#ID4ERF)
 
<a id="ID4EY"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的使用者正在存取其標題歷程記錄。| 
  
<a id="ID4EDB"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
| 參數| 必要| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| skipItems| 否| 32 位元帶正負號的整數| 傳回指定數目的項目之後開始的項目。 例如， <b>skipItems ="3"</b>會擷取項目開頭的第四個項目擷取。 | 
| continuationToken| 否| 字串| 傳回項目，始於指定的接續 token。 | 
| maxItems| 否| 32 位元帶正負號的整數| 若要從集合中，可以結合傳回的項目數目上限<b>skipItems</b>並<b>continuationToken</b>要傳回的項目範圍。 服務可能會提供預設值，如果<b>maxItems</b>不存在，而且可能會傳回少於<b>maxItems</b>，即使尚未傳回結果的最後一頁。 | 
  
<a id="ID4EFD"></a>

 
## <a name="authorization"></a>Authorization
 
| 宣告| 必要？| 描述| 如果遺失的行為| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 使用者| 呼叫端是授權的 Xbox LIVE 使用者。| 呼叫端必須是有效的使用者，在 Xbox LIVE。| 403 禁止 」| 
  
<a id="ID4EGE"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。| 
| <b>x-xbl-contract-version</b>| 32 位元不帶正負號的整數| 如果有的話，設定為 2，此 API 的 V2 版本將會使用。 否則，V1。| 
  
<a id="ID4ERF"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>父系 

[/users/xuid({xuid})/history/titles](uri-titlehistoryusersxuidhistorytitlesv2.md)

  
<a id="ID4EPG"></a>

 
##### <a name="reference"></a>參考 

[UserTitle (JSON)](../../json/json-usertitlev2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [分頁參數](../../additional/pagingparameters.md)

   