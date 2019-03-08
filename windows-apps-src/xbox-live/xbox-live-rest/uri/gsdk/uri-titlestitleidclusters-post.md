---
title: POST (/titles/{titleId}/clusters)
assetID: 0977b0b0-872d-f7ad-9ba0-30d56cff4912
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidclusters-post.html
description: " POST (/titles/{titleId}/clusters)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 91d7c49628914f887c5d2243942e10e47d47b095
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608903"
---
# <a name="post-titlestitleidclusters"></a>POST (/titles/{titleId}/clusters)
允許用戶端建立 Xbox Live 計算伺服器執行個體的 URI。 這些 Uri 的網域是`gameserverms.xboxlive.com`。
 
  * [URI 參數](#ID4EX)
  * [必要的要求標頭](#ID4EGB)
  * [Authorization](#ID4ELD)
  * [要求本文](#ID4EWD)
  * [必要的回應標頭](#ID4EZE)
  * [回應主體](#ID4E5G)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 描述| 
| --- | --- | 
| titleId| 要求應操作的標題的識別碼。| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主機名稱

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
當提出要求，所顯示下表中的標頭。
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | 
| 使用者代理程式|  | 提出要求的使用者代理程式的相關資訊。| 
| Content-Type| 應用程式/json| 正在提交的資料型別。| 
| 主機| gameserverms.xboxlive.com|  | 
| Content-Length|  | 要求物件的長度。| 
| x-xbl-contract-version| 1| API 合約版本。| 
| Authorization| XBL3.0 x=[hash];[token]| 驗證權杖。| 
  
<a id="ID4ELD"></a>

 
## <a name="authorization"></a>Authorization
 
要求必須包含有效的 Xbox Live 授權標頭。 如果呼叫端不允許存取此資源中，服務會傳回 403 禁止 」 回應。 如果標頭無效或遺失，則服務會傳回 401 未授權回應中。
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>要求本文
 
要求必須包含具有下列成員的 JSON 物件。
 
| 成員| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| 從 MPSD 的工作階段識別碼。| 
| abortIfQueued| 選擇性參數，當設定為 true 會告訴 GSMS 未排入佇列之資源的這個工作階段，如果它不能接立即。 如果要求已中止，因為此值為 true，會包含回應物件<code>"fulfillmentState" : "Aborted"</code>。 | 
 
<a id="ID4ERE"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
{
  "sessionId" : "/serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/session/scott1",
  "abortIfQueued" : "true"
}

      
```

   
<a id="ID4EZE"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
回應一定會包含下表所示的標頭。
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Cache-Control|  | 必須遵守的所有快取的機制，以及要求/回應鏈結的指示詞。| 
| Content-Type| 應用程式/json| 在回應中的資料型別。| 
| Content-Length|  | 回應主體的長度。| 
| X-Content-Type-Options|  |  | 
| X-XblCorrelationId|  | 回應主體的 mime 類型。| 
| 日期|  |  | 
  
<a id="ID4E5G"></a>

 
## <a name="response-body"></a>回應本文
 
如果呼叫成功，服務會傳回 JSON 物件，具有下列成員。
 
| 成員| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| pollIntervalMilliseconds| 建議您以毫秒為單位來輪詢已完成的間隔。 請注意，這並非的估計值時就可以使用叢集，但建議而不是呼叫端應該輪詢狀態更新，指定目前的集區的訂用帳戶 」 和 「 要求 」 和 「 履行 」 費率的頻率。| 
| fulfillmentState| 指出是否提供的工作階段立即配置資源，「 履行 」 新增至佇列的可用性為未來的資源，[已佇列]，或已中止，「 中止 」，因為無法完成要求時立即要求指定為 [true] abortIfQueued。 | 
 
<a id="ID4EWH"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
  "pollIntervalMilliseconds" : "1000",
  "fulfillmentState" : "Fulfilled" | "Queued" | "Aborted"
}
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>備註
 
標題只應該重試呼叫服務時收到下列的回應碼：
 
   * 408 — 伺服器逾時
   * 429： 太多要求
   * 500-伺服器錯誤
   * 502-錯誤的閘道
   * 503-服務無法使用
   * 504-閘道逾時
   
<a id="ID4EFBAC"></a>

 
## <a name="see-also"></a>請參閱
 [/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

  