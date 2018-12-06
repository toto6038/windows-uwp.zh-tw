---
ms.assetid: 235EBA39-8F64-4499-9833-4CCA9C737477
description: 使用「Microsoft Store 分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得應用程式的彙總廣告績效資料。
title: 取得廣告效益資料
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 廣告, 效益
ms.localizationpriority: medium
ms.openlocfilehash: c6bec86929284e49e4e882597422d316276c0a33
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8741590"
---
# <a name="get-ad-performance-data"></a>取得廣告效益資料


使用「Microsoft Store 分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得應用程式的彙總廣告績效資料。 這個方法會傳回 JSON 格式的資料。

這個方法會傳回相同的資料所提供的合作夥伴中心中的[廣告績效報告](../publish/advertising-performance-report.md)。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

如需詳細資訊，請參閱[使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述           |
|---------------|--------|--------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

若要擷取特定 App 的廣告績效資料，請使用 *applicationId* 參數。 若要擷取與您開發人員帳戶關聯之所有 App 的廣告績效資料，請省略 *applicationId* 參數。

| 參數     | 類型   | 描述     | 必要 |
|---------------|--------|-----------------|----------|
| applicationId   | 字串    | 您想要擷取廣告績效資料之 App 的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |    不可以      |
| startDate   | 日期    | 要擷取的廣告績效資料之日期範圍的開始日期，格式為 YYYY/MM/DD。 預設為目前的日期減去 30 天。 |    否      |
| endDate   | 日期    | 要擷取的廣告績效資料之日期範圍的結束日期，格式為 YYYY/MM/DD。 預設為目前的日期減去一天。 |    否      |
| top   | 整數    | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |    否      |
| skip   | 整數    | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |    否      |
| filter   | 字串    | 在回應中篩選資料列的一或多個陳述式。 如需更多資訊，請參閱下方的＜[篩選欄位](#filter-fields)＞一節。 |    否      |
| aggregationLevel   | 字串    | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。 |    否      |
| orderby   | 字串    | 將結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitId</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，用來指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |    否      |
| groupby   | 字串    | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：</p><ul><li><strong>applicationId</strong></li><li><strong>applicationName</strong></li><li><strong>date</strong></li><li><strong>accountCurrencyCode</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitName</strong></li><li><strong>adUnitId</strong></li><li><strong>pubCenterAppName</strong></li><li><strong>adProvider</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：<em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p> |    否      |


### <a name="filter-fields"></a>篩選欄位

要求本體的 *filter* 參數包含在回應中篩選資料列的一或多個陳述式。 每個陳述式都包含一個與 **eq** 或 **ne** 運算子關聯的欄位和值，而陳述式之間可以使用 **and** 或 **or** 來結合。 以下是一個範例 *filter* 參數：

-   *filter=market eq 'US' and deviceType eq 'phone'*

如需支援的欄位清單，請參閱下表。 *filter* 參數中的字串值必須由單引號括住。

| 欄位 | 描述                                                              |
|--------|--------------------------------------------------------------------------|
| market    | 內含廣告刊登市場之 ISO 3166 國家/地區代碼的字串。 |
| deviceType    | 下列其中一個字串：<strong>PC/Tablet</strong> 或 <strong>Phone</strong>。 |
| adUnitId    | 指定廣告單位識別碼的字串，用來套用到篩選。 |
| pubCenterAppName    | 指定目前 App 之 pubCenter 的字串，用來套用到篩選。 |
| adProvider    | 指定廣告提供者名稱的字串，用來套用到篩選。 |
| date    | 指定日期 (格式為 YYYY/MM/DD) 的字串，用來套用到篩選。 |


### <a name="request-example"></a>要求範例

下列範例示範數個取得廣告績效資料的要求。 請以您 App 的「Store 識別碼」取代 *applicationId* 值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=8/1/2015&endDate=8/31/2015&skip=0&$filter=market eq 'US' and deviceType eq 'phone’ eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 值      | 陣列  | 內含彙總廣告績效資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[廣告績效值](#ad-performance-values)一節。                                                                                                                      |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 5，但是查詢有超過 5 個資料項目，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                          |


### <a name="ad-performance-values"></a>廣告績效值

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                                                                                                                                                                                                                              |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 日期                | 字串 | 廣告績效資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId       | 字串 | 您要擷取廣告績效資料之 App 的「Store 識別碼」。     |
| applicationName     | 字串 | App 的顯示名稱。                         |
| adUnitId           | 字串 | 廣告單位的識別碼。        |
| adUnitName           | 字串 | 廣告單位，做為在合作夥伴中心開發人員指定的名稱。              |
| adProvider           |  字串  |  廣告提供者的名稱   |
| deviceType          | 字串 | 廣告刊登裝置的類型。 如需支援的字串清單，請參閱上方的[篩選欄位](#filter-fields)一節。                              |
| market              | 字串 | 廣告刊登市場的 ISO 3166 國家/地區代碼。             |
| accountCurrencyCode     | 字串 | 帳戶的貨幣代碼。        |
| pubCenterAppName       |  字串  |   合作夥伴中心中的應用程式相關聯的 pubCenter app 名稱。   |
| adProviderRequests        | 整數 | 指定之廣告提供者的廣告要求次數。                 |
| impressions           | 整數 | 廣告曝光次數。        |
| clicks            | 整數 | 廣告點按次數。       |
| revenueInAccountCurrency       | 數字 | 收入 (以帳戶的國家/地區貨幣表示)。       |
| requests              | 整數 | 廣告要求次數。                 |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10765920",
      "adUnitName":"TestAdUnit",
      "revenueInAccountCurrency": 10.0,
      "impressions": 1000,
      "requests": 10000,
      "clicks": 1,
      "accountCurrencyCode":"USD"
    },
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10795110",
      "adUnitName":"TestAdUnit2",
      "revenueInAccountCurrency": 20.0,
      "impressions": 2000,
      "requests": 20000,
      "clicks": 3,
      "accountCurrencyCode":"USD"
    },
  ],
  "@nextLink": "adsperformance?applicationId=9NBLGGH4R315&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=2&skip=2",
  "TotalCount": 191753
}

```

## <a name="related-topics"></a>相關主題

* [廣告績效報告](../publish/advertising-performance-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
