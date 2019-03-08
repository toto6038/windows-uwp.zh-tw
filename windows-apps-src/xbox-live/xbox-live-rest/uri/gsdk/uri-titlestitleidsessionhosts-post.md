---
title: POST (/titles/{Title Id}/sessionhosts)
assetID: 8558b336-1af9-8143-9752-477ceb3a8e4e
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionhosts-post.html
description: " POST (/titles/{Title Id}/sessionhosts)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47e3ecbf0a519b92ae467199e5d454523864310a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655143"
---
# <a name="post-titlestitle-idsessionhosts"></a>POST (/titles/{Title Id}/sessionhosts)
建立新叢集的要求。 這些 Uri 的網域是`gameserverms.xboxlive.com`。
 
  * [URI 參數](#ID4EX)
  * [必要的要求標頭](#ID4EGB)
  * [要求本文](#ID4E5B)
  * [必要的回應標頭](#ID4ELD)
  * [回應主體](#ID4ESD)
 
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
| Content-Type| 應用程式/json| 正在提交的資料型別。| 
  
<a id="ID4E5B"></a>

 
## <a name="request-body"></a>要求本文
 
要求必須包含具有下列成員的 JSON 物件。
 
| 成員| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| 這是呼叫端所指定識別項。 它會指派給會配置並傳回的工作階段主機。 稍後您可以參考特定 sessionhost 由這個識別項。 它必須是全域唯一 (也就是 GUID)。| 
| SandboxId| 您想要配置給在工作階段主機沙箱。| 
| cloudGameId| 雲端遊戲識別項。| 
| 位置| 您想要從配置的工作階段的慣用位置的已排序的清單。| 
| sessionCookie| 這是指定的呼叫端不透明的字串。 它與 sessionhost 相關聯，而且可以在您的遊戲程式碼中參考。 您可以使用這個成員，從用戶端的少量資訊傳遞到伺服器 （大小上限為 4 KB）。| 
| gameModelId| 遊戲模式識別項。| 
 
<a id="ID4EDD"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
{
        "sessionId": "3536d3e6-e85d-4f47-b898-9617d19dabcd",
        "sandboxId": "ISST.0",
        "cloudGameId": "1b7f9925-369c-4301-b1f7-1125dce25776",
        "locations": [
        "West US",
        "East US",
        "West Europe"
        ],
        "sessionCookie": "Caller provided opaque string",
        "gameModeId": "2162d32c-7ac8-40e9-9b1f-56676b8b2513"
        }
      
```

   
<a id="ID4ELD"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
無。
  
<a id="ID4ESD"></a>

 
## <a name="response-body"></a>回應本文
 
如果呼叫成功，服務會傳回 JSON 物件，具有下列成員。
 
| 成員| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| hostName| 執行個體的主機名稱。| 
| portMappings| 連接埠的對應。| 
| 區域| 在裝載區域的執行個體。| 
| secureContext| 安全的裝置的位址。| 
 
<a id="ID4ESE"></a>

 
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
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g=="
        }
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>備註
 
標題只應該重試呼叫服務時收到下列的回應碼：
 
   * 200： 成功-傳回回應。
   * 400-無效的參數或格式不正確的要求主體。
   * 401-未經授權
   * 404-標題 id 沒有任何指派給它的訂用帳戶。
   * 409，當相同的要求都會在大致上相同的時間 (相同的 sessionId) 時，此回應有可能。 如果提出配置要求和工作階段主機已經有指定的工作階段識別碼，而且已在使用我們將會傳回詳細描述該 sessionhost 的資訊。 如果工作階段主機不過，不是作用中，但您會收到發生衝突。
   * 500-未預期的伺服器錯誤。
   * 503-沒有 sessionhosts StandingBy。 其中一些資源可用時，請重試要求。
   
<a id="ID4EFG"></a>

 
## <a name="see-also"></a>請參閱
 [/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

  