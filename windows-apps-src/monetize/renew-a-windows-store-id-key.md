---
author: mcleanbyron
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: 請使用這個方法來更新 Windows 市集索引鍵。
title: 更新 Windows 市集識別碼索引鍵
---

# 更新 Windows 市集識別碼索引鍵


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

請使用這個方法來更新 Windows 市集索引鍵。 當您藉由呼叫 [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) 或 [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) 方法來產生 Windows 市集識別碼索引鍵時，該索引鍵會有效 90 天。 索引鍵到期之後，這個方法可讓您使用過期的索引鍵來重新交涉，以取得新的索引鍵。

## 先決條件


若要使用這個方法，您將需要：

-   先前利用 **https://onestore.microsoft.com** 對象 URI 所建立的 Azure AD 存取權杖。
-   藉由從 app 中用戶端程式碼來呼叫 [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) 或 [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) 方法，所產生的過期 Windows 市集識別碼索引鍵。

如需詳細資訊，請參閱 [從服務檢視及授與產品](view-and-grant-products-from-a-service.md)。

## 要求


### 要求的語法

| 索引鍵類型    | 方法 | 要求 URI                                              |
|-------------|--------|----------------------------------------------------------|
| 集合 | POST   | https://collections.mp.microsoft.com/v6.0/b2b/keys/renew |
| 購買    | POST   | https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew    |

 

### 要求的標頭

| 標頭         | 類型   | 說明                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | 字串 | 其值必須設定為 **collections.mp.microsoft.com** 或 **purchase.mp.microsoft.com**。           |
| Content-Length | 數字 | 要求主體的長度。                                                                       |
| Content-Type   | 字串 | 指定要求及回應類型。 目前唯一支援的值為 **application/json**。 |

 

### 要求主體

| 參數     | 類型   | 描述                       | 必要 |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | 字串 | Azure AD 存取權杖。        | 是      |
| key           | 字串 | 過期的 Windows 市集識別碼索引鍵。 | 否       |

 

### 要求的範例

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

## 回應


### 回應主體

| 參數 | 類型   | 描述                                                                                                            | 必要 |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| key       | 字串 | 更新的 Windows 市集索引鍵，可在未來呼叫 Windows 市集集合 API 或購買 API 時使用。 | 否       |

 

### 回應的範例

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

## 錯誤碼


| 代碼 | 錯誤        | 內部錯誤碼           | 說明                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Unauthorized | AuthenticationTokenInvalid | Azure AD 存取權杖無效。 在某些情況下，ServiceError 的詳細資料會包含更多資訊，例如權杖過期或 *appid* 宣告遺失時。 |
| 401  | Unauthorized | InconsistentClientId       | Windows 市集識別碼索引鍵的 *clientId* 宣告，和 Azure AD 存取權杖的 *appid* 不相符。                                                                     |

 

## 相關主題


* [從服務檢視及授與產品](view-and-grant-products-from-a-service.md)
* [查詢產品](query-for-products.md)
* [將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)
* [授與免費產品](grant-free-products.md)



<!--HONumber=May16_HO2-->


