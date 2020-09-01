---
description: 在 Microsoft Store 分析 API 中使用此方法來取得桌面應用程式的深入解析資料。
title: 取得傳統型應用程式的深入解析資料
ms.date: 07/31/2018
ms.topic: article
keywords: windows 10、uwp、商店服務、Microsoft Store 分析 API、見解
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd60425a5ec3c040417aded818c766db80eb59eb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172892"
---
# <a name="get-insights-data-for-your-desktop-application"></a>取得傳統型應用程式的深入解析資料

在 Microsoft Store 分析 API 中使用這個方法，即可取得與您已新增至 [Windows 桌面應用](/windows/desktop/appxpkg/windows-desktop-application-program)程式的桌面應用程式健康情況計量相關的深入解析資料。 合作夥伴中心中的桌面應用程式 [健康情況報告](/windows/desktop/appxpkg/windows-desktop-application-program#health-report) 也提供此資料。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您要取得見解資料的桌面應用程式產品識別碼。 若要取得傳統型應用程式的產品識別碼，請 [在合作夥伴中心 (中開啟桌面應用程式的任何分析報表](/windows/desktop/appxpkg/windows-desktop-application-program) ，例如 **健康情況報表**) ，並從 URL 取出產品識別碼。 如果您未指定此參數，回應主體會包含所有註冊至您帳戶的應用程式的見解資料。  |  否  |
| startDate | date | 要取出之見解資料的日期範圍開始日期。 預設為目前日期的前 30 天。 |  否  |
| endDate | date | 要取出之見解資料的日期範圍中的結束日期。 預設值是目前的日期。 |  否  |
| filter | 字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *篩選* 參數中的字串值必須由單引號括住。 例如， *filter = dataType eq ' 收購 '*。 <p/><p/>此方法目前僅支援篩選準則 **健康**情況。  | 否   |

### <a name="request-example"></a>要求範例

下列範例示範取得深入解析資料的要求。 將 *applicationId* 值取代為桌面應用程式的適當值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

### <a name="response-body"></a>回應本文

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 物件的陣列，其中包含應用程式的深入解析資料。 如需每個物件中資料的詳細資訊，請參閱下方的 [深入解析值](#insight-values) 一節。                                                                                                                      |
| TotalCount | int    | 查詢之資料結果的資料列總數。                 |


### <a name="insight-values"></a>深入解析值

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 說明                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字串 | 您抓取見解資料的桌面應用程式產品識別碼。     |
| insightDate                | 字串 | 我們在特定度量中識別變更的日期。 此日期代表一周的結尾，相較于那一周，我們偵測到計量大幅增加或減少。 |
| dataType     | 字串 | 字串，指定此深入解析所通知的一般分析區域。 目前，此方法只支援 **健全狀況**。    |
| insightDetail          | array | 一或多個 [InsightDetail 值](#insightdetail-values) ，代表目前見解的詳細資料。    |


### <a name="insightdetail-values"></a>InsightDetail 值

| 值               | 類型   | 說明                           |
|---------------------|--------|-------------------------------------------|
| FactName           | 字串 | 字串，表示目前的深入解析或目前的維度所描述的度量。 目前，這個方法只支援 **擊中數**值。  |
| SubDimensions         | array |  一或多個物件，描述深入解析的單一度量。   |
| PercentChange            | 字串 |  計量在整個客戶群之間變更的百分比。  |
| DimensionName           | 字串 |  目前維度中所描述的度量名稱。 範例包括 **：//、****市場**、 **DeviceType**和**PackageVersion**。   |
| DimensionValue              | 字串 | 目前維度中所描述的度量值。 例如，如果**DimensionName**是 DimensionValue **，** 則**DimensionValue**可能會損**毀**或停止**回應**。   |
| FactValue     | 字串 | 偵測到深入解析日期時的度量絕對值。  |
| 方向 | 字串 |  變更方向 (**正面** 或 **負面**) 。   |
| 日期              | 字串 |  我們識別出與目前見解或目前維度相關之變更的日期。   |

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

* [Windows 桌面應用程式](/windows/desktop/appxpkg/windows-desktop-application-program)
* [健康情況報告](/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)