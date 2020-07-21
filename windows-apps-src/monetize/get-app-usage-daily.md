---
ms.assetid: 99DB5622-3700-4FB2-803B-DA447A1FD7B7
description: 在 Microsoft Store 分析 API 中使用此方法，以取得給定日期範圍和其他選擇性篩選的每日應用程式使用量資料。
title: 取得每日應用程式使用量
ms.date: 08/15/2018
ms.topic: article
keywords: windows 10，uwp，存放服務，Microsoft Store 分析 API，使用方式
ms.localizationpriority: medium
ms.openlocfilehash: 4ddc741d13568ba6860902ebc5b6e375ec43d369
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493333"
---
# <a name="get-daily-app-usage"></a>取得每日應用程式使用量

在 Microsoft Store 分析 API 中使用此方法，可在指定的日期範圍（僅限過去90天）和其他選擇性篩選器中，取得應用程式 JSON 格式的匯總使用量資料（不包括 Xbox 多人遊戲）。 您也可以在合作夥伴中心的[使用方式報告](../publish/usage-report.md)中取得這項資訊。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                               |
|--------|---------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily``` |


### <a name="request-header"></a>要求標頭

| 頁首        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數     | 類型   |  說明                                                                                                    |  必要  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | 字串 | 您想要擷取評論資料之應用程式的　[Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 |  是       |
| startDate     | date   | 要擷取評論資料之日期範圍的開始日期。 預設值是目前的日期。                   |  否        |
| endDate       | date   | 要擷取評論資料之日期範圍的結束日期。 預設值是目前的日期。                     |  否        |
| top           | int    | 在要求中傳回的資料列數目。 如果未指定，最大值和預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。                          |  否        |
| skip          | int    | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。                         |  否        |  
| filter        |字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 eq 或 ne 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 and 或 or 結合。 篩選 參數中的字串值必須由單引號括住。 您可以在回應本文中指定下列欄位： <ul><li>**細分**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | 否         |  
| orderby       | 字串 | 將結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**細分**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p><em>order</em> 參數為選擇性，並可以是 **asc** 或 **desc**，以指定每個欄位的遞增或遞減順序。 預設值是**asc**。</p><p>以下是範例<em>orderby</em>字串： <em>orderby = date，市</em></p>                                                                                                   |  否        |
| groupby       | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以在回應本文中指定下列欄位： <ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**細分**</li><li>**date**</li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如： <em> &amp; Groupby = ageGroup、市場 &amp; aggregationLevel = 周</em></p>                                                                                                             |  否        |


### <a name="request-example"></a>要求範例

下列範例示範取得每日應用程式使用量資料的要求。 將 *applicationId* 值取代為您 App 的 Store 識別碼。

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily?applicationId=XXXXXXXXXXXX&startDate=2018-08-10&endDate=2018-08-14 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應



### <a name="response-body"></a>回應本文

| 值      | 類型   | 描述                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| 值      | array  | 包含匯總使用方式資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下表。 |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的評論資料，就會傳回此值。                 |
| TotalCount | int    | 查詢之資料結果的資料列總數。                                                                          |

 
### <a name="usage-values"></a>使用值

*Value* 陣列中的元素包含下列值。

| 值                     | 類型    | 描述                                                               |
|---------------------------|---------|---------------------------------------------------------------------------|
| date                      | 字串  | 使用量資料的日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。        |
| applicationId             | 字串  | 您正在抓取其使用方式資料之應用程式的存放區識別碼。          |
| applicationName           | 字串  | App 的顯示名稱。                                              |
| deviceType                | 字串  | 下列其中一個字串，指定發生使用的裝置類型：<ul><li>**PC**</li><li>**來電**</li><li>**主控台-Xbox One**</li><li>**主控台-Xbox 系列 X**</li><li>**平板電腦**</li><li>**IoT**</li><li>**Server**</li><li>**全息影像**</li><li>**Unknown**</li></ul>                                                                                                         |
| packageVersion            | 字串  | 發生使用的封裝版本。                          |
| market                    | 字串  | 客戶使用您的應用程式之市場的 ISO 3166 國家/地區代碼。 |
| subscriptionName          | 字串  | 指出使用量是否通過 Xbox 遊戲 Pass。                            |
| dailySessionCount         | long    | 當天的使用者會話數目。                                  |
| engagementDurationMinutes | double  | 使用者主動使用您的應用程式的分鐘數（以不同的時間為單位），從應用程式啟動時開始（進程開始）並于終止時結束（程式結束），或在一段時間沒有活動之後結束。             |
| dailyActiveUsers          | long    | 使用當日應用程式的客戶數目。                           |
| dailyActiveDevices        | long    | 所有使用者用來與您的應用程式互動的每日裝置數。  |
| dailyNewUsers             | long    | 一天中第一次使用您的應用程式的客戶數目。    |
| monthlyActiveUsers        | long    | 使用當月應用程式的客戶數目。                         |
| monthlyActiveDevices      | long    | 執行應用程式的裝置數目已有一段不同的時間，從應用程式啟動時開始（進程開始），到終止時結束（程式結束），或在一段時間沒有活動之後啟動。                                      |
| monthlyNewUsers           | long    | 第一次使用您的應用程式的客戶數目。  |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2018-08-10",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 17718,
      "engagementDurationMinutes": 582540.2,
      "dailyActiveUsers": 7078,
      "dailyActiveDevices": 6964,
      "dailyNewUsers": 1727,
      "monthlyActiveUsers": 123609,
      "monthlyActiveDevices": 116723,
      "monthlyNewUsers": 72271
    },
    {
      "date": "2018-08-11",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 19899,
      "engagementDurationMinutes": 655379.7,
      "dailyActiveUsers": 7877,
      "dailyActiveDevices": 7789,
      "dailyNewUsers": 2062,
      "monthlyActiveUsers": 123666,
      "monthlyActiveDevices": 116770,
      "monthlyNewUsers": 72276
    },
    {
      "date": "2018-08-12",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 21349,
      "engagementDurationMinutes": 735640.5,
      "dailyActiveUsers": 8456,
      "dailyActiveDevices": 8309,
      "dailyNewUsers": 2433,
      "monthlyActiveUsers": 124241,
      "monthlyActiveDevices": 117289,
      "monthlyNewUsers": 72732
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得每月應用程式 ussage](get-app-usage-monthly.md)
* [取得應用程式下載數](get-app-acquisitions.md)
* [取得附加元件下載數](get-in-app-acquisitions.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
