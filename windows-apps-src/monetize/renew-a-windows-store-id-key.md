---
author: mcleanbyron
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: "請使用這個方法來更新 Windows 市集金鑰。"
title: "更新 Windows 市集識別碼金鑰"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows Store collection API, Windows Store purchase API, Windows Store ID key, renew, Windows 市集集合 API, Windows 市集購買 API, Windows 市集識別碼金鑰, 更新"
ms.openlocfilehash: 22db5f1ae693c26ecf727c94a9f6746225325f74
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="renew-a-windows-store-id-key"></a>更新 Windows 市集識別碼金鑰


請使用這個方法來更新 Windows 市集金鑰。 當您[產生 Windows 市集識別碼金鑰](view-and-grant-products-from-a-service.md#step-4)時，該金鑰的有效期為 90 天。 金鑰到期之後，您可以藉由這個方法，使用過期的金鑰來重新交涉以取得一個新的金鑰。

## <a name="prerequisites"></a>先決條件


若要使用這個方法，您將需要：

* 一個利用 `https://onestore.microsoft.com` 對象 URI 建立的 Azure AD 存取權杖。
* 一個[從您 App 中用戶端程式碼產生](view-and-grant-products-from-a-service.md#step-4)的過期「Windows 市集識別碼」金鑰。

如需詳細資訊，請查看[管理服務的產品權利](view-and-grant-products-from-a-service.md)。

## <a name="request"></a>要求

### <a name="request-syntax"></a>要求的語法

| 金鑰類型    | 方法 | 要求 URI                                              |
|-------------|--------|----------------------------------------------------------|
| 集合 | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| 購買    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |

<span/>

### <a name="request-header"></a>要求的標頭

| 標頭         | 類型   | 說明                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | 字串 | 其值必須設定為 **collections.mp.microsoft.com** 或 **purchase.mp.microsoft.com**。           |
| Content-Length | 數字 | 要求主體的長度。                                                                       |
| Content-Type   | 字串 | 指定要求及回應類型。 目前唯一支援的值為 **application/json**。 |

<span/>

### <a name="request-body"></a>要求主體

| 參數     | 類型   | 描述                       | 必要 |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | 字串 | Azure AD 存取權杖。        | 是      |
| key           | 字串 | 過期的 Windows 市集識別碼金鑰。 | 否       |

<span/> 

### <a name="request-example"></a>要求的範例

```syntax
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 參數 | 類型   | 描述                                                                                                            | 必要 |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| key       | 字串 | 更新的 Windows 市集金鑰，可在未來呼叫 Windows 市集集合 API 或購買 API 時使用。 | 否       |

<span/>

### <a name="response-example"></a>回應的範例

```syntax
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## <a name="error-codes"></a>錯誤碼


| 代碼 | 錯誤        | 內部錯誤碼           | 說明                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Unauthorized | AuthenticationTokenInvalid | Azure AD 存取權杖無效。 在某些情況下，ServiceError 的詳細資料會包含更多資訊，例如權杖過期或 *appid* 宣告遺失時。 |
| 401  | Unauthorized | InconsistentClientId       | Windows 市集識別碼金鑰的 *clientId* 宣告，和 Azure AD 存取權杖的 *appid* 不相符。                                                                     |

<span/>

## <a name="related-topics"></a>相關主題


* [管理服務的產品權利](view-and-grant-products-from-a-service.md)
* [查詢產品](query-for-products.md)
* [將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)
* [授與免費產品](grant-free-products.md)
