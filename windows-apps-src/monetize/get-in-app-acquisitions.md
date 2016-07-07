---
author: mcleanbyron
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: "在 Windows 市集分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得應用程式內產品 (IAP) 的彙總下載數資料。"
title: "取得 IAP 下載數"
ms.sourcegitcommit: 02131e641cdaa76256845b38bcc50aa42d718601
ms.openlocfilehash: 21e634b1d5ab6c3ba7762c1b83c94d076d094af5

---

# 取得 IAP 下載數


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在 Windows 市集分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得應用程式內產品 (IAP) 的彙總下載數資料。 這個方法會傳回 JSON 格式的資料。

## 先決條件


若要使用這個方法，您需要進行下列動作：

-   將您會用來呼叫此方法的 Azure AD 應用程式與您的開發人員中心帳戶產生關聯。

-   取得您應用程式的 Azure AD 存取權杖。

如需詳細資訊，請參閱[使用 Windows 市集服務存取分析資料](access-analytics-data-using-windows-store-services.md)。

## 要求


### 要求的語法

| 方法 | 要求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions |

 

### 要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為**持有人**&lt;*權杖*&gt;。 |

 

### 要求主體

*applicationId* 或 *inAppProductId* 參數為必要。 若要擷取所有註冊至 app 之 IAP 的下載數資料，請指定 *applicationId* 參數。 若要擷取單一 IAP 的下載數資料，請指定 *inAppProductId* 參數。 如果您同時指定上面兩個參數，*inAppProductId* 參數將會被忽略。

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">類型</th>
<th align="left">描述</th>
<th align="left">必要</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">applicationId</td>
<td align="left">字串</td>
<td align="left">您想要擷取 IAP 下載數資料之 app 的市集識別碼。 市集識別碼可在開發人員中心儀表板的 [App 身分識別](../publish/view-app-identity-details.md) 頁面取得。 舉例來說，市集識別碼可以是「9WZDNCRFJ3Q8」。</td>
<td align="left">是</td>
</tr>
<tr class="even">
<td align="left">inAppProductId</td>
<td align="left">字串</td>
<td align="left">您想要擷取下載數資料的 IAP 產品識別碼。</td>
<td align="left">是</td>
</tr>
<tr class="odd">
<td align="left">startDate</td>
<td align="left">日期</td>
<td align="left">要擷取 IAP 下載數資料之日期範圍的開始日期。 預設為目前的日期。</td>
<td align="left">否</td>
</tr>
<tr class="even">
<td align="left">endDate</td>
<td align="left">日期</td>
<td align="left">要擷取 IAP 下載數資料之日期範圍的結束日期。 預設為目前的日期。</td>
<td align="left">否</td>
</tr>
<tr class="odd">
<td align="left">top</td>
<td align="left">整數</td>
<td align="left">在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。</td>
<td align="left">否</td>
</tr>
<tr class="even">
<td align="left">skip</td>
<td align="left">整數</td>
<td align="left">在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。</td>
<td align="left">否</td>
</tr>
<tr class="odd">
<td align="left">filter</td>
<td align="left">字串</td>
<td align="left">在回應中篩選資料列的一或多個陳述式。 如需更多資訊，請參閱下方的＜[篩選欄位](#filter-fields)＞一節。</td>
<td align="left">否</td>
</tr>
<tr class="even">
<td align="left">aggregationLevel</td>
<td align="left">字串</td>
<td align="left">指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。</td>
<td align="left">否</td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">字串</td>
<td align="left">對每個 IAP 下載數的結果資料值做出排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>。 <em>field</em> 參數可以是下列其中一個字串：
<ul>
<li><strong>日期</strong></li>
<li><strong>acquisitionType</strong></li>
<li><strong>ageGroup</strong></li>
<li><strong>storeClient</strong></li>
<li><strong>gender</strong></li>
<li><strong>market</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>orderName</strong></li>
</ul>
<p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p>
<p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p></td>
<td align="left">否</td>
</tr>
</tbody>
</table>

 

### 篩選欄位

要求本體的 *filter* 參數包含在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位和值，而陳述式可以使用 **and** 或 **or** 結合。 下列為一些範例 *filter* 參數：

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'greater than 55' or ageGroup ne ‘less than 13’)*

如需支援欄位的清單，請參閱下列表格。 *filter* 參數中的字串值必須由單引號括住。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">欄位</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">acquisitionType</td>
<td align="left">下列其中一個字串：
<ul>
<li><strong>free</strong></li>
<li><strong>trial</strong></li>
<li><strong>paid</strong></li>
<li><strong>promotional code</strong></li>
<li><strong>IAP</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">ageGroup</td>
<td align="left">下列其中一個字串：
<ul>
<li><strong>less than 13</strong></li>
<li><strong>13-17</strong></li>
<li><strong>18-24</strong></li>
<li><strong>25-34</strong></li>
<li><strong>35-44</strong></li>
<li><strong>44-55</strong></li>
<li><strong>greater than 55</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">storeClient</td>
<td align="left">下列其中一個字串：
<ul>
<li><strong>Windows Phone Store (client)</strong></li>
<li><strong>Windows Store (client)</strong></li>
<li><strong>Windows Store (web)</strong></li>
<li><strong>Volume purchase by organizations</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">gender</td>
<td align="left">下列其中一個字串：
<ul>
<li><strong>m</strong></li>
<li><strong>f</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">market</td>
<td align="left">內含發生下載之市場的 ISO 3166 國家/地區碼的字串。</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">下列其中一個字串：
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows 8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows 10</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">下列其中一個字串：
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>IoT</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">orderName</td>
<td align="left">指定用來取得 app 之促銷碼訂單名稱的字串 (這只適用於使用者透過兌換促銷碼來取得 app 的情況)。</td>
</tr>
</tbody>
</table>

 

### 要求範例

下列範例示範數個取得 IAP 下載數資料的要求。 將 *inAppProductId* 和 *applicationId* 值以適當的 IAP 產品識別碼和 app 市集識別碼取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應


### 回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                                |
|------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 值      | array  | 內含彙總 IAP 下載數資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的＜[IAP 下載數數值](#iap-acquisition-values)＞一節。                                                                                                              |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的 IAP 下載數資料，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                                                                                                                                                                                                                                 |


### IAP 下載數數值

*Value* 陣列中的元素包含下列值。

| 值               | 類型    | 描述                                                                                                                                                                                                                              |
|---------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 日期                | 字串  | 下載數資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| inAppProductId      | 字串  | 您正在擷取下載數資料的 IAP 產品識別碼。                                                                                                                                                                 |
| inAppProductName    | 字串  | IAP 的顯示名稱。                                                                                                                                                                                                             |
| applicationId       | 字串  | 您想要擷取 IAP 下載數資料之 app 的市集識別碼。                                                                                                                                                           |
| applicationName     | 字串  | App 的顯示名稱。                                                                                                                                                                                                             |
| deviceType          | 字串  | 完成下載的裝置類型。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                  |
| orderName           | 字串  | 訂單的名稱。                                                                                                                                                                                                                   |
| storeClient         | 字串  | 發生下載之市集的版本。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                            |
| osVersion           | 字串  | 發生下載的 OS 版本。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                   |
| market              | 字串  | 發生下載之市場的 ISO 3166 國家/地區碼。                                                                                                                                                                  |
| gender              | 字串  | 做出下載之使用者的性別。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                    |
| ageGroup            | 字串  | 做出下載之使用者的年齡層。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                 |
| acquisitionType     | 字串  | 下載的類型 (免費、付費等等)。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                                    |
| acquisitionQuantity | inumber | 發生的下載數目。                                                                                                                                                                                                |

 

### 回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso IAP 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "Iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## 相關主題

* [使用 Windows 市集服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得應用程式下載數](get-app-acquisitions.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
* [取得 app 評分](get-app-ratings.md)
* [取得 app 評論](get-app-reviews.md)

 

 



<!--HONumber=Jun16_HO5-->


