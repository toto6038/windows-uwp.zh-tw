---
author: mcleanbyron
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得附加元件的彙總下載數資料。
title: 取得附加元件下載數
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, Microsoft Store 服務, Microsoft Store 分析 API, 附加元件下載數
ms.localizationpriority: medium
ms.openlocfilehash: 6f6da2ae68ab2b40f11d1a9d092eb8ff447f2844
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/17/2018
ms.locfileid: "1664008"
---
# <a name="get-add-on-acquisitions"></a>取得附加元件下載數

使用「Microsoft Store 分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得您應用程式之附加元件的彙總下載數資料 (JSON 格式)。 「Windows 開發人員中心」儀表板中的[附加元件下載數報告](../publish/add-on-acquisitions-report.md)也有提供這項資訊。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述          |
|---------------|--------|--------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

*applicationId* 或 *inAppProductId* 參數為必要。 若要擷取所有註冊至 App 之附加元件的下載數資料，請指定 *applicationId* 參數。 若要擷取單一附加元件的下載數資料，請指定 *inAppProductId* 參數。 如果您同時指定上面兩個參數，*applicationId* 參數將會被忽略。

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取附加元件下載數資料之 App 的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| inAppProductId | 字串 | 您想要擷取下載數資料之附加元件的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  | 是  |
| startDate | 日期 | 要擷取附加元件下載數資料之日期範圍的開始日期。 預設為目前的日期。 |  否  |
| endDate | 日期 | 要擷取附加元件下載數資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter |字串  | 在回應中篩選資料列的一或多個陳述式。 如需更多資訊，請參閱下方的＜[篩選欄位](#filter-fields)＞一節。 | 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。 | 否 |
| orderby | 字串 | 對每個附加元件下載數的結果資料值做出排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>日期</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>日期</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：<em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  否  |


### <a name="filter-fields"></a>篩選欄位

要求的 *filter* 參數包含在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位和值，而陳述式可以使用 **and** 或 **or** 結合。 下列為一些範例 *filter* 參數：

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'greater than 55' or ageGroup ne ‘less than 13’)*

如需支援欄位的清單，請參閱下列表格。 *filter* 參數中的字串值必須由單引號括住。

| 欄位        |  描述        |
|---------------|-----------------|
| acquisitionType | 下列其中一個字串：<ul><li><strong>free</strong></li><li><strong>trial</strong></li><li><strong>paid</strong></li><li><strong>promotional code</strong></li><li><strong>IAP</strong></li></ul> |
| ageGroup | 下列其中一個字串：<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unknown</strong></li></ul> |
| storeClient | 下列其中一個字串：<ul><li><strong>Windows Phone Store (client)</strong></li><li><strong>Microsoft Store (用戶端)</strong></li><li><strong>Microsoft Store (web)</strong></li><li><strong>Volume purchase by organizations</strong></li><li><strong>Other</strong></li></ul> |
| gender | 下列其中一個字串：<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul> |
| market | 內含發生下載之市場的 ISO 3166 國家/地區碼的字串。 |
| osVersion | 下列其中一個字串：<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | 下列其中一個字串：<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| orderName | 指定用來取得附加元件之促銷碼訂單名稱的字串 (這只適用於使用者透過兌換促銷碼來取得附加元件的情況)。 |


### <a name="request-example"></a>要求範例

下列範例示範數個取得附加元件下載數資料的要求。 將 *inAppProductId* 和 *applicationId* 值以適當的附加元件或 App 的 Store 識別碼取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述         |
|------------|--------|------------------|
| 值      | 陣列  | 內含彙總附加元件下載數資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[附加元件下載數數值](#add-on-acquisition-values)一節。                                                                                                              |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的附加元件下載數資料，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>附加元件下載數數值

*Value* 陣列中的元素包含下列值。

| 值               | 類型    | 描述        |
|---------------------|---------|---------------------|
| 日期                | 字串  | 下載數資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| inAppProductId      | 字串  | 您正在擷取下載數資料之附加元件的 Store 識別碼。                                                                                                                                                                 |
| inAppProductName    | 字串  | 附加元件的顯示名稱。 除非您在 *groupby* 參數中指定 **inAppProductName** 欄位，否則只有當 *aggregationLevel* 參數設定為 **day** 時，回應資料中才會顯示這個值。                                                                                                                                                                                                            |
| applicationId       | 字串  | 您想要擷取附加元件下載數資料之 App 的 Store 識別碼。                                                                                                                                                           |
| applicationName     | 字串  | App 的顯示名稱。                                                                                                                                                                                                             |
| deviceType          | 字串  | 完成下載的裝置類型。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                  |
| orderName           | 字串  | 訂單的名稱。                                                                                                                                                                                                                   |
| storeClient         | 字串  | 發生下載之 Microsoft Store 的版本。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                            |
| osVersion           | 字串  | 發生下載的 OS 版本。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                   |
| market              | 字串  | 發生下載之市場的 ISO 3166 國家/地區碼。                                                                                                                                                                  |
| gender              | 字串  | 做出下載之使用者的性別。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                    |
| ageGroup            | 字串  | 做出下載之使用者的年齡層。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                 |
| acquisitionType     | 字串  | 下載的類型 (免費、付費等等)。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                    |
| acquisitionQuantity | inumber | 發生的下載數目。                        |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso add-on 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## <a name="related-topics"></a>相關主題

* [附加元件下載數報告](../publish/add-on-acquisitions-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [依通道取得附加元件轉換](get-add-on-conversions-by-channel.md)
* [取得 App 下載數](get-app-acquisitions.md)
* [取得應用程式取得漏斗圖資料](get-acquisition-funnel-data.md)
* [依通道取得應用程式轉換](get-app-conversions-by-channel.md)

 

 
