---
title: GET (/users/xuid({xuid}))
assetID: c97ef943-8bea-8a41-90d7-faea874284c8
permalink: en-us/docs/xboxlive/rest/uri-usersxuidget.html
description: " GET (/users/xuid({xuid}))"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5fcc5d3b6a172eccab0656da39e6896b4df50840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650923"
---
# <a name="get-usersxuidxuid"></a>GET (/users/xuid({xuid}))
探索其他使用者或用戶端的目前狀態。
這些 Uri 的網域是`userpresence.xboxlive.com`。

  * [註解](#ID4EV)
  * [URI 參數](#ID4EDB)
  * [查詢字串參數](#ID4EOB)
  * [Authorization](#ID4E4C)
  * [在資源上的隱私權設定的效果](#ID4EAE)
  * [必要的要求標頭](#ID4EVH)
  * [選擇性的要求標頭](#ID4E1BAC)
  * [要求本文](#ID4E1CAC)
  * [回應主體](#ID4EFDAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>備註

回應可以篩選提供的一部分[PresenceRecord](../../json/json-presencerecord.md)取用者不想在整個物件。

> [!NOTE] 
> 傳回的資料會受到隱私權和內容隔離規則。



<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的目標使用者。|

<a id="ID4EOB"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 參數| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- |
| level| 字串| 選用。 <ul><li><b>使用者</b>:傳回使用者節點。</li><li><b>裝置</b>:傳回使用者節點 和 裝置 節點。</li><li><b>標題</b>:預設。 傳回活動除外的整個樹狀結構。</li><li><b>所有</b>:傳回整個樹狀結構，包括活動層級組態選項。</li></ul> |

<a id="ID4E4C"></a>


## <a name="authorization"></a>Authorization

| 類型| 必要| 描述| 如果遺失的回應|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| 是| Xbox 使用者識別碼 (XUID) 的呼叫端| 403 禁止 」|

<a id="ID4EAE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果

這個方法一定會傳回 「 200 確定，但是可能不會在回應主體中傳回內容。

| 提出要求的使用者| 目標使用者的隱私權設定| 行為|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 我| -| 200 OK|
| friend| 每個人| 200 OK|
| friend| 只有朋友| 200 OK|
| friend| 封鎖| 200 OK|
| 非 friend 使用者| 每個人| 200 OK|
| 非 friend 使用者| 只有朋友| 200 OK|
| 非 friend 使用者| 封鎖| 200 OK|
| 協力廠商網站| 每個人| 200 OK|
| 協力廠商網站| 只有朋友| 200 OK|
| 協力廠商網站| 封鎖| 200 OK|

<a id="ID4EVH"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。|
| x-xbl-contract-version| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 範例值：3，vnext。|
| Accept| 字串| 可接受的內容類型。 唯一支援的目前狀態為 application/json，但您必須指定標頭中。|
| Accept-Language| 字串| 可接受的回應中的字串的地區設定。 範例值： EN-US。|
| 主機| 字串| 伺服器的網域名稱。 範例值： presencebeta.xboxlive.com。|

<a id="ID4E1BAC"></a>


## <a name="optional-request-headers"></a>選擇性的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.|

<a id="ID4E1CAC"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4EFDAC"></a>


## <a name="response-body"></a>回應主體

<a id="ID4ELDAC"></a>


### <a name="sample-response"></a>範例回應

如果沒有現有的使用者記錄，則會傳回任何裝置的記錄。


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


<a id="ID4EXDAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EZDAC"></a>


##### <a name="parent"></a>父系

[/users/xuid({xuid})](uri-usersxuid.md)
