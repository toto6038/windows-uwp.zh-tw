---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法取得 Xbox Live 分析資料。
title: 取得 Xbox Live 分析資料
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、Microsoft Store 服務、Microsoft Store 分析 API、Xbox Live 分析
ms.localizationpriority: medium
ms.openlocfilehash: 68bc3389276acc910272d57e9cc859a7e8ffd6c7
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5514952"
---
# <a name="get-xbox-live-analytics-data"></a>取得 Xbox Live 分析資料

在 Microsoft Store 分析 API 中使用此方法可取得玩 [Xbox Live-enabled game](../xbox-live/index.md) 的客戶的過去 30 天的一般分析資料，包括裝置配件使用量、網際網路連接類型、玩家分數分佈情況、遊戲統計資料以及朋友和追隨者資料。 「Windows 開發人員中心」儀表板中的 [Xbox 分析報告](../publish/xbox-analytics-report.md)也有提供這項資訊。

> [!IMPORTANT]
> 此方法僅支援 Xbox 遊戲，或使用 Xbox Live 服務的遊戲。 這些遊戲必須通盤了解[概念核准程序](../gaming/concept-approval.md)，包括 [Microsoft 合作夥伴](../xbox-live/developer-program-overview.md#microsoft-partners)發行的遊戲，以及透過 [ [ID@Xbox程式](../xbox-live/developer-program-overview.md#id)提交的遊戲。 此方法目前不支援透過 [Xbox Live 創作者計畫](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) 發佈遊戲。

可透過下列方法取得支援已啟用 Xbox Live 遊戲的其他分析資料：
* [取得 Xbox Live 成就資料](get-xbox-live-achievements-data.md)
* [取得 Xbox Live 健康情況資料](get-xbox-live-health-data.md)
* [取得 Xbox Live 遊戲中心資料](get-xbox-live-game-hub-data.md)
* [取得 Xbox Live 俱樂部資料](get-xbox-live-club-data.md)
* [取得 Xbox Live 多人遊戲資料](get-xbox-live-multiplayer-data.md)
* [取得 Xbox Live 並行使用資料](get-xbox-live-concurrent-usage-data.md)

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
| applicationId | 字串 | 您想要擷取一般 Xbox Live 分析資料之遊戲的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字串 | 指定要擷取 Xbox Live 分析資料類型的字串。 對於此方法，請指定 **productvalues**值。  |  是  |


### <a name="request-example"></a>要求的範例

以下範例示範了為玩已啟用 Xbox Live 遊戲的客戶取得一般分析資料的要求。 將 *applicationId* 值以您遊戲的 Store 識別碼取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

此方法傳回包含下列物件的*值*陣列。

| 物件      | 描述                  |
|-------------|---------------------------------------------------|
| ProductData   |   包含一個 [DeviceProperties](#deviceproperties) 物件和一個 [UserProperties](#userproperties) 物件，該物件包含過去 30 天遊戲的裝置和使用者分析資料。    |  
| XboxwideData   |  包含一個 [DeviceProperties](#deviceproperties) 物件和一個 [UserProperties](#userproperties) 物件，其中包含了過去 30 天內所有 Xbox Live 客戶裝置和使用者分析的平均資料，以百分比表示。 包含這些資料是為了與遊戲資料作比較。   |                                           


### <a name="deviceproperties"></a>DeviceProperties

此資源包含遊戲的裝置使用情況資料或過去 30 天所有 Xbox Live 客戶的平均裝置使用情況資料。

| 數值           | 類型    | 描述        |
|-----------------|---------|------|
|  applicationId               |    字串     |  遊戲中的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)，用於擷取分析資料。   |
|  connectionTypeDistribution               |    陣列     |   包含指出在 Xbox 上有多少客戶使用有線網際網路連線與無線網際網路連線的物件。 每個物件都有兩個字串欄位︰ <ul><li>**conType**：指定連線的類型。</li><li>**deviceCount**：在 **ProductData** 物件中，此欄位詳列使用連線類型的遊戲客戶的數目。 在 **XboxwideData** 物件，此欄位詳列所有使用連線類型之 Xbox Live 客戶之百分比。</li></ul>   |     
|  deviceCount               |   字串      |  在 **ProductData** 物件，此欄位詳列在過去 30 天內玩過您的遊戲的客戶裝置的數目。 在 **XboxwideData** 物件，此欄位一律 1，表示所有 Xbox Live 客戶的資料的起始百分比均為100％。   |     
|  eliteControllerPresentDeviceCount               |   字串      |  在 **ProductData** 物件中，此欄位詳列使用 Xbox Elite 無線控制器的遊戲客戶的數目。 在 **XboxwideData** 物件，此欄位詳列使用 Xbox Elite 無線控制器的所有 Xbox Live 客戶之百分比。  |     
|  externalDrivePresentDeviceCount               |   字串      |  在 **ProductData** 物件中，此欄位詳列在 Xbox 上使用外接式硬碟的遊戲客戶的數目。 在 **XboxwideData** 物件，此欄位詳列在 Xbox 上使用外接式硬碟的所有 Xbox Live 客戶之百分比。  |


### <a name="userproperties"></a>UserProperties

此資源包含遊戲的使用者資料或過去 30 天所有 Xbox Live 客戶的平均使用者資料。

| 數值           | 類型    | 描述        |
|-----------------|---------|------|
|  applicationId               |    字串     |   用於擷取分析資料的遊戲的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |
|  userCount               |    字串     |   在 **ProductData** 物件，此欄位詳列在過去 30 天玩過您的遊戲的客戶數目。 在 **XboxwideData** 物件，此欄位一律 1，表示所有 Xbox Live 客戶的資料的起始百分比均為100％。   |     
|  dvrUsageCounts               |   陣列      |  包含指示有多少客戶使用遊戲 DVR 來記錄和檢視遊戲的物件。 每個物件都有兩個字串欄位︰ <ul><li>**dvrName**：指定使用的遊戲 DVR 功能。 可能的值為 **gameClipUploads**、**gameClipViews**、**screenshotUploads** 和 **screenshotViews**。</li><li>**userCount**：在 **ProductData** 物件中，此欄位詳列使用指定的遊戲 DVR 功能的遊戲客戶數目。 在 **XboxwideData** 物件，此欄位詳列使用指定的遊戲 DVR 功能的所有 Xbox Live 客戶之百分比。</li></ul>   |     
|  followerCountPercentiles               |   陣列      |  包含為客戶提供有關追隨者數量的詳細資訊的物件。 每個物件都有兩個字串欄位︰ <ul><li>**百分比**：目前這個值都 50，表示追隨者資料是以中間值作提供的。</li><li>**數值**：在 **ProductData** 物件，此欄位詳列遊戲客戶的追隨者數量的中位數。 在 **XboxwideData** 物件，此欄位詳列所有 Xbox Live 客戶的追隨者數量的中位數。</li></ul>  |   
|  friendCountPercentiles               |   陣列      |  包含為客戶提供有關朋友數量的詳細資訊的物件。 每個物件都有兩個字串欄位︰ <ul><li>**百分比**：目前這個值都 50，表示朋友資料是以中位數提供的。</li><li>**數值**：在 **ProductData** 物件，此欄位詳列遊戲客戶的朋友數量的中位數。 在 **XboxwideData** 物件，此欄位詳列所有 Xbox Live 客戶的朋友數量的中位數。</li></ul>  |     
|  gamerScoreRangeDistribution               |   陣列      |  包含為客戶提供有關遊戲玩家分佈的詳細資訊的物件。 每個物件都有兩個字串欄位︰ <ul><li>**scoreRange**：用於下列欄位所提供使用資料的玩家分數範圍。 例如，**10K-25K**。</li><li>**userCount**：在 **ProductData** 物件，此欄位詳列遊戲客戶在指定範圍內所有擁有玩家分數的遊戲客戶數目，而這些遊戲已被他們玩過。 在 **XboxwideData** 物件，此欄位詳列遊戲客戶在指定範圍內所有擁有玩家分數的 Xbox Live 客戶之百分比，而這些遊戲已被他們玩過。</li></ul>  |
|  titleGamerScoreRangeDistribution               |   陣列      |  包含為您的遊戲提供有關遊戲玩家分佈的詳細資訊的物件。 每個物件都有兩個字串欄位︰ <ul><li>**scoreRange**：下列欄位提供使用狀況資料的玩家分數範圍。 例如，**100-200**。</li><li>**userCount**：在 **ProductData** 物件，此欄位詳列您的遊戲指定範圍內所有擁有玩家分數的遊戲客戶數目。 在 **XboxwideData** 物件，此欄位詳列遊戲客戶在您的遊戲指定範圍內所有擁有玩家分數的 Xbox Live 客戶之百分比。</li></ul>   |
|  socialUsageCounts               |   陣列      |  包含為客戶提供有關社群使用的詳細資訊的物件。 每個物件都有兩個字串欄位︰ <ul><li>**scName**：社群使用的類型。 例如，**gameInvites** 和 **textMessages**。</li><li>**userCount**：在 **ProductData** 物件，此欄位詳列參與指定社群使用類型的遊戲客戶數量。 在 **XboxwideData** 物件，此欄位詳列參與指定社群使用類型的所有 Xbox Live 客戶之百分比。</li></ul>   |
|  streamingUsageCounts               |   陣列      |  包含為客戶提供有關串流使用的詳細資訊的物件。 每個物件都有兩個字串欄位︰ <ul><li>**stName**：串流平台的類型。 例如，**youtubeUsage**、**twitchUsage** 和 **mixerUsage**。</li><li>**userCount**：在 **ProductData** 物件中，此欄位詳列已使用指定串流平台的遊戲客戶數目。 在 **XboxwideData** 物件，此欄位詳列使用指定串流平台的所有 Xbox Live 客戶之百分比。</li></ul>  |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "ProductData": {
        "DeviceProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "43806"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "104035"
              }
            ],
            "deviceCount": "148063",
            "eliteControllerPresentDeviceCount": "10615",
            "externalDrivePresentDeviceCount": "46388"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "userCount": "142345",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "31264"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "52236"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "27051"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "45640"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "30015"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "20495"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "32438"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "10608"
              },
              {
                "scoreRange": "<3K",
                "userCount": "45726"
              },
              {
                "scoreRange": ">100K",
                "userCount": "3063"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "400-600",
                "userCount": "133875"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "45960"
              },
              {
                "scoreRange": "<100",
                "userCount": "269137"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "11634"
              },
              {
                "scoreRange": "100-200",
                "userCount": "334471"
              },
              {
                "scoreRange": "600-800",
                "userCount": "123044"
              },
              {
                "scoreRange": "200-400",
                "userCount": "396725"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "82390"
              },
              {
                "scName": "textMessages",
                "userCount": "91880"
              },
              {
                "scName": "partySessionCount",
                "userCount": "68129"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "74092"
              },
              {
                "stName": "twitchUsage",
                "userCount": "13401"
              }
              {
                "stName": "mixerUsage",
                "userCount": "22907"
              }
            ]
          }
        ]
      },
      "XboxwideData": {
        "DeviceProperties": [
          {
            "applicationId": "XBOXWIDE",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "0.213677732584786"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "0.786322267415214"
              }
            ],
            "deviceCount": "1",
            "eliteControllerPresentDeviceCount": "0.0476609278128012",
            "externalDrivePresentDeviceCount": "0.173747147416134"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "XBOXWIDE",
            "userCount": "1",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "0.173210623993245"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "0.202104713778096"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "0.136682414274251"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "0.158057895120314"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "0.134709282586519"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "0.0549468789343825"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "0.017301313342277"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "0.216512780268453"
              },
              {
                "scoreRange": "<3K",
                "userCount": "0.573515440094644"
              },
              {
                "scoreRange": ">100K",
                "userCount": "0.00301430477372488"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "100-200",
                "userCount": "0.178055695637076"
              },
              {
                "scoreRange": "200-400",
                "userCount": "0.173283639825241"
              },
              {
                "scoreRange": "400-600",
                "userCount": "0.0986865193958259"
              },
              {
                "scoreRange": "600-800",
                "userCount": "0.0506375775462092"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "0.0232398822856435"
              },
              {
                "scoreRange": "<100",
                "userCount": "0.456443551582991"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "0.0196531337270126"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "0.460375855738335"
              },
              {
                "scName": "textMessages",
                "userCount": "0.429256324070832"
              },
              {
                "scName": "partySessionCount",
                "userCount": "0.378446577751268"
              },
              {
                "scName": "gamehubViews",
                "userCount": "0.000197115778147329"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "0.330320919178683"
              },
              {
                "stName": "twitchUsage",
                "userCount": "0.040666241835399"
              }
              {
                "stName": "mixerUsage",
                "userCount": "0.140193816053558"
              }
            ]
          }
        ]
      }
    }
  ],
  "@nextLink": null,
  "TotalCount": 4
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得 Xbox Live 成就資料](get-xbox-live-achievements-data.md)
* [取得 Xbox Live 健康情況資料](get-xbox-live-health-data.md)
* [取得 Xbox Live 遊戲中心資料](get-xbox-live-game-hub-data.md)
* [取得 Xbox Live 俱樂部資料](get-xbox-live-club-data.md)
* [取得 Xbox Live 多人遊戲資料](get-xbox-live-multiplayer-data.md)
* [取得 Xbox Live 並行使用資料](get-xbox-live-concurrent-usage-data.md)
