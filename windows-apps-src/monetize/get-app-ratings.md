---
author: Xansky
ms.assetid: DD4F6BC4-67CD-4AEF-9444-F184353B0072
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得彙總評分資料。
title: 取得應用程式評分
ms.author: mhopkins
ms.date: 11/29/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 評分
ms.localizationpriority: medium
ms.openlocfilehash: 6b118abe32fe350277e02ed1f1f1d721e4c1690e
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6647526"
---
# <a name="get-app-ratings"></a>取得應用程式評分

使用「Microsoft Store 分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得彙總評分資料 (JSON 格式)。 這項資訊也會在合作夥伴中心中的[評論] 報告](../publish/reviews-report.md)中提供的。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。


## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取評分資料之應用程式的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| startDate | 日期 | 要擷取評分資料之日期範圍的開始日期。 預設為目前的日期。 |  否  |
| endDate | 日期 | 要擷取評分資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 在回應中篩選資料列的一或多個陳述式。 如需更多資訊，請參閱下方的＜[篩選欄位](#filter-fields)＞一節。 | 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。 | 否 |
| orderby | 字串 | 對每個評分的結果資料值做出排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>日期</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>fiveStars</strong></li><li><strong>fourStars</strong></li><li><strong>threeStars</strong></li><li><strong>twoStars</strong></li><li><strong>oneStar</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：<em>&amp;groupby=osVersion,market&amp;aggregationLevel=week</em></p> |  否  |

 
### <a name="filter-fields"></a>篩選欄位

要求的 *filter* 參數包含在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位和值，而陳述式可以使用 **and** 或 **or** 結合。

以下為 *filter* 字串的範例：*filter=market eq 'US' and deviceType eq 'phone' and isRevised eq true*

如需支援欄位的清單，請參閱下列表格。 *filter* 參數中的字串值必須由單引號括住。

| 欄位        |  描述        |
|---------------|-----------------|
| market | 內含 App 所評分市場之 ISO 3166 國家/地區代碼的字串。 |
| osVersion | 下列其中一個字串：<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows8.1</strong></li><li><strong>Windows10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | 下列其中一個字串：<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| isRevised | 指定 <strong>true</strong> 以篩選已修訂的評分，否則請指定 <strong>false</strong>。 |


### <a name="request-example"></a>要求範例

下列範例示範取得評分資料的數個要求。 將 *applicationId* 值以您 app 的 Store 識別碼取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 值      | array  | 內含彙總評分資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[評分數值](#rating-values)一節。                                                                                                                           |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的評分資料，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                               |


### <a name="rating-values"></a>評分數值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述       |
|-----------------|---------|-------------------|
| 日期            | 字串  | 評分資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId   | 字串  | 您正在擷取評分資料之 app 的 Store 識別碼。         |
| applicationName | 字串  | App 的顯示名稱。    |
| market          | 字串  | 提交評分之市場的 ISO 3166 國家/地區碼。        |
| osVersion       | 字串  | 提交評分的 OS 版本。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。            |
| deviceType      | 字串  | 提交評分的裝置類型。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。            |
| isRevised       | 布林值 | **true** 值表示評分已修訂，否則為 **false**。   |
| oneStar         | 數字  | 一顆星評分的數目。        |
| twoStars        | 數字  | 兩顆星評分的數目。    |
| threeStars      | 數字  | 三顆星評分的數目。   |
| fourStars       | 數字  | 四顆星評分的數目。    |
| fiveStars       | 數字  | 五顆星評分的數目。    |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2015-02-13",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "CN",
      "osVersion": "8.0.10517.0",
      "deviceType": "Phone",
      "isRevised": false,
      "oneStar": 0,
      "twoStars": 0,
      "threeStars": 0,
      "fourStars": 0,
      "fiveStars": 2
    }
  ],
  "@nextLink": "ratings?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 15242
}

```

## <a name="related-topics"></a>相關主題

* [評等報告](../publish/reviews-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得應用程式下載數](get-app-acquisitions.md)
* [取得附加元件下載數](get-in-app-acquisitions.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
* [取得 App 評論](get-app-reviews.md)
