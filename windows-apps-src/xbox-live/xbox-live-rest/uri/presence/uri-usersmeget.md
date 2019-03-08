---
title: GET (/users/me)
assetID: 726c279b-73fb-02ea-cbff-700ff2dc31af
permalink: en-us/docs/xboxlive/rest/uri-usersmeget.html
description: " GET (/users/me)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b06305fde989d0c30570beda5d4b0aabe7bf0518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606723"
---
# <a name="get-usersme"></a>GET (/users/me)
取得目前的使用者[PresenceRecord](../../json/json-presencerecord.md)而不需要知道使用者的 XUID。
這些 Uri 的網域是`userpresence.xboxlive.com`。

  * [查詢字串參數](#ID4EZ)
  * [Authorization](#ID4EIC)
  * [必要的要求標頭](#ID4ELD)
  * [選擇性的要求標頭](#ID4EPF)
  * [要求本文](#ID4EPG)
  * [回應主體](#ID4E1G)

<a id="ID4EZ"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 參數| 類型| 描述|
| --- | --- | --- |
| level| 字串| 選用。 <ul><li><b>使用者</b>:傳回使用者節點。</li><li><b>裝置</b>:傳回使用者節點 和 裝置 節點。</li><li><b>標題</b>:預設。 傳回活動除外的整個樹狀結構。</li><li><b>所有</b>:傳回整個樹狀結構，包括活動層級組態選項。</li></ul> | 

<a id="ID4EIC"></a>


## <a name="authorization"></a>Authorization

| 類型| 必要| 描述| 如果遺失的回應|
| --- | --- | --- | --- | --- | --- | --- |
| XUID| 是| Xbox 使用者識別碼 (XUID) 的呼叫端| 403 禁止 」|

<a id="ID4ELD"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。|
| x-xbl-contract-version| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 範例值：3，vnext。|
| Accept| 字串| 可接受的內容類型。 唯一支援的目前狀態為 application/json，但您必須指定標頭中。|
| Accept-Language| 字串| 可接受的回應中的字串的地區設定。 範例值： EN-US。|
| 主機| 字串| 伺服器的網域名稱。 範例值： presencebeta.xboxlive.com。|

<a id="ID4EPF"></a>


## <a name="optional-request-headers"></a>選擇性的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.|

<a id="ID4EPG"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4E1G"></a>


## <a name="response-body"></a>回應主體

<a id="ID4EAH"></a>


### <a name="sample-response"></a>範例回應

這個方法會傳回[PresenceRecord](../../json/json-presencerecord.md)。


```cpp
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:W8,
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page",
      }
    }]
  }]
}

```


<a id="ID4EQH"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ESH"></a>


##### <a name="parent"></a>父系

[/users/me](uri-usersme.md)
