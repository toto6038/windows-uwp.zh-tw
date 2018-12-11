---
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器取得評論資料。
title: 取得應用程式評論
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 評論
ms.localizationpriority: medium
ms.openlocfilehash: 084158c0eb20f1d2a03c0e178064ac168c689872
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8879860"
---
# <a name="get-app-reviews"></a>取得應用程式評論


使用「Microsoft Store 分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得評論資料 (JSON 格式)。 這項資訊也會在合作夥伴中心中的[評論] 報告](../publish/reviews-report.md)中提供的。

在擷取評論之後，您可以使用 Microsoft Store 評論 API 中的[取得應用程式評論的回應資訊](get-response-info-for-app-reviews.md)和[提交應用程式評論的回應](submit-responses-to-app-reviews.md)方法，以程式設計的方式回應評論。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|---------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取評論資料之應用程式的　[Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| startDate | 日期 | 要擷取評論資料之日期範圍的開始日期。 預設為目前的日期。 |  否  |
| endDate | 日期 | 要擷取評論資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter |字串  | 在回應中篩選資料列的一或多個陳述式。 如需更多資訊，請參閱下方的＜[篩選欄位](#filter-fields)＞一節。 | 否   |
| orderby | 字串 | 將結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>日期</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li><li><strong>packageVersion</strong></li><li><strong>deviceModel</strong></li><li><strong>productFamily</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewText</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>rating</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |


### <a name="filter-fields"></a>篩選欄位

要求的 *filter* 參數包含在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位和值，而某些欄位同時也支援 **contains**、**gt**、**lt**、**ge** 及 **le** 運算子。 陳述式可以使用 **and** 或 **or** 來結合。

下列為 *filter* 字串的範例：*filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

如需支援的欄位，以及每個欄位所支援的運算子清單，請參閱下列表格。 *filter* 參數中的字串值必須由單引號括住。

| 欄位        | 支援的運算子   |  描述        |
|---------------|--------|-----------------|
| market | eq、ne | 內含裝置市場的 ISO 3166 國家/地區碼的字串。 |
| osVersion  | eq、ne  | 下列其中一個字串：<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows8.1</strong></li><li><strong>Windows10</strong></li><li><strong>Unknown</strong></li></ul>  |
| deviceType  | eq、ne  | 下列其中一個字串：<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>  |
| isRevised  | eq、ne  | 指定 <strong>true</strong> 以篩選已修訂的評論，否則請指定 <strong>false</strong>。  |
| packageVersion  | eq、ne  | 已評論的應用程式套件版本。  |
| deviceModel  | eq、ne  | 評論 app 的裝置類型。  |
| productFamily  | eq、ne  | 下列其中一個字串：<ul><li><strong>PC</strong></li><li><strong>Tablet</strong></li><li><strong>Phone</strong></li><li><strong>Wearable</strong></li><li><strong>Server</strong></li><li><strong>Collaborative</strong></li><li><strong>Other</strong></li></ul>  |
| deviceRAM  | eq、ne、gt、lt、ge、le  | 實體 RAM (以 MB 為單位)。  |
| deviceScreenResolution  | eq、ne  | 裝置的螢幕解析度，格式為「&quot;<em>width</em> x <em>height</em>&quot;」。   |
| deviceStorageCapacity  | eq、ne、gt、lt、ge、le   | 主要存放磁碟的容量 (以 GB 為單位)。  |
| isTouchEnabled  | eq、ne  | 指定 <strong>true</strong> 以篩選具有觸控功能的裝置，否則請指定 <strong>false</strong>。   |
| reviewerName  | eq、ne  |  評論者名稱。 |
| rating  | eq、ne、gt、lt、ge、le  | App 評分 (以星星為單位)。  |
| reviewTitle  | eq、ne、contains  | 評論的標題。  |
| reviewText  | eq、ne、contains  |  評論的文字內容。 |
| helpfulCount  | eq、ne  |  該評論被標記為「很有幫助」的次數。 |
| notHelpfulCount  | eq、ne  | 該評論被標記為「沒有幫助」的次數。  |
| responseDate  | eq、ne  | 提交回應的日期。  |
| responseText  | eq、ne、contains  | 回應的文字內容。  |
| id  | eq、ne  | 評論的識別碼 (這是 GUID)。        |


### <a name="request-example"></a>要求的範例

下列範例示範取得評論資料的數個要求。 將 *applicationId* 值以您 app 的 Store 識別碼取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述      |
|------------|--------|------------------|
| 值      | array  | 包含評論資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[評論數值](#review-values)一節。       |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的評論資料，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。  |

 
### <a name="review-values"></a>評論數值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述       |
|-----------------|---------|-------------------|
| 日期            | 字串  | 評論資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId   | 字串  | 您正在擷取評論資料之 App 的 Store 識別碼。         |
| applicationName | 字串  | App 的顯示名稱。    |
| market          | 字串  | 提交評論之市場的 ISO 3166 國家/地區碼。        |
| osVersion       | 字串  | 提交評論的 OS 版本。 如需支援的字串清單，請參閱上方的[篩選欄位](#filter-fields)一節。            |
| deviceType      | 字串  | 提交評論的裝置類型。 如需支援的字串清單，請參閱上方的[篩選欄位](#filter-fields)一節。            |
| isRevised       | 布林值 | **true** 值表示評論已修訂，否則為 **false**。   |
| packageVersion  | 字串  | 已評論的應用程式套件版本。        |
| deviceModel        | 字串  |評論 app 的裝置類型。     |
| productFamily      | 字串  | 裝置系列名稱。 如需支援的字串清單，請參閱上方的[篩選欄位](#filter-fields)一節。   |
| deviceRAM       | 數字  | 實體 RAM (以 MB 為單位)。    |
| deviceScreenResolution       | 字串  | 裝置的螢幕解析度，格式為「*width* x *height*」。    |
| deviceStorageCapacity | 數字 | 主要存放磁碟的容量 (以 GB 為單位)。 |
| isTouchEnabled | 布林值 | **true** 值表示已啟用觸控功能，否則為 **false**。 |
| reviewerName | 字串 | 評論者名稱。 |
| rating | 數字 | App 評分 (以星星為單位)。 |
| reviewTitle | 字串 | 評論的標題。 |
| reviewText | 字串 | 評論的文字內容。 |
| helpfulCount | 數字 | 該評論被標記為「很有幫助」的次數。 |
| notHelpfulCount | 數字 | 該評論被標記為「沒有幫助」的次數。 |
| responseDate | 字串 | 提交回應的日期。 |
| responseText | 字串 | 回應的文字內容。 |
| id | 字串 | 評論的識別碼 (這是 GUID)。 您可以在[取得應用程式評論的回應資訊](get-response-info-for-app-reviews.md)和[提交應用程式評論的回應](submit-responses-to-app-reviews.md)方法中使用此識別碼。 |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1",
      "id": "6be543ff-1c9c-4534-aced-af8b4fbe0316"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>相關主題

* [評論報告](../publish/reviews-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得應用程式評論的回應資訊](get-response-info-for-app-reviews.md)
* [提交應用程式評論的回應](submit-responses-to-app-reviews.md)
* [取得應用程式下載數](get-app-acquisitions.md)
* [取得附加元件下載數](get-in-app-acquisitions.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
* [取得 App 評分](get-app-ratings.md)
