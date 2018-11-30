---
description: 在 Microsoft Store 分析 API 中使用這個方法，取得傳統型應用程式的深入解析資料。
title: 取得傳統型應用程式的深入解析資料
ms.date: 07/31/2018
ms.topic: article
keywords: windows 10，uwp，microsoft Store 服務，Microsoft Store 分析 API，深入解析
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5545d27668b23e5b7ae91201421dfa4c92f9c8ed
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8328879"
---
# <a name="get-insights-data-for-your-desktop-application"></a>取得傳統型應用程式的深入解析資料

在 Microsoft Store 分析 API 中使用這個方法，來取得與您已新增到[Windows 傳統型應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)的傳統型應用程式的健康情況計量資料相關的深入資訊。 這項資料也會在合作夥伴中心的傳統型應用程式的[健康情況報告](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)中提供的。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | string | 您想要取得深入解析資料的傳統型應用程式的產品識別碼。 若要取得傳統型應用程式的產品識別碼，開啟任何 （例如**健康情況報告**） 的[傳統型應用程式在合作夥伴中心分析報告](https://msdn.microsoft.com/library/windows/desktop/mt826504)，並從 URL 擷取產品識別碼。 如果您未指定此參數，回應主體會包含針對登錄到您帳戶的所有應用程式的深入解析資料。  |  否  |
| startDate | 日期 | 要擷取的深入解析資料之日期範圍中開始日期。 預設為目前日期的前 30 天。 |  否  |
| endDate | 日期 | 要擷取的深入解析資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| filter | 字串  | 在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 例如， *filter = dataType eq '取得'*。 <p/><p/>目前此方法僅支援篩選**健康情況**。  | 否   |

### <a name="request-example"></a>要求範例

下列範例示範取得深入解析資料的要求。 值取代為*applicationId*傳統型應用程式的適當的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 包含應用程式的深入解析資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[深入解析值](#insight-values)一節。                                                                                                                      |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                 |


### <a name="insight-values"></a>深入了解值

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | 您已擷取的深入解析資料的傳統型應用程式的產品識別碼。     |
| insightDate                | string | 我們所識別在特定的衡量標準的變更日期。 此日期代表我們偵測到大幅增加一週結尾，或減少度量單位，相較於前一週中。 |
| 資料類型     | string | 指定的字串，會通知這個深入了解一般分析區域。 目前，此方法僅支援**健康情況**。    |
| insightDetail          | array | 一或多個[InsightDetail 值](#insightdetail-values)代表目前的深入解析的詳細資料。    |


### <a name="insightdetail-values"></a>InsightDetail 值

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | 表示為目前的深入解析或目前維度描述的衡量標準的字串。 目前，此方法僅支援值**叫用次數**。  |
| SubDimensions         | array |  一或多個物件，其描述單一的衡量標準的深入解析。   |
| PercentChange            | string |  跨整個客戶群的銷售量變更為的衡量標準的百分比。  |
| DimensionName           | string |  為目前的維度中所述的衡量標準的名稱。 範例包括**EventType**、**市場**、 **DeviceType**，以及**PackageVersion**。   |
| DimensionValue              | string | 目前的維度中所述的衡量標準的值。 例如，如果**DimensionName** **EventType**， **DimensionValue**可能會**損毀**或**停止回應**。   |
| FactValue     | string | 深入了解已偵測到的日期的衡量標準絕對值。  |
| Direction | string |  變更 （**正**或**負**） 的方向。   |
| 日期              | 字串 |  我們識別出目前的深入解析或目前的維度相關的變更日期。   |

### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>相關主題

* [Windows 傳統型應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [健康情況報告](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
