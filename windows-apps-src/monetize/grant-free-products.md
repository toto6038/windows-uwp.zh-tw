---
author: Xansky
ms.assetid: FA55C65C-584A-4B9B-8451-E9C659882EDE
description: 在 Microsoft Store 購買 API 中使用這個方法，將免費 App 或附加元件授與指定的使用者。
title: 授與免費產品
ms.author: mhopkins
ms.date: 03/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 購買 API, 授與產品
ms.localizationpriority: medium
ms.openlocfilehash: 432d5976cb018148ba0f53aae6446a046f0a3b2f
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5407019"
---
# <a name="grant-free-products"></a>授與免費產品

在 Microsoft Store 購買 API 中使用這個方法，將免費 App 或附加元件 (也稱為應用程式內產品或 IAP) 授與指定的使用者。

目前您只能授與免費的產品。 如果您的服務嘗試使用這個方法來授與非免費的產品，這個方法就會傳回錯誤。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您將需要：

* 一個具有對象 URI 值 `https://onestore.microsoft.com` 的 Azure AD 存取權杖。
* 一個 Microsoft Store 識別碼金鑰，代表您要授與免費產品的使用者身分。

如需詳細資訊，請查看[管理服務的產品權利](view-and-grant-products-from-a-service.md)。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v6.0/purchases/grant``` |


### <a name="request-header"></a>要求的標頭

| 標頭         | 類型   | 描述                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 授權  | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。                           |
| Host           | 字串 | 其值必須設定為 **purchase.mp.microsoft.com**。                                            |
| Content-Length | 數字 | 要求主體的長度。                                                                       |
| Content-Type   | 字串 | 指定要求及回應類型。 目前唯一支援的值為 **application/json**。 |


### <a name="request-body"></a>要求主體

| 參數      | 類型   | 描述        | 必要 |
|----------------|--------|---------------------|----------|
| availabilityId | 字串 | 要從 Microsoft Store 型錄授與之產品的可用性識別碼。         | 是      |
| b2bKey         | 字串 | [Microsoft Store 識別碼金鑰](view-and-grant-products-from-a-service.md#step-4)，代表您要授與產品的使用者身分識別。    | 是      |
| devOfferId     | 字串 | 會在購買之後出現在集合項目中的開發人員特定優惠識別碼。        |
| language       | 字串 | 使用者所用的語言。  | 是      |
| market         | 字串 | 使用者所在的市場。       | 是      |
| orderId        | guid   | 為訂單所產生的 GUID。 此值對使用者來說是唯一的，但在所有訂單中並不需要是唯一的。    | 是      |
| productId      | 字串 | Microsoft Store 目錄中[產品](in-app-purchases-and-trials.md#products-skus-and-availabilities)的 [ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 例如，產品的 Store 識別碼是 9NBLGGH42CFD。 | 是      |
| quantity       | 整數    | 要購買的數量。 目前唯一支援的值為 1。 如果沒有指定，則預設為 1。   | 否       |
| skuId          | 字串 | Microsoft Store 目錄中[產品 SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities)的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 例如，SKU 的 Store 識別碼是 0010。     | 是      |


### <a name="request-example"></a>要求範例

```syntax
POST https://purchase.mp.microsoft.com/v6.0/purchases/grant HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJK……
Content-Length: 1863
Content-Type: application/json

{
    "b2bKey" : "eyJ0eXAiOiJK……",
    "availabilityId" : "9RT7C09D5J3W",
    "productId" : "9NBLGGH5WVP6",
    "skuId" : "0010",
    "language" : "en-us",
    "market" : "us",
    "orderId" : "3eea1529-611e-4aee-915c-345494e4ee76",
}
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 參數                 | 類型                        | 描述             | 必要 |
|---------------------------|-----------------------------|-----------------------|----------|
| clientContext             | ClientContextV6             | 此訂單的用戶端內容相關資訊， 這將會指派給 Azure AD 權杖的 *clientID* 值。    | 是      |
| createdtime               | DateTimeOffset              | 訂單的建立時間。         | 是      |
| currencyCode              | 字串                      | *totalAmount* 和 *totalTaxAmount* 的貨幣代碼。 不適用於免費項目。     | 是      |
| friendlyName              | 字串                      | 訂單的易記名稱。 不適用於使用 Microsoft Store 購買 API 所建立的訂單。 | 是      |
| isPIRequired              | 布林值                     | 指出訂購單是否必須包含付款方式 (PI)。  | 是      |
| language                  | 字串                      | 訂單的語言識別碼 (例如「en」)。       | 是      |
| market                    | 字串                      | 訂單的市場識別碼 (例如「US」)。  | 是      |
| orderId                   | 字串                      | 可用來識別特定使用者之訂單的識別碼。                | 是      |
| orderLineItems            | list&lt;OrderLineItemV6&gt; | 訂單的明細項目清單。 通常是每個訂單 1 個項目。       | 是      |
| orderState                | 字串                      | 訂單的狀態。 有效的狀態為 **Editing**、**CheckingOut**、**Pending**、**Purchased**、**Refunded**、**ChargedBack** 及 **Cancelled**。 | 是      |
| orderValidityEndTime      | 字串                      | 在提交訂單定價之前，該訂單定價最後一次有效的時間。 不適用於免費應用程式。      | 是      |
| orderValidityStartTime    | 字串                      | 在提交訂單定價之前，該訂單定價第一次有效的時間。 不適用於免費應用程式。          | 是      |
| purchaser                 | IdentityV6                  | 描述購買者身分識別的物件。       | 是      |
| totalAmount               | 十進位                     | 訂單中所有項目含稅的總購買金額。       | 是      |
| totalAmountBeforeTax      | 十進位                     | 訂單中所有項目不含稅的總購買金額。      | 是      |
| totalChargedToCsvTopOffPI | 十進位                     | 如果使用不同的付款方式和儲存值 (CSV)，金額就會計入 CSV。            | 是      |
| totalTaxAmount            | 十進位                     | 所有明細項目的總稅金金額。    | 是      |


ClientContext 物件包含下列參數。

| 參數 | 類型   | 描述                           | 必要 |
|-----------|--------|---------------------------------------|----------|
| client    | 字串 | 建立訂單的用戶端識別碼。 | 否       |


OrderLineItemV6 物件包含下列參數。

| 參數               | 類型           | 描述                                                                                                  | 必要 |
|-------------------------|----------------|--------------------------------------------------------------------------------------------------------------|----------|
| agent                   | IdentityV6     | 上次編輯明細項目的代理程式。 如需此物件的詳細資訊，請參閱下表。       | 否       |
| availabilityId          | 字串         | 要從 Microsoft Store 型錄購買之產品的可用性識別碼。                           | 是      |
| beneficiary             | IdentityV6     | 訂單受益人的身分識別。                                                                | 否       |
| billingState            | 字串         | 訂單的帳單狀態。 完成時，此屬性會設定為 **Charged**。                                   | 否       |
| campaignId              | 字串         | 此訂單的行銷活動識別碼。                                                                              | 否       |
| currencyCode            | 字串         | 用於價格詳細資料的貨幣代碼。                                                                    | 是      |
| description             | 字串         | 當地語系化的明細項目說明。                                                                    | 是      |
| devofferId              | 字串         | 特定訂單的優惠識別碼 (如果有的話)。                                                           | 否       |
| fulfillmentDate         | DateTimeOffset | 履行發生時的日期。                                                                           | 否       |
| fulfillmentState        | 字串         | 此項目的履行狀態。 完成時，此屬性會設定為 **Fulfilled**。                      | 否       |
| isPIRequired            | 布林值        | 指出此明細項目是否需要付款方式。                                       | 是      |
| isTaxIncluded           | 布林值        | 指出該項目的價格詳細資料是否包含稅金。                                        | 是      |
| legacyBillingOrderId    | 字串         | 舊版的帳單識別碼。                                                                                       | 否       |
| lineItemId              | 字串         | 此訂單中項目的明細項目識別碼。                                                                 | 是      |
| listPrice               | 十進位        | 此訂單中項目的定價。                                                                    | 是      |
| productId               | 字串         | [產品](in-app-purchases-and-trials.md#products-skus-and-availabilities)的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)，代表 Microsoft Store 型錄中明細項目。 例如，產品的 Store 識別碼是 9NBLGGH42CFD。   | 是      |
| productType             | 字串         | 產品的類型。 支援的值為 **Durable**、**Application** 及 **UnmanagedConsumable**。 | 是      |
| quantity                | 整數            | 已訂購項目的數量。                                                                            | 是      |
| retailPrice             | 十進位        | 已訂購項目的零售價。                                                                        | 是      |
| revenueRecognitionState | 字串         | 收益辨識的狀態。                                                                               | 是      |
| skuId                   | 字串         | Microsoft Store 型錄中明細項目的 [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) 的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 例如，SKU 的 Store 識別碼是 0010。                                                                   | 是      |
| taxAmount               | 十進位        | 該明細項目的稅額。                                                                            | 是      |
| taxType                 | 字串         | 適用稅捐的稅捐類型。                                                                       | 是      |
| Title                   | 字串         | 當地語系化的明細項目標題。                                                                        | 是      |
| totalAmount             | 十進位        | 該明細項目含稅的總購金額。                                                    | 是      |


IdentityV6 物件包含下列參數。

| 參數     | 類型   | 描述                                                                        | 必要 |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | 字串 | 包含 **"pub"** 值。                                                      | 是      |
| identityValue | 字串 | 來自特定 Microsoft Store 識別碼金鑰之 *publisherUserId* 的字串值。 | 是      |


### <a name="response-example"></a>回應的範例

```syntax
Content-Length: 1203
Content-Type: application/json
MS-CorrelationId: fb2e69bc-f26a-4aab-a823-7586c19f5762
MS-RequestId: c1bc832c-f742-47e4-a76c-cf061402f698
MS-CV: XjfuNWLQlEuxj6Mt.8
MS-ServerId: 030032362
Date: Tue, 13 Oct 2015 21:21:51 GMT

{
    "clientContext": {
        "client": "86b78998-d05a-487b-b380-6c738f6553ea"
    },
    "createdTime": "2015-10-13T21:21:51.1863494+00:00",
    "currencyCode": "USD",
    "isPIRequired": false,
    "language": "en-us",
    "market": "us",
    "orderId": "3eea1529-611e-4aee-915c-345494e4ee76",
    "orderLineItems": [{
        "availabilityId": "9RT7C09D5J3W",
        "beneficiary": {
            "identityType": "pub",
            "identityValue": "user1"
        },
        "billingState": "Charged",
        "currencyCode": "USD",
        "description": "Jewels, Jewels, Jewels - Consumable 2",
        "fulfillmentDate": "2015-10-13T21:21:51.639478+00:00",
        "fulfillmentState": "Fulfilled",
        "isPIRequired": false,
        "isTaxIncluded": true,
        "lineItemId": "2814d758-3ee3-46b3-9671-4fb3bdae9ffe",
        "listPrice": 0.0,
        "payments": [],
        "productId": "9NBLGGH5WVP6",
        "productType": "UnmanagedConsumable",
        "quantity": 1,
        "retailPrice": 0.0,
        "revenueRecognitionState": "None",
        "skuId": "0010",
        "taxAmount": 0.0,
        "taxType": "NoApplicableTaxes",
        "title": "Jewels, Jewels, Jewels - Consumable 2",
        "totalAmount": 0.0
    }],
    "orderState": "Purchased",
    "orderValidityEndTime": "2015-10-14T21:21:51.1863494+00:00",
    "orderValidityStartTime": "2015-10-13T21:21:51.1863494+00:00",
    "purchaser": {
        "identityType": "pub",
        "identityValue": "user1"
    },
    "testScenarios": "None",
    "totalAmount": 0.0,
    "totalTaxAmount": 0.0
}
```

## <a name="error-codes"></a>錯誤碼


| 代碼 | 錯誤        | 內部錯誤碼           | 說明   |
|------|--------------|----------------------------|----------------|
| 401  | Unauthorized | AuthenticationTokenInvalid | Azure AD 存取權杖無效。 在某些情況下，ServiceError 的詳細資料會包含更多資訊，例如權杖過期或 *appid* 宣告遺失時。 |
| 401  | Unauthorized | PartnerAadTicketRequired   | Azure AD 存取權杖沒有傳遞到 Authorization 標頭中的服務。   |
| 401  | Unauthorized | InconsistentClientId       | 要求主體中 Microsoft Store 識別碼金鑰的 *clientId* 宣告，與授權標頭中 Azure AD 存取權杖的 *appid* 宣告不相符。       |
| 400  | BadRequest   | InvalidParameter           | 詳細資料包含要求主體的相關資訊，以及哪些欄位的值無效。           |

<span/> 

## <a name="related-topics"></a>相關主題

* [管理服務的產品權利](view-and-grant-products-from-a-service.md)
* [查詢產品](query-for-products.md)
* [將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)
* [更新 Microsoft Store 識別碼金鑰](renew-a-windows-store-id-key.md)
