---
author: Xansky
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: 請在 Microsoft Store 集合 API 中使用這個方法，來回報某個消費性產品對於特定客戶而言為已完成。 在使用者能再次購買某個消費性產品之前，您的應用程式或服務必須回報該消費性產品對於該使用者而言為已完成。
title: 將消費性產品回報為已完成
ms.author: mhopkins
ms.date: 03/16/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store collection API, fulfill, consumable, Microsoft Store 集合, 執行, 消費性產品
ms.localizationpriority: medium
ms.openlocfilehash: 69795a40ae5e46f95330cd6a6d2738adc9a74686
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7282178"
---
# <a name="report-consumable-products-as-fulfilled"></a>將消費性產品回報為已完成

請在 Microsoft Store 集合 API 中使用這個方法，來回報某個消費性產品對於特定客戶而言為已完成。 在使用者能再次購買某個消費性產品之前，您的應用程式或服務必須回報該消費性產品對於該使用者而言為已完成。

利用這個方法來將消費性產品回報為已完成的方法有兩種：

* 提供消費性產品的項目識別碼 (在[查詢產品](query-for-products.md)時所傳回的 **itemId** 參數中)，以及您提供的唯一追蹤識別碼。 如果您使用相同的追蹤識別碼來嘗試多次，即使該項目已遭取用，仍會傳回相同的結果。 如果您不確定某個取用要求是否已成功，您的服務應該要利用相同的追蹤識別碼來重新提交取用要求。 該追蹤識別碼會永遠繫結到該取用要求，且可以無限期地重新提交。
* 提供產品識別碼 (在[查詢產品](query-for-products.md)時所傳回的 **productId** 參數中)，以及從下方＜要求主體＞一節中 **transactionId** 參數的說明列出的某個來源所取得的交易識別碼。

## <a name="prerequisites"></a>先決條件


若要使用這個方法，您將需要：

* 一個具有對象 URI 值 `https://onestore.microsoft.com` 的 Azure AD 存取權杖。
* Microsoft Store 識別碼金鑰，代表您要回報消費性產品已完成之使用者的身分。

如需詳細資訊，請查看[管理服務的產品權利](view-and-grant-products-from-a-service.md)。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                   |
|--------|---------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |


### <a name="request-header"></a>要求的標頭

| 標頭         | 類型   | 描述                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 授權  | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。                           |
| Host           | 字串 | 其值必須設定為 **collections.mp.microsoft.com**。                                            |
| Content-Length | 數字 | 要求主體的長度。                                                                       |
| Content-Type   | 字串 | 指定要求及回應類型。 目前唯一支援的值為 **application/json**。 |


### <a name="request-body"></a>要求主體

| 參數     | 類型         | 描述         | 必要 |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | 取用此項目時所針對的使用者。 如需詳細資訊，請參閱下表。        | 是      |
| itemId        | 字串       | [查詢產品](query-for-products.md)時所傳回的 *itemId* 值。 請搭配 *trackingId* 來使用這個參數      | 否       |
| trackingId    | guid         | 開發人員所提供的唯一追蹤識別碼。 請搭配 *itemId* 來使用這個參數。         | 否       |
| productId     | 字串       | [查詢產品](query-for-products.md)時所傳回的 *productId* 值。 請搭配 *transactionId* 來使用這個參數   | 否       |
| transactionId | guid         | 從下列其中一個來源取得的交易識別碼值。 請搭配 *productId* 來使用這個參數。<ul><li>[PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) 類別的 [TransactionID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults.transactionid) 屬性。</li><li>由 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)、[RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 或 [GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) 所傳回的 app 或產品收據。</li><li>[查詢產品](query-for-products.md)時所傳回的 *transactionId* 參數。</li></ul>   | 否       |


UserIdentity 物件包含下列參數。

| 參數            | 類型   | 描述       | 必要 |
|----------------------|--------|-------------------|----------|
| identityType         | 字串 | 指定字串值 **b2b**。    | 是      |
| identityValue        | 字串 | [Microsoft Store 識別碼金鑰](view-and-grant-products-from-a-service.md#step-4)，代表您要回報消費性產品已完成之使用者的身分。      | 是      |
| localTicketReference | 字串 | 所傳回回應的要求識別碼。 建議您使用相同值做為在 Microsoft Store 識別碼索引鍵中[宣告](view-and-grant-products-from-a-service.md#claims-in-a-microsoft-store-id-key)的 *userId*。 | 是      |


### <a name="request-examples"></a>要求範例

下列範例使用 *itemId* 和 *trackingId*。

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

下列範例使用 *productId* 和 *transactionId*。

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```


## <a name="response"></a>回應

如果取用執行成功，就不會傳回任何內容。

### <a name="response-example"></a>回應的範例

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## <a name="error-codes"></a>錯誤碼


| 代碼 | 錯誤        | 內部錯誤碼           | 說明           |
|------|--------------|----------------------------|-----------------------|
| 401  | Unauthorized | AuthenticationTokenInvalid | Azure AD 存取權杖無效。 在某些情況下，ServiceError 的詳細資料會包含更多資訊，例如權杖過期或 *appid* 宣告遺失時。 |
| 401  | Unauthorized | PartnerAadTicketRequired   | Azure AD 存取權杖沒有傳遞到 Authorization 標頭中的服務。                                                                                                   |
| 401  | Unauthorized | InconsistentClientId       | 要求主體中 Microsoft Store 識別碼金鑰的 *clientId* 宣告，與授權標頭中 Azure AD 存取權杖的 *appid* 宣告不相符。                     |

<span/> 

## <a name="related-topics"></a>相關主題

* [管理服務的產品權利](view-and-grant-products-from-a-service.md)
* [查詢產品](query-for-products.md)
* [授與免費產品](grant-free-products.md)
* [更新 Microsoft Store 識別碼金鑰](renew-a-windows-store-id-key.md)
