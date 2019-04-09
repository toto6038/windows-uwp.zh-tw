---
description: 在 Microsoft Store 分析 API 中使用此方法取得 Xbox Live 成就資料。
title: 取得 Xbox Live 成就資料
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10、uwp、Microsoft Store 服務、Microsoft Store 分析 API、Xbox Live 分析、成就
ms.localizationpriority: medium
ms.openlocfilehash: f1d9f7f27e4d0a219aa8bf474b9f57efbb1c74a0
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58162613"
---
# <a name="get-xbox-live-achievements-data"></a>取得 Xbox Live 成就資料

在 Microsoft Store 分析 API 中使用此方法來取得在最近的一天中為您的[已啟用 Xbox Live 遊戲](https://docs.microsoft.com/gaming/xbox-live//index.md) 解除鎖定每一個成就的客戶數目，其中這些資料可用的，在這一天之前的前 30 天，直到遊戲壽命結束的那天。 這項資訊也會提供[Xbox 分析報告](../publish/xbox-analytics-report.md)在合作夥伴中心。

> [!IMPORTANT]
> 此方法僅支援 Xbox 遊戲，或使用 Xbox Live 服務的遊戲。 這些遊戲必須通盤了解[概念核准程序](../gaming/concept-approval.md)，包括 [Microsoft 合作夥伴](https://docs.microsoft.com/gaming/xbox-live//developer-program-overview.md#microsoft-partners)發行的遊戲，以及透過  [ID@Xbox程式](https://docs.microsoft.com/gaming/xbox-live//developer-program-overview.md#id)提交的遊戲。 此方法目前不支援透過 [Xbox Live 創作者計畫](https://docs.microsoft.com/gaming/xbox-live//get-started-with-creators/get-started-with-xbox-live-creators.md) 發佈遊戲。

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
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數


| 參數        | 類型   |  描述      |  必要項  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取 Xbox Live 成就資料之遊戲的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字串 | 指定要擷取之 Xbox Live 分析資料類型的字串。 對於此方法，請指定 **成就**值。  |  是  |
| top | 整數 | 要在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來循頁瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |


### <a name="request-example"></a>要求範例

以下範例示範了用於客戶取得成就資料的要求，此客戶已經為已啟用的 Xbox Live 遊戲前 10 項成就解除鎖定。 將 *applicationId* 值取代為您的遊戲的 Store 識別碼。


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 陣列  | 物件陣列包含遊戲中每項成就資料。 如需有關每個物件中資料的詳細資訊，請參閱下表。                                                                                                                      |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 100，但是查詢有超過 100 個資料列，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。  |


*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字串 | 您正在擷取成就資料之遊戲的 Store 識別碼。     |
| reportDateTime     | 字串 |  成就資料的日期。    |
| achievementId          | 數字 |  成就的識別碼。 |
| achievementName           | 字串 | 成就的名稱。  |
| gamerscore           | 數字 |  玩家分數獎勵的成就。  |
| dailyUnlocks           | 數字 |  在 *reportDateTime* 所指定當天解除鎖定成就的客戶數目。  |
| monthlyUnlocks              | 數字 |  在 *reportDateTime* 所指定日期前的 30 天內解除鎖定成就的客戶數目。   |
| totalUnlocks | 數字 |  直至 *reportDateTime* 指定的那一天為止，在遊戲的整個生命週期內解除鎖定成就的客戶數目。   |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 6,
      "achievementName": "Yoink!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10310,
      "totalUnlocks": 1215360
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 7,
      "achievementName": "Ding!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10897,
      "totalUnlocks": 1282524
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 8,
      "achievementName": "End of the Beginning",
      "gamerscore": 30,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 9848,
      "totalUnlocks": 1105074
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得 Xbox Live 的分析資料](get-xbox-live-analytics.md)
* [取得 Xbox Live 的健全狀況資料](get-xbox-live-health-data.md)
* [取得 Xbox Live 的遊戲中樞資料](get-xbox-live-game-hub-data.md)
* [取得 Xbox Live club 資料](get-xbox-live-club-data.md)
* [取得 Xbox Live 多人的資料](get-xbox-live-multiplayer-data.md)
