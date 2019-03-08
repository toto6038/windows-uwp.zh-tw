---
title: 標準 HTTP 要求和回應標頭
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
description: " 標準 HTTP 要求和回應標頭"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca97e82365eab40266b3ffdd84924f71289eede6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636513"
---
# <a name="standard-http-request-and-response-headers"></a>標準 HTTP 要求和回應標頭
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>要求標頭
 
下表列出建立 Xbox Live 服務要求時所用的標準 HTTP 標頭。
 
| 標頭| 值| 描述| 
| --- | --- | --- | 
| x-xbl-contract-version| 1| API 合約版本。 所需的所有的 Xbox Live 服務要求。| 
| Authorization| STSTokenString| STS 驗證語彙基元。 此標頭的值取自<b>GetTokenAndSignatureResult.Token</b>屬性。 | 
| Content-Type| application/xml, application/json, multipart/form-data or application/x-www-form-urlencoded| 指定正在提交要求的內容類型。| 
| Content-Length| 整數值| 指定正在提交 POST 要求中的資料長度。| 
| Accept-Language | 字串| 指定如何傳回任何字串當地語系化。 請參閱<a href="https://msdn.microsoft.com/en-us/library/bb975829.aspx">進階 Xbox 360 程式設計</a>如需有效的語言/地區組合的清單。| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>回應標頭
 
下表列出使用 Xbox Live 服務回應中的標準 HTTP 標頭。
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| application/xml，application/json| 指定要傳回的內容類型。| 
| Content-Length| 整數值| 指定要傳回的資料長度。| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>父系  

[其他參考](atoc-xboxlivews-reference-additional.md)

   