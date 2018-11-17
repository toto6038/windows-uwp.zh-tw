---
author: Xansky
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: 使用「Microsoft Store 分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得所指定應用程式的彙總廣告行銷活動績效資料。
title: 取得廣告活動效益資料
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 廣告活動
ms.localizationpriority: medium
ms.openlocfilehash: c9d348ffc0895d5adb16886c5a66af14a1884b8b
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "7104914"
---
# <a name="get-ad-campaign-performance-data"></a>取得廣告活動效益資料


使用「Microsoft Store 分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得您應用程式促銷廣告行銷活動效益資料的彙總摘要。 這個方法會傳回 JSON 格式的資料。

這個方法會傳回相同的資料所提供的合作夥伴中心中的[廣告行銷活動報告](../publish/app-install-ads-reports.md)。 如需有關廣告行銷活動的詳細資訊，請參閱[為您的 App 建立廣告行銷活動](../publish/create-an-ad-campaign-for-your-app.md)。

若要建立、更新或擷取廣告活動的詳細資料，您可以使用 [Microsoft Store 促銷 API](run-ad-campaigns-using-windows-store-services.md) 中的[管理廣告活動](manage-ad-campaigns.md)方法。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                |
|---------------|--------|---------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

若要擷取特定 App 的廣告行銷活動績效資料，請使用 *applicationId* 參數。 若要擷取與您開發人員帳戶關聯之所有 App 的廣告績效資料，請省略 *applicationId* 參數。

| 參數     | 類型   | 描述     | 必要 |
|---------------|--------|-----------------|----------|
| applicationId   | 字串    | 您想要擷取廣告行銷活動績效資料之應用程式的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 |    不可以      |
|  startDate  |  日期   |  要擷取的廣告行銷活動績效資料之日期範圍的開始日期，格式為 YYYY/MM/DD。 預設為目前的日期減去 30 天。   |   否    |
| endDate   |  日期   |  要擷取的廣告行銷活動績效資料之日期範圍的結束日期，格式為 YYYY/MM/DD。 預設為目前的日期減去一天。   |   否    |
| top   |  整數   |  在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。   |   否    |
| skip   | 整數    |  在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。   |   否    |
| filter   |  字串   |  一或多個篩選回應中資料列的陳述式。 目前唯一支援的篩選是 **campaignId**。 每個陳述式都可以使用 **eq** 或 **ne** 運算子，而陳述式之間可以使用 **and** 或 **or** 來結合。  以下是一個範例 *filter* 參數：```filter=campaignId eq '100023'```。   |   否    |
|  aggregationLevel  |  字串   | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。    |   否    |
| orderby   |  字串   |  <p>將廣告行銷活動績效資料的結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，用來指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>以下是一個範例 <em>orderby</em> 字串：<em>orderby=date,campaignId</em></p>   |   否    |
|  groupby  |  字串   |  <p>將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：<em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p>   |   否    |


### <a name="request-example"></a>要求範例

下列範例示範數個取得廣告行銷活動績效資料的要求。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述  |
|------------|--------|---------------|
| 值      | 陣列  | 內含彙總廣告行銷活動績效資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[行銷活動績效物件](#campaign-performance-object)一節。          |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 5，但是查詢有超過 5 個資料項目，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                                |


<span id="campaign-performance-object" />


### <a name="campaign-performance-object"></a>行銷活動績效物件

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述            |
|---------------------|--------|------------------------|
| 日期                | 字串 | 廣告行銷活動績效資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId       | 字串 | 您要擷取廣告行銷活動績效資料之 App 的「Store 識別碼」。                     |
| campaignId     | 字串 | 廣告行銷活動的識別碼。           |
| lineId     | 字串 |    產生此效益資料的廣告活動[播送行](manage-delivery-lines-for-ad-campaigns.md)的識別碼。        |
| currencyCode              | 字串 | 行銷活動預算的貨幣代碼。              |
| spend          | 字串 |  已花費的廣告行銷活動預算總金額。     |
| impressions           | 長整數 | 行銷活動的廣告曝光次數。        |
| installs              | 長整數 | 與行銷活動相關的 App 安裝次數。   |
| clicks            | 長整數 | 行銷活動的廣告點按次數。      |
| iapInstalls            | 長整數 | 與行銷活動相關的附加元件安裝數 (也稱為 App 內購買或 IAP)。      |
| activeUsers            | 長整數 | 按下行銷活動中廣告並返回應用程式的使用者人數。      |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "lineId": "0001",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8,
      "iapInstalls": 0,
      "activeUsers": 0
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "lineId": "0002",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5,
      "iapInstalls": 0,
      "activeUsers": 0
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

## <a name="related-topics"></a>相關主題

* [為您的應用程式建立行銷活動](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)
* [使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
