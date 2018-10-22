---
author: Xansky
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得應用程式的彙總下載數資料。
title: 取得應用程式下載數
ms.author: mhopkins
ms.date: 03/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 應用程式下載數
ms.localizationpriority: medium
ms.openlocfilehash: 997f4e088edfced94189c2c0977bcfff60166059
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5410976"
---
# <a name="get-app-acquisitions"></a>取得應用程式下載數


使用「Microsoft Store 分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得應用程式的彙總下載數資料 (JSON 格式)。 「Windows 開發人員中心」儀表板中的[下載數報告](../publish/acquisitions-report.md)也有提供這項資訊。

## <a name="prerequisites"></a>先決條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取下載數資料之應用程式的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| startDate | 日期 | 要擷取下載數資料之日期範圍的開始日期。 預設為目前的日期。 |  否  |
| endDate | 日期 | 要擷取下載數資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 例如，*filter=market eq 'US' and gender eq 'm'*。 <p/><p/>您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul> | 不可以   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。 | 否 |
| orderby | 字串 | 對每個下載數的結果資料值做出排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>日期</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：<em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  否  |

### <a name="request-example"></a>要求範例

下列範例示範數個取得應用程式下載數資料的要求。 將 *applicationId* 值以您應用程式的 Store 識別碼取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 陣列  | 物件陣列，內含應用程式的彙總下載數資料。 如需有關每個物件中資料的詳細資訊，請參閱下方的[下載數數值](#acquisition-values)一節。                                                                                                                      |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的下載數資料，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                 |


### <a name="acquisition-values"></a>下載數數值

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| 日期                | 字串 | 下載數資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId       | 字串 | 您正在擷取下載數資料之應用程式的 Store 識別碼。     |
| applicationName     | 字串 | App 的顯示名稱。   |
| deviceType          | 字串 | 下列其中一個字串，指定發生取得的裝置類型：<ul><li><strong>電腦</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>    |
| orderName           | 字串 | 訂單的名稱。  |
| storeClient         | 字串 | 下列其中一個表示收購發生的 Microsoft Store 版本之字串：<ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (用戶端)** (或**Microsoft Store (用戶端)** 如果在 2018 年 3 月 23 之前查詢資料)</li><li>**Microsoft Store (網頁)** (或**Microsoft Store (網頁)** 如果在 2018 年 3 月 23 之前查詢資料)</li><li>**Volume purchase by organizations**</li><li>**其他**</li></ul>                                                                                            |
| osVersion           | 字串 | 下列其中一個字串，指定發生下載所在的作業系統版本：<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows8.1</strong></li><li><strong>Windows10</strong></li><li><strong>Unknown</strong></li></ul>  |
| market              | 字串 | 發生下載之市場的 ISO 3166 國家/地區碼。  |
| gender              | 字串 | 下列其中一個字串，這些字串詳列進行收購的使用者的性別：<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul>    |
| ageGroup            | 字串 | 下列其中一個字串，指定完成下載之使用者的年齡層：<ul><li><strong>13 歲以下</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>不明</strong></li></ul>  |
| acquisitionType     | 字串 | 其中一個下列字串，可指出收購類型：<ul><li><strong>免費</strong></li><li><strong>試用版</strong></li><li><strong>付費</strong></li><li><strong>促銷代碼</strong></li><li><strong>Iap</strong></li><li><strong>訂閱 Iap</strong></li><li><strong>私人對象</strong></li><li><strong>Pre 順序</strong></li><li><strong>Xbox Game Pass</strong> (或<strong>Game Pass</strong>如果在 2018 年 3 月 23 之前查詢資料)</li><li><strong>磁碟</strong></li><li><strong>預付碼</strong></li></ul>   |
| acquisitionQuantity | 數字 | 指定彙總層級期間發生的下載數目。    |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "IT",
      "gender": "m",
      "ageGroup": "0-17",
      "acquisitionType": "Free",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "appacquisitions?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 466766
}
```

## <a name="related-topics"></a>相關主題

* [下載數報告](../publish/acquisitions-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得應用程式取得漏斗圖資料](get-acquisition-funnel-data.md)
* [依通道取得應用程式轉換](get-app-conversions-by-channel.md)
* [取得附加元件下載數](get-in-app-acquisitions.md)
