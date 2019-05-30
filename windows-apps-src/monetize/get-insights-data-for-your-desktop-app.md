---
description: 在 Microsoft Store analytics API 中使用這個方法，以取得您的桌面應用程式的深入解析資料。
title: 取得傳統型應用程式的深入解析資料
ms.date: 07/31/2018
ms.topic: article
keywords: windows 10、 uwp、 存放區服務、 Microsoft Store 分析 API，深入解析
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8f6f4b2df1cda14bc1f363a1f9100e416f26489b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372458"
---
# <a name="get-insights-data-for-your-desktop-application"></a>取得傳統型應用程式的深入解析資料

使用 Microsoft Store 分析 API，以取得深入解析資料中的，這個方法與您加入的桌面應用程式的健全狀況度量[Windows 桌面應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)。 這項資料也會提供[健康情況報告](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)在合作夥伴中心內的桌面應用程式。

## <a name="prerequisites"></a>先決條件

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
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要項  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要取得深入解析資料的桌面應用程式產品識別碼。 若要取得桌面應用程式的產品識別碼，請開啟任何[桌面應用程式以在合作夥伴中心中的分析報告](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)(例如**健康情況報告**)，並從 URL 擷取產品識別碼。 如果您未指定此參數，回應主體會包含您的帳戶已註冊的所有應用程式的深入解析資料。  |  否  |
| startDate | date | 若要擷取的 insights 資料的日期範圍中開始日期。 預設為目前日期的前 30 天。 |  否  |
| endDate | date | 若要擷取的 insights 資料的日期範圍中結束日期。 預設為目前的日期。 |  否  |
| filter | 字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 例如，*篩選器 = 資料類型 eq '擷取'* 。 <p/><p/>這個方法目前只支援的篩選器**健全狀況**。  | 否   |

### <a name="request-example"></a>要求範例

下列範例會示範取得深入解析資料的要求。 取代*applicationId*與桌面應用程式的適當值的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 陣列  | 包含應用程式的深入解析資料的物件陣列。 如需每個物件中資料的詳細資訊，請參閱[Insight 值](#insight-values)下一節。                                                                                                                      |
| TotalCount | ssNoversion    | 查詢之資料結果的資料列總數。                 |


### <a name="insight-values"></a>深入解析的值

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字串 | 您可以為其擷取深入解析資料的桌面應用程式產品識別碼。     |
| insightDate                | 字串 | 日期，我們所識別的特定度量的變更。 此日期表示我們偵測到大幅增加一週結尾，或減少相較於之前一週的計量。 |
| dataType     | 字串 | 字串，指定此深入解析會通知的一般分析區域。 目前，這個方法只支援**健全狀況**。    |
| insightDetail          | 陣列 | 一或多個[InsightDetail 值](#insightdetail-values)，代表目前的深入解析的詳細資料。    |


### <a name="insightdetail-values"></a>InsightDetail 值

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| FactName           | 字串 | 字串，表示目前維度的目前的深入解析描述計量。 目前，這個方法只支援值**叫用次數**。  |
| SubDimensions         | 陣列 |  描述單一計量付費的一或多個物件。   |
| PercentChange            | 字串 |  百分比度量變更跨整個客戶群。  |
| DimensionName           | 字串 |  目前維度中所述的計量名稱。 範例包括**EventType**，**市場**， **DeviceType**，以及**PackageVersion**。   |
| DimensionValue              | 字串 | 目前維度中所述的度量值。 例如，如果**DimensionName**是**EventType**， **DimensionValue**可能**損毀**或**懸置**.   |
| FactValue     | 字串 | 深入解析已偵測到當天計量的絕對值。  |
| Direction | 字串 |  變更的方向 (**正**或是**負**)。   |
| Date              | 字串 |  日期，我們找出與目前的深入解析或目前維度相關的變更。   |

### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

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
* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)
