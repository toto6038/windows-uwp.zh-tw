---
ms.assetid: F37C2CEC-9ED1-4F9E-883D-9FBB082504D4
description: 在 Microsoft Store 購買 API 中使用此方法，變更使用者訂閱的帳單狀態。
title: 變更使用者訂閱的帳單狀態
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 購買 API, 訂閱
ms.localizationpriority: medium
ms.openlocfilehash: 4d2558dec8422e198477c30097c20a875addeea4
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619494"
---
# <a name="change-the-billing-state-of-a-subscription-for-a-user"></a>變更使用者訂閱的帳單狀態

在 Microsoft Store 購買 API 中使用此方法，變更特定使用者訂閱附加元件的帳單狀態。 您可以取消、延長、退款或停用訂閱的自動續約。

> [!NOTE]
> Microsoft 佈建的開發人員帳戶才可以使用此方法，建立通用 Windows 平台 (UWP) 應用程式的訂閱附加元件。 訂閱附加元件目前並未提供給大部分開發人員帳戶使用。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您將需要：

* 一個具有對象 URI 值 `https://onestore.microsoft.com` 的 Azure AD 存取權杖。
* Microsoft Store 識別碼金鑰，表示使用者的身分識別，他有您想要變更之訂閱的權利。

如需詳細資訊，請查看[管理服務的產品權利](view-and-grant-products-from-a-service.md)。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/{recurrenceId}/change``` |


### <a name="request-header"></a>要求標頭

| 標頭         | 類型   | 描述   |
|----------------|--------|-------------|
| 授權  | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。                           |
| 主機           | 字串 | 其值必須設定為 **purchase.mp.microsoft.com**。                                            |
| Content-Length | number | 要求本文的長度。                                                                       |
| Content-Type   | 字串 | 指定要求及回應類型。 目前唯一支援的值為 **application/json**。 |


### <a name="request-parameters"></a>要求參數

| 名稱         | 類型  | 描述   |  必要  |
|----------------|--------|-------------|-----------|
| recurrenceId | 字串 | 您想要變更之訂閱的識別碼。 若要取得此識別碼，請呼叫使用者方法的 [get 訂用](get-subscriptions-for-a-user.md) 帳戶，識別代表您要變更之訂閱附加元件的回應本文專案，並使用該專案的 **ID** 欄位值。     | Yes      |


### <a name="request-body"></a>要求本文

| 欄位      | 類型   | 描述   | 必要 |
|----------------|--------|---------------|----------|
| b2bKey         | 字串 | [Microsoft Store 識別碼金鑰](view-and-grant-products-from-a-service.md#step-4)，代表您要變更其訂閱的使用者身分識別。     | Yes      |
| changeType     | 字串 |  下列其中一個字串，辨識您想要進行的變更類型：<ul><li>**Cancel**：取消訂閱。</li><li>**Extend**：延長訂閱。 如果您指定這個值，必須也包含 *extensionTimeInDays* 參數。</li><li>**Refund**：將訂閱費用退款給客戶。</li><li>**ToggleAutoRenew**：停用訂閱的自動續約。 如果訂閱目前已停用自動續約，這個值不會執行任何動作。</li></ul>   | Yes      |
| extensionTimeInDays  | 字串  | 如果 *changeType* 參數有值 **Extend**，此參數指定延長訂閱的天數。 |  如果 *changeType* 有值 **Extend** 則為「是」，否則為「否」。  |


### <a name="request-example"></a>要求範例

以下範例示範如何使用此方法，將訂閱期間延長 5 天。 使用值 [Microsoft Store 識別碼金鑰](view-and-grant-products-from-a-service.md#step-4) (代表您要變更其訂閱的使用者身分識別) 取代 *b2bKey*。

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac/change HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ...",
  "changeType": "Extend",
  "extensionTimeInDays": "5"
}
```


## <a name="response"></a>回應

這個方法會傳回 JSON 回應主體，包含已修改訂閱附加元件的相關資訊，包括任何已修改的欄位。 以下範例示範這個方法的回應主體。

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-16T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-10T21:08:13.1459644+00:00",
      "market":"US",
      "productId":"9NBLGGH52Q8X",
      "skuId":"0024",
      "startTime":"2017-01-10T21:07:49.2552941+00:00",
      "recurrenceState":"Active"
    }
  ]
}
```


### <a name="response-body"></a>回應本文

回應主體包含下列資料。

| 值        | 類型   | Description                                                                 |
|---------------|--------|-----------------------------------------------|
| autoRenew | Boolean |  指出訂閱是否設定為訂閱期間結束時自動續約。   |
| beneficiary | 字串 |  這個訂閱相關聯的權利的受益人識別碼。   |
| expirationTime | 字串 | 訂閱將結束的日期和時間 (ISO 8601 格式)。 訂閱處於特定狀態時，才可以使用這個欄位。 到期時間通常表示目前狀態到期的時間。 例如，對於使用中的訂閱，到期日表示將會發生下一個自動續約的時間。    |
| expirationTimeWithGrace | 字串 | 訂用帳戶將到期的日期和時間，包含以 ISO 8601 格式的寬限期。 此值指出當訂用帳戶無法自動續約之後，使用者將會失去訂閱的存取權。    |
| id | 字串 |  訂閱的識別碼。 當您呼叫[變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)方法，使用這個值，指出您想要修改的訂閱。    |
| isTrial | Boolean |  表示訂閱是否試用。     |
| lastModified | 字串 |  上次修改訂閱的日期和時間 (ISO 8601 格式)。      |
| market | 字串 | 使用者取得訂閱所在的國家/地區代碼（兩個字母 ISO 3166-1 alpha-2 格式）。      |
| productId | 字串 | [產品](in-app-purchases-and-trials.md#products-skus-and-availabilities)的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)，代表 Microsoft Store 型錄中訂閱附加元件。 例如，產品的 Store 識別碼是 9NBLGGH42CFD。     |
| skuId | 字串 |  [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) 的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)，代表 Microsoft Store 型錄中訂閱附加元件。 例如，SKU 的 Store 識別碼是 0010。    |
| startTime | 字串 |  訂閱的開始日期和時間，格式為 ISO 8601。     |
| recurrenceState | 字串  |  下列其中一個值：<ul><li>**無**： &nbsp; &nbsp; 這表示永久訂用帳戶。</li><li>作用 &nbsp; &nbsp; 中：訂用帳戶為使用中狀態，且使用者有權使用這些服務。</li><li>**非** 作用中：訂用帳戶 &nbsp; &nbsp; 超過到期日，而且使用者已關閉訂用帳戶的自動續約選項。</li><li>**已取消**： &nbsp; &nbsp; 訂用帳戶已特意在到期日之前終止（含或不含退款）。</li><li>**InDunning**： &nbsp; &nbsp; 訂用帳戶位於 *欠款* (也就是訂用帳戶即將到期，而 Microsoft 正在嘗試取得資金以自動更新訂用帳戶) 。</li><li>**Failed**： &nbsp; &nbsp; 欠款期限已結束，且訂用帳戶在多次嘗試之後無法更新。</li></ul><p>**注意：**</p><ul><li>**非** / 作用中 **已取消** /**失敗** 為終端狀態。 當訂閱進入這些狀態時，使用者必須重新購買訂閱以再次啟動訂閱。 使用者無權使用這些狀態的服務。</li><li>當訂閱為 **Canceled**，expirationTime 會以取消日期和時間更新。</li><li>訂閱的識別碼在整個生命週期會維持不變。 如果自動續約選項已開啟或關閉，它不會變更。 如果使用者到達終止狀態之後重新購買訂閱，將會建立新的訂閱識別碼。</li><li>訂閱的識別碼應該用來執行任何個別訂閱作業。</li><li>當使用者在取消或終止訂閱之後重新購買訂閱，如果您查詢使用者的結果，您將會取到兩個項目：一個是終止狀態的舊訂閱識別碼，另一個是使用中狀態的新訂閱識別碼。</li><li>檢查 recurrenceState 和 expirationTime 是良好的做法，因為更新 recurrenceState 可能會延遲片刻（偶爾會延遲數小時）。       |
| cancellationDate | 字串   |  使用者取消訂閱的日期和時間 (ISO 8601 格式)。     |


## <a name="related-topics"></a>相關主題


* [管理服務的產品權利](view-and-grant-products-from-a-service.md)
* [取得使用者訂閱](get-subscriptions-for-a-user.md)
* [查詢產品](query-for-products.md)
* [將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)
* [更新 Microsoft Store 識別碼金鑰](renew-a-windows-store-id-key.md)
