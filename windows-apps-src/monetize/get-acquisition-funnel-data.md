---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得應用程式的取得漏斗資料。
title: 取得應用程式取得漏斗圖資料
ms.author: mhopkins
ms.date: 08/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, Microsoft Store 服務, Microsoft Store 分析 API, 取得, 漏斗
ms.localizationpriority: medium
ms.openlocfilehash: 362bcc956fa5945f9685aac7d6351b9fda7690de
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5397592"
---
# <a name="get-app-acquisition-funnel-data"></a>取得應用程式取得漏斗圖資料

在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得應用程式的取得漏斗資料。 「Windows 開發人員中心」儀表板中的[下載數報告](../publish/acquisitions-report.md#acquisition-funnel)也有提供這項資訊。

## <a name="prerequisites"></a>先決條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取取得漏斗資料之應用程式的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 舉例來說，Store 識別碼可以是「9WZDNCRFJ3Q8」。 |  是  |
| startDate | 日期 | 要擷取取得漏斗之日期範圍的開始日期。 預設為目前的日期。 |  否  |
| endDate | 日期 | 要擷取取得漏斗資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| filter | 字串  | 在回應中篩選資料列的一或多個陳述式。 如需更多資訊，請參閱下方的＜[篩選欄位](#filter-fields)＞一節。 | 否   |

 
### <a name="filter-fields"></a>篩選欄位

要求的 *filter* 參數包含在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位和值，而陳述式可以使用 **and** 或 **or** 結合。

支援下列篩選欄位。 *filter* 參數中的字串值必須由單引號括住。

| 欄位        |  描述        |
|---------------|-----------------|
| campaignId | 與取得相關聯之[自訂應用程式促銷活動](../publish/create-a-custom-app-promotion-campaign.md)的識別碼字串。 |
| market | 內含發生下載之市場的 ISO 3166 國家/地區碼的字串。 |
| deviceType | 下列其中一個字串，指定發生取得的裝置類型：<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| ageGroup | 下列其中一個字串，指定完成取得之使用者的年齡層：<ul><li><strong>0 – 17</strong></li><li><strong>18 – 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50 或以上</strong></li><li><strong>不明</strong></li></ul> |
| gender | 下列其中一個字串，指定完成取得之使用者的性別：<ul><li><strong>M</strong></li><li><strong>F</strong></li><li><strong>不明</strong></li></ul> |


### <a name="request-example"></a>要求的範例

下列範例示範數個取得應用程式取得漏斗資料的要求。 將 *applicationId* 值取代為您應用程式的 Store 識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 物件陣列，內含應用程式的取得漏斗資料。 如需有關每個物件中資料的詳細資訊，請參閱下方的[漏斗值](#funnel-values)一節。                  |
| TotalCount | 整數    | *Value* 陣列中的物件總數。        |


### <a name="funnel-values"></a>漏斗值

*Value* 陣列中的物件包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | 字串 | 下列其中一個字串，指定此物件中包含的[漏斗資料類型](../publish/acquisitions-report.md#acquisition-funnel)：<ul><li><strong>PageView</strong></li><li><strong>Acquisition</strong></li><li><strong>Install</strong></li><li><strong>Usage</strong></li></ul> |
| UserCount       | 字串 | 執行 *MetricType* 值指定之漏斗步驟的使用者數。             |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "MetricType": "PageView",
      "UserCount": 100
    },
    {
      "MetricType": "Acquisition",
      "UserCount": 80
    },
    {
      "MetricType": "Install",
      "UserCount": 50
    },
    {
      "MetricType": "Usage",
      "UserCount": 10
    }
  ],
  "TotalCount": 4
}
```

## <a name="related-topics"></a>相關主題

* [下載數報告](../publish/acquisitions-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得 App 下載數](get-app-acquisitions.md)
