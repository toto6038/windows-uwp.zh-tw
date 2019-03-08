---
title: POST (/titles/{titleId}/variants)
assetID: 84303448-5a11-d96f-907d-77f57f859741
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants-post.html
description: " POST (/titles/{titleId}/variants)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 17974ddf7dec26abac18ccee9fda5249bc9d656f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618493"
---
# <a name="post-titlestitleidvariants"></a>POST (/titles/{titleId}/variants)
擷取一份指定的項目識別碼的遊戲變體的用戶端呼叫的 URI這些 uri 的網域`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [URI 參數](#ID4EZ)
  * [必要的要求標頭](#ID4EIB)
  * [選擇性的要求標頭](#ID4EED)
  * [Authorization](#ID4E3D)
  * [要求本文](#ID4EEE)
  * [必要的回應標頭](#ID4ELF)
  * [選擇性的回應標頭](#ID4EMG)
  * [回應主體](#ID4EEH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 描述| 
| --- | --- | 
| titleid| 要求應操作的標題的識別碼。| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主機名稱

gameserverds.xboxlive.com
 
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
當提出要求，所顯示下表中的標頭。
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | 
| Content-Type| 應用程式/json| 正在提交的資料型別。| 
| 主機| gameserverds.xboxlive.com|  | 
| Content-Length|  | 要求物件的長度。| 
| x-xbl-contract-version| 1| API 合約版本。| 
| Authorization| XBL3.0 x=[hash];[token]| 驗證權杖。| 
  
<a id="ID4EED"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
在要求時下, 表所示的標頭是選擇性的。
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | 在要求主體的 mime 類型。| 
  
<a id="ID4E3D"></a>

 
## <a name="authorization"></a>Authorization

要求必須包含有效的 Xbox Live 授權標頭。 如果呼叫端不允許存取此資源中，服務會傳回 403 禁止 」 回應。 如果標頭無效或遺失，則服務會傳回 401 未授權回應中。
 
<a id="ID4EEE"></a>

 
## <a name="request-body"></a>要求本文
 
要求必須包含具有下列成員的 JSON 物件。
 
| 成員| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 地區設定| 傳回 variant 的本機。| 
| maxVariants| 要傳回的變體的數目上限。| 
| publisherOnly|  | 
| 限制|  | 
 
<a id="ID4EDF"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
{
  "locale": "en-us",
  "maxVariants": "100",
  "publisherOnly": "false",
  "restriction": null
}

```

   
<a id="ID4ELF"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
回應一定會包含下表所示的標頭。
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| 應用程式/json| 回應主體中的資料型別。| 
| Content-Length|  | 回應主體的長度。| 
  
<a id="ID4EMG"></a>

 
## <a name="optional-response-headers"></a>選擇性的回應標頭
 
回應可能包括如下所示的標頭。
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | 回應主體的 mime 類型。| 
  
<a id="ID4EEH"></a>

 
## <a name="response-body"></a>回應本文
 
如果呼叫成功，服務會傳回 JSON 物件，具有下列成員。
 
| 成員| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 變化| Variant 的陣列。| 
| variantId| 變數的識別碼。| 
| name| 變數名稱。| 
| isPublisher|  | 
| 陣序規範|  | 
| gameVariantSchemaId|  | 
| variantSchemas| Variant 結構描述的陣列。| 
| variantSchemaId| 結構描述的識別碼。| 
| schemaContent| 結構描述內容| 
| name| 結構描述名稱| 
| gsiSets| GSI 集合的陣列。| 
| minRequiredPlayers| Variant 的播放程式的數目下限。| 
| maxAllowedPlayers| Variant 的播放程式的數目上限。| 
| gsiSetId| GSI 集合識別碼。| 
| gsiSetName| GSI 集的名稱。| 
| selectionOrder|  | 
| variantSchemaId| 設定用於 GSI varaint 結構描述的識別碼。| 
 
<a id="ID4EYBAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
 "variants": [
     { 
       "variantId": "8B6EF8A0-7807-42C4-9CB0-1D9B8B8CE742", 
       "name": "tankWarsV2.0",
       "isPublisher": "true",
       "rank": "1",
       "gameVariantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }],
  "variantSchemas": [
     {
        "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB",
        "schemaContent": "&lt;?xml version=\"1.0\" encoding=\"UTF-8\" ?>&lt;xs:schema xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">&lt;xs:element name=\"root\">&lt;/xs:element>&lt;/xs:schema>"
        "name": "tanksSchema"
     }],
     "gsiSets":
     [{ 
          "minRequiredPlayers": "5", 
          "maxAllowedPlayers": "10", 
          "gsiSetId": "B28047F5-B52F-477E-97C2-4C1C39E31D42",
          "gsiSetName": "TanksGSISet",
          "selectionOrder": "1",
          "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }]
 }

  

```

   
<a id="ID4ERCAC"></a>

 
## <a name="see-also"></a>請參閱
 [/titles/{titleId}/variants](uri-titlestitleidvariants.md)

  