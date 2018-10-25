---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法取得 Xbox Live 多人遊戲資料。
title: 取得 Xbox Live 多人遊戲資料
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、Microsoft Store 服務、Microsoft Store 分析 API、Xbox Live 分析、多人遊戲
ms.localizationpriority: medium
ms.openlocfilehash: 84255c97186accfcafc561eeb77e0fdb6924b5e9
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5517477"
---
# <a name="get-xbox-live-multiplayer-data"></a>取得 Xbox Live 多人遊戲資料


在 Microsoft Store 分析 API 中使用此方法，可以在每日或每月取得[已啟用 Xbox Live 遊戲](../xbox-live/index.md) 的多人遊戲資料。 「Windows 開發人員中心」儀表板中的 [Xbox 分析報告](../publish/xbox-analytics-report.md)也有提供這項資訊。

> [!IMPORTANT]
> 此方法僅支援 Xbox 遊戲，或使用 Xbox Live 服務的遊戲。 這些遊戲必須通盤了解[概念核准程序](../gaming/concept-approval.md)，包括 [Microsoft 合作夥伴](../xbox-live/developer-program-overview.md#microsoft-partners)發行的遊戲，以及透過 [ [ID@Xbox程式](../xbox-live/developer-program-overview.md#id)提交的遊戲。 此方法目前不支援透過 [Xbox Live 創作者計畫](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) 發佈遊戲。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數


| 參數        | 類型   |  說明      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取 Xbox Live 多人遊戲資料之遊戲的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字串 | 指定要擷取 Xbox Live 分析資料類型的字串。 對於此方法，請指定值 **multiplayerdaily** 以取得每日多人遊戲資料或指定值 **multiplayermonthly** 以取得每月多人遊戲資料。  |  是  |
| startDate | 日期 | 要擷取多人遊戲資料之日期範圍的開始日期。 對於 **multiplayerdaily**，預設為目前日期的前 3 個月。 對於 **multiplayermonthly**，預設為目前日期的前 1 年。 |  否  |
| endDate | 日期 | 要擷取多人遊戲資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以在回應本文中指定下列欄位：<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul> | 否   |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>日期</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul><p/>如果您指定一個或多個 *groupby* 欄位，則您未指定的任何其他 *groupby* 欄位將在回應本文中有 **All** 值。 |  否  |


### <a name="request-example"></a>要求的範例

以下範例示範了用於已啟用 Xbox Live 遊戲取得多人遊戲資料的要求。 將 *applicationId* 值以您遊戲的 Store 識別碼取代。


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

| 值      | 類型   | 說明                  |
|------------|--------|-------------------------------------------------------|
| 值      | 陣列  | 包含多人遊戲資料的物件陣列，其中每個物件代表指定的每日或每月期間的一組資料，並按指定的 **篩選** 和 **groupby** 值整理。 如需每個物件中的資料的詳細資訊，請參閱[每日多人遊戲分析](#daily-multiplayer-analytics) 和[每月多人遊戲分析](#monthly-multiplayer-analytics) 部分。     |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢有超過 10000 個資料列，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。 |


### <a name="daily-multiplayer-analytics"></a>每日多人遊戲分析

當您要求每日多人遊戲分析資料時 (也就是說，當您將 **metricType** 參數指定為 **multiplayerdaily** 時)，*Value* 陣列中的元素包含以下值。

| 價值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| 日期                | 字串 | 多人遊戲資料的日期。 |
| applicationId       | 字串 | 您正在擷取多人遊戲資料之遊戲的 Store 識別碼。     |
| applicationName       | 字串 |  您正在擷取多人遊戲資料之遊戲名稱。     |
| market       | 字串 | 多人資料是來自 Microsoft Store 的兩個字母 ISO 3166 國碼 (地區碼)。       |
| packageVersion     | 字串 |  遊戲的四個部分套件版本。  |
| deviceType          | 字串 | 以下字串之一指定多人遊戲資料來自的裝置類型：<p/><ul><li><strong>主控台</strong></li><li><strong>電腦</strong></li><li>**不明**</li></ul>  |
| subscriptionName     | 字串 |  用於多人遊戲資料的訂閱名稱。 可能的值包括 **Xbox Game Pass** 和 **""** (沒有訂閱)。  |
| dailySessionCount     | 數目 |  在指定日期，該遊戲中多人遊戲工作階段數目。  |
| engagementDurationMinutes     | 數目 |  在指定日期，該遊戲中客戶參與多人遊戲工作階段的總分鐘數。  |
| dailyActiveUsers     | 數目 |  在指定日期，該遊戲中使用中的多人遊戲使用者總數。  |
| dailyActiveDevices     | 數目 |  在指定日期，該遊戲中玩過多人遊戲工作階段的使用中裝置總數。  |
| dailyNewUsers     | 數目 |  在指定日期，該遊戲中新的多人遊戲使用者總數。  |
| monthlyActiveUsers     | 數目 |  指定日期發生月份的使用中的多人遊戲使用者總數。  |
| monthlyActiveDevices     | 數目 | 在指定日期發生的月份內，該遊戲中玩過多人遊戲工作階段的使用中裝置總數。   |
| monthlyNewUsers     | 數目 |  在指定日期發生的月份，該遊戲中新的多人遊戲使用者總數。  |


### <a name="monthly-multiplayer-analytics"></a>每月多人遊戲分析

當您要求每月多人遊戲分析資料時 (也就是說，當您將 **metricType** 參數指定為 **multiplayermonthly** 時)，*Value* 陣列中的元素包含以下值。

| 價值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| 日期                | 字串 | 多人遊戲資料之月份中的第一個日期。 |
| applicationId       | 字串 | 您正在擷取多人遊戲資料之遊戲的 Store 識別碼。     |
| applicationName       | 字串 |  您正在擷取多人遊戲資料之遊戲名稱。     |
| market       | 字串 | 多人資料是來自 Microsoft Store 的兩個字母 ISO 3166 國碼 (地區碼)。       |
| packageVersion     | 字串 |  遊戲的四個部分套件版本。  |
| deviceType          | 字串 | 以下字串之一指定多人遊戲資料來自的裝置類型：<p/><ul><li><strong>主控台</strong></li><li><strong>電腦</strong></li><li>**不明**</li></ul>  |
| subscriptionName     | 字串 |  用於多人遊戲資料的訂閱名稱。 可能的值包括 **Xbox Game Pass** 和 **""** (沒有訂閱)。  |
| monthlySessionCount     | 數目 |  在指定月份內，該遊戲中的多人遊戲工作階段數目。   |
| engagementDurationMinutes     | 數目 |  在指定月份內，該遊戲中客戶參與多人遊戲工作階段的總分鐘數。  |
| monthlyActiveUsers     | 數目 | 在指定月份內，使用中的多人遊戲使用者總數。   |
| monthlyActiveDevices     | 數目 | 在指定月份內，該遊戲中玩過多人遊戲工作階段的使用中裝置總數。   |
| monthlyNewUsers     | 數目 |  在指定月份內，該遊戲中新的多人遊戲使用者總數。  |
| averageDailyActiveUsers     | 數目 |  在指定月份內，該遊戲中使用中的多人遊戲使用者的平均數量。  |
| averageDailyActiveDevices     | 數目 |  在指定月份內，該遊戲中玩過多人遊戲工作階段的使用中裝置平均數量。  |


### <a name="response-example"></a>回應範例

以下範例示範此要求的每日計量變數的範例 JSON 回應本文 (也就是當您為 **metricType** 參數指定 **multiplayerdaily**時)。

```json
{
  "Value": [
    {
      "date": "2018-01-07",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 976711,
      "engagementDurationMinutes": 16836064.5,
      "dailyActiveUsers": 180377,
      "dailyActiveDevices": 153359,
      "dailyNewUsers": 8638,
      "monthlyActiveUsers": 779984,
      "monthlyActiveDevices": 606495,
      "monthlyNewUsers": 212093
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 857433,
      "engagementDurationMinutes": 14087724.5,
      "dailyActiveUsers": 166630,
      "dailyActiveDevices": 143065,
      "dailyNewUsers": 9646,
      "monthlyActiveUsers": 764947,
      "monthlyActiveDevices": 595368,
      "monthlyNewUsers": 204248
    },
  ],
  "@nextLink": null,
  "TotalCount":2
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得 Xbox Live 分析資料](get-xbox-live-analytics.md)
* [取得 Xbox Live 成就資料](get-xbox-live-achievements-data.md)
* [取得 Xbox Live 健康情況資料](get-xbox-live-health-data.md)
* [取得 Xbox Live 遊戲中心資料](get-xbox-live-game-hub-data.md)
* [取得 Xbox Live 俱樂部資料](get-xbox-live-club-data.md)
