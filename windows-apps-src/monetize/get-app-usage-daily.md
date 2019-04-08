---
ms.assetid: 99DB5622-3700-4FB2-803B-DA447A1FD7B7
description: 在 Microsoft Store analytics API 中使用這個方法，取得指定的日期範圍和其他選用的篩選器中的每日的應用程式使用量資料。
title: 取得每日應用程式使用量
ms.date: 08/15/2018
ms.topic: article
keywords: windows 10、 uwp、 存放區服務、 Microsoft Store 分析 API，使用方式
ms.localizationpriority: medium
ms.openlocfilehash: d3460b61e6a9a7c36be6fd87c4dc7fcc1ab811d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613103"
---
# <a name="get-daily-app-usage"></a>取得每日應用程式使用量

若要取得 JSON 格式指定的日期範圍 （過去 90 天只） 和其他選用的篩選條件期間的應用程式 （不包括 Xbox 多人遊戲） 的彙總使用方式資料，Microsoft Store 分析 API 中使用這個方法。 這項資訊也會提供[使用量報告](../publish/usage-report.md)在合作夥伴中心。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                               |
|--------|---------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數     | 類型   |  描述                                                                                                    |  必要  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | 字串 | 您想要擷取評論資料之應用程式的　[Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 |  是       |
| startDate     | date   | 要擷取評論資料之日期範圍的開始日期。 預設為目前的日期。                   |  否        |
| endDate       | date   | 要擷取評論資料之日期範圍的結束日期。 預設為目前的日期。                     |  否        |
| top           | 整數    | 要在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。                          |  否        |
| skip          | 整數    | 在查詢中要略過的資料列數目。 使用此參數來循頁瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。                         |  否        |  
| filter        |字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含欄位名稱從回應主體和 eq 或 ne 運算子相關聯的值，而且陳述式可以使用結合和或。 必須以篩選參數中的單引號括住的字串值。 您可以在回應本文中指定下列欄位： <ul><li>**市場**</li><li>**裝置類型**</li><li>**PackageVersion**</li></ul>                                                                                                                                              | 否         |  
| orderby       | 字串 | 將結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>。<em>field</em> 參數可以是下列其中一個字串：<ul><li>**日期**</li><li>**applicationId**</li><li>**應用程式名稱**</li><li>**市場**</li><li>**PackageVersion**</li><li>**裝置類型**</li><li>**訂閱名稱**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p><em>order</em> 參數為選擇性，並可以是 **asc** 或 **desc**，以指定每個欄位的遞增或遞減順序。 預設為 **asc**。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p>                                                                                                   |  否        |
| groupby       | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以在回應本文中指定下列欄位： <ul><li>**應用程式名稱**</li><li>**訂閱名稱**</li><li>**裝置類型**</li><li>**PackageVersion**</li><li>**市場**</li><li>**日期**</li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li>**applicationId**</li><li>**訂閱名稱**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：<em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p>                                                                                                             |  否        |


### <a name="request-example"></a>要求範例

下列範例會示範取得每日的應用程式使用量資料的要求。 將 *applicationId* 值以您應用程式的市集識別碼取代。

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily?applicationId=XXXXXXXXXXXX&startDate=2018-08-10&endDate=2018-08-14 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應



### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| 值      | 陣列  | 包含彙總使用方式資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下表。 |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的評論資料，就會傳回此值。                 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                                                                          |

 
### <a name="usage-values"></a>使用量值

*Value* 陣列中的元素包含下列值。

| 值                     | 類型    | 描述                                                               |
|---------------------------|---------|---------------------------------------------------------------------------|
| date                      | 字串  | 日期範圍中的使用量資料的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。        |
| applicationId             | 字串  | 應用程式，您要為其擷取使用量資料存放區識別碼。          |
| applicationName           | 字串  | 應用程式的顯示名稱。                                              |
| deviceType                | 字串  | 下列字串的其中一個指定使用狀況的發生位置的裝置的類型：<ul><li>**PC**</li><li>**電話**</li><li>**主控台**</li><li>**平板電腦**</li><li>**IoT**</li><li>**伺服器**</li><li>**全像攝影版**</li><li>**未知**</li></ul>                                                                                                         |
| packageVersion            | 字串  | 發生使用封裝的版本。                          |
| market                    | 字串  | ISO 3166 國碼的市場區隔客戶使用您的應用程式的位置。 |
| subscriptionName          | 字串  | 表示使用方式是否透過 Xbox Game Pass。                            |
| dailySessionCount         | 長整數    | 在那一天的使用者工作階段數目。                                  |
| engagementDurationMinutes | double  | 分鐘的使用者正在使用您的應用程式由不同的一段時間，應用程式啟動時開始測量 （處理序啟動），而結束時終止 （處理序結束） 或在閒置一段時間之後。             |
| dailyActiveUsers          | 長整數    | 使用應用程式的那一天的客戶數目。                           |
| dailyActiveDevices        | 長整數    | 用來與您的應用程式互動的所有使用者的每日裝置數目。  |
| dailyNewUsers             | 長整數    | 使用您的應用程式第一次該日的客戶數目。    |
| monthlyActiveUsers        | 長整數    | 使用應用程式該月的客戶數目。                         |
| monthlyActiveDevices      | 長整數    | 執行您的應用程式不同的一段時間，啟動應用程式啟動時 （處理序啟動） 和結束時終止 （處理序結束） 的裝置或一段閒置時間之後數。                                      |
| monthlyNewUsers           | 長整數    | 該月第一次使用您的應用程式的客戶數目。  |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

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

* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得每月的應用程式 ussage](get-app-usage-monthly.md)
* [取得應用程式售出](get-app-acquisitions.md)
* [取得附加元件併購](get-in-app-acquisitions.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
