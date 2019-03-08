---
title: GET (/qosservers)
assetID: 8b940c1b-947c-eab3-78ed-4384f57ea0bd
permalink: en-us/docs/xboxlive/rest/uri-qosservers-get.html
description: " GET (/qosservers)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 02d24dbf1d189b759784dbbfa7052e2c218ec27e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632063"
---
# <a name="get-qosservers"></a>GET (/qosservers)
若要取得可用的 QoS 伺服器清單用於 Xbox Live 計算用戶端呼叫的 URI。 這些 uri 的網域`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [必要的要求標頭](#ID4EBB)
  * [必要的回應標頭](#ID4EUC)
  * [回應主體](#ID4EVD)
 
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主機名稱

gameserverds.xboxlive.com
 
<a id="ID4EBB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
當提出要求，所顯示下表中的標頭。
 
| 標頭| 值| 描述| 
| --- | --- | --- | 
| Content-Type| 應用程式/json| 正在提交的資料型別。| 
| 主機| gameserverds.xboxlive.com|  | 
| Content-Length|  | 要求物件的長度。| 
| x-xbl-contract-version| 1| API 合約版本。| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
回應一定會包含下表所示的標頭。
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| 應用程式/json| 回應主體中的資料型別。| 
| Content-Length|  | 回應主體的長度。| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>回應本文
 
如果呼叫成功，服務會傳回 JSON 物件，具有下列成員。
 
| 成員| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| qosservers| 伺服器資訊的陣列。| 
| serverFqdn| 伺服器的完整的網域名稱。| 
| serverSecureDeviceAddress| 伺服器的安全的裝置位址。| 
| targetLocation| 伺服器的地理位置。| 
 
<a id="ID4EUE"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{ 
  "qosServers" : 
  [ 
    { "serverFqdn" : "xblqosncus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "North Central US" },
    { "serverFqdn" : "xblqoswus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "West US" },
  ]
}

      
```

   
<a id="ID4EBF"></a>

 
## <a name="see-also"></a>請參閱
 [/qosservers](uri-qosservers.md)

  