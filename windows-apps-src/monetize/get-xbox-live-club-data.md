---
description: 在 Microsoft Store 分析 API 中使用此方法取得 Xbox Live 俱樂部資料。
title: 取得 Xbox Live 俱樂部資料
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10、uwp、Microsoft Store 服務、Microsoft Store 分析 API、Xbox Live 分析、俱樂部
ms.localizationpriority: medium
ms.openlocfilehash: dbf9d06f96632237c10de0fe3b6c4723a2501254
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7845903"
---
# <a name="get-xbox-live-club-data"></a>取得 Xbox Live 俱樂部資料

在 Microsoft Store 分析 API 中使用此方法取得[已啟用 Xbox Live 遊戲](../xbox-live/index.md) 的俱樂部資料。 這項資訊也會在合作夥伴中心中的[Xbox 分析報告](../publish/xbox-analytics-report.md)中提供的。

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
| applicationId | 字串 | 您想要擷取 Xbox Live 俱樂部資料之遊戲的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字串 | 指定要擷取 Xbox Live 分析資料類型的字串。 對於此方法，請指定 **communitymanagerclub**值。  |  是  |
| startDate | 日期 | 要擷取俱樂部資料之日期範圍的開始日期。 預設為目前日期的前 30 天。 |  否  |
| endDate | 日期 | 要擷取俱樂部資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |


### <a name="request-example"></a>要求的範例

以下範例示範了用於已啟用 Xbox Live 遊戲取得俱樂部資料的要求。 將 *applicationId* 值以您遊戲的 Store 識別碼取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

| 值      | 類型   | 說明                  |
|------------|--------|-------------------------------------------------------|
| 值      | 陣列  | 包含一個 [ProductData](#productdata) 物件的陣列，其中包含與您的遊戲相關的俱樂部的資料以及一個包含所有 Xbox Live 客戶的俱樂部資料的 [XboxwideData](#xboxwidedata) 物件。 包含這些資料是為了與遊戲資料作比較。  |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢有超過 10000 個資料列，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。 |


### <a name="productdata"></a>ProductData

此資源包含您的遊戲之俱樂部資料。

| 數值           | 類型    | 描述        |
|-----------------|---------|------|
| 日期            |  字串 |   俱樂部資料的日期。   |
|  applicationId               |    字串     |  用於擷取俱樂部資料的遊戲的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。   |
|  clubsWithTitleActivity               |    整數     |  和您的遊戲有社交關聯的俱樂部數目。   |     
|  clubsExclusiveToGame               |   整數      |  只與您的遊戲有社交關聯的俱樂部數目。   |     
|  clubFacts               |   陣列      |   包含一個或多個關於與您的遊戲有社交關聯的每個俱樂部的 [ClubFacts](#clubfacts) 物件。   |


### <a name="xboxwidedata"></a>XboxwideData

此資源包含所有 Xbox Live 客戶的平均俱樂部資料。

| 數值           | 類型    | 描述        |
|-----------------|---------|------|
| 日期            |  字串 |   俱樂部資料的日期。   |
|  applicationId  |    字串     |   在 **XboxwideData** 物件，這個字串總是 **XBOXWIDE** 值。  |
|  clubsWithTitleActivity               |   整數     |  平均而言，客戶與已啟用的 Xbox Live 遊戲有社交關聯的俱樂部數目。    |     
|  clubsExclusiveToGame               |   整數      |  平均而言，只與已啟用的 Xbox Live 遊戲有社交關聯的俱樂部數目。   |     
|  clubFacts               |   物件      |  包含一個 [ClubFacts](#clubfacts) 物件。 此物件在 **XboxwideData** 物件的內容中沒有意義，並有預設值。  |


### <a name="clubfacts"></a>ClubFacts

在 **ProductData** 物件，此物件包含特定俱樂部的資料，該俱樂部具有與您的遊戲相關的網路動向。 在 **XboxwideData** 物件，此物件沒有意義的，並有預設值。

| 數值           | 類型    | 描述        |
|-----------------|---------|--------------------|
|  name            |  字串  |   在 **ProductData** 物件，這是俱樂部的名稱。 在 **XboxwideData** 物件，這總是 **XBOXWIDE** 值。           |
|  memberCount               |    整數     | 在 **ProductData** 物件，這是俱樂部會員的數目，不包括只瀏覽俱樂部的非會員。 在 **XboxwideData** 物件，這總是 0。    |
|  titleSocialActionsCount               |    整數     |  在 **ProductData** 物件，這是俱樂部成員所執行的與您的遊戲相關的社交動作次數。 在 **XboxwideData** 物件，這總是 0   |
|  isExclusiveToGame               |    布林值     |  在 **ProductData** 物件，這表示目前俱樂部是否只與您的遊戲有社交關聯。 在 **XboxwideData** 物件，這總是 True。  |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "ProductData": [
        {
          "date": "2018-01-30",
          "applicationId": "9NBLGGGZ5QDR",
          "clubsWithTitleActivity": 3,
          "clubsExclusiveToGame": 1,
          "clubFacts": [
            {
              "name": "Club Contoso Racing",
              "memberCount": 1,
              "titleSocialActionsCount": 1,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Sports",
              "memberCount": 12,
              "titleSocialActionsCount": 9,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Maze",
              "memberCount": 4,
              "titleSocialActionsCount": 2,
              "isExclusiveToGame": false
            }
          ]
        }
      ],
      "XboxwideData": [
        {
          "date": "2018-01-30",
          "applicationId": "XBOXWIDE",
          "clubsWithTitleActivity": 25,
          "clubsExclusiveToGame": 3,
          "clubFacts": [
            {
              "name": "XBOXWIDE",
              "memberCount": 0,
              "titleSocialActionsCount": 0,
              "isExclusiveToGame": true
            }
          ]
        }
      ]
    }
  ],
  "@nextLink": "gameanalytics?applicationId=9NBLGGGZ5QDR&metricType=communitymanagerclub&top=1&skip=1&startDate=2018/01/04&endDate=2018/02/02&orderby=date desc",
  "TotalCount": 27
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得 Xbox Live 分析資料](get-xbox-live-analytics.md)
* [取得 Xbox Live 成就資料](get-xbox-live-achievements-data.md)
* [取得 Xbox Live 健康情況資料](get-xbox-live-health-data.md)
* [取得 Xbox Live 遊戲中心資料](get-xbox-live-game-hub-data.md)
* [取得 Xbox Live 多人遊戲資料](get-xbox-live-multiplayer-data.md)
