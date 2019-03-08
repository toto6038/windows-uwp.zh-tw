---
title: PUT (/users/xuid(xuid)/lists/PINS/{listname})
assetID: f7964d2e-a8c8-2caa-ca97-e0d76ef586f4
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameput.html
description: " PUT (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 35527df28c2ca482d0c5ae2fe25637b3bc29f6ca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622923"
---
# <a name="put-usersxuidxuidlistspinslistname"></a>PUT (/users/xuid(xuid)/lists/PINS/{listname})
更新清單，以根據要求主體中的每個項目針對指定的索引中的項目。 這些 Uri 的網域是`eplists.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4E1B)
  * [Authorization](#ID4EFC)
  * [HTTP 狀態碼](#ID4ESC)
  * [必要的要求標頭](#ID4EPH)
  * [要求本文](#ID4EGBAC)
  * [回應主體](#ID4EWBAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
此呼叫將會更新清單，以根據要求主體中的每個項目針對指定的索引中的項目。 這個呼叫會將項目插入清單中，以及如果在指定的索引，並不存在的項目，然後呼叫會傳回 400 不正確的要求的狀態。 可以在單一呼叫中，更新多個項目，但全都必須位於目前的清單。 也就是所有更新，或無更新。
 
此呼叫將會允許更新，以指定的項目**itemId**而不是**index**。 若要這樣做，只要使用"-1"中的索引**IndexedItems**傳送至服務的結構。 在此情況下，顯然**itemId**無法更新，即變更，因此它只適用於其他中繼資料欄位的變更。 **提供者**/**providerId**可用來組合，而非**itemId**識別的項目。 就內部而言，服務會搜尋這些項目和找出適當的索引，若要更新的清單。 如果找不到項目或項目，將會傳回 400 不正確的要求的狀態，並沒有任何項目將會更新。
 
此呼叫都需要**如果-比對： versionNumber**使用索引來識別項目，會包含在要求的標頭。 如果使用項目識別碼識別的項目 （而且清單不允許重複的項目），則**If-match** header 是選擇性參數。 若有的話**If-match**標頭將一律會經過驗證。 在頁首中， **versionNumber**是目前的版本號碼的清單。 如果它不是包含 （並需要），或目前的清單版本號碼，則 HTTP 412 前置條件失敗 」 狀態碼將傳回的比對，而且是回應主體會包含清單的最新的中繼資料，包括目前的版本號碼。 這是為了防範來自欺侮在另一個不同的用戶端的更新。
  
<a id="ID4E1B"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 字串| Xbox 使用者識別碼 (XUID)。| 
| listtype| 字串| 清單 （其使用方式和它的作用是） 的型別。 一律 「 釘選 」 這些相關方法。| 
| listname| 字串| 清單的名稱 （若要處理的給定 listtype 的清單）。 一律 「 XBLPins"的項目在 Pin 中。| 
  
<a id="ID4EFC"></a>

 
## <a name="authorization"></a>Authorization
 
這個呼叫會預期中的 XSTS SAML 語彙基元**授權**標頭。 Xuid 宣告必須存在於該 SAML 權杖，以識別來電者。 這個值用來判斷呼叫端是否有問題的清單資料的存取權限。 清單本身的 Xuid 也將會識別，並將會包含在清單的 URI。 使用這種情況，我們可能在未來支援共用清單的存取權，但這不是一項功能在此階段。 目前，使用者存取的所有清單將都會自己並沒有共用存取權。 因此在 URI 中的 Xuid 必須符合在 SAML 宣告權杖 Xuid。 

> [!NOTE] 
> 目前支援 XBL Auth 2.0 和 3.0 版權杖。 


  
<a id="ID4ESC"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EPH"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 包含用來驗證和授權要求的 STS 權杖。 必須是從 XSTS 服務語彙基元，並將 XUID 納入作為其中一個宣告。 | 
| X-XBL-Contract-Version| 指定的 API 版本正在要求 （正整數）。 Pin 支援第 2 版。 如果此標頭遺失，或該服務會傳回 400-不支援的值不正確的要求與 「 不受支援或遺漏合約版本標頭 」 中的狀態描述。| 
| Content-Type| 指定是否要求/回應內文的內容會以 json 或 xml。 支援的值為"application/json"和"application/xml"| 
| If-Match| 進行修改的要求時，此標頭必須包含一份目前的版本號碼。 修改要求使用 PUT、 POST 或 DELETE 動詞命令。 如果"If-match"標頭中的版本不符合目前版本的清單，要求將會遭到拒絕，並顯示 HTTP 412-先決條件失敗傳回碼。 （選擇性）也可用於取得，如果存在且傳入的版本比對目前的清單版本則沒有清單的資料和 HTTP 304-未修改的程式碼將會傳回。 | 
  
<a id="ID4EGBAC"></a>

 
## <a name="request-body"></a>要求本文
 
<a id="ID4EMBAC"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
{"IndexedItems":
 [{ "Index": 0, 
     "Item": 
     {
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": null,
    "Provider": null,
    "ImageUrl": "https://www.bing.com/thumb/...",
    "Title": "The Dark Knight",
    "SubTitle":null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
}
}]}      
      
```

   
<a id="ID4EWBAC"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4E3BAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
  "ListVersion":  1,
  "ListCount":  1,
  "MaxListSize": 200,
  "AllowDuplicates": "true",
  "AccessSetting":  "OwnerOnly"
}        
         
```

   
<a id="ID4EGCAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EICAC"></a>

 
##### <a name="parent"></a>父系 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   