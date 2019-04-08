---
description: 若要取得附加元件訂用帳戶在指定的日期範圍和其他選用的篩選器擷取資料，Microsoft Store analytics API 中使用這個方法。
title: 取得訂閱附加元件下載數
ms.date: 01/25/18
ms.topic: article
keywords: windows 10、 uwp、 存放區服務、 Microsoft Store 分析 API，訂用帳戶
ms.openlocfilehash: e33a3ded219fb4d223137b40ebe871f66589addf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594773"
---
# <a name="get-subscription-add-on-acquisitions"></a>取得訂閱附加元件下載數

若要取得您的應用程式的附加元件訂用帳戶中的彙總取得資料，在指定的日期範圍和其他選用的篩選條件期間，Microsoft Store analytics API 中使用這個方法。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述          |
|---------------|--------|--------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | [存放區識別碼](in-app-purchases-and-trials.md#store-ids)您要擷取訂用帳戶的附加元件取得資料的應用程式。 |  是  |
| subscriptionProductId  | 字串 | [存放區識別碼](in-app-purchases-and-trials.md#store-ids)訂用帳戶附加元件，您要擷取取得資料。 如果您未指定此值，這個方法會傳回所有訂用帳戶的附加元件，為指定的應用程式的 取得資料。  | 否  |
| startDate | date | 擷取訂用帳戶的附加元件取得資料的日期範圍的開始日期。 預設為目前的日期。 |  否  |
| endDate | date | 擷取訂用帳戶的附加元件取得資料的日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 要在要求中傳回的資料列數目。 最大值，如果未指定的預設值為 100。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來循頁瀏覽大型資料集。 例如，top=100 且 skip=0 時，會擷取前 100 列資料，top=100 且 skip=100 時，會擷取下 100 列資料，以此類推。 |  否  |
| filter | 字串  | 一或多個篩選回應本文的陳述式。 每個陳述式都可以使用 **eq** 或 **ne** 運算子，而陳述式之間可以使用 **and** 或 **or** 來結合。 您可以在 篩選陳述式中指定下列字串 (這些會對應至[回應主體中的值](#subscription-acquisition-values)): <ul><li><strong>日期</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>應用程式名稱</strong></li><li><strong>SkuId</strong></li><li><strong>市場</strong></li><li><strong>裝置類型</strong></li></ul><p>以下是範例*篩選條件*參數：<em>篩選 = 日期 eq ' 2017年-07-08'</em>。</p> | 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。 | 否 |
| orderby | 字串 | 排序結果的每個訂用帳戶的附加元件取得資料值的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>。<em>field</em> 參數可以是下列其中一個字串：<ul><li><strong>日期</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>應用程式名稱</strong></li><li><strong>SkuId</strong></li><li><strong>市場</strong></li><li><strong>裝置類型</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li><strong>日期</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>應用程式名稱</strong></li><li><strong>SkuId</strong></li><li><strong>市場</strong></li><li><strong>裝置類型</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如： <em>groupby = 市場&amp;aggregationLevel = 週</em></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範如何取得訂用帳戶的附加元件取得資料。 取代*applicationId*具有適當的存放區識別碼，應用程式的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述         |
|------------|--------|------------------|
| 值      | 陣列  | 包含彙總的訂用帳戶的附加元件取得資料的物件陣列。 如需每個物件中資料的詳細資訊，請參閱[訂用帳戶擷取值](#subscription-acquisition-values)下一節。             |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，這個值會傳回如果**頂端**要求參數設定為 100，但有 100 個以上的資料列的訂用帳戶的附加元件取得資料的查詢。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>訂用帳戶擷取值

*Value* 陣列中的元素包含下列值。

| 值               | 類型    | 描述        |
|---------------------|---------|---------------------|
| date                | 字串  | 下載數資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| subscriptionProductId      | 字串  | [存放區識別碼](in-app-purchases-and-trials.md#store-ids)訂用帳戶附加元件，您要為其擷取取得資料。    |
| subscriptionProductName    | 字串  | 訂用帳戶的附加元件的顯示名稱。         |
| applicationId       | 字串  | [存放區識別碼](in-app-purchases-and-trials.md#store-ids)您擷取訂用帳戶的附加元件取得資料的應用程式。   |
| applicationName     | 字串  | 應用程式的顯示名稱。     |
| skuId     | 字串  | 識別碼[SKU](in-app-purchases-and-trials.md#products-skus)訂用帳戶附加元件，您要為其擷取取得資料。     |
| deviceType          | 字串  |  以下字串之一指定完成收購的裝置類型：<ul><li><strong>PC</strong></li><li><strong>電話</strong></li><li><strong>主控台</strong></li><li><strong>IoT</strong></li><li><strong>全像攝影版</strong></li><li><strong>未知</strong></li></ul>       |
| market           | 字串  | 發生下載之市場的 ISO 3166 國家/地區碼。     |
| currencyCode         | 字串  | 格式之前稅金的銷售毛額的 ISO 4217 貨幣代碼。       |
| grossSalesBeforeTax           | 整數  | 中所指定的本地貨幣的銷售毛額*currencyCode*值。     |
| totalActiveCount             | 整數  | 總作用中的訂用帳戶在指定的時段數目。 這相當於總和*goodStandingActiveCount*， *pendingGraceActiveCount*， *graceActiveCount*，和*lockedActiveCount*值。  |
| totalChurnCount              | 整數  | 在指定的時段內已停用的訂用帳戶的總數。 這相當於總和*billingChurnCount*， *nonRenewalChurnCount*， *refundChurnCount*， *chargebackChurnCount*， *earlyChurnCount*，並*otherChurnCount*值。   |
| newCount            | 整數  | 在指定的時間期間，包括試用版的新訂用帳戶取得的數字。    |
| renewCount     | 整數  | 在指定的時間期間，包括使用者起始的續約和自動續約訂用帳戶續約工作數目。        |
| goodStandingActiveCount | 整數 | 訂用帳戶，且在指定的時段內作用中和到期日期的數字 > = *endDate*查詢值。    |
| pendingGraceActiveCount | 整數 | 訂用帳戶，在指定的時段內期間作用，但計費的失敗，而且訂用帳戶到期日數 > = *endDate*查詢值。     |
| graceActiveCount | 整數 | 指定的時段期間作用，但計費的失敗，訂用帳戶的數目和位置：<ul><li>訂用帳戶的到期日 < *endDate*查詢值。</li><li>寬限期即將結束，> = *endDate*值。</li></ul>        |
| lockedActiveCount | 整數 | 中的訂用帳戶數目*dunning* （也就是訂用帳戶即將到期和 Microsoft 嘗試取得資金來自動續約訂用帳戶） 在指定的時間週期，而且其中：<ul><li>訂用帳戶的到期日 < *endDate*查詢值。</li><li>寬限期即將結束，< = *endDate*值。</li></ul>        |
| billingChurnCount | 整數 | 訂用帳戶，因為無法處理的帳單的費用停用指定的時間期間和 dunning 中先前屬於訂用帳戶數目。        |
| nonRenewalChurnCount | 整數 | 已停用指定的時間期間因為它們不更新的訂用帳戶數目。        |
| refundChurnCount | 整數 | 已停用，在指定的時間期間內因為它們已退還的訂用帳戶數目。        |
| chargebackChurnCount | 整數 | 所指定的時間期間停用因為計費的訂用帳戶數目。        |
| earlyChurnCount | 整數 | 已停用指定的時間期間而它們是有效的訂用帳戶數目。        |
| otherChurnCount | 整數 | 因為其他原因而指定的時間期間已停用的訂用帳戶數目。        |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9KDLGHH6R365",
      "subscriptionProductName": "Contoso App Subscription with One Month Free Trial",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "Unknown",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    },
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9JJFDHG4R478",
      "subscriptionProductName": "Contoso App Monthly Subscription",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "US",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>相關主題

* [附加元件下載數報告](../publish/add-on-acquisitions-report.md)
* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)

 

 
