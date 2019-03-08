---
title: EDS 通用標頭
assetID: 91297c9e-709d-7886-1da0-8a896c4953f4
permalink: en-us/docs/xboxlive/rest/edscommonheaders.html
description: " EDS 通用標頭"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a85383ee75fa51cb8376451ac955dcc4fa2edf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630703"
---
# <a name="eds-common-headers"></a>EDS 通用標頭

<a id="ID4EO"></a>



## <a name="entertainment-discovery-services-eds-common-request-headers"></a>娛樂探索服務的常見要求標頭 (EDS)

| 標頭名稱| 描述| 必要？| 附註|
| --- | --- | --- | --- |
| <b>x-xbl-contract-version</b>| EDS 服務版本| 是| 3.2|
| <b>x-xbl-client-type</b>| 用戶端型別標頭| 是| 向小組以取得您自己的用戶端類型。|
| <b>x-xbl-client-version</b>| 用戶端版本| 是| 任何非空白字串。|
| <b>x-xbl-parent-ig</b>| 印象 guid| 是| 用來追蹤在記錄檔，然後在其他服務呼叫的要求。|
| <b>x-xbl-device-type</b>| 裝置類型| 是| 在代表用戶端的裝置。|
| <b>接受</b>| 接受的型別| 是| XML 或 JSON。|
| <b>Authorization</b>| 驗證標頭| 是|  |
| <b>x-xbl-autoSuggest-seed-text</b>| 自動建議種子文字| 否| 使用的 BI 和相關性|
| <b>x-xbl-search-term</b>| 搜尋字詞| 否|  |
| <b>x-xbl-input-method</b>| 使用者所使用的輸入的方法| 否| 控制站，語音、 Kinect。|
| <b>x-xbl-kinect-enabled</b>| 已啟用的 Kinect| 否| 是/否。|
| <b>x-xbl-speech-session-id</b>| 語音工作階段識別碼| 否| 是否已起始工作階段，使用 語音。|
| <b>x-xbl-client-id</b>| 匿名用戶端識別碼| 否| 用於 BI 報告和相關性。|
| <b>x-xbl-device-id</b>| 裝置識別碼| 否| 用於 BI 報告和相關性。|
| <b>x-xbl-user-agent</b>| 用戶端使用者代理程式| 否| 使用 bi。 「&lt;名稱 > /&lt;版本 > (&lt;OS 版本 >;&lt;平台 >;&lt;功能 >;&lt;製造 >;&lt;模型 >) 」。|
| <b>x-xbl-parent-ig</b>| 針對 「 Chained"呼叫之前的印象 Guid| 否 （但強烈建議使用）| 重要的商業情報相關性。 例如，瀏覽呼叫 IG 是父代 IG 下列設定詳細資料的呼叫。|
| <b>delid</b>| 識別委派| 否| 用來代表使用者運作的內部服務。|

## <a name="common-response-headers"></a>常見的回應標頭

| 標頭名稱| 描述| 必要？| 附註|
| --- | --- | --- | --- | --- | --- | --- | --- |
| <b>快取</b>| 快取標頭| 是| 請參閱<a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9"> http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9 </a>。|
| <b>x-xbl-errors</b>| 錯誤| 否| 從各種資料提供者的錯誤清單。|
| <b>x-xbl-traceid</b>| 從記錄檔的追蹤識別碼| 是| 用來追蹤回要求的特定記錄檔。|
| <b>x-server-name</b>| 混亂會處理要求的伺服器名稱。| 是| 用於內部偵錯。|

<a id="ID4EECAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EGCAC"></a>


##### <a name="parent"></a>父系  

[其他參考](atoc-xboxlivews-reference-additional.md)


<a id="ID4ESCAC"></a>


##### <a name="further-information"></a>進一步的資訊

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)
