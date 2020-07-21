---
description: 在 Microsoft Store 分析 API 中使用此方法，在指定的日期範圍和其他選擇性篩選期間取得附加元件訂用帳戶的購買資料。
title: 取得訂閱附加元件下載數
ms.date: 01/25/2018
ms.topic: article
keywords: windows 10，uwp，存放服務，Microsoft Store 分析 API，訂用帳戶
ms.openlocfilehash: 4c307dbf7d17251e3d3c5f8792d8ea3d049924d9
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493543"
---
# <a name="get-subscription-add-on-acquisitions"></a>取得訂閱附加元件下載數

在 Microsoft Store 分析 API 中使用此方法，在指定的日期範圍和其他選擇性篩選期間，取得應用程式附加元件訂用帳戶的匯總取得資料。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>要求標頭

| 頁首        | 類型   | 描述          |
|---------------|--------|--------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您要取得訂用帳戶附加元件取得資料之應用程式的[存放區識別碼](in-app-purchases-and-trials.md#store-ids)。 |  是  |
| subscriptionProductId  | 字串 | 您想要取出取得資料之訂用帳戶附加元件的[存放區識別碼](in-app-purchases-and-trials.md#store-ids)。 如果您未指定此值，這個方法會傳回指定之應用程式所有訂用帳戶附加元件的取得資料。  | 否  |
| startDate | date | 要抓取之訂用帳戶附加元件取得資料的日期範圍中的開始日期。 預設值是目前的日期。 |  否  |
| endDate | date | 要取出之訂用帳戶附加元件取得資料的日期範圍中的結束日期。 預設值是目前的日期。 |  否  |
| top | int | 在要求中傳回的資料列數目。 最大值和預設值（如果未指定）為100。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | int | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top = 100 和 skip = 0 會抓取前100個數據列，top = 100 和 skip = 100 會抓取接下來的100個數據列，依此類推。 |  否  |
| filter | 字串  | 一或多個篩選回應本文的陳述式。 每個陳述式都可以使用 **eq** 或 **ne** 運算子，而陳述式之間可以使用 **and** 或 **or** 來結合。 您可以在篩選語句中指定下列字串（它們對應至[回應主體中的值](#subscription-acquisition-values)）： <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>細分</strong></li><li><strong>deviceType</strong></li></ul><p>以下是範例*篩選*參數： <em>filter = date eq ' 2017-07-08 '</em>。</p> | 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>天</strong>。 | 否 |
| orderby | 字串 | 針對每個訂閱附加元件取得的結果資料值進行排序的語句。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>細分</strong></li><li><strong>deviceType</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設值是<strong>asc</strong>。</p><p>以下是範例<em>orderby</em>字串： <em>orderby = date，市</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>細分</strong></li><li><strong>deviceType</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如： <em>groupby = 市 &amp; aggregationLevel = 周</em></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範如何取得訂用帳戶附加元件取得資料。 以適用于您應用程式的適當存放區識別碼取代*applicationId*值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應本文

| 值      | 類型   | 描述         |
|------------|--------|------------------|
| 值      | array  | 物件的陣列，其中包含匯總的訂閱附加元件取得資料。 如需每個物件中之資料的詳細資訊，請參閱下面的「訂用帳戶取得[值](#subscription-acquisition-values)」一節。             |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的**top**參數設定為100，但查詢的訂閱附加元件取得資料超過100個，則會傳回此值。 |
| TotalCount | int    | 查詢之資料結果的資料列總數。       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>訂用帳戶取得值

*Value* 陣列中的元素包含下列值。

| 值               | 類型    | 描述        |
|---------------------|---------|---------------------|
| date                | 字串  | 下載數資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| subscriptionProductId      | 字串  | 您要抓取取得資料之訂用帳戶附加元件的[存放區識別碼](in-app-purchases-and-trials.md#store-ids)。    |
| subscriptionProductName    | 字串  | 訂閱附加元件的顯示名稱。         |
| applicationId       | 字串  | 您要為其抓取訂用帳戶附加元件取得資料之應用程式的[存放區識別碼](in-app-purchases-and-trials.md#store-ids)。   |
| applicationName     | 字串  | App 的顯示名稱。     |
| skuId     | 字串  | 您要抓取取得資料之訂用帳戶附加元件的[SKU](in-app-purchases-and-trials.md#products-skus)識別碼。     |
| deviceType          | 字串  |  以下字串之一指定完成收購的裝置類型：<ul><li><strong>PC</strong></li><li><strong>來電</strong></li><li><strong>主控台-Xbox One</strong></li><li><strong>主控台-Xbox 系列 X</strong></li><li><strong>IoT</strong></li><li><strong>全息影像</strong></li><li><strong>Unknown</strong></li></ul>       |
| market           | 字串  | 發生下載之市場的 ISO 3166 國家/地區碼。     |
| currencyCode         | 字串  | 稅金之前的總銷售額貨幣代碼，格式為 ISO 4217。       |
| grossSalesBeforeTax           | integer  | *CurrencyCode*值所指定之當地貨幣的總銷售額。     |
| totalActiveCount             | integer  | 在指定的時間週期內，使用中的訂用帳戶總數。 這相當於*goodStandingActiveCount*、 *pendingGraceActiveCount*、 *graceActiveCount*和*lockedActiveCount*值的總和。  |
| totalChurnCount              | integer  | 在指定的時段內停用的訂閱總數。 這相當於*billingChurnCount*、 *nonRenewalChurnCount*、 *refundChurnCount*、 *chargebackChurnCount*、 *earlyChurnCount*和*otherChurnCount*值的總和。   |
| newCount            | integer  | 在指定的期間內，新的訂用帳戶取得次數，包括試用版。    |
| renewCount     | integer  | 在指定期間內的訂用帳戶續約數目，包括使用者起始的更新和自動更新。        |
| goodStandingActiveCount | integer | 在指定期間內作用中的訂閱數目，以及到期日 >= 查詢的*結束*值。    |
| pendingGraceActiveCount | integer | 在指定期間內作用中但發生帳單失敗的訂閱數目，以及訂閱到期日 >= 查詢的*結束*值。     |
| graceActiveCount | integer | 在指定的期間內作用中的訂用帳戶數目，但發生計費失敗，其中：<ul><li>訂閱到期日會 < 查詢的*結束*值。</li><li>寬限期的結尾是 >*= 結束時間值。*</li></ul>        |
| lockedActiveCount | integer | 在指定的期間內，*欠款*中的訂用帳戶數目（也就是訂閱即將到期，而 Microsoft 正嘗試取得資金來自動更新訂用帳戶），其中：<ul><li>訂閱到期日會 < 查詢的*結束*值。</li><li>寬限期的結尾是 <*= 結束時間值。*</li></ul>        |
| billingChurnCount | integer | 在指定的期間內停用的訂用帳戶數目，因為無法處理帳單費用，以及先前在欠款中的訂用帳戶。        |
| nonRenewalChurnCount | integer | 在指定的期間內停用的訂用帳戶數目，因為這些訂用帳戶尚未更新。        |
| refundChurnCount | integer | 在指定的期間內已停用的訂用帳戶數目，因為這些訂用帳戶已被退款。        |
| chargebackChurnCount | integer | 因為退款而在指定的期間內停用的訂閱數目。        |
| earlyChurnCount | integer | 在指定的期間內已停用的訂用帳戶數目。        |
| otherChurnCount | integer | 因其他原因而在指定的時段內停用的訂閱數目。        |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

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
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)

 

 
