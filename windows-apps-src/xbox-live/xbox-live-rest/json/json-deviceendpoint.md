---
title: DeviceEndpoint (JSON)
assetID: bd6c4af8-e491-8885-970e-e53d1d60642b
permalink: en-us/docs/xboxlive/rest/json-deviceendpoint.html
description: " DeviceEndpoint (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0eaa21072ebf14b6f6d959ff40af34724a45522f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618873"
---
# <a name="deviceendpoint-json"></a>DeviceEndpoint (JSON)
 
<a id="ID4EO"></a>

 
## <a name="deviceendpoint"></a>DeviceEndpoint
 
DeviceEndpoint 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| deviceName| 字串| 選用。 如果適用的裝置易記名稱。 目前不會使用此值。| 
| endpointUri| 字串| 必要。 用戶端平台 （Windows 或 Windows Phone） 已從其推播通知服務 （WNS 或 MPNS） 取得 URL。| 
| 地區設定| 字串| 必要。 此端點來傳送通知所需的語言。 可以是一份以逗號分隔值喜好設定順序。 範例:"DE-DE、 EN-US，en"。| 
| 平台| 字串| 選用。 目前支援的值為"W"和"Windows"。 如果未指定，它被衍生自裝置權杖。| 
| platformVersion| 字串| 選用。 此字串的格式是專門針對每個平台。 目前不會使用此值。| 
| systemId| GUID| 必要。 「 應用程式的執行個體 」 的唯一識別碼 （裝置/使用者組合）。 最佳作法是實作是產生隨機 GUID 時安裝/首次執行，讓應用程式，並繼續進行後續的執行，應用程式中使用該值。| 
| titleId| 32 位元不帶正負號的整數| 必要。 標題的識別碼發出對服務呼叫的遊戲。| 
  
<a id="ID4EGD"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
  "systemId": "e9af05b4-70de-4920-880f-026c6fb67d1b",
  "userId" : 1234567890
  "endpointUri": "https://cloud.notify.windows.com/.../",
  "platform": "Windows",
  "platformVersion": "1.0",
  "deviceName": "Test Endpoint",
  "locale": "en-US",
  "titleId": 1297290219
}
    
```

  
<a id="ID4EPD"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ERD"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>參考   