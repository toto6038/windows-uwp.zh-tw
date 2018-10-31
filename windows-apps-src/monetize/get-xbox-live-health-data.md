---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法取得 Xbox Live 健康情況資料。
title: 取得 Xbox Live 健康情況資料
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10、uwp、Microsoft Store 服務、Microsoft Store 分析 API、Xbox Live 分析、健康情況、用戶端錯誤
ms.localizationpriority: medium
ms.openlocfilehash: e2143f04b1b2641123929f5f833df2421f77b99e
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5812412"
---
# <a name="get-xbox-live-health-data"></a>取得 Xbox Live 健康情況資料


在 Microsoft Store 分析 API 中使用此方法取得[已啟用 Xbox Live 遊戲](../xbox-live/index.md) 的健康情況資料。 「Windows 開發人員中心」儀表板中的 [Xbox 分析報告](../publish/xbox-analytics-report.md)也有提供這項資訊。

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
| applicationId | 字串 | 您想要擷取 Xbox Live 健康情況資料之遊戲的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字串 | 指定要擷取 Xbox Live 分析資料類型的字串。 對於此方法，請指定 **callingpattern**值。  |  是  |
| startDate | 日期 | 要擷取健康情況資料之日期範圍的開始日期。 預設為目前日期的前 30 天。 |  否  |
| endDate | 日期 | 要擷取健康情況資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以在回應本文中指定下列欄位：<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | 否   |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>日期</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>如果您指定一個或多個 *groupby* 欄位，則您未指定的任何其他 *groupby* 欄位將在回應本文中有 **All** 值。 |  否  |


### <a name="request-example"></a>要求的範例

以下範例示範了用於已啟用 Xbox Live 遊戲取得健康情況資料的要求。 將 *applicationId* 值以您遊戲的 Store 識別碼取代。


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

| 值      | 類型   | 說明                  |
|------------|--------|-------------------------------------------------------|
| 值      | 陣列  | 包含健康情況資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下表。                                                                                                                      |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢有超過 10000 個資料列，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。   |


*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字串 | 您正在擷取健康情況資料之遊戲的 Store 識別碼。     |
| 日期                | 字串 | 健康情況資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| deviceType          | 字串 | 以下字串之一指定您使用遊戲的裝置類型：<p/><ul><li><strong>XboxOne</strong></li><li><strong>WindowsOneCore</strong> (這個值表示一台電腦)</li><li><strong>不明</strong></li></ul>  |
| sandboxId     | 字串 |   為遊戲建立的沙箱 ID。 這可以是 RETAIL 值或專用沙箱的 ID。   |
| packageVersion     | 字串 |  遊戲的四個部分套件版本。  |
| callingPattern     | 物件 |  [callingPattern](#callingpattern) 物件，為指定日期範圍內遊戲所使用的每個 Xbox Live 服務傳回的每個狀態代碼提供服務回應、裝置和使用者資料。     |


### <a name="callingpattern"></a>callingPattern

| 數值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 服務      | 字串  |   與健康情況資料相關的 Xbox Live 服務的名稱。       |
| 端點      | 字串  |   與健康情況資料相關的 Xbox Live 服務的端點。        |
| httpStatusCode      | 字串  |  這組健康情況資料的 HTTP 狀態碼。<p/><p/>**注意**&nbsp;&nbsp;狀態碼 **429E** 表示服務呼叫成功只是因為在通話期間[細緻的速率限制](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md)已豁免。 如果服務體驗量很大，細緻的速率限制可以在未來執行，在這種情況下，呼叫會導致 [HTTP 429 狀態碼](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object)。         |
| serviceResponses      | 數目  | 傳回指定狀態碼的服務回應數。         |
| uniqueDevices      | 數目  |  呼叫服務並接收指定狀態碼的唯一裝置的數目。       |
| uniqueUsers      | 數目  |   接收指定狀態碼的不重複使用者數目。       |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。 本範例中顯示的服務名稱和端點不是實際，僅用於範例。

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "date": "2018-01-30",
      "deviceType": "All",
      "sandboxId": "RETAIL",
      "packageVersion": "Unknown",
      "callingpattern": [
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchReadHandler.POST",
          "httpStatusCode": "200",
          "serviceResponses": 160891,
          "uniqueDevices": 410,
          "uniqueUsers": 410
        },
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchStatReadHandler.GET",
          "httpStatusCode": "200",
          "serviceResponses": 422,
          "uniqueDevices": 0,
          "uniqueUsers": 30
        }
      ],
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得 Xbox Live 分析資料](get-xbox-live-analytics.md)
* [取得 Xbox Live 成就資料](get-xbox-live-achievements-data.md)
* [取得 Xbox Live 遊戲中心資料](get-xbox-live-game-hub-data.md)
* [取得 Xbox Live 俱樂部資料](get-xbox-live-club-data.md)
* [取得 Xbox Live 多人遊戲資料](get-xbox-live-multiplayer-data.md)
