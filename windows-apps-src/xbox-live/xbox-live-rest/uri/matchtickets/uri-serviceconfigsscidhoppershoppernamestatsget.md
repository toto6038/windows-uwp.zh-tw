---
title: GET (/serviceconfigs/{scid}/hoppers/{name}/stats)
assetID: 4de5b07d-93e1-8ff0-05dd-1d3bb1802088
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestatsget.html
description: " GET (/serviceconfigs/{scid}/hoppers/{name}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 95de95b35de496331dd3fe0a4c69f18e047c1020
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621693"
---
# <a name="get-serviceconfigsscidhoppersnamestats"></a>GET (/serviceconfigs/{scid}/hoppers/{name}/stats)

Hopper 取得統計資料。

> [!IMPORTANT]
> 這個方法是針對與合約 103 或更新版本，搭配使用，因此需要 X Xbl-合約版本標頭項目：103 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4E5)
  * [Authorization](#ID4EJB)
  * [HTTP 狀態碼](#ID4E3C)
  * [要求本文](#ID4EFD)
  * [回應主體](#ID4EQD)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註
此 HTTP/REST 方法取得從具名 hopper 在服務組態識別碼 (SCID) 層級的統計資料。 這個方法可以前後加**Microsoft.Xbox.Services.Matchmaking.MatchmakingService.GetHopperStatisticsAsync** API。  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務組態 (SCID) 工作階段識別碼。|
| name| 字串| Hopper 名稱。|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Authorization

| 類型| 必要| 描述| 如果遺失的回應|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID （使用者識別碼）| 是| 提出要求的使用者必須是票證工作階段票證所參考的成員。 | 403|
| 特殊權限與裝置類型| 是| 當使用者的裝置類型設定為主控台時，只有在其宣告中的多人的權限的使用者可以對 「 配對 」 服務進行呼叫。 | 403|
| 標題識別碼/證明的採購/裝置類型| 是| 會比對到標題必須允許指定的標題宣告、 裝置類型組合的配對。 | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EFD"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4EQD"></a>


## <a name="response-body"></a>回應主體

| 成員| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| hopperName| 字串| 選取 hopper 名稱。|
| waitTime| 32 位元帶正負號的整數| 比對 hopper （整數秒數） 的時間的平均值。 |
| 母體擴展| 32 位元帶正負號的整數| 人員等候 hopper 中相符項目數目。|

<a id="ID4E1D"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }


```


<a id="ID4EJE"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ELE"></a>


##### <a name="parent"></a>父系  

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)
