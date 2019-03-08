---
title: GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
assetID: 613ba53f-03cb-5ed3-a5ba-be59e5a146d1
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus-get.html
description: " GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 793d634bc1e3dc431b3797759751afb6dfd9c00a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650853"
---
# <a name="get-titlestitleidsessionssessionidallocationstatus"></a>GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
傳回由其 sessionId sessionhost 配置狀態。 這些 uri 的網域`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [必要的要求標頭](#ID4E4)
  * [必要的回應標頭](#ID4EEB)
  * [回應主體](#ID4ELB)
 
<a id="ID4E4"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
無。
  
<a id="ID4EEB"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
無。
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>回應本文
 
如果呼叫成功，服務會傳回 JSON 物件，具有下列成員。
 
| 成員| 描述| 
| --- | --- | 
| description| 傳回空字串 （保留在針對回溯相容性）。| 
| clusterId| 傳回空字串 （保留在針對回溯相容性）。| 
| hostName| 工作階段主機的 URL。| 
| status| 表示排入佇列、 完成，或已中止。| 
| sessionHostId| 工作階段主機識別碼。| 
| sessionId| （在配置階段） 提供的用戶端工作階段識別碼。| 
| secureContext| 安全的裝置的位址。| 
| portMappings| 執行個體的連接埠對應。| 
| 區域| 執行個體的位置。| 
| ticketId| 目前的工作階段識別碼 （保留在針對回溯相容性）。| 
| gameHostId| 目前的 sessionHostId （保留在針對回溯相容性）。| 
 
<a id="ID4EGD"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
        "hostName": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.cloudapp.net",
        "portMappings": [
        {
        "Key": "GSDKTCPTest",
        "Value": {
        "internal": 10100,
        "external": 10103
        }
        },
        {
        "Key": "GSDKUDPTest",
        "Value": {
        "internal": 5000,
        "external": 5000
        }
        }
        ],
        "status",:"Fulfilled",
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g==",
        "sessionId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "description": "",
        "clusterId": "",
        "sessionHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0",
        "ticketId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "gameHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0"

      
```

  
<a id="remarks"></a>

 
### <a name="remarks"></a>備註
 
標題只應該重試呼叫服務時收到下列的回應碼：
 
   * 200-成功 
   * 400-要求包含無效的參數 
   * 401-未經授權 
   * 404-標題識別碼或票證識別碼已無效或找不到 
   * 500-未預期的伺服器錯誤。 
    