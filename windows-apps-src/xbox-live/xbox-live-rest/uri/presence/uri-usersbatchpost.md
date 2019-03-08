---
title: POST (/users/batch)
assetID: bd0b18fe-8a6d-d591-5b13-bcd9643e945a
permalink: en-us/docs/xboxlive/rest/uri-usersbatchpost.html
description: " POST (/users/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47376338a1c515aa00a7e0247df4cc16ee0db8d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655673"
---
# <a name="post-usersbatch"></a>POST (/users/batch)
取得一批使用者存在。
這些 Uri 的網域是`userpresence.xboxlive.com`。

  * [註解](#ID4EV)
  * [Authorization](#ID4EAB)
  * [在資源上的隱私權設定的效果](#ID4EDC)
  * [必要的要求標頭](#ID4EYF)
  * [選擇性的要求標頭](#ID4EGAAC)
  * [要求本文](#ID4EGBAC)
  * [回應主體](#ID4ESEAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>備註

這個方法應該供任何用戶端、 服務或想要了解一批使用者的目前狀態資訊的標題。

針對此批次要求的回應可以是篩選器的深度和路徑。 取用者可以使用這個來找出並顯示有關一組使用者存在。 此 API 上的篩選器作用於 Or 作為屬性，但 ANDs 屬性。

<a id="ID4EAB"></a>


## <a name="authorization"></a>Authorization

| 類型| 必要| 描述| 如果遺失的回應|
| --- | --- | --- | --- |
| XUID| 是| Xbox 使用者識別碼 (XUID) 的呼叫端| 403 禁止 」|

<a id="ID4EDC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果

這個方法一定會傳回 「 200 確定，但是可能不會在回應主體中傳回內容。

| 提出要求的使用者| 目標使用者的隱私權設定| 行為|
| --- | --- | --- | --- | --- | --- | --- |
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

<a id="ID4EYF"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。|
| x-xbl-contract-version| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 範例值：3，vnext。|
| Accept| 字串| 可接受的內容類型。 唯一支援的目前狀態為 application/json，但您必須指定標頭中。|
| Accept-Language| 字串| 可接受的回應中的字串的地區設定。 範例值： EN-US。|
| 主機| 字串| 伺服器的網域名稱。 範例值： presencebeta.xboxlive.com。|
| Content-Length| 字串| 要求主體的長度。 範例值：312.|

<a id="ID4EGAAC"></a>


## <a name="optional-request-headers"></a>選擇性的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.|

<a id="ID4EGBAC"></a>


## <a name="request-body"></a>要求本文

<a id="ID4EMBAC"></a>


### <a name="required-members"></a>必要的成員

| 成員| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 使用者| 清單 XUIDs 的使用者存在您想要深入了解，最多為 1100 XUIDs 一次。|

<a id="ID4EHCAC"></a>


### <a name="optional-members"></a>選擇性的成員

| 成員| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| deviceTypes| 您想要知道使用者所使用的裝置類型的清單。 如果陣列為空白，則預設為所有可能的裝置類型 （也就是 none 則濾除）。|
| 標題| 裝置清單輸入您想要了解其的使用者。 如果陣列為空白，則預設為所有可能的項目 （也就是 none 則濾除）。|
| level| 可能的值： <ul><li>使用者-get 使用者節點</li><li>裝置-取得使用者和裝置節點</li><li>標題-取得基本項目層級資訊</li><li>all-取得豐富的目前狀態資訊、 媒體資訊，或兩者</li></ul><br> 預設值為"title"。|
| onlineOnly| 如果這個屬性為 true，批次作業將會篩選出離線使用者 （包括已隱匿的項目） 的記錄。 如果未提供，則會傳回線上和離線的使用者。|

<a id="ID4E4DAC"></a>


### <a name="prohibited-members"></a>禁止的成員

所有其他成員均不得在要求中。

<a id="ID4EIEAC"></a>


### <a name="sample-request"></a>範例要求


```cpp
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}

```


<a id="ID4ESEAC"></a>


## <a name="response-body"></a>回應主體

<a id="ID4E1EAC"></a>


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


<a id="ID4EKFAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EMFAC"></a>


##### <a name="parent"></a>父系

[/users/batch](uri-usersbatch.md)
