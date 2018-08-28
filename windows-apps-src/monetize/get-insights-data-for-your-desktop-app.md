---
author: mcleanbyron
description: 取得您的桌面應用程式的觀點資料 Microsoft 存放區分析 API 中的使用此方法。
title: 取得您的桌面應用程式的觀點資料
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 Store 服務、 Microsoft 存放區分析 API 觀點
ms.localizationpriority: medium
ms.openlocfilehash: e7ca6eed40af37276b5b4c98ec7b1b709bdadfb9
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2018
ms.locfileid: "2882984"
---
# <a name="get-insights-data-for-your-desktop-application"></a>取得您的桌面應用程式的觀點資料

若要取得您已新增至[Windows 桌面應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)的桌面應用程式的健康狀況計量資料相關的觀點，Microsoft 存放區分析 API 中使用此方法。 此資料也可用於 Windows 開發人員中心儀表板中的桌面應用程式的[狀況報告](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)。

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
| applicationId | 字串 | 您要取得前瞻資料桌面應用程式的產品識別碼。 若要取得傳統型應用程式的產品識別碼，請開啟任何[傳統型應用程式的開發人員中心分析報告](https://msdn.microsoft.com/library/windows/desktop/mt826504) (例如**健康報告**)，並從 URL 擷取產品識別碼。 如果您未指定此參數，回應本文將會包含前瞻資料的所有應用程式註冊您的帳戶。  |  否  |
| startDate | 日期 | 若要擷取之前瞻資料的日期範圍中開始日期。 預設為目前日期的前 30 天。 |  否  |
| endDate | 日期 | 若要擷取前瞻資料的日期範圍內結束日期。 預設為目前的日期。 |  否  |
| filter | 字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 例如，*篩選 = dataType eq '擷取'*。 <p/><p/>目前此方法僅支援篩選**狀況**。  | 否   |

### <a name="request-example"></a>要求範例

下列範例會示範如何取得前瞻資料的要求。 *ApplicationId*值取代為您的桌面應用程式的適當的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 包含應用程式的觀點資料的陣列。 如需每個物件中的資料的詳細資訊，請參閱下面的[洞察力值](#insight-values)] 區段。                                                                                                                      |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                 |


### <a name="insight-values"></a>洞察力值

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字串 | 您將前瞻資料擷取其桌面應用程式的產品識別碼。     |
| insightDate                | 字串 | 我們找出在某個特定評量變更的日期。 此日期代表結尾的星期我們偵測到大幅增加或減少評量相較於之前的週中。 |
| 資料類型     | 字串 | 會指定一般的分析區域據此通知字串。 目前，此方法僅支援**狀況**。    |
| insightDetail          | array | 一或多個[InsightDetail 值](#insightdetail-values)，代表目前洞察力的詳細資料。    |


### <a name="insightdetail-values"></a>InsightDetail 值

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| FactName           | 字串 | 字串，表示目前的獨到或目前的維度說明評量。 目前，此方法僅支援值**叫用次數**。  |
| SubDimensions         | array |  說明洞察力單一計量的一或多個物件。   |
| PercentChange            | 字串 |  評量變更跨整個客戶群的百分比。  |
| DimensionName           | 字串 |  目前的維度中所述的計量的名稱。 範例包括**EventType**、**市場**、 **DeviceType**及**PackageVersion**。   |
| DimensionValue              | 字串 | 目前的維度中所述的衡量標準的值。 例如，如果**EventType** **DimensionName** ， **DimensionValue**可能**損毀**或**當機**。   |
| FactValue     | 字串 | 在偵測到洞察力日期計量絕對值。  |
| Direction | 字串 |  變更 （**正數**或**負數**） 的方向。   |
| 日期              | 字串 |  我們所識別的變更與目前的獨到或目前的維度相關的日期。   |

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

* [Windows 桌面應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [健康情況報告](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
