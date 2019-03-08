---
title: GET (/users/xuid(xuid)/lists/PINS/{listname})
assetID: a63f595a-61dd-5885-c405-9833230abb94
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameget.html
description: " GET (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a31e6a6b541276d3191ba5d40767a1929c70168
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641623"
---
# <a name="get-usersxuidxuidlistspinslistname"></a>GET (/users/xuid(xuid)/lists/PINS/{listname})
傳回清單的內容。 這些 Uri 的網域是`eplists.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4EIB)
  * [查詢字串參數](#ID4ETB)
  * [Authorization](#ID4ESD)
  * [必要的要求標頭](#ID4E6D)
  * [要求本文](#ID4EVF)
  * [HTTP 狀態碼](#ID4EAG)
  * [回應主體](#ID4E5CAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
**ListCount**中傳回的資料欄位會指出維護服務，因此總清單中有多少項目，它可以用來判斷是清單的結尾，而這可能是從幾個不同的數字要求未傳回特定項目。
 
如果清單尚未存在，則結果會不包含任何的清單項目**listCount**將會是零， **listVersion**會是零。
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 字串| Xbox 使用者識別碼 (XUID)。| 
| listtype| 字串| 清單 （其使用方式和它的作用是） 的型別。 一律 「 釘選 」 這些相關方法。| 
| listname| 字串| 清單的名稱 （若要處理的給定 listtype 的清單）。 一律 「 XBLPins"的項目在 Pin 中。| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| 32 位元帶正負號的整數| 選用。 傳回結果之前列舉型別中要略過的項目數目。 預設值：0.| 
| maxItems| 32 位元帶正負號的整數| 選用。 要傳回的項目數目上限。 如果沒有最大值在要求中指定，預設值是 25 個項目。 服務不會對這個值; 最多如果值大於清單中的項目數，則會發生任何錯誤傳回所有項目。| 
| filterItemId| 字串| 選用。 指定要在清單中尋找的項目。 傳回清單中的所有執行個體的項目。 允許用戶端，可快速判斷以及其中一個項目是清單中。 方便大型清單，以判斷項目的所有執行個體，而不需要逐一查看整個清單。 預設值： null。| 
| filterContentType| 字串| 選用。 指定要傳回的內容類型的逗號分隔清單 （將不會傳回類型不在清單中）。 用來從清單中只取得特定內容類型。 以逗號分隔清單的內容類型用於此篩選器。 （一次呼叫中可以查詢多個內容的型別）。支援的內容類型包括娛樂探索服務 (EDS) 所定義的所有媒體類型。 預設值： null （所有的內容類型）。| 
| filterDeviceType| 字串| 選用。 指定要傳回的裝置類型的逗號分隔清單 （將不會傳回類型不在清單中）。 篩選傳回的集合，只傳回已插入從一組特定的裝置類型的項目。 裝置類型的逗號分隔的清單用於此篩選器 （一次呼叫中，多個裝置類型可以進行查詢）。 可能的值：XboxOne、 MCapensis、 w、 WindowsPhone7、 Web、 電腦、 MoLive。 預設值： null （所有的內容類型）。| 
  
<a id="ID4ESD"></a>

 
## <a name="authorization"></a>Authorization
 
這個呼叫會預期中的 XSTS SAML 語彙基元**授權**標頭。 Xuid 宣告必須存在於該 SAML 權杖，以識別來電者。 這個值用來判斷呼叫端是否有問題的清單資料的存取權限。 清單本身的 Xuid 也將會識別，並將會包含在清單的 URI。 使用這種情況，我們可能在未來支援共用清單的存取權，但這不是一項功能在此階段。 目前，使用者存取的所有清單將都會自己並沒有共用存取權。 因此在 URI 中的 Xuid 必須符合在 SAML 宣告權杖 Xuid。 

> [!NOTE] 
> 目前支援 XBL Auth 2.0 和 3.0 版權杖。 


  
<a id="ID4E6D"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 包含用來驗證和授權要求的 STS 權杖。 必須是從 XSTS 服務語彙基元，並將 XUID 納入作為其中一個宣告。 | 
| X-XBL-Contract-Version| 指定的 API 版本正在要求 （正整數）。 Pin 支援第 2 版。 如果此標頭遺失，或該服務會傳回 400-不支援的值不正確的要求與 「 不受支援或遺漏合約版本標頭 」 中的狀態描述。| 
| Content-Type| 指定是否要求/回應內文的內容會以 json 或 xml。 支援的值為"application/json"和"application/xml"| 
| If-Match| 進行修改的要求時，此標頭必須包含一份目前的版本號碼。 修改要求使用 PUT、 POST 或 DELETE 動詞命令。 如果"If-match"標頭中的版本不符合目前版本的清單，要求將會遭到拒絕，並顯示 HTTP 412-先決條件失敗傳回碼。 （選擇性）也可用於取得，如果存在且傳入的版本比對目前的清單版本則沒有清單的資料和 HTTP 304-未修改的程式碼將會傳回。 | 
  
<a id="ID4EVF"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4EAG"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 代表要求順利完成。 回應主體應該包含要求的資源 （GET)。 POST 和 PUT 要求會收到最新清單中繼資料 （清單版本、 計數等等）。| 
| 201| 建立時間| 已建立新的清單。 這會傳回對初始插入至清單。 回應會包含在清單上的最新中繼資料和 location 標頭包含清單的 URI。| 
| 304| 未修改| 傳回上取得。 表示用戶端具有最新版本的清單。 服務會比較中的值<b>If-match</b>清單版本標頭。 如果兩者相等，則取得傳回 304 沒有資料。| 
| 400| 錯誤的要求| 要求的格式不正確。 可能是無效的值或 URI 或查詢字串參數的型別。 也可以是遺漏的必要參數的結果，或資料值或要求指示 遺失或無效的 api 版本。 請參閱<b>X XBL-合約版本</b>標頭。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 403| 已禁止| 要求不允許使用者或服務。| 
| 404| 找不到| 呼叫端沒有存取資源。 這表示已建立的任何這類清單。| 
| 412| 先決條件失敗| 表示清單的版本已變更，而且修改要求失敗。 修改要求是插入、 更新或移除。 服務會檢查<b>If-match</b>清單版本標頭。 如果它不符合目前版本的清單，作業就會失敗，這會傳回目前的清單中繼資料 （其中包含目前的版本）。| 
| 500| 內部伺服器錯誤| 服務會拒絕要求，因為伺服器端錯誤。| 
| 501| 未實作| 呼叫端要求的 URI，在伺服器上尚未實作。 （目前僅使用時的要求是針對未列入允許清單的清單名稱。）| 
| 503| 服務無法使用| 伺服器拒絕要求時，通常是因為過多的負載。 在延遲之後 (請參閱<b>retry-after</b>標頭)，可以重試要求。| 
  
<a id="ID4E5CAC"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4EEDAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{ 
"ListMetadata":
  {"ListVersion": 12,
   "ListCount": 6,
   "MaxListSize": 200,
   "AccessSetting": "OwnerOnly",
   "AllowDuplicates": true
  },
"ListItems":
  [{ 
   "Index": 0,
   "DateAdded": "\/Date(1198908717056)/",
   "DateModified": "\/Date(1198908717056)/",
   "HydrationResult": "Indeterminate",
   "HydratedItem": null

   "Item":
   {
     "ContentType": "Movie",
     "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
     "ProviderId": null,
     "Provider": null,
     "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC",
     "Title": "The Dark Knight",
     "SubTitle": null,
     "Locale": "en-us",
     "AltImageUrl": null,
     "DeviceType": "XboxOne"
    }
  }]
}
         
```

   
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>父系 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

  
<a id="ID4E1DAC"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   