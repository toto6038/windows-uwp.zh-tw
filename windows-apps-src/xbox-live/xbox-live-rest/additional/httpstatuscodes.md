---
title: 標準 HTTP 狀態碼
assetID: 7a19de56-7acd-ad2b-e8e6-53126991093b
permalink: en-us/docs/xboxlive/rest/httpstatuscodes.html
description: " 標準 HTTP 狀態碼"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c77972e88dbf4950f716594ee61220d1734a7fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635553"
---
# <a name="standard-http-status-codes"></a>標準 HTTP 狀態碼
 
「 超文字傳輸通訊協定 (HTTP) 標準說明多個用戶端要求的回應中的伺服器所傳回的狀態碼。 Xbox Live 服務方法會傳回 HTTP 通訊協定符合規範的狀態碼，來描述要求的狀態。
 
以下是傳回的 Xbox Live 服務和其一般意義的狀態碼的清單。
 
<a id="ID4EAB"></a>

 
## <a name="xbox-live-services-status-codes"></a>Xbox Live 服務的狀態碼
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | 
| 200| 確定| 要求成功。| 
| 201| 建立時間| 建立實體。| 
| 202| 接受| 要求被接受，但尚未完成。| 
| 204| 沒有內容| 要求已完成，但並沒有可傳回的內容。| 
| 301| 已永久移動| 服務已移動至不同的 URI。| 
| 302| 找到| 要求的資源暫時位於不同 URI 之下。| 
| 307| 暫時重新導向| 已暫時變更此資源的 URI。| 
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 403| 已禁止| 要求不允許使用者或服務。| 
| 404| 找不到| 找不到指定的資源。| 
| 406| 無法接受| 不支援資源版本。| 
| 408| 要求逾時| 該要求花了太多時間完成。| 
| 409| 衝突| 與資源的目前狀態發生衝突，因此未完成要求。| 
| 410| 不存在| 無法再使用所要求的資源。| 
| 412| 先決條件失敗| 伺服器不符合其中一項申請者對要求的先決條件。| 
| 416| 要求無法滿足的範圍| 找不到要求的範圍。| 
| 500| 內部伺服器錯誤| 伺服器發生未預期的狀況，導致無法完成要求。| 
| 501| 未實作| 伺服器不支援完成要求所需的功能。| 
| 503| 服務無法使用| 要求已節流處理，以秒為單位 （例如 5 秒之後） 的用戶端重試值之後，再次嘗試要求。| 
 

> [!NOTE] 
> 某些資源和方法提供該資源或方法的內容中的特定狀態碼的意義的特定資訊。 如需詳細資訊，請參閱資源或您使用的方法的文件。 

  
<a id="ID4E3BAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E5BAC"></a>

 
##### <a name="parent"></a>父系  

[其他參考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EKCAC"></a>

 
##### <a name="reference--universal-resource-identifier-uri-referenceuriatoc-xboxlivews-reference-urismd"></a>參考[統一資源識別元 (URI) 參考](../uri/atoc-xboxlivews-reference-uris.md)

 [其他參考](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EZCAC"></a>

 
##### <a name="external-links--w3org-http11-status-code-definitionshttpswwww3orgprotocolsrfc2616rfc2616-sec10htmlsec10"></a>外部連結[W3.org:HTTP/1.1 狀態碼定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10)

   